---
name: anthropic-agent-design
description: >
  Three principles for effective agents from Anthropic: maintain simplicity, prioritize transparency,
  and carefully craft the Agent-Computer Interface (ACI). Key distinction: workflows (predefined code
  paths) vs. agents (model-directed processes). Most problems don't need agents.
survey_date: 2026-03-05
lang: en
---

# Anthropic's Agent Design Principles (Anthropic智能体设计原则)

> Survey date: 2026-03-05.

---

**Core idea:** Three principles for effective agents: (1) Maintain simplicity, (2) Prioritize transparency, (3) Carefully craft the Agent-Computer Interface (ACI).

**Key architectural distinction:** **Workflows** (LLMs orchestrated through predefined code paths) vs. **Agents** (LLMs dynamically direct their own processes). Both are "agentic systems" but architecturally different.

**Key proponents:** Anthropic engineering team (Erik Schluntz, Barry Zhang).

**Practical guidance:**
- Start with LLM APIs directly; many patterns need only a few lines of code
- Frameworks can obscure what's happening; understand the underlying code
- Use agents only when flexibility and model-driven decision-making are needed at scale
- For well-defined tasks, workflows (predictable, consistent) are usually sufficient

**Anti-pattern warning:** "Incorrect assumptions about what's under the hood are a common source of customer error" when using agent frameworks.

**Why it matters:** Provides a corrective to the industry tendency to over-engineer. Most problems don't need agents.

**Sources:**
- [Anthropic: Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)
- [Anthropic Cookbook: Agent Patterns](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns/agents)
