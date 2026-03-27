---
name: eval-driven-development
description: >
  Make repeatable LLM evaluations ("evals") the core step of the development lifecycle — the TDD
  equivalent for AI systems. Changes accepted only when they improve metrics. EDDOps unifies offline
  and online evaluation in a closed feedback loop for probabilistic, non-deterministic systems.
survey_date: 2026-03-05
lang: en
---

# Evaluation-Driven Development / EDDOps (评估驱动开发)

> Survey date: 2026-03-05.

---

**Core idea:** Make repeatable LLM evaluations ("evals") the core step of the development lifecycle. The TDD equivalent for AI systems. Changes accepted only when they improve metrics. Evals are first-class development artifacts, versioned and run in CI/CD.

**Key proponents:** Dex Horthy & Andrew Ng (both emphasize evals as #1 predictor of success); Boming Xia et al. (EDDOps academic paper on arXiv).

**EDDOps framework:** Unifies offline (development-time) and online (runtime) evaluation in a closed feedback loop. Addresses the problem that traditional TDD/BDD methods assume deterministic systems, but LLM agents are probabilistic and open-ended.

**Key insight:** Classical "eyeballing" outputs is ending. For compound AI systems, evaluation must extend beyond model-level benchmarks to system-level assessment — architecture, pipeline, memory, guardrails.

**Why it matters:** Without systematic evals, you cannot reliably improve AI systems. Amazon's production experience confirms: single-model benchmarks are insufficient for agentic systems.

**Sources:**
- [arXiv: EDDOps Process Model](https://arxiv.org/html/2411.13768v3)
- [Amazon: Evaluating AI Agents](https://aws.amazon.com/blogs/machine-learning/evaluating-ai-agents-real-world-lessons-from-building-agentic-systems-at-amazon/)
- [AI Technology Radar: EDD Pattern](https://ai-radar.aoe.com/architecture-pattern/evaluation_driven_development/)
