---
Belongs to: "[[Agent工程]]"
aliases: []
tags: ["skill-engineering", "skill-optimization", "agent-tools"]
created: 2026-06-02
source: ai-generated
source_url: "https://x.com/hooeem/article/2061528919786791154"
concepts: ["SkillOpt", "text-space optimizer", "skill optimization", "validation gate", "bounded edits", "skill portability", "answer key", "textual learning rate"]
confidence: medium
---
# SkillOpt自进化技能优化

> [!abstract]- AI 摘要
> 微软 SkillOpt 是一种文本空间优化器，将 Agent 技能文件视为可训练对象：用优化模型对技能文本提出有限编辑，通过验证门控在留出集上检验效果，只保留提升分数的编辑。最终产出是一个可读、可审计、可跨模型跨平台迁移的紧凑技能文件。

---

## 扫读

> [!tip] 💡 一句话
> SkillOpt 将 Agent 技能从"手工猜测修改"升级为"训练-验证-保留"的优化循环：你提供带正确答案的示例数据集，它自动对技能文本做小步编辑，只有测出分数提升的编辑才会被保留，最终输出一个经验证的最优技能文件。

> [!important] 📌 关键结论
> - SkillOpt 是"文本空间优化器"——不改变模型权重，不修改单次 prompt，而是优化模型和任务之间的持久技能文档，四个组件：冻结目标模型、优化模型、有限编辑预算（文本学习率）、验证门控
> - 微软论文在 6 个基准测试、7 个目标模型、3 种执行模式的 52 个实验单元中全部最优或并列最优，GPT-5.5 上六个基准平均从 58.8 提升至 82.3（+23.5 分）
> - 单次运行仅产生 1-4 次被接受的编辑，最终技能文件 380-2000 token，小到可以几分钟内读完并审计
> - 训练出的技能具有跨平台可移植性：在 Codex 中训练的表格技能迁移到 Claude Code 仍带来 +59.7 分的提升
> - 最大收益来自程序性任务（模型有能力但马虎的任务），而非需要更多原始知识的任务

> [!quote] 🎬 可行动项
> - 找出一个已有手工技能且"有标准答案"的任务（提取、分类、结构化生成、带参考答案的问答），准备 20-40 个示例，按 4:1:5 拆分为 train/val/test
> - 借用现有 benchmark 的数据格式（首选 SearchQA 格式），将任务表达为 question + context + answers 的 JSON 结构
> - 用相同模型同时做 target 和 optimizer 跑一次低成本验证（num_epochs=1，batch_size=数据集大小），观察验证分数是否有提升
> - 将已有手工技能文本粘贴到 config yaml 中作为初始技能，让 SkillOpt 进化现有技能而非从零构建

---

## 精读

### 论证链

```
问题：当前技能优化全靠手工 —— 加规则、删行、跑评估、测试、观察 Agent 行为、重复。缺乏量化反馈，修改是否有效纯靠猜测。

方案：SkillOpt = 文本空间优化器，将技能文件视为可训练的"参数"。

循环四组件：
1. 冻结目标模型用当前技能跑任务
2. 优化模型（第二 LLM，仅训练时使用）读取成功和失败案例，提出结构化编辑（添加规则、删除行、替换内容）
3. 有限编辑预算（默认 4，逐步衰减到 2）限制每次变化的幅度，防止覆盖已有有效规则
4. 验证门控在留出集上测试编辑后的技能，只接受严格提升分数的编辑，拒绝的编辑被记录以防止重复提议

实验结果：52/52 实验单元全部最优或并列最优。程序性任务收益最大（模型"会做但不严谨"的场景），技能可跨模型、跨执行平台迁移。

工程实践：
- 数据准备是最关键的输入：你需要准备带正确答案的示例，答案键决定一切
- 首次运行用 20-40 个示例、同模型双角色、单 epoch 验证效果
- 已有技能贴入 config 让 SkillOpt 进化而非重建
- 部署：best_skill.md 直接作为技能文件使用，无额外推理开销

训练成本：每个测试分数点的提升需要 0.6M-46M token（取决于任务复杂度），但这是一次性成本。
```

### 关键引述

> "SkillOpt treats that file as something you can train, like training a model, except the thing being trained is the text, not the weights."

> "Bounded edits keep each version close to the last, so the skill accumulates improvements instead of thrashing."

> "Skills stop being disposable prompts you rewrite on instinct every time something breaks, and become assets you train, validate, keep, and carry forward."

> "The answer key is the whole bloody game. everything downstream trusts it."

### 局限与盲区

- 本文未覆盖：对于没有"标准答案"的开放式任务（创意写作、策略建议、设计评审）SkillOpt 完全不适用，作者在开头就做了过滤但未讨论替代方案。LLM-as-judge 评分器的具体实现细节和可靠性数据——文中提到它"更难、更不可靠"但未给出量化对比。多轮交互 Agent 任务（需要多工具调用链才能得到最终结果）的评分方案
- 隐含假设：假设用户有能力准备高质量的示例数据集——但现实中"正确答案"的获取成本可能远高于手工调优技能本身。假设优化模型提出的编辑是安全的且不会引入逻辑矛盾——虽然验证门控能挡掉分数不提升的编辑，但无法检测"分数提升但引入了新的边界错误"的情况。假设同一任务的不同示例之间是独立同分布的，对于有顺序依赖的任务可能不成立
- 可能的反例：对于非常简单、已经高度优化的技能（如格式模板），SkillOpt 可能找不到提升空间，训练成本成为沉没成本。对于非英语技能文本，优化模型可能因为训练数据偏差而提出不符合语言习惯的编辑。技能的可移植性虽然在论文中得到了验证，但在高度依赖特定平台 API 的技能上（如特定 MCP 工具的调用方式），跨平台迁移的实际收益有限
- 训练成本提到的"一次性"忽略了技能随模型版本更新、工具变更、需求变化而需要重新训练的场景——在实际工程中，技能可能需要反复训练
- 未讨论多技能之间的冲突和组合优化问题：当多个技能同时加载时，可能存在指令冲突，而 SkillOpt 的单技能优化范式无法处理这一场景

---

## 关联

- [[顶级Skill设计]] —— 手工技能拆分与设计方法论，SkillOpt 的输入基础
- [[ClaudeSkill本质]] —— 技能作为 Agent 核心架构资源
- [[提示词工程九原则]] —— 从手工 prompt 工程到自动化优化
- [[Agent开发十大核心概念]] —— 技能在 Agent 工程体系中的定位
- [[Harness自进化]] —— 自进化系统的另一种实现路径
- [[LongCoT思维分子结构]] —— 模型推理与技能指令的关系
- [[Agent记忆升级实录]] —— 技能文件作为 Agent 外部记忆的一种形式
