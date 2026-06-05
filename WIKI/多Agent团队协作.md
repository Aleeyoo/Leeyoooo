---
Belongs to: "[[Agent工程]]"
aliases: ["Multi-Agent", "Agent编排", "Agent协作模式"]
tags:
  - AI Agent
  - 多Agent系统
  - Agent架构
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/eng_khairallah1/status/2055215784092401966"
concepts:
  - Pipeline模式
  - Fan-Out模式
  - Specialist Team模式
  - Dreaming记忆机制
  - 输出格式标准化
  - outcomes rubric
  - Agent handoff
confidence: high
---

# 多Agent团队协作

> [!abstract]- AI 摘要
> 单 Agent 是厉害的员工，多 Agent 团队是完全不同的品类。Anthropic 在 2026 年 5 月推出 Managed Agents 多 Agent 编排，支持最多 20 个专业 Agent 并行工作。Netflix、Harvey、Shopify 已在生产环境运行多 Agent 系统，本篇拆解三种有效协作模式和完整构建步骤。

---

## 扫读

> [!tip] 💡 一句话
> 多 Agent 系统的核心优势不是速度（虽然能快 5 倍），而是专业化——每个 Agent 只做一件事且做到极致，就像人类团队优于单个人处理复杂项目。

> [!important] 📌 关键结论
> - 三种验证有效的协作模式：Pipeline（顺序传递）、Fan-Out（一对多并行分发）、Specialist Team（多专家协同），根据任务特征选择而非混用
> - 输出格式标准化是多 Agent 系统中最重要的技术决策——如果 Agent A 输出自由文本而 Agent B 期望结构化 JSON，交接就会断裂
> - Dreaming（后台定期回顾+提取模式+管理记忆）让 Agent 团队越用越强，Harvey 报告启用后完成率提升约 6 倍

> [!quote] 🎬 可行动项
> - 从 2 个 Agent 的简单 Pipeline 开始，不要一上来就设计 10 个 Agent 的系统——先跑通通信和输出格式
> - 定义每个 Agent 的三要素：清晰角色、具体工具、结构化输出格式
> - 为 Agent 交接点设计显式 fallback 行为：如果某个 Agent 拿不到数据，不要崩溃，记录失败并继续

---

## 精读

### 论证链

![[multi-agent-cover.jpg]]

```
为什么多 Agent 优于单 Agent：
  速度：5 个 Agent 并行 = 30 分钟任务变为 6 分钟
  质量：专业化 Agent 各司其职 vs 单 Agent 注意力分散导致全盘平庸
  类比：人类团队 vs 个人——同样的原因
        ↓
三种有效模式：
  ① Pipeline: Research→Analysis→Writing→Review，适合步骤间有清晰输入输出
  ② Fan-Out: Commander 拆任务→分发到 N 个 Worker 并行执行→收集合成
      Netflix 用此模式分析构建日志
  ③ Specialist Team: 多种专长 Agent 协同一个复杂任务，各贡献专业能力
      Harvey 用此模式处理法律工作
        ↓
构建步骤：
  Step 1: 定义团队（目标→子任务→哪些可并行→需要什么专长）
  Step 2: 设计每个 Agent（角色+工具+定义输出格式）← 输出格式是最重要的技术决策
  Step 3: 构建编排（哪些并行/哪些串行/如何传数据/失败如何fallback）
  Step 4: 添加 Dreaming（后台定期提取模式→管理记忆→越用越强）
  Step 5: 定义 Outcomes（rubric制质量评估→自我迭代直到达标）
  Step 6: 从小开始测试（2 Agent → 3 Agent → 逐步扩展）
  Step 7: 监控和迭代（handoff失败/重复工作/质量下降/token膨胀）
        ↓
五大常见错误：
  ① Agent 太通用（失去了多 Agent 的意义）
  ② 输出格式不标准化（交接断裂）
  ③ 过早并行太多 Agent
  ④ 没有 Agent 间错误处理
  ⑤ 忽略 token 成本
```

### 关键引述

> A single agent is like a single employee. No matter how talented they are, they can only do one thing at a time. A multi-agent system is like a team. Five agents, each specialized in one part of the task, working simultaneously.

> Standardize your output formats across agents. This is the most important technical decision you will make.

> Harvey reported that enabling Dreaming on their legal agents increased completion rates approximately 6x. Not from a model change — purely from agents carrying institutional knowledge across sessions.

> Multi-agent systems are not magic. They are software engineering applied to AI. The fundamentals are the same as building any team-based system: clear roles, clear communication, defined interfaces, error handling, and iteration.

### 局限与盲区

- **本文未覆盖**：多 Agent 系统的 token 成本量化分析（相比单 Agent 具体贵多少）；Agent 间冲突解决机制（两个 Agent 对同一问题给出矛盾结论时如何处理）；开源框架（AutoGen、CrewAI）与 Anthropic Managed Agents 的对比；大规模 Agent 团队（>20 个）的编排挑战
- **隐含假设**：用户使用 Claude Managed Agents API；任务能被清晰分解为独立子任务；Dreaming 机制在所有场景下都能可靠提升性能
- **可能的反例**：简单任务用多 Agent 反而增加复杂度和成本（杀鸡用牛刀）；某些创意工作不适合 Fan-Out 并行（需要全局一致性）；Dreaming 提取的错误模式可能固化偏见而非纠正错误

---

## 关联

- [[AI商业]]
- [[企业级Agent构建指南]]
- [[Agent架构三省六部反思]]
- [[Agent最简实现原理]]
