# KB Synthesis: Core Patterns (Agent, Context, Specs, MCP)

> Part 1 of kb-synthesis split. See also: `kb-synthesis-practice.md`
> Date: 2026-03-27

---

## 0. Paradigm Evolution (2026 Update)

**The Three-Phase Evolution:**

| Paradigm | Years | Question | Scope |
|----------|-------|----------|-------|
| Prompt Engineering | 2022-2024 | "What to ask?" | Wording, phrasing |
| Context Engineering | 2025 | "What to send?" | Everything model sees |
| **Harness Engineering** | 2026 | "How does it operate?" | Full operational environment |

**Key insight:** Context Engineering ⊂ Harness Engineering. The 2026 focus is on building infrastructure that controls agents, not more agents.

See: [harness-engineering.md](../../concepts/harness-engineering.md)

---

## 1. Agent Design Patterns & Orchestration

### The Three Complementary Lenses (Use All Three)

**Ng's Vocabulary** — Four behavioral building blocks:
1. **Reflection**: Have the LLM critique its own output and iterate. Low implementation difficulty.
2. **Tool Use**: LLM calls APIs/external systems via function calling or MCP. Medium difficulty.
3. **Planning**: Decompose complex tasks into sequential steps (ReAct, Chain-of-Thought). High difficulty.
4. **Multi-Agent Collaboration**: Specialized agents collaborate on subtasks. High difficulty.

**Anthropic's Philosophy** — Three design principles:
1. **Maintain Simplicity**: Start with direct LLM API calls. Don't reach for frameworks first.
2. **Prioritize Transparency**: Understand what's under the hood. Frameworks obscure critical behavior.
3. **Craft the ACI**: Tool descriptions, parameter schemas, error messages are UX for agents.

**Willison's Practice** — What actually works:
- **Context Offloading**: Move state out of prompts into durable storage.
- **Prompt Injection = "SQL Injection of our day"**: Treat with equal seriousness.
- AI-assisted development is expertise amplification, not replacement.

### Workflow vs. Agent Decision Matrix

| Dimension | Workflow | Agent |
|-----------|----------|-------|
| Control flow | Predefined code paths | Model dynamically decides |
| Predictability | High | Low |
| Best for | Well-defined tasks | Runtime flexibility |
| Debugging | Easy | Hard |

**Rule of thumb**: If you can draw the flowchart before runtime, use a workflow.

### Multi-Agent Orchestration

- **Five core components**: Task Router, Memory Layer, Tool Hub, Workflow Engine, Monitoring
- **Thin vs. Thick**: 80% scenarios → Thin frameworks (OpenAI Agents SDK, Pydantic AI). 20% → Thick (LangGraph, CrewAI).
- **MCP + A2A**: MCP = agent-to-tool. A2A = agent-to-agent. Production systems need both.

---

## 2. Context Engineering Principles

### Core Definition
Context engineering = designing EVERYTHING the model sees. THE central discipline of AI engineering.

### Anthropic's Four-Pattern Framework

| Pattern | Action | Claude Code Implementation |
|---------|--------|---------------------------|
| **Write** | Persist externally | `.claude/memory/` files |
| **Select** | Pull relevant context | Skills, @imports, RAG |
| **Compress** | Reduce token usage | `/compact`, agent summaries |
| **Isolate** | Partition systems | Sub-agents, sandboxes |

### Context Rot — Critical Facts

- **300-token focused context often outperforms 113,000-token unfocused context.**
- Critical threshold: ~40% of context window capacity.
- At 90% fill, model capability is measurably degraded.

### Claude Code Memory Hierarchy

1. Enterprise Policy
2. Project CLAUDE.md (always loaded, ~50-200 lines budget)
3. Project Rules `.claude/rules/*.md` (conditional, path-scoped)
4. User CLAUDE.md

### Context Placement Decision

| Destination | When |
|-------------|------|
| **Skill** | Large (>100 lines) + infrequent + self-contained |
| **Rule** | Directive + path-matchable + always relevant |
| **Memory** | Reference material, accumulative knowledge |

---

## 3. Spec-Driven Development

### Core Principles
- **"Intent is the source of truth"** replaces "code is the source of truth."
- **Pragmatic school**: Specs drive generation, executable code remains authoritative.

### Team Structure Inversion

| Role | Traditional | AI-Era |
|------|-------------|--------|
| Product judgment | 20% | **60%** |
| Engineering architecture | 60% | **30%** |
| Design precision | 20% | **10%** |

### DO/DON'T — Specs

| DO | DON'T |
|----|-------|
| Write specific constraints | Write aspirational generalities |
| Version specs alongside code | Treat specs as informal |
| Make rules path-scoped | Make every rule global |

---

## 4. MCP Integration

### Core Facts
- **"USB-C for AI integrations"** — standardized protocol.
- **Three primitives**: Tools, Resources, Prompts.
- Governed by Linux Foundation.

### DO/DON'T — MCP

| DO | DON'T |
|----|-------|
| Keep tool count manageable (7±2 per domain) | Install 35+ tools (~26K tokens overhead) |
| Write excellent tool descriptions | Write minimal/vague descriptions |
| Use Meta-Tool pattern for large tool sets | Expose every tool directly |

### A2A Protocol
- Complements MCP: MCP = agent→tool, A2A = agent→agent.
- Core concepts: Agent Cards, Tasks lifecycle, Capability Discovery.
