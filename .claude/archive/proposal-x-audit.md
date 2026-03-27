# proposal_x Claude Code Setup Audit

**Date**: 2026-03-07
**Project**: `/Users/placenameday/R_Project/Research/proposal_x/`
**Type**: AI-assisted grant proposal writing system (Psychology/Cognitive Psychology domain)
**Git**: Yes (`.git/` present)

---

## Current State Summary

A sophisticated, production-grade Claude Code project for Chinese research proposal generation (NSFC, provincial, military). Uses the full 5-agent team (Nova/Sage/Iris/Hawk/Rex) plus 1 custom agent (Reviewer). Features a phase-based workflow (`/proposal` command routes through research → writing → export), 9 skills, 5 rules, 12 memory files across 4 subdirectories, a local secrets schema, 4 MCP servers, and a project template system. Two active projects exist under `projects/`. Total `.claude/` directory: 344KB.

---

## CLAUDE.md Analysis

- **Line count**: 43 lines
- **Structure**: Compliant with index-only convention
  - Title + description (3 lines)
  - Memory Index table (11 entries, well-organized)
  - Skills table (9 entries with triggers)
  - Rules table (4 entries with scope and content summary)
- **Compliance**: EXCELLENT. Pure index, no inline knowledge. All substance in memory files.
- **Issues**:
  - Rules table lists 4 rules but 5 exist on disk (`memory-discipline.md` not listed in CLAUDE.md)
  - Memory index references `local/personal/*.md` and `local/team.md` — these are under `.claude/local/`, not `.claude/memory/`. Correct but could confuse since table is labeled "Memory Index"
  - No `kb/system-design-reference.md` listed in Memory Index (208 lines, largest memory file)

---

## Memory Organization

**File count**: 12 files across 4 subdirectories (52KB total)

| File | Lines | Role | Quality |
|------|-------|------|---------|
| `toolkit.md` | 49 | User toolstack inventory | GOOD — well-structured tables, current |
| `gotchas.md` | 31 | Known pitfalls | EXCELLENT — battle-tested, specific, actionable |
| `nsfc-guide.md` | 43 | NSFC structure & criteria | GOOD — concise reference card |
| `reviewer-guide.md` | 17 | Review scoring criteria | OK — thin, mostly duplicates reviewer agent content |
| `research-design.md` | 19 | Study design tips | GOOD — domain-specific, practical |
| `writing/expressions.md` | 98 | Chinese academic templates | EXCELLENT — comprehensive, well-categorized |
| `cases/analysis.md` | 22 | Exemplary case analysis | PLACEHOLDER — empty table, no cases populated yet |
| `cases/postmortem-template.md` | 63 | Retrospective template | GOOD — structured template with process metrics |
| `kb/system-design-reference.md` | 208 | System architecture doc | VERY LARGE — authoritative but 208 lines; serves as onboarding doc, not daily reference |
| `kb/cross-project-reuse.md` | 43 | Multi-project setup guide | GOOD — symlink strategy, reuse classification |
| `kb/word-template-filling.md` | 28 | python-docx decision log | GOOD — specific technical decisions |

**Naming**: Consistent kebab-case with logical subdirectory grouping (cases/, kb/, writing/).

**Issues**:
1. `cases/analysis.md` is empty placeholder — no value until populated
2. `kb/system-design-reference.md` at 208 lines is the largest single file; has YAML frontmatter (unique among memory files). Content is excellent but heavy for routine loading
3. `reviewer-guide.md` content overlaps significantly with `agents/reviewer.md` (review dimensions, deduction items). Could be eliminated
4. No `domain/` subdirectory exists yet (referenced in skills as future knowledge correction target)
5. Memory index in CLAUDE.md is missing `kb/system-design-reference.md` — the largest and most important reference doc

---

## Skills Assessment

**Count**: 9 skills (9 SKILL.md files)
**Total size**: 208KB (includes reference docs and Python scripts)

| Skill | Lines | Has Frontmatter | Has References | Has Scripts | Notes |
|-------|-------|-----------------|----------------|-------------|-------|
| `proposal-research` | 179 | Yes | No | No | Phase 0-1, well-structured |
| `proposal-write` | 330 | Yes | No | No | LARGEST skill; Phase 2-3 |
| `proposal-export` | 61 | Yes | No | No | Phase 4, concise |
| `module-writing-guide` | 170 | Yes | No | No | Module-specific writing rules |
| `proposal-style-review` | 264 | Yes | No | No | Full style guide from 4 exemplary proposals |
| `external-tool-templates` | 199 | Yes | No | No | T1-T7 user operation templates |
| `pubmed-database` | 81 | Yes | Yes (3 files) | No | K-Dense skill, compact |
| `openalex-database` | 131 | Yes | Yes (2 files) | Yes (2 py) | K-Dense skill, has client code |
| `semantic-scholar` | 140 | Yes | Yes (1 file) | Yes (2 py + __pycache__) | Has client code |

**Format consistency**: ALL skills have proper YAML frontmatter with `name` and `description`. K-Dense skills additionally have `license` and `metadata.skill-author`. All use `SKILL.md` filename convention.

**Issues**:
1. `proposal-write` at 330 lines is very long — contains Phase 2 (5 modules) + Phase 3 (review cycle) + preference learning + domain correction + profile accumulation. Borderline for single-file management
2. `__pycache__/` directory exists inside `semantic-scholar/scripts/` with a `.pyc` file — should be gitignored or cleaned
3. Content overlap: `module-writing-guide` and `proposal-style-review` share significant content (three-level gap progression, evidence funnel, NSFC vs SSFC comparison, innovation packaging). `proposal-style-review` is the superset; `module-writing-guide` is the operational subset. Intentional layering but worth noting
4. `external-tool-templates` could be split into individual template files for on-demand loading (currently 199 lines loaded as one block)

---

## Rules Assessment

**Count**: 5 rules (158 lines total, 20KB on disk)

| Rule | Lines | Has path_patterns | Scope | Effectiveness |
|------|-------|-------------------|-------|---------------|
| `phase-context.md` | 16 | No | global | EXCELLENT — compact loading matrix, prevents unnecessary context |
| `writing-style.md` | 38 | Yes (`projects/**`) | projects/ | GOOD — core writing norms, banned phrases |
| `writing-pitfalls.md` | 40 | Yes (`projects/**`) | projects/ | GOOD — domain-specific pitfalls |
| `project-file-org.md` | 40 | No | global | GOOD — canonical directory structure |
| `memory-discipline.md` | 24 | Yes (`projects/**`) | projects/ | EXCELLENT — hard limits, agent boundaries |

**Best practices compliance**:
- 3/5 rules have proper `path_patterns` frontmatter for scoped auto-loading
- `phase-context.md` and `project-file-org.md` lack `path_patterns` — they're global rules, which is intentional but not explicitly documented in frontmatter
- All rules are concise (16-40 lines), well within recommended limits
- No contradictions between rules detected
- `memory-discipline.md` duplicates content from `commands/proposal.md` Memory Discipline section — intentional reinforcement

**Issues**:
1. `memory-discipline.md` not listed in CLAUDE.md rules table (only 4 rules listed, 5 exist)
2. `phase-context.md` has no path_patterns frontmatter — loads globally on every file edit, including non-project files. Should either add a path pattern or document why it's global
3. `project-file-org.md` similarly lacks path_patterns — appropriate for global scope but inconsistent with other rules having frontmatter

---

## Settings & Security

### settings.local.json (79 lines)
- **Permissions**: 74 allow rules, no deny rules
- **MCP**: `enableAllProjectMcpServers: true`, `enabledMcpjsonServers: ["scite"]`
- **Security observations**:
  - `Bash(rm:*)` is allowed — broad deletion permission
  - `Bash(git:*)` is allowed — all git operations permitted
  - Many domain-specific WebFetch allows (scite.ai, consensus.app, semanticscholar.org, etc.)
  - No `Bash(sudo:*)` or `Bash(chmod:*)` — good
  - Permissions are accumulated organically (74 entries suggests interactive approval over time, not curated)
  - `settings.local.json` is in `.gitignore` — correct for local-only settings

### .mcp.json (4 servers)
- `scite`: HTTP remote MCP (OAuth)
- `zotero`: uvx-based, local mode (`ZOTERO_LOCAL=true`)
- `markitdown`: uvx-based, file conversion
- `pdf-reader`: npx-based, PDF reading

### Local secrets schema
- `.claude/local-schema.md` documents expected structure for `.claude/local/` (gitignored)
- 5 local files exist: `personal/profile.md`, `personal/expertise.md`, `personal/publications.md`, `personal/projects.md`, `team.md`
- All properly gitignored via `.gitignore` entry `.claude/local/`
- Schema includes bilingual instructions (Chinese primary)

### Credential locations (documented in toolkit.md)
- `~/.zotero.env`: ZOTERO_USER_ID + ZOTERO_API_KEY
- `~/.semantic_scholar.env`: S2_API_KEY
- Both in home directory, not in project — correct

---

## Additional Components

### agents/ (1 file)
- `reviewer.md` (194 lines): Custom reviewer agent with dynamic role config (NSFC/provincial/military), 5-dimension scoring, 3-tier problem severity, review report template. Well-structured, uses `model: opus`, properly scoped tools (Read, Grep, Glob, mcp__scite).

### commands/ (1 file)
- `proposal.md` (140 lines): Main router command. Handles project selection, profile loading, inbox processing, phase routing, memory health checks, session handoff. This is the orchestration brain.

### templates/ (1 file)
- `project-progress-init.md` (45 lines): Template for new project progress files. Has proper placeholder format with `{PROJECT_NAME}` and `{YYYY-MM-DD}`.

### .gitignore
- Properly ignores: `projects/`, `.claude/local/`, binary files, `__pycache__/`, `settings.local.json`
- Explicitly preserves: `.claude/`, `.mcp.json`, `.gitignore`, `CLAUDE.md`
- Issue: `__pycache__/` gitignored at root but exists inside `.claude/skills/semantic-scholar/scripts/` — the gitignore pattern should catch it but the directory exists on disk

---

## Identified Issues

### Critical
1. **CLAUDE.md index incomplete**: `memory-discipline.md` rule and `kb/system-design-reference.md` memory file missing from index. Agents relying on CLAUDE.md as discovery mechanism will miss these.

### Moderate
2. **`proposal-write` skill at 330 lines**: Approaches complexity threshold. Contains 5 module specs + review cycle + 3 accumulation mechanisms (preference learning, domain correction, profile accumulation). Consider splitting accumulation mechanisms into a separate skill.
3. **`reviewer-guide.md` redundancy**: 17-line memory file overlaps with 194-line `agents/reviewer.md`. The agent file is the authoritative source; the memory file adds no unique content.
4. **`cases/analysis.md` empty**: Placeholder with no populated cases. Framework exists (postmortem template) but no data yet — expected for a newer project.
5. **74 permission entries in settings.local.json**: Accumulated organically, not curated. Many are one-off approvals (e.g., `Bash(printf writing:*)`, `Bash(do echo:*)`, `Bash(do cp:*)`). Could be consolidated.
6. **`__pycache__/` in skills directory**: `semantic-scholar/scripts/__pycache__/` contains compiled Python — should be cleaned.

### Minor
7. **Phase-context rule loading**: `phase-context.md` references `{PROJECT_DIR}/.claude-phase` file but has no `path_patterns` frontmatter, meaning it loads globally even for non-project file edits.
8. **Content overlap between skills**: `module-writing-guide` and `proposal-style-review` share ~40% content. Intentional (operational vs audit views) but increases total token load if both loaded.
9. **No `.claude/agents/`, `.claude/commands/`, `.claude/templates/` listed in CLAUDE.md**: Only skills, rules, and memory are indexed. Agent, command, and template are discoverable by convention but not documented in the index.
10. **Mixed language in skill files**: K-Dense skills (pubmed, openalex) are in Chinese; custom skills are in English. Minor inconsistency.

---

## Architecture Strengths (for optimization guide context)

1. **Phase-based context loading**: `phase-context.md` prevents unnecessary memory loading — saves tokens
2. **Memory discipline with hard limits**: 3 files / 30KB per project memory — prevents bloat
3. **Agent write boundaries**: Clear rules on which agent writes where — prevents conflicts
4. **Local secrets schema**: Documented expected structure for gitignored sensitive data
5. **Bidirectional inbox pattern**: Single staging area for both system-to-user and user-to-system communication
6. **Cross-project reuse design**: Symlink strategy for shared knowledge (expressions, local data)
7. **Skill layering**: Compact SKILL.md + on-demand reference files + Python scripts — K-Dense pattern
8. **Session handoff protocol**: Explicit cross-session continuity mechanism
9. **Literature zero-hallucination protocol**: Three-layer defense with verification status annotations

---

## Metrics Summary

| Component | Count | Total Lines | Total Size |
|-----------|-------|-------------|------------|
| CLAUDE.md | 1 | 43 | 1.9KB |
| Memory files | 12 | ~570 | 52KB |
| Skills | 9 | ~1,555 | 208KB (incl. refs/scripts) |
| Rules | 5 | 158 | 20KB |
| Agents | 1 | 194 | - |
| Commands | 1 | 140 | - |
| Templates | 1 | 45 | - |
| MCP servers | 4 | - | - |
| Permissions | 74 allow | - | - |
| Local files | 5 | - | gitignored |
| **Total .claude/** | - | - | **344KB** |
