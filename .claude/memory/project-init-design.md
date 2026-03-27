# project-init Skill — Design Decisions

> Date: 2026-03-07. Record of design choices for future iteration.

## Final Design

- **Location**: `~/.claude/skills/project-init/SKILL.md` (global)
- **Trigger**: `/project-init` or natural language "项目初始化"

## Interactive Flow

1. Safety check: detect existing CLAUDE.md → warn before overwrite
2. Q1: Project type — research / engineering / docs
3. Q2: Complexity — L2 (standard) / L3 (full)
4. Q3: One-line project description (free text)

## What Gets Generated

| Level | Files |
|-------|-------|
| L2 | `CLAUDE.md` (slim: Description + Memory Index + Conventions) + `.claude/memory/` |
| L3 | L2 + `.claude/rules/` + `.claude/skills/` + `.claude/commands/` |

## Key Decisions (with rationale)

| Decision | Choice | Why |
|----------|--------|-----|
| CLAUDE.md format | Fixed index template ≤30 lines | User standard: index-only, knowledge in memory files |
| L2 template sections | Description + Memory Index + Conventions | No Skills/Rules tables until L3 (cleaner) |
| L3 adds | Skills table + Rules table in CLAUDE.md | Matches proposal_x pattern |
| settings.local.json | NOT generated | User switched to global full-auto permissions |
| Hooks | NOT touched | Global SessionStart hook already checks CLAUDE.md size |
| Old projects | NOT migrated | Separate concern, avoid scope creep |
| Type-specific conventions | research: YAML+bilingual; engineering: tests+lint; docs: YAML+structure | Minimal, non-obvious rules only |

## Upgrade Ideas (not yet implemented)

- Add `mixed` project type (research + engineering)
- Optional `.mcp.json` generation for project-specific MCP servers
- L2→L3 upgrade command (add missing directories + update CLAUDE.md template)
- Integration with `/init` (run /init first, then slim down with project-init)
- Template for `.claude/rules/` starter files per project type
- Detect monorepo and suggest subdirectory CLAUDE.md files

## Related Memory Files

- `cc-init-research.md` — Best practices research (353 lines)
- `setup-analysis.md` — Full user config ecosystem analysis (189 lines)
- `claude-code-skill-discovery.md` — Skill/tool search mechanism
