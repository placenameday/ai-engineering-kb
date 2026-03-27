# Claude Code Skill Search & Discovery Mechanisms

## Investigation Date: 2026-03-07

## Answer Summary

There are **two separate mechanisms** that solve context pollution in Claude Code. The user is likely conflating them, but both are real and official.

---

## Mechanism 1: Skills Progressive Disclosure (Built-in, Ships with Claude Code)

**How it works — three levels:**
1. **Startup**: Only skill `name` + `description` (from YAML frontmatter) are loaded into context. ~100 tokens per skill.
2. **On-demand**: When Claude determines a skill is relevant, it invokes the `Skill` tool, which loads the full `SKILL.md` body (~2,000 tokens).
3. **References**: The skill body can reference additional files (templates, scripts, examples) that Claude reads only if needed.

**Context budget**: Skill descriptions share a budget of **2% of context window** (fallback: 16,000 chars). Configurable via `SLASH_COMMAND_TOOL_CHAR_BUDGET` env var. If exceeded, skills are silently dropped.

**Key detail**: With 20 skills, ~2,000 tokens at startup instead of ~40,000 if all were fully loaded. The model itself decides relevance based on description text — no algorithmic/embedding-based search.

**Control knobs**:
- `disable-model-invocation: true` — removes skill from Claude's context entirely (user-only)
- `user-invocable: false` — hides from `/` menu but Claude can still load it
- Run `/context` to check if skills are being excluded due to budget

**Official docs**: https://code.claude.com/docs/en/skills

---

## Mechanism 2: MCP ToolSearch / Deferred Tool Loading (For MCP Tools)

**How it works:**
1. Claude Code checks if MCP tool descriptions exceed **10K tokens** (or 10% of context)
2. If threshold exceeded, tools are marked `defer_loading: true`
3. Claude receives a special **ToolSearch** tool instead of all tool definitions
4. When Claude needs a tool, it searches using keywords (regex mode or BM25 mode)
5. Only **3-5 relevant tools** (~3K tokens) are loaded per query

**Results**: 85% reduction in tool token usage. Selection accuracy improved (Opus 4.5: 79.5% → 88.1%).

**Shipped in**: Claude Code v2.1.7+ to all users

**Configure threshold**: `ENABLE_TOOL_SEARCH=auto:30` (raises to 30% of context)

**Official docs**: https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool

---

## Mechanism 3: SkillSearch / AgentSearch (NOT YET IMPLEMENTED)

**Status**: Feature request #19445 was filed to apply the ToolSearch pattern to skills and agents. **Closed as "not planned"** (Feb 28, 2026).

The proposal was:
- `SkillSearch("keyword")` → loads matching skill definitions on demand
- `AgentSearch("keyword")` → loads matching agent definitions on demand
- Would save ~15,000 tokens per session

This does NOT exist yet. The skills system relies on the progressive disclosure mechanism (Mechanism 1) instead.

---

## Key Distinction

| Feature | Skills | MCP Tools |
|---------|--------|-----------|
| Discovery | Description in context, body lazy-loaded | ToolSearch with keyword matching |
| Selection | Claude's reasoning from descriptions | Regex/BM25 search algorithm |
| Budget | 2% of context window | 10% threshold triggers deferral |
| Scale | Dozens (limited by description budget) | Hundreds/thousands |
| Standard | AgentSkills.io open standard | MCP protocol |

## What the User Likely Saw

The user probably read about **ToolSearch for MCP tools** (which does have search/discovery with deferred loading) and assumed or hoped it applied to skills too. Skills have a simpler but effective mechanism: progressive disclosure via frontmatter descriptions + on-demand body loading. There is no "SkillSearch" — the proposal was rejected.

Alternatively, the user may have read about the skills system itself, which IS a form of "register many without polluting context" — just not via search, but via the 2% budget + lazy body loading.

## Sources
- Official Skills docs: https://code.claude.com/docs/en/skills
- Tool Search docs: https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool
- Skills as lazy-loaded context: https://taylordaughtry.com/posts/claude-skills-are-lazy-loaded-context/
- Skills vs MCP comparison: https://lucumr.pocoo.org/2025/12/13/skills-vs-mcp/
- SkillSearch feature request (closed): https://github.com/anthropics/claude-code/issues/19445
- MCP ToolSearch guide: https://www.atcyrus.com/stories/mcp-tool-search-claude-code-context-pollution-guide
- Deferred loading blog: https://paddo.dev/blog/claude-code-hidden-mcp-flag/
- Advanced tool use (Anthropic engineering): https://www.anthropic.com/engineering/advanced-tool-use
