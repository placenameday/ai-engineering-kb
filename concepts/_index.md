---
name: concepts-index
description: >
  Organizational framework, concept map, learning path, and key thinkers reference for the
  AI engineering knowledge base. Uses a 5-layer model from paradigm vision to production safety,
  with harness engineering as the overarching discipline (2026 paradigm).
survey_date: 2026-03-27
lang: en
---

# AI Engineering Concepts: Organizational Framework

> 14 frontier concepts (survey date 2026-03-27). This index provides structure, not just a list.
>
> **综述报告：** [2026年初AI工程前沿综述](../tooling/ai-engineering-survey-2026.md) — 基于本框架的十章中文全景分析，含最新模型/框架/生态数据。

---

## 1. The Framework: Five Layers of AI Engineering (AI工程五层模型)

These concepts are not a flat collection. They form a layered stack. Each layer answers a different question, and each layer depends on understanding the ones above it.

```
 ╔═══════════════════════════════════════════════════════════════════╗
 ║  HARNESS ENGINEERING (驾驭工程) — 2026 Paradigm                    ║
 ║  "How does the whole system operate?"                             ║
 ║  Permissions · Hooks · Guardrails · State · Feedback Loops       ║
 ╠═══════════════════════════════════════════════════════════════════╣
 ║                                                                   ║
 ║  ┌─────────────────────────────────────────────────────────────┐ ║
 ║  │  L1  PARADIGM & VISION  (范式与愿景)                        │ ║
 ║  │  Software 3.0 · Vibe Coding → Agentic Engineering          │ ║
 ║  ├─────────────────────────────────────────────────────────────┤ ║
 ║  │  L2  SYSTEM ARCHITECTURE  (系统架构)                        │ ║
 ║  │  Compound AI Systems · Cognitive Architectures · LLM Orch. │ ║
 ║  ├─────────────────────────────────────────────────────────────┤ ║
 ║  │  L3  DESIGN PATTERNS  (设计模式)                    ┌──────┐│ ║
 ║  │  Agentic Design Patterns · Anthropic Agent Design │ C    ││ ║
 ║  ├────────────────────────────────────────────────────┐│ O    ││ ║
 ║  │  L4  ENGINEERING METHODOLOGY  (工程方法论)         ││ N    ││ ║
 ║  │  Eval-Driven Dev · Spec-Driven Dev · 12-Factor    ││ T    ││ ║
 ║  ├────────────────────────────────────────────────────┐│ E    ││ ║
 ║  │  L5  SAFETY & GOVERNANCE  (安全与治理)             ││ X    ││ ║
 ║  │  Guardrails & Progressive Autonomy                 ││ T    ││ ║
 ║  └────────────────────────────────────────────────────┘└──────┘│ ║
 ║                                                               │ ║
 ║     Context Engineering (上下文工程) ⊂ Harness Engineering    │ ║
 ║     "What the model sees" is a subset of "how it operates"    │ ║
 ╚═══════════════════════════════════════════════════════════════════╝
```

**2026 Paradigm Evolution:**
| Era | Paradigm | Question | Analogy |
|-----|----------|----------|---------|
| 2022-2024 | Prompt Engineering | What to ask? | Writing the email |
| 2025 | Context Engineering | What to send? | Attachments + context |
| **2026** | **Harness Engineering** | How does it operate? | The entire office |

### Why this structure?

- **L1 orients you.** Before building anything, understand *what changed* about software and *where you sit* on the practitioner spectrum.
- **L2 gives you architecture.** How to decompose AI systems into components, design memory and reasoning, and coordinate models.
- **L3 gives you patterns.** Concrete, reusable solutions to recurring design problems in agentic systems.
- **L4 gives you process.** How to spec, build, evaluate, and iterate on AI systems with engineering rigor.
- **L5 gives you safety.** How to deploy with guardrails, earn trust incrementally, and operate responsibly.
- **Context Engineering** is not a layer -- it is the discipline that runs through everything. Every layer requires managing what the model sees.

---

## 2. Layer Detail & Cross-Cutting Relationships

### L1: Paradigm & Vision (范式与愿景)

The "why" layer. These concepts establish the macro shift and the human role within it.

| Concept | File | Core Question |
|---------|------|---------------|
| Software 3.0 | [software-3.0.md](software-3.0.md) | What is the new paradigm of software? |
| Vibe Coding -> Agentic Engineering | [agentic-engineering.md](agentic-engineering.md) | Where am I on the practitioner maturity spectrum? |

**Cross-cutting links:** Software 3.0's "autonomy slider" connects to L5 (Guardrails). The "generator-verifier loop" connects to L4 (Eval-Driven Dev). Agentic Engineering's quality concerns connect to L4 (12-Factor Agents).

---

### L2: System Architecture (系统架构)

The "structure" layer. How to decompose, organize, and coordinate AI system components.

| Concept | File | Core Question |
|---------|------|---------------|
| Compound AI Systems | [compound-ai-systems.md](compound-ai-systems.md) | How do multiple components create intelligence together? |
| Cognitive Architectures | [cognitive-architectures.md](cognitive-architectures.md) | How do you design memory, reasoning, and decision-making? |
| LLM Orchestration | [llm-orchestration.md](llm-orchestration.md) | How do you coordinate models, agents, tools, and data? |

**Cross-cutting links:** Compound AI Systems provides the philosophy that L3 patterns implement. Cognitive Architectures' memory design requires Context Engineering. LLM Orchestration is the runtime realization of L3 patterns.

---

### L3: Design Patterns (设计模式)

The "solutions" layer. Reusable answers to recurring design problems.

| Concept | File | Core Question |
|---------|------|---------------|
| Agentic Design Patterns | [agentic-design-patterns.md](agentic-design-patterns.md) | What are the four foundational agent behaviors? |
| Anthropic Agent Design | [anthropic-agent-design.md](anthropic-agent-design.md) | When should you use agents vs. workflows? What's ACI? |
| Agentic Patterns (Willison) | [agentic-patterns-willison.md](agentic-patterns-willison.md) | What works in daily practice with coding agents? |

**Cross-cutting links:** Ng's patterns (Reflection, Tool Use, Planning, Multi-Agent) are behavioral building blocks for L2 architectures. Anthropic's simplicity principle is a corrective to over-engineering at every layer. Willison's context offloading is a practical expression of Context Engineering.

**Reading these three together:** Ng provides the *vocabulary* (four pattern types). Anthropic provides the *philosophy* (start simple, prefer workflows). Willison provides the *practice* (what actually works day-to-day). They are complementary, not competing.

---

### L4: Engineering Methodology (工程方法论)

The "process" layer. Principles and practices that bring engineering discipline to AI development.

| Concept | File | Core Question |
|---------|------|---------------|
| Evaluation-Driven Development | [eval-driven-development.md](eval-driven-development.md) | How do you measure and improve AI system quality? |
| Spec-Driven Development | [spec-driven-development.md](spec-driven-development.md) | How do you specify intent so AI generates correct code? |
| 12-Factor Agents | [12-factor-agents.md](12-factor-agents.md) | What engineering principles make agents production-grade? |

**Cross-cutting links:** Evals are the "generator-verifier loop" from L1 (Software 3.0). Spec-Driven Dev operationalizes the L1 shift from "code as source of truth" to "intent as source of truth." 12-Factor Agents' "own your context window" (Factor 5) is Context Engineering in practice. Factor 6 (Launch/Pause/Resume) connects to L5 (Progressive Autonomy).

---

### L5: Safety & Governance (安全与治理)

The "responsibility" layer. How to deploy AI systems that earn and deserve trust.

| Concept | File | Core Question |
|---------|------|---------------|
| Guardrails & Progressive Autonomy | [guardrails-progressive-autonomy.md](guardrails-progressive-autonomy.md) | How do you layer safety controls and increase autonomy responsibly? |

**Cross-cutting links:** Progressive Autonomy implements Karpathy's "autonomy slider" from L1. Guardrail architecture depends on L2 (system decomposition). HITL escalation is a pattern from L3. Evals from L4 determine when autonomy can increase.

---

### Overarching: Harness Engineering (驾驭工程)

| Concept | File | Core Question |
|---------|------|---------------|
| Harness Engineering | [harness-engineering.md](harness-engineering.md) | How do you build the operational environment around AI agents? |

**Harness Engineering (2026 paradigm)** is the discipline of building the complete operational environment around AI agents — permissions, hooks, tools, guardrails, state management, and feedback loops. It subsumes Context Engineering.

| Component | Purpose |
|-----------|---------|
| Permissions | Access control, tool-level restrictions |
| Hooks | Lifecycle intervention, deterministic control |
| Guardrails | Safety boundaries, control plane |
| State | Checkpoints, memory persistence |
| Feedback Loops | Linters, tests, evals, human review |

---

### Cross-Cutting: Context Engineering (上下文工程) ⊂ Harness Engineering

| Concept | File | Core Question |
|---------|------|---------------|
| Context Engineering | [context-engineering.md](context-engineering.md) | How do you design everything the model sees? |

**Context Engineering is a subset of Harness Engineering.** While Harness Engineering covers the entire operational environment ("how the system behaves"), Context Engineering focuses specifically on information flow ("what the model sees").

| Layer | How Context Engineering Manifests |
|-------|-----------------------------------|
| L2 Architecture | MCP protocol design, memory hierarchy, retrieval pipelines |
| L3 Patterns | Prompt construction, tool descriptions, ACI design |
| L4 Methodology | Context window curation (12-Factor #5), spec-as-context |
| L5 Safety | Input sanitization, context isolation, information boundaries |

**Key distinction:**
- Context Engineering: "Make sure the model sees the right database schema"
- Harness Engineering: "Ensure it still has to run the linter, pass tests, respect permissions"

---

## 3. Learning Path (学习路径)

For someone new to AI engineering, follow this numbered sequence. Each step builds on the previous.

```
 ORIENT                          ARCHITECT                      BUILD
 ──────                          ─────────                      ─────
 [1] Software 3.0           ──→  [3] Compound AI Systems   ──→  [5] Context Engineering
     Understand the               Grasp the multi-component      Master context design:
     paradigm shift               architecture philosophy        what the model sees

 [2] Agentic Engineering    ──→  [4] Agentic Design Patterns──→ [6] Anthropic Agent Design
     Locate yourself on           Learn the four behavioral      Learn when NOT to use agents
     the maturity spectrum        building blocks                and how to keep things simple


 DEEPEN                          SYSTEMATIZE                    SHIP
 ──────                          ───────────                    ────
 [7] Cognitive Arch.        ──→  [9] 12-Factor Agents      ──→  [11] Guardrails &
     + LLM Orchestration          Production principles           Progressive Autonomy
     Memory, reasoning,           Engineering discipline          Safety, trust, governance
     coordination design          for reliability

 [8] Agentic Patterns       ──→  [10] Eval-Driven Dev      ──→  [12] Harness Engineering
     (Willison)                    + Spec-Driven Dev              2026 paradigm: how systems
     Practitioner wisdom           Process and measurement        operate, not just what they see
     from the trenches             methodology
```

**Three reading speeds:**

| Speed | What to Read | Time | Outcome |
|-------|-------------|------|---------|
| Sprint | Steps 1-2-3-5 (four files) | 1 hour | Grasp the paradigm and core skill |
| Standard | Steps 1 through 6 | 3 hours | Understand paradigm, architecture, and patterns |
| Deep Dive | All 12 steps | 6-8 hours | Comprehensive AI engineering mental model |

---

## 4. Concept Relationship Matrix

Each cell shows how the row concept relates to the column concept. Read row-first: "How does [row] relate to [column]?"

```
                    Soft  Agen  Comp  Cogn  LLM   ADP   Anth  Will  Eval  Spec  12FA  Guar  Ctxt
                    3.0   Eng   AIS   Arch  Orch        Agnt  ison  Driv  Driv  Agen  drls  Eng
 Software 3.0       --    evol  impl  impl  impl  impl  impl  impl  impl  impl  impl  impl  impl
 Agentic Eng.       from  --    ....  ....  ....  uses  uses  uses  need  need  need  need  need
 Compound AI Sys.   impl  ....  --    uses  uses  uses  filt  ....  need  ....  appl  need  need
 Cognitive Arch.    ....  ....  desg  --    ....  uses  ....  ....  need  ....  ....  ....  need
 LLM Orchestr.     ....  ....  runs  ....  --    runs  filt  ....  need  ....  appl  need  need
 Agentic DP (Ng)   ....  ....  blck  ....  ....  --    comp  comp  ....  ....  ....  ....  ....
 Anthropic Design  ....  ....  filt  ....  ....  comp  --    comp  ....  ....  algn  algn  algn
 Willison Patterns ....  from  ....  ....  ....  comp  comp  --    ....  ....  ....  warn  prac
 Eval-Driven Dev   verf  ....  meas  ....  ....  ....  ....  ....  --    comp  algn  gate  ....
 Spec-Driven Dev   from  meth  ....  ....  ....  ....  ....  ....  comp  --    ....  ....  ....
 12-Factor Agents  ....  disc  appl  ....  ....  ....  algn  ....  algn  ....  --    algn  F5
 Guardrails        sldr  ....  arch  ....  ....  ....  algn  warn  gate  ....  F6    --    isol
 Context Eng.      ....  need  arch  mem   ....  ....  ACI   ofld  ....  spec  F5    isol  --

 Legend: impl=implements, from=derives from, evol=evolves into, uses=uses,
         desg=design for, runs=runtime of, blck=building blocks, comp=complements,
         filt=filters/constrains, meas=measures, verf=verifier for, meth=methodology for,
         algn=aligns with, appl=applies to, need=requires, prac=practices,
         warn=warns about, gate=gates, isol=isolates, ofld=offloads, mem=memory design,
         sldr=autonomy slider, disc=discipline for, F5/F6=specific factor, spec=spec-as-context
```

---

## 5. Key Thinkers (关键人物)

| Person | Affiliation | Primary Layer | Key Contributions |
|--------|-------------|---------------|-------------------|
| Andrej Karpathy | ex-OpenAI/Tesla | L1 Paradigm | Software 3.0, Vibe Coding, Agentic Engineering, LLM Psychology, Autonomy Slider, "Context engineering is the delicate art..." |
| Andrew Ng | DeepLearning.AI | L3 Patterns | 4 Agentic Design Patterns (Reflection, Tool Use, Planning, Multi-Agent), Evals as #1 success predictor |
| Simon Willison | Datasette/Django | L3 Patterns | Agentic Engineering Patterns, Context Offloading, Prompt Injection awareness, ground-truth practitioner reporting |
| Matei Zaharia | UC Berkeley / Databricks | L2 Architecture | Compound AI Systems paradigm -- intelligence from components, not bigger models |
| Erik Schluntz & Barry Zhang | Anthropic | L3 Patterns | Building Effective Agents, Workflow vs Agent distinction, ACI, simplicity-first philosophy |
| Dex Horthy | HumanLayer | L4 Methodology | 12-Factor Agents framework, bridging demo-to-production gap |
| Boming Xia et al. | Academic | L4 Methodology | EDDOps -- unifying offline and online evaluation for probabilistic systems |
| Louis Bouchard | AI Researcher | Overarching | Harness Engineering definition, "The model is the engine; harness is the rest of the car" |
| OpenAI Codex Team | OpenAI | Overarching | Harness Engineering methodology, "Agents aren't hard; the Harness is hard" |
| CNCF Platform WG | CNCF | Overarching | Four Pillars of Platform Control applied to agent management |

---

## 6. Complete File Index (概念文件索引)

Organized by layer. Primary layer assignment shown; cross-cutting relationships noted in Section 2 above.

### L1: Paradigm & Vision

| # | File | Concept |
|---|------|---------|
| 1 | [software-3.0.md](software-3.0.md) | Software 3.0 |
| 2 | [agentic-engineering.md](agentic-engineering.md) | Vibe Coding -> Agentic Engineering |

### L2: System Architecture

| # | File | Concept |
|---|------|---------|
| 3 | [compound-ai-systems.md](compound-ai-systems.md) | Compound AI Systems |
| 4 | [cognitive-architectures.md](cognitive-architectures.md) | Cognitive Architectures |
| 5 | [llm-orchestration.md](llm-orchestration.md) | LLM Orchestration |

### L3: Design Patterns

| # | File | Concept |
|---|------|---------|
| 6 | [agentic-design-patterns.md](agentic-design-patterns.md) | Agentic Design Patterns (Ng) |
| 7 | [anthropic-agent-design.md](anthropic-agent-design.md) | Anthropic Agent Design |
| 8 | [agentic-patterns-willison.md](agentic-patterns-willison.md) | Agentic Engineering Patterns (Willison) |

### L4: Engineering Methodology

| # | File | Concept |
|---|------|---------|
| 9 | [eval-driven-development.md](eval-driven-development.md) | Evaluation-Driven Development |
| 10 | [spec-driven-development.md](spec-driven-development.md) | Spec-Driven Development |
| 11 | [12-factor-agents.md](12-factor-agents.md) | 12-Factor Agents |

### L5: Safety & Governance

| # | File | Concept |
|---|------|---------|
| 12 | [guardrails-progressive-autonomy.md](guardrails-progressive-autonomy.md) | Guardrails & Progressive Autonomy |

### Overarching Discipline (2026 Paradigm)

| # | File | Concept |
|---|------|---------|
| 14 | [harness-engineering.md](harness-engineering.md) | Harness Engineering (驾驭工程) |

### Cross-Cutting Discipline

| # | File | Concept |
|---|------|---------|
| 13 | [context-engineering.md](context-engineering.md) | Context Engineering |

### Survey Report (综述报告)

| # | File | Description |
|---|------|-------------|
| — | [ai-engineering-survey-2026.md](../tooling/ai-engineering-survey-2026.md) | 2026年初AI工程前沿综述：从提示工程到上下文工程的范式演进（中文，~15,000字） |
