## ADDED Requirements

### Requirement: 项目分类规则
系统 SHALL 将全库项目分为 8 个一级领域（原 6 个 + Agent工程 + AI技术原理）。AI商业 项目拆分为 3 个独立项目，其余项目不变。

#### Scenario: 新 WIKI 归属 Agent工程
- **WHEN** 新 raw 素材内容涉及 Agent 系统构建（Harness、Skill、Hooks、多Agent 协作、编排机制、Agent 记忆、提示词工程、Agent 架构模式）
- **THEN** 该 WIKI 的 `Belongs to` SHALL 指向 `[[Agent工程]]`

#### Scenario: 新 WIKI 归属 AI技术原理
- **WHEN** 新 raw 素材内容涉及 LLM 底层技术或基础设施（RAG、向量检索、模型推理机制、API 中转、Agent 安全威胁、数据质量、AI 协议、开源生态趋势）
- **AND** 内容不以 Agent 构建为主要视角
- **THEN** 该 WIKI 的 `Belongs to` SHALL 指向 `[[AI技术原理]]`

#### Scenario: 新 WIKI 归属 AI商业
- **WHEN** 新 raw 素材内容涉及 AI 商业模式、战略、赚钱方法、组织转型、产品设计、市场化
- **AND** 内容不满足 Agent工程 或 AI技术原理 的条件
- **THEN** 该 WIKI 的 `Belongs to` SHALL 指向 `[[AI商业]]`

### Requirement: 项目分类优先级
当 WIKI 内容同时符合多个分类条件时，系统 SHALL 按以下优先级判定：Agent工程 > AI技术原理 > AI商业。

#### Scenario: 内容同时涉及 Agent 构建和商业策略
- **WHEN** WIKI 同时涉及 Agent 构建实践和商业策略（如 Agent 产品的商业模式）
- **THEN** `Belongs to` SHALL 优先指向 `[[Agent工程]]`，可选附加 `[[AI商业]]`

#### Scenario: 内容同时涉及 LLM 技术和商业应用
- **WHEN** WIKI 同时涉及 LLM 技术原理（如 RAG）和商业应用场景
- **THEN** `Belongs to` SHALL 优先指向 `[[AI技术原理]]`，可选附加 `[[AI商业]]`

### Requirement: 项目文件完整性
每个项目 SHALL 包含：
- `Library/projects/项目名.md`：项目主页，模板见 `Library/temples/type-project.md`
- `Library/bases/项目名.base`：项目专属数据库视图，过滤条件为 `file.hasLink("项目名")`

#### Scenario: 新建项目
- **WHEN** 创建新项目时
- **THEN** 系统 SHALL 同步创建 `.md` 和 `.base` 文件
- **AND** `.base` 文件 SHALL 包含标准的 `views` 配置（list 视图，按 `file.hasLink` 过滤）
