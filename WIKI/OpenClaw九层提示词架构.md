---
Belongs to: "[[Agent工程]]"
aliases: ["OpenClaw System Prompt", "九层提示词架构", "OpenClaw架构"]
tags: ["system-prompt", "openclaw", "agent-architecture", "prompt-engineering", "hook-system"]
created: 2026-06-06
source: ai-generated
source_url: "https://x.com/servasyy_ai/article/2029489020208848966"
concepts: ["nine-layer-prompt-architecture", "workspace-files", "bootstrap-hook-system", "inbound-context", "framework-core", "skills-registry", "protocol-specifications", "prompt-assembly-pipeline"]
confidence: high
---
# OpenClaw九层提示词架构

> [!abstract]- AI 摘要
> OpenClaw Agent 发送给 LLM 的完整 System Prompt 不是单一文件，而是 9 层架构的精心编排。Layer 1-6 由框架自动生成保证一致性和稳定性，Layer 7-8 是用户可控层（静态配置文件+Bootstrap Hook 动态注入），Layer 9 是框架自动注入的实时上下文。理解这 9 层的分工才能掌控 Agent 的配置能力。

---

## 扫读

> [!tip] 💡 一句话
> OpenClaw 的 System Prompt 是 9 层分层架构：框架层（L1-L6）保证所有 Agent 基础行为一致，用户可控层（L7 静态文件 + L8 动态 Hook）实现个性化，入站上下文层（L9）注入实时对话状态。用户通过编辑 IDENTITY.md/AGENTS.md 和编写 Bootstrap Hook 脚本控制 Agent 行为。

> [!important] 📌 关键结论
> - 九层架构分层：(L1) Framework Core 框架核心——Agent 身份+运行环境+工具调用规范+安全边界；(L2) Tool Definitions 工具定义——JSON Schema 定义所有可用工具；(L3) Skills Registry 技能注册表——目录扫描自动发现 Skill；(L4) Model Aliases 模型别名——简化模型调用和 A/B 测试；(L5) Protocol Specifications 协议规范——Silent Replies/Heartbeats/Reply Tags 等标准交互协议；(L6) Runtime Info 运行时信息——当前时间/模型/环境/OS/Shell；(L7) Workspace Files 用户静态配置——IDENTITY.md/AGENTS.md/MEMORY.md 等 8 个 Bootstrap 文件；(L8) Bootstrap Hook System 动态注入——4 种 Hook 机制按场景选择；(L9) Inbound Context 入站上下文——消息元信息/发送者/对话历史
> - 用户可控层有 2 层（L7+L8），而非仅 L7：静态文件适合定义 Agent 身份和工作规范；Hook 系统适合动态注入（根据 channel/sender/时间条件注入不同内容）
> - 四种 Hook 机制：(1) agent:bootstrap——完全控制 bootstrapFiles 数组（增删改排序）；(2) bootstrap-extra-files——仅追加文件不修改现有；(3) before_prompt_build——修改最终 prompt 或 prepend context；(4) bootstrapMaxChars——控制字符预算（单文件 20K/总计 150K）
> - 提示词大小对比：L7+L8 是各 Agent 差异的主要来源（~40K），框架层（L1-L6）相对稳定（~10K），L9 入站上下文每次请求注入（~3K）

> [!quote] 🎬 可行动项
> - 审计当前 Agent 的 System Prompt 组成：哪些来自框架不可控？哪些来自用户配置可优化？各层占比多少
> - 精简 L7 静态文件：IDENTITY.md 用表格代替段落，AGENTS.md 用 checklist 代替长段落，删除对框架已知内容的重复描述
> - 选对 Hook 场景：简单追加文件用 bootstrap-extra-files，复杂条件判断用 agent:bootstrap，实时上下文用 before_prompt_build
> - 设置 bootstrapMaxChars 预算——单文件默认 20K/总计 150K，超出部分会按前 70%+后 20% 截断

---

## 精读

### 论证链

```
核心问题：Agent 的 System Prompt 到底是什么、用户能控制什么？
  常见误解：System Prompt 是单一文件或全靠 prompt 编写
  真相：OpenClaw 是 9 层架构的精心编排
        ↓
框架自动生成层（L1-L6）——保证一致性：
  L1 Framework Core：Agent 身份声明 + 工具调用 XML 规范 + 安全边界（禁止 rm -rf、加密敏感信息）
    设计权衡：灵活性 vs 一致性 → 选择框架统一生成，用户不能直接修改核心规则
  L2 Tool Definitions：JSON Schema 定义工具参数，框架验证参数类型 → 灵活性 vs 类型安全
  L3 Skills Registry：自动扫描 ~/skills/ 目录，放入即可用 → 灵活性 vs 维护成本
  L4 Model Aliases：简化模型切换（glm-5 代替 zhipu/glm-5），支持 A/B 测试
  L5 Protocol Specifications：Silent Replies/Heartbeats/Reply Tags 标准交互协议
  L6 Runtime Info：每次请求注入实时状态（时间/模型/环境/路径/OS/Shell），避免模型误判
        ↓
用户可控层（L7-L8）——实现个性化：
  L7 Workspace Files（静态）：IDENTITY.md/TELOS 框架、AGENTS.md/工作规范、MEMORY.md/MemOS 自动导出
    → 设计权衡：框架稳定性 vs 用户自由度 → "变"和"不变"分离
  L8 Bootstrap Hook System（动态）：4 种 Hook 机制对应不同场景
    ① agent:bootstrap：完全控制文件数组（插件/内部模块用）
    ② bootstrap-extra-files：只追加不修改（简单场景首选）
    ③ before_prompt_build：修改最终 prompt 或注入实时上下文（最灵活）
    ④ bootstrapMaxChars：字符预算控制（单文件 20K/总计 150K）
        ↓
框架自动注入层（L9）——注入实时对话上下文：
  L9 Inbound Context：消息元信息 + 发送者信息 + 对话历史 + 是否被 @
    每次请求消耗 ~3KB token，保证对话连贯性和发送者识别
        ↓
组装流程：L1-L6 基础框架 → L7 加载静态文件 → L8 Hook 动态修改 → L9 注入实时上下文 → 完整 System Prompt
```

### 关键引述

> "OpenClaw 的 System Prompt 不是一个单一的文件，而是 9 层架构的精心编排。"

> "框架层统一生成，保证所有 Agent 的基础行为一致。用户可以通过 Layer 7/8 间接实现个性化。"

> "用户可控的层有 2 个（Layer 7 + 8），而不是之前错误说的'只有 Layer 7'。"

> "Layer 7 和 8 是用户可控层，大小因 Agent 配置而异。其他层由框架自动生成，理论上所有 Agent 应该相同。"

### 局限与盲区

- 本文未覆盖：9 层架构在各 LLM Provider（Claude/GPT/Gemini）上的兼容性差异——不同模型对长 System Prompt 的注意力分布和行为响应可能不同；Hook 脚本的错误恢复机制——如果 Hook 脚本崩溃或超时，System Prompt 的降级策略是什么；多 Agent 场景下共享同一套 Bootstrap 文件时的隔离和个性化策略
- 隐含假设：假设 LLM 能有效处理 ~60K+ 的 System Prompt 且不会因过长而注意力衰减——实际上超长 System Prompt 可能导致模型忽略中间或尾部的指令；假设 9 层架构的 token 消耗在可接受范围内——对成本敏感的部署场景可能过高
- 可能的反例：简单 Agent（单一功能无需复杂身份定义）不需要完整的 9 层架构，精简为 3-4 层即可；某些模型（如小参数模型）对分层 System Prompt 的解析能力不足，扁平化 prompt 可能更有效

---

## 关联

- [[AgentHarness架构]] —— System Prompt 分层是 Agent Harness 在上下文管理层的具体实现
- [[Harness工程全景]] —— OpenClaw 的九层架构是 Harness 工程中 progressive disclosure 和 context management 的系统化实践
- [[ClaudeSkill本质]] —— L3 Skills Registry 的目录扫描自动注册与 Claude Code Skill 的设计对应
- [[ClaudeCodeHooks管理]] —— L8 Bootstrap Hook 系统与 Claude Code Hooks 机制的对比和互鉴
- [[Agent开发十大核心概念]] —— System Prompt 分层是 Agent 架构设计的核心概念之一
- [[CLAUDE.md优化规则]] —— L7 Workspace Files（AGENTS.md/IDENTITY.md）的优化实战
