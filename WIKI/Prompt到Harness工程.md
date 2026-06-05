---
Belongs to: "[[Agent工程]]"
aliases: ["LLM工程三阶段", "Harness Engineering"]
tags: ["Agent工程", "Prompt工程", "软件工程"]
created: 2026-05-24
source: ai-generated
source_url: "https://x.com/yan5xu/article/2059117572826746979"
concepts: ["Prompt Engineering", "Context Engineering", "Harness Engineering", "螺旋进化", "自主反馈", "能力边界", "代码Agent"]
confidence: high
---
# Prompt到Harness工程

> [!abstract]- AI 摘要
> LLM 工程经历了三个阶段：Prompt Engineering（人工优化单次输入）、Context Engineering（程序按规则编排多步信息流）、Harness Engineering（放手让 Agent 自主获取反馈 + 框定能力边界作为保险）。三阶段不是替代而是螺旋叠加——每一圈不消失，变成下一圈的基础设施。

---

## 扫读

> [!tip] 💡 一句话
> Harness Engineering 的核心公式：给 Agent 工具让它自主判断 + 用硬约束兜底防止失控。瓶颈从"模型不够强"移到了"环境设计不够好"。

> [!important] 📌 关键结论
- Prompt Engineering 的局限：解决不了多步任务的动态信息需求——修复 bug 的全貌在执行中逐步展开，无法在一次 prompt 里组织好所有信息
- Context Engineering 的局限：人硬编码的信息编排规则限制了模型的自主能动性——模型已经有能力判断哪些信息重要，但框架不给它这个权力
- Harness Engineering 的两个动作：(1) 放手——给 Agent 工具链（lint/test/search）让它自主获取反馈；(2) 上保险——用 sandbox、CI 轮数限制、文件权限、架构测试等硬约束兜底

> [!quote] 🎬 可行动项
> - 先放手再观察失败模式，然后针对性加保险——边界设计从事故倒推，不是凭空画线
> - 区分 Harness 和 Agent Runtime：compaction、session 续接是 runtime 的事，harness 关心的是 Agent 能否自主获取反馈和是否被约束住

---

## 精读

### 论证链

```
第一阶段 Prompt Engineering（2022-）：
手工优化单次输入，Few-shot/CoT/Role assignment
      ↓
天花板：任务是动态展开的，修 bug 的全貌不可能一次 prompt 覆盖
      ↓
第二阶段 Context Engineering（2024-）：
程序按预设规则管理步与步之间的信息流转
Cursor 的动态检索、Lovable 的自动注入 LSP/测试反馈
      ↓
天花板：(a) context rot 使模型性能下降 39%
(b) 更致命：模型已强到能自主判断信息重要性，但框架不给它这个权力
      ↓
第三阶段 Harness Engineering（2025-）：
      ↓
机制① 放手：给 Agent 工具链（lint/test/search/browser）
让它自主决定什么时候用什么工具
Stripe Minions：Agent 自主决定先修哪个 lint error
      ↓
机制② 上保险：框定能力边界防止失控
- Sandbox 隔离（不能碰生产环境）
- CI 轮数限制（两轮就停）
- 硬约束 feature_list.json（只能改 passes 字段）
- Structural tests 强制执行模块边界
      ↓
螺旋叠加：Harness 内部仍跑 Context Engineering pipeline
Context Engineering 内部仍写精心设计的 system prompt
      ↓
结论：Agents aren't hard; the Harness is hard
从 Prompt→Context→Harness，瓶颈每次移动到新的层
下一层瓶颈：Eval（怎么评估 Agent 输出质量）和 Governance（多 Agent 认证和权限）
```

### 关键引述

> Context engineering is the delicate art and science of filling the context window with just the right information for the next step. —— Karpathy

> Agents aren't hard; the Harness is hard. —— Ryan Lopopolo, OpenAI Codex

> 每一次演化都不是凭空设计出来的，而是上一阶段的做法不够用了，新的实践才被逼出来。

> Harness 关心的不是 agent 能不能跑下去，而是 agent 跑的时候能不能自主获取反馈、会不会被约束住。

### 局限与盲区

- 本文未覆盖：Harness Engineering 在小团队（1-3 人）的可行性——Stripe、Anthropic、OpenAI 都是重资源团队，个人开发者构建类似 harness 的最小可行版本是什么？
- 隐含假设：假设三阶段是普适的演进路径。但在某些领域（如法律文书生成），任务本质上是静态的，Prompt Engineering 可能始终是最佳方案
- 可能的反例：当模型自身内置了强大的自主纠错能力（如 recursive self-critique），Harness 的外部约束可能成为多余甚至阻碍

---

## 关联

- [[WIKI/AgentHarness架构]]
- [[WIKI/Harness工程控制论]]
- [[WIKI/世界级Agentic工程师方法论]]
- [[WIKI/ClaudeCodeHooks管理]]
- [[WIKI/提示词工程九原则]]
