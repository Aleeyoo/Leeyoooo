---
Belongs to: "[[AI技术原理]]"
aliases: ["Karpathy LLM Wiki", "LLM知识库范式", "LLM Wiki工程化教程"]
tags: ["LLM Wiki", "知识管理", "Claude Code Skill", "开源项目文档"]
created: 2026-05-26
source: ai-generated
source_url: "https://x.com/LufzzLiz/article/2058542686551028006"
concepts: ["三层架构(raw/wiki/schema)", "Ingest/Query/Lint三大操作", "知识编译vs解释执行", "双语导航层", "可追溯锚点", "LLM增量维护"]
confidence: high
---
# LLMWiki范式教程

> [!abstract]- AI 摘要
> 基于 Karpathy 75 行 LLM Wiki gist 思想的工程化实践教程，通过三层架构（raw/wiki/schema）和三大核心操作（Ingest/Query/Lint），让 LLM 将任意开源项目源码编译为可读、可查、可信的架构 wiki，结论可追溯到源码文件行号。

---

## 扫读

> [!tip] 💡 一句话
> 过去几十年个人知识库失败的原因都是"维护成本超线性增长"——Karpathy 的方案是让 LLM 来维护，通过 raw/wiki/schema 三层架构将源码编译成交叉引用预建好、矛盾预标注、合成预完成的可信 wiki。

> [!important] 📌 关键结论
> - 三层架构各司其职：raw 是真相的锚（永不修改），wiki 是被编译过的知识（交叉引用/矛盾已标注），schema 是人对 LLM 的教学大纲（命名/标签/创建规则）
> - 三大操作覆盖全生命周期：Ingest（5-15 页拆分原文，强制读全文再决定更新）、Query（综合多页回答，够重自动归档 synthesis）、Lint（9 类问题自动扫描包括矛盾/过时/孤儿页）
> - 硬规则：所有页面结论必须能追溯到源码 `文件:行号` 或原文 URL，做不到宁可不写——这是 wiki 区别于"AI 自由总结"的核心

> [!quote] 🎬 可行动项
> - 五步给任何开源项目建 wiki：填 SCHEMA.md（Domain 描述+Tag Taxonomy）→ 让 LLM 通读源码产首批页 → Lint 全量核对 → 加 guide/ 双语导航层 → 持续 lint 打磨
> - 安装 lanshu-wiki-skill，在 Claude Code 中用自然语言说"添加到 wiki <URL>"即可触发
> - 知识被编译而非每次解释执行——这是 RAG 和 LLM Wiki 的本质区别

---

## 精读

### 论证链

```
传统知识库困境：维护成本随规模超线性增长，人最终放弃
      ↓
RAG的局限：每次查询重新检索合成，知识不积累不复利
      ↓
Karpathy范式：LLM增量维护持久化markdown wiki
三层架构：raw(真相锚)→wiki(编译知识)→schema(教学大纲)
      ↓
三大操作：
Ingest(导入→拆5-15页，强制读全文再决定更新)
Query(综合多页回答，够重自动归档)
Lint(9类问题自动扫描)
      ↓
工程化落地：Claude Code Skill + 五步教程 + Web渲染层
      ↓
规模上限~2000页，token成本$5-30/项目，同概念多命名是头号杀手
      ↓
结论：LLM让知识管理的根本矛盾（维护成本）松动了——人策展提问，LLM负责维护
```

### 关键引述

> 所有页面的结论必须能追溯到源码 `文件:行号` 或原文 URL；做不到，宁可不写。这条规则是 wiki 区别于"AI 自由总结"的核心。

> Bush 当年给后人留了一个未解问题："谁来维护这些关联？"这个问题压垮了之后 80 年所有的个人知识库尝试。Karpathy 给出了答案：让 LLM 来做维护。

> 知识应该编译而非每次解释执行。RAG 每次查询都重新检索、重新合成，知识不积累、不复利。

### 局限与盲区

- 本文未覆盖：多人并发协作的 merge 冲突处理细节；2000 页以上 wiki 的分片维护策略；非代码类知识库（如人文社科）的三层架构适配方案
- 隐含假设：假设用户有 Claude Code 使用经验和 GitHub 基础操作能力，纯非技术用户上手有门槛；假设 LLM 对源码的理解准确度足以支撑"可追溯"的承诺
- 可能的反例：快速迭代的开源项目（周更数百 commits）的 wiki 维护跟不上源码变化；某些复杂架构（分布式系统、编译器）源码层面的"行号追溯"可能不足以真正说清因果关系

---

## 关联

- [[LLM知识库搭建]]
- [[CLAUDE.md优化规则]]
- [[顶级Skill设计]]
- [[Obsidian知识库教程]]
- [[GitHub知识协作底座]]
- [[ClaudeCode斜杠命令]]
