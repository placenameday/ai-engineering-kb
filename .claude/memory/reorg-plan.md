# Knowledge Base Reorganization Plan

## Goal
Reorganize 4 flat markdown files into a topic-based directory structure that scales to ~30 articles, supports incremental updates, and maintains bilingual (EN/ZH) content.

## Design Decisions

**Why topic-based dirs (not type-based):** The content clusters naturally into domains (concepts, tools, practices). A user looking up "guardrails" wants to find it by topic, not by "surveys" vs "deep-dives." Topic dirs also allow each area to grow independently.

**Why split the monolithic frontier survey:** The 20KB file covers 13 independent concepts. Splitting enables: (1) per-concept updates without touching other sections, (2) granular git diffs, (3) linking from other articles, (4) independent growth of each concept. The concept map and thinkers table become a standalone index/overview file.

**Why NOT over-nest:** With ~4-30 files, two levels max (category/article) is sufficient. Deeper nesting adds friction.

## Target Directory Structure

```
ai-engineering-kb/
├── CLAUDE.md                          # Index only (updated)
├── README.md                          # Human-facing nav + concept map (NEW)
├── concepts/                          # Core AI engineering concepts (split from frontier survey)
│   ├── _index.md                      # Concept map + thinkers table (extracted)
│   ├── context-engineering.md         # MOVED from root (existing deep-dive, merged with survey sec 1)
│   ├── software-3.0.md               # Split from survey sec 2
│   ├── agentic-engineering.md         # Split from survey sec 3 (vibe coding → agentic eng)
│   ├── agentic-design-patterns.md    # Split from survey sec 4
│   ├── compound-ai-systems.md        # Split from survey sec 5
│   ├── 12-factor-agents.md           # Split from survey sec 6
│   ├── eval-driven-development.md    # Split from survey sec 7
│   ├── spec-driven-development.md    # Split from survey sec 8
│   ├── cognitive-architectures.md    # Split from survey sec 9
│   ├── anthropic-agent-design.md     # Split from survey sec 10
│   ├── agentic-patterns-willison.md  # Split from survey sec 11
│   ├── guardrails-progressive-autonomy.md  # Split from survey sec 12
│   └── llm-orchestration.md          # Split from survey sec 13
├── tooling/                           # Tools, frameworks, infrastructure
│   ├── research-tools-survey.md       # MOVED from root (ZH, unchanged)
│   └── skills-ecosystem.md           # MOVED from root (unchanged)
└── .claude/
    ├── memory/
    │   └── reorg-plan.md             # This file
    └── settings.local.json
```

## Naming Conventions
- Dirs: lowercase, plural nouns (`concepts/`, `tooling/`)
- Files: lowercase, kebab-case, descriptive (`eval-driven-development.md`)
- `_index.md` for directory-level overview/navigation (underscore prefix = meta-file)
- YAML frontmatter on all articles: `name`, `description`, `date`, `lang` (en/zh), `tags`

## Frontmatter Standard (NEW)

```yaml
---
name: eval-driven-development
description: >
  One-line description for search and CLAUDE.md index.
date: 2026-03-05
lang: en
tags: [methodology, evaluation, quality]
sources:
  - https://arxiv.org/html/2411.13768v3
---
```

## Step-by-Step Execution Plan

### Step 1 — Rex: Create directory structure
- `mkdir -p concepts/ tooling/`
- Produces: empty dirs
- Verify: `ls -R` shows structure

### Step 2 — Rex: Split `ai-engineering-frontier-2025.md` into 13 concept files
For each section (1-13):
- Extract section content into individual file under `concepts/`
- Add YAML frontmatter with name, description, date (2026-03-05), lang (en), tags
- Keep all source links with each concept
- File names mapped above (section number → file name)

Special handling:
- **Section 1 (Context Engineering):** Merge with existing `context-engineering.md`. The survey section becomes a brief overview at the top; the existing deep-dive content follows. Avoid duplication.
- **Concept Map + Key Thinkers table:** Extract to `concepts/_index.md`

- Produces: 13 concept files + `_index.md`
- Verify: `wc -l concepts/*.md` shows reasonable distribution; no empty files; all source links preserved

### Step 3 — Rex: Move tooling files
- `mv research-tools-survey.md tooling/`
- `mv skills-ecosystem.md tooling/`
- Produces: files in new locations
- Verify: `ls tooling/` shows both files

### Step 4 — Rex: Delete the original monolithic file
- `rm ai-engineering-frontier-2025.md`
- `rm context-engineering.md` (merged into concepts/)
- Verify: root dir only has CLAUDE.md and README.md

### Step 5 — Iris: Create `README.md`
Human-facing navigation document:
- Project title and purpose (bilingual)
- Visual concept map (the mermaid or ASCII version from the survey)
- Directory guide with brief descriptions
- Links to all articles organized by category
- Contribution guidelines (how to add new articles: frontmatter, naming, where to put them)

- Produces: `README.md` at project root
- Verify: all internal links resolve; concept map is accurate

### Step 6 — Iris: Update `CLAUDE.md`
Rewrite as pure index following the project convention:
- Update File Inventory table to reflect new paths
- Add directory descriptions
- Update conventions section with new frontmatter standard and naming rules
- Keep it under 30 lines per the CLAUDE.md rules

- Produces: updated `CLAUDE.md`
- Verify: under 30 lines; all paths valid; memory index updated

### Step 7 — Hawk: Review
- Verify no content was lost in the split (diff total line counts)
- Verify all source URLs preserved
- Verify all internal cross-references work
- Verify frontmatter on every file
- Verify CLAUDE.md is accurate index
- Verify README.md links resolve

## Risks

1. **Content loss during split:** Mitigated by Hawk diffing line counts before/after and checking source URLs.
2. **Context engineering merge conflict:** The survey section 1 and deep-dive file overlap. Rex must merge carefully — survey as overview, deep-dive as detail, no duplication.
3. **Broken references:** If any external system references the old flat file paths. Low risk since this is a personal KB with no external consumers.

## Growth Pattern
When adding new content:
- New concept → `concepts/new-concept-name.md` + update `_index.md` concept map + update CLAUDE.md inventory
- New tool/framework survey → `tooling/tool-name.md`
- New category (e.g., `case-studies/`, `papers/`) → create dir when 2+ files justify it, not before

## What NOT to Do
- Don't create empty placeholder directories for future categories
- Don't add a `drafts/` folder — use git branches instead
- Don't create a separate `zh/` directory — bilingual content stays in place with `lang` frontmatter
