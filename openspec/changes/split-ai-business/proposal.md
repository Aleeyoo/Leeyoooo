## Why

AI商业 项目承载 60 条 WIKI（全库 54%），内部混杂三个独立领域：Agent 工程实践、AI 底层技术、AI 商业策略。新条目归入时缺乏清晰边界，查找与浏览效率持续下降。现有 6 项目分类粒度不均——AI商业一家独大，其他 5 项目合计仅 52 条。

## What Changes

- **BREAKING**: 拆分 `AI商业` 项目为 3 个独立项目
- 新建 `Agent工程` 项目：承载 Agent 架构、Harness、Skill、多Agent 协作、编排、记忆等工程实践条目（~31 条）
- 新建 `AI技术原理` 项目：承载 RAG、LongCoT、API 中转、安全、协议、开源趋势等条目（~14 条）
- 保留 `AI商业` 项目（缩小范围）：仅承载商业模型、战略、组织转型、产品设计条目（~15 条）
- 单个 WIKI 可跨多个项目，`Belongs to` 支持 YAML 数组 `["[[项目A]]", "[[项目B]]"]`
- 为两个新项目创建 `.md` 主页 + `.base` 数据库文件
- 迁移受影响 WIKI 的 frontmatter `Belongs to` 字段
- 重建受影响项目的 `wikis:[]` 列表

## Capabilities

### New Capabilities
- `project-classification`: 项目分类规则——Agent工程（构建/管理 Agent 系统）、AI技术原理（LLM/RAG/安全/协议）、AI商业（战略/赚钱/组织）的判定标准
- `wiki-cross-linking`: WIKI 跨项目归属机制——前景条目明确的项目判断流程，跨领域条目的多归属处理

### Modified Capabilities
<!-- No existing specs to modify -->

## Impact

- 新建: `Library/projects/Agent工程.md`, `Library/bases/Agent工程.base`
- 新建: `Library/projects/AI技术原理.md`, `Library/bases/AI技术原理.base`
- 保留: `Library/projects/AI商业.md`（wikis 列表从 60 缩减至 ~15）
- 修改: ~60 条 `WIKI/*.md` 的 `Belongs to` frontmatter
- 无破坏: 所有 `[[WIKI/xxx]]` 内部链接不变，仅 frontmatter 归属字段调整
