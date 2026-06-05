---
status: processed
wiki: "[[WIKI/RealEngineer技能组]]"
---
# X 上的 Gyro：“别让重流程框架偷走你的控制权，这才是Real Engineers该用的技能组” / X

https://x.com/gyro_ai/status/2055198700016660826?s=20

你用 AI 写代码的时候，有没有遇到过这种情况：

跟 AI 聊了半天需求，它生成的代码跟你想的完全不是一回事。

改了几个版本，代码库里到处都是重复的类和方法。

辛辛苦苦构建的架构，没过多久就变成一团烂泥。

GitHub 上有个仓库，82.9k stars，专门解决这些问题。

## Matt Pocock 和他的技能组

![图像](https://pbs.twimg.com/media/HIVxBAoXYAAe226?format=png&name=large)

GitHub 仓库：[https://github.com/mattpocock/skills](https://github.com/mattpocock/skills)

作者是 **Matt Pocock**，前 Vercel 开发者布道师，Total TypeScript 作者，现在在做 AI Hero 平台。

他把自己凝聚力几十年的软件工程之魂直接“蒸馏”出来，把自己日常用 Claude Code 的工程实战流程直接开源了。

仓库名字很直接：**Skills for Real Engineers**。

这套技能组有三个核心设计哲学：

**Small** — 单文件可读，每个技能都很小，几分钟就能看懂。

**Composable** — 自由组合，可以单独用，也可以串起来用。

**Hackable** — 可改造，不喜欢哪个环节，直接改成你自己的。

为什么不用 GSD、BMAD、Spec-Kit 这些工具？Matt 说：

> "这些工具试图通过接管整个流程来帮你。但在这个过程中，它们拿走了你的控制权，流程里的 bug 也变得很难解决。"

我之前用过类似的工具，确实有这种感觉。

就比如 GSD 的工作流，头脑风暴、生成计划、执行计划、测试，一环扣一环，看起来很高大上。但是哪个环节出问题，你要改动非常麻烦。流程太重，改不动。

Matt 的方案是：用小的、可组合的技能，而不是一个大的流程框架。

![图像](https://pbs.twimg.com/media/HIVxG_jXwAAlj3P?format=jpg&name=large)

## 要解决的四个问题

Matt 的技能组针对 AI 编程的四个核心问题：

**1\. Misalignment（理解偏差）**

开发者和 agent 之间的理解不一致。你以为它懂了，其实它理解偏了。最后生成的代码跟你想的完全不是一回事。

**2\. Ubiquitous Language（缺少共享语言）**

你说的"用户"和代码里的 User 是同一个东西吗？你说的"订单"和数据库里的 Order 是同一个概念吗？没有统一术语，AI 就会自己猜，猜错了你就得返工。

**3\. Feedback Loops（反馈回路缺失）**

AI 一口气给你生成几百行代码，你怎么验证？哪行有问题你知道吗？没有快速反馈，错误会越积越多。

**4\. Software Entropy（软件熵加速）**

代码库变成一团烂泥的速度比以前快多了。AI 没有全局观，它只会根据当前上下文尽力而为。如果你不控制，它就会越写越散。

![图像](https://pbs.twimg.com/media/HIVxN_kWkAAVURb?format=jpg&name=large)

## 核心技能有哪些

Matt 把技能分成三类：工程类、生产力类、杂项。

**1.工程类技能**：

> **grill-with-docs** — 编程场景下用得最多。

它会一直问问题，问到你没问题为止。但它做了一个关键改动：会生成一个 context.md 文档，叫"共享术语表"。

你跟 AI 沟通需求时，对话里的术语会被记下来，方便后面编程时使用。

这个文档就是你的 **Ubiquitous Language**（统一语言）。

术语的作用是用简短的词去解决复杂的概念。在业务领域里，几个字就能代表一个完整的领域。

除了 context.md，它还会生成架构决策记录（ADR），作为后续流程的上下文。

这个直接解决 **Misalignment** 问题。

> **tdd** — 基于测试的开发。

流程是：规划、写测试、测试失败、最小代码通过。

在 AI Coding 中，TDD 的核心作用是控制每一步的粒度。强制你把工作切成小片，每一步都可验证。

这个直接解决 **Feedback Loops** 问题。

> **diagnose** — 诊断 bug。

用了六个阶段，是一套完整的debug方法论。

> **improve-codebase-architecture** — 架构改进。

它会搜索你整个代码库，用一些好的架构和接口设计方法（比如 **Deep Modules**：功能丰富但接口简单），看你是否匹配。

找到可以优化的点，让你选择是不是要往下优化，然后给优化意见。

这个直接解决 **Software Entropy** 问题。

用 AI 编程一段时间，或者完成一个模块后，执行这个技能，让它帮你找优化点。

> **to-prd 和 to-issues** — 跟 GitHub 关系紧密。

to-prd 会总结你的历史对话，生成 PRD 文档。

to-issues 把 PRD 转成 issues，给 issues 打标签（bug 还是功能）。

然后回到项目代码库里，去修改、迭代、更新这些 issues。

这是一个基于 GitHub issues 的小工作流。

**2.生产力类技能**：

> **grill-me** — 非编程场景下的拷问模式。

> **caveman** — 让 AI 用最简单的方式解释复杂概念。

> **handoff** — 交接工作时生成上下文文档。

> **write-a-skill** — 帮你写新的技能。

![图像](https://pbs.twimg.com/media/HIVxRy0XQAAVAUX?format=jpg&name=large)

## 怎么安装

两种方式：

**方式一：通过 Claude Code 安装（推荐）**

```bash
# 在 Claude Code 中输入
/install mattpocock/skills
```

**方式二：手动安装**

```bash
# 克隆仓库到 .claude 目录
cd ~/.claude
git clone https://github.com/mattpocock/skills.git
```

安装完之后，执行初始化：

```bash
# 在项目根目录执行
/setup
```

它会做这几件事：

1. 扫描你的项目，检查是否已配置
2. 修改你的 CLAUDE.md 文件，加入全局上下文
3. 创建 context.md（术语表）
4. 创建 docs/adr/ 目录（架构决策记录）
5. 让你选择 issue 追踪器（推荐 GitHub）
6. 让你选择领域文档模式（单项目用根目录，多项目用子目录）

初始化完成后，你会看到这些文件：

```text
your-project/
├── CLAUDE.md          # 已更新
├── context.md         # 新建
├── docs/
│   ├── adr/          # 新建
│   ├── agents.md     # 新建
│   └── issues.md     # 新建
```

## 推荐使用顺序

![图像](https://pbs.twimg.com/media/HIVxZRrXcAAVhSV?format=jpg&name=large)

Matt 推荐的完整工作流：

**第一步：需求对齐（grill-with-docs）**

有新需求时，先用 grill-with-docs 进行头脑风暴。

```bash
/grill-with-docs
```

它会问你 15-20 个问题，问到你没问题为止。

问完之后，生成 context.md（术语表）和 ADR（架构决策）。

**第二步：生成 PRD（to-prd）**

需求对齐后，生成 PRD 文档。

```bash
/to-prd
```

它会总结你的历史对话，生成结构化的 PRD 文档，发布到 GitHub。

PRD 里有：

- 用户故事
- 模块划分
- 测试计划
- 不在范围内的需求

**第三步：拆分任务（to-issues）**

把 PRD 拆成可执行的小任务。

```bash
/to-issues
```

它会把大的 PRD 切成 3-5 个小任务，每个任务都是一个 GitHub issue。

然后给每个 issue 打标签：

- bug — 修复问题
- feature — 新功能
- ready-for-agent — 可以交给 AI 处理

**第四步：实现功能（tdd）**

选一个 issue，用 TDD 方式实现。

```bash
/tdd #123
```

它会：

1. 拉取 issue 描述
2. 规划实现方案
3. 写测试（先失败）
4. 写最小代码让测试通过
5. 重构优化

**第五步：诊断 bug（diagnose）**

遇到 bug 时，用 diagnose 诊断。

```bash
/diagnose
```

它会用六个阶段系统性地找问题：

1. 复现问题
2. 收集信息
3. 提出假设
4. 验证假设
5. 定位根因
6. 修复验证

**第六步：架构优化（improve-codebase-architecture）**

每隔几天，或者完成一个模块后，跑一次架构优化。

```bash
/improve-codebase-architecture
```

它会：

1. 扫描整个代码库
2. 给出 3-5 个优化方向
3. 让你选择优先级
4. 针对选中的方向给出具体方案

## 实际使用案例

我用 grill-with-docs 测试了一个需求：增加一个充值功能，充值完成后送积分。

它问了 20 个问题：

- 充值金额范围是多少？
- 支持哪些支付方式？
- 积分赠送比例是多少？
- 充值失败怎么处理？
- 退款时积分怎么扣减？
- 页面布局是什么样的？
- 需要发送通知吗？

问完之后，生成了 context.md：

```markdown
## 术语表

**积分（Points）**
用户在平台上的虚拟货币，可用于兑换商品或服务。

**充值（Recharge）**
用户通过支付真实货币获得积分的过程。

## 关键规则

1. 充值金额：10-10000 元
2. 积分比例：1 元 = 10 积分
3. 赠送规则：充值满 100 元额外赠送 10%
4. 退款规则：退款时扣除已使用积分，剩余积分按比例退回
```

然后用 to-prd 生成 PRD，用 to-issues 拆成 4 个任务：

1. 充值页面 UI
2. 支付接口对接
3. 积分计算逻辑
4. 退款处理逻辑

每个任务都标记了 ready-for-agent，可以直接让 AI 去实现。

用 tdd 实现第一个任务，它会先写测试，再写代码，保证代码质量。

完成后，用 improve-codebase-architecture 检查，它发现了 2 个优化点：

1. 支付接口可以抽象成统一的 PaymentGateway
2. 积分计算逻辑可以提取成独立的 PointsCalculator

按照它的建议优化后，代码可维护性明显提升。

## 不止是写代码

Matt Pocock 的这套技能，核心是两个字：控制。

你不是被工具推着走，而是你决定什么时候用哪个技能。每个技能都很小，可以单独用，

也可以组合用。出问题了，你知道是哪个环节的问题，改起来很快。

AI 让写代码变快了，但代码从来没变得更便宜。

代码会腐烂、会积累技术债、会需要维护。AI

只是让你更快地产出代码，但不会替你思考代码该怎么组织。

这套技能组教你的，不只是怎么用 AI 写代码，而是怎么在 AI 时代保持对工作的控制权。 写文章、做设计、做分析，任何需要 AI

协作的工作，都需要你先想清楚、建立共识、控制节奏、定期优化。

装上试试，你会发现不只是写代码的节奏变了。

关于我：Gyro，从游戏里长出来的工程脑。分享 AI 工具实践、系统思维、认知升级。

👉 关注我的 X 账号 [@gyro](https://x.com/rwayne)