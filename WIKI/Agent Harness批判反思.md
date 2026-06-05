---
Belongs to: "[[Agent工程]]"
aliases: ["Harness经济学批判", "Agent成本反思", "Harness is Rich Man's Toy"]
tags: ["harness-critique", "agent-economics", "agent-cost"]
created: 2026-06-02
source: ai-generated
source_url: "https://x.com/bythyag/article/2061335891910721929"
concepts: ["harness economics", "context efficiency", "agent bloat", "cost sustainability", "harness as rich man's toy", "simpler alternatives"]
confidence: medium
---
# Agent Harness批判反思

> [!abstract]- AI 摘要
> 对当前 Harness 工程热潮的质疑：一个项目是否具有经济价值不在于它有多漂亮的架构，而在于它的产出能否被用于高风险任务而无需人工干预。当前 Harness 生态倾向于过度工程化（bloat），将简单问题复杂化以获取 VC 青睐，但真正的效率来自对问题本质的理解，而非堆叠基础设施。

---

## 扫读

> [!tip] 💡 一句话
> Harness 看起来诱人——但在烧了 VC 的钱产出漂亮的 demo 之后，它是否真的产生了经济价值？大多数 Agent 项目从"好的 Agent 工程"开始，最终滑向"豪华但无用的软件工程"。

> [!important] 📌 关键结论
> - 当前 Harness 生态存在三个结构性瓶颈：(1) 上下文效率低下——行业默认"烧 token 比优化效率更容易"，O(n) 还是 O(n^2) 没人关心；(2) 功能膨胀——大多数 feature 对 power user 都无实际价值，只是看起来 fancy；(3) 成本不可持续——API 价格下降不改变计算资源有限的根本约束
> - 编码 Agent 的问题相对简单——文件锁、竞态、grep 定位、修复——但知识工作任务完全不同：上下文质量、工具输出精度、用户意图、系统 prompt 大小都直接影响经济价值
> - 一个反直觉判断：很多 embedding、retrieval、chunking 问题可以用经典模型（RoBERTa）、微调、或数学方法（reranking）解决，但这样做"看起来不够难"，因此没有人为此买单——行业存在系统性激励偏差，偏好复杂方案而非有效方案

> [!quote] 🎬 可行动项
> - 在开始任何 Agent Harness 项目之前先问：产出能否在高风险任务中无需人工干预即可使用？如果不能——当前是玩具还是产品？
> - 审计 Agent 的 token 效率：每条 tool output 是否必要？系统 prompt 是否可以压缩 40%？用更简单的模型（而非最强模型）处理可结构化的子任务
> - 在引入新的 Harness 组件之前，先检查是否能用更简单的方式解决：微调的小模型、经典 NLP 管线、或数学方法

---

## 精读

### 论证链

```
历史回顾：LangChain → LangGraph → Claude Code → OpenClaw/Hermes
  每波工具都以新范式出现，但基础问题未变——状态、记忆、持久化仍是核心挑战
        ↓
核心批判：Coding Agent 和知识工作 Agent 有本质差异
  Coding：工具跑对、文件锁好、grep 定位、修复即可→输出对错易验证
  知识工作：上下文质量敏感、工具输出精度要求高、用户意图关键、结果验证主观
  → 当前 Harness 将两者等同，用同一套工程范式处理
        ↓
三个结构性瓶颈：
  ① 上下文效率：烧 token > 优化效率是行业默认态度
     O(n) vs O(n^2) token 消耗——只要结果出来就没人关心
     核心矛盾：让你少烧 token 不在 provider 的商业利益里
  ② 功能膨胀（bloat）：
     项目起点是好的 Agent 工程 → 逐渐滑向软件工程
     多数 feature 对 power user 都无价值，只是看起来 fancy
     更关键：这剥夺了算力有限的人使用你的软件的权利
     后果：Claude Code/Codex 用更好的工程吸收你的 feature → 你出局
  ③ 成本不可持续：
     API 价格在降但不会永远降——计算能力是有限的
     商业模式：烧 VC 钱买 MRR → 悄悄涨价
     没有免费午餐：compute 有限，runway 有限
        ↓
RAG 案例研究（过度工程的典型）：
  行业在 embedding、retrieval、chunking 上投入了海量基础设施
  但很多问题可以用 RoBERTa（经典模型）、微调、或数学 reranking 解决
  "如果你真的懂你的文档，结构化抽取可以便宜得多"
  问题：简单方案没人付钱——因为"问题看起来不够大了"
        ↓
反思：为什么没人做简单方案？
  简单方案 → 问题不再是山 → VC 不投 → 无法获利
  系统激励偏向复杂方案，即使简单方案更有效、更便宜、更可持续
        ↓
作者立场：没有答案只有直觉——"trust me bro"——但相信当前路径不可持续
```

### 关键引述

> "Every project idea might start with good agent engineering but eventually drifts towards software engineering. You might be building a beautiful piece of software but if it isn't economically valuable, what is the point?"

> "That's why I call harness a rich man's toy. It is fun to play with, seeing yourself burning $100 on simple tasks you could have done yourself, but then hey, my agent did that."

> "Burning tokens is easier than making something efficient. O(n) or O(n^2), who cares - I got the work done. This is the current approach of the industry."

> "If you know your game, things work out cheaply and efficiently. You don't need a million dollar infra and $3M in API credits."

> "There are simpler ways for solving hard problems but then nobody will pay you for that because you present a simple solution and the problem isn't looking like a mountain anymore."

### 局限与盲区

- 本文未覆盖：缺乏具体的数据或案例来量化"过度工程的成本 vs 简单方案的成本"；对"简单方案"的具体技术路径（RoBERTa、微调、数学 reranking）仅点到为止，未提供实现参考；未讨论在必须使用 LLM 的强推理场景中，Harness 的效率优化具体如何做
- 隐含假设：知识工作任务的复杂性与编码任务有明确边界——实际上很多知识工作任务也涉及编码（如数据分析 Agent 既需要理解业务逻辑也需要写查询）且两类问题的工程方案存在交叉；假设 VC 驱动的创业公司代表了整个行业，但企业内部 Agent 项目有不同的经济模型和激励机制
- 可能的反例：Anthropic 和 OpenAI 的 Harness 工程实践确实产出了可验证的生产力提升（百万行代码、数月持续交付），这些不是"rich man's toy"而是真实的工程产出；某些场景下简单模型确实无法替代 LLM 的推理能力（如跨领域的复杂分析）；token 优化和功能精简的前提是对问题空间有清晰认知——这本身就需要通过"过度工程"的探索阶段来积累

---

## 关联

- [[AgentHarness架构]]
- [[Harness工程控制论]]
- [[Agent最简实现原理]]
- [[编排税]]
- [[编排税即你]]
- [[世界级Agentic工程师方法论]]
- [[提示词工程九原则]]
- [[Agent项目落地难]]
