---
name: practical-guide-cc
description: "Claude Code specific implementation - CLAUDE.md, skills, memory, hooks, permissions, MCP"
survey_date: "2026-03-27"
lang: "en/zh"
---

# Claude Code 实战指南

> Claude Code 特定实现。通用原则见 `practical-guide-universal.md`。
> Harness 模式：`MODEL + LOOP + TOOLS + MEMORY + PERMISSIONS + HOOKS`

---

## 1. 快速配置

### 最小 CLAUDE.md (≤30 lines)

```markdown
# CLAUDE.md
## Description
[1-2 sentences about project]

## Structure
| Path | Purpose |
|------|---------|
| src/ | Main code |
| tests/ | Test files |

## Conventions
- TypeScript strict, no any
- Run tests before commit

## Memory Index
| File | Description |
|------|-------------|
| .claude/memory/topic.md | [what] |
```

### 必装 MCP (Solo Dev)

| Purpose | Server |
|---------|--------|
| Documentation | Context7 |
| Code intelligence | Serena |
| Web research | Built-in WebSearch |

---

## 2. Context Loading 成本

| Layer | Loading | Budget | Cost |
|-------|---------|--------|------|
| CLAUDE.md | **Always** | ≤30 lines | Every message |
| @imports | **Always** (expanded) | Sparse | Inlined on load |
| .claude/rules/*.md | **Conditional** | path_patterns | Per-directory |
| .claude/skills/ | **On-demand** | ~2% for desc | Description match |
| .claude/memory/ | **On-demand** | Unlimited | Agent reads |

---

## 3. 文件放置决策

```
每次对话都需要？ ──yes──→ CLAUDE.md（≤30行）
      │no
      ↓
只在特定路径下需要？ ──yes──→ .claude/rules/*.md (path_patterns)
      │no
      ↓
大段 + 不常用 + 自成一体？ ──yes──→ .claude/skills/*.md
      │no
      ↓
.claude/memory/*.md（默认安全）
```

### Rules 示例

```yaml
# .claude/rules/frontend.md
---
path_patterns:
  - "src/components/**"
  - "src/pages/**"
---

- Use functional components
- Follow existing patterns
```

---

## 4. Permission Modes

| Mode | Behavior | Use Case |
|------|----------|----------|
| `default` | Prompt first use | Daily dev |
| `plan` | Read-only | Planning |
| `acceptEdits` | Auto-accept edits | Fast iteration |
| `bypassPermissions` | Skip (except .git/.claude) | Trusted env |
| `auto` | Full autonomy w/ boundaries | Production |

### 配置

```json
// ~/.claude/settings.json
{
  "permissions": {
    "allow": ["Read(**)", "Edit(src/**)", "Bash(npm run *)"],
    "deny": ["Bash(rm -rf /*)", "Read(.env)"]
  }
}
```

---

## 5. Hooks

| Hook | Block? | Use Case |
|------|--------|----------|
| `PreToolUse` | ✓ | Block dangerous commands |
| `PostToolUse` | ✗ | Auto-lint after edits |
| `UserPromptSubmit` | ✓ | Validate input |
| `Stop` | ✓ | Quality gates |
| `SessionStart` | ✗ | Load context |
| `SessionEnd` | ✗ | Cleanup |

### Hook 配置

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{ "type": "command", "command": "check-danger.sh" }]
    }],
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{ "type": "command", "command": "npm run lint --fix" }]
    }]
  }
}
```

### Blocking Script

```bash
#!/bin/bash
if echo "$TOOL_INPUT" | grep -q "rm -rf /*"; then
  echo '{"hookSpecificOutput":{"permissionDecision":"deny"}}'
  exit 2
fi
```

---

## 6. Skills

```markdown
# .claude/skills/commit.md
---
description: Create git commits with conventional commit messages
---

1. Run git status and git diff
2. Draft conventional commit message
3. Add relevant files only
4. Create commit
```

**Best practice:** Excellent description = better matching.

---

## 7. Memory 系统

| Type | Purpose |
|------|---------|
| `user` | User profile/preferences |
| `feedback` | Guidance on approach |
| `project` | Ongoing work context |
| `reference` | External system pointers |

### 格式

```markdown
---
name: [name]
description: [one-line]
type: [user|feedback|project|reference]
---

[Content]

**Why:** [reason]
**How to apply:** [when/where]
```

---

## 8. Session 卫生

| Practice | Command |
|----------|---------|
| Clear between tasks | `/clear` |
| Compact with focus | `/compact Focus on [task]` |
| Time-box | 30-min sprints |
| Audit memory | Every 2 weeks |

### 反模式

| Anti-pattern | Fix |
|--------------|-----|
| Everything in CLAUDE.md | Index only |
| Too many MCP | Only needed |
| Wait for 95% compact | Compact at 70% |
| No /clear between tasks | Clear at boundaries |

---

## 9. Control Plane 检查

```
□ Budget check → 费用阈值？
□ Scope check → 允许路径/域名？
□ Confidence check → 置信度够高？
□ Rate limit → 未超配额？
□ Audit log → 已记录？
```

---

## 10. 关键数字

| Metric | Value |
|--------|-------|
| CLAUDE.md budget | ≤30 lines |
| Proactive compact | 70% |
| Memory audit | Every 2 weeks |
| Tools per domain | 7±2 |
| Session sprint | 30 min |

---

## 11. 常用命令

| Command | Purpose |
|---------|---------|
| `/clear` | Clear context |
| `/compact` | Compress context |
| `/compact Focus on X` | Focused compression |
| `/permissions` | View permissions |
| `/hooks` | View hooks |

---

> **Source:** Claude Code Docs, AI Engineering Frontier KB
>
> Last updated: 2026-03-27
