## Context

当前 6 项目分类中，AI商业 承载 60 条 WIKI（全库 112 条的 54%）。其余 5 项目均不超过 20 条。AI商业 内部存在三个语义独立的领域：

- **Agent 工程**（~31 条）：围绕 Agent 系统构建的工程实践——Harness、Skill、Hooks、多Agent 协作、编排、记忆、提示词工程
- **AI 技术原理**（~14 条）：LLM 底层机制与基础设施——RAG、LongCoT、API 中转、安全、协议、开源趋势
- **AI 商业策略**（~15 条）：商业模式、战略、组织转型、产品设计方法

Obsidian 的 `Belongs to` YAML 字段原生支持数组，支持单 WIKI 跨项目归属。

## Goals / Non-Goals

**Goals:**
- 将 AI商业 拆分为 3 个语义独立的项目，每项目 10-35 条，粒度均匀
- 定义清晰的分类规则，使后续新条目归入有据可依
- 保持所有已有 `[[WIKI/xxx]]` 内部链接不变
- 支持跨领域 WIKI 归属多个项目

**Non-Goals:**
- 不修改 WIKI 正文内容（仅改 frontmatter 的 Belongs to）
- 不修改其他 5 个项目结构
- 不引入子分类或嵌套项目层级
- 不迁移非 AI商业 项目的条目

## Decisions

### 项目命名
- **Agent工程**: 面向构建 Agent 系统的工程师。涵盖 Harness、Skill、Hooks、编排、多Agent 协作
- **AI技术原理**: 面向理解 LLM/AI 底层机制的读者。涵盖 RAG、LongCoT、API、安全、协议
- **AI商业**: 保留原名，范围缩小。面向关注 AI 商业化的读者。涵盖战略、赚钱、组织、产品

### 分类规则

```
WIKI 是否涉及 Agent 系统构建（Harness/Skill/Hooks/多Agent/编排/记忆）？
  ├── 是 → Agent工程 （+ 可能跨属其他项目）
  └── 否 → 是否涉及 LLM 底层技术/基础设施（RAG/模型/API/安全/协议）？
              ├── 是 → AI技术原理
              └── 否 → AI商业
```

### 跨项目归属（Belongs to 数组）

部分 WIKI 跨越两个领域。例如：
- `顶级Skill设计` → Agent工程（主要）+ AI商业（Skill 的商业影响）
- `提示词工程九原则` → Agent工程（工程实践）+ AI技术原理（提示词机制）
- `Agent即服务Aeon自治` → Agent工程 + AI商业

决策：本次迁移以**主领域**为准。跨领域归属作为未来迭代，由用户手动添加。

**替代方案 considered**: 为每对边界条目逐个判断多归属 → 复杂度高，引入主观判断，延后处理。

### Migration Strategy

1. 创建新项目文件（`.md` + `.base`）
2. 批量更新 WIKI frontmatter 的 `Belongs to`
3. 分别重建 3 个项目 `wikis:[]` 列表
4. 验证：确认 AI商业 wikis 从 60 缩减至 ~15，新项目各有预期条目数

## Risks / Trade-offs

- **AI商业 命名歧义**: 保留原名但缩小范围后，用户可能习惯性将新 Agent 条目归入 → 通过新项目文件中的 `aliases` 和简介描述降低
- **Pi 系列归属**: Pi 本身是 Agent 工具，其哲学/指南/实践均围绕 Agent 构建 → 归入 Agent工程，可能丢失 Pi 作为“Coding Agent”教程的工具属性 → 通过 aliases 和跨项目链接弥补
- **迁移遗漏**: 手动迁移 60 条 frontmatter 可能遗漏个别条目 → 用 bash 脚本+diff 验证

## Open Questions

- 是否需要为 `Agent工程` 增加子标签（如 `#harness`, `#multi-agent`, `#skill`）？
- `AI技术原理` 中的 `中国开源社区现象`、`去中心化AI道路` 是产业趋势还是技术原理？暂归 AI技术原理，后续可微调
