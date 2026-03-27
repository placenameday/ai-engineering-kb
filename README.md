---
name: ai-engineering-kb
description: >
  Knowledge base covering frontier AI engineering concepts and philosophies (2025-2026). Organized
  into concept deep-dives and tooling surveys.
survey_date: 2026-03-05
lang: en
---

# AI Engineering Knowledge Base

A curated reference covering frontier AI engineering concepts, design patterns, and tooling as of 2026. Survey date: 2026-03-05.

---

## Directory Structure

```
ai-engineering-kb/
├── README.md                    # This file
├── concepts/                    # Individual concept deep-dives
│   ├── _index.md                # Concept map + key thinkers table
│   ├── context-engineering.md   # Context Engineering (merged deep-dive)
│   ├── software-3.0.md          # Software 3.0
│   ├── agentic-engineering.md   # Vibe Coding → Agentic Engineering
│   ├── agentic-design-patterns.md  # Agentic Design Patterns (Andrew Ng)
│   ├── compound-ai-systems.md   # Compound AI Systems (BAIR)
│   ├── 12-factor-agents.md      # 12-Factor Agents (Dex Horthy)
│   ├── eval-driven-development.md  # Evaluation-Driven Development
│   ├── spec-driven-development.md  # Spec-Driven Development
│   ├── cognitive-architectures.md  # Cognitive Architectures for Language Agents
│   ├── anthropic-agent-design.md   # Anthropic's Agent Design Principles
│   ├── agentic-patterns-willison.md  # Agentic Engineering Patterns (Willison)
│   ├── guardrails-progressive-autonomy.md  # Guardrails & Progressive Autonomy
│   └── llm-orchestration.md     # LLM Orchestration as Discipline
└── tooling/                     # Tool surveys and ecosystem catalogs
    ├── ai-engineering-survey-2026.md  # 2026 AI Engineering comprehensive survey (Chinese)
    ├── research-tools-survey.md  # AI research writing tools (Chinese)
    └── skills-ecosystem.md       # Claude Code skills & plugins ecosystem
```

---

## Five-Layer Framework (AI工程五层模型)

```
 ┌─────────────────────────────────────────────────────────────────┐
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
  Context Engineering (上下文工程) — the cross-cutting core discipline.
```

> See [concepts/_index.md](concepts/_index.md) for layer details, learning path, relationship matrix, and key thinkers.

---

## All Articles

### L1: Paradigm & Vision

| Article | Description |
|---------|-------------|
| [concepts/software-3.0.md](concepts/software-3.0.md) | Software 3.0 — natural language as programming language (Karpathy) |
| [concepts/agentic-engineering.md](concepts/agentic-engineering.md) | Vibe Coding → Agentic Engineering spectrum |

### L2: System Architecture

| Article | Description |
|---------|-------------|
| [concepts/compound-ai-systems.md](concepts/compound-ai-systems.md) | Intelligence from multiple interacting components (BAIR/Zaharia) |
| [concepts/cognitive-architectures.md](concepts/cognitive-architectures.md) | Cognitive science principles for LLM agent design (CoALA, EGI, CCA) |
| [concepts/llm-orchestration.md](concepts/llm-orchestration.md) | Coordination layer for multi-model, multi-agent systems |

### L3: Design Patterns

| Article | Description |
|---------|-------------|
| [concepts/agentic-design-patterns.md](concepts/agentic-design-patterns.md) | Four foundational agent workflow patterns (Andrew Ng) |
| [concepts/anthropic-agent-design.md](concepts/anthropic-agent-design.md) | Simplicity, transparency, ACI — Anthropic's three principles |
| [concepts/agentic-patterns-willison.md](concepts/agentic-patterns-willison.md) | Practical coding patterns from Simon Willison |

### L4: Engineering Methodology

| Article | Description |
|---------|-------------|
| [concepts/eval-driven-development.md](concepts/eval-driven-development.md) | TDD equivalent for AI — evals as first-class artifacts |
| [concepts/spec-driven-development.md](concepts/spec-driven-development.md) | Intent as source of truth; AI generates from specs |
| [concepts/12-factor-agents.md](concepts/12-factor-agents.md) | 12 engineering principles for production LLM apps (Dex Horthy) |

### L5: Safety & Governance

| Article | Description |
|---------|-------------|
| [concepts/guardrails-progressive-autonomy.md](concepts/guardrails-progressive-autonomy.md) | Layered safety architecture for production agents |

### Cross-Cutting Discipline

| Article | Description |
|---------|-------------|
| [concepts/context-engineering.md](concepts/context-engineering.md) | Context Engineering — MCP, context rot, Anthropic framework, advanced patterns |
| [concepts/_index.md](concepts/_index.md) | **Framework index** — layer details, learning path, relationship matrix, key thinkers |

### Tooling

| Article | Description |
|---------|-------------|
| [tooling/ai-engineering-survey-2026.md](tooling/ai-engineering-survey-2026.md) | **2026年初AI工程前沿综述** — 十章全景报告，覆盖五层框架全部主题（中文） |
| [tooling/research-tools-survey.md](tooling/research-tools-survey.md) | AI research writing tools survey (中文) |
| [tooling/skills-ecosystem.md](tooling/skills-ecosystem.md) | Claude Code skills & plugins ecosystem catalog |

---

## How to Add New Content

### File naming
- Concept files: `concepts/<kebab-case-name>.md`
- Tooling/survey files: `tooling/<kebab-case-name>.md`

### Required frontmatter
Every file must begin with:

```yaml
---
name: <kebab-case-name>
description: >
  One-line description of the concept (used for quick reference)
survey_date: YYYY-MM-DD
lang: en
---
```

### Content guidelines
- Keep ALL sources, tables, and code blocks intact
- Use `---` horizontal rules between major sections
- Update `concepts/_index.md` when adding a new concept (add a row to the Concept File Index table)
- Update this README's article table when adding any file
- Bilingual content is welcome — `research-tools-survey.md` is intentionally in Chinese
