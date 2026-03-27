---
name: practical-guide
description: "AI project optimization entry point - diagnose, plan, implement via Harness engineering"
survey_date: "2026-03-27"
lang: "zh"
---

# AI 项目优化实践指导

> 拿到一个项目，按以下路径优化 AI 工程能力。
> 从诊断到实施到验证，每步链接到具体文档。

---

## Step 0: 定位你的场景

```
你的项目是什么？

A. AI 编程工具项目 (Cursor/Windsurf/Claude Code)
   → 用户是人，AI 是助手
   → 优化目标：让 AI 理解项目、高效执行

B. AI Agent/应用项目 (SDK/API 开发)
   → 用户调用 API，AI 自主执行
   → 优化目标：系统可控、质量可靠

C. Multi-Agent 系统 (多 Agent 协作)
   → 多个 Agent 协同完成复杂任务
   → 优化目标：高效协作、架构清晰
```

| 场景 | 主要参考 |
|------|---------|
| A | [通用版](practical-guide-universal.md) → [CC版](practical-guide-cc.md) |
| B | [通用版](practical-guide-universal.md) → [SDK/API版](practical-guide-oc.md) |
| C | [架构指南](architecture-guide.md) → [通用版](practical-guide-universal.md) |

---

## Step 1: 诊断 — 你的项目缺什么？

按以下检查清单逐项评估。每项标注 ✅/❌/⚠️

### 1.1 基础设施层 (Harness)

| # | 检查项 | 缺失影响 | 修复参考 |
|---|--------|---------|---------|
| 1 | **有项目配置文件？** (CLAUDE.md/.cursorrules) | AI 不了解项目约定 | [通用版 §1](practical-guide-universal.md) |
| 2 | **权限控制？** (什么能做/不能做) | AI 可能执行危险操作 | [通用版 §2](practical-guide-universal.md) |
| 3 | **工具边界？** (MCP/API 范围) | 工具过多→token 浪费 | [通用版 §3](practical-guide-universal.md) |
| 4 | **安全护栏？** (预算/范围限制) | 无上限→成本失控 | [通用版 §4](practical-guide-universal.md) |

### 1.2 认知能力层 (Context)

| # | 检查项 | 缺失影响 | 修复参考 |
|---|--------|---------|---------|
| 5 | **项目记忆系统？** (memory/索引) | 每次会话从零开始 | [CC版 §2](practical-guide-cc.md) |
| 6 | **按需加载？** (progressive disclosure) | Context 膨胀→能力下降 | [架构指南 §5](architecture-guide.md) |
| 7 | **Context 预算管理？** (压缩/清理) | 90%填充→模型退化 | [优化指南 §2](optimization-guide.md) |
| 8 | **领域知识注入？** (skills/rules) | AI 缺少专业知识 | [CC版 §3](practical-guide-cc.md) |

### 1.3 质量保证层 (Feedback)

| # | 检查项 | 缺失影响 | 修复参考 |
|---|--------|---------|---------|
| 9 | **代码审查流程？** (AI review/human review) | 质量无保证 | [优化指南 §3](optimization-guide.md) |
| 10 | **测试覆盖？** (Agent 自测) | 改了不知道坏了没 | [优化指南 §3](optimization-guide.md) |
| 11 | **评估体系？** (evals) | 不知道优化是否有效 | [优化指南 §1](optimization-guide.md) |

### 1.4 协作架构层 (Multi-Agent)

| # | 检查项 | 缺失影响 | 修复参考 |
|---|--------|---------|---------|
| 12 | **Agent 职责分工？** | 一人干所有事→低效 | [架构指南 §4](architecture-guide.md) |
| 13 | **任务协调机制？** (Task Queue) | 任务混乱/重复/遗漏 | [架构指南 §3](architecture-guide.md) |
| 14 | **Agent 间通信？** (Message Bus) | 信息孤岛 | [架构指南 §3](architecture-guide.md) |
| 15 | **状态持久化？** (Checkpoint) | 中断后丢失进度 | [架构指南 §6](architecture-guide.md) |

### 诊断结果

```
❌ 0-3 项: 基础缺失 → 从 Step 2 开始
⚠️ 4-8 项: 部分就绪 → 按 Step 2 补齐缺失项
✅ 9+ 项:  基础就绪 → 跳到 Step 3 深度优化
```

---

## Step 2: 实施 — 按优先级建设

### 实施路线图

```
Phase 1: 基础 (必做) ────────────────────────────────────────
│
├── 2.1 建立项目配置 ← 一切的基础
│   CC: CLAUDE.md (索引模式, <30行)
│   Cursor: .cursorrules
│   API: system_prompt config
│   → 详见 [通用版 §1] 或 [CC版 §1]
│
├── 2.2 建立权限体系 ← 安全底线
│   CC: settings.json permissions
│   Cursor: allow/deny lists
│   API: tool whitelist + guardrails
│   → 详见 [通用版 §2]
│
├── 2.3 建立记忆系统 ← 跨会话持续
│   CC: .claude/memory/ + MEMORY.md 索引
│   Cursor: 项目规则文件
│   API: external memory store
│   → 详见 [CC版 §2] 或 [架构指南 §5.3]
│
└── 2.4 建立审查循环 ← 质量保证
    CC: 代码变更后自动 review (hooks)
    API: generator-evaluator 模式
    → 详见 [优化指南 §3]

Phase 2: 增强 (推荐) ────────────────────────────────────────
│
├── 2.5 Context 优化 ← 效率提升
│   Progressive Disclosure: 按需加载
│   Budget: 70% 阈值压缩
│   记忆索引: <200 行
│   → 详见 [架构指南 §5]
│
├── 2.6 工具优化 ← 能力扩展
│   MCP: 按域分组, 7±2 个/域
│   Skills: 大块知识按需加载
│   → 详见 [CC版 §3] 或 [通用版 §3]
│
└── 2.7 质量体系 ← 可度量
    Evals: 关键路径评估
    自动化测试: Agent 执行前
    → 详见 [优化指南 §1]

Phase 3: 高级 (大型项目) ──────────────────────────────────────
│
├── 2.8 多 Agent 架构 ← 复杂任务
│   选择协作模式 (Pipeline/Parallel/Hierarchical)
│   建立协调层 (Orchestrator + Task Queue)
│   → 详见 [架构指南 §2-4]
│
└── 2.9 成本/延迟优化 ← 规模化
    模型分级: 简单任务用小模型
    缓存: 相似请求复用
    → 详见 [优化指南 §2]
```

### 每步的时间投入参考

| Phase | 内容 | 预期效果 |
|-------|------|---------|
| Phase 1 (2.1-2.4) | 基础 Harness | AI 从"随机"变"可控" |
| Phase 2 (2.5-2.7) | Context + 质量 | AI 从"可用"变"高效" |
| Phase 3 (2.8-2.9) | 多Agent + 优化 | 从"单兵"变"团队" |

---

## Step 3: 深度优化 — 针对具体问题

完成基础建设后，针对具体问题深入优化：

### 问题 → 方向 → 参考

| 症状 | 根因 | 方向 | 参考 |
|------|------|------|------|
| AI 总是不理解项目意图 | Context 不足 | 增强项目配置 + 记忆 | [CC版 §1-2](practical-guide-cc.md) |
| AI 经常犯同类错误 | 无反馈循环 | Hooks + Rules + Memory | [通用版 §4](practical-guide-universal.md) |
| AI 响应变慢/质量下降 | Context 膨胀 | Progressive Disclosure | [架构指南 §5](architecture-guide.md) |
| 任务太复杂 AI 搞不定 | 单 Agent 能力不足 | 多 Agent 分工 | [架构指南 §2-4](architecture-guide.md) |
| 成本越来越高 | 无预算控制 | 模型分级 + 缓存 | [优化指南 §2](optimization-guide.md) |
| 质量不稳定 | 无评估体系 | Evals + Review | [优化指南 §1,3](optimization-guide.md) |
| 跨会话知识丢失 | 无持久化 | 记忆系统 | [架构指南 §5.3](architecture-guide.md) |
| Agent 间协作混乱 | 无协调机制 | Orchestrator + Task Queue | [架构指南 §3](architecture-guide.md) |

---

## Step 4: 验证 — 检查优化效果

### 验证清单

```
□ AI 是否理解项目约定？ → 给一个任务，看是否遵循规范
□ 权限是否生效？ → 尝试一个应被拒绝的操作
□ 记忆是否持久？ → 新会话中问上次学到的内容
□ Context 是否受控？ → 检查 context 使用量 < 70%
□ 质量是否提升？ → 对比优化前后的 eval 结果
□ 多 Agent 是否协作？ → 跑一个需要 2+ Agent 的任务
□ 成本是否降低？ → 对比每任务 token 消耗
```

### 持续优化循环

```
度量 → 发现问题 → 定位根因 → 参考对应文档 → 实施 → 验证 → 度量
  │                                                          │
  └──────────────── 每个迭代周期 ─────────────────────────────┘
```

---

## Step 5: 深度探索 — 当你需要理解"为什么"

实践操作解决了"怎么做"。当你需要理解背后的原理、做架构决策、或遇到实践指南无法覆盖的问题时，进入深度探索。

### 探索路径：按问题触发

```
你在做什么时遇到了理解瓶颈？

"不知道怎么设计 Agent 的控制流"
  → 你需要理解 Agent 设计原则
  → 阅读: concepts/12-factor-agents.md (12 条原则)
  → 然后: concepts/anthropic-agent-design.md (Anthropic 官方设计)
  → 深潜: concepts/llm-orchestration.md (编排模式)

"不知道为什么 Context 膨胀后 AI 变笨了"
  → 你需要理解 Context Rot
  → 阅读: concepts/context-engineering.md §2 (Context Rot)
  → 然后: concepts/context-engineering.md §3-4 (Write/Select/Compress/Isolate)
  → 实践: architecture-guide.md §5 (Context 层实现)

"不确定该用单 Agent 还是多 Agent"
  → 你需要理解 Agentic 模式
  → 阅读: concepts/agentic-design-patterns.md (4 大模式)
  → 然后: concepts/compound-ai-systems.md (复合系统设计)
  → 决策: architecture-guide.md §2 (5 种协作模式对比)

"不确定怎么控制 Agent 不做危险的事"
  → 你需要理解 Guardrails
  → 阅读: concepts/guardrails-progressive-autonomy.md (渐进自治)
  → 然后: concepts/harness-engineering.md (Harness 7 组件)
  → 实践: practical-guide-universal.md §4 (护栏配置)

"不知道怎么评估 AI 系统的好坏"
  → 你需要理解 Eval-Driven Development
  → 阅读: concepts/eval-driven-development.md
  → 然后: optimization-guide.md §1 (质量评估框架)
  → 实践: optimization-guide.md §3 (自动化质量门)

"想理解 AI 软件工程的整体范式"
  → 你需要理解 Software 3.0
  → 阅读: concepts/software-3.0.md (范式转变)
  → 然后: concepts/agentic-engineering.md (Agentic Engineering)
  → 全景: concepts/cognitive-architectures.md (认知架构)

"想用 Spec 驱动 AI 开发"
  → 你需要理解 Spec-Driven Development
  → 阅读: concepts/spec-driven-development.md
  → 实践: practical-guide-cc.md §3 (Skills as specs)

"想了解社区实践者的思路"
  → 阅读: concepts/agentic-patterns-willison.md (Simon Willison 模式)
```

### 知识层级

```
Layer 1: 实践 (你现在在这)
├── practical-guide.md          ← 操作入口
├── practical-guide-*.md        ← 按工具的实践细节
├── architecture-guide.md       ← 架构蓝图
└── optimization-guide.md       ← 优化方法
    │
    │ 当你需要理解 WHY 而不只是 HOW
    ▼
Layer 2: 概念 (理解原理)
├── harness-engineering.md      ← Harness 范式定义
├── context-engineering.md      ← Context 工程理论
├── 12-factor-agents.md         ← Agent 设计原则
├── compound-ai-systems.md      ← 复合系统设计
├── eval-driven-development.md  ← 评估驱动开发
└── guardrails-*.md             ← 安全与自治
    │
    │ 当你需要设计决策的底层逻辑
    ▼
Layer 3: 深潜 (设计哲学)
├── software-3.0.md             ← 范式转变
├── agentic-engineering.md      ← Agentic 工程观
├── cognitive-architectures.md  ← 认知架构
├── anthropic-agent-design.md   ← Anthropic 官方设计
├── agentic-design-patterns.md  ← 4 大设计模式
└── llm-orchestration.md        ← 编排模式
```

### 阅读策略

**快速参考 (5 分钟)：** 只看 Layer 1，按 Step 2 路线图执行
**深度理解 (30 分钟)：** 遇到问题时读 Layer 2 对应文档
**架构决策 (2 小时)：** 重大决策前读 Layer 3，理解设计哲学
**全量学习：** 按 `_index.md` 导航顺序阅读

---

## 文档索引

### Layer 1: 实践指南

| 文档 | 核心内容 | 何时阅读 |
|------|---------|---------|
| [practical-guide-universal.md](practical-guide-universal.md) | Harness 7 组件 × 4 工具对照 | 建立基础时 |
| [practical-guide-cc.md](practical-guide-cc.md) | CLAUDE.md, skills, memory, hooks | 用 CC 时 |
| [practical-guide-oc.md](practical-guide-oc.md) | 框架选择, Agent 循环, 代码模板 | SDK/API 开发时 |
| [architecture-guide.md](architecture-guide.md) | 5 层蓝图, 5 种协作模式, Context 层 | 设计架构时 |
| [optimization-guide.md](optimization-guide.md) | Quality/Cost/Latency 三维优化 | 优化系统时 |

### Layer 2: 概念原理

| 文档 | 核心内容 | 何时阅读 |
|------|---------|---------|
| [harness-engineering.md](concepts/harness-engineering.md) | Harness 范式定义, 7 组件体系 | 理解整体框架时 |
| [context-engineering.md](concepts/context-engineering.md) | 逐步揭露, Context Rot, 记忆管理 | AI 能力下降时 |
| [12-factor-agents.md](concepts/12-factor-agents.md) | Agent 设计 12 原则 | 设计 Agent 时 |
| [compound-ai-systems.md](concepts/compound-ai-systems.md) | 复合 AI 系统设计 | 系统复杂性增长时 |
| [eval-driven-development.md](concepts/eval-driven-development.md) | 评估驱动开发 | 建立质量体系时 |
| [guardrails-progressive-autonomy.md](concepts/guardrails-progressive-autonomy.md) | 渐进自治, 安全边界 | Agent 自主性提升时 |

### Layer 3: 设计哲学

| 文档 | 核心内容 | 何时阅读 |
|------|---------|---------|
| [software-3.0.md](concepts/software-3.0.md) | AI 时代的软件范式 | 理解大方向时 |
| [agentic-engineering.md](concepts/agentic-engineering.md) | Agentic 工程观 | 设计工程流程时 |
| [cognitive-architectures.md](concepts/cognitive-architectures.md) | 认知架构设计 | 深入理解 Agent 思维时 |
| [anthropic-agent-design.md](concepts/anthropic-agent-design.md) | Anthropic 官方 Agent 设计 | 遵循最佳实践时 |
| [agentic-design-patterns.md](concepts/agentic-design-patterns.md) | 4 大设计模式 | 选择设计模式时 |
| [llm-orchestration.md](concepts/llm-orchestration.md) | LLM 编排模式 | 多步骤流程设计时 |
| [agentic-patterns-willison.md](concepts/agentic-patterns-willison.md) | Simon Willison 实践模式 | 社区实践参考 |
| [spec-driven-development.md](concepts/spec-driven-development.md) | Spec 驱动开发 | 需求→代码自动化时 |

---

> **Source:** AI Engineering Frontier KB
>
> Last updated: 2026-03-27
