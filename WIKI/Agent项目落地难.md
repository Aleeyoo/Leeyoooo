---
Belongs to: "[[Agent工程]]"
aliases: ["Agent难以落地", "Agent工程化壁垒"]
tags: ["agent", "harness", "工程化"]
created: 2026-05-31
source: ai-generated
source_url: "https://x.com/anorth_chen/article/2058554497333407927"
concepts: ["Agent Harness", "LLM三层架构", "Harness平台化", "Agent工程边界", "模型兑现率", "基础设施抽象"]
confidence: medium
---
# Agent项目落地难

> [!abstract]- AI 摘要
> Agent 项目难以落地的根本原因不是模型能力不够，而是业务团队80%的精力被耗在了手搓 harness 基础设施上——这层本该由专业平台提供，就像 Web 时代不需要每个团队自建服务器。

---

## 扫读

> [!tip] 💡 一句话
> 模型决定 Agent 的上限，但 harness 决定兑现率——你的 Agent 没做好，和模型能力无关，是 harness 层缺失。

> [!important] 📌 关键结论
> - Agent 工程拆为三层：LLM（大脑）、Agent Harness（身体/神经系统）、Agent（有职业的人）；绝大多数失败项目漏掉了中间层
> - Claude Code 和你 DIY 的 Agent 用的是同一个模型，差距全在 harness：上下文管理、工具 diff 机制、错误恢复、模型路由、隐性 prompt 工程
> - Harness 平台化是必然趋势——如同 Web 从自建服务器到 AWS，Agent 时代需要把基础设施交给平台，业务团队 100% 聚焦业务逻辑

> [!quote] 🎬 可行动项
> - 评估 Agent 项目时先问"我们团队真正擅长什么"——如果答案是懂客户懂业务，就把 harness 交给专业平台
> - 停止自研 retry、sandbox、context compression——这些不是你的护城河

---

## 精读

### 论证链

```
Agent 项目落地难的表象：效果差、上下文爆、工具调用不稳定
      ↓
常见误诊：以为是 prompt 没调好 / RAG 没做好 / 模型不够强
      ↓
真正原因：LLM（大脑）和 Agent（职业人）之间缺了一整层 harness（身体/神经系统）
      ↓
Harness 的工程清单：模型客户端、context 组装、工具系统、执行环境、状态记忆、
调度循环、身份权限、可观测性、安全策略、API 接入层——每一项都是生产级门槛
      ↓
现实对照：Claude Code 赢在 harness——上下文自动组装、diff 补丁机制、错误自动回填、
模型路由、数千字精心打磨的 system prompt
      ↓
结构性矛盾：业务团队的核心能力是业务理解（工单分类、销售漏斗、合规边界），
但80%时间被耗在写 retry、接 monitoring、调 context window 上
      ↓
历史规律：Web 从自建服务器 → AWS/Vercel 平台化；移动互联网同样；
Agent 正处在「web还没有AWS」的阶段
      ↓
解决方案：harness 平台化 → 业务团队100%精力回到业务 →
三层格局（模型层/ harness 平台层/ Agent 应用层）各自分化
      ↓
结论：停止自己造 harness，把注意力放回你最懂的那个业务
```

### 关键引述

> 模型决定上限，harness 决定兑现率。两个 IQ 完全相同的人，一个受过专业训练、有合适工具、有团队配合；另一个赤手空拳。最后能交付的工作完全不在一个量级。

> 让一个 SaaS 公司的产品团队从零搭 harness，约等于让电商团队自己造数据库内核。他们也许真能造出来。但等他们造完，市场窗口已经错过了。

> 你的客户不会因为你"自研了 agent 框架"而多付一分钱。他们只会因为你比别人更懂他们的业务而留下来。

### 局限与盲区

- 本文未覆盖：未讨论不同行业（金融、医疗、法律）对 harness 层的合规性和安全性特殊需求，这些可能是平台化最难统一的部分
- 隐含假设：假设 harness 平台能像 AWS 一样成为通用基础设施——但 Agent 场景高度异构，一个交易 agent 和一个医疗 agent 对工具系统、安全策略的要求可能差异巨大，平台化可能不如文章乐观
- 可能的反例：某些大厂拥有足够工程资源且业务场景极其特殊，自研 harness 可能是合理选择；另有一些团队的核心商业化就是 harness 本身

---

## 关联

- [[AgentHarness架构]]
- [[长任务Agent工程闭环]]
- [[Agent架构三省六部反思]]
- [[编排税即你]]
- [[FDE企业AI接入]]
