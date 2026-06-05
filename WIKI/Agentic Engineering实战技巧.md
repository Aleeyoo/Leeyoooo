---
Belongs to: "[[Agent工程]]"
aliases: ["Agentic Engineering Hacks", "Agent工程实战技巧", "Compound Engineering实践"]
tags: ["Agent工程实践", "Skill", "Compound Engineering", "工作流", "工具链"]
created: 2026-06-03
source: ai-generated
source_url: "https://x.com/mvanhorn/status/2061877533885473181"
concepts: ["Plan-first工作流", "Compound Engineering复合工程", "Human Signal人作为品味信号", "Voice-to-LLM语音输入", "Agent原生Skill", "Printing Press终端自动化", "多会话并行(cmux)"]
confidence: medium
---

# Agentic Engineering实战技巧

> [!tip] 💡 一句话核心主张
> 传统开发是 80% 编码、20% 规划，Agentic Engineering 翻转这个比例——思考放进 plan.md，执行是机械化的。人的独特价值不再是敲代码，而是提供品味、方向和判断信号。

> [!important] 📌 关键结论
> - 结论1：/ce-plan → /ce-work 是核心循环：任何想法、bug、截图立即生成 plan.md，plan 强制 Agent 做研究、承诺方案、写出验收标准，没有 plan 的 Agent 会偷懒和提前终止。
> - 结论2：plan.md 是写给 Agent 的，人不需要读——skim 标题、用 TLDR/eli5 对话式理解即可。让 plan 存在是为了约束 Agent 的惰性，不是增加人的阅读负担。
> - 结论3：当同时运行 4-6 个 Agent 会话时，人的角色从"执行者"变成"信号源"——提供品味、方向、反应和重定向，Agent 提供产量。

> [!quote] 🎬 可行动项
> - 安装 Compound Engineering 插件，养成"有想法就 /ce-plan、再 /ce-work"的习惯
> - 在 ~/.claude/settings.json 中启用 `remoteControlAtStartup: true`，实现桌面和手机无缝切换
> - 用 cmux 同时开 4-6 个 tab，每个做不同任务，一个在写 plan 时另一个在 build
> - 将记录工具（Bear、Obsidian）和 Granola 会议转录接入 Agent，让个人知识库成为 Agent 的复合上下文

### 论证链

```
**观点 → 论据 → 案例**

**观点1：Plan-first 是 Agent 工程的基石**

作者（Matt Van Horn）分享了从"三个月前 913K 阅读量的 Claude Code Hacks"到现在的演进：通过 Compound Engineering 插件的 /ce-plan 和 /ce-work，将工作流固化为先 plan 后 build。实证包括 last30days（27K stars）、Printing Press（4K+ stars）、Agent Cookie，以及成为 Python、Go、GStack、Paperclip 等顶级开源项目的主要贡献者。

**观点2：非代码工作同样适用 Plan-first**

作者与 GV 前研究合伙人 Michael Margolis 的案例：将 2 小时 Granola 会议转录和一本 PDF 书交给 Agent，先说 "/ce-plan make a plan for the plan"，Agent 花 45 分钟创建详细计划后再执行，产出比直接要求交付物深得多。关键技巧：先让 Agent 规划如何产出，再执行，能克服 LLM 惰性。

**观点3：多 Agent 并行 + 人为信号 = 高效产出**

4-6 个 cmux tab 分别处理不同任务：一个写 plan、一个 build、一个运行 last30days、一个修 bug。人的工作不是做执行，而是看结果、选方案、给方向。作者将此总结为"Human Signal"——Agent 提供产量，人提供品味和判断。

**观点4：Printing Press 将 Agent 带出终端**

通过 Printing Press 生成的 CLI 工具（Tesla、Instacart、ESPN、Alaska Airlines），Agent 可以预热汽车、下单购物、监控比分、规划旅行。"不是 AI 写我的代码，是 Agentic Engineering 跑我的杂事、看我的比赛、热我的车、订我的行程"。

**观点5：Skill 和开源贡献的复利效应**

任何重复超过两次的操作都做成 Skill。方法是"看一个已有 Skill 的结构，让 Agent 复制它的形状来做你自己的"。开源贡献（数百 PR 合并到 Python、Go、OpenCV 等）不是刻意计划，而是日常使用的工具链的自然延伸。
```
### 关键引述

> The plan is the leash. A coding agent with a plan ships finished work. A coding agent without one cuts corners and stops early.

> The rare, valuable thing in the loop is your judgment, not your typing. Be the taste. Let them be the hands.

> The cheapest token is the one never spent because the system already knew what mattered.

> Building with agents is the greatest video game ever made, and the loop is that good.

### 局限与盲区

- 本文未覆盖：团队协作中的 plan.md 版本管理和冲突解决（作者基本是单人工作流）；$400/月（两个 $200 计划）的成本对小团队和个人的负担；M5 Max + 64GB RAM 的硬件门槛对多数用户的可行性。
- 隐含假设：假设用户的代码库有足够的模式和约定让 /ce-plan 的 research agent 找到参考；假设"人不需要读 plan"适用于所有复杂度级别的任务——关键基础设施变更可能仍需人工审查；假设 Agent 输出质量稳定到可以充分信任 YOLO 模式。
- 可能的反例：skipDangerousModePermissionPrompt + defaultMode bypassPermissions 的安全风险被轻描淡写——GitHub 可恢复代码但无法恢复被覆盖的本地密钥或误删的非代码文件；在安全敏感环境中，YOLO 模式不可接受。

## 关联

- [[世界级Agentic工程师方法论]]
- [[Harness工程全景]]
- [[顶级Skill设计]]
- [[Agent Doom Loop检测与防护]]
- [[Harness自进化]]
- [[AgentHarness架构]]
- [[ClaudeCodeHooks管理]]
- 所属项目：[[Agent工程]]
