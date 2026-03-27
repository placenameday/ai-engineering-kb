# project-init Skill — Design (v2)

> Updated: 2026-03-27. v2 incorporates full Harness engineering + Context engineering understanding.

## What Changed (v1 → v2)

| Aspect | v1 | v2 |
|--------|----|----|
| Project types | 3 (research/engineering/docs) | 4 (+ agent) |
| Setup philosophy | File structure | Harness engineering maturity |
| Context engineering | Not considered | Built-in from day 1 |
| Memory system | Just create dir | MEMORY.md index + progressive disclosure |
| Permissions | Ignored | Analyze + recommend |
| Output | File list | Harness maturity checklist |

## Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| Add `agent` project type | Agent projects need MCP, guardrails, control plane from start |
| Create MEMORY.md at init | Progressive disclosure: index loaded automatically, details on-demand |
| Output maturity checklist | User sees the optimization path from day 1 |
| Permissions = recommend only | Don't auto-modify settings; user decides |
| CLAUDE.md = Harness Status section | Makes Harness components visible, trackable |

## Location

- Skill: `~/.claude/skills/project-init/SKILL.md` (global)
- Trigger: `/project-init` or "项目初始化"

## Related

- `project-refine-design-core.md` — Refine companion skill
- `practical-guide.md` — Full optimization entry point
