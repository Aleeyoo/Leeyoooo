---
Belongs to: "[[AI技术原理]]"
aliases: ["Software After Software", "后软件时代", "代码丰裕时代"]
tags: ["软件工程", "AI范式转移", "Agent", "代码自动化", "工程经济学"]
created: 2026-06-06
source: x-article
source_url: "https://x.com/thorstenball/article/2059304318055150066"
concepts: ["代码稀缺假设瓦解", "工程瓶颈从写代码转向做决策", "委托任务为新工作单元", "软件从为人设计转向为Agent设计", "Just-in-Time软件", "围绕模型重组", "前沿派驻"]
confidence: high
---
# Software After Software

> [!abstract]- AI 摘要
> 软件行业过去 40 年建立在两个假设上——代码难写、代码稀缺。两个假设都已不再成立。价值从代码向数据、权限、分发、信任和合规迁移，赢家是那些围绕模型重组而非试图把 AI 塞进旧流程的人。

---

## 扫读

> [!tip] 💡 一句话
> 当 AI 能以零边际成本生成代码时，软件行业的竞争基础将从"谁能写出好代码"变成"谁有更好的判断力、数据和分发渠道"——代码本身不再值钱。

> [!important] 📌 关键结论
> - 代码写作的本质经济规律已经改变：写作有效代码曾是瓶颈，现在不再是；工程错误（优先级、排序、权衡）才是剩余瓶颈。
> - 工作单元从"要写的代码"转变为"被委托的任务"——Agent 不再是助手侧边栏，它们将被放任长时间自主运行。
> - 软件形态变化：未来软件主要为 Agent 使用而建，更多软件将是即时（just-in-time）生成而非提前构建，"使用软件"和"构建软件"的边界模糊甚至消失。
> - 价值迁移：编码工作流的软件贬值，数据、权限、分发、信任、合规和物理资产升值。
> - 每八周应给模型松绑一次，否则你将卡在低点上。

> [!quote] 🎬 可行动项
> - 在组织中建立一个小型自主"前沿团队"，其目的不是给旧流程加 AI 而是发现新工作方式。
> - 审视现有流程中哪些是"代码稀缺时代"的遗产（排期、交接、审查），问自己：为什么花一小时排优先级去做三十分钟能完成的事？
> - 每八周系统地重新评估给 Agent 的自主权边界。

---

## 精读

### 论证链

```

核心前提：软件行业40年建立在两个假设上
   代码难写 → 软件工程需要严格流程（排期、交接、审查）
   代码稀缺 → 软件公司能靠代码收费
        ↓
两个假设因 AI 而瓦解
   模型不需要完美，只需要比平均水平好就能打破旧经济学
        ↓
第一层推论：瓶颈迁移
   从"写代码"迁移到"工程决策"（优先级、排序、权衡）
   工程错误而非编码错误成为剩余瓶颈
        ↓
第二层推论：工作单元改变
   从"要写的代码"转变为"被委托的任务"
   Agent 不再是助手侧边栏，而是长时间自主运行
        ↓
第三层推论：软件形态改变
   软件主要为 Agent 使用而建，而非仅为人
   更多软件即时生成（just-in-time），而非提前构建
   "使用软件"和"构建软件"的边界模糊甚至消失
        ↓
第四层推论：价值链重构
   编码工作流的软件贬值
   数据、权限、分发、信任、合规和物理资产升值
        ↓
战略推论：赢家围绕模型重组
   输家是那些护城河只是"客户无法自建"的厂商
   小团队+强判断+多Agent > 大团队+旧流程+AI插件
        ↓
行动纲领
   每八周给模型松绑一次，重新评估自主权边界
   建立小型"前沿团队"发现新工作方式，不给旧流程加 AI
   不坐等终局明朗——前沿在移动，不去就是留在原地
```

### 关键引述


> Why put training wheels on someone who never wobbles? Why give a perfect typist a spellchecker?
>
> An agent forced to work like a human is a wasted agent.
>
> AI is not only an accelerator for X. It changes whether X should exist at all.
>
> The unit of work becomes the delegated task, not the code to be written.
>
> The last ones to admit all of this will be software companies. Their business models were built on the old scarcity.
>
> A small team with strong judgment and many agents will outrun a large team trying to fit AI into processes from before the transformation.

### 局限与盲区


- 本文未覆盖：安全与对齐问题——当 Agent 长时间自主运行且写大多数代码时，如何确保不被恶意注入或产生系统性漏洞；非软件行业（制造业、医疗、法律）的影响未展开。
- 隐含假设：模型能力将持续指数增长（"每八周松绑"暗示持续改进）；Agent 生成代码的质量和可维护性足以替代人手代码。文章自己也承认"终局尚未明朗"。
- 可能的反例：受监管行业（金融、国防）中代码可追溯性和人工审查是合规要求，即时生成的代码可能无法满足；某些场景下"写代码"和"做决策"不可分割。

---

## 关联

- [[后摩尔工程方法论]]
- [[编排税]]
- [[AI时代人才六特质]]
- [[去中心化AI道路]]
- [[Agent本质一文讲清]]
- [[Agentic设计模式]]
