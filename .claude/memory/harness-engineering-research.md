---
name: harness-engineering-research
description: Harness Engineering深度调研笔记，2026-03-27
type: project
---

# Harness Engineering Research Notes

## Core Discovery

**2026 Paradigm Shift:** AI interaction evolved through three phases:
1. Prompt Engineering (2022-2024): "What to ask?"
2. Context Engineering (2025): "What to send?"
3. **Harness Engineering (2026): "How does it operate?"**

## Key Distinction

- **Context Engineering**: What the model sees
- **Harness Engineering**: How the system behaves

Quote (Louis Bouchard): "Context engineering might make sure the model sees the right database schema. Harness engineering is the reason it still has to run the linter, pass the tests, respect permissions, and save progress."

## Harness Components

1. **Permissions** - Access control, tool-level restrictions
2. **Hooks** - Lifecycle intervention, deterministic control
3. **Guardrails** - Safety boundaries, control plane
4. **State Management** - Checkpoints, memory persistence
5. **Feedback Loops** - Linters, tests, evals, human review
6. **Isolation** - Sandboxing, worktree separation

## Claude Code Harness Pattern

```
MODEL + LOOP + TOOLS + MEMORY + PERMISSIONS + HOOKS
```

### Permission Modes
- `default`: Prompt on first use
- `plan`: Read-only
- `acceptEdits`: Auto-accept edits
- `bypassPermissions`: Skip prompts (with exceptions)
- `auto`: Full autonomy with boundaries

### Key Hooks
- `PreToolUse`: Can block tool calls
- `PostToolUse`: React after execution
- `Stop`: Quality gates before completion
- `SessionStart/End`: Context lifecycle

## Control Plane Architecture

Agent cannot directly access outside world. All requests go through deterministic control plane:
- Budget check
- Scope check
- Confidence check
- Rate limit
- Audit log

## Industry Consensus

- "Agents aren't hard; the Harness is hard" (OpenAI/Anthropic)
- 80% of Fortune 500 have active AI agents (Microsoft)
- Gartner: 40% enterprise apps will have agents by late 2026
- 2026 focus: Building infrastructure that controls agents, not more agents

## Sources

- Louis Bouchard: Harness Engineering
- Epsilla Blog: Paradigm Evolution
- CIO.com: Agent Control Plane
- htek.dev: Agent Harnesses
- Claude Code Docs: Hooks, Permissions
