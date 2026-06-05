## Why

当前 WIKI 模板只是一个骨架（标题+摘要+要点+内容），缺少 AI 检索索引、可信度标注和对原文的批判性视角。这导致 WIKI 沦为 raw 的镜像复述，而非有价值的知识资产。需要把 WIKI 从"摘要工具"升级为"三层价值载体"：人能扫读、人能深读、AI 能检索。

## What Changes

- **WIKI 模板重构**：新增 `concepts`（AI 检索关键词）、`confidence`（AI 自评可信度）两个 frontmatter 字段；正文重构为扫读区（一句话+关键结论+可行动项）+ 精读区（论证链+引述+局限/盲区）+ 关联区
- **project 模板微调**：移除正文中的关联 WIKI 区块（`.base` 视图已覆盖），保留 `wikis: []` 属性供 AI 维护
- **工作流规范同步更新**：反映新模板结构和字段用法
- **首次跑通**：用 `raw/` 中已有文章端到端验证全流程

## Capabilities

### New Capabilities
- `wiki-template-v2`: WIKI 模板从简单摘要升级三层结构，支持 AI 检索索引与可信度标注

### Modified Capabilities
- `knowledge-pipeline`: 更新 WIKI 生成步骤以匹配新模板字段和结构

## Impact

- `Library/temples/WIKI.md` — 模板重写
- `Library/temples/type-project.md` — 微小调整
- `openspec/specs/knowledge-pipeline.md` — Step 4 和模板描述更新
