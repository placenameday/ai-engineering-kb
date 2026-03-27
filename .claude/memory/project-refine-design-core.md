# project-refine Design: Core

**Date**: 2026-03-20 | **Type**: Command | **Status**: Finalized

## Design Decisions

| Domain | Decision |
|--------|----------|
| Detection | Exact match + line dedup + structure compare (no semantic) |
| Scoring | 5-dimension weighted, 100-point, A-F grade |
| Split | By topic/section, thresholds: CLAUDE.md>50, Memory>200, Skill>300 |
| Token | Simplified to structural indicators, no direct estimation |

## Positioning

```
Layer 1 (High-freq): revise-claude-md
├── Trigger: Every session end
└── Scope: Session learnings → CLAUDE.md incremental

Layer 2 (Low-freq): /project-refine
├── Trigger: Monthly/quarterly OR "project feels heavy"
└── Scope: Full .claude/ architecture audit
```

**Why Command not Skill**: Low frequency, saves ~100 words description from always-loaded context.

## Workflow

```
Phase 0: Environment Detection
Phase 1: Discovery (scan, count, build dependency map)
Phase 2: Diagnosis (structure, redundancy, budget, consistency)
Phase 3: Report (score, issues, recommendations, preview diffs)
Phase 4: Execution (user selects, auto-execute, log for rollback)
```

## Detection Dimensions

| Dimension | Checks | Priority |
|-----------|--------|----------|
| Structure | CLAUDE.md lines, memory count/size | P0 |
| Consistency | Index completeness, frontmatter | P0 |
| Redundancy | Duplicates, stale, empty files | P1 |

## Redundancy Detection

```
Phase 1: Exact Match
├── Detect identical lines/paragraphs
├── Flag file pairs with >30% overlap
└── Output duplicate line list

Phase 2: Structure Comparison
├── Compare heading/section structure
├── Detect isomorphic but different content
└── Flag as "suspected functional overlap"

Thresholds:
├── Identical lines >5 → suggest merge
├── File overlap >30% → flag for review
└── Structure similarity >70% → flag suspected overlap
```

No semantic similarity detection (high cost, low value).
