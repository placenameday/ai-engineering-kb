# project-refine Design: Metrics & Execution

> Part 2 of project-refine-design split. See also: `project-refine-design-core.md`

## Health Metrics

| Metric | Healthy | Warning | Critical |
|--------|---------|---------|----------|
| CLAUDE.md lines | ≤30 | 30-50 | >50 |
| Memory files | ≤12 | 12-15 | >15 |
| Single memory | ≤100 lines | 100-200 | >200 |
| Single skill | ≤200 lines | 200-300 | >300 |
| Rules with path_patterns | 100% | <100% | - |
| Total memory size | ≤60KB | 60-100KB | >100KB |

## Health Score Formula

```
Score = Σ (dimension_score × weight)

Dimensions:
- CLAUDE.md: 25% (≤30 lines, index structure)
- Memory: 25% (≤12 files, ≤60KB, ≤100 lines each)
- Skills: 20% (each ≤200 lines)
- Rules: 15% (100% have path_patterns)
- Redundancy: 15% (no duplicates, no empty files)

Grading:
- 90-100: A (Healthy)
- 70-89: B (Good)
- 50-69: C (Needs optimization)
- 30-49: D (Needs restructuring)
- 0-29: F (Needs rebuild)
```

## Split Thresholds

| File Type | Threshold | Method |
|-----------|-----------|--------|
| CLAUDE.md | >50 lines | Extract details → memory/skill |
| Memory | >200 lines | Split by heading into topic files |
| Skill | >300 lines | Split by Phase/function |

## Secondary Index Strategy

- <12 memory files: Flat, use Category column in CLAUDE.md
- ≥12 memory files: Suggest creating subdirectories by topic

## Edge Cases

| Scenario | Detection | Action |
|----------|-----------|--------|
| No .claude/ | Directory missing | Suggest `/project-init` |
| Only CLAUDE.md | 1 file | Light mode, check lines only |
| No CLAUDE.md | Has memory/rules but no index | P0: Create index |
| Empty files | 0 bytes or only frontmatter | Suggest delete or fill |
| Large project | >50 memory files | Warning + suggest archive |
| Nested CLAUDE.md | Subdirs have own CLAUDE.md | List, don't recurse |

## Safety Boundary

**Auto-fix (No confirmation):**
- Add missing frontmatter (path_patterns)
- Update CLAUDE.md index
- Delete empty files/placeholders

**Requires Confirmation:**
- Split/merge files
- Remove suspected redundant content
- Move content location

**Never Auto-execute:**
- Delete files with content
- Modify business logic content
- Change project-specific conventions

## Output Format

```markdown
## project-refine Report
### Summary (score, metrics)
### Issues Found (P0/P1/P2 tables)
### Proposed Changes (diffs with Apply? [Y/n])
### Execution Summary (before/after, recommendations)
```
