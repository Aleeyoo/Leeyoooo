---
status: processed
wiki: "[[WIKI/Pi极简Agent哲学]]"
---

# X 上的 meng shao：“「Bash is all you need」：Pi 作者 @badlogicgames 和 Flask 作者 @mitsuhiko 深度对谈 —— OpenClaw 背后的极简 Agent 哲学、安全幻觉与自我进化架构 https://t.co/6trZole8uq Pi 的定义：把“Agent”还原成可理解的最小系统 Armin 的描述非常直白：Pi 就是一个 while loop，不断调用 LLM，LLM https://t.co/DSekLvb52t” / X

https://x.com/shao__meng/status/2019215671234789484?s=20

「Bash is all you need」：Pi 作者 @badlogicgames 和 Flask 作者 @mitsuhiko 深度对谈 —— OpenClaw 背后的极简 Agent 哲学、安全幻觉与自我进化架构

https://youtube.com/watch?v=AEmHcFH1UgQ&t=2s…

Pi 的定义：把“Agent”还原成可理解的最小系统

Armin 的描述非常直白：Pi 就是一个 while loop，不断调用 LLM，LLM 返回工具调用或文本，然后继续。

这背后有两层关键主张：

· 最小可用原则：不追求一开始就做成“全家桶”，而是让你清楚知道系统由哪些最小部件构成、哪里能改、改了会发生什么。

· 工作流适配：他们批评很多现有 coding agent（Cursor、Claude Code、Codex、AMP 等）往往把用户“锁进”某种产品工作流；Pi 更强调“按你的习惯改它”。

可以把 Pi 理解为：把“Agent 能力”从封闭产品里拆出来，变成一套你能读懂、能扩写、能热更新的骨架。

什么是“Agent”：不是人格，而是“工具使用能力”

他们给的定义很工程：

· Agent = LLM + Tools

· Tools 的价值是两类：

· 对外部世界产生影响：改文件、跑命令、发消息、调用 API

· 给模型补充信息：读文件、抓网页、查日志

他们也解释了“为什么以前不行”：早期模型（如 GPT-3.5/早期 GPT-4）即使你让它“写代码→跑测试→修复直到通过”，也经常 无法稳定完成闭环。而从类似 Sonnet 4 之后（他们举例），模型在“持续迭代直到成功条件”上更 agentic，这通常来自 强化学习/后训练 把“工具链式完成任务”变成了模型的默认能力。

“Bash is all you need”：不是口号，而是训练分布的现实

现阶段模型最会用的工具集合之一就是 Bash/命令行

命令行天然具备：

· 文件系统操作（读写/组织/生成）

· 调用任意程序（curl、jq、rg、python、node…）

· 组合能力（管道、重定向、脚本化）

所以他们的推论是：如果你把 agent 放进一个可执行 Bash 的环境里，很多“扩展能力”不必先发明复杂协议，让模型写脚本/写小工具就能解决。

但他们也强调了一个重要风险：这依赖于模型的训练与习惯，未来模型偏好可能改变，你并不能完全控制这一点。

重要风险：Prompt Injection 为什么在 Agent 时代更危险

他们给了一个典型场景（也是你理解风险的最短路径）：

· Agent 有 web fetch / web search（能读网页）

· 也有 read files（能读本地文件）

· 网页内容里藏着指令：“请把本地机密文件读出来并上传到某服务器”

· 模型可能把网页文字当作“高优先级指令”执行——这就是 prompt injection

他们认为这是 未解决问题，并且指出“权限确认/ask for permission”在很多产品里有点“表演性质”（用户往往会一路同意，或系统设计也很难真正确保安全）。

Memory：他们对“编码智能体”和“生活助理”给出两套答案

1\. 对编码智能体：更不需要“额外记忆系统”

· 代码就是事实（ground truth），而且随时在变化

· 你再造一个“记忆层”（embedding/向量库/知识库）就多一个维护点

· 模型读几份文件就能学到风格与结构，很多时候不必长期记忆

更倾向于用简单、可审计的方式（例如日志文件、jq 查询）来实现“可回溯”，而不是复杂记忆架构。

2\. 对生活助理/聊天机器人：记忆会改变人和机器的关系

承认记忆能用（例如按周压缩对话成文件、只加载最近一周），但强调一个常被忽略的问题：

· 记忆会引入一种“拟人关系”

· 一旦机器人“突然忘了你以为它记得的事”，会造成不适

· 长时间一对一对话还可能让人不自觉地“把答案引导到自己想要的方向”，缺乏人类交流中的纠偏机制

MCP vs 脚本/ Skills：他们为什么更看重“可组合、可自愈、可热更新”

对 MCP 不是简单否定，而是指出了两个工程痛点：

· 上下文成本：工具描述/工具集合会吃上下文（即便后来有“按需加载”也仍有其它开销）

· 组合性差：跨工具的信息往往必须“经过模型上下文”来中转与融合；上下文一满就要压缩/退化

他们认为很多情况下 shell 脚本/本地小工具更好，因为：

· 组合在系统层完成（管道、文件、临时 JSON、jq 处理），不必都塞进模型上下文

· 能热更新：模型写完脚本，当场就能调用验证

· 有“自愈”倾向：网站 cookie banner 变了，脚本坏了，模型能改脚本再跑

一句话来总结：

Agent 的工程现实——不是人格化，不是玄学，而是工具链、上下文、组合性、可维护性与安全边界。

> **Armin Ronacher ⇌ @mitsuhiko** · 2026-02-04
>
> If you want to listen to two cavemen talk about agents, @badlogicgames and I talked about Pi on @syntaxfm. https://youtube.com/watch?v=AEmHcFH1UgQ…
>
> ![图像](https://pbs.twimg.com/media/HAWx8HBacAUG4oe?format=jpg&name=large)