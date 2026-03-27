# KB Synthesis: Practice (Quality, Safety, Checklists, Tooling)

> Part 2 of kb-synthesis split. See also: `kb-synthesis-core.md`
> Date: 2026-03-27

---

## 5. Evaluation and Quality

### Core Principle
- **Evals are the TDD of AI systems.** Changes accepted only when metrics improve.
- "The single biggest predictor of success is the ability to drive a disciplined process for evals." — Ng

### Eval Tools (2026)

| Tool | Type | Best For |
|------|------|----------|
| Braintrust | Commercial | Enterprise full-pipeline |
| Promptfoo | Open source | Fast iteration, red-team |
| LangSmith | Commercial | LangChain ecosystem |
| Arize Phoenix | Open source | Observability-first |
| DeepEval | Open source | LLM unit testing |

### LLM-as-Judge
- Using LLM to evaluate LLM output is standard.
- **Challenges**: Judge bias, position bias, self-preference.
- **Best practices**: Use rubrics, multiple judges, human calibration.

### DO/DON'T — Quality

| DO | DON'T |
|----|-------|
| Build evals before features | Ship without measurable criteria |
| Run evals in CI/CD | Rely on manual eyeballing |
| Evaluate at system level | Only benchmark model performance |

---

## 6. Safety and Guardrails

> **Note:** Guardrails are one of seven components in [Harness Engineering](../../concepts/harness-engineering.md). The control plane below is the harness's deterministic logic gate between agent and outside world.

### Control Plane Architecture (Harness Pattern)

```
Agent (LLM Brain) — NO direct access to outside world
        │
        ▼
┌─────────────────────────────────────────────────────────────┐
│                   CONTROL PLANE (Deterministic)              │
├─────────────────────────────────────────────────────────────┤
│ Guardrail 1: Budget check     → <$500 autonomous?          │
│ Guardrail 2: Scope check      → Allowed domain?            │
│ Guardrail 3: Confidence check → >threshold?                │
│ Guardrail 4: Rate limit       → Within quota?              │
│ Guardrail 5: Audit log        → Record all actions         │
└─────────────────────────────────────────────────────────────┘
        │
        ▼
              [ALLOW / DENY / ESCALATE]
```

### Four-Layer Architecture

```
Request → API Gateway → Auth → Input Sanitizer → Agent Engine →
  Retrieval → Tool Runner (Sandbox) → Output Verifier → Audit Log → Response
```

| Layer | Tools |
|-------|-------|
| Pre-tool Policy Gate | NeMo Guardrails, Guardrails AI |
| Drift Detection | Patronus AI, LangSmith |
| HITL Escalation | 12-Factor Agent Factor 6/7 |

### Progressive Autonomy

| Phase | Mode | AI Does | Human Does |
|-------|------|---------|------------|
| 1 | Advisory | Suggests | All decisions |
| 2 | Supervised | Executes w/ confirmation | Approve/reject |
| 3 | Constrained | Autonomous in boundaries | Exceptions |

### Prompt Injection Defense

1. Input sanitization
2. Permission minimization
3. Context isolation
4. Output verification
5. Audit trail

### Critical Stats
- **87%** of enterprises lack AI security frameworks.
- **97%** of 2025 AI breaches in environments without access controls.

---

## 7. Master Checklists

### 12-Factor Agents (Production Readiness)

1. Natural language → structured tool calls
2. Own your prompts
3. Tools = structured outputs
4. Own the control flow
5. Own your context window
6. Launch/Pause/Resume (HITL points)
7. Contact humans via tool calls
8. Compact errors into useful context
9. Small, focused agents
10. Trigger from anywhere
11. Unify execution and business state
12. Pre-fetch context

### Five-Layer Mental Model

```
L1 PARADIGM: Why is everything changing?
L2 ARCHITECTURE: How to structure AI systems?
L3 PATTERNS: What solutions to recurring problems?
L4 METHODOLOGY: What process guides development?
L5 SAFETY: How to deploy responsibly?
CROSS-CUTTING: Context Engineering
```

### Daily Claude Code Practice

| DO | DON'T |
|----|-------|
| CLAUDE.md as concise index (≤30 lines) | Bloat CLAUDE.md |
| `/compact Focus on X` at 70% context | Wait for 95% auto-compact |
| `/clear` between unrelated tasks | Let context pollute |
| 30-minute focused sprints | Unbounded sessions |
| Audit memory every 2 weeks | Let stale memory accumulate |

---

## 8. Key Numbers

| Metric | Value |
|--------|-------|
| Context rot threshold | ~40% of window |
| 200K window safe limit | ~80K tokens |
| MCP tool overhead (35 tools) | ~26K tokens |
| Multi-agent accuracy improvement | +60% |
| Meta-tool token reduction | 85-95% |

### Framework Selection

| Need | Choose |
|------|--------|
| Simple workflows | Direct LLM API (no framework) |
| Type-safe output | Pydantic AI |
| Minimal orchestration | OpenAI Agents SDK |
| Complex stateful multi-agent | LangGraph |
| Role-based teams | CrewAI |

---

## Cross-Cutting Themes

1. **Simplicity wins**: Start simple, add complexity when proven necessary.
2. **Context is king**: Every token matters.
3. **Evals are non-negotiable**: Without systematic evals, you're guessing.
4. **Own your stack**: Own prompts, control flow, context window.
5. **Safety is architecture**: Layer defenses, use progressive autonomy.
6. **Focused 300 tokens > unfocused 113K tokens**: Quality beats quantity.
