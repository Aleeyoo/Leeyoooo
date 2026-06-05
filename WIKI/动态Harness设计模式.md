---
Belongs to: "[[Agent工程]]"
aliases: ["Dynamic Workflow Patterns", "Dynamic Harness Design", "动态工作流设计", "Workflow Patterns"]
tags: ["Claude Code", "Dynamic Workflow", "Harness", "设计模式", "Agent编排", "多Agent"]
created: 2026-06-06
source: ai-generated
source_url: "https://x.com/trq212/article/2061907337154367865"
concepts: ["Dynamic Workflow", "Agentic Laziness", "Self-preferential Bias", "Goal Drift", "Fan-out-and-Synthesize", "Adversarial Verification", "Classify-and-Act", "Tournament Pattern"]
confidence: high
---

# 动态Harness设计模式

> [!abstract]- AI 摘要
> Anthropic 官方发布的动态工作流（Dynamic Workflow）设计指南：Claude Code 可自行编写 JavaScript harness 脚本，将复杂任务拆分为多个并发子 Agent 并编排其执行。核心贡献在于识别了单上下文窗口的三种退化模式，并归纳了六种可组合的编排设计模式。

---

## 扫读

> [!tip] 💡 一句话
> Claude Code 的动态工作流本质是让 Claude 即时编写任务专属 harness，而非依赖预定义的通用 harness——由此催生了六种可组合的编排模式，分别解决分类路由、并行合成、对抗验证、生成过滤、竞争淘汰和循环收敛六类问题。

> [!important] 📌 关键结论
> - 默认 Claude Code harness 为编程优化，但在长运行、大规模并行或高度结构化对抗性任务中会出现三种退化：agentic laziness（中途声明完成）、self-preferential bias（偏爱自己结果）、goal drift（跨轮次目标偏移），其根因是单一上下文窗口中规划与执行争抢注意力
> - 动态工作流通过分配独立上下文窗口的 SubAgent 来对抗这三种退化——每个 Agent 只持有聚焦的、隔离的目标，编排层负责汇总
> - 六种核心编排模式：classify-and-act（分类路由）、fan-out-and-synthesize（并行合成）、adversarial verification（对抗验证）、generate-and-filter（生成过滤）、tournament（竞标淘汰）、loop until done（循环收敛），这些模式可组合使用
> - 动态工作流比静态工作流更精准，因为前者是根据具体任务即时生成的"定制 harness"，后者是为覆盖所有边界情况而设计的通用方案
> - 使用门槛低但 token 成本高——通过触发词"ultracode"或直接要求创建 workflow 即可启动，但需设置预算上限和控制使用频率

> [!quote] 🎬 可行动项
> - 判断任务是否需要动态工作流：是否包含大规模并行子任务、是否涉及自己验证自己的对抗性检查、是否预期超过 20+ 次工具调用——满足任一条件则考虑使用
> - 为动态工作流设置 token 预算（如"use 10k tokens"），避免单次任务消耗失控
> - 高频重复的工作流按"s"键保存至 `~/.claude/workflows/`，或打包为 Skill 分发
> - 将工作流与 `/loop` 和 `/goal` 组合使用，实现定时触发和硬性完成条件
> - 在迁移和重构场景中，要求 Agent 使用 worktree 隔离每次修改，并配备对抗性审查 Agent

---

## 精读

### 论证链

```
问题定义：默认 harness 适合编程任务但无法覆盖所有任务类别
  → Anthropic 内部为 Research、Security Analysis、Agent Teams、Code Review 各自构建了专属 harness
  → 目标：让 Claude 自行按需生成 harness，而非人类预先为每种任务类型编写
        ↓
为什么需要动态工作流（三种退化模式）：
  ① Agentic Laziness：Claude 独立上下文中运行过长 → 提前宣布完成，只处理了 50 项中的 20 项
  ② Self-preferential Bias：Claude 倾向偏爱自己的结果/发现 → 尤其体现在被要求验证或评估自己产出时
  ③ Goal Drift：多轮压缩过程中目标逐渐丢失 → 每次压缩是有损的，边缘条件约束最先消失
  共同根因：单一上下文窗口中规划者和执行者争抢同一注意力空间
        ↓
解法：动态工作流 = Claude 即时编写 JavaScript 编排脚本
  → 技术实现：JavaScript 文件 + 特殊函数（生成/协调 SubAgent）+ 标准 API（JSON/Math/Array）
  → 每个 SubAgent 持有独立上下文窗口和聚焦目标 → 消除规划-执行注意力竞争
  → 支持按任务选择不同模型和 run_in_worktree 隔离级别
  → 支持中断恢复：工作流被用户操作或终端退出中断后，恢复会话可继续执行
        ↓
动态 vs 静态工作流：
  静态工作流：通过 Agent SDK 或 claude -p 预编写，为覆盖所有边界情况而设计得通用
  动态工作流：由 Claude（Opus 4.8+）即时生成，为当前具体任务量身定制 → 更精准但消耗更多 token
        ↓
六种核心编排模式（可组合）：
  ① Classify-and-Act：分类 Agent 判断任务类型 → 路由到对应处理 Agent → 可选终局分类验证输出
  ② Fan-out-and-Synthesize：拆分为多个独立子步骤 → 并行执行（各自干净上下文窗口避免交叉污染）→ 汇总合成
  ③ Adversarial Verification：每个执行 Agent 配一个验证 Agent → 按评分标准审查输出
  ④ Generate-and-Filter：生成大量候选 → 按标准过滤 → 去重 → 只返回最高质量的已验证结果
  ⑤ Tournament：多个 Agent 竞争同一任务 → 配对淘汰赛 → 评判 Agent 逐轮比较 → 选出最优
  ⑥ Loop Until Done：当工作量未知时 → 循环派生子 Agent → 直至停止条件满足（无新发现/无新错误）
        ↓
应用场景（十大类）：
  迁移重构：子任务拆分 + worktree 隔离 + 对抗审查 + 合并
  深度研究：fan-out 搜索 + 获取来源 + 对抗验证声明 + 合成引用报告
  深度验证：识别所有事实声明 → 每个声明独立核实 → 再次验证来源质量
  排序：锦标赛配对比较（比较性判断比绝对打分更可靠）→ 桶排并行 → 合并
  规则遵守：每条规则一个验证 Agent → 怀疑论者 Agent 审查规则避免误报
  根因调查：从分离证据中独立生成假设 → 多个假设面对验证与反驳面板
  大规模分类：分类 + 去重 + 行动（修复或升级人审）→ 隔离模式：阅读不受信内容的 Agent 禁止高权限操作
  探索与品味：多方案探索 → 评审 Agent 持有评分标准 → 锦标赛排序选出最优
  Evals：独立 worktree 中运行 Agent → 比较 Agent 按评分标准对比输出
  模型路由：分类 Agent 预研任务复杂度 → 根据预期复杂度路由到 Sonnet/Opus
        ↓
实践建议：
  - 详细提示词 + 明确指定上述模式 → 最佳结果
  - 快速工作流：轻量级对抗审查只需"quick workflow"提示
  - 设置 token 预算上限（"use 10k tokens"）
  - 保存可复用工作流 → 通过 Skill 分发
  - 不适用于日常编程任务——问自己"这真的需要更多算力吗？"
```

### 关键引述

> "When you ask the default Claude Code harness to do a task, it needs to both plan and execute in the same context window. For many coding tasks, this is highly effective, but it can sometimes break down over long-running, massively parallel and/or highly structured adversarial tasks."

> "Creating a workflow helps combat these by orchestrating separate Claudes with their own context windows and focused, isolated goals."

> "With Claude Opus 4.8 and dynamic workflows, Claude is now intelligent enough to write a custom harness tailor-made for your use case."

> "Think creatively of when and how to ask Claude Code to make dynamic workflows. I've found that workflows are sometimes even more useful for non-technical work."

### 局限与盲区

- **本文未覆盖**：动态工作流失败时的回滚与部分成功状态处理——当 fan-out 的 50 个 SubAgent 中 5 个失败，编排层如何决策重试 vs 降级 vs 跳过？token 消耗的量化对比数据——一个典型动态工作流比同任务的单 Agent 执行多消耗多少 token？工作流的可调试性——JavaScript 编排脚本异常时如何排查、是否有日志和断点能力？
- **隐含假设**：假设 Claude（Opus 4.8+）生成的 JavaScript 编排脚本本身可靠且正确——但脚本 bug 可能导致整个工作流静默失败或产生错误合成结果。假设任务可被有效拆分为隔离子任务——对于有强依赖链的任务（如顺序重构），fan-out 模式的收益会被破坏。假设 SubAgent 输出质量一致——实际中不同 Agent 的输出质量波动会影响合成质量，但编排层未提及质量把关机制。
- **可能的反例**：对需要全局一致性的任务（如统一重构一个跨模块接口），fan-out 的独立上下文窗口可能产生相互矛盾的结果，拼合成本可能超过收益。对创意性任务（如命名设计），tournament 模式的逐轮淘汰可能过早排除"怪但好"的选项。小规模任务（<5 个子任务）使用动态工作流的编排开销可能超过收益。
- **生态限定**：本文完全基于 Claude Code 生态（Opus 4.8、Worktree、Skill 分发），但设计模式本身（fan-out/synthesize、tournament、adversarial verification）是多 Agent 架构的通用模式，跨框架适用。

---

## 关联

- [[Claude Code动态工作流]] —— 同一功能的中文解读，侧重四阶段进化与 token 消耗阶梯，本文提供官方视角与设计模式体系
- [[Harness工程全景]] —— 动态工作流是 Harness First 理念在 Claude Code 中的具象化实现
- [[编排税]] —— 动态工作流的核心价值是在不增加人类审查负担的前提下扩展 Agent 规模，但审查瓶颈仍存在
- [[Agent Doom Loop检测与防护]] —— 动态工作流通过隔离上下文窗口防止自偏好偏差和目标漂移，与 Doom Loop 防护形成互补
- [[长任务Agent工程闭环]] —— 动态工作流的 loop-until-done 模式与该文的长任务工程方法论直接对应
- [[Agent Skill替代工具软件]] —— 动态工作流可通过 Skill 分发和复用，形成 skill → workflow 的分层体系
- 所属项目：[[Agent工程]]
