# AI Engineering Web Research (2026-03)

> **Compiled by**: Sage (research agent)
> **Date**: 2026-03-05
> **Methodology**: Synthesized from training knowledge (through mid-2025), existing KB analysis, and limited web fetches. Items marked [VERIFY] need fresh source confirmation. Items marked [KB] are corroborated by existing KB articles.

---

## 1. Industry State & Trends (AI Engineering 2025-2026)

### Key Developments
- **AI Engineering established as a distinct discipline** (2024-2026). The term shifted from "prompt engineering" → "AI engineering" → "context engineering" as the field matured. [KB]
- **Andrej Karpathy's "Software 3.0" keynote** (Y Combinator AI Startup School, June 2025) defined the paradigm: natural language as programming language. YC reported 25% of Winter 2025 startups had codebases 95% AI-generated. [KB, VERIFY stat]
- **"Vibe coding" coined by Karpathy** (Feb 2025), later evolved to "agentic engineering" (early 2026) as the professional discipline. Named Collins Dictionary Word of the Year 2025. [KB]
- **AI agent infrastructure became a major investment category** in 2025. a16z, Sequoia, and other top VCs published AI agent infrastructure maps.
- **Gartner prediction**: 40% of enterprise apps will feature task-specific AI agents by late 2026. [KB]
- **Enterprise AI adoption accelerating**: Focus shifted from experimentation to production deployment, with emphasis on reliability, evaluation, and safety.
- **CodeRabbit analysis**: AI co-authored code had 1.7x more "major" issues across 470 PRs analyzed — highlighting the gap between demos and production. [KB]

### Key People
- **Andrej Karpathy**: Software 3.0, vibe coding, agentic engineering, context engineering framing
- **Dex Horthy** (HumanLayer): 12-Factor Agents, eval-driven development advocacy
- **Matei Zaharia** (Databricks/Berkeley): Compound AI systems concept
- **Andrew Ng** (AI Fund): Four agentic design patterns (reflection, tool use, planning, multi-agent)
- **Erik Schluntz & Barry Zhang** (Anthropic): Building effective agents guide
- **Simon Willison**: Practical agent patterns, prompt injection taxonomy, open source AI advocacy

### Key Stats [VERIFY all]
- GitHub Copilot: 1.8M+ paid subscribers (as of mid-2025)
- OpenAI: $13B+ revenue run rate (2025)
- AI coding tool market: >$5B projected for 2025
- Stack Overflow Developer Survey 2024: 76% of developers using or planning to use AI tools

---

## 2. Context Engineering

### Core Concept
- **Definition**: "The systematic discipline of designing and optimizing ALL contextual information flowing through AI systems — not just prompts, but retrieved documents, memory, tool descriptions, state, and control flow." [KB]
- **Karpathy quote**: "Context engineering is the delicate art and science of filling the context window with just the right information for the next step." [KB]

### Anthropic's Four Patterns (Write/Select/Compress/Isolate)
| Pattern | Description | Claude Code Implementation |
|---------|-------------|---------------------------|
| Write | Persist context externally | `.claude/memory/` files |
| Select | Pull relevant context in | Skills (description match), @imports |
| Compress | Reduce token usage | `/compact`, agent summaries |
| Isolate | Partition across systems | Sub-agents with own context |
[KB]

### Key Research Findings
- **Context Rot** (Chroma Research, Jul 2025): 18 SOTA models tested; performance degrades non-linearly with context length. [KB]
- **arXiv 2510.05381**: Even with perfect retrieval, performance drops 13.9%-85% as length increases. [KB]
- **Critical threshold**: ~40% of context window capacity. For 200K windows: stay below ~80K tokens. [KB]
- **Core insight**: A focused 300-token context often outperforms an unfocused 113,000-token context. [KB]

### Advanced Patterns
- **Microsoft CORPGEN** (Feb 2026): Three-tier memory (Working → Structured LTM → Semantic). Up to 3.5x improvement. [KB]
- **Synaptic Labs Bounded Context Packs**: 7±2 tools per domain. Meta-tool pattern: 85-95% token reduction. [KB]
- **MemGPT/Letta**: Context window as RAM, external storage as disk. Agent manages page in/out. [KB]

---

## 3. MCP (Model Context Protocol) Ecosystem

### What is MCP
- **Definition**: "An open-source standard for connecting AI applications to external systems" — described as "USB-C for AI integrations." [KB + web fetch confirmed]
- **Three primitives**: Tools, Resources, Prompts. [KB]
- **Architecture**: Client-server model. MCP Hosts (e.g., Claude Desktop, IDEs) connect to MCP Servers that expose tools/resources. [web fetch confirmed]

### Governance & Adoption
- **Linux Foundation governance**: MCP was contributed to the Linux Foundation for open governance. [KB claims; VERIFY exact date]
- **Adopted by major players**: Anthropic (creator), OpenAI, Google, Microsoft all support MCP. [KB]
- **SDK downloads**: 97M+ monthly SDK downloads claimed. [KB, VERIFY]

### Ecosystem
- **MCP Specification**: Latest version dated 2025-11-25 per KB sources.
- **Use cases**: Agents accessing Google Calendar, Notion; Claude Code generating web apps from Figma designs; Enterprise chatbots connecting to multiple databases; AI models controlling physical devices
- **SDK availability**: TypeScript, Python SDKs are primary. Community SDKs in Go, Rust, Java, C#.
- **MCP server ecosystem**: Thousands of community-built servers. Official servers include: filesystem, GitHub, Google Drive, Postgres, Slack, and more.
- **Competing/complementary protocol**: Google's A2A (Agent-to-Agent) protocol — focuses on inter-agent communication vs MCP's tool integration focus.

### Key Dates
- Nov 2024: MCP open-sourced by Anthropic
- Mar 2025: OpenAI announced MCP support
- Apr 2025: Google announced A2A protocol
- 2025-11-25: MCP spec version date [KB]
- Late 2025/Early 2026: Linux Foundation governance

---

## 4. Agentic AI Frameworks

### Framework Landscape (2025-2026)

| Framework | Maintainer | Focus | Key Feature |
|-----------|-----------|-------|-------------|
| **LangGraph** | LangChain | Graph-based agent orchestration | Stateful, cyclic graphs; checkpointing |
| **CrewAI** | CrewAI Inc | Multi-agent collaboration | Role-based agents, process flows |
| **AutoGen** | Microsoft | Multi-agent conversation | Conversational agent patterns |
| **OpenAI Agents SDK** | OpenAI | Lightweight agent primitives | Handoffs, guardrails, tracing |
| **Google ADK** | Google | Agent Development Kit | Multi-agent, A2A protocol support |
| **Semantic Kernel** | Microsoft | Enterprise orchestration | .NET/Python/Java, planner architecture |
| **DSPy** | Stanford NLP | Programmatic LLM pipelines | Compiler-based prompt optimization |
| **Vercel AI SDK** | Vercel | Web-first AI apps | Streaming, React hooks, multi-provider |
| **Pydantic AI** | Pydantic | Type-safe agent framework | Pydantic-native, structured outputs |

### OpenAI Agents SDK (Released Mar 2025)
- Successor to OpenAI Swarm (experimental, Oct 2024). Production-grade version.
- Core concepts: Agents (with instructions + tools), Handoffs (agent-to-agent delegation), Guardrails (input/output validation), Tracing (built-in observability).
- Philosophy: Minimal abstraction, "just enough framework."

### Google ADK + A2A Protocol
- ADK announced at Google Cloud Next 2025 (Apr 2025).
- A2A: Open protocol for inter-agent communication. Complementary to MCP (MCP = agent-to-tool, A2A = agent-to-agent).
- A2A supporters: Google, Salesforce, SAP, Atlassian, ServiceNow, MongoDB.

### Framework Comparison — Thin vs Thick
- **Thin** (OpenAI Agents SDK, Pydantic AI): Minimal abstraction, developer owns control flow. 12-Factor Agents philosophy.
- **Thick** (LangGraph, CrewAI, AutoGen): More built-in orchestration, state management, memory.
- Anthropic's "Building Effective Agents" advocates simplicity: "start with workflows, not agents."

---

## 5. AI Coding Agents Landscape

### Major Players (2025-2026)

| Agent | Company | Type |
|-------|---------|------|
| **Claude Code** | Anthropic | Terminal-based agentic coder |
| **Cursor** | Anysphere | IDE (VS Code fork) |
| **Windsurf** | Codeium→OpenAI | IDE (VS Code fork) |
| **GitHub Copilot** | GitHub/Microsoft | IDE extension + Agent mode |
| **Devin** | Cognition AI | Fully autonomous SWE agent |
| **Codex** | OpenAI | Cloud-based agent |
| **Cline** | Community | VS Code extension (open source) |
| **Aider** | Paul Gauthier | Terminal-based (open source) |
| **Amazon Q Developer** | AWS | IDE + CLI agent |

### Key Developments
- **Claude Code** (2025): Terminal-native, skills/plugins ecosystem, memory hierarchy, MCP integration. [KB]
- **Cursor**: Breakout IDE of 2024-2025, reported $100M+ ARR. [VERIFY]
- **GitHub Copilot Agent Mode** (2025): Evolution from autocomplete to multi-file agentic coding.
- **Windsurf**: Acquired by OpenAI in 2025. [VERIFY]
- **SWE-bench**: Primary benchmark for coding agents. Top agents solving ~50%+ of issues.

---

## 6. Frontier Models (2025-2026)

| Model Family | Company | Key Releases | Notes |
|-------------|---------|-------------|-------|
| **Claude** | Anthropic | 3.5 Sonnet, 4 Sonnet, Opus 4, Opus 4.6 | Extended thinking, computer use, coding excellence |
| **GPT** | OpenAI | GPT-4o, o1/o3, GPT-4.5, GPT-5 | Reasoning models (o-series) separate track |
| **Gemini** | Google | 2.0 Flash, 2.5 Pro | Native multimodal, 1M+ tokens context |
| **Llama** | Meta | 3.1 (405B), 4 Scout/Maverick | Open weights, MoE in Llama 4 |
| **DeepSeek** | DeepSeek | V3, R1 | MoE, open weights, cost-efficient |
| **Qwen** | Alibaba | 2.5 series, Qwen 3 | Strong multilingual, open weights |

### Trends
- Reasoning models as distinct category (o1/o3, DeepSeek-R1, Gemini thinking, Claude extended thinking)
- MoE (Mixture of Experts) becoming dominant architecture
- Cost deflation: Per-token prices dropping 90%+ YoY
- Open weights narrowing gap with proprietary models

---

## 7. AI Safety, Guardrails & Regulation

### EU AI Act
- Entered into force Aug 1, 2024. Phased implementation:
  - Feb 2025: Bans on prohibited AI practices
  - Aug 2025: GPAI model obligations
  - Aug 2026: Full enforcement including high-risk systems

### Chinese Regulations
- Generative AI regulations in effect (Aug 2023). Algorithm registry requirement.
- Deepfake regulations. Export controls on AI models.

### Enterprise Safety Tools
- NVIDIA NeMo Guardrails, Guardrails AI, Patronus AI, LangChain HITL middleware
- Progressive autonomy pattern: advisory → supervised → constrained autonomy [KB]
- Prompt injection remains #1 security concern. OWASP Top 10 for LLM Apps.

---

## 8. Eval-Driven Development & AI Testing

### Eval Frameworks
| Tool | Type | Key Features |
|------|------|-------------|
| **Braintrust** | Platform | Evals, logging, CI/CD |
| **Promptfoo** | Open source | CLI-based, red teaming |
| **LangSmith** | Platform | Tracing, evals, datasets |
| **Arize Phoenix** | Open source | Tracing, embeddings |
| **Ragas** | Open source | RAG-specific metrics |
| **DeepEval** | Open source | Unit testing for LLMs |

### LLM-as-Judge Pattern
- Using LLMs to evaluate LLM outputs. Increasingly standard.
- Challenges: Judge model bias, positional bias, self-preference, cost.
- Best practices: Use rubrics, multiple judges, human calibration.

---

## 9. Chinese AI Ecosystem

### Major Players
| Company | Model | Notes |
|---------|-------|-------|
| **DeepSeek** | V3, R1 | R1 shook global AI industry Jan 2025 |
| **Alibaba** | Qwen 2.5 | Strong multilingual, open weights |
| **ByteDance** | Doubao | Integrated into TikTok/Douyin |
| **Baidu** | ERNIE 4.0 | Wenxin platform |
| **Moonshot AI** | Kimi | Long context pioneer |
| **Zhipu AI** | GLM-4 | ChatGLM from Tsinghua |
| **01.AI** | Yi series | Founded by Kai-Fu Lee |

### Key Developments
- **DeepSeek-R1** (Jan 2025): Open-source reasoning model, ~$5.5M training cost. Triggered global discussion.
- **Price war**: Chinese providers aggressively cutting API prices.
- **Open source leadership**: DeepSeek and Qwen among most downloaded on HuggingFace globally.
- **Agent platforms**: Alibaba Tongyi, Baidu Wenxin Agent, ByteDance Coze.

---

## 10. 12-Factor Agents & SDD Adoption

### 12-Factor Agents [KB]
- Author: Dex Horthy (HumanLayer), AI Engineer World's Fair 2025
- Strong alignment with OpenAI Agents SDK philosophy and Anthropic's simplicity principle
- The 12 factors cover: tool calls, prompt ownership, control flow, context management, human contact, error handling, agent sizing, triggering, state management, context prefetching

### Spec-Driven Development
- Proponents: Thoughtworks, GitHub (Spec Kit), Karpathy
- CLAUDE.md/.cursorrules as de facto spec files for coding agents
- Team structure shift: 20/60/20 → 60/30/10 (product/eng/design)

---

*Generated by Sage | 2026-03-05*
