## 1. 创建新项目文件

- [x] 1.1 创建 `Library/projects/Agent工程.md`，按 `type-project.md` 模板填充（含 aliases: Agent开发/Agent架构）
- [x] 1.2 创建 `Library/bases/Agent工程.base`，过滤条件 `file.hasLink("Agent工程")`
- [x] 1.3 创建 `Library/projects/AI技术原理.md`，按模板填充（含 aliases: AI技术/AI底层）
- [x] 1.4 创建 `Library/bases/AI技术原理.base`，过滤条件 `file.hasLink("AI技术原理")`

## 2. 迁移 WIKI 到 Agent工程

- [x] 2.1 迁移 Harness 系列（6 条）：AgentHarness架构、Harness工程控制论、HarnessScaffold术语、Harness自进化、Prompt到Harness工程、RealEngineer技能组
- [x] 2.2 迁移 ClaudeCode/Skill 系列（4 条）：ClaudeCodeHooks管理、ClaudeSkill本质、CLAUDE.md优化规则、顶级Skill设计
- [x] 2.3 迁移 Pi 系列（3 条）：PiCodingAgent指南、Pi极简Agent哲学、Pi自举构建
- [x] 2.4 迁移 Agent 架构/协作（7 条）：Agent最简实现原理、Agent架构三省六部反思、Agentic设计模式、Agent开发十大核心概念、Agent本质一文讲清、多Agent分工协作、多Agent团队协作、企业级Agent构建指南
- [x] 2.5 迁移 Agent 落地/记忆（5 条）：Agent项目落地难、Agent记忆升级实录、PydanticAgent记忆、长任务Agent工程闭环、AI Agent长程任务评测
- [x] 2.6 迁移 编排/工程实践（9 条）：编排税、编排税即你、世界级Agentic工程师方法论、AI工程团队管理、Agent即服务Aeon自治、提示词工程九原则、提示词许愿陷阱、提示工程师到全栈AI、文科生Agent开发之路、弟子规与Agent规则
- [x] 2.7 验证：Agent工程 wikis 共 36 条

## 3. 迁移 WIKI 到 AI技术原理

- [x] 3.1 迁移底层技术（4 条）：LongCoT思维分子结构、RAG向量检索核心抉择、后摩尔工程方法论、GBRAIN知识引擎
- [x] 3.2 迁移基础设施/安全（3 条）：自建AI API中转站、Agent第三方API中转风险、AI数据质量
- [x] 3.3 迁移协议/范式/趋势（5 条）：Markdown作为AI协议、LLMWiki范式教程、中国开源社区现象、去中心化AI道路、轻量模型量化交易
- [x] 3.4 验证：AI技术原理 wikis 共 12 条

## 4. 精简 AI商业 项目

- [x] 4.1 确认 AI商业 保留条目（12 条）：AI产品赚钱悖论、AI公司转型、DeepSeek十万亿美元战略、FDE企业AI接入、AIFirst组织架构、AI海外市场分析、AI辅助设计方法、AI辅助需求挖掘、AI内容自动化流水线、Agent时代个人网站、AI庇护所技术、人文工作者AI使用指南
- [x] 4.2 清理 AI商业.md 的 `wikis:[]`，改为 12 条
- [x] 4.3 确认无遗漏 WIKI（原 60 条 - 36 Agent工程 - 12 AI技术原理 = 12 AI商业）

## 5. 最终验证

- [x] 5.1 全库 WIKI 总数不变（112 条）
- [x] 5.2 每个 WIKI 有且仅有一个 `Belongs to`（无遗漏、无重复归属）
- [x] 5.3 所有项目 wikis 计数：Agent工程=36, AI技术原理=12, AI商业=12, 个人成长=20, 内容创作=12, 工具教程=11, 商业经营=6, 出海实操=3
- [x] 5.4 所有 `.base` 视图正常过滤对应条目
