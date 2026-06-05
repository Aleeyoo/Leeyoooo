---
Belongs to: "[[Agent工程]]"
aliases: ["Pi Agent", "开源模型Agent", "Ring Agent配置"]
tags:
  - AI Agent
  - 开源模型
  - Pi
  - Agent工具
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/wquguru/status/2056235143623495975"
concepts:
  - minimal core设计
  - model-agnostic agent harness
  - plan-first工作流
  - thinking档位分派
  - skill固化工作流
  - provider适配
  - subagent分工
confidence: medium
---

# PiCodingAgent指南

> [!abstract]- AI 摘要
> Pi 不是另一个 Claude Code，而是一个更可拆的 Agent 底座——用户自主决定装哪些组件、给多少上下文、什么时候 high/xhigh、哪些工具进入 prompt。代价是要会配置，收益是测试开源模型（如 Ring-2.6-1T）时更透明、更可控、更容易复现。

---

## 扫读

> [!tip] 💡 一句话
> Pi 的价值不是让 Claude Code 用户换一个更省心的工具，而是给开源模型一个更干净、更透明、更可控的 Agent 运行环境——真正的关键不是模型是否无所不能，而是目标、上下文、工具、plan、skill 和验收标准有没有配好。

> [!important] 📌 关键结论
> - Pi 和 Claude Code 的根本差异：Claude Code 是产品化的完整工程环境（工具注入了什么、权限怎么拼都内置好了），Pi 是 minimal core（只有读写文件+grep/find/ls），其他能力全是用户自主安装的扩展
> - 模型分工策略：贵的深推理只花在真正需要的地方——Ring xhigh 用于复杂规划/核心逻辑/最终审查，Ring high 用于日常工程推进，DeepSeek 用于测试/样板代码/快速修补
> - plan-first + skill 是开源模型发挥最大功效的关键——Ring 能"先拆解再落地"，但你必须给它明确的拆解框架和验收标准

> [!quote] 🎬 可行动项
> - 从核心 5 个包开始（pi-mcp-adapter, pi-web-access, pi-subagents, pi-fff, pi-context-prune），不要一次装 22 个
> - 配 models.json 时注意三个关键点：compat.supportsDeveloperRole: false、thinkingLevelMap 含 xhigh、contextWindow/maxTokens 不要填小
> - 强制 plan-first：复杂任务先让模型输出"要读哪些文件→风险点→验收标准→分几步→哪些需要测试覆盖"，评审 plan 后再执行

---

## 精读

### 论证链

![[pi-agent-comparison.jpg]]

```
Claude Code vs Pi 对照：
  Claude Code = 产品化全家桶（subagents, Plan Mode, MCP, 权限, 上下文压缩, skills, commands 全焊进产品）
  Pi = minimal core + 插件市场（用户决定装什么，组件注册的 Tools 只占 7.7k 上下文）
        ↓
Pi 的四类扩展生态：
  TypeScript Extensions：用代码挂生命周期事件（对应 Claude Code hooks，但可写逻辑而非声明式 JSON）
  Skills：SKILL.md + 脚本（和 Claude Code skills 同一类东西）
  Prompt Templates：对应 Claude Code slash commands
  Pi Packages：pi install npm:<pkg> 或 pi install git:<repo>
        ↓
模型配置实战（以 Ring-2.6-1T 为例）：
  - provider 选择：DeepSeek 用内置 provider（只给 key），Ring 装自定义 provider（手填 models.json）
  - 选 OpenAI 兼容端点 /v1 而非 Anthropic 兼容层（cache_control/thinking blocks/tool schema 在转换中容易丢失）
  - models.json 三处不能省：
      compat.supportsDeveloperRole: false（否则 400 错误）
      thinkingLevelMap 含 "xhigh":"xhigh"（否则选不到最高推理档）
      contextWindow/maxTokens 不要填小（否则答案被截断误判为不会做）
        ↓
thinking 档位分工：
  Ring xhigh → 复杂规划 / 核心逻辑 / 最终审查
  Ring high → 默认工程执行
  DeepSeek → 测试 / review / 非核心代码 / 快速修补
        ↓
plan-first + skill 工作流为什么关键：
  Claude Code 内建工程纪律（用户不一定显式感知），Pi 没有默认这些
  → 必须以扩展形式安装：plan-first / subagent(planner-executor-reviewer) / skill / context prune
  → skill 的核心价值不是外挂能力，而是把隐性经验显式化：
      测试怎么写、UI怎么验收、bug fix先看哪些文件、什么时候停止旧上下文重开
        ↓
真实工程任务验证（现货-永续资金费率监控系统）：
  Ring 暴露的问题不是模型不会 → 是没有 Plan Mode 把问题拦在实现前
  UI 状态契约未在 plan 阶段锁死 / funding interval 的 symbol 级 override 未被测试覆盖 / 测试有同义反复倾向
  解决方案：Ring 不该被当作"一次性生成完整系统"的黑盒，而应放入 plan-first + skill-amplified + review-driven 工作流
```

### 关键引述

> Pi 不是另一个 Claude Code，而是一个更可拆的 agent 底座。用户自主决定装哪些组件、给多少上下文、什么时候 high/xhigh、哪些工具进入 prompt。代价是要会配置；收益是测试模型时更透明、更可控、更容易复现。

> Pi 的价值不是插件越多越强，使用者需要知道每个组件为什么在上下文里。

> Ring 不应该被当成"一次性生成完整系统"的黑盒。它更适合被放进一个 plan-first、skill-amplified、review-driven 的工作流里。

> skill 的价值不在给模型外挂能力，而是把隐性经验显式化：测试应该怎么写、UI 应该怎么验收、repo bug fix 应该先看哪些文件、什么情况下要停止旧上下文重开。

### 局限与盲区

- **本文未覆盖**：Pi 与 Claude Code 在同等任务下的速度和 token 成本对比；除 Ring-2.6-1T 外的其他开源模型（Qwen、Llama）在 Pi 上的表现；Pi 的权限和 sandbox 安全体系（作者提到这是缺口但未展开）；长期使用 Pi 的维护成本和组件升级兼容性问题
- **隐含假设**：用户有足够的工程能力来配置 models.json 和排查 provider 问题；开源模型的能力提升会持续追赶闭源模型；plan-first 工作流在各类型任务上都有正向收益
- **可能的反例**：对于简单任务，plan-first 额外的 plan 步骤可能增加 token 成本而收益有限；Ring-2.6-1T 是纯文本模型不能看图，视觉相关任务需要额外的多模态模型配合；某些闭源模型（Claude Opus）在 Claude Code 原生产品上的表现可能远超 Pi 上的任何配置

---

## 关联

- [[AI商业]]
- [[CLAUDE.md优化规则]]
- [[企业级Agent构建指南]]
- [[Agent最简实现原理]]
