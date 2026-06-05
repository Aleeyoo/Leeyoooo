## ADDED Requirements

### Requirement: 单 WIKI 多项目归属
WIKI 条目的 `Belongs to` frontmatter SHALL 支持 YAML 数组格式，允许多项目归属。

#### Scenario: 跨领域 WIKI 归属多项目
- **WHEN** WIKI 内容跨越两个明确领域（如 Agent 构建 + 商业策略）
- **THEN** `Belongs to` SHALL 写作 `["[[项目A]]", "[[项目B]]"]`
- **AND** 该 WIKI SHALL 出现在所有归属项目的 `wikis:[]` 列表中
- **AND** 各项目的 `.base` 视图 SHALL 通过 `file.hasLink` 正确过滤出该条目

#### Scenario: 单一领域 WIKI
- **WHEN** WIKI 内容仅属于一个明确领域
- **THEN** `Belongs to` SHALL 写作 `"[[项目名]]"`（单字符串，兼容已有格式）

### Requirement: 跨项目迁移保持链接完整性
当 WIKI 的 `Belongs to` 从 `[[AI商业]]` 改为 `[[Agent工程]]` 或 `[[AI技术原理]]` 时，系统 SHALL 保持：
- WIKI 正文中的 `[[内部链接]]` 不变
- WIKI 的 `tags` 字段不变
- WIKI 的 `concepts` 字段不变
- 仅 `Belongs to` 字段更新

#### Scenario: 迁移 WIKI 到新项目
- **WHEN** 将 WIKI 从 `AI商业` 迁移到 `Agent工程`
- **THEN** 该 WIKI 的 `Belongs to` frontmatter SHALL 更新
- **AND** 该 WIKI 的正文和其余 frontmatter SHALL 不变
- **AND** 该 WIKI SHALL 从原项目 `wikis:[]` 移除并添加到新项目 `wikis:[]`

### Requirement: 迁移验证
迁移完成后，系统 SHALL 验证：
- 原 `AI商业.md` 的 `wikis:[]` 仅包含 ~15 条 AI 商业类条目
- 新 `Agent工程.md` 的 `wikis:[]` 包含 ~31 条条目
- 新 `AI技术原理.md` 的 `wikis:[]` 包含 ~14 条条目
- 全库 WIKI 总数不变（112 条），无遗漏
- 无 WIKI 同时出现在多个项目的 `wikis:[]` 中（本次迁移以主领域归属为准）

#### Scenario: 验证迁移完整性
- **WHEN** 迁移完成后
- **THEN** 系统 SHALL 生成验证报告：各项目条目数、遗漏条目列表（如有）、冲突条目列表（如有）
