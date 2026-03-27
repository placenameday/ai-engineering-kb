---
name: context-engineering
description: >
  The systematic discipline of designing and optimizing all contextual information flowing through AI
  systems — not just prompts, but retrieved documents, memory, tool descriptions, state, and control
  flow. Covers MCP, progressive disclosure, context rot, Anthropic's Write/Select/Compress/Isolate
  framework, Claude Code memory hierarchy, and advanced patterns (CORPGEN, Synaptic Labs, MemGPT).
survey_date: 2026-03-27
lang: en
---

# Context Engineering (上下文工程)

> Survey section updated 2026-03-27. Deep-dive research conducted 2026-03-02.
> Sources: Anthropic official docs, academic papers, community best practices, frontier survey.

---

## Relationship to Harness Engineering

**Context Engineering is a subset of Harness Engineering.**

| Paradigm | Years | Question | Scope |
|----------|-------|----------|-------|
| Prompt Engineering | 2022-2024 | "What to ask?" | Wording |
| Context Engineering | 2025 | "What to send?" | Everything model sees |
| **Harness Engineering** | 2026 | "How does it operate?" | Full operational environment |

```
┌─────────────────────────────────────────────────────────────┐
│                  HARNESS ENGINEERING                         │
│            "How does the whole system behave?"               │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────────┐    │
│  │            CONTEXT ENGINEERING                       │    │
│  │         "What does the model see?"                   │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │         PROMPT ENGINEERING                   │    │    │
│  │  │        "What do we ask?"                     │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  Other Harness Components:                                   │
│  - Permissions (access control)                              │
│  - Hooks (lifecycle intervention)                            │
│  - Guardrails (safety boundaries)                            │
│  - State management (persistence)                            │
│  - Feedback loops (quality validation)                       │
│  - Isolation (sandboxing)                                    │
└─────────────────────────────────────────────────────────────┘
```

> See [harness-engineering.md](harness-engineering.md) for the full paradigm.

---

## Overview (Frontier Survey)

**Core idea:** The systematic discipline of designing and optimizing ALL contextual information flowing through AI systems — not just prompts, but retrieved documents, memory, tool descriptions, state, and control flow. Replaces "prompt engineering" as the central discipline.

**Key proponents:** Andrej Karpathy ("Context engineering is the delicate art and science of filling the context window with just the right information for the next step"), Anthropic engineering team, Gartner.

**Why it matters:** Prompt engineering = "what you say"; context engineering = "everything the model sees." As models improve, the bottleneck shifts from wording to information architecture. Gartner predicts 40% of enterprise apps will feature task-specific AI agents by late 2026 — all requiring robust context engineering.

**Key framework:** Model Context Protocol (MCP) — the "USB-C for AI integrations," now governed by Linux Foundation, adopted by Anthropic/OpenAI/Google/Microsoft. Defines three primitives: Tools, Resources, Prompts. 97M+ monthly SDK downloads.

**Sources:**
- [Anthropic: Effective Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Karpathy's Context Engineering handbook](https://github.com/davidkimai/Context-Engineering)
- [MCP Architecture & Design Philosophy](https://modelcontextprotocol.info/docs/concepts/architecture/)
- [MCP Specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)

---

## Deep Dive: Context Engineering & Memory Management for AI Agent Systems

---

### 1. "Progressive Disclosure" — Terminology & Status

**Yes, it is an established term**, widely adopted in the AI/LLM agent community (2025-2026).

| Term | Scope | Source |
|------|-------|--------|
| **Progressive Disclosure** | General pattern — reveal context incrementally | Anthropic, Synaptic Labs |
| **Just-In-Time Context** | Synonymous — load when needed | Anthropic official docs |
| **Context Engineering** | Umbrella discipline | Anthropic, LangChain, LlamaIndex |
| **Bounded Context Packs** | Domain-specific tool grouping | Synaptic Labs (DDD-inspired) |
| **Meta-Tool Pattern** | 2 tools (discover + execute) replace N tools | Synaptic Labs |
| **Tiered Memory** | Working / Structured LTM / Semantic layers | Microsoft CORPGEN (Feb 2026) |

---

### 2. Context Rot — Why This Matters

**Chroma Research (Jul 2025)**: 18 SOTA models tested. Performance degrades non-linearly with context length.

**arXiv 2510.05381**: Even with **perfect retrieval**, performance drops 13.9%-85% as length increases.

**Jan 2026 study**: Critical threshold: ~40% of context window capacity. For 200K: stay below ~80K tokens.

**Core insight**: A focused 300-token context often outperforms an unfocused 113,000-token context.

---

### 3. Four Core Patterns (Anthropic Framework)

| Pattern | What | Claude Code Implementation |
|---------|------|---------------------------|
| **Write** | Persist externally | `.claude/memory/` files |
| **Select** | Pull relevant context in | Skills (description match), @imports |
| **Compress** | Reduce token usage | `/compact`, agent summaries |
| **Isolate** | Partition across systems | Sub-agents with own context |

---

### 4. Claude Code Specific Patterns

#### Memory Hierarchy (priority order)
1. Enterprise Policy (highest)
2. Project CLAUDE.md
3. Project Rules (.claude/rules/*.md)
4. User CLAUDE.md

#### Always-Loaded vs On-Demand

| Layer | Loading | Budget Target |
|-------|---------|---------------|
| CLAUDE.md | Always | ~50-200 lines |
| @imports | Always (expanded) | Use sparingly |
| .claude/rules/*.md | Conditional (path-scoped) | Per-directory context |
| .claude/skills/ | On-demand (description match) | 2% of context for descriptions |
| .claude/memory/ | On-demand (agent reads) | Unlimited |

#### Session Management
- `/clear` between unrelated tasks
- `/compact` at 70% capacity (auto at 95% is too late)
- Custom: `/compact Focus on X`
- 30-minute focused sprints

---

### 5. Advanced Patterns

#### Microsoft CORPGEN (Feb 2026)
Three tiers: Working Memory -> Structured LTM -> Semantic Memory (Mem0). Up to 3.5x improvement.

#### Synaptic Labs Bounded Context Packs
7+/-2 tools per domain. Meta-tool pattern: 85-95% token reduction in schemas.

#### MemGPT/Letta
Context window as RAM, external storage as disk. Agent manages "page in/page out."

---

### 6. Anti-Patterns

1. Stuffing CLAUDE.md (every line costs tokens on every message)
2. Generic instructions ("Write clean code" wastes tokens)
3. Stale memory (audit every 2 weeks)
4. Too many MCP tools (35 tools = ~26K tokens overhead)
5. Over-importing (@imports are always loaded)
6. Ignoring context budget (at 90% fill, model is measurably less capable)

---

### 7. Context Placement Decision Criteria (this project)

| Destination | Criteria |
|-------------|----------|
| **Skill** | Large file (>100 lines) + infrequently used + complete self-contained knowledge body → `.claude/skills/` with description |
| **Rule** | Directive ("don't do X") + path-matchable + always relevant → `.claude/rules/` with path_patterns |
| **Memory** | Reference type (expression lib/cases) \| accumulative \| sensitive (personal info) \| system-level (not path-specific) |
| **Not Rule** | Module-level slices — path can't distinguish modules; use on-demand skill loading instead |

---

## Sources

### Anthropic Official
- [Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Claude Code Memory Docs](https://code.claude.com/docs/en/memory)
- [Claude Code Skills Docs](https://code.claude.com/docs/en/skills)

### Research Papers
- [Context Rot (Chroma, Jul 2025)](https://research.trychroma.com/context-rot)
- [Context Length Alone Hurts (arXiv 2510.05381)](https://arxiv.org/abs/2510.05381)
- [Intelligence Degradation (Jan 2026)](https://arxiv.org/html/2601.15300)
- [Mem0 Paper (arXiv 2504.19413)](https://arxiv.org/pdf/2504.19413)

### Community
- [Meta-Tool Pattern (Synaptic Labs)](https://blog.synapticlabs.ai/bounded-context-packs-meta-tool-pattern)
- [Microsoft CORPGEN](https://www.marktechpost.com/2026/02/26/microsoft-research-introduces-corpgen/)
- [LangChain Context Engineering](https://blog.langchain.com/context-engineering-for-agents/)
