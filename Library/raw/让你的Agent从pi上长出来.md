---
status: processed
wiki: "[[WIKI/Pi Harness Agent开发]]"
---
# 让你的Agent从pi上长出来

## 太长不看：把这个丢给你的codex/claude：https://github.com/enderzcx/pi-docs-playbook

## 

## **别急着写 Agent，先把 pi 文档喂给 Codex 读对**

一开始接了个业务，只是把大模型封装了一下，以为这样就能当业务 Agent 卖出去了。

很快你会发现：要想真正让 AI 帮你做业务、查单、提效率，不能只靠一个大模型。你还需要给它装上底座、轮子、手脚，以及验证机制。

既然我们要做一个可靠、稳定、可验证的系统，而不只是一个 Chatbot，那就需要一个 harness。pi 正是这样一个好用的 harness 底座。~~一个含苞待放、待人开发的尤物~~

本文想讲的不是“pi 教程”，而是如何学习 pi，如何把你的业务嫁接上去。为此我专门建了一个 repo，把 pi 官方文档整理归档、分类清楚，告诉你和你的 coding agent 应该怎么读、读哪些、什么时候读。

重点是：在这个大家都用最强 coding agent 去构建其他 Agent 的时代，你可以直接把这个 repo 丢给 Codex / Claude / Cursor。它会按文档指引提问，结合你的业务，针对性地把 pi 拓展成你需要的业务 Agent。

**我建了 pi-docs-playbook 这个 repo**

为了让别人少走弯路，我把 pi 官方文档和相关 repo docs 整理归档，做成了：

https://github.com/enderzcx/pi-docs-playbook

它不是 pi 教程，也不是 fork，而是一个 ****documentation harness****。

结构大概是这样：

- source/：原样镜像官方 Markdown，保留上游路径，可离线精确引用

- catalog/：按用途重新分类，比如 core runtime & harness、official coding-agent docs、examples、upstream prompts / skills、validation & changelogs

- usage/task-reading-matrix.md：核心文档。按真实开发场景告诉你该读哪些文件，比如 SDK vs RPC 选型、会改业务数据的 tool wrapper 设计、session trace 与 application audit 的区分、context / compaction 风险、approval UX 搭建、skills / packages 组织方式

- AGENTS.md：给 coding agent 的铁律。进来必须先读 README + task-reading-matrix，按问题类型精准阅读 `source/` 里的原始文档，回答时必须引用本地 `source/` 路径，禁止凭记忆猜测 pi 行为

- PROMPT.md：直接复制扔给 Codex / Claude / Cursor 即可使用的 prompt

- examples/：真实问题示例

- usage/how-to-use-this-repo.md：人类自己怎么用

这个 repo 解决的核心问题是：

**让最强的 coding agent 也能高效、正确地获取 pi 信息，而不是到处幻觉。**

**具体怎么用**

很简单，扔给 Agent 就行。

```bash

git clone https://github.com/enderzcx/pi-docs-playbook

```

然后：

1. 把整个 repo + PROMPT.md 里的 prompt 一起给你的 coding agent

2. 让它先读 AGENTS.md 和 usage/task-reading-matrix.md

3. 再把你的业务丢给它：你想做什么 Agent、核心业务动作是什么、哪些操作需要强验证、你的 channel 是什么

它会自己按矩阵去 source/ 里读对文档，并主动问你业务里的关键决策点：

- 哪些操作必须实时查 DB？

- 审批门控怎么切入？

- 业务状态要不要显式建模在 harness 里？

- channel 适配放哪一层？

- tool wrapper 和 audit 映射怎么设计？

人类自己也可以直接看catalog/ 分类和 usage/how-to-use-this-repo.md，快速定位到该读的部分。

想立刻让 Agent 用上，直接复制 `PROMPT.md` 内容扔给它。

Star 一下，后面我会持续往里面加真实踩坑后的 matrix。

**---**

**不要急着写代码**

先把 pi 的文档喂对，再开始设计你的 harness。

否则你配的 Agent 只会用上下文幻觉去“实现业务”，跑起来才发现到处都是坑。

https://x.com/0xenderzcx/article/2061778310934516097

— [Sunny (@0xenderzcx)](https://x.com/0xenderzcx/status/2061778310934516097) · 2026-06-02 19:53
