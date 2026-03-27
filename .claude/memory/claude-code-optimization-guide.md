# Claude Code 项目优化指南

**目标项目**: `proposal_x` (AI辅助科研课题申请书写作系统)
**基于审计**: 2026-03-07 完整审计结果
**总规模**: .claude/ 344KB, 9 skills, 5 rules, 12 memory files

---

## 使用说明

本指南可在 `proposal_x` 项目内由任意 Claude Code session 直接执行。每项操作独立，按优先级排列。完成一项后在该项末尾标注 `[DONE YYYY-MM-DD]`。建议单次 session 处理同一优先级的所有项目。

---

## P0 关键修复

> 影响 agent 发现和加载机制，必须立即修复。

### 1. CLAUDE.md 索引缺失项

**问题**: CLAUDE.md 仅列出 4 条 rules（实际 5 条），且 Memory Index 缺少最大的参考文档。Agent 依赖 CLAUDE.md 作为发现入口，遗漏项将不被感知。

**操作**:

(a) 在 CLAUDE.md Rules 表格中添加一行:
```
| memory-discipline.md | `projects/**` | 内存纪律：硬限制3文件/30KB，agent写入边界 |
```

(b) 在 CLAUDE.md Memory Index 表格中添加一行:
```
| kb/system-design-reference.md | 系统架构设计文档（208行），项目 onboarding 参考 |
```

**验证**: 确认 CLAUDE.md 中 Rules 表格有 5 行，Memory Index 涵盖所有 `.claude/memory/` 下的文件。

---

## P1 结构优化

### 2. 拆分 proposal-write skill（330行 → ~200 + ~130）

**问题**: `.claude/skills/proposal-write/SKILL.md` 包含 Phase 2 模块写作 + Phase 3 审稿循环 + 三种累积机制（偏好学习、领域纠错、profile 累积），超出单文件管理阈值。

**操作**:
- 新建 `.claude/skills/proposal-accumulate/SKILL.md`
- 将以下内容从 `proposal-write/SKILL.md` 迁移至新 skill:
  - 偏好学习（preference learning）
  - 领域纠错（domain correction）
  - Profile 累积（profile accumulation）
- 新 skill frontmatter 示例:
```yaml
name: proposal-accumulate
description: 写作过程中的偏好学习、领域纠错与 profile 累积
```
- 在 `proposal-write/SKILL.md` 中添加引用: `累积机制详见 proposal-accumulate skill`
- 更新 CLAUDE.md Skills 表格，添加 `proposal-accumulate` 条目

**验证**: 两个 skill 均 ≤200 行，`/proposal` 命令流程不中断。

### 3. 删除冗余 reviewer-guide.md

**问题**: `.claude/memory/reviewer-guide.md`（17行）与 `.claude/agents/reviewer.md`（194行）内容高度重叠。Agent 文件是权威来源，memory 文件无独有内容。

**操作**:
- 删除 `.claude/memory/reviewer-guide.md`
- 从 CLAUDE.md Memory Index 中移除对应行
- 确认 `agents/reviewer.md` 中包含评审维度和扣分项的完整定义

### 4. 处理 cases/analysis.md 空占位符

**问题**: `.claude/memory/cases/analysis.md` 仅有空表格框架，无实际案例数据。

**操作**（二选一）:
- **填充**: 从已完成的项目中提取 1-2 个真实案例填入（参照 `cases/postmortem-template.md` 格式）
- **移除**: 若近期无案例可填，删除文件并从 CLAUDE.md 中移除条目，待有真实数据时再创建

### 5. settings.local.json 权限整理（74条 → ~30条）

**问题**: 74 条 allow 规则为交互式累积，存在大量一次性批准（如 `Bash(printf writing:*)`、`Bash(do echo:*)`）。

**操作**:
- 按类别分组审查:
  - **保留**: `Bash(git:*)`, `Bash(python:*)`, `Bash(rm:*)`, `WebFetch(domain:*)` 等常用项
  - **合并**: 多个同类 Bash 命令用通配符合并
  - **删除**: 明显的一次性操作（含完整文件路径的、拼写错误的、调试用的）
- 建议结构: 按 Bash / WebFetch / mcp / Read 分组，每组附注释
- 目标: ≤35 条，每条有明确用途

---

## P2 维护清理

### 6. 清理 __pycache__

```bash
rm -rf .claude/skills/semantic-scholar/scripts/__pycache__/
```
确认 `.gitignore` 中 `__pycache__/` 模式已覆盖此路径（现有模式应已生效，但目录残留在磁盘上）。

### 7. 补充 path_patterns frontmatter

为以下两个 rules 添加 frontmatter，明确其全局作用域:

`.claude/rules/phase-context.md` 顶部添加:
```yaml
---
path_patterns:
  - "**/*"
---
```

`.claude/rules/project-file-org.md` 同上处理。

**理由**: 显式声明全局作用域，与其他 3 条 rules 的 frontmatter 风格保持一致。

### 8. CLAUDE.md 索引补充 agents/commands/templates

在 CLAUDE.md 中增加一个简表:
```markdown
## 其他组件
| 目录 | 文件 | 说明 |
|------|------|------|
| agents/ | reviewer.md | 自定义评审 agent，5维评分体系 |
| commands/ | proposal.md | 主路由命令，阶段分发与内存管理 |
| templates/ | project-progress-init.md | 新项目进度文件模板 |
```

### 9. 处理 skill 内容重叠

**问题**: `module-writing-guide` 与 `proposal-style-review` 有 ~40% 重叠（三级差距递进、证据漏斗、创新包装等）。

**操作**: 在 `module-writing-guide/SKILL.md` 顶部添加说明:
```
> 本 skill 为模块写作操作指南。完整风格规范和审稿视角请参考 proposal-style-review skill。
> 重叠内容为有意保留：本文件用于写作时，style-review 用于审稿时。
```
此声明使重叠成为有意识的设计决策，而非遗漏。

---

## P3 架构增强建议

### 10. 健康检查文件

创建 `.claude/health-check.md`，记录项目预期状态:
```markdown
# 项目健康检查
## 预期指标
- CLAUDE.md: ≤50 行
- Memory 文件: ≤12 个, 总计 ≤60KB
- 单个 memory: ≤100 行
- Skills: 每个 SKILL.md ≤200 行
- Rules: 每个 ≤50 行, 全部有 path_patterns
- settings.local.json: ≤35 条 allow
- 无 __pycache__、.pyc、.env 文件
## 检查频率: 每月一次或重大重构后
```

### 11. Memory 文件新鲜度追踪

对包含外部信息（API文档、工具版本等）的 memory 文件，在 frontmatter 中添加:
```yaml
last_verified: 2026-03-07
```
适用文件: `toolkit.md`, `kb/word-template-filling.md`, `kb/cross-project-reuse.md`

### 12. Skill 版本约定

重大重构时在 SKILL.md frontmatter 中添加版本号:
```yaml
version: 2.0
changelog: 从 proposal-write 拆分累积机制
```

---

## 通用最佳实践

适用于任何 Claude Code 项目，非 proposal_x 特有。

| 规则 | 标准 | 说明 |
|------|------|------|
| CLAUDE.md | 纯索引，≤30行理想，≤50行可接受 | 不内嵌知识，只做目录 |
| Memory 文件 | topic-scoped, kebab-case, ≤100行/文件 | 超过则拆分为子主题 |
| Skill 文件 | ≤200行/SKILL.md | 超过则拆分功能模块 |
| Rules | 必须有 path_patterns frontmatter | 全局规则用 `**/*` |
| Settings | 每季度整理，合并通配符 | 目标: 实际用途清晰可辨 |
| 本地机密 | schema 文档化, gitignored | 用 local-schema.md 说明结构 |
| Git 卫生 | 禁止: `__pycache__/`, `.pyc`, `.env` | .gitignore + 定期检查磁盘 |
| 索引完整性 | CLAUDE.md 与磁盘文件 1:1 对应 | 新增/删除文件后立即更新索引 |
