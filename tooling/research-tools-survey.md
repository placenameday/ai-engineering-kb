---
name: research-tools-survey
description: >
  Survey of AI research writing tools (Semantic Scholar, Elicit, Scite, Jenni AI, Consensus, Paperpal),
  agentic workflow frameworks (AutoGen, CrewAI, MetaGPT, AgentLaboratory), Claude Code project-level
  configuration (skills/agents/commands structure), Zotero MCP integration, NSFC/provincial grant template
  standards, and technical implementation roadmap. Use when evaluating new tools, comparing approaches,
  setting up new project infrastructure, or onboarding. Historical reference from 2026-02-28.
survey_date: 2026-02-28
lang: zh
---

# 科研申请书 AI 辅助工具与最佳实践调研报告

**调研日期**: 2026-02-28
**调研范围**: AI 科研写作工具、Agentic Workflow、Claude Code Skills/Agents、文献管理集成、标书模板规范

---

## 一、现有 AI 科研写作工具对比

| 工具 | 核心功能 | 最佳场景 | 局限性 |
|------|---------|---------|--------|
| **Semantic Scholar** | AI 文献搜索、引用网络可视化 | 文献发现、趋势分析 | 仅搜索，不生成内容 |
| **Elicit** | 系统性文献综述、自动提取方法/结果 | 系统综述、Meta分析 | 需验证引用准确性 |
| **Scite.ai** | 智能引用分析(支持/反对/提及) | 引用验证 | 不涉及写作 |
| **Jenni AI** | 学术写作、大纲生成 | 论文/申请书起草 | 引用需手动验证 |
| **Consensus** | 证据综合、研究结论聚合 | 循证研究合成 | 功能相对单一 |

### 推荐工具组合

| 研究阶段 | 推荐工具 |
|---------|---------|
| 文献发现 | Semantic Scholar + Research Rabbit |
| 文献分析 | Elicit + Scite.ai |
| 写作起草 | Claude / Gemini |
| 语言润色 | Paperpal |
| 引用管理 | Zotero |

---

## 二、Agentic Workflow 案例

### 主流多 Agent 框架

| 框架 | 特点 | 适用场景 |
|------|------|---------|
| **AutoGen** | 开源、Magentic-One | 通用多 Agent |
| **CrewAI** | 中央编排器、角色专业化 | 团队协作模拟 |
| **MetaGPT** | 60k+ stars | 软件开发 |
| **AgentLaboratory** | 端到端科研 | 学术研究自动化 |

### Andrew Ng 四大 Agentic 设计模式

1. Reflection & Self-Correction
2. Planning & Tool Use
3. Multi-Agent Collaboration
4. Memory & Retrieval (RAG)

---

## 三、Claude Code 项目级配置

### Skills vs Commands vs Agents

| 组件 | 位置 | 触发方式 | 用途 |
|------|------|---------|------|
| **Commands** | `.claude/commands/*.md` | `/command-name` | 一次性任务 |
| **Skills** | `.claude/skills/*/SKILL.md` | 描述匹配/`/skill-name` | 领域知识包 |
| **Agents** | `.claude/agents/*.md` | 主 session 委托 | 独立子任务执行 |

---

## 四、文献管理集成 (Zotero)

| 方案 | 难度 | 功能 |
|------|------|------|
| **zotero-gpt 插件** | 低 | AI 摘要、智能标签 |
| **MCP Zotero Server** | 中 | Claude 直接访问 Zotero |
| **Zotero API** | 中 | 自定义集成 |

---

## 五、国自然模板规范

### 评审四大核心问题
1. 你要解决什么科学问题？
2. 为什么这个问题重要？
3. 你如何解决这个问题？
4. 你有什么能力解决？

### 五大致命扣分点
- 模糊或含糊不清的表述
- 评审专家无法准确理解研究意图
- 内容不翔实、层次不分明
- 删除/改动提纲标题
- 格式不规范

---

## 六、关键资源链接

- [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/skills)
- [K-Dense Scientific Skills](https://github.com/K-Dense-AI/claude-scientific-skills) - 148+ skills
- [Zotero MCP](https://github.com/54yyyu/zotero-mcp)
- [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) - 33k stars
- [Superpowers](https://github.com/obra/superpowers) - 16k stars

---

*报告生成: Sage | 版本: 1.0*
