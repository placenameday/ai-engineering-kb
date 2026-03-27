---
name: compound-ai-systems
description: >
  Intelligence emerges from multiple interacting components — models, retrievers, tools, memory layers,
  and orchestrators — rather than a single model. Coined by Berkeley AI Research (BAIR) lab, Feb 2024.
  At Databricks, 60% of LLM apps use RAG, 30% use multi-step chains.
survey_date: 2026-03-05
lang: en
---

# Compound AI Systems (复合AI系统)

> Survey date: 2026-03-05.

---

**Core idea:** Intelligence emerges not from a single model but from multiple interacting components — models, retrievers, tools, memory layers, and orchestrators. Mirrors software engineering principles: loose coupling, high cohesion, separation of concerns.

**Key proponents:** Berkeley AI Research (BAIR) lab (Matei Zaharia et al., Feb 2024); Databricks.

**Design patterns within:**
- Modular Component Architecture (generator, retriever, ranker, classifier modules)
- Retrieval-Augmented Generation (RAG) and Agentic RAG
- Multi-Agent Orchestration
- Internally Compound Architectures (e.g., GPT-5 as a smart router between internal model variants)

**Key stat:** At Databricks, 60% of LLM apps use RAG, 30% use multi-step chains.

**Academic taxonomy (2025 arXiv survey):** Four foundational axes — retrieval, agency, multimodal perception, orchestration.

**Why it matters:** More parameters no longer guarantee better results. The new wave of innovation is about better systems, not bigger models.

**Sources:**
- [BAIR: The Shift from Models to Compound AI Systems](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/)
- [arXiv Survey: From Standalone LLMs to Integrated Intelligence](https://arxiv.org/html/2506.04565v1)
