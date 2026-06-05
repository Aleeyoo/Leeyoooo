---
Belongs to: "[[Agent工程]]"
aliases: []
tags: ["agent-practice", "agent-pitfalls"]
created: 2026-06-02
source: ai-generated
source_url: "https://x.com/gkisokay/article/2059986597391823286"
concepts: ["multi-agent architecture", "research-first agent", "agent cost tracking", "model diversity", "agent maintenance", "supervisor agent", "building in public"]
confidence: medium
---
# Agent构建21个惨痛教训

> [!abstract]- AI 摘要
> 作者 Graeme 总结三个月每日构建 Hermes 和 OpenClaw Agent 的 21 条实战教训，核心主张是：Agent 工程的成败不取决于模型能力，而取决于架构设计、成本控制、可靠性基础设施和持续维护。

---

## 扫读

> [!tip] 💡 一句话
> 构建 Agent 时最容易犯的 21 个错误，每个错误对应一条经过实战验证的逆向教训，核心是"先建研究层、再建执行层、永远监控成本"。

> [!important] 📌 关键结论
> - 不要构建单一巨型 Agent，应拆分为各司其职的 Agent 团队，每条任务线有明确的归属和边界
> - 先构建研究型 Agent 作为输入智能层，它收集的结构化信息会成为后续所有 Agent 的训练数据和决策依据
> - 自主循环必须配备成本追踪，本地模型处理低风险任务，前沿模型只用于规划、调试和硬推理，模型多样性是韧性保障
> - Agent 如果不持续维护会退化——工具更新、模型迭代、MCP 变更后必须做周度审计
> - 模型不是产品，围绕模型的系统（研究、路由、记忆、监督、反馈循环、自我改进）才是产品

> [!quote] 🎬 可行动项
> - 审视现有 Agent 架构：是否存在一个巨型 Agent 承担了过多职责？是否有独立的研究层？
> - 为所有 Agent 运行添加成本日志，记录单次运行的确切成本
> - 建立周度 Agent 审计清单，检查工具兼容性、模型变更、MCP 更新
> - 区分任务层级：用本地/廉价模型做扫描、总结、头脑风暴；用前沿模型做规划和调试

---

## 精读

### 论证链

```
核心论点：Agent 工程失败的主因不是模型不够聪明，而是架构和运维基础设施缺失。

论证递进：
1. 架构层面：单一巨型 Agent 难以调试和信任 → 应拆分为专职 Agent 团队，每个 Agent 承担明确职责
2. 输入层面：Agent 需要结构化、可验证的输入而非原始抓取数据 → 研究 Agent 作为输入智能层，路由发现到各执行 Agent
3. 执行层面：自主工作流必须有监督者监控预期流程 vs 实际流程，捕获失败并中途修复
4. 成本层面：自主循环运行 24/7 后，单次运行成本成为关键指标 → 区分模型使用场景，引入本地模型降低成本
5. 可靠性层面：明确"完成"的定义，要求 Agent 测试、验证、引用证据，信任来自证据而非自信
6. 维护层面：Agent 随时间退化 → 周度审计 + 持续迭代
7. 元认知层面：先建自我思考层（Auto-think）再建自我构建层（Auto-build），思考层负责识别摩擦、失败运行、缺失工具和重复瓶颈
```

### 关键引述

> "Don't think the model is the product. The system around the model comprises research, routing, memory, supervision, feedback loops, and self-improvement."

> "The magic comes after boring infrastructure like clean inputs, clear handoffs, monitoring, recovery, evals, and cost control."

> "Building in public is a magical thing. The truth is that no one really knows what's best, and it's up to you to figure it out and connect with others on your journey along the way."

### 局限与盲区

- 本文未覆盖：21 条教训均为方向性建议，缺乏具体的实现代码、架构图和量化对比数据。例如"多 Agent 协作"没有讨论 Agent 间的通信协议、状态共享机制和冲突解决策略
- 隐含假设：假设读者已具备 Agent 开发基础（熟悉 OpenClaw/Hermes 生态），且拥有一定预算来运行多模型组合。假设"研究 Agent"是普适的最佳第一步，但对执行确定性任务（如 CI/CD）的 Agent 系统未必成立
- 可能的反例：对于简单、单一领域的 Agent 应用（如单一定时报告生成），多 Agent 架构反而是过度设计。某些场景下前沿模型成本已经极低，模型分层策略的收益可能被额外的调度复杂度抵消
- 作者的工具链高度依赖特定生态（OpenClaw + Hermes + ClawHub），部分建议（如第 6 条"尽早用 Hermes 监管 OpenClaw"）对其他 Agent 框架的普适性存疑

---

## 关联

- [[多Agent分工协作]] —— 多 Agent 团队拆分策略
- [[编排税]] —— 多 Agent 协作的调度成本
- [[世界级Agentic工程师方法论]] —— Agent 工程系统思维
- [[Agent开发十大核心概念]] —— Agent 工程核心概念体系
- [[顶级Skill设计]] —— Agent 技能拆分与设计
- [[长任务Agent工程闭环]] —— 自主循环的监控与可靠性
- [[Agent项目落地难]] —— 从原型到生产的基础设施差距
