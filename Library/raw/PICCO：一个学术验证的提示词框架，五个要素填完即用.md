---
status: processed
wiki: "[[WIKI/PICCO提示词框架]]"
---
# PICCO：一个学术验证的提示词框架，五个要素填完即用

![](https://pbs.twimg.com/media/HJUOKuCaIAAZNDt.jpg)

2026 年4月 [**arXiv 上有篇论文，编号 2604.14197**](https://arxiv.org/pdf/2604.14197)。研究团队把市面上 11 个已经验证有效的提示词框架分析了一遍，包括CRISPE、APE、RACE、CREATE，一个个比较。

他们就想搞清楚一个问题：**好用的 Prompt，共性是啥？**

![](https://pbs.twimg.com/media/HJUGsFqaMAAUL0_.png)

结论很简单，只需五个要素。

- **P** = Persona，角色
- **I** = Instructions，指令
- **C** = Context，上下文
- **C** = Constraints，约束
- **O** = Output，输出格式
五个字母凑起来刚好是 PICCO。听起来也很简单，但请问我们每次都能做到吗？

但我以为，**这TM哪是个提示词框架，这TM是思维模型。**

---

## 五条逐个来看

**P = Persona：给 AI 一个身份**

这一步大家应该也都懂，但可能经常就会随便写一句“你是一个助手”。

“助手”这个词太宽了。什么助手？写代码的还是写文案的？干了十年还是刚入行？

同一个任务，AI 扮演“有十年经验的 SaaS 定价产品经理”，比扮演"、“一个助手”，输出差距很大。训练数据里，高质量的行业分析和泛泛的日常问答，不在同一个池子里。给它一个具体身份，等于告诉它去哪个池子捞答案。

[https://abs.twimg.com/emoji/v2/svg/274c.svg](https://abs.twimg.com/emoji/v2/svg/274c.svg) 你是一个助手。 

[https://abs.twimg.com/emoji/v2/svg/2705.svg](https://abs.twimg.com/emoji/v2/svg/2705.svg) 你是一个在 SaaS 行业做了 10 年定价策略的产品经理，主导过 3 次 B2B 产品的定价体系重构。写东西的风格：数据先上，结论直接，不写废话。

也不用写很长。回答三个问题就够了：你是谁、擅长什么、说话什么风格。有时候我就写一句，也比不写强。

---

**I = Instructions：给 AI 一条轨道**

这一步我们通常也会，但大部分都写得比较模糊。

“帮我分析一下 Anthropic 的商业模式”：从什么角度？用什么逻辑？给谁看的？

指令的关键是选一个框架。框架就是轨道，有轨道它跑得飞快，没轨道它开始瞎绕。做什么不重要，按什么逻辑做才重要。

[https://abs.twimg.com/emoji/v2/svg/274c.svg](https://abs.twimg.com/emoji/v2/svg/274c.svg) 帮我分析一下 Anthropic 的商业模式。

 [https://abs.twimg.com/emoji/v2/svg/2705.svg](https://abs.twimg.com/emoji/v2/svg/2705.svg) 用 SCQA 框架分析 Anthropic 的商业模式。先讲 AI 行业当前格局（情境），再说 Anthropic 和 OpenAI 走了两条完全不同的路（冲突），提出核心问题：为什么 B2B 路线更赚钱（问题），最后给数据分析的结论（答案）。3000 字，给有 SaaS 背景的创业者看。

SWOT、SCQA、PAS，随便搞一个熟悉的框架进去就行。画好了轨道，它自然跑得快。

---

**C = Context：给 AI 画一个圈**

这一步是我个人觉得最管用的，也是最容易被忽略掉的。

你就来一句“帮我写个分析”，什么背景信息都没有。AI 只能从训练数据里猜你要什么。猜对的概率，跟买彩票一样。

上下文的作用就一个：**告诉 AI 哪些是事实，别自己编。**

[https://abs.twimg.com/emoji/v2/svg/274c.svg](https://abs.twimg.com/emoji/v2/svg/274c.svg) （直接丢一个链接，什么说明也没有）

[https://abs.twimg.com/emoji/v2/svg/2705.svg](https://abs.twimg.com/emoji/v2/svg/2705.svg) 以下是 Anthropic 2026 年 Q2 的三项关键数据：年化营收 109 亿美元，环比涨了 127%。运营利润约 6 亿美元。企业客户超过 1000 家，大客户年均消费约 100 万美元。基于这三项数据分析它的商业模式能不能持续。

给它画了一个圈。圈里是事实，圈外它不敢乱编。加了这个之后，AI 胡编数据的次数基本能降到零。

---

**C = Constraints：画红线，不是给建议**

这一步容易写成废话。“尽量简洁一点”、“最好不要太啰嗦”，写了跟没写一样。

对 AI 来说，“尽量”就等于没说。

约束的本质是硬规矩。你跟设计师说“自由发挥”，出来不知道是什么。你跟他说：“只能用黑白、左上角放 logo、标题不超过 8 个字”，反而出一张好稿。AI 也是这个道理，有边界才有方向。

[https://abs.twimg.com/emoji/v2/svg/274c.svg](https://abs.twimg.com/emoji/v2/svg/274c.svg) 尽量简洁一点。 [https://abs.twimg.com/emoji/v2/svg/2705.svg](https://abs.twimg.com/emoji/v2/svg/2705.svg) 以下内容禁止出现：'首先/其次/最后'结构、'在当今时代''毋庸置疑'等套话、每段超过 5 行、没有数据支撑的结论、'不是…而是…'句式。

一条具体的禁令肯定比十条模糊的“尽量”有用。

---

**O = Output：告诉它答案长什么样**

这步是比较简单的，越简单越容易出错。

写了半天，告诉 AI 干什么、基于什么、不能干什么，最后没给输出格式。它就只能按默认模板来。开头下定义，中间展开，最后总结。教科书味扑面而来。

[https://abs.twimg.com/emoji/v2/svg/274c.svg](https://abs.twimg.com/emoji/v2/svg/274c.svg) 不写。 [https://abs.twimg.com/emoji/v2/svg/2705.svg](https://abs.twimg.com/emoji/v2/svg/2705.svg) 输出格式：第一段结论先行，50 字以内，一句话说清核心判断。第二段三个论据，每个 150 字左右，各附一个具体数据。第三段给读者一个今晚就能做的行动建议，100 字以内。

格式越具体，成品越接近你想要的样子，后期改的时间至少省一半。

---

## 完整模板，直接复制

```
【Persona】
你是一个[角色]，有[X]年[领域]经验，擅长[具体技能]。
说话风格：[2-3 个关键词]

【Instructions】
用[框架名称]框架完成以下任务：[一句话描述任务]

【Context】
背景信息：
1. [关键数据 / 事实]
2. [关键数据 / 事实]
3. [目标读者 / 发布平台 / 字数限制]

【Constraints】
禁止：
- [句式 / 结构限制]
- [用词限制]
- [风格限制]

【Output】
按以下格式输出：
1. [第一部分说明]
2. [第二部分说明]
3. [第三部分说明]
```

存到备忘录或者 Obisidian。每次打开 ChatGPT 或 Claude，先花一分钟填空。五个地方填不满，说明你还没想清楚要 AI 干什么。所有一定要先想清楚，也是完善思路的过程。

---

## 一个对比

任务：写一段产品介绍。

**第一种，大多数人熟悉的写法：**

> 帮我写一段项目管理和团队协作工具的简介。
AI 输出：

> 本产品是一款集项目管理和团队协作于一体的综合性工具，旨在提高团队工作效率，促进成员间的沟通与协作。通过多样化的功能和直观的用户界面，为团队提供一站式的解决方案……
读完了。能记住什么？什么都记不住。每一句都说"我很好"，没一句说"我好在哪里"。

**第二种，PICCO 版本：**

```
【Persona】
SaaS 产品定位专家，8 年 B2B 经验。写法：场景切入 + 数据锚点 + 一句话差异。

【Instructions】
写 150 字产品简介。结构：一个真实的协作痛点开场 → 引出产品 → 说清和 Notion 的区别。

【Context】
目标用户 5-20 人远程开发团队。功能：任务看板 + 文档协作 + AI 自动周报。定价 $8/人/月。

【Constraints】
禁止：'一站式' '赋能' '高效' '无缝体验'。每段不超过 3 行。

【Output】
一段 150 字以内的产品简介。
```

AI 输出：

> 周五下午 5 点，你打开三个不同的工具，试图拼出一份周报。项目进度在 Linear 里，文档在 Notion 里，工时记录还在同事的聊天记录里。这个产品把这三件事放在一个页面里：左边任务看板，右边协作文档，AI 在后台自动生成周报草稿。和 Notion 的区别：它是给"干活的人"用的，不是给"写文档的人"用的。$8/人/月，一个项目周期回本。
同一个 AI，不同的 Prompt，输出的结果就大有不同。

---

不要把每次写 Prompt 当成填空。得养成这种习惯，哪怕你觉得很八股，很烦，每次要想半天。但时间长了，强迫自己这么考虑问题，其实你会发现**这不是个提示词框架，这TM就是思维框架，典型的需求洞察分析逻辑。**

到那时候你会感觉到一个变化：以前是你跟着AI一问一答，慢慢你就变成一个凡事都有了**owner思维**。

---

## 附：写完 Prompt 看看

- [ ] **Persona**：AI 知道它在扮演谁吗？
- [ ] **Instructions**：你给它画轨道了吗？（有具体框架？）
- [ ] **Context**：你给它事实了吗？（至少两条？）
- [ ] **Constraints**：禁止项是硬规矩还是软建议？（有一条"尽量"就算没写）
- [ ] **Output**：AI 知道答案长什么样吗？（格式够不够具体？）
五条都没打勾，你的 Prompt 只能发挥三成功力。

---

[https://abs.twimg.com/emoji/v2/svg/1f4da.svg](https://abs.twimg.com/emoji/v2/svg/1f4da.svg)**� 历史文章汇**总

1. [超级虚拟团队：多Agent协作实战指南](https://x.com/thinkszyg/status/2055586218973491708?s=20)
2. [AI 正在提高你的创业失败率](https://x.com/thinkszyg/status/2055840093794074816?s=20)
3. [我手搓了一个 Chrome 插件，把 X 收藏夹批量整理成 Obsidian 知识库](https://x.com/thinkszyg/status/2055882529849344342?s=20)
4. [DeepSeek 一张 JD，就是 2026 年 AI 入行说明书](https://x.com/thinkszyg/status/2056532044809990215?s=20)
5. [管10个AI员工3个月，烧了234个坑才悟出来的5条铁律](https://x.com/thinkszyg/status/2056895769257693389?s=20)
6. [一个失败老新人的 X 账号避坑复盘：我差点把 16 年老号玩废了](https://x.com/thinkszyg/status/2057273598361076017?s=20)
7. [别把 Codex 只当代码助手，它正在变成工作流系统](https://x.com/thinkszyg/status/2057427584909291987?s=20)
8. [Codex 的 Pinned Threads，到底该怎么用？](https://x.com/thinkszyg/status/2057736054657130695?s=20)
9. [Codex App 不折腾上手指南：先会这几个命令就够了](https://x.com/thinkszyg/status/2058004216904564747?s=20)
10. [保姆级：用AI搭建你的选题流水线，从信息源到选题入库全流程](https://x.com/thinkszyg/status/2058411489908973679?s=20)
11. [选题有了然后呢？从文案公式到去AI味，AI辅助写作全流程](https://x.com/thinkszyg/status/2058740603450806527?s=20)
12. [10 分钟搭一个会自己进化的知识库：Obsidian + Claude Code 实操](https://x.com/thinkszyg/status/2059208197206868248?s=20)
13. [AnySearch：给 AI Agent 装上一双会搜索的眼睛](https://x.com/thinkszyg/status/2059289301775741411?s=20)

---

如果这篇对你有帮助，欢迎 关注 + 收藏 + 转发 [https://abs.twimg.com/emoji/v2/svg/1f44f-1f3fb.svg](https://abs.twimg.com/emoji/v2/svg/1f44f-1f3fb.svg)🏻

关注 [@thinkszyg](https://x.com/@thinkszyg) ，持续分享真实战，生产级，AI真干货。

https://x.com/thinkszyg/article/2059568031513424020

— [爆裂队长NEXT (@thinkszyg)](https://x.com/thinkszyg/status/2059568031513424020) · 2026-05-27 17:30
