---
Belongs to: "[[AI技术原理]]"
aliases: ["世界模型功能分类", "World Model Taxonomy", "Fei-Fei Li世界模型"]
tags: ["World Model", "空间智能", "3D生成", "具身智能", "模拟仿真", "Fei-Fei Li"]
created: 2026-06-06
source: x-article
source_url: "https://x.com/drfeifei/article/2062247238143996275"
concepts: ["POMDP循环", "Renderer渲染器", "Simulator模拟器", "Planner规划器", "Simulation-as-linchpin", "Unified World Model", "Sim-to-real gap", "空间智能长弧"]
confidence: high
---
# World Model功能分类法

> [!abstract]- AI 摘要
> Fei-Fei Li 与 World Labs 团队提出世界模型的功能分类法：渲染器（输出像素）、模拟器（输出几何物理状态）、规划器（输出动作），三者共享同一套底层知识（几何、物理、动力学），边界正在消融，终点是统一世界模型。

---

## 扫读

> [!tip] 💡 一句话
> "世界模型"是AI领域最被滥用的词——视频模型、语言模型、物理引擎各说各话；本文将其按功能输出拆为三类：Renderer输出观测（给人看）、Simulator输出状态（给人和程序计算）、Planner输出动作（给Agent执行），Simulator是三者中关注度最低但最关键的桥梁。

> [!important] 📌 关键结论
> - World Model的三功能分类：Renderer（视觉保真，商业最成熟）、Simulator（几何物理保真，最被低估）、Planner（动作决策，最前沿但最不成熟），三者并非割裂而是共享同一底层世界知识
> - POMDP循环（Agent-Action-State-Observation）是理解"世界模型"术语分歧的元框架——不同领域说的"世界模型"本质是在输出这个循环的不同片段
> - Simulator是Renderer和Planner之间的桥梁——既能为渲染提供可信结构，又能为规划提供动作后果预测；只会渲染或只会规划的模型无法做到对方的事
> - 三类边界正在消融：渲染器变得可交互（action-conditioned）、模拟器输出可控可编辑世界、规划器从反应式走向深思熟虑，终点是单一基础模型切换输出模态
> - 核心挑战：3D标注数据极度稀缺（vs互联网视频海量）、sim-to-real gap、生成式模拟器可能生成"看起来对但物理上错"的几何、多物理场联合模拟计算成本极高

> [!quote] 🎬 可行动项
> - 评估任何自称"世界模型"的产品/论文时，先问：它的输出是什么？像素（Renderer）、状态（Simulator）、还是动作（Planner）？不要被同一个词混淆
> - 关注Simulation方向的技术进展和基础设施投资（NVIDIA Omniverse等），这是连接视觉生成和具身智能的战略咽喉
> - 如果你是Agent/机器人方向从业者，警惕仅靠Renderer生成的训练数据——视觉保真不等于物理正确

---

## 精读

### 论证链

```
**问题起点：术语通胀** → "世界模型"一词被CV、RL、GenAI三个领域各自使用但含义完全不同（视频模型 vs 语言模型即兴游戏 vs 物理引擎），AI领域在需要精确性的时刻继承了古希腊人讨论"世界由什么构成"时的混乱。

**元框架引入：POMDP循环** → 追溯到Sutton & Barto的强化学习经典框架：Agent执行Action → 影响State → 产生Observation → 驱动新Action。State是物理学家意义的"世界完整描述"（所有对象、位置、速度、属性），Observation是Agent的局部视图。不同"世界模型"本质是输出这个循环的不同片段。

**功能分类法：按输出分三类** →
- **Renderer（渲染器）**：输出Observation（像素），契约为视觉保真。典型产品：视频生成、Genie 3、World Labs RTFM。局限：建筑从空中看完美，开进去就塌了——没有三维结构理解。
- **Simulator（模拟器）**：输出State（几何+物理+动力学），契约为结构保真。服务两个消费者：人类专业人士（建筑师、设计师）和计算机程序（RL Agent、机器人控制器）。必须经得起检验、遵守牛顿定律。
- **Planner（规划器）**：输出Action，是Renderer的逆过程。Renderer：Action→Observation；Planner：Observation→Action。VLA模型、World Action Model都是此类。

**Simulator是战略枢纽** → Renderer商业最成熟（Google Nano Banana数亿用户）但优化视觉可信而非物理精度；Planner最前沿但全在实验室受限场景（窄物体集、短任务时长），与真实部署差距巨大。Simulator是二者的结构骨架——掌握了Simulation就能向两个方向投影。NVIDIA Omniverse瞄准超万亿美金市场。

**边界消融与统一模型** → 最新研究趋势：预训练视频渲染器作为世界+动作联合预测的backbone（Renderer→Planner桥梁）、Marble同时输出高斯泼溅和碰撞网格（Renderer+Simulator融合）、三者都从被动输出走向交互系统。逻辑终点：统一世界模型，根据下游消费者需求切换输出模态。

**硬核Open Problems** →
- 数据不均：Renderer有海量互联网视频，Simulator和Planner极度缺乏3D资产和机器人演示数据
- 视觉美丽vs物理精度的张力：优化一个可能牺牲另一个
- 生成式模拟器的"看起来对但物理错"风险
- 多物理场联合模拟计算成本呈数量级增长
```

### 关键引述

> Language models have given machines an extraordinary command of concepts, vocabulary, and reasoning, but the physical world, virtual or real, runs on a different substrate. Where language models learn the statistical structure of text, world models learn the statistical structure of space and time.

> A renderer outputs observations in the form of pixels meant for human eyes, and the quality that matters most is visual fidelity. [...] The model carries no explicit understanding of three-dimensional structure. It produces what a viewer would see, not what is.

> A simulator outputs state: a geometrically, physically or dynamically faithful representation of the world that humans and computer programs can both compute on and interact with.

> If language is an abstraction of the world and pixels are a projection of it, then geometry, physics, and dynamics are the world itself. A simulator must work at that level: the structural backbone from which both visual appearance (for renderers) and action consequences (for planners) can be derived.

> The most important pattern in the field right now is that the three categories are starting to blend into one another. The shared insight is that the knowledge required to render a world, simulate it, and act in it is largely the same.

> The logical endpoint is a unified world model: one foundation model that can render photorealistic views, produce physically accurate structure, and plan action sequences, switching between output modalities depending on what the downstream consumer needs.

### 局限与盲区

- 本文未覆盖：三类功能的具体技术实现路径和架构对比（Transformer、扩散模型、NeRF/3DGS等各自主导哪一类）；不同应用场景（游戏、影视、工业仿真、医疗）对三类功能的质量标准差异；训练数据配比和混合训练策略的实操细节；开源vs闭源世界模型的技术栈对比
- 隐含假设：假设Simulator向Renderer和Planner的"投影"是双向可行的（即掌握了物理状态就能反向生成像素和规划动作），但这条路径在技术上尚未被大规模验证；假设商业市场会自然为Simulator付费（但渲染和规划有更直接的市场需求，仿真作为"中间件"的商业模式仍在验证中）；文章立场来自World Labs（正在构建Marble模拟器），对Simulator战略地位的论述带有机构利益视角
- 可能的反例：如果端到端的Renderer→Action路径（即跳过显式Simulator直接从视觉到动作）被证明足够有效，Simulator的战略枢纽地位可能被绕过；扩散模型和视频生成模型在零样本条件下展现出的"涌现物理理解"可能模糊Renderer和Simulator的边界；纯语言世界模型（如LLM通过文本推理物理）在某些抽象规划任务中可能比3D模拟更高效

---

## 关联

- [[Agent最简实现原理]] — Planner作为Agent的核心能力：从观测到动作的闭环
- [[Agent本质一文讲清]] — Agent的"感知-决策-执行"能力光谱对应World Model的Planner功能
- [[后摩尔工程方法论]] — 同为技术领域的分类框架方法论：将复杂系统按功能维度拆解、找到战略枢纽
- [[编排税]] — POMDP循环中"从观测到动作"的决策开销，Planner需要最小化的核心成本
- [[AI数据质量]] — Simulator的核心瓶颈：3D标注数据稀缺性
