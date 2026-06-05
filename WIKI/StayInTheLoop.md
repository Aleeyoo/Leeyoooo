---
Belongs to: "[[个人成长]]"
aliases: ["不脱离循环", "开发者与Agent协作", "Stay in the loop"]
tags: ["AI协作", "代码审查", "开发者角色", "用户体验"]
created: 2026-05-24
source: ai-generated
source_url: "https://x.com/jlongster/article/2058197974321070379"
concepts: ["Agent缺乏判断力", "开发者必须在循环内", "用户体验无法被Agent替代", "diff审查是最低底线", "后期发现问题代价更高"]
confidence: medium
---
# StayInTheLoop

> [!abstract]- AI 摘要
> 当前主流叙事是"开发者应该退出编程循环，让 Agent 干活"。但 Agent 没有好判断力——关键决策、用户体验感受、架构形态的判断必须由开发者在循环内完成。退出的代价是后期发现问题时回滚成本远高于早期介入。

---

## 扫读

> [!tip] 💡 一句话
> 不管你用不用 Agent 写代码，你都必须亲自看 diff、亲自运行产品、亲自感受变化——因为用户不会退出循环，你的用户会体验你交付的一切。

> [!important] 📌 关键结论
> - Agent 的核心盲区是缺乏判断力（opinions）：它们能执行任务，但无法感知用户体验的好坏——一个快捷键从"f"改成"tab"的决策只能由人在使用中做出
> - 退出的代价是非线性的：后期发现问题的回归成本远高于早期介入，即使 Agent 能加速代码修改，已经依赖错误功能的用户让变更更加困难
> - 最低底线是看 diff 读代码：编辑器可以不一直打开，但每次改动必须亲自审查代码变化

> [!quote] 🎬 可行动项
> - 建立个人习惯：每次 Agent 完成改动后，先看 diff 再运行产品，亲身体验交互变化
> - 对架构层面的 Agent 建议保持警觉——架构形态决定你未来能交付什么功能，这个判断不能外包给 AI

---

## 精读

### 论证链

```
Agent编码效率提升 → "开发者应该退出循环"成为流行叙事
      ↓
反论：Agent没有好的判断力（do not have good opinions）
      ↓
论据1：用户体验判断不可外包
  - 快捷键从"f"→"tab"的决策来自亲身体验，不是逻辑推导
  - 用户不会退出循环 → 你交付的一切都会被真实体验
      ↓
论据2：架构判断不可外包
  - 后端架构形态决定未来能交付什么功能
  - 架构决策影响面广，修改代价大
      ↓
论据3：延迟介入的代价非线性增长
  - 后期发现的问题更难回滚
  - 即使Agent使代码修改更容易，已有用户依赖的"坏决策"也难以改变
      ↓
结论：最低底线是留在循环内看diff+运行产品——从源头上让事情做对，而不是事后靠Agent重构
```

### 关键引述

> The problem is agents do not have good opinions. More importantly, I always have my product running beside my coding agent. I can interact with it and see how the changes feel. You know who is not staying out of the loop? Your users! Whatever you ship will be experienced by them. If you're staying out of the loop, how are you guaranteeing a good experience?

> It's a lot better to just stay in the loop and make things good from the start.

### 局限与盲区

- 本文未覆盖：未讨论不同程度的"留在大循环内"如何分级——是所有决策都亲自做还是有选择地委托；未给出 Agent 可安全自主处理的场景分类和边界条件；对前端 UI 的判断较多，对纯后端/数据处理场景的论证较弱
- 隐含假设：假设开发者有能力同时跟进 Agent 的所有输出（在 Agent 高速产出时的认知负载未讨论）；假设"留在循环内"的成本（时间、精力）低于后期修复的成本，但这是情境依赖的
- 可能的反例：对于标准化程度高的任务（如 linting 修复、固定模式的 CRUD），Agent 自主完成+事后抽查可能同时实现了效率和质量；经验丰富的开发者可能发现 Agent 的高质量输出使得"退出循环"的风险实际上低于文中估计

---

## 关联

- [[CLAUDE.md优化规则]]
- [[VibeCoding面试策略]]
- [[AI工程团队管理]]
- [[世界级Agentic工程师方法论]]
