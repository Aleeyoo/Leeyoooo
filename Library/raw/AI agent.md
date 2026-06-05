---
status: processed
wiki: "[[WIKI/Agent最简实现原理]]"
---
# Post by @GoSailGlobal on X

https://x.com/GoSailGlobal/status/2056395622111170753?s=20

AI agent 这个词被吹了一整年 真打开看 一共 300 行代码

GitHub 有个项目叫 Simple-ReAct-Agent

把 ReAct 论文那个循环直接写了一遍

没用 LangChain，没用 LlamaIndex，没用 AutoGen

就是一个 while 循环

循环里只有三件事

第一 把当前历史 + 任务 + 工具列表 拼成 prompt 喂给模型

第二 模型输出 Thought / Action / Action Input

第三 执行 Action 把结果拼回历史 进下一轮

完事！！！

· 300 行看完一遍，几个被框架包装得很玄的概念立刻祛魅

memory？历史拼接

tool？JSON schema + 函数指针

planning？让模型在 Thought 里写下一步该干什么

self-correction？把错误结果也拼回历史 让模型自己看到然后改

· 但最有杀伤力的发现，不在祛魅这一段

是作者那句

「Action 可以是任何东西」

你给 agent 接了 shell.exec

就等于把 rm -rf 交给了模型

最近几条新闻全是这么来的

agent 自己 commit 把 API key 写进了仓库

agent 自己 npm publish 把 http://source.zip 推上去

agent 跑了一个不该跑的 shell 命令，把机器删了

agent 跟工具单独看都没问题

问题出在「工具暴露面」这一层被低估了

· 第二个被忽略的细节

context window 每一轮都把整段历史重发一次

10 步循环，同一段 prompt 送进模型 10 次

prompt caching 能省一些 但省不掉结构性消耗

· 作者那句话挺扎心

「让你少烧 token 这件事，不在 provider 的商业利益里」

· 写完这 300 行 你能换一种视角看每一个 AI 助手

我的 context 里现在装着什么

我接出去的工具能碰到什么

模型答错的时候，损失会从哪一处扩散

Prompt 在这一切里的权重到底有多大（剧透 非常大）

· GitHub

https://github.com/rulyone/Simple-ReAct-Agent…

· 原文（300 行代码 + 完整拆解）

https://quantumentangled.dev/viewpost/11/whats-actually-inside-an-ai-agent-a-300-loc-react-loop…

![图像](https://pbs.twimg.com/media/HInI0R9aoAAx5zt?format=jpg&name=large) ![图像](https://pbs.twimg.com/media/HInI3PmasAAjIiF?format=jpg&name=large)