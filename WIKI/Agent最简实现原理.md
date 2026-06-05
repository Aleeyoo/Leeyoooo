---
Belongs to: "[[Agent工程]]"
aliases: ["ReAct循环", "Agent拆解", "Simple-ReAct-Agent"]
tags:
  - AI Agent
  - Agent架构
  - 技术祛魅
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/GoSailGlobal/status/2056395622111170753"
concepts:
  - ReAct循环
  - 工具暴露面
  - prompt caching
  - context window膨胀
  - 函数指针tool
  - 自我纠错
  - 历史拼接memory
confidence: high
---

# Agent最简实现原理

> [!abstract]- AI 摘要
> 一个 Agent 的本质就是 300 行代码的 while 循环——把 Thought/Action/Action Input 拼进 prompt，执行工具调用，结果回传历史，如此往复。框架包装的概念（memory、tool、planning、self-correction）在这个最简实现面前全部祛魅。

---

## 扫读

> [!tip] 💡 一句话
> AI Agent 的核心就是一个 while 循环——把当前历史+任务+工具列表拼成 prompt 喂给模型，让模型输出 Thought/Action/Action Input，执行 Action 把结果拼回历史，进入下一轮。300 行代码足以实现。

> [!important] 📌 关键结论
> - Agent 被框架包装得很玄的概念在 300 行代码里全部祛魅：memory 就是历史拼接，tool 就是 JSON schema + 函数指针，planning 就是让模型在 Thought 里写下一步，self-correction 就是把错误结果也拼回历史
> - Agent 的真正风险不在模型本身，而在"工具暴露面"——给 Agent 接了 shell.exec 就等于把 rm -rf 交给了模型，Agent 自己 commit API key 进仓库这类事故全来自这一层被低估
> - context window 的结构性消耗被忽视：10 步循环就要把同一段 prompt 送进模型 10 次，prompt caching 能省一些但省不掉结构性消耗，而让用户少烧 token 不在 provider 的商业利益里

> [!quote] 🎬 可行动项
> - 读完 Simple-ReAct-Agent 的 300 行源码，对照理解每个框架概念到底在做什么
> - 审计你使用的 Agent 工具暴露面：它能接触什么文件系统？能执行什么命令？能访问什么网络？
> - 关注 Agent 的 context 里装了什么——模型答错时损失会从哪一处扩散

---

## 精读

### 论证链

![[agent-simple-react-demo1.jpg]]

```
Agent 框架层的概念膨胀：
  memory / tool / planning / self-correction
  LangChain, LlamaIndex, AutoGen 等框架层层包装
        ↓
Simple-ReAct-Agent 300 行拆解：
  while 循环 3 件事：
    ① 把当前历史 + 任务 + 工具列表拼成 prompt 喂给模型
    ② 模型输出 Thought / Action / Action Input
    ③ 执行 Action，把结果拼回历史，进下一轮
        ↓
祛魅结论：
  memory = 历史拼接
  tool = JSON schema + 函数指针
  planning = 让模型在 Thought 里写下一步该干什么
  self-correction = 把错误结果也拼回历史，让模型自己看到然后改
        ↓
两个被忽视的关键风险：
  ① 工具暴露面：Action 可以是任何东西，接了 shell.exec 就是把系统控制权交给模型
  ② context window 结构性消耗：每轮重发整段历史，prompt caching 省不掉根本问题
        ↓
新视角：看每个 AI 助手时关注 context 里装着什么、工具能碰到什么、错误会从哪扩散
```

### 关键引述

> Action 可以是任何东西。你给 agent 接了 shell.exec，就等于把 rm -rf 交给了模型。最近几条新闻全是这么来的——agent 自己 commit 把 API key 写进了仓库，agent 自己 npm publish 把 source.zip 推上去，agent 跑了一个不该跑的 shell 命令，把机器删了。

> 让你少烧 token 这件事，不在 provider 的商业利益里。

> context window 每一轮都把整段历史重发一次。10 步循环，同一段 prompt 送进模型 10 次。prompt caching 能省一些，但省不掉结构性消耗。

### 局限与盲区

- **本文未覆盖**：Simple-ReAct-Agent 在多步复杂任务上的实际成功率；与其他 Agent 框架（LangChain、CrewAI）的横向性能对比；不同模型（GPT、Claude、Gemini）对 ReAct 循环的兼容性差异；生产环境中工具暴露面的具体安全方案设计
- **隐含假设**：模型能正确理解 JSON schema 格式的工具定义；ReAct 循环总能收敛到正确结果；用户有足够的技术能力审计工具暴露面
- **可能的反例**：复杂任务（超过 20 步）可能需要更复杂的规划机制而非简单 ReAct 循环；某些场景下框架封装（如 LangChain 的工具管理器）确实能减少事故；对于非技术用户，300 行自建 agent 的实践门槛仍然很高

---

## 关联

- [[AI商业]]
- [[Agent架构三省六部反思]]
- [[企业级Agent构建指南]]
- [[多Agent团队协作]]
