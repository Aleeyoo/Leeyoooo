# Leeyoooo 知识仓库

个人知识管理系统，Obsidian 格式。用户只负责往 `Library/raw/` 丢原始素材，AI 驱动全流程。

## 目录

| 路径 | 用途 |
|------|------|
| `Library/raw/` | 原始素材（用户放入） |
| `Library/assets/` | 图片、附件 |
| `Library/temples/` | 全库模板（WIKI.md、type-project.md） |
| `Library/projects/` | 项目 `.md` 主页 |
| `Library/bases/` | 项目专属 `.base` 数据库 |
| `WIKI/` | AI 生成的结构化知识条目 |
| `Type/` | 全局统计视图（五类 `.base`） |

## 核心约定

- **模板**: 所有生成文件必须按 `Library/temples/` 下的模板填充
- **链接**: 统一用 `[[文件名]]`（Obsidian 全局解析），长标题用 `[[长名\|别名]]`
- **命名**: 项目 = 一级领域词，WIKI = 缩减突出核心主题
- **raw 标记**: 处理后加 frontmatter `status: processed` + `wiki: [[WIKI/...]]`

## 知识管线

处理 raw 素材 → 遵循完整工作流：**`Workflow/knowledge-pipeline.md`**
