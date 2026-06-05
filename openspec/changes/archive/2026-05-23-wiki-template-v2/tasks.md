## 1. 模板更新

- [x] 1.1 重写 `Library/temples/WIKI.md`：新增 `concepts`、`confidence` frontmatter 字段，正文重构为扫读区（💡一句话+📌关键结论+🎬可行动项）+ 精读区（论证链+引述+局限/盲区）+ 关联区
- [x] 1.2 更新 `Library/temples/type-project.md`：移除正文中的「关联 WIKI」区块（已就绪，无需修改）

## 2. 规范同步

- [x] 2.1 更新 `openspec/specs/knowledge-pipeline.md`：Step 4 WIKI 生成步骤反映新模板字段（concepts、confidence、三区结构）

## 3. 端到端验证

- [x] 3.1 分析 `Library/raw/为什么接下来 18 个月，将决定你未来 的10 年.md`，确定归属项目（新建或匹配）→ 新建「个人成长」项目
- [x] 3.2 创建项目 `.md`（如需新建）及同名 `.base` 文件 → 创建「个人成长.md」+「Type/个人成长.base」
- [x] 3.3 生成 WIKI 条目到 `WIKI/`，按新模板填充所有字段和三区内容 → 创建「WIKI/18个月信任红利窗口.md」
- [x] 3.4 下载文章中的外部图片到 `Library/assets/`，WIKI 中替换为本地引用，末尾提议用户确认 → 下载 trust-window-timeline.jpg (277KB)
- [x] 3.5 更新项目 `wikis: []` 列表，验证 `.base` 视图可显示新条目 → hasLink("个人成长") ✅
