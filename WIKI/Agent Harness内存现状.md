---
Belongs to: "[[Agent工程]]"
aliases: ["Agent Harness Memory", "Agent内存现状", "Harness内存对比", "Agent记忆基础设施"]
tags: ["Agent记忆", "Harness", "技术对比", "基础设施", "Mem0"]
created: 2026-06-03
source: ai-generated
source_url: "https://x.com/mem0ai/status/2061822612398014782"
concepts: ["三级记忆分类(Working/External/Parametric)", "Harness内存边界", "多信号检索(Multi-signal retrieval)", "记忆过时处理(Staleness handling)", "记忆隔离与污染", "内存基准评测批判", "记忆基础设施化"]
confidence: medium
---

# Agent Harness内存现状

> [!tip] 💡 一句话核心主张
> 每个主要 Agent Harness 都发布了内存功能，但它们都在同一个边界处断裂——有界本地存储、关键词检索、Harness 范围限制、弱过时处理、隔离缺口。记忆正在从每个 Harness 的独立特性变成需要统一基础设施的横向层。

> [!important] 📌 关键结论
> - 结论1：Agent 记忆分三层——工作记忆（会话内上下文窗口）、外部记忆（持久化的向量库/知识图谱/文件）、参数记忆（通过权重更新编码）——2026 年几乎所有生产环境记忆都在外部记忆层。
> - 结论2：各 Harness 的实现差异巨大但短板趋同：Claude Code 按文件名选择（非语义）、Codex 用 grep 检索（子串匹配）、Hermes 用 FTS5（关键词）、Copilot 是唯一有发布数据验证过时处理机制的系统（PR 合并率从 83% 提升到 90%）。
> - 结论3：当前内存基准评测本身有严重缺陷——LoCoMo 仅 10 个对话、trivial grep 基线 ~74% 得分、不测试记忆引导行动的能力、不测试生产级 token 量（10M+）。

> [!quote] 🎬 可行动项
> - 评估 Harness 选择时，不仅看功能列表，还要实际测试其内存检索是语义还是关键词、跨会话持久化是否有截断上限
> - 团队使用场景优先考虑支持语义检索和身份隔离的内存方案，避免跨用户记忆污染（论文报告 57-71% 的交叉污染率）
> - 对内存基准分数持怀疑态度——LoCoMo 近饱和，MemoryArena 才测试内存"引导行动"的能力但多数系统不报告
> - 如需跨 Harness 的持久记忆，考虑外部内存基础设施（如 Mem0）而非被单一 Harness 锁定

### 论证链

```
**观点 → 论据 → 案例**

**观点1：每个 Harness 都在用自己的方式解决内存问题**

文章逐一分析了 9 个主要 Harness 的内存实现：
- **Claude Code**：CLAUDE.md（人类编写）+ Auto-memory（Agent 自动提取，MEMORY.md 索引上限 200 行/25KB，四个类别）。检索按文件名选择，每轮最多载入 5 个文件，超限静默截断。
- **Codex**：~/.codex/memories/ 目录，两阶段写入（每 rollout 提取 + 全局合并），读取用 5000 token 摘要 + grep 搜索。六小时空闲后才触发提取，连续会话可能永远不合并。
- **Copilot**：结构化记忆对象（subject + factual content + 文件引用 + reasoning），使用前验证引用是否过期，28 天自动过期。唯一的经验数据：PR 合并率从 83% 提升到 90%（p<0.00001）。
- **Hermes Agent**：MEMORY.md（2200 字符）+ USER.md（1375 字符），共约 800 token 持久记忆，FTS5 关键词检索。
- **OpenClaw**：markdown + SQLite 索引 + 混合检索（70% 向量 + 30% BM25），但内存内容完全依赖模型在一个"静默内部轮次"中自行决定写入什么。
- **Managed Agents (Anthropic)**：追加式事件日志，/mnt/memory/ 文件系统，每个 store 上限 ~100KB，多 Agent 共享但非跨会话个人记忆。
- **Devin**：Knowledge（人工审批触发-内容）+ DeepWiki（参考文档），无自动捕获。

**观点2：内存基准评测是薄弱环节**

LoCoMo 被认为是最差的常用基准：仅 10 个对话对比不可靠、大量问题不需要内存（trivial grep 基线 ~74%）、对抗性问题与目标共享表面相似性，模型靠模式匹配而非记忆取胜。LongMemEval 尚可但仍是回忆中心。MemoryArena 测试"记忆引导行动"的能力，但多数系统不报告。BEAM（ICLR 2026）是唯一为 10M+ token 生产级设计的基准。

**观点3：研究揭示了深层未解问题**

外部记忆并未终结灾难性遗忘——"When Continual Learning Moves to Memory" 显示新旧记忆竞争检索槽位，简单任务向困难任务的前向迁移下降 9.5%。选择性遗忘（去学习过期事实但保留周围结构）未被解决。跨用户记忆污染在正常使用下达到 57-71%，投毒攻击成功率 6-38%。

**观点4：记忆基础设施化的方向**

Mem0 v3 架构：单次 ADD 提取 + 多信号检索（语义 + BM25 + 实体链接）+ 实体链接内置向量库。查询成本约 6900 token、1.44 秒，对比全上下文检索的 ~26000 token、17.12 秒。跨 21 个框架和 20 个向量库的插件生态。
```
### 关键引述

> An agent that's capable within a session but amnesiac across them is fundamentally limited.

> The same gaps recur. Storage is bounded and local. Retrieval is mostly keyword. Memory is harness-scoped. Staleness handling barely exists. Isolation is an afterthought.

> The benchmarks the field uses to measure memory are mostly bad. They test recall of facts from past conversations, they're near-saturated, and a high score doesn't predict better decisions.

### 局限与盲区

- 本文未覆盖：各 Harness 内存方案的实际端到端延迟和 token 成本对比（仅有 Mem0 自身数据）；开源 vs 闭源内存方案的长期维护成本；内存层的可观测性和调试工具（如何排查"为什么 Agent 记得/忘记 X"）。
- 隐含假设：来自 Mem0 官方博客，存在产品推销立场——将各 Harness 的本地内存方案定性为"有缺陷"而外部基础设施为"解决方向"，但外部方案引入了网络延迟、服务依赖、额外成本等新问题；假设"跨 Harness 记忆"是普遍需求，但很多团队实际上只用单一 Harness。
- 可能的反例：对于安全敏感的企业，外部内存服务比本地文件存储引入更大的攻击面；关键词检索在某些场景下反而是更可控的（可预测、可审计），语义检索可能带回不相关但语义相似的干扰信息。

## 关联

- [[Agent记忆升级实录]]
- [[PydanticAgent记忆]]
- [[Memory即Purpose]]
- [[AgentHarness架构]]
- [[Harness工程全景]]
- [[企业级Context Layer架构]]
- 所属项目：[[Agent工程]]
