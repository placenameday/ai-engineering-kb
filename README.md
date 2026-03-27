---
name: ai-engineering-kb
description: >
  Knowledge base covering frontier AI engineering concepts and philosophies (2025-2026). Organized
  into concept deep-dives and tooling surveys.
survey_date: 2026-03-27
lang: en
---

# AI Engineering Knowledge Base

A curated reference covering frontier AI engineering concepts, design patterns, and tooling as of 2026. Survey date: 2026-03-27.

---

## How to Navigate

```
你想要什么？

"优化我的 AI 项目" ──→ practical-guide.md (操作入口)
"理解某个概念" ──→ concepts/_index.md (概念导航)
"了解工具生态" ──→ tooling/ (工具调查)
"全面学习" ──→ 按下方六层模型从 L0 读到 L5
```

---

## Practice Guides (实践指南)

> **Entry point:** [practical-guide.md](practical-guide.md) — 5 步操作：定位→诊断→实施→验证→深度探索

| File | Role | When to Read |
|------|------|-------------|
| [practical-guide.md](practical-guide.md) | 操作入口 (诊断→实施→验证→深潜) | 拿到一个项目要优化时 |
| [practical-guide-universal.md](practical-guide-universal.md) | Harness 7 组件 × 4 工具对照 | 建基础 / 跨工具参考 |
| [practical-guide-cc.md](practical-guide-cc.md) | Claude Code 配置细节 | 用 CC 时 |
| [practical-guide-oc.md](practical-guide-oc.md) | SDK/API 代码模板 | 用 API 开发 Agent 时 |
| [architecture-guide.md](architecture-guide.md) | Multi-Agent 架构设计 | 设计系统架构时 |
| [optimization-guide.md](optimization-guide.md) | Quality/Cost/Latency 优化 | 优化系统效率时 |

### File Relationships

```
practical-guide.md (入口：诊断+路线图)
    │
    ├─→ practical-guide-universal.md (Harness 理论 × 工具映射)
    │       └─→ concepts/harness-engineering.md (Harness 范式定义)
    │
    ├─→ practical-guide-cc.md (CC 配置)
    │
    ├─→ practical-guide-oc.md (代码模板)
    │       └─→ concepts/12-factor-agents.md (Agent 原则)
    │
    ├─→ architecture-guide.md (架构蓝图)
    │       ├─→ practical-guide-oc.md §3 (Harness 代码实现)
    │       └─→ concepts/context-engineering.md (Context 理论)
    │
    └─→ optimization-guide.md (优化技术)
            └─→ architecture-guide.md §5 (Context 层设计)
```

---

## Concepts (概念深潜)

### Six-Layer Framework (AI工程六层模型)

**Paradigm Hierarchy:**
- **Harness Engineering** (2026) — "How does the system behave?"
  - ⊃ **Context Engineering** (2025) — "What does the model see?"
    - ⊃ **Prompt Engineering** (2022-2024) — "What do we ask?"

```
 ┌─────────────────────────────────────────────────────────────────┐
 │  L0  PARADIGM EVOLUTION  (范式演进)                             │
 │  "How has AI engineering evolved?"                              │
 │  Prompt (2022-24) → Context (2025) → Harness (2026)            │
 ├─────────────────────────────────────────────────────────────────┤
 │  L1  PARADIGM & VISION  (范式与愿景)                            │
 │  "Why is everything changing?"                                  │
 │  Software 3.0 · Vibe Coding → Agentic Engineering              │
 ├─────────────────────────────────────────────────────────────────┤
 │  L2  SYSTEM ARCHITECTURE  (系统架构)                            │
 │  "How do you structure AI systems?"                             │
 │  Compound AI Systems · Cognitive Architectures · LLM Orchestr. │
 ├─────────────────────────────────────────────────────────────────┤
 │  L3  DESIGN PATTERNS  (设计模式)                        ┌──────┤
 │  "What patterns solve recurring problems?"              │  C   │
 │  Agentic Design Patterns · Anthropic Agent Design ·     │  O   │
 │  Agentic Patterns (Willison)                            │  N   │
 ├─────────────────────────────────────────────────────────┐│  T   │
 │  L4  ENGINEERING METHODOLOGY  (工程方法论)              ││  E   │
 │  "What process and principles guide development?"       ││  X   │
 │  Eval-Driven Dev · Spec-Driven Dev · 12-Factor Agents   ││  T   │
 ├─────────────────────────────────────────────────────────┐│      │
 │  L5  SAFETY & GOVERNANCE  (安全与治理)                  ││  E   │
 │  "How do you deploy and operate responsibly?"           ││  N   │
 │  Guardrails & Progressive Autonomy                      ││  G   │
 └─────────────────────────────────────────────────────────┘└──────┘
```

> See [concepts/_index.md](concepts/_index.md) for layer details, learning path, relationship matrix, and key thinkers.

### All Articles

#### L0: Paradigm Evolution

| Article | Description |
|---------|-------------|
| [harness-engineering.md](concepts/harness-engineering.md) | **Harness Engineering (2026)** — the operational environment around AI agents |

#### L1: Paradigm & Vision

| Article | Description |
|---------|-------------|
| [software-3.0.md](concepts/software-3.0.md) | Software 3.0 — natural language as programming language (Karpathy) |
| [agentic-engineering.md](concepts/agentic-engineering.md) | Vibe Coding → Agentic Engineering spectrum |

#### L2: System Architecture

| Article | Description |
|---------|-------------|
| [compound-ai-systems.md](concepts/compound-ai-systems.md) | Intelligence from multiple interacting components (BAIR/Zaharia) |
| [cognitive-architectures.md](concepts/cognitive-architectures.md) | Cognitive science principles for LLM agent design (CoALA, EGI, CCA) |
| [llm-orchestration.md](concepts/llm-orchestration.md) | Coordination layer for multi-model, multi-agent systems |

#### L3: Design Patterns

| Article | Description |
|---------|-------------|
| [agentic-design-patterns.md](concepts/agentic-design-patterns.md) | Four foundational agent workflow patterns (Andrew Ng) |
| [anthropic-agent-design.md](concepts/anthropic-agent-design.md) | Simplicity, transparency, ACI — Anthropic's three principles |
| [agentic-patterns-willison.md](concepts/agentic-patterns-willison.md) | Practical coding patterns from Simon Willison |

#### L4: Engineering Methodology

| Article | Description |
|---------|-------------|
| [eval-driven-development.md](concepts/eval-driven-development.md) | TDD equivalent for AI — evals as first-class artifacts |
| [spec-driven-development.md](concepts/spec-driven-development.md) | Intent as source of truth; AI generates from specs |
| [12-factor-agents.md](concepts/12-factor-agents.md) | 12 engineering principles for production LLM apps (Dex Horthy) |

#### L5: Safety & Governance

| Article | Description |
|---------|-------------|
| [guardrails-progressive-autonomy.md](concepts/guardrails-progressive-autonomy.md) | Layered safety architecture for production agents |

#### Cross-Cutting

| Article | Description |
|---------|-------------|
| [context-engineering.md](concepts/context-engineering.md) | Context Engineering — MCP, context rot, Anthropic framework, advanced patterns |
| [_index.md](concepts/_index.md) | **Framework index** — layer details, learning path, relationship matrix, key thinkers |

### Tooling

| Article | Description |
|---------|-------------|
| [ai-engineering-survey-2026.md](tooling/ai-engineering-survey-2026.md) | **2026年初AI工程前沿综述** — 十章全景报告（中文） |
| [research-tools-survey.md](tooling/research-tools-survey.md) | AI research writing tools survey (中文) |
| [skills-ecosystem.md](tooling/skills-ecosystem.md) | Claude Code skills & plugins ecosystem catalog |

---

## How to Add New Content

### File naming
- Concept files: `concepts/<kebab-case-name>.md`
- Tooling/survey files: `tooling/<kebab-case-name>.md`
- Practice guides: `<kebab-case-name>.md` (root level)

### Required frontmatter

```yaml
---
name: <kebab-case-name>
description: >
  One-line description
survey_date: YYYY-MM-DD
lang: en
---
```

### Content guidelines
- Keep ALL sources, tables, and code blocks intact
- Use `---` horizontal rules between major sections
- Update `concepts/_index.md` when adding a new concept
- Update this README when adding any file
- Bilingual content is welcome
