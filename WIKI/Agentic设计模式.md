---
Belongs to: "[[Agent工程]]"
aliases: ["Agent设计模式", "Agent四级分类", "Agentic Design Patterns"]
tags: ["AI Agent", "设计模式", "Context Engineering", "Reflection", "多Agent协作", "Memory分层"]
created: 2026-05-24
source: ai-generated
source_url: "https://x.com/yanhua1010/article/2058552177912947044"
concepts: ["Agent四个能力等级", "Context Engineering四层信息模型", "Producer-Critic反射模式", "Memory三层架构（Session/State/Memory）", "Multi-Agent六种通信拓扑", "自变形Agent系统"]
confidence: medium
---
# Agentic设计模式

> [!abstract]- AI 摘要
> 从 Google 工程总监 Antonio Gulli 的 453 页著作提炼出 AI Agent 开发的 21 种设计模式，核心是四个等级（裸 LLM → 工具使用者 → 战略思考者 → 多 Agent 协作）、Context Engineering 四层模型、Producer-Critic 反射循环，以及 Memory 三层架构。

---

## 扫读

> [!tip] 💡 一句话
> 大多数人的 Agent 不好用不是因为模型差，而是因为还在把 LLM 当聊天机器人用（Level 0）——升级到 Level 2（工具+规划+Context Engineering+Reflection）就能覆盖绝大多数实际场景。

> [!important] 📌 关键结论
> - Agent 的四个等级定义清晰：Level 0 裸 LLM（不是 Agent）、Level 1 工具使用者（自己判断何时调工具）、Level 2 战略思考者（Context Engineering + 自我反思）、Level 3 多 Agent 协作——大多数人卡在 Level 1
> - Producer-Critic 反射模式是全书最具实战价值的 Pattern：用两个不同 Agent 分别生产和审查，简单但质量提升翻倍；关键是必须用不同的 system prompt，同一个 persona 审自己的东西一定有盲区
> - 多 Agent 不需要贪复杂，三种拓扑够用：单 Agent（独立执行）、对等网络（去中心化）、Supervisor（中心调度）；大多数场景 Level 2 单 Agent + Reflection 就够了

> [!quote] 🎬 可行动项
> - 给现有 Agent 加一个 Critic：在 workflow 末尾让另一个 Agent 审查上一步输出，多一次 LLM 调用但质量提升翻倍
> - 将 Prompt Engineering 升级为 Context Engineering：补上 Agent 当前所在项目、历史决策、用户偏好等"它应该知道但你没说"的上下文

---

## 精读

### 论证链

```
问题：Agent开发缺乏可复用的底层逻辑
      ↓
本书提炼21种设计模式，按能力分层：
  Level 0 → Level 1（工具使用，自己判断何时调用）
  Level 1 → Level 2（Context Engineering四层 + Producer-Critic反射循环）
  Level 2 → Level 3（六种通信拓扑：单Agent / P2P / Supervisor / 层级 / 混合）
      ↓
核心可落地模式：
  Context Engineering：system prompt（层1）+ 外部数据（层2）+ 隐式数据（层3）+ 反馈回路（层4）
  Reflection：Producer写→Critic审→Producer改→循环直至CODE_IS_PERFECT或达最大迭代次数
  Memory三分层：Session（对话窗口）→ State（任务中间态）→ Memory（跨会话持久化）
      ↓
结论：过去半年在Agent上踩的坑都已被整理成模式——不需要重新发明Reflection/Memory分层/通信拓扑
```

### 关键引述

> 书里最狠的判断：大多数人在用的"AI"只是 Level 0——裸 LLM，没有工具、没有记忆、不会行动。你问它 2025 年奥斯卡最佳影片是哪部，它猜。Level 0 的东西，不是 Agent。

> Producer 和 Critic 必须用两个不同的 Agent，给不同的 system prompt。同一个 persona 审自己的东西，一定有盲区。你让同一个 LLM 先写代码再审查自己写的代码，它大概率会说"挺好的"。

> 让 AI 达到最高准确率，必须给它短小、聚焦、有力的上下文。Context Engineering 就是干这件事的。

> 很多人搭 Multi-Agent 花了 80% 的时间在通信协议上，忘了问一个更基本的问题：这个任务真的需要多个 Agent 吗？Level 2 的单 Agent + Reflection 往往已经够用了。

### 局限与盲区

- 本文未覆盖：书中的 21 种设计模式只展开了最核心的几个，其他模式（如 Routing、Orchestrator-Worker、Evaluator-Optimizer）未提及；代码示例的具体实现和框架差异未深入；实际落地中各模式在不同模型上的性能对比数据缺失
- 隐含假设：假设读者有基本 LLM 开发经验，能理解 Agent 调用的成本和延迟权衡；"两个 Agent 比一个好"的前提是 Critic 的审查能力确实优于 Producer 的自我检查，这在某些简单任务上可能不成立
- 可能的反例：简单任务（如固定格式的文本分类）加 Reflection 可能纯浪费 token；轻量场景中 Memory 三层架构可能过度设计；自变形 Agent（第五个假设）在当前技术成熟度下可能更多是概念推演而非近期可落地

---

## 关联

- [[Agent开发十大核心概念]]
- [[多Agent分工协作]]
- [[AgentHarness架构]]
- [[世界级Agentic工程师方法论]]
- [[企业级Agent构建指南]]
- [[Agent最简实现原理]]
