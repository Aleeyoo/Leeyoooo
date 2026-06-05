---
Belongs to: "[[工具教程]]"
aliases: ["AI视频GitHub仓库", "lanshu-awesome-ai-video-kit", "AI视频全品类仓库"]
tags: ["AI视频", "开源工具", "Claude Skill", "方法论SOP"]
created: 2026-05-26
source: ai-generated
source_url: "https://x.com/LufzzLiz/article/2059052200425619791"
concepts: ["模型专属Prompt公式", "跨模型对照矩阵", "自动监控巡检", "Claude Code Skill", "source追溯机制", "方法论SOP体系"]
confidence: high
---
# 企业AI视频工具包

> [!abstract]- AI 摘要
> 作者构建了一个覆盖 15 个 AI 视频模型、含 411 条 prompt、7 个 Claude Code Skill 和 14 篇方法论 SOP 的开源工具包仓库，核心创新在于用 source 追溯、自动监控和各家官方公式解决 AI 视频 prompt 领域的三大顽疾。

---

## 扫读

> [!tip] 💡 一句话
> 把市面上主流 AI 视频模型的官方写法、实测样本、Claude Code Skill 和方法论 SOP 整合成一个可 git clone 的工具包，用 source 追溯杀来源不明、自动监控杀过期失效、各家官方公式杀混杂厨房。

> [!important] 📌 关键结论
> - 每家 AI 视频模型的 prompt 写法完全不同（Sora 用分层 Shot List，Veo 用 8 元素双标记，Kling 用 5 层进阶，Seedance 用 8 要素结构化），不应捏成通用模板
> - 32 个监控端点每周一自动巡检 SHA-256 hash，模型更新自动开 issue，解决"人记不住版本迭代"的痛点
> - 7 个 Claude Code Skill 覆盖模型选择、prompt 翻译和单模型写作，其中 model-selector 和 prompt-translator 是跨模型核心 skill

> [!quote] 🎬 可行动项
> - git clone 仓库后一行命令安装全部 skill，接到视频制作需求时先用 model-selector 选模型，再用对应 prompter 生成 prompt
> - 将 110 条跨模型对照矩阵作为选模型直觉的训练材料，同场景 11 家写法对比能快速建立判断力

---

## 精读

### 论证链

```
AI视频prompt领域三大顽疾
      ↓
来源不明 → 每条prompt加source字段，链到官方文档/论文/博客
      ↓
过期失效 → GitHub Action每周一自动监控32个官方端点hash变化
      ↓
混杂厨房 → 每家模型按各自官方公式收录，不捏成通用模板
      ↓
证据：①411条prompt分301条单模型+110条跨模型对照矩阵
       ②7个Skill按信息密度拆分(Seedance独做3个因53页PDF够厚)
       ③14篇SOP按主题分四组，02进阶8要素+13六大速查为必读
       ④Web工具层零依赖单文件HTML，三维筛选+URL状态可分享
      ↓
结论：一套可复用的企业级AI视频生产工具链，从选模型到写prompt到排错都有据可查
```

### 关键引述

> 市面上"AI 视频 prompt 大全"类内容很多，常见的 3 个问题：来源不明——不标谁写的、在哪个模型上测的、成功率如何。过期失效——大量 2024 年的写法在 2026 的模型上跑不出来。混杂厨房——一条 prompt 既不是 Sora 的 Shot List 也不是 Kling 的 5 层公式，只是"看起来高级"。

> 每条 prompt 必须有 source 字段。链到原始博客、官方文档、论文。不能写"我编的"。

### 局限与盲区

- 本文未覆盖：社区实测视频贡献机制还停留在"预留字段"阶段，跨模型对照矩阵的实际视频对照效果尚未验证；工具包依赖 GitHub 生态，非技术用户直接使用有门槛
- 隐含假设：假设各家模型的官方公式是最佳实践，但官方文档本身可能为了降低门槛而简化，进阶玩法未必覆盖
- 可能的反例：模型选型在某些场景下受预算、合规、本地部署需求限制，远不止 prompt 公式对比这么简单

---

## 关联

- [[AI短视频生成实战]]
- [[顶级Skill设计]]
- [[ClaudeCode斜杠命令]]
- [[RAG向量检索核心抉择]]
