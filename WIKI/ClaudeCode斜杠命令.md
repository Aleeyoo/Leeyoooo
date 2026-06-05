---
Belongs to: "[[工具教程]]"
aliases: ["Claude Code命令大全", "ClaudeCode slash commands", "CC命令参考"]
tags: ["Claude Code", "命令行", "AI编程", "开发工具"]
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/yupi996/status/2057452400366272822"
concepts: ["斜杠命令", "会话管理", "上下文压缩", "子代理", "goal模式", "代码审查", "后台任务", "定时任务"]
confidence: medium
---
# ClaudeCode斜杠命令

> [!abstract]- AI 摘要
> Claude Code 内置 50+ 斜杠命令，覆盖会话管理、上下文控制、模型切换、代码审查和并行协作，了解这些命令可以显著提升 AI 编程效率。

---

## 扫读

> [!tip] 💡 一句话
> Claude Code 内置大量斜杠命令，虽然用自然语言也能触发，但了解命令的能力边界才能打开使用思路，真正把效率翻 10 倍。

> [!important] 📌 关键结论
> - 大多数命令不需要死记，直接用中文描述需求，Claude 会自动执行对应命令。了解命令是为了知道能力边界。
> - `/compact` 和 `/context` 配合使用是管理上下文的核心手段：超过 80% 用量时主动压缩，比等自动触发更可控。
> - `/goal` 是最强大的命令之一，适合设定完成条件后自动持续工作，但必须写好可自证的完成条件并加熔断限制。

> [!quote] 🎬 可行动项
> - 养成习惯：每完成一个阶段性任务主动执行 `/compact`，保持上下文清爽
> - 尝试 `/goal` 做批量重构或模块迁移，配合熔断限制（如"20 轮还没搞定就停"）
> - 在提交代码前用 `/diff` 过一遍改动，用 `/review` 做 AI 预审

---

## 精读

### 论证链

```
核心学习策略：
  知道命令存在 > 记住具体用法
  大部分操作用中文自然语言即可触发 Claude 自动执行对应命令
  了解命令是为了知道能力边界，不是为了死记
        ↓
六大命令分类体系：
  ① 会话管理：/clear /compact /resume /branch /rewind /recap /btw /copy /export /exit
        ↓
  ② 信息与诊断：/usage /context /diff /status /help /insights
        ↓
  ③ 模型和模式控制：/plan /goal /model /effort /fast
        ↓
  ④ 配置与扩展：/config /mcp /skills /plugin
        ↓
  ⑤ 代码审查：/review /simplify
        ↓
  ⑥ 子代理和并行：/agents /tasks /background /loop
        ↓
两个重点命令深入：
  /compact：上下文超过 80% 用量时主动压缩，比等自动触发更可控
    每完成一个阶段性任务主动执行，保持上下文清爽
        ↓
  /goal：最强大的命令，设定完成条件后自动持续工作
    关键：完成条件必须是 Claude 能自证的客观条件
    必须加熔断限制（如"20 轮还没搞定就停"）
```

### 关键引述

> "AI 时代，该给自己的大脑减减负了~"

> "虽然现在 Claude Code 已经有 50 多个命令了，但是你不用死记这些命令。日常使用时，直接用中文跟 Claude 说你的需求。"

> "/goal 命令的关键在于你怎么编写完成条件。条件必须是 Claude Code 执行过程中能自证的，比如'跑某个命令的结果是什么'。"

### 局限与盲区

- 本文未覆盖：未涉及命令在大型团队协作中的最佳实践，也未讨论不同模型下命令行为的差异。
- 隐含假设：读者已安装并能正常使用 Claude Code，网络环境和账户订阅无障碍。对于 API Key 接入第三方模型的用户，部分命令（如 /usage 的费用估算）可能不准确。
- 可能的反例：对于简单的一次性任务，了解和配置命令的时间可能超过直接对话完成的时间。/goal 的自动循环可能产生较高 token 消耗。

---

## 关联

- [[工具教程]]
- [[RealEngineer技能组]]
- [[顶级Skill设计]]
- [[AgentHarness架构]]
