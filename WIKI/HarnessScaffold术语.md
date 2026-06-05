---
Belongs to: "[[Agent工程]]"
aliases: ["Agent术语表", "Harness vs Scaffold", "Agent定义"]
tags: ["AI Agent", "术语定义", "Harness Engineering", "RL训练", "Agent架构"]
created: 2026-05-24
source: ai-generated
source_url: "https://huggingface.co/blog/agent-glossary"
concepts: ["Harness执行层", "Scaffold行为定义层", "Agent = Model + Harness", "Policy行为策略", "Sub-agent递归代理", "Context Engineering上下文设计", "RL训练管线（Rollout/Reward/Trainer）"]
confidence: high
---
# HarnessScaffold术语

> [!abstract]- AI 摘要
> Hugging Face 发布的 AI Agent 术语表，精确定义了 Model、Scaffold、Harness、Agent、Context Engineering、Policy、Tool Use、Skills、Sub-agents 及 RL 训练相关术语。核心公式：Agent = Model + Harness。Scaffold 是行为定义层（system prompt、工具描述、上下文管理），Harness 是执行层（调用模型、处理工具调用、决定何时停止）。

---

## 扫读

> [!tip] 💡 一句话
> 当有人说"Agent = Model + Harness"时，意思是如果你不是模型，你就是 Harness——Scaffold 是模型看到的规则，Harness 是让它运转的引擎。

> [!important] 📌 关键结论
> - Agent 领域术语混乱的根本原因是不同框架对同一词的使用不同——目标不是强制统一词汇，而是提供一个实用的心智模型让讨论可进行
> - Scaffold vs Harness 的最清晰区分：Scaffold 是"模型看到的"（system prompt、工具描述、上下文格式），Harness 是"让 Agent 跑起来的东西"（调用模型、处理工具调用、停止条件）
> - RL 训练四个关键术语：RL Environment（交互环境）、Trainer（使模型变好的引擎）、Rollout（一次完整 Agent 运行轨迹）、Reward（评分信号，可以是可验证的、学习的、稀疏或密集的）

> [!quote] 🎬 可行动项
> - 下次阅读 Agent 相关文章时用本术语表做对照翻译，避免因术语歧义误解作者意图
> - 在自己团队内部约定一套术语使用规范（参考本文的"实用心智模型"而非"强制统一词汇"原则）

---

## 精读

### 论证链

```
Agent领域术语混乱：同一概念被不同框架重新命名，或同一词用于不同含义
      ↓
需要一套实用心智模型（不是强制统一词汇，而是让讨论可进行）
      ↓
核心区分链：
  Model：LLM本身，输入文本输出文本，无记忆无循环
  Scaffold：行为定义层 — system prompt、工具描述、输出格式解析、跨步骤记忆管理
  Harness：执行层 — 调用模型、处理工具调用、决定停止条件
  Agent = Model + Scaffold + Harness（社区简化公式：Agent = Model + Harness）
      ↓
衍生概念：
  Context Engineering = 设计什么进入上下文窗口（四层：system prompt、外部数据、隐式数据、反馈回路）
  Policy = 行为策略（部分在模型权重中学习，部分由Scaffold/Harness决定）
  Skill = 可复用的知识包（vs Tool = 单一动作，Sub-agent = 可推理可调用工具的独立Agent）
      ↓
RL训练专属术语：Environment → Rollout → Reward → Trainer（构成完整训练管线）
      ↓
结论：Scaffold/Harness的区分在训练管线中最重要（需要分别推理），在日常使用中"Harness"常被宽泛用作"除模型外的一切"
```

### 关键引述

> The model is the LLM: it takes text in and produces text out. On its own, it has no memory between calls, and no loop. The model can express the intent to call a tool, but it needs a harness to actually execute it. Wrap it in scaffolding and a harness and it becomes an agent.

> Scaffolding is the behavior-defining layer around the model: system prompt, tool descriptions, how the model's responses get parsed, what it remembers across steps. It shapes how the model sees the world and acts in it. Harness is the execution layer inside the agent: it calls the model, handles its tool calls, decides when to stop.

> Agent = Model + Harness. If you're not the model, you're the harness. Two products using the same underlying model can feel completely different because their harnesses make different choices.

### 局限与盲区

- 本文未覆盖：虽然是术语表但省略了一些常见概念（如 Guardrails、Human-in-the-Loop、Agent-as-a-Service 等）；Skill 与 Tool 的区分在不同框架中确实不同，本文的标准定义不一定能在所有框架中适用
- 隐含假设：假设读者对 RL 有基础了解（Rollout/Trajectory/Trace 的区分对 RL 新手可能仍然模糊）；"如果感觉不精确欢迎反馈"的声明表明作者自己也承认这些定义仍在演化
- 可能的反例：某些框架（如 AutoGPT）中 Scaffold 和 Harness 的边界极为模糊，实际代码中两者交叠严重；"Agent = Model + Harness"的简化公式忽略了 Scaffold 的独立价值，在讨论 prompt 设计时可能产生误导

---

## 关联

- [[Harness工程控制论]]
- [[AgentHarness架构]]
- [[Agent开发十大核心概念]]
- [[Agent最简实现原理]]
- [[AIFirst组织架构]]
