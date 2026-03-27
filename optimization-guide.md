---
name: optimization-guide
description: "Systematic AI system optimization framework - efficiency metrics, decision trees, frontier techniques, trade-offs"
survey_date: "2026-03-27"
lang: "en/zh"
---

# AI 系统优化指南 (System Optimization)

> 系统性提升 AI 系统效率。基于 2026 前沿研究。
> 核心理念：优化是持续过程，不是一次性配置。

---

## 0. 效率三维模型

```
              QUALITY (质量)
                 ▲
                /│\
               / │ \
              /  │  \
             /   │   \
            /    │    \
           /     │     \
          ┌──────┼──────┐
          │\     │     /│
          │ \    │    / │
          │  \   │   /  │
          │   \  │  /   │
COST ◄────┼────\ │ /────┼────► LATENCY
(成本)    │     \│/     │      (延迟)
          └──────┼──────┘
                 │
                 ▼
```

**优化目标：** 在约束条件下，最大化质量，最小化成本和延迟。

| Dimension | Metric | Target |
|-----------|--------|--------|
| **Quality** | Task success rate, Eval scores | >95% for production |
| **Cost** | Token usage, API calls, $ per task | Minimize within quality constraints |
| **Latency** | Time to first token, Total response time | <2s for interactive, <30s for batch |

---

## 1. 优化决策树

```
"我该优化什么？"

问题是什么？
├── 质量不够？ ──────────────────────────────────────┐
│   ├── 任务定义不清？ → Spec-Driven Dev           │
│   ├── Context 不对？ → Context Engineering       │
│   ├── 模型能力不足？ → 换模型 / Multi-Agent       │
│   └── 缺少验证？ → Eval-Driven Dev               │
│                                                    │
├── 成本太高？ ──────────────────────────────────────┤
│   ├── Token 浪费？ → Context 精简 / Caching      │
│   ├── 调用太多？ → 批处理 / 路由优化              │
│   ├── 模型太贵？ → 路由到小模型                   │
│   └── 重复计算？ → 记忆 / 缓存                    │
│                                                    │
├── 延迟太高？ ──────────────────────────────────────┤
│   ├── 模型太慢？ → 换快模型 / Streaming          │
│   ├── 并行不够？ → Multi-Agent 并行化            │
│   ├── Context 太大？ → 压缩 / 选择性加载          │
│   └── 工具太慢？ → 异步 / 预计算                  │
│                                                    │
└── 都没问题但想更好？ → 持续优化循环               │
```

---

## 2. Context 优化

> Context 层架构设计见 [architecture-guide.md §5](architecture-guide.md)。
> Context 理论基础见 [concepts/context-engineering.md](concepts/context-engineering.md)。

### 2.1 Context Rot 研究 (Jan 2026)

| Finding | Value | Implication |
|---------|-------|-------------|
| Performance degradation starts | ~40% of window | Don't fill past this |
| 90% fill | Measurably worse quality | Already too late |
| Proactive compression point | 70% | Run /compact here |

### 2.2 Token 预算分配

```
200K Window (Claude)
├── System Prompt + Tools: ~10-15K (fixed)
├── Working Memory: ~20-40K (active task)
├── Retrieved Context: ~20-40K (RAG/memory)
├── Conversation History: ~20-40K (compressed)
└── Model Reasoning: ~80-120K (MUST PRESERVE)
```

### 2.3 Context 优化技术

| Technique | Savings | When to Use |
|-----------|---------|-------------|
| **Prompt Caching** | 90% on repeated content | System prompts, tool schemas |
| **Selective Loading** | 50-80% | Skills, rules on-demand |
| **Context Compression** | 30-60% | Long conversations |
| **Sub-agent Isolation** | 100% (separate window) | Independent tasks |

### 2.4 CORPGEN 三层记忆 (Microsoft Feb 2026)

```
┌─────────────────────────────────────────────────────────────┐
│                    L3: Semantic Memory                       │
│              (Vector Store, External Knowledge)              │
│                    Retrieval: On-demand                      │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    L2: Structured LTM                        │
│              (Project Memory, User Preferences)              │
│                    Retrieval: Task-relevant subset           │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    L1: Working Memory                        │
│              (Current Session, Active Context)               │
│                    Budget: 20-40K tokens                     │
└─────────────────────────────────────────────────────────────┘
```

**Key finding:** Three-layer architecture improves memory retrieval by up to **3.5x**.

---

## 3. 成本优化

> 成本控制是 Control Plane 的一部分。架构见 [architecture-guide.md §6.2](architecture-guide.md)。

### 3.1 模型路由策略

```
                    Task Complexity
                         │
    ┌────────────────────┼────────────────────┐
    │                    │                    │
    ▼                    ▼                    ▼
  Simple              Medium              Complex
    │                    │                    │
    ▼                    ▼                    ▼
Haiku/GPT-4.1-mini  Sonnet/GPT-4.1      Opus/o1-pro
  $0.25/1M           $3/1M               $15-150/1M
```

**Routing Rules:**
| Task Type | Route To | Reason |
|-----------|----------|--------|
| Classification, extraction | Smallest capable | Simple patterns |
| Code generation, writing | Mid-tier | Balance cost/quality |
| Complex reasoning, research | Top-tier | Quality critical |
| Multi-step with verification | Mid-tier + eval | Iteration over power |

### 3.2 Prompt Caching

| Cache Type | Savings | Claude Code Support |
|------------|---------|---------------------|
| System prompt | 90% | Automatic |
| Tool schemas | 90% | Automatic |
| Large context blocks | 90% | Use `cache_control` |

**Implementation:**
```python
# Claude API with caching
response = client.messages.create(
    model="claude-sonnet-4-6",
    system=[
        {
            "type": "text",
            "text": LONG_SYSTEM_PROMPT,
            "cache_control": {"type": "ephemeral"}
        }
    ],
    messages=messages
)
```

### 3.3 批处理与异步

| Pattern | Savings | When |
|---------|---------|------|
| Batch API | 50% cost | Non-time-sensitive |
| Parallel agents | 50% time | Independent tasks |
| Async tools | Reduced latency | I/O bound |

---

## 4. 延迟优化

### 4.1 模型选择

| Model | TTFT | Use Case |
|-------|------|----------|
| Haiku | <100ms | Interactive, streaming |
| Sonnet | ~500ms | Standard tasks |
| Opus | ~1-2s | Complex reasoning |

### 4.2 Streaming

```python
# Always stream for interactive
with client.messages.stream(...) as stream:
    for text in stream.text_stream:
        yield text  # Progressive rendering
```

### 4.3 Parallel Execution

```
Sequential: A → B → C = 30s
Parallel:   A ┐
            B ├→ merge = 10s
            C ┘
```

**Multi-Agent Parallel Pattern:**
```python
# Independent subtasks
results = await asyncio.gather(
    agent_a.run(task_a),
    agent_b.run(task_b),
    agent_c.run(task_c)
)
final = coordinator.merge(results)
```

---

## 5. 质量优化

### 5.1 Generator-Evaluator 模式

> 模式定义见 [architecture-guide.md §2.2 Gen-Eval](architecture-guide.md)

**Key insight:** Models cannot reliably evaluate their own work. Use separate generator and evaluator.

**优化效果：**

| Technique | Accuracy Gain | When to Use |
|-----------|---------------|-------------|
| Debate | +20-40% | High-stakes decisions |
| Voting | +15-30% | Classification tasks |
| Review | +25-50% | Code, writing |
| Specialization | +30-60% | Complex multi-domain |

### 5.2 Multi-Agent 协作

> 协作模式设计见 [architecture-guide.md §2](architecture-guide.md)。

**Research finding:** Multi-agent systems show **+60% accuracy** and **+45% speed** improvement.

| Pattern | Accuracy Gain | When to Use |
|---------|---------------|-------------|
| Debate | +20-40% | High-stakes decisions |
| Voting | +15-30% | Classification tasks |
| Review | +25-50% | Code, writing |
| Specialization | +30-60% | Complex multi-domain |

**Research finding:** Multi-agent systems show **+60% accuracy** and **+45% speed** improvement.

### 5.3 Test-Time Compute (Extended Thinking)

| Mode | Tokens | Quality | Cost |
|------|--------|---------|------|
| Standard | Output only | Baseline | 1x |
| Extended thinking | Output + reasoning | +10-30% | 1.5-2x |

**When to use:**
- Complex reasoning tasks
- Math, logic, analysis
- Quality > cost scenarios

---

## 6. 系统优化检查清单

### 6.1 每日检查

```
□ Context 是否超过 70%？（压缩）
□ 有没有重复的 token？（缓存）
□ 任务是否清晰定义？（spec）
□ 是否用了最合适的模型？（路由）
```

### 6.2 每周检查

```
□ Eval 分数有变化吗？（趋势）
□ 成本趋势如何？（监控）
□ 延迟是否可接受？（SLA）
□ Memory 是否过期？（清理）
```

### 6.3 发布前检查

```
□ Evals 全部通过？
□ 成本在预算内？
□ 延迟满足 SLA？
□ 安全检查通过？
□ 监控和告警就绪？
```

---

## 7. 优化度量仪表盘

| Metric | Formula | Target |
|--------|---------|--------|
| **Token Efficiency** | Output tokens / Input tokens | >0.1 |
| **Cost per Task** | Total $ / Tasks completed | Project-specific |
| **Quality Score** | Eval pass rate | >95% |
| **Latency P50/P95** | Response time percentiles | P95 < 5s |
| **Cache Hit Rate** | Cached / Total tokens | >50% |
| **Context Utilization** | Useful tokens / Total context | >60% |

---

## 8. 持续优化循环

```
    ┌─────────────────────────────────────────────────────┐
    │                                                     │
    ▼                                                     │
┌────────┐    ┌────────┐    ┌────────┐    ┌────────┐    │
│ MEASURE│───►│ ANALYZE│───►│  FIX   │───►│ VERIFY │────┘
│ (度量) │    │ (分析) │    │ (修复) │    │ (验证) │
└────────┘    └────────┘    └────────┘    └────────┘
    │             │             │             │
    │             │             │             │
    ▼             ▼             ▼             ▼
  收集指标     找瓶颈       实施改进     Evals通过
```

**Key principle:** Evals are the gate. No change ships without passing evals.

---

## 9. 前沿优化技术 (2026)

### 9.1 已验证

| Technique | Benefit | Maturity |
|-----------|---------|----------|
| Prompt Caching | 90% cost on cached | Production |
| CORPGEN Memory | 3.5x retrieval | Research |
| Multi-Agent | +60% accuracy | Production |
| Context Compression | 30-60% reduction | Production |

### 9.2 实验性

| Technique | Benefit | Status |
|-----------|---------|--------|
| Test-time compute scaling | +10-30% quality | Early |
| Adaptive context | Dynamic budget | Research |
| Neural caching | Learned cache | Research |

---

## 10. 关键数字

| Metric | Value | Source |
|--------|-------|--------|
| Context rot threshold | ~40% | Jan 2026 study |
| CORPGEN improvement | up to 3.5x | Microsoft Feb 2026 |
| Multi-agent accuracy gain | +60% | Industry data |
| Multi-agent speed gain | +45% | Industry data |
| Prompt caching savings | 90% | Anthropic |
| Meta-tool token reduction | 85-95% | Synaptic Labs |
| AI co-authored issue rate | 1.7x more "major" | CodeRabbit |

---

> **Source:** AI Engineering Frontier KB, Microsoft CORPGEN (Feb 2026), Anthropic, Jan 2026 Context Study
>
> Last updated: 2026-03-27
