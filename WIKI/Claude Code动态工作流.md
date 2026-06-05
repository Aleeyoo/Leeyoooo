---
Belongs to: "[[Agent工程]]"
aliases: ["Dynamic Workflow", "Claude Code工作流", "多Agent编排", "批量Agent调度"]
tags: ["Claude Code", "Dynamic Workflow", "SubAgent", "Agent Teams", "多Agent", "工作流", "编排"]
created: 2026-06-03
source: ai-generated
source_url: "https://x.com/aehyok/article/2061698672128348380"
concepts: ["Dynamic Workflow", "SubAgent独立执行", "Agent Teams双向通信", "流水线编排", "JavaScript工作流脚本", "并行Agent调度", "Claude Code四阶段进化", "token消耗阶梯"]
confidence: medium
---

# Claude Code动态工作流

> [!tip] 💡 一句话核心主张
> Dynamic Workflow 是一条自动化流水线，用 JavaScript 脚本批量调度几十上百个 SubAgent 并行执行独立子任务，将原本需要数月的工作压缩到几天，但它不是在"讨论"而是在"编排"——流水线上的工人互相不聊天。

> [!important] 📌 关键结论
> - Claude Code 的多Agent体系经历了四个阶段：单Agent → SubAgent（独立帮手，互不通信）→ Agent Teams（小团队，边干边沟通）→ Dynamic Workflow（大规模流水线编排），三者是梯度互补而非替代关系。
> - Dynamic Workflow 底层跑的是标准 SubAgent，Workflow 只在上面加了一层编排层；Agent Teams 用的是 Teammate（SubAgent 增强版，加了双向通信但代价是更高 token 消耗）。
> - 当前最大并行执行个数受限于本地系统配置，理论上限1000个，实际受电脑性能约束。每次触发工作流会先显示即将执行的内容并请求用户确认。

> [!quote] 🎬 可行动项
> - 决策流程：简单独立任务用 SubAgent，复杂需实时讨论的用 Agent Teams，规模大可拆分且流程固定的用 Dynamic Workflow，三者都不适用的直接单 Agent 会话。
> - 执行工作流后通过 `/workflows` 命令查看每个阶段的 Agent 数量、token 总数和耗时，经常用的流程按 `s` 键保存以复用。
> - 如果暂时不需要该功能或 token 消耗过高，通过 `/config` 关闭 Dynamic workflows，或在 `~/.claude/settings.json` 中设置 `"disableWorkflows": true`。

### 论证链

```
**观点一：Claude Code 的进化本质是从"一个人干活"到"一群人干活"**

文章梳理了四个阶段的进化路线。阶段一（单人模式）：一问一答，所有事情一个会话完成。阶段二（SubAgent）：主 Claude 可以派出一个或多个独立帮手，各干各的、互不通信，干完各自汇报。阶段三（Agent Teams）：一支分工协作的小团队，队员与队长、队员与队员之间双向实时通信，边干边调整。阶段四（Dynamic Workflow）：不再是团队讨论，而是自动化流水线，批量调度几十上百个工人各自在工位上干活，最后汇总成果。
```
### 观点二：SubAgent、Agent Teams、Dynamic Workflow 的核心差异在于"通信"与"规模"

三个概念的本质区别用餐厅类比说清：SubAgent 是叫几个帮厨同时干活但各干各的不商量；Agent Teams 是后厨团队，总厨带队，厨师之间随时喊话协调；Dynamic Workflow 是中央厨房流水线，工人各司其职、做完进入下一环节、互相不说话。通信成本与并行规模成反比：通信越多（Agent Teams），规模越小；通信越少（Workflow），规模越大。

### 实战机制：两种触发方式与可复用的工作流保存

触发动态工作流有两种方式：在 `/effort` 中选择 ultracode 模式，或在提示词中加入 "workflow" 标记。工作流执行后可通过 `/workflows` 命令查看每个阶段的 Agent 数量、token 总数和耗时详情。高频使用的工作流可按 `s` 键保存，下次直接复用。组织管理员可通过管理设置全局禁用工作流。

### Token 消耗与资源约束

动态工作流消耗的资源远高于普通 Claude Code 会话。最大并行执行个数理论上限 1000，但受限于本地系统配置。文章实测使用国产大模型 DeepSeek 而非 Claude 官方账号，以 146 个文档翻译为 5 种语言作为测试案例。

### 技术本质：JavaScript 脚本编排

动态工作流的核心技术是一段 JavaScript 脚本，Claude 为描述的任务编写该脚本，运行时在后台执行，确保主会话保持响应。工作流会动态制定计划，将任务拆分为多个子任务并分配给多个并行子 Agent，各子 Agent 从不同角度处理同一问题，其他 Agent 尝试反驳结果，整个迭代进行直至结果趋于一致。

## 关键引述

> "Dynamic Workflow 不是团队，而是一条自动化流水线。你用脚本定义好流程，它批量调度几十上百个 SubAgent 同时干活，各自完成工位任务，最后汇总成果。"

> "三者构成阶梯：任务越简单独立，越往 SubAgent 走；越需要讨论协作，越往 Agent Teams 走；越需要大规模批量处理，越往 Workflow 走。"

> "有些本来可能需要数月才能完成的工作，现在可能只需要几天就能搞定了。这就是 Dynamic Workflow 带来的变化。"

> "Dynamic Workflow 的每一份实际工作，底层都是标准 SubAgent 在干活。Workflow 只是加了一层编排层。"

## 局限与盲区

- **未覆盖方面**：文章未讨论工作流失败时的回滚和重试机制——当100个并行 SubAgent 中部分失败，编排层如何处理部分成功状态？也未涉及工作流脚本的可调试性，JavaScript 编排脚本出错时如何排查。组织级治理方面，除全局禁用外，未提及权限分级和用量配额控制。
- **隐含假设**：假设任务可被有效拆分为相互独立的子任务——实际上许多大规模任务（如代码库重构）的子任务之间有隐藏依赖，拆错粒度可能导致结果不一致。另一个假设是 SubAgent 的输出质量稳定，不需要中间质量把关——但编排层只说"逐一检查"结果，未说明检查标准是什么。
- **可能的反例**：对需要全局一致性的任务（如统一重构一个跨模块的接口），流水线式的独立处理可能产生相互矛盾的结果，拼合成本可能超过收益。对创造性任务（如设计新架构），"不同Agent从不同角度处理并相互反驳以趋于一致"的机制可能压制创新。
- **边界条件**：文章基于 Claude Code 生态系统，但基础概念（编排、并行、通信阶梯）在多 Agent 架构中普遍适用。国产大模型（文中用 DeepSeek）的 SubAgent 表现可能与 Claude 原生有显著差异，这会影响工作流的实际产出质量。

## 关联

- [[软件构建即学习]]——动态工作流的批量并行本质是加速"构建-反馈"循环的工程化手段
- [[离职工程师技能蒸馏]]——工作流中的 SubAgent 可加载蒸馏出的 SKILL.md 技能文件
- [[Pi Harness Agent开发]]——pi harness 提供了 Agent 的运行时底座，而 Dynamic Workflow 提供了编排层，两者可互补
- 所属项目：[[Agent工程]]
