---
Belongs to: "[[Agent工程]]"
aliases: ["Full Stack AI Engineer", "AI工程师成长路径", "提示工程到系统工程"]
tags: [AI工程, 提示工程, 技能成长]
created: 2026-05-31
source: ai-generated
source_url: "https://x.com/hooeem/article/2059640615344754947"
concepts: ["上下文工程", "AI工程栈", "结构化输出", "检索增强", "Guardrail系统", "评测体系", "Agent vs Workflow决策"]
confidence: medium
---
# 提示工程师到全栈AI

> [!abstract]- AI 摘要
> 从"提示工程师"升级为"全栈AI工程师"的核心转变：从关注"对模型说什么"到设计"模型需要知道什么、能做什么、如何验证、如何安全运行"的完整系统。

---

## 扫读

> [!tip] 💡 一句话
> 全栈AI工程师不再问"用什么prompt"，而是问"这个系统的目的层、上下文层、输出层、检索层、工具层、护栏层、评估层、日志层分别是什么"。

> [!important] 📌 关键结论
> - 提示工程未死，但"只靠提示"的思维已死——prompt仍是人机接口，但必须与上下文工程、工具调用、护栏等层协同
> - 11层AI工程栈：目的层→提示层→上下文层→输出层→检索层→工具层→Workflow/Agent层→评估层→护栏层→日志层→改进层
> - 区分Workflow（步骤可预测、风险低、可靠优先）与Agent（步骤需动态发现、开放式搜索、须安全护栏）是核心架构决策

> [!quote] 🎬 可行动项
> - 建立个人的AI工程模板库（目的层/提示层/上下文层/输出层/检索层/工具层/护栏层），每次新任务直接套用
> - 为每个重要AI系统定义自动失败条件（发明事实、忽略格式、执行禁止操作、无证据的高置信度）
> - 区分手头任务属于Workflow还是Agent场景，不可默认选择Agent

---

## 精读

### 论证链

```
层次1（基础提示技能）：Specificity（具体性）→Roles（角色定义）→Examples（示例）→Reasoning（推理控制）→Output Control（输出格式）→Iteration（迭代）→Constraints（约束）
      ↓
层次2（上下文工程）：从"对模型说什么"转向"模型需要知道什么"，包含目标/受众/项目背景/源材料/偏好/约束/历史决策/已知失败模式
      ↓
层次3（外部能力）：检索（RAG/搜索/文件）→工具调用（Function Calling）→MCP协议连接外部系统
      ↓
层次4（系统架构）：Workflow vs Agent 决策→评估体系（评分标准/自动失败条件/回归测试）→护栏（允许/需批准/禁止三层权限）→Image & Multimodal Prompting
      ↓
层次5（完整AI工程栈）：定义目的层→提示层→上下文层→输出层→检索层→工具层→架构层→评估层→护栏层→日志层→改进层，每层有可复用模板
      ↓
核心转变：从一次性prompt心态→可重复、可评估、可改进的AI系统心态
```

### 关键引述

> Stop thinking "what prompt should I use now?" Start thinking "what can I do to make this output reliable every week?"

> Prompting asks: What should I say to the model? Context engineering asks: What does the model need to know to do the job well?

> Do not build an agent by default. An agent is not automatically better than a workflow. A workflow is better when the path is predictable. An agent is better when the path must be discovered dynamically.

> Guardrails are not there to make the system polite. They are there to make the system controlled.

### 局限与盲区

- 本文未覆盖：11层架构对于简单任务（如"帮我写封邮件"）是否过度工程化；不同层面的维护成本和迭代频率差异
- 隐含假设：用户有时间和意愿为每个AI系统建立完整的11层架构——实际上大多数使用场景仍是对话式即兴交互
- 可能的反例：许多高效用户仅用精心设计的CLAUDE.md和Skill就能达到类似效果，无需完全搭建11层架构；过度结构化的prompt可能限制模型发挥其推理能力；Karpathy本人的方法（Claude+文件上下文）相对简单但极有效

---

## 关联

- [[提示词工程九原则]]
- [[CLAUDE.md优化规则]]
- [[Agent开发十大核心概念]]
- [[世界级Agentic工程师方法论]]
- [[Agentic设计模式]]
- [[编排税]]
- [[ClaudeCodeHooks管理]]
