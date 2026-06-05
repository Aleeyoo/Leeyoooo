---
Belongs to: "[[工具教程]]"
aliases: ["Agent Skill教程", "Claude Skill入门"]
tags: ["Skill", "Claude Code", "Agent", "工具教程", "AI工作流"]
created: 2026-05-24
source: ai-generated
source_url: "https://x.com/ai_xiaomu/status/2058360688293331143"
concepts: ["Agent Skills开放标准", "SKILL.md", "三层渐进式披露", "Progressive Disclosure", "hook式触发", "Skill设计模式"]
confidence: high
---
# Skill小白入门教程

> [!abstract]- AI 摘要
> Skill是一个包含SKILL.md的文件夹，将工作流封装为可复用、可自动触发的AI能力模块。它是Anthropic推出的开放标准，同一份Skill可在Claude Code、Cursor、Codex、Gemini CLI等工具间通用。核心机制是三层渐进式披露（元数据→正文→附录），让十几个Skill并存而不爆context。

---

## 扫读

> [!tip] 💡 一句话
> Skill是把"每次都要教AI的事"封装成一次性工作手册，AI根据description自动判断触发，不绑定任何单一平台。

> [!important] 📌 关键结论
> - Skill本质是Markdown工作手册，三层渐进加载（L1元数据→L2正文→L3参考资料）是它能在十几个Skill并存时仍不爆context的根本原因
> - Description是决定Skill生死的关键字段，必须"WHAT+WHEN"一起写，宁可pushy也不要保守
> - 用"解释为什么"替代"死板的MUST"能让模型举一反三，但输出格式这类机械要求直接写死

> [!quote] 🎬 可行动项
> - 安装skill-creator元Skill，按六步标准流程做出第一个Skill（从新建到能用不超过20分钟）
> - 写作类Skill按拆分成多个协作Skill（大纲/初稿/编辑/SEO/元信息），主Skill显式调用子Skill
> - 复杂hook逻辑遵循"hook收事件+后台跑重活"模式，hook本身必须在一秒内返回

---

## 精读

### 论证链

```
每个AI用户都遇到"教会了→新会话→归零"的重复劳动
      ↓
Skill解决此问题：将"某类事怎么做"封装为SKILL.md文件夹，AI自动判断触发
      ↓
核心架构：三层渐进式披露（L1元数据→L2正文→L3参考资料）→省context、稳品质
      ↓
Description的好坏直接决定Skill被触发的准确率（模糊描述只有55%，pushy风格可大幅提升）
      ↓
五大设计模式（流程链/决策树/模板填充/状态机/多Agent协作），实际中可混用
      ↓
黄金法则：用"为什么"替代"MUST"，让模型理解意图→举一反三；信息不能同时在SKILL.md和references中重复
      ↓
进阶：多Skill拆分协作，主Skill显式调用子Skill；复杂hook必须拆为"hook快收+worker慢做"
      ↓
Skill不仅是效率工具，还重新定义了AI应用的生产方式：零代码门槛、极速验证、可封装为产品出售
```

### 关键引述

> Skill不是设计出来的，是从一次又一次重复劳动里长出来的。

> AI从"你每次都要教它"变成"你教一次就好"。

> 与其用一堆死板的MUST来压模型，不如跟它解释为什么这件事重要。原因让模型举一反三，规则只能覆盖你想得到的情境。

> hook负责接住事件，后台服务负责干重活。两个八竿子打不着的应用，一个做记忆，一个做可视化，架构却长得一模一样。

### 局限与盲区

- 本文未覆盖：Skill在团队共享时的版本管理、冲突解决机制；跨工具兼容性的具体边界case
- 隐含假设：用户具备基本Markdown编辑能力，使用支持Agent Skills标准的工具
- 可能的反例：一次性任务不值得做Skill；粒度过细导致管理成本反超收益；v1过度追求完美反而无法实际使用

---

## 关联

- [[ClaudeCode斜杠命令]]
- [[顶级Skill设计]]
- [[ClaudeCodeHooks管理]]
- [[CLAUDE.md优化规则]]
- [[Agent开发十大核心概念]]
