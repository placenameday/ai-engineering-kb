---
name: practical-guide
description: "Practical quick-reference for building Claude Code projects, distilled from AI engineering frontier knowledge"
survey_date: "2026-03-27"
lang: "en/zh"
---

# Claude Code 项目构建实战指南

## Quick Reference from AI Engineering Frontier (2025-2026)

> 本指南从14篇前沿概念文章中提炼，面向使用 Claude Code 独立构建项目的开发者。
> 规则优先，不讲理论。钉在桌面，每天翻阅。

---

### 0. 核心原则 (Core Rules)

1. **Focused 300 tokens > unfocused 113K tokens.** Context quality always beats quantity.
2. **Start with workflows, not agents.** If you can draw the flowchart before runtime, use a workflow.
3. **Own your prompts, control flow, and context window.** Never let a framework hide what the model sees.
4. **Evals are the TDD of AI.** No evals = guessing. Build them before features.
5. **Simple first, complex only when proven necessary.** Anthropic, 12-Factor, Willison all converge here.
6. **Harness > Context.** The 2026 paradigm: control the operational environment, not just what the model sees.

---

### 1. 项目设置 (Project Setup)

#### CLAUDE.md — 把它当作索引，不是百科全书

| Rule | Why |
|------|-----|
| Keep CLAUDE.md under 30 lines | 每条消息都加载它，每行都消耗 token |
| Content: project description + structure + conventions only | 详细知识放别处 |
| No tutorials, no accumulated learnings inline | 用 `.claude/memory/` 代替 |

**Template:**

```markdown
# CLAUDE.md
## Description
[1-2 sentences]
## Structure
[directory table]
## Conventions
[3-5 bullet rules: naming, formatting, patterns]
```

#### 文件系统布局 (File Placement Decision Tree)

```
需要放在哪？问自己：

这段知识是...
├── 每次都需要？ → CLAUDE.md（但保持精简）
├── 只在特定目录下需要？ → .claude/rules/rule-name.md（设 path_patterns）
├── 大段 + 不常用 + 自成一体？ → .claude/skills/skill-name.md
├── 参考资料 / 累积知识？ → .claude/memory/topic.md
└── 不确定 → .claude/memory/（最安全的默认选择）
```

#### Context Loading 成本一览

| Layer | Loading | Token Budget | 注意 |
|-------|---------|--------------|------|
| CLAUDE.md | **Always** | 50-200 lines max | 每行每消息必付代价 |
| @imports | **Always** (expanded) | Use sparingly | 引入即付费，无法按需 |
| .claude/rules/*.md | **Conditional** | Per-directory | path_patterns 触发 |
| .claude/skills/ | **On-demand** | ~2% for descriptions | 描述匹配才加载 |
| .claude/memory/ | **On-demand** | Unlimited | Agent 主动读取 |

#### MCP 工具选择 (Tool Selection)

| Rule | Detail |
|------|--------|
| 7±2 tools per domain | DDD 启发的 Bounded Context Packs |
| Audit total tool count | 35 tools = ~26K tokens overhead (just schemas!) |
| Prefer Meta-Tool pattern for large sets | 2 tools (discover + execute) replace N → 85-95% token savings |
| Write excellent tool descriptions | 这就是 ACI（Agent-Computer Interface），等同于给 agent 设计 UI |
| Don't install every MCP server | 每个都吃 context，即使不用 |

**推荐 MCP 搭配（solo dev 场景）：**

| Purpose | Server |
|---------|--------|
| Documentation | Context7 |
| Code intelligence | Serena |
| File operations | Built-in (filesystem) |
| Web research | WebSearch + WebFetch |

---

### 2. Agent 设计 (Agent Design)

#### Workflow vs Agent — 一句话判断

> **能在运行前画出流程图 → Workflow。需要模型动态决定路径 → Agent。**

| Dimension | Workflow | Agent |
|-----------|----------|-------|
| Control flow | Predefined code paths | Model decides at runtime |
| Predictability | High | Low |
| Debugging | Easy | Hard |
| Best for | Well-defined tasks | Open-ended, flexible tasks |

**Solo dev rule of thumb:** 80% of your tasks are workflows. Don't reach for agents by default.

#### Multi-Agent 编排规则

如果确实需要多个 agent：

| Rule | 原因 |
|------|------|
| Keep agents small and single-purpose (Factor 9) | 单一职责，容易调试 |
| One coordinator, never let agents call each other | 避免不可控的递归 |
| Pre-fetch context before agent runs (Factor 12) | 减少昂贵的中途查询 |
| Design explicit pause points (Factor 6) | 人类可以在关键节点审查 |
| Contact humans via tool calls (Factor 7) | HITL 是工具，不是异常 |
| Compact errors into useful context (Factor 8) | 原始报错浪费 token |

#### 12-Factor Agents 精简速查

| # | Factor | 一句话 | Solo Dev 操作 |
|---|--------|--------|---------------|
| 1 | NL → Tool Calls | LLM 输出结构化调用 | Use function calling / structured output |
| 2 | Own Your Prompts | 不让框架藏 prompt | 直接写 prompt，不用框架抽象层 |
| 3 | Tools = Structured Output | Tool call 就是 JSON 生成 | 工具描述 = agent 的 UI |
| 4 | Own Control Flow | 掌控执行流程 | 不用不透明的框架循环 |
| 5 | Own Context Window | 主动管理模型看到什么 | 精选 context，不 dump everything |
| 6 | Launch/Pause/Resume | 可暂停检查点 | 长任务设中断点 |
| 7 | HITL via Tool Calls | 人类参与是工具 | `ask_human` 是一个 tool |
| 8 | Compact Errors | 错误压缩再送回 | 别把 raw stacktrace 全塞进去 |
| 9 | Small Focused Agents | 单一职责 | 一个 agent 做一件事 |
| 10 | Trigger Anywhere | 多入口 | webhook / cron / CLI / API |
| 11 | Unify State | 执行态=业务态 | 别让两套状态分裂 |
| 12 | Pre-fetch Context | 预加载 | 运行前备好上下文 |

#### 框架选择（如果你真的需要框架）

| 你的场景 | 选择 |
|---------|------|
| 简单单 agent | **不用框架**，直接 API 调用 |
| 需要类型安全结构化输出 | Pydantic AI |
| 轻量 agent 编排 | OpenAI Agents SDK |
| 复杂有状态多 agent | LangGraph |
| Web 流式场景 | Vercel AI SDK |

---

### 3. 上下文工程 (Context Engineering)

> *"Context engineering is the delicate art and science of filling the context window with just the right information for the next step."* — Karpathy

#### Token 预算管理

| Metric | Value | Action |
|--------|-------|--------|
| Context rot threshold | **~40% of window** | 超过此比例性能显著下降 |
| 200K window safe limit | **~80K tokens** | 留 120K 给模型思考 |
| 90% fill | **Measurably degraded** | 远在此之前就该 compact |
| Proactive compact | **70%** | 运行 `/compact Focus on X` |
| Auto-compact trigger | 95% | 太晚了，质量已损失 |

#### Write / Select / Compress / Isolate — 四步决策

每次上下文管理决策，问自己用哪个模式：

| Pattern | 动作 | Claude Code 实现 | 什么时候用 |
|---------|------|------------------|-----------|
| **Write** | 持久化到外部 | `.claude/memory/` files, DB | 跨 session 需要的信息 |
| **Select** | 拉入相关上下文 | Skills, @imports, RAG | 只加载当前任务需要的 |
| **Compress** | 减少 token | `/compact`, summaries | Context 膨胀时 |
| **Isolate** | 分区隔离 | Sub-agents with own windows | 防止跨任务干扰 |

#### Session 卫生 (Session Hygiene)

| Practice | Command / Action | 注意 |
|----------|------------------|------|
| Clear between unrelated tasks | `/clear` | 不相关 context 是毒药 |
| Compact with focus | `/compact Focus on [current task]` | 比无焦点 compact 效果好 |
| Time-box sessions | 30-minute focused sprints | 然后 clear 或 compact |
| Audit memory files | Every 2 weeks | 删除过时的 memory 文件 |

#### Context 反模式速查

| 反模式 | 代价 | 正确做法 |
|--------|------|---------|
| 把所有知识塞进 CLAUDE.md | 每消息每行付费 | 只放索引，详情放 memory |
| 装太多 MCP server | 35 tools ≈ 26K tokens 开销 | 只装需要的 |
| 等 95% 才 compact | 质量已下降 | 70% 主动 compact |
| 不 /clear 就切换任务 | 旧 context 干扰新任务 | 任务边界清晰切割 |
| 泛泛规则 "write clean code" | 浪费 token，无实际指导 | 具体："TypeScript strict, no any" |
| @import 大文件 | Always-loaded，无法按需 | 改用 skill 或 memory |

---

### 4. 开发流程 (Development Process)

#### Spec-Driven 方法 — 意图是源代码

| 原则 | 操作 |
|------|------|
| **Intent is the source of truth** | 写 spec 先于写代码 |
| CLAUDE.md / rules = spec files | 它们就是你给 agent 的需求规格 |
| Specs: specific, structured, testable | "TypeScript strict, no any" ✓ / "Write clean code" ✗ |
| Version specs alongside code | spec 变更 = 代码变更，同步提交 |
| Define acceptance criteria | 每个 spec 都要有"完成"的定义 |

**Solo Dev Spec Workflow:**

```
1. 写 spec（在 .claude/memory/ 或直接作为 prompt）
2. Claude Code 按 spec 生成代码
3. 运行 eval / test 验证
4. 失败 → 修 spec 或修 code，不要盲目重试
5. 通过 → commit spec + code together
```

#### Eval-Driven 质量保障

| Rule | Detail |
|------|--------|
| Build evals BEFORE features | Evals 是 AI 的 TDD |
| Run evals in CI/CD | 不要依赖肉眼检查 |
| Evaluate at system level | Pipeline + memory + guardrails，不只是 model benchmark |
| Use rubrics for LLM-as-Judge | 结构化评分标准，不让 LLM 自由评判 |
| Track the Generator-Verifier gap | 生成容易验证难，验证是真正的工程挑战 |

**Eval 工具推荐（Solo Dev）：**

| Tool | Type | Best For |
|------|------|----------|
| Promptfoo | Open source | Fast iteration, red-team testing |
| DeepEval | Open source | LLM unit testing |
| Ragas | Open source | RAG evaluation |

---

### 5. Harness 配置 (Harness Configuration)

> 2026 范式：Harness Engineering。控制 operational environment，不只是 context。

#### Permission Modes 速查

| Mode | 行为 | 适用场景 |
|------|------|----------|
| `default` | 首次使用时提示 | 日常开发 |
| `plan` | 只读工具 | 规划模式 |
| `acceptEdits` | 自动接受编辑 | 快速迭代 |
| `bypassPermissions` | 跳过提示（除.git/.claude） | 信任环境 |
| `auto` | 全自主但有边界 | 生产部署 |

#### Hooks 使用场景

| Hook | 能否阻止 | 用途 |
|------|---------|------|
| `PreToolUse` | ✓ | 拦截危险命令 (rm -rf) |
| `PostToolUse` | ✗ | 编辑后自动 lint |
| `Stop` | ✓ | 完成前质量检查 |
| `SessionStart` | ✗ | 加载上下文 |
| `SessionEnd` | ✗ | 清理、持久化状态 |

#### Control Plane 检查清单

每个 agent 操作都应通过：

```
□ Budget check → 费用在阈值内？
□ Scope check → 允许的域名/路径？
□ Confidence check → 置信度够高？
□ Rate limit → 未超配额？
□ Audit log → 已记录？
```

#### 配置位置

| 配置类型 | 路径 |
|---------|------|
| 全局权限 | `~/.claude/settings.json` |
| 项目权限 | `.claude/settings.local.json` |
| Hooks | `settings.json` 的 `hooks` 字段 |
| Rules | `.claude/rules/*.md` |

---

### 6. 安全与部署 (Safety & Deployment)

#### Progressive Autonomy — 分阶段给权限

| Phase | Mode | AI Does | Human Does | 进入条件 |
|-------|------|---------|------------|----------|
| 1 | **Advisory** | Suggests only | All decisions | 初始部署 |
| 2 | **Supervised** | Executes with confirmation | Approve/reject | 低风险任务可靠性已验证 |
| 3 | **Constrained** | Autonomous within boundaries | Exception handling | 系统化 eval 证明可靠 |

**Solo dev rule:** Start every new capability in Phase 1. Use evals to justify graduation.

#### 五层防御 (Defense in Depth)

| Layer | Action | Solo Dev 操作 |
|-------|--------|---------------|
| 1. Input sanitization | Filter injection patterns | 检查用户输入 |
| 2. Permission minimization | Strict tool/data boundaries | MCP 权限最小化 |
| 3. Context isolation | Anthropic's Isolate pattern | Sub-agent 隔离不可信输入 |
| 4. Output verification | Check agent output | 验证生成内容再使用 |
| 5. Audit trail | Log all operations | 保留操作日志 |

#### Prompt Injection — 像对待 SQL Injection 一样严肃

| Fact | Value |
|------|-------|
| OWASP LLM Top 10 排名 | **#1** |
| 无访问控制的 AI 安全事件 (2025) | **97%** |
| 缺乏 AI 安全框架的企业 | **87%** |

**Rules:**
- Never trust user-provided content as instructions
- Sandbox all tool execution
- Use per-action permission controls, not crude kill switches
- "Set and forget" safety = no safety

---

### 6. 关键数字速查 (Key Numbers)

| Metric | Value | Source | 操作含义 |
|--------|-------|--------|---------|
| Context rot threshold | ~40% of window | Jan 2026 study | 超过就该 compress |
| 200K window safe budget | ~80K tokens | Same | 为模型思考留空间 |
| Proactive compact point | 70% | Best practice | 运行 `/compact` |
| Auto-compact (too late) | 95% | Claude Code default | 别等到这步 |
| MCP tool schema overhead (35 tools) | ~26K tokens | Anthropic | 控制工具数量 |
| Optimal tools per domain | 7±2 | Synaptic Labs / DDD | Bounded Context Packs |
| Meta-tool token reduction | 85-95% | Synaptic Labs | 大工具集必用 |
| CORPGEN memory improvement | up to 3.5x | Microsoft Feb 2026 | 三层记忆架构 |
| AI co-authored issue rate | 1.7x more "major" issues | CodeRabbit | 必须有 eval + review |
| Multi-agent accuracy gain | +60% | Industry data | 复杂任务考虑多 agent |
| Multi-agent speed gain | +45% | Industry data | — |
| CLAUDE.md budget | ≤30 lines | Convention | 索引，不是百科 |
| Skill description budget | ~2% of context | Claude Code | 描述驱动按需加载 |
| Memory audit frequency | Every 2 weeks | Best practice | 删除过期 memory |
| Session sprint length | 30 minutes | Best practice | 然后 clear 或 compact |
| Focused 300 tokens vs unfocused 113K | 300 wins | Chroma research | 质量 >> 数量 |

---

### Quick Decision Cards 速查卡

#### "我该用 Agent 还是 Workflow？"

```
能画流程图？ ──yes──→ Workflow
      │no
      ↓
需要 LLM 动态决定路径？ ──yes──→ Agent
      │no
      ↓
Workflow（默认选择）
```

#### "这段信息放在哪？"

```
每次对话都要用？ ──yes──→ CLAUDE.md（≤30行！）
      │no
      ↓
只在特定路径下需要？ ──yes──→ .claude/rules/ (path_patterns)
      │no
      ↓
大段 + 不常用 + 自成一体？ ──yes──→ .claude/skills/
      │no
      ↓
.claude/memory/（默认安全选择）
```

#### "需要框架吗？"

```
能用直接 API 调用解决？ ──yes──→ 不用框架
      │no
      ↓
需要复杂状态管理？ ──yes──→ LangGraph
      │no
      ↓
需要类型安全输出？ ──yes──→ Pydantic AI
      │no
      ↓
轻量编排？ ──yes──→ OpenAI Agents SDK
```

---

> **来源：** 本指南提炼自 [AI Engineering Frontier KB](concepts/_index.md) 的 13 篇概念文章与 2 篇工具调研。
> 涵盖 Karpathy (Software 3.0), Ng (Agentic Patterns), Anthropic (Agent Design), Willison (Practitioner Patterns),
> Horthy (12-Factor Agents), Zaharia (Compound AI) 等前沿思想。
>
> Last updated: 2026-03-05
