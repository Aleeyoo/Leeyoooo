---
Belongs to: "[[Agent工程]]"
aliases: ["ReAct Loop Implementation", "Agent内部结构", "300行Agent"]
tags: ["react-loop", "agent-internals", "context-management", "tool-safety", "agent-basics"]
created: 2026-06-06
source: ai-generated
source_url: "https://quantumentangled.dev/viewpost/11/whats-actually-inside-an-ai-agent-a-300-loc-react-loop"
concepts: ["ReAct-loop", "context-window-as-cost", "tool-as-attack-surface", "shell-exec-danger", "context-flooding", "custom-domain-agents"]
confidence: high
---
# Agent ReAct三百行实现

> [!abstract]- AI 摘要
> 亲手实现一个约 300 行 ReAct Agent 循环后，所有"AI 助手"都不再是黑箱。你会发现 Agent 的核心结构极其简单（观察-思考-行动循环），但每一层都暗藏风险——工具是攻击面、上下文是成本中心、prompt 是关键变量。

---

## 扫读

> [!tip] 💡 一句话
> Agent 的本质是 ReAct 循环：观察→思考→调用工具→接收结果→再观察。亲手实现它之后你会问对问题：我的上下文里有什么？我的工具能碰什么？模型错了会怎样？Prompt 到底多重要？

> [!important] 📌 关键结论
> - Agent 内部结构极简：一个 ~300 行的 ReAct 循环（Observe → Think → Act → Observe），但简洁不等于安全——Actions 是任意函数调用，一旦接入 shell.exec 就等于给了模型 rm -rf 的能力
> - 上下文窗口是每次迭代的成本放大器：每一步都重发完整历史，不做管理的 Agent 会迅速耗尽 token 预算——且 provider 的利益是让你多烧 token
> - 工具（Actions）是 Agent 的真正能力边界——内部 API 调用、告警生成、数据库查询、网页搜索、内部流程执行，这些都可以成为 Actions，让 Agent 从通用助手变成领域专用工具
> - 自建 Agent 的最大价值是让人从"用黑箱"变成"问对问题"：我的上下文里有什么、工具能碰什么、模型错了怎么办、prompt 多关键

> [!quote] 🎬 可行动项
> - 亲手实现一个 ~300 行的 ReAct Agent（参考 Simple-ReAct-Agent），理解 Agent 的内部循环结构
> - 审计每个 Action/工具的安全边界：它能删除文件吗？能执行任意命令吗？能访问敏感数据吗
> - 为 Agent 添加上下文预算控制：最大迭代次数限制 + 每轮 token 计数 + 超限自动压缩或中断
> - 从自建 Agent 开始思考领域专用 Actions——你的业务系统需要哪些内部 API 作为 Agent 工具

---

## 精读

### 论证链

```
作者动机：去看穿 AI Agent 的炒作 → 亲手实现一个简化 ReAct Agent
  → 先写伪代码 → 读相关论文 → 和主流 Agent 对话验证 → 产出 ~300 行实现
        ↓
Agent 核心 = ReAct 循环：Observe → Think → Act → Observe
  每次迭代：从环境获取观察 → LLM 推理下一步 → 调用工具执行 → 接收结果 → 继续循环
  简单模型 + 单步错误 → 整个链条被污染（错误传播）
        ↓
两个关键认识：
  ① Actions 可以是任何东西——这是最大的风险和最大的机会
     风险侧：shell.exec = rm -rf，模型可直接删除文件
     机会侧：内部 API、告警生成、数据库查询、网页搜索、内部流程都可以是 Actions
     → 生产事故的根源：git commits 含 secrets、source.zip 推入 npm registry、Agent 进入不该进入的 shell
  ② 上下文窗口是每次迭代的成本中心
     每步都重发完整历史 → 上下文越长越贵
     prompt caching、compaction、summaries 是缓解手段，但 provider 的利益是让你多烧 token
     → 严肃对待上下文管理是软件工程师的责任
        ↓
结论：亲手写完 ReAct 循环后 "AI 助手" 不再是黑箱
  → 你会开始问对问题：context 里有什么、tool 能碰什么、模型错了会怎样、prompt 有多关键
  → 应该开始为各自领域构建定制 Agent，Actions 可以是业务系统的任意功能
```

### 关键引述

> "An Action is any function call you wire up. If you wire up shell.exec, you have wired up rm -rf for the model, allowing it to delete your files at will."

> "The context window matters, because every step re-sends the entire history. Keep it small or pay for it on every iteration."

> "It isn't in the provider's interest to help you spend fewer tokens. That's how they actually profit from this."

> "Once you've written the loop yourself, every 'AI assistant' stops being a black box. You start asking the right questions."

> "What we as Software Engineers should do is take context management seriously and start building custom agents for our own domains."

### 局限与盲区

- 本文未覆盖：生产级 Agent 需要的基础设施（错误恢复、并发控制、状态持久化、审计日志）远超出 300 行 demo 的范围；不同模型（Frontier vs 本地小模型）在 ReAct 循环中的行为差异和适配策略；Agent 的安全沙箱方案（容器化、权限最小化、只读文件系统）的具体实现
- 隐含假设：假设读者有亲手实现 Agent 的技术能力和意愿——实际上很多 AI 从业者停留在用现成工具的层面；假设本地小模型的表现缺陷可以通过更好的工程实践弥补——但某些推理能力是模型规模决定的硬边界
- 可能的反例：对于纯文本对话 Agent（无工具调用），ReAct 循环退化为单纯的对话轮次，上下文管理策略完全不同；某些生产 Agent（如 Claude Code）的架构远超 ReAct 简单循环，包含了 worktree 隔离、动态编排、对抗性验证等复杂机制

---

## 关联

- [[Agent最简实现原理]] —— 从工程角度解释 Agent 如何工作，与本文的 ReAct 循环互相补充
- [[Agent本质一文讲清]] —— Agent 概念的高层解释
- [[动态Harness设计模式]] —— Anthropic 官方对 Agent 退化（laziness/bias/drift）的解决方案
- [[Agent Doom Loop检测与防护]] —— Agent 在死循环中反复失败的问题与防护
- [[Harness四层控制体系]] —— 本文提到的上下文管理和工具约束在 Harness 工程中的系统化实现
