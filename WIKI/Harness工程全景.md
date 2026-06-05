---
Belongs to: "[[Agent工程]]"
aliases: ["Harness Engineering全景", "The Harness Is Everything", "ACI设计原则"]
tags: ["harness-engineering", "agent-architecture", "aci"]
created: 2026-06-02
source: ai-generated
source_url: "https://x.com/rohit4verse/article/2033945654377283643"
concepts: ["Agent-Computer Interface (ACI)", "harness vs model", "progressive disclosure", "git worktree isolation", "mechanical architecture enforcement", "integrated feedback loops", "context window as consciousness"]
confidence: medium
---
# Harness工程全景

> [!abstract]- AI 摘要
> 从 SWE-agent 论文的 ACI 概念到 Anthropic 的长期 Agent 架构再到 OpenAI Codex 的零人工代码实验，三条独立工程路线得出同一个结论：模型能力已接近商品化，决定 Agent 实际表现的是 Harness——上下文管理、工具接口、反馈回路和架构约束构成的环境设计。

---

## 扫读

> [!tip] 💡 一句话
> 模型是推理引擎，Harness 是推理引擎所处的完整环境——包括上下文结构、工具接口、反馈机制和架构约束。决定 Agent 能否可靠工作的不是选哪个模型，而是环境设计。

> [!important] 📌 关键结论
> - SWE-agent 论文用 GPT-4 做对照实验，仅更换 ACI 接口就从 3.97% 提升到 12.47% 的 issue 解决率（64% 相对提升），未改动模型任何参数，证明了接口设计是 Agent 性能的第一性杠杆
> - Anthropic 解决跨上下文窗口长任务的方案是"初始化 Agent + 编码 Agent"双架构：初始化 Agent 创建 feature list（200+ 条可验证功能描述）和 init.sh 环境启动脚本，后续编码 Agent 按单条 feature 增量推进，每 session 结束时必须 commit + 更新 progress file
> - OpenAI Codex 团队用 3 人 5 个月通过 Agent 生成 100 万行代码、1500+ PR，核心经验是将仓库本身作为 system of record——用短 AGENTS.md（~100 行）做入口地图，深层知识分布在结构化 docs/ 目录，配合 git worktree 隔离实现并行 agent 作业

> [!quote] 🎬 可行动项
> - 用 Harness 思维替代提示词思维审计 Agent：不问你该写什么 prompt，问 Agent 需要但拿不到的信息是什么、缺失的反馈回路是什么、上下文在哪里被污染
> - 为跨 session 长任务添加最小三件套：persistent progress file（每次读/写）、structured task list（可验证的 binary 完成状态）、每 session 结束必须 git commit
> - Web 应用开发 Agent 必须接入浏览器自动化（Puppeteer/CDP），代码级验证和端到端用户级验证之间的差距是未检测 bug 的最大来源

---

## 精读

### 论证链

```
核心命题：模型能力已近商品化 → Harness 是区分因子
  SWE-agent 论文：同一 GPT-4，ACI 替换 bash → 3.97%→12.47%（64% 提升）
  OpenAI Codex：100 万行代码/1500+ PR，0 行人类代码，关键是环境设计
  Anthropic 内部实验：Opus 4.5 裸跑→失败，加 harness→数月增量交付
        ↓
认知基础：context window ≠ RAM，是 agent 的"工作意识"
  token 噪声竞争注意力 → 模型无选择性忽略机制
  SWE-agent 的 capped search（50 条上限+强制精炼）→ 单次设计决策最高杠杆
        ↓
SWE-agent ACI 四组件：
  ① 搜索：capped results + 超过阈值强制细化 → 阻断 context flooding
  ② 文件查看器：100 行窗口 + 行号显式标记 + 有状态位置 → 减少认知负荷
  ③ 编辑器 + linter：编辑即检查，语法错误在引入点阻断 → 防止级联失败
  ④ 上下文管理：>5 轮旧观察压缩为单行摘要 → 保持活跃上下文干净
        ↓
Anthropic 跨窗口长任务架构：
  初始化 Agent：init.sh + feature list（JSON，200+ end-to-end 功能）+ progress file + git init
  编码 Agent：单 feature 推进 + session 结束 clean state + commit + 更新 progress
  关键设计：feature list 用 JSON 非 Markdown（模型更不易随意修改）+ passes: true/false 显式消除"部分完成看起来像完成"
        ↓
OpenAI Codex 的企业级实践：
  仓库即 system of record：短 AGENTS.md 作地图 → 深层 docs/ 目录结构化分布
  progressive disclosure：agent 从稳定入口开始，被教会去哪找更多而非一次倾倒
  应用可观测性：每 agent task 独立 git worktree + 隔离的 logs/metrics/traces + Chrome DevTools Protocol
  架构约束机械化：custom linter + structural tests 取代 PR review，linter 错误信息含修复指令注入 agent context
        ↓
Awesome Agent Harness 七层分类法：
  L1 人类监督 → L2 Spec 工具 → L3 全生命周期平台 → L4 Task Runner
  → L5 Agent 编排器（git worktree 隔离）→ L6 框架与运行时 → L7 编码 Agent（商品层）
        ↓
五大跨系统重复设计模式：
  ① Progressive Disclosure：短入口 + 深度指针，不一次倾倒
  ② Git Worktree 隔离：一 agent 一 worktree，并行安全
  ③ Spec First + 仓库即 system of record：意图在 repo 中机器可读
  ④ Mechanical Architecture Enforcement：linter+structural test 取代 review
  ⑤ Integrated Feedback Loops：编辑即 lint → 运行即观测 → 浏览即验证
        ↓
终局判断：模型是思考什么，Harness 是思考什么关于——长期 moat 在 Harness 不在模型
```

### 关键引述

> "The model is almost irrelevant. The harness is everything."

> "The interface is not a convenience layer. For an LM agent, the interface is the mind."

> "When you run grep on a large codebase from inside an agent loop and return ten thousand lines of matches, you have not given the agent more information to work with. You have flooded its working memory."

> "The primary job of the engineering team became enabling the agents to do useful work, not doing the work themselves."

> "The execution layer is a commodity. The agent's effectiveness is primarily determined by everything above it in the stack."

### 局限与盲区

- 本文未覆盖：小型项目（单人、非长期维护）是否值得投入完整 Harness 架构的成本收益分析；非编码场景（研究、分析、客服）的 ACI 设计原则差异；不同 LLM 对同一 Harness 设计的敏感度差异（GPT vs Claude vs Gemini 的行为差异）
- 隐含假设：团队有能力维护 Harness 本身（linter 规则、docs/ 目录、CI 配置）的持续更新——Harness 的腐烂同样会导致 Agent 性能退化；假设 Agent 的主要应用场景是软件工程，其他垂直领域（医疗、法律、金融）的 Harness 设计可能遵循不同约束
- 可能的反例：对于一次性任务或探索性原型（<5 次 session），完整的初始化 Agent + feature list 架构可能是过度投资；随着模型上下文窗口持续扩展（数百万 token），context management 的部分策略可能从必选项降级为可选项；Anthropic 和 OpenAI 的内部实践均有未公开的失败案例，公开发表的都是成功经验，存在幸存者偏差

---

## 关联

- [[AgentHarness架构]]
- [[Harness工程控制论]]
- [[Agentic设计模式]]
- [[Agent开发十大核心概念]]
- [[长任务Agent工程闭环]]
- [[Agent最简实现原理]]
- [[企业级Agent构建指南]]
- [[编排税]]
- [[世界级Agentic工程师方法论]]
- [[ClaudeCodeHooks管理]]
