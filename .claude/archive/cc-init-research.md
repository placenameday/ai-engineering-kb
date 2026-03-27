# Claude Code Project Initialization — Best Practices Research

> Research date: 2026-03-07. Comprehensive survey of official docs, community patterns, and cross-tool comparisons.

---

## 1. Official Docs: CLAUDE.md & Project Setup

### The `/init` Command
- Run `/init` to auto-generate a starter `CLAUDE.md` by analyzing your codebase (build systems, test frameworks, code patterns).
- Detects project type from build files (package.json, .csproj, pom.xml, etc.), primary language/framework, directory structure, existing documentation.
- Claude's inner tools (BatchTool & GlobTool) collect package*.json, *.md, .cursor/rules/**, .github/copilot-instructions.md as context.
- Treat `/init` output as a starting point — refine iteratively based on what Claude gets wrong.
- Source: [Best Practices - Claude Code Docs](https://code.claude.com/docs/en/best-practices)

### CLAUDE.md Content Guidelines (Official)

**Include:**
| Category | Examples |
|----------|----------|
| Bash commands Claude can't guess | `npm run test`, `npm run build` |
| Code style rules differing from defaults | "Use ES modules, not CommonJS" |
| Testing instructions & preferred runners | "Use React Testing Library, avoid mocks" |
| Repo etiquette | Branch naming, PR conventions |
| Architectural decisions | "State management via Zustand in src/stores" |
| Dev environment quirks | Required env vars, gotchas |

**Exclude:**
- Anything Claude can figure out by reading code
- Standard language conventions Claude already knows
- Detailed API docs (link instead)
- Frequently changing information
- File-by-file codebase descriptions
- Self-evident practices ("write clean code")

### Size & Format
- Keep root CLAUDE.md to **50-100 lines** (max ~200 lines).
- Each line: ask "Would removing this cause Claude to make mistakes?" If not, cut it.
- No required format — keep it short and human-readable.
- Use `@imports` for detailed sections: `@docs/git-instructions.md`.
- ~100 lines ≈ 4,000 tokens ≈ 2% of Opus context (200K).
- Emphasis markers ("IMPORTANT", "YOU MUST") improve adherence for critical rules.
- Source: [Best Practices - Claude Code Docs](https://code.claude.com/docs/en/best-practices), [Writing a good CLAUDE.md | HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md)

### File Hierarchy
```
~/.claude/CLAUDE.md          → Global (all sessions)
./CLAUDE.md                  → Project root (git-tracked, team-shared)
./CLAUDE.local.md            → Project root (gitignored, personal)
./subdir/CLAUDE.md           → Child (loaded on-demand when working in subdir)
../CLAUDE.md                 → Parent (monorepo root)
```
Claude loads hierarchically: root rules apply everywhere, child rules apply contextually.

---

## 2. Complete Project Directory Structure

### Canonical `.claude/` Layout (Community Consensus)
```
project-root/
├── CLAUDE.md                    # Primary instructions (git-tracked)
├── CLAUDE.local.md              # Personal overrides (gitignored)
├── .mcp.json                    # MCP server config (git-tracked)
├── .claude/
│   ├── settings.json            # Shared settings: permissions, hooks (git-tracked)
│   ├── settings.local.json      # Personal settings (gitignored)
│   ├── commands/                # Custom slash commands (*.md)
│   ├── skills/                  # Domain knowledge modules
│   │   └── <skill-name>/
│   │       └── SKILL.md         # YAML frontmatter + instructions
│   ├── agents/                  # Subagent definitions (*.md)
│   ├── rules/                   # Modular instruction files (*.md)
│   │   ├── code-style.md
│   │   ├── testing.md
│   │   └── security.md
│   ├── hooks/                   # Hook automation scripts
│   └── memory/                  # Cross-session memory files
└── .github/workflows/           # CI/CD (optional)
```
Source: [claude-code-showcase](https://github.com/ChrisWiles/claude-code-showcase), [Trail of Bits config](https://github.com/trailofbits/claude-code-config)

---

## 3. Hooks: Deterministic Quality Guarantees

### Available Hook Events
| Event | When | Use Case |
|-------|------|----------|
| `PreToolUse` | Before a tool runs | Block risky ops, validate inputs, protect files |
| `PostToolUse` | After a tool succeeds | Auto-format, run lint, audit logging |
| `SessionStart` | Session begins | Inject context, load recent tickets, reminders |
| `UserPromptSubmit` | User submits prompt | Add context, suggest skills |
| `Notification` | Claude sends alert | Desktop notifications |
| `Stop` | Agent finishes response | Continuation checks, rationalization guards |
| `ConfigChange` | Config file modified | Compliance logging |

### Exit Code Control Flow
- **Exit 0**: Action proceeds
- **Exit 2**: Action BLOCKED (stderr sent to Claude as feedback)
- **Other**: Action proceeds (stderr logged, not shown to Claude)

### Key Patterns
1. **Auto-format after edits**: PostToolUse → run prettier/gofmt on changed files
2. **Protect sensitive files**: PreToolUse → block writes to `.env`, `.git/`, production configs
3. **Branch protection**: PreToolUse → block commits to main/master
4. **Session context injection**: SessionStart → load `git log --oneline -5`, recent tickets
5. **Skill suggestion**: UserPromptSubmit → analyze prompt keywords, suggest relevant skills
6. **Desktop notifications**: Notification → terminal-notifier/osascript when input needed
7. **Audit trail**: PostToolUse → log all mutations for post-session review

### Configuration Location
- Global: `~/.claude/settings.json`
- Project (shared): `.claude/settings.json`
- Project (personal): `.claude/settings.local.json`

### Security
- Hooks run with user credentials — review scripts carefully.
- Claude snapshots hooks at session start; changes require `/hooks` review.
- Quote variables, validate inputs, use absolute paths.

Sources: [Hooks Guide - Claude Code Docs](https://code.claude.com/docs/en/hooks-guide), [Demystifying Claude Code Hooks](https://www.brethorsting.com/blog/2025/08/demystifying-claude-code-hooks/), [eesel.ai hooks reference](https://www.eesel.ai/blog/hooks-reference-claude-code)

---

## 4. MCP Servers: Common Per-Project Configurations

### Most Popular (by usage frequency)
| Server | Purpose | Scope |
|--------|---------|-------|
| **GitHub** | PRs, issues, CI/CD, code search | Universal |
| **Context7** | Up-to-date library docs & examples | Universal |
| **Playwright** | Browser automation, UI testing | Web projects |
| **PostgreSQL** | Natural language DB queries | Backend |
| **Filesystem** | Secure file operations | Universal |
| **Sequential Thinking** | Structured problem-solving | Complex tasks |
| **Memory** | Cross-session context persistence | Universal |
| **Figma** | Design spec extraction | Frontend |
| **Notion/Jira/Linear** | Project management integration | Team projects |
| **Docker** | Container management | DevOps |
| **Firecrawl** | Web scraping to LLM-ready data | Research |

### Configuration Locations
- **Project scope**: `.mcp.json` at project root (git-tracked, team-shared)
- **Local scope**: `~/.claude.json` under project path (personal)
- **User scope**: `~/.claude.json` (cross-project)
- **NOT** in `.claude/settings.json` (known issue — MCP config ignored there)

### Env Var Expansion
`.mcp.json` supports environment variable expansion for API keys — teams share config, individuals set env vars.

### Lazy Loading
Claude Code's MCP Tool Search enables lazy loading, reducing context usage by up to 95%. Safe to configure many servers without context bloat.

Source: [MCP - Claude Code Docs](https://code.claude.com/docs/en/mcp), [Best MCP Servers](https://mcpcat.io/guides/best-mcp-servers-for-claude-code/)

---

## 5. `.claude/settings.json` for Teams

### Shared Settings (git-tracked `.claude/settings.json`)
```json
{
  "permissions": {
    "allow": ["Read(./src/**)", "Bash(git *)", "Bash(npm run lint)"],
    "deny": ["Read(.env*)", "Bash(curl *)", "Bash(wget *)"]
  },
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{ "type": "command", "command": "prettier --write $CLAUDE_FILE_PATH" }]
    }]
  }
}
```

### Permission Evaluation Order
1. **Deny** (always wins)
2. **Ask** (prompts user)
3. **Allow** (proceeds silently)

### Scope Precedence (highest → lowest)
1. Enterprise managed policies (`managed-settings.json`)
2. Command line arguments
3. Local project settings (`.claude/settings.local.json`)
4. Shared project settings (`.claude/settings.json`)

### Array Merging
Array settings (permissions.allow, permissions.deny) **merge across scopes** — concatenated and deduplicated, not replaced.

### Best Practices
- Deny everything risky by default, explicitly allow safe commands
- Place personal paths/tokens in `.claude/settings.local.json`
- Environment variables can be configured in settings.json for team-wide env setup
- Use hooks in shared settings for team-wide automation (formatting, linting)

Source: [Settings - Claude Code Docs](https://code.claude.com/docs/en/settings), [eesel.ai settings guide](https://www.eesel.ai/blog/settings-json-claude-code)

---

## 6. Custom Commands & Skills

### Commands (`.claude/commands/*.md`)
- File name → slash command name
- Simple markdown with natural language instructions
- `$ARGUMENTS` placeholder for arguments; `$1`, `$2` for positional args
- Example: `.claude/commands/fix-issue.md` → `/fix-issue 1234`

### Skills (`.claude/skills/<name>/SKILL.md`) — Newer Format
- Superset of commands: includes frontmatter metadata, supporting files
- `name` field → slash command; `description` → auto-discovery by Claude
- `disable-model-invocation: true` for workflows with side effects (manual-only)
- Skills auto-activate when relevant; commands are always manual

### Community Repos
- [wshobson/commands](https://github.com/wshobson/commands): 57 production-ready slash commands
- [qdhenry/Claude-Command-Suite](https://github.com/qdhenry/Claude-Command-Suite): 216+ commands, 12 skills
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code): Curated community list

### `.claude/rules/` — Modular Instructions (v2.0.64+)
- Auto-discovered `.md` files in `.claude/rules/` (including subdirectories)
- Same priority as CLAUDE.md, but can be **path-scoped** via frontmatter
- Solves the "priority saturation" problem — rules activate only where relevant
- Reduces merge conflicts (multiple files vs. one monolithic CLAUDE.md)
- Source: [Claude Code Rules Directory](https://claudefa.st/blog/guide/mechanics/rules-directory)

---

## 7. Memory Files Across Sessions

### Three Memory Systems
| System | Location | How it works |
|--------|----------|-------------|
| **CLAUDE.md** (manual) | Project root / `~/.claude/` | Author-curated, loaded every session |
| **Auto Memory** | `~/.claude/projects/<project>/memory/` | Claude self-writes based on corrections/preferences |
| **Session Memory** | `~/.claude/projects/<hash>/<session>/session-memory/summary.md` | Background summaries every ~5K tokens |

### Cross-Session Pattern (Agent Workflows)
1. **Initializer session**: Set up progress log, feature checklist, startup references
2. **Subsequent sessions**: Open by reading memory artifacts, pick up where left off
3. **`/remember`**: Promotes recurring patterns from session memory to permanent CLAUDE.md config

### Best Practices
- CLAUDE.md survives `/compact` (re-read from disk); conversation-only instructions don't
- Manually prune auto memory after ~10 sessions (30%+ becomes redundant)
- Total memory files should stay under 10,000 tokens
- Use structured memory files for agent coordination (this project's pattern: `.claude/memory/<topic>.md`)

### Third-Party Extensions
- [memory-mcp](https://github.com/yuvalsuede/memory-mcp): Two-tier memory with Haiku-powered consolidation
- [claude-mem](https://github.com/thedotmack/claude-mem): Auto-capture + AI compression for session context

Source: [Memory - Claude Code Docs](https://code.claude.com/docs/en/memory), [Persistent Memory Guide](https://agentnativedev.medium.com/persistent-memory-for-claude-code-never-lose-context-setup-guide-2cb6c7f92c58)

---

## 8. The "12-Factor" Connection

The user's KB concept [12-Factor Agents](/concepts/12-factor-agents.md) maps directly to Claude Code project setup:

| 12-Factor Principle | Claude Code Feature |
|---------------------|-------------------|
| F2: Own Your Prompts | CLAUDE.md + skills (no framework abstraction) |
| F3: Tools = Structured Outputs | MCP servers + tool definitions |
| F4: Own the Control Flow | Hooks (deterministic) vs. CLAUDE.md (advisory) |
| F5: Own Your Context Window | CLAUDE.md curation, `/compact`, subagents |
| F6: Launch/Pause/Resume | Checkpoints, `--continue`, `--resume` |
| F8: Compact Errors | Auto-compaction, error context injection |
| F9: Small Focused Agents | `.claude/agents/` subagent definitions |
| F10: Trigger from Anywhere | `claude -p` non-interactive mode, CI/CD integration |
| F11: Unify Execution & Business State | Progress files, feature checklists in memory |
| F12: Pre-fetch Context | SessionStart hooks, `@imports`, rules with path scoping |

**Key insight from Dex Horthy**: Context degrades past ~40% utilization. Claude Code's architecture (subagents, `/compact`, `/clear`, memory files) directly addresses this "dumb zone" problem.

Source: [12 Factor Agents](https://paddo.dev/blog/12-factor-agents/), [humanlayer/12-factor-agents CLAUDE.md](https://github.com/humanlayer/12-factor-agents/blob/main/CLAUDE.md)

---

## 9. Cross-Tool Comparison

### Project-Level AI Config Files
| Tool | Config File | Location | Unique Feature |
|------|------------|----------|----------------|
| **Claude Code** | `CLAUDE.md` | Project root | `@import` syntax, hierarchical loading, rules/ directory |
| **Cursor** | `.cursorrules` / `.mdc` | Project root | MDC frontmatter, Notepads for reusable contexts |
| **Windsurf** | `.windsurfrules` | Project root | Cascade Memories (AI-generated session notes) |
| **GitHub Copilot** | `copilot-instructions.md` | `.github/` | VS Code native integration |
| **Cline** | `.clinerules` | Project root | — |
| **Google Gemini** | `GEMINI.md` | Project root | — |
| **OpenAI Agents** | `AGENTS.md` | Project root | — |

### Content Overlap
90%+ of content is identical across formats. Core content: project description, tech stack, code style, testing instructions, architecture decisions.

### Multi-Tool Sync Strategy
Use a pre-commit hook to copy a single source file (e.g., `docs/ai-rules-base.md`) to all config locations. Tool-specific additions (like `@import` or MDC frontmatter) live only in the relevant file.

### What Claude Code Does Better
- **Terminal-native**: Works over SSH, in CI/CD, with any editor
- **Hierarchical config**: Parent/child CLAUDE.md files for monorepos
- **Deterministic hooks**: Guaranteed actions vs. advisory instructions
- **Subagents**: Isolated context for investigation/review
- **Skills ecosystem**: Reusable, auto-discovered domain knowledge

### What Claude Code Can Learn From Others
- **Cursor's Notepads**: Reusable context bundles that can be attached to specific conversations
- **Windsurf's Cascade Memories**: Automatic AI-generated session notes (Claude has this via auto memory, but Windsurf's is more visible)
- **Copilot's VS Code integration**: Tighter editor integration (Claude Code addresses this with the desktop app and plugins)

Source: [Agent Rules Builder comparison](https://www.agentrulegen.com/guides/cursorrules-vs-claude-md), [DEV Community comparison](https://dev.to/pockit_tools/cursor-vs-windsurf-vs-claude-code-in-2026-the-honest-comparison-after-using-all-three-3gof)

---

## 10. Trail of Bits Reference Config (Advanced Example)

The [trailofbits/claude-code-config](https://github.com/trailofbits/claude-code-config) repo provides an opinionated production config:

### Global CLAUDE.md Philosophy
- No speculative features, no premature abstraction
- Replace don't deprecate
- Hard limits: function length, complexity, line width
- Language-specific toolchains: Python/uv, Node/oxlint, Rust/clippy

### Sandboxing Strategy
- Enable `/sandbox` for filesystem isolation
- Combine with permission deny rules blocking `~/.ssh`, `~/.aws`, etc.
- Use devcontainers for complete host isolation

### Hook Implementation
- PreToolUse: Block `rm -rf`, suggest safer alternatives
- PostToolUse: Audit logging, mutation tracking
- Stop: Anti-rationalization checks (prevent premature completion)

### Status Line
Custom `statusline.sh` showing real-time context usage, cost, and cache hit rate.

---

## Summary: Minimum Viable Claude Code Project

For a new project, the minimum setup is:

1. `git init` (Claude is git-aware, creates checkpoints)
2. Run `/init` to generate starter `CLAUDE.md`
3. Prune CLAUDE.md to <100 lines of what Claude truly needs
4. Create `.claude/settings.json` with team permissions + key hooks
5. Create `.mcp.json` if team uses shared MCP servers
6. Add `.claude/commands/` or `.claude/skills/` for repeated workflows
7. Add `.claude/rules/` for domain-specific modular instructions
8. Iterate: when Claude makes mistakes, add rules; when rules are ignored, prune or restructure

The advanced setup adds subagents, memory patterns, SessionStart hooks, and CI integration.
