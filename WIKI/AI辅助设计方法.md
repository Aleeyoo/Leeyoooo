---
Belongs to: "[[AI商业]]"
aliases: ["AI Design", "AI设计工作流", "Moonchild设计"]
tags: [产品设计, AI工具, 工作流]
created: 2026-05-31
source: ai-generated
source_url: "https://x.com/Av1dlive/article/2060035425574764708"
concepts: ["AI设计工作流", "设计系统驱动开发", "多Agent协作", "产品规格驱动", "设计迭代", "设计词汇提取"]
confidence: high
---
# AI辅助设计方法

> [!abstract]- AI 摘要
> 用AI做产品设计有严格的八阶段工作流：从Brief编写到落地页，核心理念是"功能先于形式"——每个阶段产出markdown文件作为下一阶段的不可变输入。

---

## 扫读

> [!tip] 💡 一句话
> AI辅助设计的正确顺序是Brief→设计词汇→流程→PRD→视觉设计→架构→编码→落地页，每个阶段产出一个markdown文件作为下游的"合同"。

> [!important] 📌 关键结论
> - 设计流程在像素之前：在画任何视觉之前，先定义Brief（为谁、做什么、什么感觉）、提取设计词汇、画出用户流程
> - 设计系统必须活在代码仓库里（markdown文件），而非Figma或Notion中，否则一定会漂移
> - 使用两个Agent分工：Cursor负责构建，Codex负责独立审查，两个Agent的思维模式不同（"想发布"vs"想找bug"），分工显著降低bug率

> [!quote] 🎬 可行动项
> - 建立个人的"设计词汇库"（design-library.md），每发现一种喜欢的审美风格就提取并归档
> - 在项目中创建`principles.md`文件，写下≤3条可在争论中捍卫的设计原则
> - 让模型产出三个不同方向的设计方案而非一个，再手动合成最佳组合

---

## 精读

### 论证链

```
核心原则：设计流程先于像素（Jobs名言：Design is how it works）
      ↓
Phase 1-2：编写Brief（5个问题定义具体用户和单一任务），用非知名参考网站提取设计词汇（避免模型输出"Notion-like"等泛化词）
      ↓
Phase 3：设计用户流程而非屏幕——命名每个流程，发现开放问题（模型没理解的部分），流程决定UI架构
      ↓
Phase 4-5：生成PRD作为"合同"（功能+数据类型+状态变体+明确排除项），产出style.md（颜色/字体/网格/组件/状态）
      ↓
Phase 6-7：先设计架构（状态层、持久化hook、CSS变量主题）再编码，Cursor构建+Codex审查的双Agent模式
      ↓
Phase 8：落地页是独立产品，用同样的工作流但更小的scope
      ↓
核心洞见：模型会给你一千个合理选项，选择哪个是你的判断——工具把其他成本压缩到零，但品味的成本无法压缩
```

### 关键引述

> The pixel is the last thing you decide. Before the pixel is the screen. Before the screen is the state. Before the state is the action. Before the action is the person, and what they were trying to do when they arrived.

> The design system lives in the repo. Not in Figma. Not in a Notion page. In a markdown file checked in next to the code, edited in the same pull requests, committed in the same history.

> Building and reviewing are different mindsets. The building agent wants to ship. The reviewing agent wants to find faults. Put one model in both seats and "ship" usually wins.

> Taste, in the end, is refusing to ship the median. The tools collapsed every other cost. They cannot collapse that one.

### 局限与盲区

- 本文未覆盖：8阶段工作流对非技术背景的纯设计师是否适用（Moonchild需要Cursor配合）；团队协作场景下多个设计师如何共享design-library.md
- 隐含假设：Moonchild能够正确解析所有参考网站并提取一致的词汇；用户有足够的设计判断力在"三个方向"中做出选择
- 可能的反例：对于快速原型验证，完整8阶段流程过于重；某些产品的最佳设计来自直觉而非系统化流程；Figma仍然在团队协作场景有不可替代的实时多人编辑能力

---

## 关联

- [[AI内容自动化流水线]]
- [[人文工作者AI使用指南]]
- [[Agentic设计模式]]
- [[Markdown作为AI协议]]
