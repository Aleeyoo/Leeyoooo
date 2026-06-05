---
Belongs to: "[[Agent工程]]"
aliases: ["CLAUDE.md 12规则", "Karpathy规则", "AI编码规则优化"]
tags:
  - Claude Code
  - AI编码
  - 行为规范
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/Mnilax/status/2053116311132155938"
concepts:
  - 行为契约
  - 规则合规率衰减
  - token预算硬约束
  - 冲突表面化
  - 失败可见化
  - 检查点机制
  - 规则过载阈值
  - 测试意图验证
confidence: high
---

# CLAUDE.md优化规则

> [!abstract]- AI 摘要
> Karpathy 在 2026 年 1 月对 Claude 代码质量的批评催生了 4 条规则（Forrest Chang 整理，12 万星），将错误率从 41% 降到 11%。经过 30 个代码库 6 周实测，作者新增 8 条规则覆盖 Agent 时代的新问题（Agent 循环、跨会话工作流、多代码库一致性），将错误率进一步降到 3%。

---

## 扫读

> [!tip] 💡 一句话
> CLAUDE.md 不是愿望清单，而是行为契约——每条规则必须回答"这个规则防止什么错误"。从 4 条到 12 条，错误率从 41% 降到 3%，但超过 12 条后合规率从 76% 暴跌到 52%。

> [!important] 📌 关键结论
> - Karpathy 的 4 条基础规则（思考先行、极简优先、外科手术式修改、目标驱动执行）解决了 40% 的失败模式，但它们是 2026 年 1 月的产物——Agent 流水线、多步骤任务、跨代码库一致性问题当时还不存在
> - 8 条新增规则的核心逻辑：把非语言工作还给代码（Rule 5）、硬 token 预算（Rule 6）、冲突表面化而非折中（Rule 7）、先读后写（Rule 8）、测试验证意图而非行为（Rule 9）、检查点（Rule 10）、遵守而非创新（Rule 11）、失败可见化（Rule 12）
> - 200 行天花板真实存在：CLAUDE.md 超过 200 行后合规率急剧下降，重要规则被噪音淹没

> [!quote] 🎬 可行动项
> - 先采用 Karpathy 的 4 条基线，再从 8 条新增中挑选匹配你实际踩过的坑的规则
> - 每条规则追问"这个规则防止什么错误"——无法回答的规则不要加
> - 总行数控制在 200 行以内，项目专属规则（测试命令、错误模式）放在通用规则下方

---

## 精读

### 论证链

![[claudemd-mistake-rates.jpg]]

```
Karpathy 1月抱怨三种失败模式：
  沉默的错误假设 / 过度工程化 / 对不该碰的代码造成正交损害
        ↓
Forrest Chang 打包为 4 条规则 → 12 万星 → 错误率 41%→11%
  ① 思考先行  ② 极简优先  ③ 外科手术式修改  ④ 目标驱动执行
        ↓
30 个代码库 6 周实测 → 发现 4 条规则的 4 个盲区：
  - 长程 Agent 任务（无预算/无检查点/无"大声失败"）
  - 多代码库一致性（"匹配已有风格"在 12 个服务的 monorepo 里不适用）
  - 测试质量（"测试通过=成功"但测试可能什么都没测）
  - 生产 vs 原型（"极简优先"对需要 100 行探索性代码的原型杀伤力过大）
        ↓
新增 8 条规则覆盖 2026 年 5 月的新问题：
  Rule 5: 模型只做判断，不做确定性逻辑（retry/routing用普通代码）
  Rule 6: 每任务 4K token，每会话 30K token 硬预算
  Rule 7: 两个模式冲突时选一个（更新的/更经过测试的），不要折中
  Rule 8: 添加代码前先读导出、调用方、共享工具
  Rule 9: 测试必须编码"为什么"行为重要，而非只测"做什么"
  Rule 10: 多步骤任务的每一步后 check point（做了什么/验证了什么/还剩什么）
  Rule 11: 匹配代码库约定而非引入新范式（即使你觉得你的方式更好）
  Rule 12: 不能确定时明确说出来（"迁移完成"错了如果跳了 30 条记录）
        ↓
数据验证：50 个任务 × 3 配置 × 6 周
  基础线(无规则): 错误率 41%, 合规率 78%
  4条规则: 错误率 11%, 合规率 78%
  12条规则: 错误率 3%, 合规率 76%
  关键发现：从 4 条到 12 条几乎不增加合规开销，但额外减少了 8% 错误率——新旧规则覆盖不同失败模式，不争夺同一注意力预算
        ↓
反模式（试过但不生效）：
  - "小心"/"多想"/"真正专注于" → 合规率 ~30%，不可测试
  - 要求 Claude 做"senior" → Claude 已经认为自己是 senior，身份提示关不掉"想到做到"的鸿沟
  - CLAUDE.md 里放示例 → 示例比规则重，3 个示例占 10 条规则的上下文
  - >12 条规则 → 合规率从 76% 暴跌到 52%
```

### 关键引述

> CLAUDE.md is not a wishlist. It's a behavioral contract that closes specific failure modes you've observed. Every rule should answer: what mistake does this prevent?

> The interesting result isn't the headline drop from 41% to 3%. It's that going from 4 rules to 12 added almost no compliance overhead (78% -> 76%) but cut the mistake rate by another 8 points. The new rules cover failure modes the original 4 didn't address — they don't compete for the same attention budget.

> Claude said a database migration "completed successfully." It had silently skipped 14% of records that hit a constraint violation. The skip was logged but not surfaced. Discovered the problem 11 days later.

> "Average" code that satisfies both rules is the worst code.

### 局限与盲区

- **本文未覆盖**：规则在不同模型（GPT、Gemini）上的适用性差异——测试仅在 Claude Code 上进行；非英语代码库的规则有效性；小项目 vs 企业级项目的规则裁剪策略量化对比；规则随 Claude 模型版本更新的降效风险
- **隐含假设**：用户使用 Claude Code（而非 Codex、Pi 等其他 Agent 工具）；代码库有足够的规模和复杂度来暴露这些失败模式；200 行天花板在不同项目中一致
- **可能的反例**：某些代码库的 linting/格式化已经完全自动化，Rule 11（约定优先）可能冗余；对于单人项目，Rule 7（冲突表面化）几乎没有用武之地；经验丰富的开发者可能不需要 Rule 8（先读后写）

---

## 关联

- [[AI商业]]
- [[Harness工程控制论]]
- [[Agent最简实现原理]]
- [[PiCodingAgent指南]]
