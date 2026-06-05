---
Belongs to: "[[Agent工程]]"
aliases: ["Agent记忆结构化", "知识图谱Schema设计"]
tags: ["Agent记忆", "知识图谱", "结构化数据"]
created: 2026-05-24
source: ai-generated
source_url: "https://x.com/akshay_pachaar/article/2058976178908885210"
concepts: ["Agent记忆", "知识图谱", "本体论", "Pydantic Schema", "实体抽取", "多跳推理", "10/10/10约束"]
confidence: medium
---
# PydanticAgent记忆

> [!abstract]- AI 摘要
> Agent 记忆系统不能依赖无结构的向量检索或自由抽取，必须用 Pydantic 定义领域本体 Schema 来约束实体类型、关系标签和属性，才能在查询时实现精确过滤和多跳推理。

---

## 扫读

> [!tip] 💡 一句话
> 不给 Agent 定义记忆 Schema，等于让它记住了所有事却一件也查不出来。

> [!important] 📌 关键结论
> - 向量检索在单事实查询时有效，但遇到需要连接多个事实的多跳推理就会失效
> - 知识图谱的记忆质量取决于抽取阶段的 Schema 定义——未定义类型时，所有节点都是通用"Object"，所有关系都是"RELATES_TO"
> - 用 Pydantic 定义 EntityModel 和 EdgeModel，不仅约束分类，还通过 docstring 教给抽取模型领域词汇

> [!quote] 🎬 可行动项
> - 先用 3-4 个实体类型和 3-4 个关系类型覆盖 80% 领域逻辑，不要试图建模一切
> - Pydantic 的 Field description 要写具体示例，描述本身就是分类指令

---

## 精读

### 论证链

```
向量检索：按相似度匹配文本块
      ↓
问题：多跳推理需要连接不同块中的事实
（Alice→Project Atlas→PostgreSQL→周二宕机）
      ↓
知识图谱：实体为节点，关系为边，遍历替代匹配
      ↓
但图谱质量取决于抽取步骤
      ↓
默认行为：LLM 自主决定实体类型和关系标签
→ 所有工单变成"Topic"，所有客户变成"Object"
      ↓
机制：用 Pydantic Schema 约束抽取
① EntityModel 定义领域实体类型（User, Ticket, Project）
② EdgeModel 定义关系类型和属性（WORKS_ON, USES_TECHNOLOGY）
③ EntityEdgeSourceTarget 限制 source/target 类型组合
④ 10/10/10 约束强制聚焦核心领域概念
      ↓
结论：Schema 定义 Agent 的"合法记忆空间"——没定义的关系不会被记录
```

### 关键引述

> Agent memory without schema discipline is a graph that behaves like a vector store. You pay the cost of graph construction without getting the benefit of structured retrieval.

> The schema defines the space of valid memories. This is the same principle behind typed function calling, where we constrain the LLM's output space so that it can't produce invalid arguments.

> Zep enforces a hard limit of 10 custom entity types, 10 custom edge types, and 10 fields per type. That's intentional to force a dev to think about what matters in a domain rather than modeling everything.

### 局限与盲区

- 本文未覆盖：Schema 设计本身的迭代方法——领域模型如何随业务演进更新？已有历史数据如何迁移到新 Schema？
- 隐含假设：假设 Agent 的领域概念可以预先穷举。对于开放域对话 Agent，10/10/10 的约束可能太紧
- 可能的反例：如果问题涉及跨多个 Schema 边界的推理（如"客户的问题与竞品特性有关"），Schema 约束反而会阻止关键关系的记录

---

## 关联

- [[WIKI/Agent最简实现原理]]
- [[WIKI/RAG向量检索核心抉择]]
- [[WIKI/AgentHarness架构]]
