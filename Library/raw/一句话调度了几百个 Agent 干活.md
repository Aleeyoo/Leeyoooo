---
status: processed
wiki: "[[WIKI/Claude Code动态工作流]]"
---
# 一句话调度了几百个 Agent 干活，Claude Code 这次更新的动态工作流有点猛

前几天 Claude Opus 4.8 闪亮登场，铺天盖地的消息。但是个人认为更重磅的更新在于 Claude Code 中的 dynamic workflows。有些本来可能需要数月才能完成的工作，现在可能只需要几天就能搞定了。看到前面这句，我一下子想起了 SubAgent 和 Agent Teams。

我的系列文章，三分钟大白话讲解我自己还不明白的 AI 流行词。

-  1、三分钟大白话：什么是 AI Agent？https://x.com/aehyok/status/2028653502281748617
- 2、三分钟大白话：什么是 Skill？ https://x.com/aehyok/status/2021479968845476114
- 3、三分钟大白话：什么是 SubAgent？ https://x.com/aehyok/status/2032255002769834173
- 4、三分钟大白话：什么是 Agent Teams？ https://x.com/aehyok/status/2037464355437220171
如果你对 SubAgent 子代理或者 Agent Teams 还不太熟悉的话，可以看看我上面的文章。

这次主要就是大白话讲解 Dynamic Workflow（动态工作流），顺便把 SubAgent、Agent Team、Workflow 三者的关系和区别一次性讲清楚。

本文内容目录如下，可进行选看：

- 1、先说背景：从一个人干活到一堆人干活
- 2、再来看看 Claude Code 中如何接入
- 3、在 Claude Code 中实战（重点）
- 4、SubAgent、Agent Teams和 Dynamic Workflow比对
- 5、 怎么选？一个快速决策流程
- 6、总结
#  一、先说背景：从一个人干活到一堆人干活

回想一下 Claude Code 的进化路线：

- **阶段一：单人模式**
你问一句，Claude 答一句。一个会话就是一个 AI，所有事情它自己干。能干，但遇到大活就慢。

- **阶段二：SubAgent（子代理）**
主 Claude 可以派"帮手"出去干活——可以派一个，也可以同时派好几个。但每个帮手都是独立干活，互相不说话，干完各自回来汇报。相当于你可以叫好几个帮厨同时打下手，但他们各干各的，彼此不交流。

- 阶段三：Agent Teams（智能体团队）
不仅是帮手了，而是一支后厨团队——总厨带队，不同厨师分工协作，并且大家能随时沟通。你一句我一句，边干边调整。这和 SubAgent "各干各的、互不聊天"有本质区别。

- 阶段四：Dynamic Workflow（动态工作流）
这不再是"组团队讨论"，而是一条自动化流水线。你把流程设计好，它能同时调度几十上百个工人，各自在自己的工位上干活，最后汇总成果。

下面逐个讲清楚。

##  1、 SubAgent：独立执行的帮手（可以是一个，也可以是多个）

>  SubAgent 是主 Claude 派出去的**独立帮手**——可以派一个，也可以同时派好几个。每个帮手的模式都一样：接任务 → 自己闷头干 → 干完回来汇报结果。**干活过程中不说话，帮手之间也不交流。**
用餐厅来理解：你是餐厅老板，可以让一个帮厨去查食材供应商的电话，同时让另一个帮厨去清点冰库库存，再让第三个去整理今天的订单。三个人同时干活，但**各干各的，互不打扰，谁也不跟谁商量**。干完了一个一个回来跟你汇报。

![](https://pbs.twimg.com/media/HJxFDhia4AAHkdA.jpg)

**什么时候用：**

- 任务可以拆成独立子任务，互不依赖
- 任务明确，不需要反复确认
-  不需要中间讨论，只关心最终结果
**什么时候不用：**

-  任务需要中间沟通、实时调整
-  子任务之间有依赖，需要互相配合协调
## 2、 Agent Teams：后厨团队

关于 Agent Teams 我在[上一篇文章](https://x.com/aehyok/status/2037464355437220171)里用米其林后厨详细打过比方，这里快速回顾核心要点。

>  Agent Teams 是一支**分工协作的小团队**。队长（主 Claude）+ 几个队员，大家各司其职，**随时可以互相沟通**。
总厨坐镇后厨，下面有：

* 备菜厨师

* 主菜厨师

* 前菜厨师

* 甜点厨师

* 摆盘厨师

他们不是各干各的然后交差——而是边干边沟通：

* "主菜快好了，前菜那边准备好了没？"

* "这个摆盘颜色不对，重来一下。"

* "甜点需要冰一下，主菜稍微慢一点。"

**总厨不仅是检查，重磅菜品也亲自上手。**

![](https://pbs.twimg.com/media/HJxFs2jaYAA8b2S.jpg)

**核心特征：**

-  双向通信：队员与队长之间、队员与队员之间都能实时沟通
-  小规模：通常几个人，不宜太多
-  独立会话：每个队员在自己的终端窗格中运行
- Token 消耗大：每个队员都是完整 Claude 会话，还有通信开销
**什么时候用：**

-  任务比较复杂，而且需要多角度思考
-  需要实时讨论、并调整方案的
- 像设计新菜单、策划活动方案这类需要"商量着来"的事
**什么时候不用：**

-  简单小任务（杀鸡用牛刀，沟通成本比干活成本还高）
-  任务可以完全拆成独立子任务、不需要中间沟通（这种情况 Workflow 更合适）
## 3、 Dynamic Workflow：流水线调度系统

>  Dynamic Workflow 不是团队，而是一条**自动化流水线**。你用脚本定义好流程，它批量调度几十上百个 SubAgent 同时干活，各自完成工位任务，最后汇总成果。
前面的 SubAgent 是"可以叫几个帮厨同时干活，但各干各的"，Agent Teams 是"后厨团队边干边沟通"。

Dynamic Workflow 则更像是**中央厨房的流水线**：

![](https://pbs.twimg.com/media/HJxGawpaUAAfVhO.jpg)

当工作流开始运行之后，Claude 会根据你的指令动态地制定计划，将任务拆分成多个子任务，并分配给多个并行运行的子Agent 来处理。在各个子Agent的结果被整合之前，都会进行逐一检查。最终，你会得到一个统一、协调的结果。各个子Agent 从不同的角度来处理问题，其他子Agent则试图反驳这些结果。整个过程会不断迭代，直到所有结果趋于一致——这就是工作流能够实现单次处理无法达到的结果的方式。

注意关键区别：流水线上的工人互相不聊天。他们只做自己工位的事，做完汇报，然后下一个阶段开始。和 Agent Teams 的"随时沟通"完全不同。

核心技术：动态工作流是一种用于大规模协调各个子Agent 的 JavaScript 脚本。Claude 会为你描述的任务编写这样的脚本，然后由运行时在后台执行该脚本，同时确保你的会话能够保持正常响应状态。

![](https://pbs.twimg.com/media/HJxGk3kbMAAPUaB.jpg)

**什么时候用：**

- 任务可以明确拆分成多个独立子任务
- 需要大规模并行处理（几十个文件、几百条数据）
-  流程固定，不需要中间讨论调整
-  比如：代码库审计、批量测试生成、大规模搜索整理
**什么时候不用：**

- 任务需要边做边讨论调整（用 Agent Teams）
- 只有一个简单任务（直接 SubAgent 或单会话就行）
- 任务很难提前拆分成独立子任务
## 二、再来看看 Claude Code 中如何接入

![](https://pbs.twimg.com/media/HJxHaNhbUAAstE2.jpg)

这里有劳伦斯老师的独家手把手教程

##  三、在 Claude Code 中实战

这里实战自己已经没有 Claude 官方账号了，还是直接使用的国产大模型 DeepSeek。

先说一下有两种方式来触发创建动态工作流：

-  1、直接使用 /effort中选择 urltracode如上图所示
- 2、直接在提示词中添加 “workflow”的标记或“动态工作流”
直接让他给我处理146个文档，翻译成5种语言的动态工作流

![](https://pbs.twimg.com/media/HJxJeWNawAAx31w.jpg)

Workflow 1000上限内，也就是目前来看最大并行执行个数是1000个，受限于我的电脑配置。

原来并发subagent的个数跟系统的配置有关系

![](https://pbs.twimg.com/media/HJxJjmNboAAy4xI.jpg)

然后如果你执行了动态工作流可以通过命令/workflows进行查看

![](https://pbs.twimg.com/media/HJxJpsxbEAA47vt.jpg)

展示了每个阶段的代理数量、令牌总数以及所经过的时间。可以深入查看各个阶段的详细信息，了解每个代理的运作情况。

当前这个工作流如果你经常使用，那么你可以直接按上面的s键，然后这个动态工作流就保存下来了。

![](https://pbs.twimg.com/media/HJxJzwfbIAA9H1i.jpg)

下次就可以直接进行复用。

动态工作流所消耗的资源远远高于普通的 Claude Code 使用情况。每当有工作流被触发时，Claude Code 会先显示即将执行的内容，并请求用户确认。组织管理员还可以通过管理设置来禁用这些工作流。

如果你觉得 Workflow 太费 token 或者暂时用不上：

- 在 Claude Code 里：`/config` → 把 **Dynamic workflows** 关掉
-  直接改配置：在 `~/.claude/settings.json` 里加 `"disableWorkflows": true`
-  环境变量：`CLAUDE_CODE_DISABLE_WORKFLOWS=1`
-  组织级关闭：在 managed settings 里设置 `"disableWorkflows": true`
关闭后，`/deep-research` 等内置工作流命令不可用，`workflow` 关键词不再触发，ultracode 从 `/effort` 菜单消失。

## 四、SubAgent、Agent Teams 和 Dynamic Workflow比对

三者对比：一张表看清楚

![](https://pbs.twimg.com/media/HJxKEu7bkAA7ht3.jpg)

它们是什么关系？不是替代，是梯度互补。

![](https://pbs.twimg.com/media/HJxKLVebMAAxgmj.jpg)

**结论：**

- Dynamic Workflow 的每一份实际工作，底层都是**标准 SubAgent** 在干活。Workflow 只是加了一层编排层。
- Agent Teams 用的是 **Teammate**，它是 SubAgent 的增强版——加了双向通信能力，但代价是更高的 token 消耗和协调成本。
-  三者构成阶梯：**任务越简单独立，越往 SubAgent 走；越需要讨论协作，越往 Agent Teams 走；越需要大规模批量处理，越往 Workflow 走。**
##  五、 怎么选？一个快速决策流程

用餐厅场景复习一下：

```bash
你的任务来了
  │
  ├── 简单、独立、能拆成互不依赖的子任务？
  │     └── 用 SubAgent（杀鸡不用牛刀）
  │
  ├── 复杂、需要多角度、需要随时讨论调整？
  │     └── 用 Agent Teams（组个后厨小队）
  │          （注意：目前需手动开启实验功能）
  │
  ├── 规模大、可拆成独立子任务、流程固定？
  │     └── 用 Dynamic Workflow（上流水线）
  │
  └── 三个都不适合？
        └── 直接在会话里让 Claude 干（单 Agent 模式也很快）
```

![](https://pbs.twimg.com/media/HJxKl4MaUAAi4xu.jpg)

##  六、总结

Claude Code 的多 Agent 体系正在快速的进化。从最早的 SubAgent 可以派几个帮手各干各的，到 Agent Teams 一支小队边干边沟通，再到 Dynamic Workflow 一条流水线批量调度——每一步都在把"一个人能干的事"扩展成"一群人能干的事"。

但本质没有变：工具永远是为任务服务的。不是为了用 Workflow 而用 Workflow，而是看你的任务最适合哪种协作模式。

简单任务就老老实实单 Agent 或派几个 SubAgent 各自干活，别把简单问题复杂化。复杂需要讨论的就上 Agent Teams，别让一个人憋着硬想。大规模可拆分的就上 Workflow，别让一群人排队等前一个人干完。

有些本来可能需要数月才能完成的工作，现在可能只需要几天就能搞定了。这就是 Dynamic Workflow 带来的变化。

选择适合自己的，才是最重要的。

最后再次建议大家都来使用一下Claude Code。

https://x.com/aehyok/article/2061698672128348380

— [AI少年 (@aehyok)](https://x.com/aehyok/status/2061698672128348380) · 2026-06-02 14:37
