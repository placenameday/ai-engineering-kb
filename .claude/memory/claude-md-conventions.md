---
name: CLAUDE.md Conventions
description: KB conventions, workflow rules, and gotchas extracted from CLAUDE.md
type: project
---

## Conventions

- Frontmatter: `name`, `description`, `survey_date`, `lang`
- Naming: `kebab-case.md`; `_index.md` for nav

## Workflow

Multi-step → TeamCreate + background agents. Research → WebSearch → extract → write.

## Gotchas

- Box-drawing chars (┌│├└) are multi-byte UTF-8 — Edit tool may fail to match. Use shorter strings.
- Updating concepts/ often requires updating README.md, _index.md, and .claude/memory/kb-synthesis-*.md
- After editing tables/lists, grep for duplicate rows — duplicates can survive across sessions
- Cross-refs: replace duplicated content with `→ [file.md §section]` links; don't duplicate across files
