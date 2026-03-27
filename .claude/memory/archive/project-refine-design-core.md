# project-refine Design: Core (v2)

> Updated: 2026-03-27. v2 upgrades from file-structure audit to Harness maturity diagnosis.

## What Changed (v1 → v2)

| Aspect | v1 | v2 |
|--------|----|----|
| Scope | .claude/ file structure | 4-layer: Infrastructure + Context + Quality + Architecture |
| Scoring | File size metrics | Harness maturity score |
| Diagnosis | Lines, counts, redundancy | + Progressive disclosure, context budget, agent coordination |
| Output | Issues + fixes | + Optimization roadmap with priorities |
| Architecture | Not considered | Multi-agent readiness check |

## Workflow (v2)

```
Phase 0: Environment Detection
Phase 1: Discovery — scan 4 layers (Infrastructure, Context, Quality, Architecture)
Phase 2: Diagnosis — health check per layer
Phase 3: Scoring — weighted Harness maturity score
Phase 4: Report — layer status + issues + proposed changes
Phase 5: Execution — apply with safety boundary
Phase 6: Optimization Roadmap — prioritized next steps
```

## Positioning

```
Layer 1 (High-freq): revise-claude-md
├── Trigger: Every session end
└── Scope: Session learnings → CLAUDE.md incremental

Layer 2 (Low-freq): /project-refine
├── Trigger: Monthly OR "project feels heavy" OR post-project-init
└── Scope: Full Harness maturity audit

Layer 3 (On-demand): practical-guide.md
├── Trigger: User wants deep optimization
└── Scope: Step-by-step optimization path
```

## 4-Layer Model

| Layer | Checks | Weight |
|-------|--------|--------|
| Infrastructure (Harness) | CLAUDE.md, permissions, hooks, MCP | 20% |
| Context Engineering | Progressive disclosure, memory, budget, rules | 30% |
| Quality | Review, tests, evals | 15% |
| File Health | Sizes, counts, redundancy | 25% |
| Redundancy | Duplicates, empty files | 10% |

## Location

- Command: `~/.claude/commands/project-refine.md` (global)
- Trigger: `/project-refine`

## Related

- `project-refine-design-metrics.md` — Scoring details
- `project-init-design.md` — Init companion
- `practical-guide.md` — Full optimization entry point
