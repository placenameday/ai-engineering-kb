# project-refine Skill Research

**Date**: 2026-03-20
**Purpose**: Design direction for Claude Code project architecture optimization skill

---

## 1. Context Engineering Core Principles

From `concepts/context-engineering.md`:

### 1.1 Four Core Patterns (Anthropic Framework)

| Pattern | What | Claude Code Implementation |
|---------|------|---------------------------|
| **Write** | Persist externally | `.claude/memory/` files |
| **Select** | Pull relevant context in | Skills (description match), @imports |
| **Compress** | Reduce token usage | `/compact`, agent summaries |
| **Isolate** | Partition across systems | Sub-agents with own context |

### 1.2 Memory Loading Mechanisms

| Layer | Loading | Budget Target |
|-------|---------|---------------|
| CLAUDE.md | Always | ~50-200 lines |
| @imports | Always (expanded) | Use sparingly |
| .claude/rules/*.md | Conditional (path-scoped) | Per-directory context |
| .claude/skills/ | On-demand (description match) | 2% of context for descriptions |
| .claude/memory/ | On-demand (agent reads) | Unlimited |

### 1.3 Context Placement Decision Criteria

| Destination | Criteria |
|-------------|----------|
| **Skill** | Large (>100 lines) + infrequent + self-contained |
| **Rule** | Directive + path-matchable + always relevant |
| **Memory** | Reference type / accumulative / sensitive / system-level |

### 1.4 Anti-Patterns

1. Stuffing CLAUDE.md (every line costs tokens)
2. Generic instructions (waste tokens)
3. Stale memory (audit every 2 weeks)
4. Too many MCP tools (35 = ~26K tokens)
5. Over-importing (@imports always loaded)
6. Ignoring context budget (90% fill = degraded performance)

---

## 2. Existing Tools Analysis

### 2.1 claude-md-improver (Official Plugin)

**Focus**: CLAUDE.md file quality only

**Workflow**:
1. Discovery: Find all CLAUDE.md files
2. Quality Assessment: 6 criteria scoring (Commands, Architecture, Patterns, Conciseness, Currency, Actionability)
3. Quality Report: Grade A-F
4. Targeted Updates: User-approved diffs

**Limitations**:
- Only audits CLAUDE.md, not entire .claude/ architecture
- No memory file analysis
- No rules/skills optimization

### 2.2 revise-claude-md (Official Command)

**Focus**: Session learnings → CLAUDE.md updates

**Workflow**:
1. Reflect on session learnings
2. Find CLAUDE.md files
3. Draft additions
4. Show proposed changes
5. Apply with approval

**Limitations**:
- Reactive (after session)
- Only updates, no architecture analysis

### 2.3 claude-code-optimization-guide.md (This Project)

**Focus**: Comprehensive project optimization

**Structure**:
- P0 Critical Fixes (index completeness)
- P1 Structural Optimization (file splitting, redundancy removal)
- P2 Maintenance (cleanup, frontmatter)
- P3 Architecture Enhancement (health check, freshness tracking)

**Strengths**: Priority-based, actionable, specific metrics

---

## 3. Gap Analysis

### What's Missing

| Gap | Description |
|-----|-------------|
| **Holistic View** | No tool analyzes entire .claude/ architecture together |
| **Memory Audit** | No automated check for stale/fragmented memory files |
| **Context Budget** | No tool estimates token cost of current setup |
| **Rules Efficiency** | No analysis of path_patterns effectiveness |
| **Skills Coverage** | No check for description overlap/ambiguity |

### Opportunity Space

```
project-refine should:
1. Analyze ENTIRE .claude/ ecosystem (not just CLAUDE.md)
2. Apply context engineering principles systematically
3. Estimate context budget impact
4. Suggest structural improvements with priority
```

---

## 4. Design Direction Options

### Option A: Audit-Only (Low Risk)

- Scan .claude/ structure
- Output health report
- User manually fixes

### Option B: Guided Refinement (Medium Risk)

- Audit + specific recommendations
- User approves each change
- Agent executes approved changes

### Option C: Auto-Optimize (High Risk)

- Audit + auto-apply safe optimizations
- Flag risky changes for review
- Requires strong safety guards

### Recommendation

**Option B** - balances automation with user control

---

## 5. Key Metrics to Track

| Metric | Healthy Range | Warning |
|--------|--------------|---------|
| CLAUDE.md lines | ≤30 ideal, ≤50 acceptable | >50 |
| Memory files count | ≤12 | >15 |
| Single memory size | ≤100 lines | >200 |
| Skill size | ≤200 lines | >300 |
| Rules with path_patterns | 100% | <100% |
| Total memory size | ≤60KB | >100KB |

---

## 6. Next Steps

1. Define audit dimensions (structure, budget, freshness, redundancy)
2. Design output format (report + actionable recommendations)
3. Determine safe auto-fixes vs. user-approval-required
4. Prototype on this project
