---
Belongs to: "[[Agent工程]]"
aliases: ["MemOS记忆方案", "Reflect2Evolve", "Agent长期记忆", "MemOS Local Plugin"]
tags: ["agent-memory", "memos", "reflect2evolve", "skill-crystallization", "context-engineering"]
created: 2026-06-06
source: ai-generated
source_url: "https://x.com/yanhua1010/article/2062345653079224458"
concepts: ["reflect2evolve", "trace-to-policy-to-skill", "active-vs-passive-memory", "cross-session-learning", "memory-as-learning-not-storage", "multi-model-cost-optimization", "skill-version-evolution"]
confidence: medium
---
# MemOS Reflect2Evolve记忆

> [!abstract]- AI 摘要
> MemOS 突破了现有 Agent 记忆方案"存旧聊天"的局限，通过 Reflect2Evolve 机制从对话中自动提炼可复用的行为策略：trace（事件轨迹）→ policy（行为规则）→ skill（可调用技能）。实测表明，高频任务的写作风格和工程习惯可跨 session 自动迁移，但低频偏好（如设计风格）仍需多轮积累。核心理念：记忆不是存你说过什么，而是从你做事的过程里学出下次怎么做。

---

## 扫读

> [!tip] 💡 一句话
> 现有记忆方案（ChatGPT memory、Claude project knowledge）只解决了"存"的问题，没解决"学"的问题——MemOS 的 Reflect2Evolve 从对话中自动提炼 trace→policy→skill 三级进化链，让 Agent 不只记住你说过不用破折号，而是学会你的写作策略并在新 session 中自动应用。

> [!important] 📌 关键结论
> - 现有记忆方案的三个根本缺陷：(1) 存的是原始对话，信噪比低——三个月前的闲扯被检索回来模型自己判断哪些有用；(2) 记忆扁平——所有信息平等存储，无"这条比那条重要""这个结论已更新"的层次；(3) 不会从错误中学习——有十条纠正记录但没有归纳出"以后别再这样做"的策略
> - MemOS 核心机制 Reflect2Evolve：trace（事件轨迹）→ policy（行为策略）→ skill（可调用技能）。不是"记住你说过什么"，是"从你做事的过程里提炼出下次怎么做"。成熟流程会结晶为带版本号的 skill（如 check_obsidian_vault_path_env V1→V2）
> - 双轨记忆架构：内置 memory = 用户主动告知的显性记事贴（名字/角色/偏好/规则，存本地 JSON）；MemOS = 后台自动学习积累的隐形记忆（从对话中自动提取 trace→policy→skill）
> - 实测效果：写作风格（零破折号、首句抛观点、类比自然带出）成功跨 session 迁移——Agent 在新 session 中自动加载 skill、搜索记忆、读旧文章摸节奏后产出符合风格的推文；产品页设计风格（暖色 vs 深色）未跨项目迁移——低频偏好需要更多轮的 support 积累才能固化为策略
> - 多模型成本优化：本地 Xenova 做 embedding（免费）、DeepSeek V4 Flash 做摘要（便宜）、DeepSeek V4 Pro 做技能进化（只在需要强推理时调用）——贵的模型只用在刀刃上

> [!quote] 🎬 可行动项
> - 先区分 Agent 的"显性记忆"和"隐性学习"：CLAUDE.md/AGENTS.md 是显性说明书（你手写的规则），需要补充自动从行为中学习的机制
> - 评估当前记忆方案处于哪一级：L1 存对话 → L2 结构化摘要 → L3 提炼策略（Reflect2Evolve 对应 L3）
> - 对于高频重复任务（如固定格式的内容生成），优先考虑记忆自动化——人工维护说明书的成本随规则增多会非线性增长
> - 注意记忆的跨项目隔离——风格偏好不会自动跨项目迁移（这是设计选择而非缺陷），需要全局偏好时可手动标记为共享 policy

---

## 精读

### 论证链

```
问题发现：在 CLAUDE.md 写了 200 多行规则 → Agent 在执行规则但没学会规则
  "Agent 知道不用破折号，但不知道为什么不用，不知道我纠正过它几次"
  → 说明书是死的，Agent 没有从交互历史中学习的能力
        ↓
现有方案分析（ChatGPT memory / Claude project knowledge / 第三方插件）：
  共同做法：存旧对话 → 向量索引 → 检索相关段落塞回 context
  三个缺陷：信噪比低（原始对话噪音多）、记忆扁平（不分重要性/时效性）、不会从错误学习
  → 解决了"存"的问题，没解决"学"的问题
        ↓
MemOS Reflect2Evolve 三级进化链：
  trace（事件轨迹）：从对话中自动提取的事件记录
    ↓ 有价值 trace 被归纳
  policy（行为策略）：如"Convert WeChat article to Xiaohongshu format"（support 25）
    ↓ 成熟 policy 结晶
  skill（可调用技能）：带版本号（V1→V2），support 越高越成熟
        ↓
实测一（高频任务-写作风格）：跨 session 成功迁移
  第一轮：教 Agent 写 X 推文风格（不用破折号、首句抛观点、有活人感）
  → 纠正一次："开头太平了，换更锐利的钩子"
  第二轮（新 session，无任何偏好提醒）：
    Agent 自动：加载 x-content-writing skill → 搜索记忆 → 找 Obsidian vault → 读旧文章摸节奏
    → 产出完全符合风格：零破折号、首句抛观点、后端类比自然带出
        ↓
实测二（低频任务-设计风格）：跨项目未迁移
  第一轮：ReddTrends 产品页（奶油白底+暖色+独立开发者风格）
  第二轮（新 session，MoleUninstaller 产品页，无风格指示）：
    结果走完全不同的方向（深色底+英文标题+橙色强调）
    → 风格偏好只出现一次，support 不够固化为策略——符合设计预期
        ↓
技术架构：
  检索管线：FTS5 全文 + 向量混合 → RRF 融合排序 → MMR 去重 → 14天半衰期时间衰减 → LLM 过滤
  → 复杂度远超 "embedding + cosine similarity"，但检索质量是记忆有用与否的分水岭
  多模型分工：本地 Xenova embedding（免费）+ DeepSeek V4 Flash 摘要（便宜）+ DeepSeek V4 Pro 进化（按需）
  本地运行：所有数据存本机 SQLite，Viewer 只监听本地，零云依赖
  跨 Agent 共用：OpenClaw 和 Hermes 共享同一 Reflect2Evolve 核心，记忆资产不会因切换工具归零
```

### 关键引述

> "说明书是死的：我写下'不用破折号'，Agent 就不用。但它不知道为什么不用，不知道我纠正过它几次，不知道这个规则背后是'我觉得破折号让中文失去节奏感'这个判断。我的 Agent 在执行规则，但它没有学会规则。"

> "内置 memory 是我主动存的显性记事贴，MemOS 是后台自动学习积累的隐形记忆。"

> "不是'记住你说过什么'，是'从你做事的过程里提炼出下次怎么做'。"

> "我当时的反应是：这不对啊，我没让你去读我的旧文章。但它自己判断这件事该做，这个感觉和'帮我搜一下上次聊过什么'完全不同。"

> "我写了半年 CLAUDE.md，越写越觉得这活不该是人干的。"

### 局限与盲区

- 本文未覆盖：MemOS 在不同 LLM Provider（Claude/GPT/Gemini）上的兼容性和效果差异——实测仅基于 Hermes（本地 Agent）；策略提炼的误判风险——从有限交互中过度归纳错误的 behavior pattern，且错误的 policy 可能自我强化；大规模记忆下的检索性能衰减——47 条记忆的表现不代表 4700 条时的效果
- 隐含假设：假设用户的行为模式有足够的重复性值得自动提炼——对于探索性强、重复度低的工作模式，Reflect2Evolve 的收益可能有限；假设 Agent 能正确判断"哪些 trace 值得归纳为 policy"——但这本身是一个需要元判断的难题
- 可能的反例：对需要严格遵守不断更新的外部规则的任务（如法规合规），自动从历史行为中学习的策略可能与最新规则冲突——此时"死说明书"反而更安全；多用户共享的 Agent 场景中，从一个用户学习到的 policy 可能对其他用户不适用甚至有害；技能结晶的版本升级（V1→V2）在没有人工审核的情况下，升级方向可能偏离用户真实需求

---

## 关联

- [[Agent记忆升级实录]] —— Agent 记忆需求的演变和实践探索
- [[Agent Harness内存现状]] —— Agent 内存管理的技术现状和 Harness 层面的挑战
- [[CLAUDE.md优化规则]] —— 本文的起点问题（CLAUDE.md 越写越多）与该文的优化方法论互补
- [[Agent开发十大核心概念]] —— 记忆是 Agent 架构设计的核心概念之一
- [[Memory即Purpose]] —— 记忆与 Agent 目标/身份的关系的哲学探讨
- [[SkillOpt自进化技能优化]] —— Reflect2Evolve 中 skill 结晶和版本进化与 Skill 自优化的对应
