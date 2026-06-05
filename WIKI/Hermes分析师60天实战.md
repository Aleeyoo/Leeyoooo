---
Belongs to: "[[Agent工程]]"
aliases: []
tags: ["agent-workflow", "agent-tools", "skill-bundling"]
created: 2026-06-02
source: ai-generated
source_url: "https://x.com/0xJeff/article/2061361760968560887"
concepts: ["skill bundling", "provider selection", "feedback loops", "agent memory", "x402 tool payment", "agent architecture vs model", "personal agent training"]
confidence: medium
---
# Hermes分析师60天实战

> [!abstract]- AI 摘要
> 作者 0xJeff 在使用 Hermes 分析师 Agent 的 60 天中总结了 6 条核心教训，核心主张是：构建 Agent 90% 靠架构，10% 靠 AI 模型，工具/技能设计、记忆持久化和反馈循环才是决定性因素。

---

## 扫读

> [!tip] 💡 一句话
> 用 Hermes 做投资数据分析 Agent 两个月的实战经验证明：模型之间的智能差距越来越小，真正的分水岭在于工具设计、技能打包、记忆持久化和可持续的单位经济学。

> [!important] 📌 关键结论
> - 频繁切换模型供应商代价高昂（每次切换消耗 2-3 轮调试），开源模型已追上前沿水平，选一个供应商深耕比反复切换更高效
> - 工具和技能远比模型重要：对接正确的工具（Exa、Firecrawl、Browser CDP）产出的质量远超通用搜索，技能打包从 2000 字的巨型 prompt 变为目录结构可节省数万 token
> - 反馈循环是训练个人 Agent 的最快路径：Agent 产出 → 立即阅读标记错误 → 给出具体修正 → Agent 将其编码为永久规则 → 下次输出更精准
> - x402 工具支付协议解决了 Agent 工具选型的经济学难题：用 $5-10 USDC 即可探索数百个付费工具，每次调用仅几美分
> - 原生记忆 + 外部记忆提供商的组合让 Agent 能在事实和事件之间建立关联、回忆过去学习，产出高度个性化的输出

> [!quote] 🎬 可行动项
> - 将现有巨型 prompt 技能重构为目录结构：SKILL.md 保留流程逻辑（~100 行），references/ 放 API 规格和字段细节，scripts/ 放可执行脚本
> - 建立每日反馈循环：每天早上查看 Agent 产出，立即标记错误并给出具体修正指令
> - 评估当前模型供应商切换频率：如果一个月内切换了超过 1 次，考虑锁定一个直接 API 供应商
> - 为重复性工作流创建独立技能，让 Agent 在识别到重复模式时自动生成技能文件

---

## 精读

### 论证链

```
论点：Agent 失败的主因是架构问题而非模型智能不足。

1. 模型层面：60 天内切换 5-6 个供应商 → 每次切换消耗 2-3 轮调试 → 模型智能差距缩小，开放权重模型已接近前沿水平 → 选择一个供应商深耕是最优策略
2. 工具层面：模型是常量，工具是变量 → 对接正确工具（API/MCP/技能文件）比通用网页搜索质量高且成本低 → 一次性爬取用 Browser CDP，重复性流程用专用工具
3. 记忆层面：原生记忆（User.md/Memory.md/Soul.md）+ 外部记忆供应商（Hindsight）→ 注意推理成本（reflect 操作导致每天烧 $20-30）→ 时效性任务用 Recall 替代 Reflect
4. 训练层面：6 步反馈循环 → 每日迭代 → 但存在回声室问题（"为什么重要"倾向于已有持仓，来源和分析师重复讨论大市值标的）
5. 支付层面：x402 之前需要花大量时间寻找免费工具/API → x402 之后用 agentic wallet 按需付费 → 降低了工具探索门槛
6. 打包层面：巨型 prompt 每次重头推导 → 技能目录化：SKILL.md（流程）+ references/（API 规格）+ scripts/（可执行文件）→ 加载 500 token 替代 5000+ token 重复解释
```

### 关键引述

> "Agents like Hermes or OpenClaw don't fail on intelligence, they tend to fail on architecture. Most bugs were tools fighting each other, not the model being dumb."

> "Building an agent is 90% architecture, 10% AI. Everyone has access to the same models (most of which are highly intelligent). What separates useful agent to useless agent is tool/skill design, memory persistence, feedback/learning loop, and the unit economics that make it sustainable to run."

> "A well-bundled skill costs ~500 tokens to load (SKILL.md + 2-3 reference files) but saves 5000+ tokens of re-explaining context in every session."

### 局限与盲区

- 本文未覆盖：回声室问题和自我强化循环的解决方案（作者自己也承认"还没解决"）。技能打包的具体格式规范和最佳实践。x402 支付协议的安全模型细节和托管风险。多 Agent 协作场景（本文仅讨论单个 Hermes Analyst 的工作流）
- 隐含假设：假设读者使用 Hermes 或类似的 Agent 框架，且具备 crypto/web3 领域知识。假设 Agent 运行的 token 成本是可接受的，未讨论在极端成本敏感场景下的优化策略。假设"选择一个供应商深耕"适用于所有场景，但对于需要模型冗余的高可用场景可能不适用
- 可能的反例：对于不需要外部记忆的简单 Agent 任务，外部记忆供应商的复杂度和成本可能超过收益。技能打包在一次性任务中的优势不明显，其收益主要在重复执行中累积。x402 依赖于 crypto 基础设施，对于非 web3 场景的 Agent 构建者不具备参考价值
- 作者的分析工作流高度偏向金融/投资领域，其结论（如每日晨报、持仓分析）在通用 Agent 场景中的可迁移性需要验证

---

## 关联

- [[Agent记忆升级实录]] —— Agent 记忆持久化方案
- [[顶级Skill设计]] —— 技能拆分与打包方法论
- [[ClaudeSkill本质]] —— 技能作为 Agent 核心资源
- [[AgentHarness架构]] —— Agent 架构与工具编排
- [[长任务Agent工程闭环]] —— 重复工作流的可靠性
- [[Agent开发十大核心概念]] —— Agent 工程知识体系
- [[编排税即你]] —— 工具和技能的编排成本
