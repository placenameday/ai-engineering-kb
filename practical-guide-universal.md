---
name: practical-guide-universal
description: "Universal harness engineering guide with tool mapping for Cursor, Windsurf, Claude Code, and SDK development"
survey_date: "2026-03-27"
lang: "en/zh"
---

# Harness Engineering 实战指南 (通用版)

> 适用任何 AI 编程工具。2026 范式：Context ⊂ Harness。

---

## 0. 核心原则

| # | Rule | Why |
|---|------|-----|
| 1 | **Harness > Context** | Context = model sees; Harness = system behaves |
| 2 | **Focused 300 > unfocused 113K** | Quality beats quantity |
| 3 | **Workflow first** | Can draw flowchart → Workflow |
| 4 | **Own control flow** | Never let framework hide execution |
| 5 | **Evals before features** | No evals = guessing |
| 6 | **Deterministic guardrails** | Agent never touches outside directly |

---

## 1. Harness 七组件

```
PERMISSIONS → HOOKS → TOOLS → GUARDRAILS → STATE → ISOLATION → FEEDBACK
```

| Component | Purpose | Key Question |
|-----------|---------|--------------|
| Permissions | Access control | "Can this action run?" |
| Hooks | Lifecycle intervention | "When to intercept?" |
| Tools | External capabilities | "What can it do?" |
| Guardrails | Control plane | "What gates must pass?" |
| State | Persistence | "How to save/restore?" |
| Isolation | Sandboxing | "Where can it operate?" |
| Feedback | Quality validation | "How to verify output?" |

---

## 2. 工具映射表 (Tool Mapping)

### 2.1 Harness 组件对照

| Component | Claude Code | Cursor | Windsurf | SDK Dev |
|-----------|-------------|--------|----------|---------|
| **Project Config** | CLAUDE.md | .cursorrules | .windsurfrules | System prompt |
| **Rules** | .claude/rules/*.md | .cursor/rules | cascade/memories | Code logic |
| **Memory** | .claude/memory/ | (none built-in) | cascade/memories | DB/Vector store |
| **Skills** | .claude/skills/*.md | .cursor/commands | (none) | Function definitions |
| **Permissions** | settings.json | (none) | (none) | Custom middleware |
| **Hooks** | hooks in settings | (none) | (none) | Lifecycle callbacks |
| **MCP** | Native support | (none) | (none) | MCP client lib |

### 2.2 配置文件位置

| Tool | Config File | Purpose |
|------|-------------|---------|
| Claude Code | `~/.claude/settings.json` | Global permissions/hooks |
| Claude Code | `.claude/settings.local.json` | Project permissions |
| Claude Code | `CLAUDE.md` | Project context (≤30 lines) |
| Cursor | `.cursorrules` | Project rules (≤500 lines rec) |
| Cursor | `.cursor/rules/` | Directory-specific rules |
| Windsurf | `.windsurfrules` | Project rules |
| Windsurf | `cascade/memories/` | Persistent memory |

### 2.3 特性对比

| Feature | Claude Code | Cursor | Windsurf |
|---------|-------------|--------|----------|
| Agent mode | ✓ (agentic loop) | ✓ (Composer) | ✓ (Cascade) |
| Background agents | ✓ | ✗ | ✓ |
| Multi-file edit | ✓ | ✓ | ✓ |
| Shell access | ✓ | Limited | ✓ |
| MCP support | ✓ Native | ✗ | ✗ |
| Permission modes | 5 modes | ✗ | ✗ |
| Hooks | 7 event types | ✗ | ✗ |
| Worktree isolation | ✓ | ✗ | ✗ |

---

## 3. 配置模板

### 3.1 CLAUDE.md (Claude Code)

```markdown
# CLAUDE.md
## Description
[1-2 sentences]

## Structure
| Path | Purpose |
|------|---------|

## Conventions
- [Specific rule 1]
- [Specific rule 2]
```

### 3.2 .cursorrules (Cursor)

```markdown
# Project Rules

## Tech Stack
- TypeScript, React, Node.js

## Conventions
- Use functional components
- Run `npm test` before commit
- No `any` types

## Code Style
- Prettier for formatting
- ESLint for linting
```

### 3.3 .windsurfrules (Windsurf)

```markdown
# Project Context

## Overview
[What this project does]

## Guidelines
- Follow existing patterns
- Write tests for new features
- Document public APIs
```

---

## 4. Context 决策树

```
这段知识放哪？

每次对话都需要？ ──yes──→ 项目配置文件 (CLAUDE.md/.cursorrules)
      │no
      ↓
只在特定目录需要？ ──yes──→ 目录规则 (.claude/rules / .cursor/rules)
      │no
      ↓
大段且不常用？ ──yes──→ 独立文档/Skill
      │no
      ↓
参考/累积知识？ ──yes──→ Memory/外部存储
```

---

## 5. Control Plane 架构

> Control Plane 是 Harness 的核心确定性组件。架构设计见 [architecture-guide.md §6.2](architecture-guide.md)。

**核心原则：** Agent 永远不直接接触外部世界，所有操作通过 Control Plane。

| Guardrail | Check | CC Config | SDK Config |
|-----------|-------|-----------|------------|
| Budget | 费用阈值 | Hooks/Stop | `budget_limit` |
| Scope | 路径白名单 | Permissions | `scope_patterns` |
| Confidence | 置信度 | — | `confidence_threshold` |
| Rate Limit | 调用频率 | — | `rate_limiter` |
| Audit | 全量记录 | Hooks/PostToolUse | `audit_log` |

SDK 代码实现：[practical-guide-oc.md §3.2](practical-guide-oc.md)

---

## 6. Progressive Autonomy

| Phase | Mode | AI Does | Human Does | Entry Criteria |
|-------|------|---------|------------|----------------|
| 1 | Advisory | Suggests only | All decisions | Initial |
| 2 | Supervised | Executes w/ confirmation | Approve/reject | Low-risk proven |
| 3 | Constrained | Autonomous in boundaries | Exceptions | Eval-validated |

---

## 7. 关键数字

> CC 特定数字见 [practical-guide-cc.md §10](practical-guide-cc.md)。
> 优化指标见 [optimization-guide.md §10](optimization-guide.md)。

| Metric | Value | Action |
|--------|-------|--------|
| Context rot threshold | ~40% window | Compress before |
| Proactive compact | 70% | Don't wait for 95% |
| Tools per domain | 7±2 | Bounded context |
| CLAUDE.md budget | ≤30 lines | Index only |
| .cursorrules budget | ≤500 lines | Recommended |
| Focused 300 vs unfocused 113K | 300 wins | Quality >> Quantity |

---

## 8. 工具选择指南

```
需要最多控制力？ ──yes──→ SDK 开发 (自定义 harness)
      │no
      ↓
需要 MCP/后台 agent？ ──yes──→ Claude Code
      │no
      ↓
主要在 VSCode？ ──yes──→ Cursor
      │no
      ↓
需要 cascade memory？ ──yes──→ Windsurf
      │no
      ↓
任选主流工具，遵循通用原则
```

---

> **Source:** AI Engineering Frontier KB, Claude Code Docs, Cursor Docs, Windsurf Docs
>
> Last updated: 2026-03-27
