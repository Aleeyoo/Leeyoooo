---
Belongs to: "[[Agent工程]]"
aliases: ["Harness Engineering", "控制论与AI工程", "生成验证不对称"]
tags:
  - AI工程
  - 控制论
  - Agent管理
created: 2026-05-23
source: ai-generated
source_url: "https://x.com/odysseus0z/status/2030416758138634583"
concepts:
  - 控制论循环
  - 传感器与执行器
  - 生成验证不对称性
  - 架构知识外化
  - 反馈回路闭合
  - 校准传感器
  - harness engineering
confidence: high
---

# Harness工程控制论

> [!abstract]- AI 摘要
> OpenAI 的"Harness Engineering"本质上是控制论（Cybernetics）在代码层的重现——工程师不再写代码，而是设计环境、构建反馈回路、编码架构约束。三次历史循环（瓦特离心调速器→Kubernetes→LLM Agent）揭示同一个模式：当传感器和执行器强大到能闭合某一层的反馈回路，人的角色就从操作者变为设计者。

---

## 扫读

> [!tip] 💡 一句话
> Agent 工程不是让 AI 写代码，而是把"什么是好的代码"这个判断从你的大脑外化到系统里——不做这件事，Agent 会在第 1 次和第 100 次犯同样的错误。

> [!important] 📌 关键结论
> - 控制论模式三次重现：瓦特离心调速器（人工调阀门→设计调速器）、Kubernetes（手动重启服务→写期望状态声明）、Harness Engineering（手写代码→设计约束让 Agent 写代码）
> - 代码库长期无法闭合反馈回路是因为缺少"传感器和执行器"来评判架构级别的质量——LLM 同时提供了两者，第一次能让反馈回路闭合在重要决策层
> - 大多数人对 Agent 的抱怨（"它不理解我们的代码库"）诊断错误——不是 Agent 能力不够，而是"什么是好"的知识锁在你的大脑里没有外化

> [!quote] 🎬 可行动项
> - 将团队的架构知识外化为机器可读的文档：架构分层、依赖方向、禁止模式
> - 把 linter 从语法检查升级为带有修复说明的编码规范执行器
> - 建立快速反馈回路：测试、CI 输出、错误信息必须让 Agent 能直接看到并据此调整

---

## 精读

### 论证链

![[harness-cybernetics-cover.jpg]]

```
三次控制论循环闭合：
  ① 1780s 瓦特离心调速器：传感器(飞球测速) + 执行器(调节阀门) → 工人从操作阀变为设计调速器
  ② Kubernetes：传感器(观测实际状态) + 执行器(调副本/重启) → 工程师从重启服务变为写 spec
  ③ Harness Engineering：传感器(LLM判断代码质量) + 执行器(LLM重构/修复) → 工程师从写代码变为设计约束
        ↓
为什么代码库是最后一个被突破的：
  - 低层已有反馈回路：编译器(语法)、测试(行为)、linter(风格)
  - 高层(架构、设计、抽象质量)只有人类能操作——直到 LLM 同时提供了传感器和执行器
        ↓
闭合回路必要但不充分——关键在"校准"：
  - 基础层：测试可跑、CI 输出可解析、错误信息指向修复
  - 困难层：把"什么是好"的判断外化——架构文档、编码原则、团队口味
  - 反面教材：OpenAI 每周五花 20% 时间清理"AI slop"，直到把标准编码进 harness
        ↓
生成-验证不对称性(P vs NP 直觉)：
  生成正确解比验证解更难 → 不需要比机器更会写，需要比机器更会评
        ↓
旧实践的惩罚从不明显变成不可承受：
  跳过文档→Agent 在每个 PR 以机器速度无视约定
  跳过测试→反馈回路无法闭合
  跳过架构约束→漂移速度超过修复速度
```

### 关键引述

> The agent isn't failing because it lacks capability. It's failing because the knowledge it needs — what "good" means for your system, which patterns your architecture rewards, which it avoids — is locked inside your head, and you haven't externalized it. Agents don't learn through osmosis.

> The practices this demands — documentation, automated testing, codified architectural decisions, fast feedback loops — were always correct. Every engineering book written in the last thirty years recommends them. Most people skip them because the cost of skipping was slow and diffuse. Agentic engineering makes the cost extreme.

> The generation-verification asymmetry points to where this goes. Generating a correct solution is harder than verifying one. You don't need to out-implement the machine. You need to out-evaluate it: specify what "correct" looks like, recognize when the output misses, judge whether the direction is right.

### 局限与盲区

- **本文未覆盖**：小团队或 solo 开发者如何平衡"写文档/写测试"与"实际交付速度"的 tradeoff；架构知识外化的具体方法论和工具链；不同 LLM 对相同 harness 约束的遵从度差异
- **隐含假设**：团队有能力和意愿将隐性知识显性化；组织文化支持"先约束后执行"的工作方式；LLM 的能力进步不会让当前的外化方式过时
- **可能的反例**：探索性项目的架构约束频繁变化，过度外化可能增加维护负担；某些判断（如"好的交互体验"）难以用规则编码，仍需人类把关；Kubernetes 的例子中 spec 本身也是人写的，不是所有场景都能找到合适的抽象层

---

## 关联

- [[AI商业]]
- [[Agent最简实现原理]]
- [[Agent架构三省六部反思]]
- [[CLAUDE.md优化规则]]
