# KB Audit Report (2026-03)

**Auditor**: Sage
**Date**: 2026-03-05
**Scope**: All 15 files (13 concepts, 2 tooling), README.md, _index.md

---

## Executive Summary

- **Overall quality is high.** Well-organized, coherent body of knowledge with a thoughtful 5-layer framework.
- **The 5-layer framework is valid and well-designed.** Layer assignments are appropriate. Cross-cutting Context Engineering is the right architectural decision.
- **Factual accuracy is generally strong** but contains several claims requiring verification — particularly star counts, statistics, and precise attributions.
- **Context Engineering is the strongest article** — deep, well-sourced, with practical patterns. Flagship piece.
- **Most concept articles are too thin** (20-35 lines). Read as extended glossary entries rather than deep dives.
- **Andrew Ng's 4 patterns are misrepresented** in `research-tools-survey.md` — lists "Memory & Retrieval (RAG)" as 4th pattern when Ng's actual 4th is "Multi-Agent Collaboration."
- **Major 2025-2026 topics missing entirely**: structured outputs/tool use standardization, prompt caching & cost optimization, voice/multimodal agents, AI coding agent landscape, enterprise platform patterns, model capability leaps, observability/tracing.
- **MCP coverage is surface-level** — mentioned in context-engineering.md but deserves its own dedicated article.
- **Tooling directory is narrowly scoped** — focused on research/academic tools and Claude Code skills. Missing: orchestration framework comparisons, observability tools, deployment platforms.
- **L5 (Safety & Governance) is underweight** — single article covering a massive domain.

---

## Per-Article Assessment

### L1: Paradigm & Vision

#### 1. software-3.0.md
- **Accuracy**: 4/5 | **Currency**: 3/5
- Karpathy keynote attribution correct; YC 25% stat needs verification
- Missing: practical implications, economic analysis, counter-arguments, community adoption/contestation
- Article is thin (35 lines) for a foundational paradigm concept

#### 2. agentic-engineering.md
- **Accuracy**: 4/5 | **Currency**: 3/5
- Karpathy "vibe coding" Feb 2025 correct; Collins Dictionary Word of Year claim needs verification
- Missing: specific tooling (Claude Code, Cursor, Windsurf, Devin), context engineering bridge, backlash from senior engineers

### L2: System Architecture

#### 3. compound-ai-systems.md
- **Accuracy**: 5/5 | **Currency**: 3/5
- BAIR blog attribution correct; "GPT-5 as smart router" claim is speculative, should be flagged
- Missing: production examples, MCP relationship, compound vs monolithic benchmarks
- Thin (36 lines) for the most important architectural concept

#### 4. cognitive-architectures.md
- **Accuracy**: 4/5 | **Currency**: 4/5
- CoALA paper reference correct; EGI and CCA frameworks harder to verify
- Missing: MemGPT/Letta evolution, practical memory implementations (Mem0, Zep, LangGraph memory)

#### 5. llm-orchestration.md
- **Accuracy**: 3/5 | **Currency**: 3/5
- CrewAI 2.0 version label needs verification; Microsoft Agent Framework naming may not be official
- **Thinnest article (28 lines)**. Missing: LangChain vs LlamaIndex, OpenAI Agents SDK, DSPy, Vercel AI SDK, thin vs thick framework debate

### L3: Design Patterns

#### 6. agentic-design-patterns.md
- **Accuracy**: 5/5 | **Currency**: 3/5
- Ng's four patterns correctly listed; OpenAI Swarm was experimental, not production
- Missing: implementation examples, when-to-use guidance, community refinements

#### 7. anthropic-agent-design.md
- **Accuracy**: 5/5 | **Currency**: 4/5
- Attribution to Schluntz & Zhang correct; three principles accurately captured
- Missing: specific workflow patterns (chaining, routing, parallelization, orchestrator-workers)

#### 8. agentic-patterns-willison.md
- **Accuracy**: 4/5 | **Currency**: 3/5
- Willison correctly identified; "YOLO mode" terminology source unclear
- Thin (32 lines). Missing: tool/function calling patterns, open source perspectives, prompt injection taxonomy

### L4: Engineering Methodology

#### 9. eval-driven-development.md
- **Accuracy**: 4/5 | **Currency**: 4/5
- EDDOps paper reference plausible; Amazon experience reference valuable
- Missing: specific eval frameworks (Braintrust, Promptfoo, LangSmith), LLM-as-judge patterns

#### 10. spec-driven-development.md
- **Accuracy**: 4/5 | **Currency**: 4/5
- Thoughtworks as proponent plausible; GitHub Spec Kit needs verification
- Missing: relationship to CLAUDE.md/cursor rules as "specs", counter-arguments

#### 11. 12-factor-agents.md
- **Accuracy**: 5/5 | **Currency**: 4/5
- All 12 factors accurately listed; Dex Horthy / HumanLayer attribution correct
- Missing: adoption in practice, comparison with Anthropic's principles

### L5: Safety & Governance

#### 12. guardrails-progressive-autonomy.md
- **Accuracy**: 4/5 | **Currency**: 3/5
- "87% lack frameworks" and "97% of breaches" stats lack source attribution
- **Weakest layer** — Missing: AI regulatory landscape, red teaming, prompt injection deep dive, data privacy

### Cross-Cutting

#### 13. context-engineering.md
- **Accuracy**: 5/5 | **Currency**: 5/5
- **Strongest article in KB**. Well-researched, multi-sourced, practical patterns
- MCP Linux Foundation governance claim needs verification; 97M+ SDK downloads needs verification
- Minor gap: prompt caching, token counting tools

### Tooling

#### 14. research-tools-survey.md (ZH)
- **Accuracy**: 3/5 | **Currency**: 3/5
- **FACTUAL ERROR**: Ng's 4th pattern listed as "Memory & Retrieval (RAG)" — should be "Multi-Agent Collaboration"
- Missing: NotebookLM, Perplexity, Google AI Studio

#### 15. skills-ecosystem.md
- **Accuracy**: 3/5 | **Currency**: 3/5
- Multiple star counts seem inflated (29k, 33k, 50k) — need verification
- BixBench GPT-5 comparison needs verification; claims 16 official skills but only lists 8

---

## Framework Assessment

**The 5-layer framework is fundamentally sound.** Suggested modifications:

1. **L2 rebalancing**: LLM Orchestration reads more like tool comparison than architecture
2. **L5 expansion needed**: Split into Technical Safety, Governance & Compliance, Security
3. **Consider L0 (Foundations)**: Prerequisites for newcomers
4. **Consider L6 (Operations)**: Production operations, observability, cost management
5. **Learning path needs "fast track" variants** for different audiences

---

## Missing Topics

### Critical Gaps

| Topic | Layer | Why Critical |
|-------|-------|-------------|
| **MCP Protocol** | L2/Cross-cutting | Standardization of AI tool integration; only mentioned in context-engineering.md |
| **AI Coding Agents** | L1/L3 | Cursor, Claude Code, Windsurf, Devin — most visible agentic AI application |
| **Structured Outputs & Tool Use** | L2/L3 | Function calling standardization across all providers |
| **Observability & Tracing** | L4/L5 | LangSmith, Braintrust, Arize — production AI needs this |
| **Model Capability Landscape** | L1 | No article tracking actual model capabilities and engineering implications |

### Important Gaps

| Topic | Layer |
|-------|-------|
| Prompt Caching & Cost Optimization | L4/Cross-cutting |
| Multi-Agent Orchestration Frameworks (deep) | L2 |
| Voice & Multimodal Agents | L1/L2 |
| Enterprise AI Platform Patterns | L2/L4 |
| RAG Architecture Deep Dive | L2/L3 |
| Fine-Tuning vs Prompting Decision Framework | L4 |
| AI Regulation & Compliance | L5 |
| Prompt Injection & AI Security | L5 |
| Open Source Models & Self-Hosting | L1/L2 |

---

## Recommendations

### Priority 1: Fix Errors (immediate)
1. Fix Ng's patterns in research-tools-survey.md
2. Flag GPT-5 architecture claim in compound-ai-systems.md as speculative
3. Verify all star counts in skills-ecosystem.md
4. Source security statistics in guardrails-progressive-autonomy.md

### Priority 2: Expand Thin Articles
1. llm-orchestration.md (28→80+ lines)
2. compound-ai-systems.md (add production examples)
3. agentic-patterns-willison.md (more patterns)
4. software-3.0.md (practical implications)

### Priority 3: Add Missing Articles
1. mcp-protocol.md
2. ai-coding-agents.md
3. structured-outputs-tool-use.md
4. observability-tracing.md
5. rag-architecture.md

### Priority 4: Structural Improvements
1. Add "Last Verified" date to each article
2. Add "Confidence" field for claims
3. Expand L5 to multiple articles
4. Add inter-article cross-references
5. Standardize minimum article depth (60+ lines)

---

*Generated by Sage | 2026-03-05*
