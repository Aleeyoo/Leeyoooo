---
Belongs to: "[[Agent工程]]"
aliases: ["Matt Pocock技能组", "Real Engineers Skills", "AI编程技能", "可组合开发流程"]
tags: ["AI编程", "工程技能", "TDD", "架构优化", "开发流程"]
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/gyro_ai/status/2055198700016660826"
concepts: ["Small/Composable/Hackable", "grill-with-docs", "Ubiquitous Language", "TDD粒度控制", "Deep Modules", "软件熵", "反馈回路", "控制权"]
confidence: medium
---
# RealEngineer技能组

> [!abstract]- AI 摘要
> Matt Pocock 开源的 Skills for Real Engineers 用小而可组合的技能替代重型流程框架，解决 AI 编程的理解偏差、缺乏共享语言、反馈缺失和软件熵加速四大核心问题。

---

## 扫读

> [!tip] 💡 一句话
> 不要用重流程框架（GSD、BMAD、Spec-Kit）让工具接管流程偷走你的控制权，而应该用小的、可组合的技能，每个技能解决一个具体问题，自由组合。

> [!important] 📌 关键结论
> - 三大设计哲学：Small（单文件可读）、Composable（自由组合）、Hackable（可改造）。每个技能几分钟就能看懂。
> - 针对 AI 编程四大核心问题：Misalignment（用 grill-with-docs 建立共享术语表）、Feedback Loops（用 TDD 控制每一步粒度）、Software Entropy（用 improve-codebase-architecture 定期扫描优化）、Ubiquitous Language（术语表统一概念）。
> - 推荐工作流：grill-with-docs（需求对齐）-> to-prd（生成 PRD）-> to-issues（拆分任务）-> tdd（实现功能）-> diagnose（诊断 bug）-> improve-codebase-architecture（架构优化）。

> [!quote] 🎬 可行动项
> - 通过 `/install mattpocock/skills` 安装，在项目根目录执行 `/setup` 初始化
> - 新需求启动时先用 grill-with-docs 进行需求对齐，生成 context.md 术语表
> - 每完成一个模块后运行 improve-codebase-architecture 检查架构优化点

---

## 精读

### 论证链

```
问题起点：AI 生成的代码跟你想的完全不是一回事
        ↓
AI 编程四大核心问题：
  ① Misalignment      — 理解偏差，AI 不知道你真正要什么
  ② Feedback Loops    — 缺乏反馈回路，出问题不知道
  ③ Software Entropy  — AI 加速代码产出，也加速软件熵增
  ④ Ubiquitous Language — 缺乏共享术语表，沟通混乱
        ↓
重流程框架的陷阱：
  GSD、BMAD、Spec-Kit 接管整个流程 → 拿走你的控制权
  流程里的 bug 变得很难定位和修复
        ↓
Matt Pocock 的设计哲学（Small / Composable / Hackable）：
  Small      → 单文件可读，几分钟看懂
  Composable → 自由组合，按需搭配
  Hackable   → 可改造，适配自己的场景
        ↓
技能分为三类：
  工程类：grill-with-docs / tdd / diagnose / improve-codebase-architecture / to-prd / to-issues
  生产力类：grill-me / caveman / handoff / write-a-skill
  杂项类
        ↓
推荐六步工作流（充值送积分案例）：
  grill-with-docs（20 个问题对齐需求）
  → to-prd（生成 PRD + context.md 术语表）
  → to-issues（拆分为 4 个 GitHub issues）
  → tdd（逐个实现功能）
  → diagnose（诊断 bug）
  → improve-codebase-architecture（发现 2 个优化点）
        ↓
核心结论：保持对工作的控制权
  AI 让写代码变快，但代码从来没变得更便宜
  AI 不会替你思考代码该怎么组织
```

### 关键引述

> "这些工具试图通过接管整个流程来帮你。但在这个过程中，它们拿走了你的控制权，流程里的 bug 也变得很难解决。"

> "AI 让写代码变快了，但代码从来没变得更便宜。代码会腐烂、会积累技术债、会需要维护。AI 只是让你更快地产出代码，但不会替你思考代码该怎么组织。"

### 局限与盲区

- 本文未覆盖：技能组在大型团队（10 人以上）中的协作实践、与 CI/CD 管线的深度集成、多项目并行管理。技能组主要面向个人开发者和单项目场景。
- 隐含假设：用户使用 Claude Code 作为主要开发工具，且项目使用 GitHub 进行 issue 追踪。对使用 GitLab、Bitbucket 或其他工具链的团队适配性未讨论。
- 可能的反例：对于简单的脚本型项目，完整走一遍六步工作流可能过度工程化；grill-with-docs 的 15-20 个问题对于"快速原型验证"场景来说过于冗长。

---

## 关联

- [[AI商业]]
- [[AI工程团队管理]]
- [[ClaudeCode斜杠命令]]
- [[顶级Skill设计]]
- [[AgentHarness架构]]
