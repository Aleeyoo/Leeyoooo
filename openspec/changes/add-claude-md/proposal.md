## Why

`Workflow/knowledge-pipeline.md` 需要 agent 手动读取才能生效，新 agent 不知道仓库约定和目录结构。需要 `CLAUDE.md` 作为每个 session 自动加载的入口，提供即时上下文。

## What Changes

- 新建根目录 `CLAUDE.md`：目录地图、核心约定、指向知识管线的入口（~40行）
- `Workflow/knowledge-pipeline.md` 保留为详细操作文档，由 CLAUDE.md 指引按需读取

## Capabilities

### New Capabilities
- `claude-md`: agent 自动加载的仓库指引文件，提供目录结构、命名规范、模板位置和管线入口

### Modified Capabilities
（无）

## Impact

- 新建 `CLAUDE.md`（根目录）
- `Workflow/knowledge-pipeline.md` 不变
