---
Belongs to: "[[Agent工程]]"
aliases: ["Enterprise Context Layer", "企业上下文层", "Context Substrate"]
tags: ["context-layer", "enterprise-ai", "agent-infrastructure"]
created: 2026-06-02
source: ai-generated
source_url: "https://x.com/prukalpa/article/2061512556590809342"
concepts: ["context layer", "knowledge expertise norms triplet", "context substrate", "context mining", "context SDLC", "compounding learning loops", "semantic vs episodic memory"]
confidence: medium
---
# 企业级Context Layer架构

> [!abstract]- AI 摘要
> 企业 Context Layer 不是数据目录、不是语义层、不是知识图谱——它是将企业的 knowledge（知识）、expertise（专业技能）和 norms（规范约束）转化为机器可用上下文的系统，由三部分 substrate（数据+语义+技能）和五项能力组成。第十个 Agent 比第一个更聪明的关键，在于这个层在每次交互中学习并积累。

---

## 扫读

> [!tip] 💡 一句话
> Context Layer 是 Agent 之间的"共享企业大脑"——每个 Agent 从中获取知识、按规则行事、并将经验回写，使组织级的 AI 能力产生复利效应，而非每次从零开始。

> [!important] 📌 关键结论
> - Agent 在企业中运作需要三种 context：Knowledge（实体、指标、关系的"商业地图"）、Expertise（SOP、工作流、隐性 know-how）、Norms（权限、审批路径、合规约束）。三者缺一不可——知道"收入"的定义不等于知道"怎么关账"，知道流程不等于知道能批多少折扣
> - Context Substrate 由三部分组成：AI-ready 数据+知识图谱（可信数据源+规范知识文档）、语义+本体（概念定义与关系结构）、技能（可复用、可版本化的 how-to 单元）。三者必须集成——数据无语义不可解释，语义无数据是空中楼阁，缺技能则只能分析不能操作
> - 五项能力构成操作层：Context Mining（从系统日志、查询历史、Agent 轨迹中逆向挖掘隐性知识）、Context SDLC（上下文从创建到退役的完整生命周期管理）、Compounding Learning Loops（每次交互沉淀为持久语义/程序记忆）、Activation & Retrieval（多协议分发到不同 Agent 框架）、Governance & Observability（质量/漂移/血缘/版本/审批五维治理）

> [!quote] 🎬 可行动项
> - 审计当前 Agent 系统缺失的 context 类型：Agent 能查到数据定义（knowledge）吗？知道实际工作流（expertise）吗？知道当前用户的权限边界（norms）吗？
> - 禁止"一个大 AGENTS.md"的反模式：改为短入口文件（~100 行）作为知识地图，指向深层结构化文档，利用 progressive disclosure 避免上下文污染
> - 建立 context 的变更传播机制：一个核心定义（如 ICP 重新定义）变更后，所有下游 skill 和 Agent 如何感知并更新？明确 queue for review / auto-propagate / leave old version 的策略

---

## 精读

### 论证链

```
问题定义：CIO 每天都在问——Context Layer 到底是什么？
  不是数据目录（为人类分析师设计），不是语义层（仅覆盖指标和维度），
  不是知识图谱（缺少技能和流程），不是长期记忆（只是五项能力中的一小部分）
        ↓
Agent 需要的三种 Context：
  Knowledge → 商业地图（实体、定义、指标、关系）
  Expertise → 如何完成工作（流程、SOP、隐性 know-how）
  Norms → 什么行为被允许（权限、审批路径、合规约束）
        ↓
Core Context Substrate 三部分：
  ① AI-ready 数据 + 知识图谱：结构化数据+描述+join路径+规范知识文档
     关键但被低估：canonical knowledge（战略文档、品牌定位、产品理念）
  ② 语义 + 本体：共享定义（词汇表）+ 概念关系结构（Customers→Accounts→Transactions）
     新变化：AI 可以持续构建和策展这些知识，读取查询日志并调和冲突文档
  ③ Skills：程序性知识的新原语——可命名、可版本、可测试、可从任意 Agent 调用的 how-to 单元
     类比：代码中的函数之于逻辑 = Skills 之于程序性知识
        ↓
五项能力（操作层）：
  ① Context Mining：从 SQL 查询历史发现 Sales/Finance 对 ARR 的定义冲突→AI 提炼→人类仲裁
      技能挖掘更困难（大部分未写下来）→系统主导法：Agent session 后提取+事件日志建流程+结构化 AI 访谈
  ② Context SDLC：context 需要完整的"创建→测试→审批→部署→退役"生命周期
      关键问题：变更传播——CMO 更新定位 skill 后，social media skill / SDR pitch skill / analyst call skill 如何响应？
  ③ Compounding Learning Loops：
      工作记忆/情景记忆 → 接近 Agent harness 执行层
      语义记忆/程序记忆 → 应在 Context Layer，由组织拥有和治理
      例：客服 Agent 发现客户儿子有乳制品过敏→暂时记忆→验证后升级为客户档案语义记忆→未来所有 Agent 无需重新发现
  ④ Activation & Retrieval：Context 需多协议分发——MCP、API、SQL、向量检索、图遍历
      核心架构判断：不应强迫所有生态说一种语言，应翻译规范上下文为多种本地方言
  ⑤ Governance & Observability：五个必须实时追踪的维度
      Quality（owner 已验证？）、Drift（底层世界是否变化？）、Lineage（来源及依赖方？）、
      Versioning（可回滚？两个 Agent 因版本不同产生分歧？）、Approval（谁能合并影响多团队的变更？）
        ↓
市场格局：Agent builder（垂直绑定）/ 平台（数据驻留限定）/ 单组件专家→
  未来将加速整合，因为共享性是 Context Layer 的核心价值——四个孤岛不是一层
```

### 关键引述

> "A skill is the new primitive that does for procedural knowledge what code did for logic in software."

> "Procedural knowledge in the enterprise is stuck where logic was before functions. The way you actually run the monthly close, qualify a lead, or handle a refund exception lives as prompts in someone's Notion doc, tribal memory, and steps people improvise differently each time."

> "The companies that win the next decade will not be the ones with the best models, because everyone will have access to the same models. They will be the ones whose context compounds, where the tenth agent is dramatically smarter than the first because the layer underneath it learned something every time."

> "A Fortune 500 with one layer for its CS agents, another for analytics, a third for memory, and a fourth for process mining does not have a context layer. It has four context islands."

### 局限与盲区

- 本文未覆盖：Context Layer 的构建和维护成本估算（人员、计算资源、时间投入），以及与其产出的 ROI 量化框架；不同行业（高度监管的金融、快速迭代的 SaaS、传统制造）对五项能力的需求优先级差异；与现有企业数据基础设施（Data Lake、MDM、ESB）的集成路径和迁移策略
- 隐含假设：企业已有足够的数据基础（数据仓库、API 化系统）来支撑 mining 能力——对于数字化转型早期的传统企业，Context Mining 可能因缺乏可挖掘的数据源而无从下手；假设企业愿意将隐性知识显性化并接受 AI 对"如何工作"的标准化——这本质上是组织变革而非技术问题
- 可能的反例：对于规模较小的组织（<200 人），完整的 Substrate+五项能力可能过度设计，简单的共享文档 + MCP server 或可覆盖需求；Context Layer 的"翻译为多方言"架构可能在工程上极其复杂，跨框架的语义保真度是未解决的难题；Skills 作为程序知识原语的设计前提是业务流程可被充分标准化——高度依赖人类判断的领域（如战略决策、创意评估）可能无法被 skill 封装

---

## 关联

- [[AgentHarness架构]]
- [[Harness工程控制论]]
- [[Agent记忆升级实录]]
- [[企业级Agent构建指南]]
- [[Agent开发十大核心概念]]
- [[长任务Agent工程闭环]]
- [[世界级Agentic工程师方法论]]
- [[PydanticAgent记忆]]
