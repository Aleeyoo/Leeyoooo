---
Belongs to: "[[Agent工程]]"
aliases: ["多智能体协作", "Agent分工", "子代理调度", "Multi-Agent系统"]
tags: ["多智能体", "Agent架构", "上下文隔离", "任务调度", "fan-out"]
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/Russell3402/status/2056331558223786416"
concepts: ["fan-out/fan-in", "星型拓扑", "Gateway routing", "durable queue", "delegation contract", "触发方式四分类", "上下文隔离", "merge/reduce"]
confidence: medium
---
# 多Agent分工协作

> [!abstract]- AI 摘要
> 系统对比 Codex、Claude Code、OpenClaw、Hermes Agent 四种多智能体系统的设计差异：从触发方式、拓扑结构到调用链，提炼出多 Agent 协作的工程原则和反模式。

---

## 扫读

> [!tip] 💡 一句话
> 多智能体协作不是"多开几个模型实例"这么简单，真正工程化的系统需要考虑触发方式、上下文隔离、权限控制、状态管理和结果合并，先设计边界再增加 Agent 数量。

> [!important] 📌 关键结论
> - 四种触发方式：显式触发（Codex）、语义触发（Claude Code description 匹配）、路由触发（OpenClaw 按入口分发）、队列触发（Hermes Kanban 持久化任务）。
> - 拓扑选择核心原则：星型（fan-out/fan-in）适合独立并行任务，链式 pipeline 适合强顺序任务，网状 team 适合多假设互相挑战，Gateway routing 适合多入口隔离，durable board 适合跨天长期协作。
> - 七个反模式：把复杂度当触发器、不给 delegation contract、多个 worker 写同一片代码、没有 reducer、短任务做队列/长任务做 RPC、权限过宽、没有观测和审计。

> [!quote] 🎬 可行动项
> - 在决定用多 Agent 之前，先问"单 Agent 能不能做"，能做就别拆
> - 为每个 worker 写 delegation contract（角色、目标、上下文、权限、所有权、禁止项、输出格式、停止条件）
> - 区分短程并行（用 fork/join）和长期协作（用 durable queue），不要把两种任务混用

---

## 精读

### 论证链

```
打破简化认知：多 Agent ≠ 多个模型实例
        ↓
工程化需解决的六个核心问题：
  ① 谁有权创建 worker
  ② worker 拿到多少上下文
  ③ 能不能写文件
  ④ 写入冲突怎么处理
  ⑤ 失败怎么恢复
  ⑥ 结果怎么 merge
        ↓
四种主流系统对比：
  Codex：显式 fan-out，控制权留给用户
  Claude Code：description 驱动自动路由 + Agent Teams
  OpenClaw：Gateway 路由 + 入口身份隔离 + ACP 外部 harness
  Hermes：delegate_task 短程 RPC + Kanban 长程 durable queue
        ↓
五个场景验证选型逻辑：
  PR review → 生产登录故障 → 多渠道助理 → 两天调研 → repo-wide migration
        ↓
七个反模式 + 选择顺序：先设计边界，再增加 Agent 数量

### 关键引述

> "对 subagent 来说，委派信息就是需求文档。你不能把一个 worker 拉进来只说'修一下'，然后期待它理解项目路径、错误现场、相关文件、验收标准、禁止事项和输出格式。"

> "一次性并行和持久协作不是同一种东西。前者需要 fork/join，后者需要队列、状态、重试、评论、handoff 和审计轨迹。"

> "先设计边界，再增加 agent 数量。这个顺序不会显得炫，但它更接近真实工程。"

### 局限与盲区

- 本文未覆盖：各系统的实际成本对比（token 消耗、延迟数据）、在真实生产环境中的大规模部署案例数据、不同模型能力下各系统的表现差异。
- 隐含假设：用户对四种系统的文档和 API 有基本了解。假设团队有能力编写和维护 delegation contract 模板。
- 可能的反例：对于初创团队或个人开发者，文中描述的多层路由和持久化队列架构可能过度设计，简单的星型 subagent 已经足够。

---

## 关联

- [[AI商业]]
- [[AgentHarness架构]]
- [[AI工程团队管理]]
- [[ClaudeCode斜杠命令]]
