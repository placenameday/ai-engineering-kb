---
name: architecture-guide
description: "Harness-compliant AI system architecture design - multi-agent coordination, system blueprint, evolution path"
survey_date: "2026-03-27"
lang: "en/zh"
---

# AI 系统架构设计指南

> 构建 Harness 工程规范的多 Agent 系统。从架构蓝图到协作模式。
> 目标：高效协作，实现任务目标。

---

## 0. 核心架构蓝图

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         HARNESS LAYER                                    │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │ Permissions │  │    Hooks    │  │ Guardrails  │  │   State     │    │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘    │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        COORDINATION LAYER                                │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    ORCHESTRATOR (协调者)                          │   │
│  │         Task Decomposition │ Routing │ Merging │ Escalation      │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐           │
│  │   Task    │  │  Shared   │  │  Message  │  │  Conflict │           │
│  │   Queue   │  │  Memory   │  │   Bus     │  │  Resolver │           │
│  └───────────┘  └───────────┘  └───────────┘  └───────────┘           │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          AGENT LAYER                                     │
│                                                                         │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐     │
│  │ Planner │  │ Builder │  │Reviewer │  │Research │  │  ...    │     │
│  │ (规划)  │  │ (执行)  │  │ (审核)  │  │ (调研)  │  │         │     │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘  └─────────┘     │
│                                                                         │
│  每个 Agent: 单一职责 + 独立 Context Window + 通过 Bus 通信            │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        CONTEXT LAYER (关键!)                             │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────────────┐       │
│  │  Progressive  │  │  Memory       │  │  Context Assembly     │       │
│  │  Disclosure   │  │  Index+Retrieval│ │  (按需组装)           │       │
│  └───────────────┘  └───────────────┘  └───────────────────────┘       │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────────────┐       │
│  │  Compressor   │  │  Budget       │  │  Scoped Rules         │       │
│  │  (压缩/摘要)  │  │  Manager      │  │  (路径/条件加载)       │       │
│  └───────────────┘  └───────────────┘  └───────────────────────┘       │
│                                                                         │
│  Principle: Just-In-Time > Pre-Load; Focused 300tok > Unfocused 113Ktok│
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          TOOL LAYER                                      │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌───────────┐           │
│  │    MCP    │  │   APIs    │  │  Storage  │  │  External │           │
│  │  Servers  │  │           │  │           │  │  Systems  │           │
│  └───────────┘  └───────────┘  └───────────┘  └───────────┘           │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 1. 架构设计原则

### 1.1 Harness 七组件实现

| Component | Architecture Role | Implementation |
|-----------|-------------------|----------------|
| **Permissions** | 访问控制层 | 每个工具调用前检查 |
| **Hooks** | 生命周期拦截 | Pre/Post 执行点 |
| **Guardrails** | 控制平面 | 预算/范围/置信度检查 |
| **State** | 状态管理层 | Checkpoint + Memory |
| **Isolation** | 边界隔离 | Agent 独立 context |
| **Tools** | 能力层 | MCP protocol |
| **Feedback** | 质量循环 | Eval + Review |

### 1.2 核心设计原则

| Principle | Rule | Why |
|-----------|------|-----|
| **单一职责** | 一个 Agent = 一个能力 | 调试、测试、复用 |
| **协调者模式** | 永远只有一个 Orchestrator | 避免递归混乱 |
| **隔离上下文** | Agent 不共享 context window | 防止干扰 |
| **显式通信** | 通过 Message Bus，不直接调用 | 可追踪、可调试 |
| **状态统一** | 执行态 = 业务态 | 单一真相来源 |
| **渐进自治** | Advisory → Supervised → Autonomous | 安全部署 |

---

## 2. Multi-Agent 协作模式

### 2.1 协作模式选择

```
任务类型？

├── 顺序依赖？ ──yes──► PIPELINE 模式
│   (A → B → C)        适合：代码生成 → 测试 → 部署
│
├── 独立子任务？ ──yes──► PARALLEL 模式
│   (A, B, C 同时)     适合：同时调研多个方向
│
├── 需要辩论/投票？ ──yes──► DEBATE 模式
│   (A ↔ B ↔ C)        适合：高风险决策
│
├── 专业化分工？ ──yes──► HIERARCHICAL 模式
│   (Lead → Workers)    适合：复杂项目
│
└── 需要验证？ ──yes──► GENERATOR-EVALUATOR 模式
    (Producer → Reviewer) 适合：代码、文档
```

### 2.2 模式详解

#### PIPELINE (流水线)

```
┌────────┐    ┌────────┐    ┌────────┐
│ Agent A│───►│ Agent B│───►│ Agent C│
│ Planner│    │ Builder│    │Reviewer│
└────────┘    └────────┘    └────────┘
    │              │              │
    ▼              ▼              ▼
  Output ──────► Input ──────► Final
```

**适用：** 有明确依赖顺序的任务
**实现：** Task Queue + 状态传递

#### PARALLEL (并行)

```
              ┌────────┐
         ┌───►│ Agent A│───┐
         │    └────────┘    │
┌────────┐                  │    ┌────────┐
│ Split  │    ┌────────┐    ├───►│ Merge  │
│        ├───►│ Agent B│───┤    │        │
└────────┘    └────────┘    │    └────────┘
         │    ┌────────┐    │
         └───►│ Agent C│───┘
              └────────┘
```

**适用：** 独立子任务，需要加速
**实现：** Async + gather + merge

#### DEBATE (辩论)

```
        ┌─────────────────────────┐
        │                         │
        ▼                         │
   ┌────────┐              ┌────────┐
   │ Agent A│◄────────────►│ Agent B│
   │ Pro    │              │ Con    │
   └────────┘              └────────┘
        │                         │
        ▼                         ▼
   ┌─────────────────────────────────┐
   │           Moderator             │
   │      (Final Decision)           │
   └─────────────────────────────────┘
```

**适用：** 高风险决策，需要多角度
**实现：** 轮次制 + Moderator 裁决

#### HIERARCHICAL (层级)

```
                    ┌────────────┐
                    │   LEAD     │
                    │ Orchestrator│
                    └────────────┘
                          │
            ┌─────────────┼─────────────┐
            │             │             │
            ▼             ▼             ▼
       ┌────────┐    ┌────────┐    ┌────────┐
       │Worker A│    │Worker B│    │Worker C│
       │ Research│    │ Coding │    │ Testing│
       └────────┘    └────────┘    └────────┘
```

**适用：** 复杂项目，需要专业化
**实现：** Lead 分解 → Worker 执行 → Lead 合并

#### GENERATOR-EVALUATOR (生成-评估)

```
┌────────────┐         ┌────────────┐
│  GENERATOR │────────►│  EVALUATOR │
│  (Produce) │         │  (Review)  │
└────────────┘         └────────────┘
      ▲                      │
      │      ┌───────────────┘
      │      │  Revise if fail
      └──────┘
```

**适用：** 代码、文档、创作
**实现：** 迭代直到通过

### 2.3 模式对比

| Pattern | Speed | Quality | Cost | Complexity |
|---------|-------|---------|------|------------|
| Pipeline | Medium | Medium | Low | Low |
| Parallel | Fast | Medium | Medium | Medium |
| Debate | Slow | High | High | High |
| Hierarchical | Medium | High | Medium | Medium |
| Gen-Eval | Variable | High | Variable | Low |

---

## 3. 协调层设计

### 3.1 Orchestrator 职责

```
Orchestrator 负责：
├── 1. 任务分解 (Decomposition)
│   └── 大任务 → 子任务 → 分配给 Agent
├── 2. 路由决策 (Routing)
│   └── 哪个 Agent 处理哪个任务
├── 3. 状态合并 (Merging)
│   └── 多个 Agent 输出 → 统一结果
├── 4. 冲突解决 (Conflict Resolution)
│   └── Agent 间意见不一致时裁决
├── 5. 异常升级 (Escalation)
│   └── 无法处理 → 人工介入
└── 6. 进度追踪 (Progress Tracking)
    └── Task Queue 状态管理
```

### 3.2 Task Queue 设计

```python
class Task:
    id: str
    type: str           # research, code, review, etc.
    status: str         # pending, in_progress, completed, failed
    assigned_agent: str
    dependencies: list[str]
    input: dict
    output: dict
    priority: int

class TaskQueue:
    def get_next(self, agent_capabilities: list) -> Task:
        """获取下一个可执行任务（依赖已满足）"""
        pass

    def complete(self, task_id: str, output: dict):
        """标记完成，解锁依赖任务"""
        pass
```

### 3.3 Shared Memory 设计

```
┌─────────────────────────────────────────────────────────────┐
│                     SHARED MEMORY                            │
├─────────────────────────────────────────────────────────────┤
│  Project Context                                             │
│  ├── Project goals, constraints                              │
│  ├── Architecture decisions                                  │
│  └── Conventions, patterns                                   │
├─────────────────────────────────────────────────────────────┤
│  Session State                                               │
│  ├── Current task, progress                                  │
│  ├── Intermediate results                                    │
│  └── Active agents, their status                             │
├─────────────────────────────────────────────────────────────┤
│  Cross-Agent Knowledge                                       │
│  ├── Findings from research agents                           │
│  ├── Code patterns from builder agents                       │
│  └── Issues from reviewer agents                             │
└─────────────────────────────────────────────────────────────┘
```

**Access Pattern:**
- Agent 启动时加载相关 Project Context
- 执行中读写 Session State
- 完成时写入 Cross-Agent Knowledge

### 3.4 Message Bus 设计

```python
# 消息格式
class AgentMessage:
    from_agent: str
    to_agent: str | "broadcast"
    type: str          # task_complete, request_help, share_finding
    payload: dict
    timestamp: datetime

# Bus 实现
class MessageBus:
    def publish(self, message: AgentMessage):
        """发布消息"""
        pass

    def subscribe(self, agent_id: str, handler: Callable):
        """订阅消息"""
        pass
```

---

## 4. Agent 设计

> Agent 代码模板见 [practical-guide-oc.md §10](practical-guide-oc.md)。
> CC 内置 Agent 类型见 §9.2。

### 4.1 Agent 模板

```python
class Agent:
    def __init__(self, config: AgentConfig):
        self.id = config.id
        self.role = config.role              # planner, builder, reviewer, etc.
        self.capabilities = config.capabilities
        self.llm = config.llm
        self.tools = self._load_tools(config.tools)
        self.context_window = []

    async def run(self, task: Task) -> AgentResult:
        # 1. 加载相关 context
        context = await self.load_context(task)

        # 2. 执行任务
        response = await self.llm.call(
            system=self.system_prompt,
            tools=self.tools,
            messages=context + [task.to_message()]
        )

        # 3. 处理工具调用
        while response.has_tool_calls():
            for tool_call in response.tool_calls:
                result = await self.execute_tool(tool_call)
                context.append(tool_result(tool_call, result))

            response = await self.llm.call(
                system=self.system_prompt,
                tools=self.tools,
                messages=context
            )

        # 4. 返回结果
        return AgentResult(
            task_id=task.id,
            output=response.content,
            artifacts=self.artifacts
        )

    async def execute_tool(self, tool_call) -> ToolResult:
        # Harness: Permission check
        if not self.permissions.check(tool_call):
            return ToolResult(denied=True, reason="Permission denied")

        # Harness: Control plane
        allowed, reason = self.control_plane.evaluate(tool_call)
        if not allowed:
            return ToolResult(denied=True, reason=reason)

        # Execute
        return await self.tools.execute(tool_call)
```

### 4.2 Agent 角色定义

| Role | Responsibility | Tools | Output |
|------|----------------|-------|--------|
| **Planner** | 分解任务，制定计划 | Read, WebSearch | Plan, Task List |
| **Researcher** | 调研信息，收集资料 | WebSearch, Read | Findings, Summary |
| **Builder** | 执行实现，生成代码 | Read, Write, Edit, Bash | Code, Files |
| **Reviewer** | 质量检查，发现问题 | Read, Grep | Review Report |
| **Tester** | 运行测试，验证功能 | Bash, Read | Test Results |
| **Coordinator** | 协调多 Agent | TaskQueue, MessageBus | Status, Decisions |

### 4.3 Agent 通信协议

```python
# 任务完成通知
{
    "type": "task_complete",
    "from": "builder_1",
    "to": "coordinator",
    "payload": {
        "task_id": "T-001",
        "status": "success",
        "artifacts": ["src/module.py"]
    }
}

# 请求帮助
{
    "type": "request_help",
    "from": "builder_1",
    "to": "researcher_1",
    "payload": {
        "question": "What's the best pattern for X?",
        "context": "..."
    }
}

# 共享发现
{
    "type": "share_finding",
    "from": "researcher_1",
    "to": "broadcast",
    "payload": {
        "finding": "Library Y has a bug in version 2.0",
        "relevance": ["builder_1", "builder_2"]
    }
}
```

---

## 5. Context 能力层设计

> **Context ⊂ Harness**。Context Layer 是 Agent 的"认知系统"。
> 详细理论见 [context-engineering.md](concepts/context-engineering.md)

### 5.1 Context 组装流程

```
Agent 收到 Task
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│ 1. ASSEMBLE: 组装初始 context                                │
│    ├── System prompt (角色 + 约束)                           │
│    ├── Task description                                      │
│    ├── Relevant memory (检索匹配)                            │
│    └── Tool descriptions (按需加载)                          │
│    Budget: < 30% of context window                           │
└─────────────────────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. EXECUTE: Agent 循环执行                                    │
│    ├── LLM 推理                                               │
│    ├── Tool calls (progressive disclosure: 按需获取)          │
│    └── Append results to context                              │
└─────────────────────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. COMPRESS: 当 context > 70% capacity                       │
│    ├── Summarize older turns                                  │
│    ├── Extract key decisions to memory                        │
│    └── Drop resolved details                                  │
└─────────────────────────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. PERSIST: 任务完成后                                       │
│    ├── Write findings to shared memory                        │
│    ├── Update memory index                                    │
│    └── Checkpoint state (if needed)                           │
└─────────────────────────────────────────────────────────────┘
```

### 5.2 Progressive Disclosure (逐步揭露)

**核心原则：** 只在需要时加载需要的信息。不要预加载一切。

```
信息加载策略：

Level 0 - Always Loaded (每轮都带)
├── System prompt (~100 lines)
├── Active task description
└── Permission rules (minimal)

Level 1 - Index Only (只加载索引)
├── Memory index (MEMORY.md)
├── Tool description list
└── File structure overview

Level 2 - On-Demand (按需读取)
├── Specific memory file (agent reads when relevant)
├── Source code files (agent reads when editing)
├── Research documents (agent reads when referencing)
└── MCP resource content

Level 3 - Just-In-Time (执行时获取)
├── Web search results
├── API responses
├── Database query results
└── External system state
```

**实现示例：**
```python
class ProgressiveDisclosure:
    def __init__(self, memory_store, file_store):
        self.memory = memory_store
        self.files = file_store

    def get_context(self, task: Task, current_context_size: int) -> list:
        """按需组装 context，控制总量"""
        context = []

        # Level 0: 始终加载
        context.append(self.get_system_prompt())
        context.append(task.to_message())

        # Level 1: 只加载索引
        if self.needs_memory(task):
            context.append(self.memory.get_index())  # NOT full memory

        # Level 2: 按需读取 (通过 tool call 触发)
        # Agent 通过 read_memory() / read_file() 工具按需获取

        # Budget check
        total = estimate_tokens(context)
        if total > current_context_size * 0.7:
            context = self.compress(context)

        return context

    def compress(self, context: list) -> list:
        """压缩 context：摘要 + 关键点保留"""
        system = context[0]  # 保留 system prompt
        summary = summarize(context[1:])  # 摘要历史
        return [system, summary]
```

### 5.3 记忆系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                    MEMORY ARCHITECTURE                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Working Memory (工作记忆)                             │    │
│  │ = 当前 context window                                 │    │
│  │ 内容: 当前任务 + 最近交互 + 活跃工具                  │    │
│  │ 预算: < 70% context window                            │    │
│  │ 生命周期: 单次会话                                    │    │
│  └─────────────────────────────────────────────────────┘    │
│                         │ read / write                       │
│                         ▼                                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Structured Long-Term Memory (结构化长期记忆)          │    │
│  │ = 文件系统 / 数据库                                   │    │
│  │ 内容: 项目知识、决策、约定、模式                      │    │
│  │ 结构: 索引文件 (MEMORY.md) + 分类文件                 │    │
│  │ 检索: 关键词匹配 / 向量搜索 / 路径匹配                │    │
│  │ 生命周期: 跨会话持久化                                │    │
│  └─────────────────────────────────────────────────────┘    │
│                         │ semantic search                    │
│                         ▼                                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Semantic Memory (语义记忆) [可选]                     │    │
│  │ = 向量数据库 (Mem0 / Chroma)                          │    │
│  │ 内容: 历史会话摘要、学习到的模式                      │    │
│  │ 检索: Embedding 相似度                                │    │
│  │ 生命周期: 永久 (除非主动清理)                         │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─    │
│  Page In/Out (MemGPT 模式):                                  │
│  Context Window = RAM, External Storage = Disk               │
│  Agent 自主决定何时 page in (加载) / page out (写回)         │
└─────────────────────────────────────────────────────────────┘
```

**记忆索引设计：**
```markdown
# MEMORY.md (索引文件 - 始终加载)

## User
- [User Profile](user-profile.md) — 角色、偏好、知识背景

## Project
- [Architecture Decisions](arch-decisions.md) — 重要技术决策
- [Conventions](conventions.md) — 代码规范、命名约定

## Feedback
- [Code Review Notes](review-feedback.md) — 代码审查偏好

## Reference
- [API Endpoints](api-reference.md) — 外部系统接口
```

> 索引只放**标题+一行描述**，详细内容按需读取。
> MEMORY.md 限制 ~200 行以内，每行 < 150 字符。

### 5.4 Context Budget 管理

```
Context Window 分配策略：

┌────────────────────────────────────────────┐
│ Context Window (200K tokens)               │
├────────────────────────────────────────────┤
│ ┌──────────────────────┐                   │
│ │ System Prompt        │ ~5%  (~10K)       │
│ └──────────────────────┘                   │
│ ┌──────────────────────┐                   │
│ │ Tool Descriptions    │ ~10% (~20K)       │
│ └──────────────────────┘                   │
│ ┌──────────────────────┐                   │
│ │ Conversation History │ ~40% (~80K)       │
│ └──────────────────────┘                   │
│ ┌──────────────────────┐                   │
│ │ Working Space        │ ~45% (~90K)       │
│ │ (留给推理 + 工具结果) │                   │
│ └──────────────────────┘                   │
└────────────────────────────────────────────┘

警告线:
- 70% (140K): 触发压缩 /compact
- 90% (180K): 模型能力明显下降
- 95% (190K): 自动压缩 (太晚了!)
```

### 5.5 Context Anti-Patterns

| Anti-Pattern | Problem | Solution |
|-------------|---------|----------|
| **预加载一切** | 浪费 token，Context Rot | Progressive Disclosure |
| **CLAUDE.md 塞满** | 每轮都付 token 成本 | 索引 + 按需读取 |
| **Memory 无索引** | Agent 不知道有什么 | MEMORY.md 索引文件 |
| **不压缩** | Context 膨胀 → 能力下降 | 70% 阈值自动压缩 |
| **所有工具同时加载** | Tool schema 占 ~26K | 分域加载 / Meta-Tool |
| **跨 Agent 共享 context** | 互相干扰 | 隔离 + Message Bus |

---

## 6. Harness 层实现

> 架构设计决定了 Harness 需要什么。具体代码实现见 [practical-guide-oc.md §3](practical-guide-oc.md)。
> CC 配置见 [practical-guide-cc.md §4-5](practical-guide-cc.md)。

### 6.1 Permissions — 设计要点

- Precedence: deny > ask > allow
- Default: ask (安全优先)
- 每个 Agent 可以有独立权限集

### 6.2 Control Plane — 五道护栏

| Guardrail | Check | Config |
|-----------|-------|--------|
| Budget | 费用阈值 | `budget_limit` |
| Scope | 路径/域名白名单 | `scope_patterns` |
| Confidence | 置信度下限 | `confidence_threshold` |
| Rate Limit | 调用频率 | `rate_limit` |
| Audit | 全量记录 | always on |

### 6.3 State Management — 设计要点

- Checkpoint 包含: Task Queue + Agent States + Shared Memory
- 恢复时完整还原系统状态
- 实现代码见 [practical-guide-oc.md §3.4](practical-guide-oc.md)

---

## 7. 系统演进路径

### 7.1 从简单到复杂

```
Phase 1: Single Agent + Tools
┌─────────────────────────────────┐
│           SINGLE AGENT          │
│    LLM + Tools + Basic Harness  │
└─────────────────────────────────┘
适用：简单任务，快速原型

Phase 2: Generator + Evaluator
┌────────────┐    ┌────────────┐
│  Generator │───►│  Evaluator │
└────────────┘    └────────────┘
适用：需要质量保证的任务

Phase 3: Pipeline Agents
┌────────┐    ┌────────┐    ┌────────┐
│ Agent A│───►│ Agent B│───►│ Agent C│
└────────┘    └────────┘    └────────┘
适用：有明确阶段的任务

Phase 4: Full Multi-Agent System
┌─────────────────────────────────┐
│          ORCHESTRATOR           │
├─────────────────────────────────┤
│  ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐│
│  │Agent│ │Agent│ │Agent│ │Agent││
│  └─────┘ └─────┘ └─────┘ └─────┘│
├─────────────────────────────────┤
│    Task Queue + Memory + Bus    │
└─────────────────────────────────┘
适用：复杂项目，需要专业化
```

### 7.2 演进决策

```
当前系统问题？

├── 质量不够？
│   ├── 无验证？ → 加 Evaluator Agent
│   └── 单一视角？ → 加 Debate 模式
│
├── 速度太慢？
│   ├── 顺序瓶颈？ → 并行化
│   └── 单 Agent 过载？ → 专业化分工
│
├── 复杂任务无法处理？
│   ├── 分解不清？ → 加 Planner Agent
│   └── 协调混乱？ → 加 Orchestrator
│
└── 系统不可控？
    ├── 无权限控制？ → 加 Permissions
    ├── 无监控？ → 加 Control Plane
    └── 状态丢失？ → 加 State Management
```

---

## 8. 架构检查清单

### 8.1 设计阶段

```
□ 任务是否清晰定义？
□ Agent 职责是否单一？
□ 协作模式是否合适？
□ 通信协议是否明确？
□ Harness 组件是否完整？
```

### 8.2 实现阶段

```
□ 每个 Agent 有独立 context window？
□ 有统一的 Orchestrator？
□ Task Queue 实现正确？
□ Message Bus 可追踪？
□ Shared Memory 有访问控制？
```

### 8.3 部署阶段

```
□ Permissions 规则配置完成？
□ Control Plane 阈值合理？
□ State Checkpoint 可恢复？
□ 监控和告警就绪？
□ Evals 通过？
```

### 8.4 运行阶段

```
□ Agent 协作效率如何？
□ 冲突解决是否顺畅？
□ 异常升级是否及时？
□ 成本在预算内？
□ 质量满足要求？
```

---

## 9. Claude Code 团队模式实现

### 9.1 Team 模式

```python
# 使用 Claude Code 内置 Team 功能
from claude_code import TeamCreate, Agent, TaskCreate, TaskUpdate

# 创建团队
TeamCreate(
    team_name="dev-team",
    description="Full-stack development team",
    agent_type="general-purpose"
)

# 创建任务
TaskCreate(subject="Build feature X", description="...")
TaskCreate(subject="Review code", description="...", add_blocked_by=["1"])

# 生成 Agent
Agent(name="builder", subagent_type="rex", team_name="dev-team")
Agent(name="reviewer", subagent_type="hawk", team_name="dev-team")

# 协调
SendMessage(to="builder", message="Start on task 1")
# builder 完成后
SendMessage(to="reviewer", message="Review builder's output")
```

### 9.2 内置 Agent 类型

| Type | Role | Best For |
|------|------|----------|
| `general-purpose` | 通用 | 灵活任务 |
| `Explore` | 探索 | 代码库搜索 |
| `Plan` | 规划 | 架构设计 |
| `sage` | 调研 | 研究、根因分析 |
| `rex` | 执行 | 技术实现 |
| `hawk` | 审核 | 质量检查 |
| `iris` | 内容 | 写作、分析 |

---

## 10. 关键决策点

| Decision | Options | Recommendation |
|----------|---------|----------------|
| 用框架还是直接 API？ | Direct / Pydantic AI / LangGraph | 简单 → Direct；复杂 → LangGraph |
| 多少个 Agent？ | 1 / 2-5 / 5+ | 从少开始，按需增加 |
| 协作模式？ | Pipeline / Parallel / Debate / Hierarchical | 匹配任务特性 |
| 状态存储？ | Memory / DB / Checkpoint | 短期 → Memory；长期 → DB |
| 通信方式？ | 直接调用 / Message Bus | 小系统 → 直接；大系统 → Bus |

---

> **Source:** AI Engineering Frontier KB, 12-Factor Agents, Anthropic Agent Design, Claude Code Team Mode
>
> Last updated: 2026-03-27
