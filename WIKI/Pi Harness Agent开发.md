---
Belongs to: "[[Agent工程]]"
aliases: ["pi-docs-playbook", "pi harness 文档攻略", "Agent底座开发", "从pi上长出来Agent"]
tags: ["pi", "Agent开发", "harness", "文档工程", "Coding Agent", "上下文管理"]
created: 2026-06-03
source: ai-generated
source_url: "https://x.com/0xenderzcx/article/2061778310934516097"
concepts: ["pi harness底座", "文档harness", "task-reading-matrix", "AGENTS.md铁律", "coding agent上下文投喂", "tool wrapper设计", "session trace与audit区分", "approval UX搭建"]
confidence: medium
---

# Pi Harness Agent开发

> [!tip] 💡 一句话核心主张
> 别急着写 Agent 代码，先把 pi 的文档以结构化方式喂给你的 coding agent——让它按任务矩阵精准读取对应文档章节，而不是凭记忆幻觉去"实现业务"，这比你选的模型版本更重要。

> [!important] 📌 关键结论
> - 做一个可靠、稳定、可验证的业务 Agent，不能只靠封装一个大模型，必须给它装上底座（harness）、轮子（tools）、手脚（actions）和验证机制（audit），pi 正是这样一个 harness 底座。
> - pi-docs-playbook 的核心创新不是文档归档，而是"文档 harness"：通过 task-reading-matrix 将真实开发场景映射到该阅读的具体文档文件，让 coding agent 按图索骥而非随机检索。
> - AGENTS.md 给 coding agent 设定铁律：必须先读 README + task-reading-matrix，按问题类型精准阅读 source/ 中的原始文档，回答时必须引用本地路径，禁止凭记忆猜测 pi 行为——这是对抗幻觉的工程手段。

> [!quote] 🎬 可行动项
> - 直接 clone pi-docs-playbook 仓库，将整个 repo 加 PROMPT.md 中的 prompt 一起丢给你的 coding agent（Codex / Claude / Cursor）。
> - 让 Agent 先读 AGENTS.md 和 usage/task-reading-matrix.md，再告诉它你的业务需求：想做什么 Agent、核心业务动作是什么、哪些操作需要强验证、channel 是什么。
> - 不要急着写代码，先等 coding agent 按矩阵问完关键决策点：哪些操作必须实时查 DB、审批门控怎么切入、业务状态是否应该在 harness 中显式建模、channel 适配放哪一层。
> - 人类自己可以通过 catalog/ 分类和 usage/how-to-use-this-repo.md 快速定位到该读的文档部分。

### 论证链

```
**起点：封装一个大模型不等于业务 Agent**

作者接手了一个"把大模型封装一下"的业务需求，很快发现：要让 AI 真正帮你做业务、查单、提效率，不能只靠一个大模型。你还需要底座（harness）、轮子（tools）、手脚（actions）和验证机制（audit）。Chatbot 和可靠业务系统之间的差距，就是 harness 层需要填补的空白。
```
### 核心方法：让 coding agent 按文档矩阵精准阅读

pi-docs-playbook 不是又一个 pi 教程或 fork，而是一个"文档 harness"。它的组织逻辑是：将 pi 官方文档按用途重新分类（catalog/），然后建立一份 task-reading-matrix（usage/task-reading-matrix.md），将真实开发场景映射到最相关的文档文件。coding agent 拿到这个 repo 后，不再需要遍历全部文档，而是按场景精准定位。

### AGENTS.md：给 coding agent 的铁律

AGENTS.md 设定了三条规则：进入后必须先读 README + task-reading-matrix；按问题类型精准阅读 source/ 里的原始文档；回答时必须引用本地 source/ 路径，禁止凭记忆猜测 pi 行为。这是对抗 coding agent 上下文幻觉的工程化手段——不是让模型"更聪明"，而是约束它的行为边界。

### 关键决策点清单

coding agent 在阅读矩阵后会主动向开发者提问一系列业务关键决策点：哪些操作必须实时查 DB（vs 用缓存）？审批门控（approval gating）怎么切入？业务状态要不要显式建模在 harness 里？channel 适配放哪一层？tool wrapper 和 audit 映射怎么设计？这些问题是 harness 设计的核心骨架，忽略任何一个都会导致后期返工。

### 仓库结构：source / catalog / usage 三层分离

repo 采用三层结构：source/（原样镜像官方 Markdown，保留上游路径，支持离线精确引用）、catalog/（按用途重新分类，如 core runtime、official coding-agent docs、examples、validation）、usage/（核心的是 task-reading-matrix.md，按场景告诉 agent 读哪些文件）。外加 PROMPT.md 提供可直接复制的 prompt。

## 关键引述

> "要想真正让 AI 帮你做业务、查单、提效率，不能只靠一个大模型。你还需要给它装上底座、轮子、手脚，以及验证机制。"

> "重点不是 pi 教程，而是如何学习 pi，如何把你的业务嫁接上去。我专门建了一个 repo，把 pi 官方文档整理归档、分类清楚，告诉你和你的 coding agent 应该怎么读、读哪些、什么时候读。"

> "不要急着写代码。先把 pi 的文档喂对，再开始设计你的 harness。否则你配的 Agent 只会用上下文幻觉去'实现业务'，跑起来才发现到处都是坑。"

> "AGENTS.md：给 coding agent 的铁律。进来必须先读 README + task-reading-matrix，按问题类型精准阅读 source/ 里的原始文档，回答时必须引用本地 source/ 路径，禁止凭记忆猜测 pi 行为。"

## 局限与盲区

- **未覆盖方面**：文章聚焦于 pi harness 的文档投喂策略，未讨论当 coding agent 读完文档后仍然产生幻觉或错误实现时的兜底方案。也未涉及实际操作中的验证流程——Agent 根据矩阵建议的 harness 设计如何被人类验证其正确性？repo 承诺"持续加真实踩坑后的 matrix"，但当前版本覆盖了多少边界场景未量化。
- **隐含假设**：假设 coding agent 能够有效理解并严格遵守 AGENTS.md 中的规则——但当前 coding agent 的实际行为表明，长上下文中的规则遵循仍不稳定，尤其是在复杂多步任务中。假设文档足够准确且完整，足以消除幻觉——但文档本身可能有过时或遗漏，coding agent 未必能区分"文档没说"和"不存在"。
- **可能的反例**：如果 pi 本身的文档质量不足以支撑精准问答（比如某些边缘行为没有文档化），那么再好的 matrix 也只是把 coding agent 指向了不完整的信息源。对于已经有大量 pi 实践经验的团队，这套玩法可能带来过度的流程开销——他们已经在踩坑中形成了自己的知识。
- **生态限定**：本文完全基于 pi harness 生态，不适用于选择其他 Agent 框架（如 LangGraph、CrewAI、AutoGen）的团队。方法论的通用部分（结构化文档投喂、task-reading-matrix）可以迁移，但具体 source/ 路径和 catalog/ 分类是 pi 专属的。

## 关联

- [[软件构建即学习]]——pi harness 业务嫁接的过程本质是学习过程，快速验证反馈循环同样适用
- [[离职工程师技能蒸馏]]——pi-docs-playbook 将文档知识喂给 Agent 的思路，与 dot-skill 将工程师经验喂给 Agent 的逻辑本质相同，都是对抗"人走知识走"或"文档不用知识就死"
- [[Claude Code动态工作流]]——pi harness 提供 Agent 运行时底座，Dynamic Workflow 提供编排层，两者在架构上互补
- 所属项目：[[Agent工程]]
