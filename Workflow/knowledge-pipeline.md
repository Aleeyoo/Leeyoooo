# 知识管线工作流规范

## 系统架构

```
raw/ ──(AI处理)──▶ WIKI/ ──(Belongs to)──▶ project.md ──▶ Projects.base
                      │                            │
                      │ tags: [子分类]              └──▶ 项目.base (统计视图)
                      │
                      └── 可属于多个项目
```

## 目录职责

| 目录 | 职责 | 谁维护 |
|------|------|--------|
| `Library/raw/` | 原始素材，用户放入 | 用户 |
| `Library/assets/` | 图片、附件 | AI |
| `Library/temples/` | 全库模板 | 人工定义，AI 遵守 |
| `Library/projects/` | 项目主页 `.md` | AI 创建，按模板 |
| `Library/bases/` | 项目专属 `.base` | AI 创建 |
| `WIKI/` | AI 整理的结构化知识 | AI |
| `Type/` | 全局统计视图（五类 `.base`） | AI 维护 |

## 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| raw | 用户随意 | `某篇文章.md` |
| WIKI | 缩减长度，突出核心主题 | `18个月信任红利窗口.md` |
| project | 一级领域词，简洁 | `个人成长.md` |
| .base | 与项目同名 | `个人成长.base` |

## 模板

模板文件位于 `Library/temples/`，Agent 生成文件时必须严格遵循对应模板的 frontmatter 和结构。

### type-project.md — 项目模板

```yaml
Belongs to: "[[Projects.base]]"
aliases: []
tags: []
created: {{date}}
status: active
wikis: []
```

必含 `![[{{title}}.base]]` 嵌入，关联统计视图。

### WIKI.md — 知识条目模板

**Frontmatter 字段：**

| 字段 | 说明 |
|------|------|
| `Belongs to` | 所属项目，多项目用 YAML 数组 `["[[项目A]]", "[[项目B]]"]` |
| `aliases` | 中文别名，方便多关键词检索 |
| `tags` | 项目下的子话题标签 |
| `created` | 生成日期 YYYY-MM-DD |
| `source` | 固定为 `ai-generated` |
| `source_url` | raw 原文来源链接 |
| `concepts` | **3-8 个**核心概念词，用于 AI 跨项目语义检索。必须是本文独立讨论的概念，不是泛化描述 |
| `confidence` | AI 自评可信度：`high`（事实/一手资料）、`medium`（有逻辑支撑的观点）、`low`（推测/预测） |

**正文三区结构：**

| 区域 | Callout | 内容 | 目的 |
|------|---------|------|------|
| **扫读区** | `[!tip] 💡` | 一句话核心主张 | 5 秒判断是否值得读 |
| | `[!important] 📌` | ≤3 条关键结论 | |
| | `[!quote] 🎬` | 可行动项 | |
| **精读区** | 论证链 | 观点 → 论据 → 案例 | 深入理解 |
| | 关键引述 | 原文最有价值的段落 | |
| | 局限与盲区 | 未覆盖方面、隐含假设、反例 | 批判视角，**不允许留空** |
| **关联区** | `[[条目]]` | 相关 WIKI / 项目链接 | 知识网状连接 |

## .base 文件格式

Obsidian 原生数据库格式，YAML 结构：

```yaml
views:
  - type: list|cards|table
    name: 视图名称
    filters:
      and:
        - 过滤条件
    groupBy:
      property: 分组字段
      direction: ASC|DESC
    order:
      - 排序字段
```

常用过滤条件：
- `file.ext == "md"` — 文件类型
- `file.hasLink("目标")` — 包含指向目标的链接
- `file.folder == "目录名"` — 所在目录
- `!file.ext.containsAny("md", "canvas")` — 排除特定类型

### 项目 .base 模板

```yaml
views:
  - type: list
    name: WIKI 条目
    filters:
      and:
        - file.hasLink("项目名")
```

## 工作流步骤

### Step 1: 扫描 raw 目录

检测 `Library/raw/` 下未处理的新文件。

### Step 2: 分析内容，确定归属项目

对每篇 raw：

1. 提取主题关键词
2. 检查已有项目（通过 `Type/Projects.base` 的视图或扫描项目 `.md` 文件）
3. 匹配规则：
   - 能匹配到已有项目 → 直接归属
   - 无法匹配 → 用 `type-project.md` 模板创建新项目
   - 内容跨领域 → 可归属到多个项目（生成多篇 WIKI 或使用标签）

### Step 3: 创建/匹配项目（如需）

在 `Library/projects/` 下使用模板 `Library/temples/type-project.md` 创建项目文件，同时在 `Library/bases/` 下创建同名 `.base` 文件。

### Step 4: 生成 WIKI 条目

使用模板 `Library/temples/WIKI.md`，严格遵循三区结构。

**特别注意：**
- `concepts` 必须是 3-8 个本文独立讨论的核心概念，不是泛化描述
- `confidence` 根据来源性质如实标注
- 局限与盲区**不允许留空或写"无"**，必须识别原文未覆盖的方面、隐含假设、可能的反例

### Step 5: 处理图片

1. 检测 raw 内容中的外部图片链接
2. 下载到 `Library/assets/`
3. WIKI 中使用 `![[图片名]]` 引用
4. **在响应末尾提议下载**，让用户确认是否保留

### Step 6: 更新项目 wikis

在项目 `.md` 的 `wikis: []` 中添加新生成的 WIKI 条目文件名。

### Step 7: 标记 raw 文件

在 raw 文件顶部添加 frontmatter 标记处理状态：

```yaml
---
status: processed
wiki: "[[WIKI/对应条目]]"
---
```

`status: processed` 防止 AI 重复处理，`wiki` 建立 raw → WIKI 双向链接。

### Step 8: 汇报

完成后向用户汇报：
- 创建/更新了哪些文件
- 归属了哪个项目
- 标签分类
- 待处理的图片下载请求

## 用户交互

- **自动化**: 分析、归类、生成、链接维护
- **确认点**: 新建项目时告知用户归类决定
- **提议点**: 图片下载在末尾提议
- **用户只需**: 往 `raw/` 丢素材，确认归类

## 快速检查清单

生成 WIKI 条目后逐项验证：

- [ ] frontmatter 字段完整（Belongs to / aliases / tags / created / source / source_url / concepts / confidence）
- [ ] `concepts` 为 3-8 个核心概念词，非泛化描述
- [ ] `confidence` 已根据来源性质如实标注（high / medium / low）
- [ ] 扫读区包含 💡 一句话 + 📌 关键结论 + 🎬 可行动项
- [ ] 精读区包含论证链 + 关键引述 + 局限与盲区
- [ ] 局限与盲区不为空，至少覆盖「未覆盖方面」「隐含假设」两项
- [ ] 关联区有实际链接，非空
- [ ] 项目 `.md` 的 `wikis` 已更新
- [ ] 图片已下载到 `Library/assets/` 并在 WIKI 中用 `![[图片名]]` 引用
- [ ] 新建项目时同步创建了同名 `.base` 文件
