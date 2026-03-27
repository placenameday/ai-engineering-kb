---
name: harness-engineering
description: >
  The discipline of building the operational environment around AI agents — permissions,
  hooks, tools, guardrails, state management, and control planes. Succeeds context engineering
  as the 2026 paradigm: "how the system behaves" vs "what the model sees."
survey_date: 2026-03-27
lang: en
---

# Harness Engineering (驾驭工程)

> Survey date: 2026-03-27. Deep-dive research on the 2026 paradigm shift.
>
> Sources: Anthropic engineering blog, OpenAI Codex research, CIO.com, community analysis.

---

## Overview

**Core idea:** Harness Engineering is the practice of building the complete operational environment around an AI agent. It defines constraints, toolchains, feedback loops, and lifecycle management — ensuring agents operate continuously, reliably, and safely.

**Key distinction:**
| Paradigm | Years | Question | Analogy |
|----------|-------|----------|---------|
| Prompt Engineering | 2022-2024 | *What to ask?* | Writing the email |
| Context Engineering | 2025 | *What to send?* | Attachments + context |
| Harness Engineering | 2026 | *How does it operate?* | The entire office |

**Why it matters:**
- "Agents aren't hard; the Harness is hard" (OpenAI/Anthropic thesis)
- Gartner: 40% of enterprise apps will have AI agents by late 2026
- Microsoft: 80% of Fortune 500 already have active AI agents
- Without harnesses: inconsistent safety, duplicated costs, unmonitored access, compliance gaps

**Key proponents:** Anthropic engineering team, OpenAI (Codex agents), Gartner, CNCF platform engineering working group.

---

## The Paradigm Evolution

```
2022-2024: PROMPT ENGINEERING
├── Focus: Wording, phrasing, instruction design
├── Limitation: Assumes single-shot interaction
└── Analogy: "What to ask"

2025: CONTEXT ENGINEERING
├── Focus: Everything the model sees — prompts, docs, memory, tools
├── Key insight: "A focused 300-token context beats 113K unfocused tokens"
├── Patterns: Write/Select/Compress/Isolate (Anthropic)
└── Analogy: "What to send so the model can answer confidently"

2026: HARNESS ENGINEERING
├── Focus: The entire operational environment
├── Key insight: "The model is the engine; the harness is the rest of the car"
├── Components: Permissions, hooks, guardrails, state, feedback loops
└── Analogy: "How the whole thing operates"
```

**The car analogy (Louis Bouchard):**
- Model = Engine (you can't do much without it)
- Context = Fuel, oil, dashboard (things you optimize)
- Harness = Steering, brakes, lane boundaries, warning lights, doors that don't fall off

---

## Core Components

### 1. Permission System (权限系统)

Controls what actions the agent can take without human approval.

| Layer | Purpose | Implementation |
|-------|---------|----------------|
| **Permission Modes** | Execution autonomy level | `default`, `plan`, `auto`, `bypassPermissions`, `acceptEdits` |
| **Rule Evaluation** | Deterministic access control | `deny > ask > allow` precedence |
| **Tool-level Permissions** | Per-tool access control | `Bash`, `Read`, `Write`, `Edit`, `WebFetch` |
| **Path-based Rules** | Scope restrictions | `~/.claude/**`, `.env` protection |

**Claude Code Permission Modes:**
```
default        → Prompt on first use of each tool
plan           → Read-only tools only (Plan Mode)
acceptEdits    → Auto-accept file edits, ask for commands
bypassPermissions → Skip prompts (except .git, .claude, .vscode)
auto           → Full autonomy with safety boundaries
```

### 2. Hooks Mechanism (钩子机制)

Lifecycle scripts that fire at specific moments, enabling deterministic intervention.

| Hook Event | Can Block? | Purpose |
|------------|------------|---------|
| `PreToolUse` | Yes | Intercept tool calls before execution |
| `PostToolUse` | No | React after tool completes |
| `UserPromptSubmit` | Yes | Validate/transform user input |
| `PermissionRequest` | Yes | Override permission decisions |
| `Stop` | Yes | Prevent premature termination |
| `SessionStart` | No | Load context before agent starts |
| `SessionEnd` | No | Cleanup, persist state |

**Hook Decision Pattern:**
```json
// PreToolUse blocking example
{
  "hookSpecificOutput": {
    "permissionDecision": "deny",
    "permissionDecisionReason": "Destructive command blocked"
  }
}
```

**Use cases:**
- Block `rm -rf` commands
- Auto-run linter after edits (PostToolUse)
- Inject context on session start
- Quality gates before task completion (Stop)

### 3. Tool Access Control (工具访问控制)

Mediates agent-to-external-world interaction via MCP (Model Context Protocol).

| Concept | Description |
|---------|-------------|
| **MCP Protocol** | "USB-C for AI integrations" — standardized tool interface |
| **Three Primitives** | Tools, Resources, Prompts |
| **Tool Budget** | 7±2 tools per domain (bounded context) |
| **Meta-Tool Pattern** | 2 tools (discover + execute) replace N tools |

**Tool access control pattern:**
```
Agent → Request → Control Plane → Decision → Execution
                ↓
         Guardrails check:
         - Is tool allowed?
         - Is parameter valid?
         - Is budget exhausted?
         - Does it require human approval?
```

### 4. Guardrails & Control Plane (护栏与控制平面)

Deterministic logic gates between the agent and the outside world.

**CIO.com Control Plane Architecture:**
```
┌─────────────────────────────────────────────────────────────┐
│                     AGENT (LLM Brain)                       │
│            Free to reason, draft, formulate                 │
│            NO direct access to outside world                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   CONTROL PLANE                              │
│              (Hard-coded, deterministic)                     │
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

**Circuit Breaker Pattern:**
- Monitor agent confidence in real-time
- If confidence drops below threshold → circuit breaks
- Escalate to human or safe fallback

### 5. State Management (状态管理)

Ensures agent persistence across sessions and iterations.

| Component | Purpose |
|-----------|---------|
| **Checkpoints** | Save/restore agent state |
| **Memory Hierarchy** | Working memory → Structured LTM → Semantic memory |
| **Session Memory** | Per-session context preservation |
| **Cross-session Memory** | `.claude/memory/` persistence |

**Claude Code Memory Hierarchy:**
1. Enterprise Policy (highest priority)
2. Project CLAUDE.md
3. Project Rules (`.claude/rules/*.md`)
4. User CLAUDE.md

### 6. Isolation & Sandboxing (隔离与沙箱)

Prevents agent actions from affecting unintended systems.

| Mechanism | Description |
|-----------|-------------|
| **Worktree Isolation** | Git worktree for parallel development |
| **Sandbox Mode** | Restricted execution environment |
| **Path Restrictions** | Block access to sensitive paths |
| **Environment Variables** | Isolated secrets management |

### 7. Feedback Loops (反馈循环)

Ensures quality through automated validation.

| Loop Type | Implementation |
|-----------|----------------|
| **Linter** | Auto-run after code edits |
| **Tests** | Must pass before task completion |
| **Evals** | Generator + Evaluator agent pattern |
| **Human Review** | Escalation triggers |

**Key insight (Anthropic):** Models cannot reliably evaluate their own work. Solution: GAN-inspired Generator + Evaluator separation.

---

## Claude Code Harness Architecture

### The Harness Pattern

```
MODEL + LOOP + TOOLS + MEMORY + PERMISSIONS + HOOKS
   │       │       │        │          │         │
   │       │       │        │          │         └── Lifecycle intervention
   │       │       │        │          └────────── Access control
   │       │       │        └───────────────────── Context persistence
   │       │       └────────────────────────────── External capabilities
   │       └────────────────────────────────────── Agentic loop
   └────────────────────────────────────────────── Core intelligence
```

### Agentic Loop

```python
# Simplified Claude Code loop
while True:
    response = model.call(system_prompt, tools, context)
    if response.is_text_only():
        return response  # Done
    for tool_call in response.tool_calls:
        if permission.check(tool_call) == ALLOW:
            result = execute(tool_call)
            context.append(result)
        else:
            context.append(permission_denied_message)
```

### Permission Pipeline

```
Tool Call → Rule Matching (deny > ask > allow)
         → Mode Check (default/plan/auto/bypass)
         → Hook Intervention (PreToolUse)
         → Execution OR Block
         → Post-processing (PostToolUse hook)
```

---

## Industry Comparison

| Framework | Guardrails | Hooks | Control Plane | State |
|-----------|------------|-------|---------------|-------|
| **Claude Code** | Permission rules + modes | 23+ events | Deterministic | Memory hierarchy |
| **OpenAI Agents SDK** | Guardrails, handoffs | Lifecycle hooks | Input/output validation | Checkpoints |
| **LangGraph** | Interrupts | State hooks | Conditional edges | Checkpointing |
| **CrewAI** | Human input | Task hooks | Delegation rules | Memory |
| **AutoGen** | Termination conditions | Conversation hooks | Human proxy | Context |
| **Semantic Kernel** | Filters | Middleware | Pipeline | State |

---

## CNCF Four Pillars of Platform Control

Applied to agent management:

| Pillar | Agent Application |
|--------|-------------------|
| **Observability** | Audit trails, cost tracking, confidence monitoring |
| **Security** | Permission boundaries, path restrictions, secret isolation |
| **Governance** | Policy enforcement, compliance logging, rate limits |
| **Lifecycle** | Session start/end, checkpoints, state persistence |

---

## Best Practices

### DO
- Use hooks to enforce state validation at commit time
- Implement circuit breakers for confidence monitoring
- Separate Generator and Evaluator agents
- Keep tool count manageable (7±2 per domain)
- Use Meta-Tool pattern for large tool sets
- Log all agent actions for audit

### DON'T
- Rely on model self-evaluation
- Skip permission checks for "trusted" operations
- Allow direct database/API access without control plane
- Ignore context budget (90% fill = degraded capability)
- Build agents without feedback loops

---

## Anti-Patterns

1. **Over-autonomy**: Giving agents unrestricted access without guardrails
2. **Under-observability**: No audit trails or cost tracking
3. **No circuit breakers**: Allowing cascading failures
4. **Self-evaluation**: Expecting models to judge their own work
5. **Global permissions**: Not scoping rules to specific paths/contexts
6. **Missing hooks**: No intervention points in agent lifecycle

---

## Relationship to Context Engineering

Context Engineering is a **subset** of Harness Engineering.

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

**Key distinction:**
- Context Engineering: "Make sure the model sees the right database schema"
- Harness Engineering: "Ensure it still has to run the linter, pass tests, respect permissions, and save progress"

---

## 2026 Trends

1. **Agent Management Platforms**: Gartner's call for centralized governance
2. **AI Gateways**: Kong, enterprise LLM governance, audit trails
3. **Standardized Protocols**: MCP adoption (97M+ SDK downloads)
4. **A2A Protocol**: Agent-to-agent communication standard
5. **Confidence-based Circuit Breakers**: Real-time quality monitoring
6. **Zero-trust Agent Architecture**: Every action requires verification

---

## Sources

### Core References
- [Louis Bouchard: Harness Engineering](https://www.louisbouchard.ai/harness-engineering/)
- [Epsilla: Why Harness Engineering Replaced Prompting](https://www.epsilla.com/blogs/harness-engineering-evolution-prompt-context-autonomous-agents)
- [CIO.com: The Agent Control Plane](https://www.cio.com/article/4130922/the-agent-control-plane-architecting-guardrails-for-a-new-digital-workforce.html)
- [htek.dev: Agent Harnesses](https://htek.dev/articles/agent-harnesses-controlling-ai-agents-2026/)

### Claude Code
- [Claude Code Hooks Reference](https://code.claude.com/docs/en/hooks)
- [Claude Code Permissions](https://code.claude.com/docs/en/permissions)
- [Inside Claude Code Architecture](https://medium.com/@kanishks772/inside-claude-code-the-architecture-nobody-explains-01c9aec630ef)
- [Claude Code Harness (GitHub)](https://github.com/Chachamaru127/claude-code-harness)

### Research
- [Anthropic: Effective Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Microsoft CORPGEN (Feb 2026)](https://www.marktechpost.com/2026/02/26/microsoft-research-introduces-corpgen/)
