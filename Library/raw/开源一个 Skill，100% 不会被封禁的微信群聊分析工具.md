---
status: processed
wiki: "[[WIKI/微信社群活跃度分析]]"
---
# 开源一个 Skill，100% 不会被封禁的微信群聊分析工具 🔥

**你有没有想过，怎么知道你的微信群里，到底谁是活人，谁是死人？**

*PS：全文将近 1900 字，建议先收藏再慢慢阅读 *[https://abs.twimg.com/emoji/v2/svg/1f618.svg](https://abs.twimg.com/emoji/v2/svg/1f618.svg)�

我运营社群十几年了，早期有一个非常粗暴但极其有效的方法——**把活跃的人提出来当管理员，不活跃的直接踢**。花半年时间，群的氛围就形成了。之后我什么都不用管，这个氛围自己延续了十几年

但问题是——**你怎么知道谁活跃、谁不活跃？**

最近两个月谁说过话？最近三个月呢？六个月呢？微信本身根本不提供这个功能

所以我花了一点点[https://abs.twimg.com/emoji/v2/svg/1f90f.svg](https://abs.twimg.com/emoji/v2/svg/1f90f.svg)�时间，做了一个工具，今天把它开源了

**本文大纲：**

[https://abs.twimg.com/emoji/v2/svg/1f916.svg](https://abs.twimg.com/emoji/v2/svg/1f916.svg)**� 第一反应：群聊机器人，但封号风险太**高

[https://abs.twimg.com/emoji/v2/svg/1f4a1.svg](https://abs.twimg.com/emoji/v2/svg/1f4a1.svg)**� 换个思路：微信聊天记录是加密的，但能**解

[https://abs.twimg.com/emoji/v2/svg/1f513.svg](https://abs.twimg.com/emoji/v2/svg/1f513.svg)**� 技术路径：从本地数据库解密到数据分**析

[https://abs.twimg.com/emoji/v2/svg/1f4ca.svg](https://abs.twimg.com/emoji/v2/svg/1f4ca.svg)**� 三小时做出来的东西长什么**样

[https://abs.twimg.com/emoji/v2/svg/1f3af.svg](https://abs.twimg.com/emoji/v2/svg/1f3af.svg)**� 不只是踢人：群聊分析还能干什**么

[https://abs.twimg.com/emoji/v2/svg/26a1.svg](https://abs.twimg.com/emoji/v2/svg/26a1.svg)** 彩蛋：这篇文章本身就是 AI 流水线的产物**

Github 地址：https://github.com/punk2898/wechat-group-stats

废话不多说，正文开始[https://abs.twimg.com/emoji/v2/svg/1f64f.svg](https://abs.twimg.com/emoji/v2/svg/1f64f.svg)�

[https://abs.twimg.com/emoji/v2/svg/1f9f5.svg](https://abs.twimg.com/emoji/v2/svg/1f9f5.svg)[https://abs.twimg.com/emoji/v2/svg/1f9f5.svg](https://abs.twimg.com/emoji/v2/svg/1f9f5.svg)[https://abs.twimg.com/emoji/v2/svg/1f9f5.svg](https://abs.twimg.com/emoji/v2/svg/1f9f5.svg)�🧵

## 一、第一反应：群聊机器人，但封号风险太高 [https://abs.twimg.com/emoji/v2/svg/1f916.svg](https://abs.twimg.com/emoji/v2/svg/1f916.svg)�

**想做社群数据分析，第一反应肯定是群聊机器人**

![](https://pbs.twimg.com/media/HJ70G2tbwAAkN5b.jpg)

这个方案确实很成熟。它能帮你统计发言频次、关键词、活跃度排名，功能非常全面

但它有一个天生的死穴——**会被微信封号**，懂得都懂，反正我不想被封号

说白了，群聊机器人这条路，封号风险太高了。你为了分析几个群的数据，把用了十几年的微信号搭进去？

不值

所以我在想——**有没有一种方式，完全不碰微信的 API，完全不跟微信服务器交互，但还是能拿到群聊数据？**

## 二、换个思路：微信聊天记录是加密的，但能解 [https://abs.twimg.com/emoji/v2/svg/1f4a1.svg](https://abs.twimg.com/emoji/v2/svg/1f4a1.svg)�

我突然想起来一件事——微信的聊天记录是存在本地的

![](https://pbs.twimg.com/media/HJ70JGnbAAEm6jX.jpg)

以前我经常干的一件事是导出微信聊天记录做备份。虽然它是加密的，但早就有各种方式可以解密

顺着这个思路，答案就很清楚了：

**我只需要把微信本地的加密数据库解密，剩下的就是数据分析的事了**

微信跟我半毛钱关系都没有。我操作的是自己电脑上的本地文件，不调用任何微信接口，不注入任何进程，不发送任何消息

**你封我？你凭什么封我？我又没碰你**

这就是 **100% 不会被封禁** 的底气

## 三、技术路径：从本地数据库解密到数据分析 [https://abs.twimg.com/emoji/v2/svg/1f513.svg](https://abs.twimg.com/emoji/v2/svg/1f513.svg)�

***ps：跳过吧，不用看！***[https://abs.twimg.com/emoji/v2/svg/1f64f.svg](https://abs.twimg.com/emoji/v2/svg/1f64f.svg)***�跳过吧，不用看***[https://abs.twimg.com/emoji/v2/svg/1f64f.svg](https://abs.twimg.com/emoji/v2/svg/1f64f.svg)***🙏跳过吧，不用***[https://abs.twimg.com/emoji/v2/svg/1f64f.svg](https://abs.twimg.com/emoji/v2/svg/1f64f.svg)***！🙏跳过吧，不***[https://abs.twimg.com/emoji/v2/svg/1f64f.svg](https://abs.twimg.com/emoji/v2/svg/1f64f.svg)***看！🙏跳过吧，***[https://abs.twimg.com/emoji/v2/svg/1f64f.svg](https://abs.twimg.com/emoji/v2/svg/1f64f.svg)***用看！🙏跳过吧***[https://abs.twimg.com/emoji/v2/svg/1f64f.svg](https://abs.twimg.com/emoji/v2/svg/1f64f.svg)不用看！🙏

微信 4.0 用的是 SQLCipher 4 加密本地数据库，加密算法是 AES-256-CBC + HMAC-SHA512，KDF 迭代 256000 次

![](https://pbs.twimg.com/media/HJ70LR0bwAAqMru.jpg)

听起来很吓人对吧？但腾讯自己的 WCDB 封装会在进程内存中缓存解密后的 raw key。GitHub 上已经有成熟的开源库可以自动扫描内存提取密钥，Windows、macOS、Linux 三个平台都支持

整个流程其实就三步：

[https://abs.twimg.com/emoji/v2/svg/31-20e3.svg](https://abs.twimg.com/emoji/v2/svg/31-20e3.svg)** 运行解密工具，从微信进程内存提取加密密钥**

[https://abs.twimg.com/emoji/v2/svg/32-20e3.svg](https://abs.twimg.com/emoji/v2/svg/32-20e3.svg)** 用密钥解密本地 SQLCipher 数据库，得到标准的 SQLite 文件**

[https://abs.twimg.com/emoji/v2/svg/33-20e3.svg](https://abs.twimg.com/emoji/v2/svg/33-20e3.svg)** 在 SQLite 里查询你想要的群聊数据，按你的需求整理分析**

macOS 上有个小坑——需要先给微信重签名一次，不然读不了进程内存。一行命令的事，codesign --force --sign - 搞定，只需要做一次

这是整个项目最麻烦的一步，但走通了之后，后面就非常简单了

解密完你能拿到什么？会话列表、聊天记录、联系人、媒体文件索引……基本上微信里有的数据，本地都有

## 四、三小时做出来的东西长什么样 [https://abs.twimg.com/emoji/v2/svg/1f4ca.svg](https://abs.twimg.com/emoji/v2/svg/1f4ca.svg)�

![](https://pbs.twimg.com/media/HJ70NHxagAAMUFS.jpg)

有了解密后的数据，剩下的就是按需求整理

我的核心需求很简单：**我要知道某个群里，谁活跃，谁不活跃**

所以我做了一个 Web Dashboard，暗色主题，可排序、可搜索。顶部是概览数据，下面是每个成员的详细统计

拿我自己的一个群来说——**414 人，累计 15588 条消息，近一个月活跃 90 人，从来没说过话的"死号" 149 个**

149 个死号，占了整个群的 35%。你说这群活跃度能高吗？

每个成员会自动分成六档：

- [https://abs.twimg.com/emoji/v2/svg/1f525.svg](https://abs.twimg.com/emoji/v2/svg/1f525.svg)�** 超活**跃 — 群里的核心发言者
- [https://abs.twimg.com/emoji/v2/svg/1f7e2.svg](https://abs.twimg.com/emoji/v2/svg/1f7e2.svg)�** 活**跃 — 经常冒泡
- [https://abs.twimg.com/emoji/v2/svg/1f7e1.svg](https://abs.twimg.com/emoji/v2/svg/1f7e1.svg)�** 偶**尔 — 隔三差五说一句
- [https://abs.twimg.com/emoji/v2/svg/1f7e0.svg](https://abs.twimg.com/emoji/v2/svg/1f7e0.svg)�** 低**频 — 几个月才出现一次
- [https://abs.twimg.com/emoji/v2/svg/1f534.svg](https://abs.twimg.com/emoji/v2/svg/1f534.svg)�** 沉**水 — 基本消失了
- [https://abs.twimg.com/emoji/v2/svg/1f480.svg](https://abs.twimg.com/emoji/v2/svg/1f480.svg)�** 死**号 — 从来没说过话
最重要的就是那份 [https://abs.twimg.com/emoji/v2/svg/1f480.svg](https://abs.twimg.com/emoji/v2/svg/1f480.svg)� 死号名单—**—把这些人筛出来，该踢的**踢

这是我十几年社群运营的核心方法论：**把活跃的人找出来当管理员，把不活跃的给踢出去**。花半年时间做这件事，群的氛围就自然形成了。之后不需要你管，这个氛围十几年都不会散

从开始写到做完，大概三个小时。又花了一个小时把代码整理了一下，开源到 GitHub 上

整个东西做成了一个 Skill，Skill 会自动引导你完成签名、密钥提取、解密、分析、启动 Dashboard 的全流程。分析结果也会导出 JSON，你可以接入自动化脚本或者定时任务

## 五、不只是踢人：群聊分析还能干什么 [https://abs.twimg.com/emoji/v2/svg/1f3af.svg](https://abs.twimg.com/emoji/v2/svg/1f3af.svg)�

![](https://pbs.twimg.com/media/HJ70OyFbIAADXOg.jpg)

除了社群管理，这个工具还有很多玩法

![](https://pbs.twimg.com/media/HJ70QczaYAAegcM.jpg)

你可以分析群里的人都在聊什么——聊美股、聊数字货币、聊 AI、聊八卦？**把关键词提取出来做个词频分析**，你就知道这个群最近的情绪和关注点

你还可以做**跨群对比**——你的投资群跟你的技术群，聊天活跃度差多少？话题分布有什么不同？

甚至可以做**时间序列分析**——某个话题在群里的讨论热度变化，"降息"这个词在过去三个月被提到了多少次，趋势是上升还是下降

所有这些，都建立在一个前提上：**100% 纯本地操作，不触碰微信任何接口，零封号风险**

这是跟群聊机器人方案最本质的区别

## 六、彩蛋：这篇文章本身就是 AI 流水线的产物 [https://abs.twimg.com/emoji/v2/svg/26a1.svg](https://abs.twimg.com/emoji/v2/svg/26a1.svg)

说到这个 Skill，顺便聊一下我现在整套内容产出流程

你现在看到的这篇文章，产出流程是这样的：

[https://abs.twimg.com/emoji/v2/svg/31-20e3.svg](https://abs.twimg.com/emoji/v2/svg/31-20e3.svg) **我口述内容（语音输入）**

[https://abs.twimg.com/emoji/v2/svg/32-20e3.svg](https://abs.twimg.com/emoji/v2/svg/32-20e3.svg) **AI 根据我的口述做格式排版、校正、风格适配**

[https://abs.twimg.com/emoji/v2/svg/33-20e3.svg](https://abs.twimg.com/emoji/v2/svg/33-20e3.svg) **自动生成配图**

[https://abs.twimg.com/emoji/v2/svg/34-20e3.svg](https://abs.twimg.com/emoji/v2/svg/34-20e3.svg) **自动生成封面图**

[https://abs.twimg.com/emoji/v2/svg/35-20e3.svg](https://abs.twimg.com/emoji/v2/svg/35-20e3.svg) **自动归档到 GitHub**

从一个想法到一篇完整的文章发布，中间几乎不需要我打字。这就是我下一篇文章要讲的——**AI 时代的产业协作模式，已经天翻地覆了**（***因为依然附带了一个 Skill 所以需要多一点时间，加个小铃铛很快发出来***）

这套东西其实比今天开源的群聊分析 Skill 更加牛逼，但太重了，不是三小时能整理清楚的

加个小铃铛，敬请期待吧[https://abs.twimg.com/emoji/v2/svg/1f601.svg](https://abs.twimg.com/emoji/v2/svg/1f601.svg)�

https://x.com/punk2898/article/2062354507754029175

— [Punk（2898 ） (@punk2898)](https://x.com/punk2898/status/2062354507754029175) · 2026-06-04 10:03
