---
status: processed
wiki: "[[WIKI/Agent架构三省六部反思]]"
---
# X 上的 Yeuoly：”企业不需要三省六部制的 Agent 流程” / X

https://x.com/Yeuoly1/status/2054922914458464754?s=20

> 本文仅作为个人 Blog，关于产品的正式文章与 showcase 将在后面几天逐步放出

Hello 各位，我是 Yeuoly，可能更熟悉我的另一个名字，Dify 的后端工程师周宇。

今天不是很想聊 Dify，更想跟大家聊聊过去六个月我们一直以来都在思考的一些问题，我们一个小团队烧掉了数千亿 token，本应该更轻松，却在 Agent 元年变得越来越累的事情

![图像](https://pbs.twimg.com/media/HIAVcelakAAGqmh?format=jpg&name=large)

我们有这样一个观点：**真正的增效，并不是用 Coding Agent 把一个人的判断力压榨到极限，反而应该是把富有创造力的人，浪费在事务性工作中的大量时间，释放出来，减轻他们的压力。**

而我们的目标只有一件事：**不再让人成为传话筒。**

也许大家在一个月前看到过一篇长文：《三省六部幻觉》

> 4月14日

什么是三省六部式的 Agent？

给不同的 Agent 以不同的角色设定：一只龙虾负责产品设计，另一只龙虾负责架构设计，再来一只负责测试，还有的负责运营，有的负责财务，现在大家管这个叫龙虾军团，于是我也去尝试了。

## iOS 上架

我让一个 Agent 专门去负责 iOS 上架，被苹果审核员打回审核的时候，他告诉我他已经完成了，**关键是我根本不知道他完没完成，我还得检查，于是我的注意力又被劫持一次，这个 Agent 还需要抢工程 Agent 的注意力，导致它为了专门给苹果过审核，让工程 Agent 在代码里留了一个后门**，我都不知道还有这么回事。

## UI/UX 设计

我让另一个 Agent 专门去负责设计，我不想自己一个人去管 iOS Native / Web / Client 等好几个端的 UI / UX 设计，**但 Agent 写的 UI 让我厌蠢**，我也试过无数的 design skill，但也许是胃口被我们的 UI 设计师养刁了，我觉得这些 design skill 基本没什么用，GPT 就是喜欢写蓝紫色渐变，Claude 就是喜欢写左边有一撮小颜色的卡片。

## 工程设计

Codex 写了大量没有用的单元测试集成测试，留了大量的技术债务，导致 CI/CD 变慢，浪费时间，我想删掉，但到底删哪些？全部交给 Agent 自己判断？最后出了问题怎么处理？光是复现问题就需要半个小时以上，**更是很难确认它的修复到底是给你打了个 mock 还是真的修复了。**

我觉得我失败了，或者说，我的注意力爆炸了，原因是我要一个人要在短时间内跑完原本几十个人上百个人所构建的流程，**且对所有事负责，要对品质负责。**

于是我想看看有多少人和我一样的，我专门做了个 [骂了么](https://leaderboard.sbai.uk/)，发现大家骂 AI 都骂的不少。

![图像](https://pbs.twimg.com/media/HIAV4c2aIAAyqHI?format=jpg&name=large)

不仅有这些，还有财务合规、法务合规、运营策略、Slide 设计、易拉宝设计、官网设计、写 blog、写发布文章、想 GTM 策略，这里面有大量需要人投入精力才能做好的事情，且各个环节是环环相扣的，例如易拉宝的材料需要用官网的图片，文章里的表述要和运营策略吻合，Slide 的 UI 风格要和产品一致……

如果不投入心思，那 Agent 只会给你一个大约六十分，有时候甚至三十分不到的成果，最后你会发现，好像去咸鱼找个人会更快更好，自己还没那么累。

![图像](https://pbs.twimg.com/media/HIAWAd4a0AAfN6G?format=jpg&name=large)

我考试能考 60 分，我觉得我至少不是一个不合格的人吧，但我处理不来这么多繁琐的事情。

OpenAI 自己也说，[2028 年目标才是实现 AI 自己研究自己](https://www.techradar.com/ai-platforms-assistants/chatgpt/openai-roadmap-revealed-ai-research-interns-by-2026-full-blown-agi-researchers-by-2028?utm_source=chatgpt.com)，过去曾经说 2026。

Anthropic 之前大火的项目，Claude 自己完成[生产可用的 GCC](https://github.com/anthropics/claudes-c-compiler) ，可现在已经三个月没有动静了。

所以我去看了看真实的人，在企业里究竟做着什么，AI 真的取代一切了吗？

## 企业内的 SOP

现在的企业都在做什么？

我们看到了有的企业给全员配了没有**使用上限的 Claude Code**，让随便用，探索 AI 场景，员工拿到了 Claude Code 每个人都开始 Vibe 自己的系统，人人都是产品经理，人人都是工程师，可是企业内真的有这么多需要 Vibe Coding 的场景吗？

于是我们看到了类似于这样的事情发生：

![图像](https://pbs.twimg.com/media/HILRyXaagAA6Eac?format=jpg&name=large)

[https://t.me/xhqcankao/29277](https://t.me/xhqcankao/29277)

我们以前工作大多数都是 SOP，而大部分企业内的 SOP，并不是一个人就能完成全部的，他们是围绕人的，这个过程中人的**判断力**非常重要，里面包含了大量人的品味，例如：宣传册字是不是太大了，**蓝色**是不是不够蓝，这么写的文章粉丝会不会喜欢。

我们连精品店选什么品、文章用什么标题、视频用什么封面都要发起一个投票，让人选一选，现在却说 Agent 可以接管一切。

于是每个人的 Claude Code 之间出现了壁垒，文件要通过 IM 复制黏贴，上下文有时候甚至是截图，还有的时候，我直接成了 Claude Code 的附庸，帮 Claude 做这做那，一整个大号的 Human in the loop。

我越来越觉得，很多用户需要的东西，确实不完全是一个 Agent，更像是一个 Agentic Flow，但不是一个门槛极高，像代码的 Workflow，他们只关心：这玩意能帮我审发票吗？能帮我审合同吗？能帮我起草催款书吗？我们的 Agent 能一起操作同一个文件吗？

我相信这些场景看起来并不怎么酷，也不怎么性感，对于做技术的朋友来说，这些也太简单不过了。

可是我们公司的财务问了我这样的问题：她说她找了几乎市面上能找到的所有财务报销软件和 AI Workflow，没有一个能符合他的要求，她就是想要一个报销流程。

那我问她，你的要求到底是什么？就两点：

1. AI 预查一下报销合不合理，不合理的时候拒绝掉或者让对方补充材料，补充完以后继续提交
2. 流程能自动识别上级、财务、法务等职责，先让上级审一审合不合理，再到财务进行审批

![图像](https://pbs.twimg.com/media/HIAWYLGbgAAvCLM?format=jpg&name=large)

飞书没有 AI 审查，即便是 AI 审查，它也没法按照公司自己的规章制度进行审批，例如在一线城市的报销标准是多少，能不能坐商务舱，大于多少钱的时候需要额外的审批流程。

AI Workflow 的产品里，根本不涉及多个人之间的协作。

然后我接着问她，你直接把 Claude Code 接入到飞书流程里呢？飞书钉钉都开放了飞书 CLI，她告诉我这太繁琐了，她根本不愿意看到那个 TUI 黑框框，也不愿意去接那个飞书 Webhook API，她只能找外包帮忙做，做出来了也需要外包团队帮忙维护，Claude Code 写出来的代码她负不了责，出了问题她没法向老板交差。

我并不认为她是一个什么很土的财务，她也是一个愿意积极拥抱 AI 的人，她的需求就因为看起来土，就应该被我们忽视吗？

![图像](https://pbs.twimg.com/media/HIAWhhQbEAEBmgi?format=jpg&name=large)

这不是个例，运营、法务等 back office 部门、工程和产品部门都有类似的 “多人 SOP” 的情况。

我们可以看看下面这张图，红点，是使用 AI Coding 的人群，而没有使用过 AI 的人群，占到了整张图的 84%。

解决这些涉及人与人的 SOP 自动化，降低人与人的摩擦，人与环境的摩擦，就是我们未来的目标，也是我们的愿景，也是我们的新产品：Syncless

![图像](https://pbs.twimg.com/media/HIAWog2aAAAavM_?format=jpg&name=large)

我不知道各位有没有被同事折磨过：

1. 同事用 AI 写长的要命的文档，让你读一下，实际上有用的也许只有其中的两句话，然后你把它复制粘贴到你的 Claude Code 或者 Codex 里，说出那一句 “帮我总结一下”
2. 售后同事给你转了一堆消息记录过来，看了以后你也摸不着头脑，例如，说产品性能不好，也不说哪里性能不好，什么标准的性能不好。
3. 产品同事丢过来一篇 PRD，要求你做可行性方案出来，你丢给 Codex，聊半天发现做不了，于是又转头告诉 PM，PM 一看，诶，不对啊，我的 Claude Code 说能做，好了，现在该你们吵一架了。
4. 运营同事给你发了一份文档，让你填一下，读一下，改一下，可能也许就是一个活动现场的安排，需要你填个东西，但你要先点进去，再找到填什么，再思考半天填什么。

除了这些，应该还有更多协同上的阻塞点：确认 OKR / 提交一下发票 / 报关表单填错了打回来重改……

极其宝贵的人的注意力，被浪费在了无数杂事的沟通与摩擦当中，而且他们高度重复，你一遍又一遍地处理着这些事情。

## Syncless

所以 25 年末，我们决定做 Syncless，那个时候它有一个内部代号：Echo，看起来和 Dify、n8n、Coze 都很像，但它更关注人，关注人与人之间的 SOP 怎么自动化。

![图像](https://pbs.twimg.com/media/HIAXJkDb0AA_ouX?format=jpg&name=large)

首先是 Syncless 的第一个设计：**Project 模板**

如上图所示，这是一个三个人协作的流程，从售后到产品到工程，而人与人的协作流程其实一般都是有隐性的模板的，例如，售后在反馈问题的时候应该提前说明客户的问题是在什么情况下发生的，问题看起来有什么特点，客户的画像是什么样的……

对于传统的在线表单，CRM 之类的方案，Syncless 有很大的优势：

1. 所有这些信息，都可以通过自然语言的形式定义，不需要定义复杂级联表单、条件表单，不需要鼠标反复地点击新建问题。
2. Agent 可以动态地根据用户反馈调整询问的内容，例如售后反馈的是 Bug 的时候，那么 Agent 应该强调怎么复现 Bug，如果反馈的是功能优化，那么 Agent 应该强调用户的原始需求和预期是什么。

回到上文提到的财务流程，它也可以预先定义好，应该怎么查，例如在小于多少金额的时候，可以直接通过，在一线城市出差的标准是多少，二线城市是多少，能不能坐商务舱……

还例如在物流行业，有一个路线规划的流程的场景，他们有一个专门的路线规划岗位，会处理例如非标大件（风车叶片等）怎么运输，一般是产品经理会把需要运什么的需求给到路线规划的同事，然后他们根据需求出具线路图，这个过程中也涉及大量的信息同步，例如你要运的是个什么，有多重，形状什么样的，有没有喜欢的路径，有没有特殊情况。

过往我们都是通过填表，可是在发生意外情况的时候，例如产品需要修改重量，或者路线规划的同事反馈做不了，两个人要协调一下的时候，就得拉群了。

## 产品设计 SOP

我不知道大家有没有关注过 Codex 的产品团队是怎么设计产品的：[https://openai.com/index/harness-engineering/](https://openai.com/index/harness-engineering/)

他们提到，PM 的工作每天就是让 Codex 先处理繁杂的用户反馈，然后自己和 Codex 聊方案，聊设计，差不多了就把设计给到工程师，但同时，这一步 PM 也会先自己处理一些技术相关的问题，这些问题很多都是由工程师预设的，这更像是 PM 和工程师共同开发

重要的是，PM 和 Codex 的聊天记录，上下文都给到了工程师，这极大减小了磨损。

所以我们也借鉴了他们的思路，在 Syncless 的产品设计模板中，我们有三个节点：销售和产品、工程，这个画布定义了他们品应该如何协作，这里并不是 Agent 接管一切，每个 Node 的背后，都是人。

例如我们在 Sales 中定义了提出一个需求的时候，应该注意一些什么，售后如果就说了句 “ Dify 性能太差了”，Agent 就会询问 “什么场景？什么指标？”，补充完成以后，流程才会进入下一个节点。

![图像](https://pbs.twimg.com/media/HIAXeD4bgAAZ8AK?format=jpg&name=large)

接下来我们切换到产品视角，现在你作为产品经理，你接到了来自前线售后团队的问题，Agent 已经把自己和 Agent 已经聊完的上下文带了过来。

![图像](https://pbs.twimg.com/media/HIAXl0DaMAAZaqU?format=jpg&name=large)

大多数时候，Syncless 已经可以帮你挡住一层，Agent 已经提前和人沟通过了，但总有的时候会出现意外情况，例如就是缺了什么信息，Syncless 并没有一棒子打死式的硬限制，而是在能尽可能减少繁琐沟通的情况下，留出冗余。

所以在这个 Demo 中，我作为产品，我非常想知道到底是个什么场景，没有具体的场景我很难和工程继续沟通，所以我选择询问销售，此时 Agent 会去询问销售，到底是什么场景，让销售进一步补充信息。

![图像](https://pbs.twimg.com/media/HIAXrQHa0AA7EiF?format=jpg&name=large)

回到销售的聊天框，就能看到 PM 发来了一条消息，说我需要更多的信息，我们此时只需要正常回复就好了，补充完信息，Agent 就会接着进入到下一步。

## 财务报销 SOP

上文我们提到了财务报销场景，在 Syncless 中是如何完成呢？

![图像](https://pbs.twimg.com/media/HIAXz17aIAAeeG_?format=jpg&name=large)

我们定义了一个三节点，并告知了 Agent 不同城市的出差标准怎么样，餐标怎么样，并预定义了检查报销是否合理等操作。

![图像](https://pbs.twimg.com/media/HIAX7heakAA6DyX?format=jpg&name=large)

于是当有人提出报销的时候，Agent 就可以预查报销是否合理，在上图中，它就查出来了 526￥ 并不满足四个人的报销标准，并且它也不是简单的拒绝操作，而是它可以只报销 480￥，并通知报销人，只给你报了一部分，这一切都是自动的，但最后的决策决断，都是由人来完成。

这大大省去了原来财务需要用肉眼去看发票，并仔细比对抬头对不对、金额对不对、企业名称对不对、时间对不对等一系列问题，有时候报销人就是拍了张 711，全家的发票，人肉眼去看是非常痛苦的。

不过有的时候，报销流程会更加复杂， 例如，当金额超过 50000 的时候，需要引入 CXO 审批，不满足就不需要，或者有时候财务查出金额对不上，打回去让修改，修改以后发现，报销金额变少了，不需要 CXO 审批了，流程就又需要变动。

这是一个完全动态的流程，Syncless 目前可以做到，但是目前完成这种极其复杂的流程有一定的难度，例如可以在协作模板里说明，当金额大于 300 时，需要另一位同事参与审批，此时 Syncless 会把这位同事加入流程。

![图像](https://pbs.twimg.com/media/HIA5d--aEAA-JvA?format=jpg&name=large)

我们会持续为让它变得更简单努力，让搭建这种流程变得像喝水一样简单。

## Syncless 有什么不同？

不同于群聊式的信息大杂烩的群聊， Syncless 更聚焦于 “上下游协同”，SOP 并不是一个 Agent 照着执行就可以包罗万象的东西，它往往串联了企业内无数团队的大脑，信息和各种制成品在一个流水线中按照企业自己的 SOP 流转。

在过去，他们是无数的 CRM、OA、IM，我们见到了一个又一个 Workflow 系统，人在这个 Workflow 中充当螺丝钉，重复且机械地执行一个又一个 SOP。

但多人的 SOP 是可以被完全取代的吗，我觉得这是一个关于 ROI 的故事，Anthropic 的 token 卖到天价，企业最后一看好像 token 比人还贵，陷入焦虑，不做 AI 死路一条，做了 AI ROI 死活跑不正，AI 还在稀释微薄的利润。

而我们认为，**真正的增效，并不是用 Coding Agent 把人的判断力压榨到极限，反而应该是把富有创造力的人释放出来，减轻他们的压力，把他们浪费在同步中的大量的时间释放出来**，让他们真正有时间去思考怎么创造。

[Asana 认为 60% 的知识工作者的时间都花在了协调与追踪](https://asana.com/resources/anatomy-of-work-index)，[微软的员工每天被打断 275 次](https://www.microsoft.com/en-us/worklab/work-trend-index/breaking-down-infinite-workday)，[Grammarly 认为美企每年因为沟通低效损失了 1.2T$](https://www.grammarly.com/business/learn/introducing-2024-state-of-business-communication)

![图像](https://pbs.twimg.com/media/HIAYHDaaEAEESS6?format=jpg&name=large)

这些数字多多少少会有不精确，会有夸大的地方，但这个现象本身反映的是我们为 Sync / Align 这个概念付出的代价，现在大家都在着急把一切都换成 AI，换成龙虾，组件自己的龙虾军团，但我相信各位已经有了充分的体感，tokens 烧得更快，过不了两个月企业就开始限制 AI 使用。

## 人与设备、Agent 与设备之间的壁垒

不仅是人与人的同步存在壁垒，机器和机器的同步，平台与平台的同步，也存在壁垒，我们在 Reddit 上随便一搜，就能发现有太多的人有这个困扰：因为自己的发票涉及几十个不同的平台，要一个一个自己去拿真的麻烦得要死，而又不可能每个平台都有 MCP。

老实说，我自己作为报销人，我不想自己去搜集乱七八糟的报销材料，我想要的是 Codex、Claude Code 能不能自己给我做了，因为我的 Email 里就有这些材料，我不想当翻译官、传话筒，把要 Agent 做个啥都写给 Agent，最后再把 Agent 的结果复制回来。

这在今天已经是常态了，但我不喜欢，因此，Syncless 接入了多设备，你的浏览器、Macbook、服务器都可以接入 Syncless。

![图像](https://pbs.twimg.com/media/HIGdoEsb0AAGkhf?format=jpg&name=large)

而使用他们，可以直接通过 @，你既可以 @ 你的同事，也可以 @ 你的设备，还可以 @ Syncless 自己的云环境或者直接 @ skills。

![图像](https://pbs.twimg.com/media/HIHgK33bQAAJRt2?format=jpg&name=large)

甚至能做这种事

![图像](https://pbs.twimg.com/media/HIKpsg6awAA6TPj?format=jpg&name=large)

或者也可以让它直接去用你的浏览器做点什么

![图像](https://pbs.twimg.com/media/HIGdoEvbsAArloF?format=jpg&name=large)

浏览器已经有了你已经在各个平台登录的身份，Agent 有了它就可以代你做更多更。

运维同事跟我说，自己有非常多不同的集群和机器需要控制，一个一个去不同的机器上敲命令实在太麻烦了，所以我们把所有这些外部接入都称作 Device，在未来，也许你的 Apple Watch 也可以接入 Syncless。

## 像 iCloud 一样的 Skills

同样，Syncless 也具备 Skills 能力，这是一个像 iCloud 的 skills，不管你在使用哪个设备，运行在哪里，它都永远为你在线。

![图像](https://pbs.twimg.com/media/HIAYbA6a8AAmYuT?format=jpg&name=large)

Syncless 的一位内测用户跟我们表达：“他很喜欢 Syncless 的 Skills 设计，在手机、电脑、浏览器、服务器上，Agent 都可以用到他的所有 skills，不用在不同设备上同步这些文件”。

也只需要一句 “帮我把上面的流程保存为 skills”，Syncless 就会自己去记住这些可重复使用的流程，在下次使用的时候就可以绕开很多弯路。

![图像](https://pbs.twimg.com/media/HIAYghUaUAArF97?format=jpg&name=large)

它不会自大到碰到个什么事情就建一个 Skill，碰到什么就记住，这会导致一个堆成山的 skills 列表，然后在下一次你做另外一个任务的时候告诉你，我找到了十几个相关的 skills，严重污染 Agent 宝贵的注意力和上下文。

## 写在最后

在 26 年年初，龙虾大量出现在了公众的视野中，我们看到了全民龙虾热，每个人都想往自己名字里放一只龙虾，我能看到大量的人说自己不用工作了，AI 可以帮自己完成一切。

我们是一个五人不到的小团队，所以正好在这个时间点，我们也去尝试了这些很酷的 OPC、NPC 理念，最后发现我们被压到吃不消，AI 给我们带来的债务在很多时候远大于产出，我们在一遍又一遍地铲 AI 的代码，在一遍又一遍地改宣发材料，这让我们很累，而从 Syncless 终于终于运行起来的开始，我们的缺陷管理，产品管理，真的轻松了很多。

Syncless 不是又一款要取代取代 Slack、Lark 等平台 的 AI 协作工具，也不是 Agent 协同模拟器，我们是希望帮人类处理掉事务性的工作，不要再因为一件又一件的小事浪费人类在这个时代宝贵的注意力。

最后，Syncless 现在并不算非常成熟，希望我们成长的路上有你的加入，欢迎试用 [https://syncless.ai](https://syncless.ai/) ，也欢迎联系我们的团队：hello@syncless.ai

或者加入我们的 Discord 社区：[https://discord.gg/G6AbY5zgsP](https://discord.gg/G6AbY5zgsP)

中文社区：

![图像](https://pbs.twimg.com/media/HIAYsJPbkAAcrSq?format=png&name=large)