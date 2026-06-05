---
Belongs to: "[[Agent工程]]"
aliases: ["pi-goal评测", "Agent基准测试", "模型对比"]
tags:
  - AI Agent
  - 模型评测
  - 长程任务
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/wquguru/status/2057852569054278045"
concepts:
  - 长程自动循环
  - thinking深度幻觉
  - 软预算探针
  - prompt caching优势
  - audit盲区
  - agent基准缺陷
confidence: high
---

# AI Agent长程任务评测

> [!abstract]- AI 摘要
> 对 pi-goal Agent 框架进行四路模型实测（Gemini 3.5 Flash / Sonnet 4.6 / DeepSeek V4 Pro × 2），发现更贵 ≠ 更好，更深推理反而放大幻觉，且当前 benchmark 测不出 agent 的真正核心能力。

---

## 扫读

> [!tip] 💡 一句话
> 价格标签和模型命名（Flash/Pro）在 Agent 任务中具有欺骗性——DeepSeek V4 Pro 比 Gemini 3.5 Flash 便宜 31 倍且质量更高，而 max thinking 买到的洞察深度以幻觉风险为代价。

> [!important] 📌 关键结论
> - DeepSeek V4 Pro（high thinking）在「读多源+综合+自审」任务上质量分最高、成本最低（$0.072），Gemini Flash 反而贵 31 倍（$2.26）
> - max thinking 比 high 多产出了新模式和新洞察，但也凭空捏造了 2 条 commit 语义——深度推理放大了「叙事连贯性压倒事实精度」
> - pi-goal 的 soft budget 是意外的「模型行为探针」：超预算后不同模型的行为（拼命跑 vs 老实收尾）暴露了其 agentic 倾向

> [!quote] 🎬 可行动项
> - 选模型先看定价表而非命名——「Flash」不一定比「Pro」便宜
> - thinking 档位不是越高越好：长程任务用 high 档平衡深度与幻觉，max 档适合需要洞察但对准确性容忍度高的场景
> - pi-goal 工作流天然适合 prompt caching（每轮重发目标+历史 → prefix 稳定 → cache hit 97.8%）

---

## 精读

### 论证链

```
pi-goal 核心机制（467行 index.ts）：
  状态机 (active/paused/budget_limited/complete)
  → agent_end 事件自动排队下一轮
  → continuationPrompt 每轮注入硬约束（audit checklist）
  → budget_limited 是软提示而非硬切断
        ↓
四路模型同任务对比（clone 12 个 Karpathy 仓 → 读源码+git历史 → 写带SHA引用的洞察报告 → 自验所有引用）
        ↓
反直觉发现：
  ① Gemini Flash 比 DeepSeek Pro 贵 3-10 倍/token（"Flash"名字有欺骗性）
  ② max thinking 多了洞察也多了幻觉（2条捏造commit语义）
  ③ 接入是真门槛（DeepSeek reasoning_content 回传协议多数router不兼容）
  ④ soft budget 暴露模型agentic性格
  ⑤ audit 盲区：只验SHA可达，不验commit message语义匹配
  ⑥ 这个benchmark本质是RAG+写作，测不到真正agent核心
        ↓
核心结论：token价格暴跌领先agent harness生态，选型需要实测而非看名
```

### 关键引述

> 按价格，Gemini 3.5 "Flash" 反而比 DeepSeek V4 "Pro" 贵。同一个任务跑下来，Gemini $2.26, DeepSeek V4 Pro $0.072——31 倍差距，而且 DeepSeek 的质量分还更高。

> 把 DeepSeek 的 thinking 从 high 调到 max，结果反而变差了。深度推理放大了「叙事连贯性压倒事实精度」这个经典失败模式。

> pi-goal 这种「每轮重发目标+历史」的工作流，在 prompt caching 面前不是劣势，是优势：重发越多、prefix 越稳定，命中率越高。

> 这个 benchmark 测不到 agent 的核心。它测的任务做错了会不会有人/系统受影响、回滚贵不贵、有没有时间压力？如果没有，那它测的就只是「长文写作+检索」，不是 agent 核心。

### 局限与盲区

- **本文未覆盖**：pi-goal 在真实生产任务（改bug/提PR/等CI）上的表现；其他 Agent 框架（AutoGPT、CrewAI、LangGraph）的横向对比；Gemini 3.5 Flash 之外 Google 模型的表现；不同 prompt 设计对 audit 盲区的影响
- **隐含假设**：200K token 预算对多数长程任务足够；用户有接入多个模型 API 的技术能力和预算；pi-goal 的 audit 机制在模型愿意配合时才会生效
- **可能的反例**：某些任务上 max thinking 的多余洞察可能恰好有价值（创意类任务）；不同模型提供商可能调整定价和性能；Sonnet 跑在 Claude Code（不同harness）而非 Pi 上，对比不完全公平
- **关键漏洞**：作者自己也指出「Sonnet 跑在 Claude Code 上不是同一个 harness」——不同 agent harness 的对比较果可能被框架差异污染

---

## 关联

- [[AI商业]]
- [[自建AI API中转站]]
- [[AI产品赚钱悖论]]
