# Claude Code Configuration Ecosystem Analysis

**Investigator**: Sage | **Date**: 2026-03-07 | **Scope**: Full user setup across ~/R_Project/

---

## 1. Global Config (`~/.claude/`)

### CLAUDE.md — Global Instructions
- **Concision directive**: "Sacrifice grammar for concision"
- **Search priority**: GitHub MCP > Metaso > Bailian > Default web search
- **5-agent team**: Main (Opus coordinator), Nova (planner), Rex (Sonnet implementer), Iris (Opus writer), Sage (Opus researcher), Hawk (Sonnet reviewer)
- **Hard orchestration rules**:
  - Complex tasks → Nova first
  - Unknown territory → Sage before acting
  - Code written → ALWAYS Hawk next
  - Agents don't call each other; main session coordinates
- **Memory protocol**: ≤200 words → return directly; >200 → write `.claude/memory/<topic>.md`, return path only
- **CLAUDE.md structure rule**: Project CLAUDE.md is INDEX ONLY (≤30 lines ideal). All knowledge → memory files.

### settings.json — Global Settings
- **Model**: `opus` (default)
- **Env vars**:
  - `API_TIMEOUT_MS`: 600000 (10 min)
  - `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`: 70%
  - `SERENA_DISABLE_UI`: 1
  - `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`: 1
- **Hook — SessionStart**: Auto-detects if CLAUDE.md > 50 lines, warns to slim down → enforces the "index only" rule
- **MCP servers**: Metaso (Chinese AI search engine, HTTP MCP with bearer token)
- **8 enabled plugins** (active): claude-md-management, code-simplifier, context7, feature-dev, github, playwright, serena, skill-creator
- **Permissions**: Empty allow/deny lists (relies on project-level)

### Plugin Ecosystem (14 installed total)
From `claude-plugins-official` marketplace:
- context7, code-simplifier, github, ralph-loop, frontend-design, feature-dev, superpowers (v4.3.1), claude-md-management, playwright, serena, typescript-lsp, pyright-lsp, skill-creator

From `zai-coding-plugins` marketplace (local npm):
- glm-plan-usage (v0.0.1)

**Blocklist**: code-review (test), fizz (security test)

### Custom Slash Commands (`~/.claude/commands/`)
| Command | Purpose |
|---------|---------|
| `backup.md` | Backs up ~/.claude/ config with secret redaction → tarball on Desktop |
| `quarto-spec.md` | Quarto .qmd format reference (read-only) |
| `rmd-to-quarto.md` | Converts .Rmd → .qmd with full YAML/chunk transformation |

### Skills (`~/.claude/skills/`)
- `learned/` directory exists but is **empty** — no globally learned skills yet

---

## 2. Project CLAUDE.md Patterns

### Pattern A: Index-Only (follows global rule)
**Example**: `proposal_x/CLAUDE.md` (32 lines)
- 1-line description
- Memory Index table (8 entries)
- Skills table (4 entries with triggers)
- Rules table (5 entries with scope)
- **Most mature project** — full skills/rules/memory ecosystem

**Example**: `ai-engineering-kb/CLAUDE.md` (27 lines)
- Description + Structure table + Framework summary + Conventions
- No memory index table yet (memory files exist but aren't indexed)

### Pattern B: Comprehensive (pre-dates the index rule)
**Example**: `memo-recall/CLAUDE.md` (202 lines) — full architecture docs inline
**Example**: `aichat2md/CLAUDE.md` (230 lines) — complete project manual inline
- These violate the ≤50-line rule; SessionStart hook would warn
- Represent older projects not yet migrated to index+memory pattern

### All Projects with CLAUDE.md (14 total)
**Research** (6): 光电项目, proposal_x_back_up, ai-engineering-kb, mindfullness_plot, edf_demo, proposal_x
**Tech** (8): Yc, claude-code-backup, my_immich, aichat2md, cc-glm, web总结, agentic_work_flow, memo-recall

---

## 3. Memory File System

### Projects with `.claude/memory/` (8 total)
| Project | Memory Files | Notable |
|---------|-------------|---------|
| proposal_x | 7 files + `cases/`, `kb/`, `writing/` subdirs | Most structured; uses subdirectories |
| ai-engineering-kb | 9 files | Research-heavy (audit, synthesis, survey) |
| 光电项目 | exists | — |
| proposal_x_back_up | exists | Mirror of proposal_x |
| Yc | 2 files | Infra notes, mobile SSH research |
| claude-code-backup | 1 file | project.md |
| agentic_work_flow | exists | — |
| proposal_x/projects/陕西省... | nested subproject memory | — |

### Memory File Categories Observed
- **Audit/review**: kb-audit-2026-03.md, proposal-x-audit.md
- **Plans**: reorg-plan.md, survey-report-plan.md, index-framework-design.md
- **Research output**: survey-web-research.md, survey-report-output.md, kb-synthesis.md
- **Optimization guides**: claude-code-optimization-guide.md
- **Domain knowledge**: nsfc-guide.md, reviewer-guide.md, research-design.md, toolkit.md, gotchas.md
- **Writing resources**: writing/expressions.md (Chinese academic templates)
- **Case studies**: cases/*.md, kb/*.md (proposal examples)

### Naming Convention
- kebab-case.md
- Descriptive topic names, not dates (exception: kb-audit-2026-03.md includes date)
- Subdirectories for related collections (cases/, kb/, writing/)

---

## 4. Rules System (Project-Level)

Only `proposal_x` uses rules (5 total):
| Rule | Scope | Purpose |
|------|-------|---------|
| `memory-discipline.md` | `projects/**` | Agent memory write boundaries |
| `phase-context.md` | global | Phase-aware context loading |
| `writing-style.md` | `projects/**` | Core writing rules |
| `writing-pitfalls.md` | `projects/**` | Common writing mistakes |
| `project-file-org.md` | global | inbox/ staging + references/ structure |

---

## 5. Skills System (Project-Level)

Only `proposal_x` uses project skills (9 skills with SKILL.md):
| Skill | Purpose |
|-------|---------|
| proposal-write | Grant writing with per-module references |
| proposal-research | Research phase of proposal |
| proposal-export | Export proposal |
| proposal-style-review | Hawk review / style audit |
| external-tool-templates | Consensus/Elicit operations |
| pubmed-database | PubMed literature search (with API reference, search syntax, common queries) |
| openalex-database | OpenAlex bibliometrics (with API guide, common queries) |
| semantic-scholar | Semantic Scholar search (with API guide) |

**Pattern**: Each skill has `SKILL.md` + optional `references/` subdirectory for domain-specific guides.

---

## 6. Project-Level Settings

All 15 projects have `.claude/settings.local.json`. Current project's:
```json
{
  "permissions": {
    "allow": ["mcp__plugin_serena_serena__list_dir", "WebSearch", "Bash(echo EXIT:$?:*)"]
  }
}
```
Used for per-project permission allowlists (tool-specific).

---

## 7. Architecture Summary

```
~/.claude/
├── CLAUDE.md                    # Global agent rules (39 lines)
├── settings.json                # Plugins, hooks, MCP, env
├── commands/                    # 3 global slash commands
├── skills/learned/              # Empty (no global skills)
├── plugins/                     # 14 installed, 8 enabled
│   ├── installed_plugins.json
│   ├── known_marketplaces.json  # 2 marketplaces
│   └── blocklist.json           # 2 blocked plugins
└── [per-project]
    └── .claude/
        ├── settings.local.json  # Per-project permissions
        ├── memory/              # Agent output storage
        ├── rules/               # Auto-loaded behavioral rules
        ├── skills/              # On-demand skill definitions
        └── commands/            # Project slash commands
```

### Design Philosophy
1. **Separation of concerns**: Global config = behavior/tooling; Project config = domain knowledge
2. **Memory as first-class**: Agent output persists in structured memory files, not inline in CLAUDE.md
3. **Progressive complexity**: Simple projects get just CLAUDE.md + settings; complex ones get full skills/rules/memory
4. **Enforcement via hooks**: SessionStart validates CLAUDE.md size automatically
5. **Bilingual**: EN primary, ZH for Chinese research/grant contexts
6. **Search chain**: Metaso (Chinese AI) → Bailian → Default — optimized for bilingual research

### Maturity Levels Observed
- **Level 3 (Full)**: proposal_x — index CLAUDE.md + memory + skills + rules + commands
- **Level 2 (Structured)**: ai-engineering-kb — index CLAUDE.md + memory (no skills/rules)
- **Level 1 (Monolithic)**: memo-recall, aichat2md — all-in-one CLAUDE.md (legacy)
- **Level 0 (Minimal)**: Some projects with just CLAUDE.md + settings.local.json
