## Context

当前 `Workflow/knowledge-pipeline.md` 是仓库唯一的操作文档，但 agent 不会自动读取它。Claude Code 支持 `CLAUDE.md` 文件，每个 session 自动加载到上下文。

## Goals / Non-Goals

**Goals:**
- CLAUDE.md 在 ~40 行内覆盖 agent 所需的全部即时上下文
- 包含目录地图、核心约定、管线入口指引
- 不影响 `Workflow/knowledge-pipeline.md` 的完整性

**Non-Goals:**
- 不在 CLAUDE.md 中重复完整工作流步骤
- 不修改现有文件放置逻辑

## Decisions

### CLAUDE.md 内容边界

| 包含 | 不包含 |
|------|--------|
| 仓库身份说明 | 完整 Step 1-8 流程 |
| 目录结构表 | .base 格式语法 |
| 命名规范 | 过滤条件列表 |
| 模板位置 | 快速检查清单 |
| 管线文档引用 | 用户交互规范 |

### 管线入口策略

CLAUDE.md 指向 `Workflow/knowledge-pipeline.md`，agent 在需要跑管线时按路径读取。

## Risks / Trade-offs

- ~40 行在每次 session 中占用上下文，但相比无指引带来的理解成本，可接受
- 未来目录结构变化需同步更新 CLAUDE.md → 自然维护
