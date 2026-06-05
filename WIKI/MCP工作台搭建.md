---
Belongs to: "[[工具教程]]"
aliases: ["MCP教程", "Model Context Protocol入门", "Claude Code MCP配置"]
tags: ["MCP", "Claude Code", "Agent工具", "协议", "工具教程"]
created: 2026-06-06
source: ai-generated
source_url: "https://x.com/ai_xiaomu/article/2059202018682831129"
concepts: ["MCP协议", "Host-Client-Server三角色", "三原语Tools/Resources/Prompts", "USB-C协议抽象层", "MCP与Skill协同", "MCP Registry元数据目录", "stdio传输"]
confidence: high
---
# MCP工作台搭建

> [!abstract]- AI 摘要
> MCP（Model Context Protocol）是让 AI 真正拥有"手脚"的开放协议——类比 USB-C 接口标准化了 AI 应用与外部工具的连接方式。掌握 7 个基础 MCP Server 的安装、自建最小 MCP Server、以及上架公开 Registry 的完整流程，AI 工作台从"知道怎么做"升级为"真的能做"。

---

## 扫读

> [!tip] 💡 一句话
> Skill 让 AI 知道做事的章法，MCP 让 AI 真的有手有脚——两者协同构成完整的 AI 工作台：Claude Code 是大脑，Obsidian 是记忆，Skill 是章法，MCP 是手脚。

> [!important] 📌 关键结论
> - MCP 不是新模型、不是新框架，是和 HTTP/USB 同级的**协议层抽象**——解决"AI 应用 x 工具 = NxM 组合爆炸"的工程问题，降为 N+M
> - MCP 三原语中 Tools（AI 主动调用的动作）覆盖 90% 场景，Resources（只读数据）和 Prompts（提示词模板）是高阶玩法，新手不用纠结
> - MCP 不替代传统 API，而是在传统 API 上面多包一层翻译层：GitHub MCP Server = 上面接 MCP 协议（给 AI）+ 下面调 GitHub REST API
> - 自建 MCP Server 的最小骨架仅 25 行 Python：定义函数 → @mcp.tool() 装饰 → mcp.run()，FastMCP 自动完成协议转换、schema 生成、JSON-RPC 通信

> [!quote] 🎬 可行动项
> - 装 7 个基础 MCP：Sequential Thinking / Filesystem / Playwright / Context7 / Chrome MCP / Firecrawl / GitHub，覆盖 80% 日常 AI 编程场景
> - 用 "claude mcp add <名字> -s user -- <命令>" 统一管理，记住 -s user 全局生效
> - 自己写一个 25 行 Python 计算器 MCP Server 搞懂机制，再用 MCP Inspector (npx @modelcontextprotocol/inspector) 调试

---

## 精读

### 论证链

```
起点：前文 Skill 教程已搭建 SOP → 但 Skill 不会让 AI 多长一只手
  Skill = 让 AI 知道做事章法（SOP）
  MCP  = 让 AI 真的有手有脚（工具执行能力）
        ↓
一、MCP 本质：USB-C 类比
  没有 MCP 之前：N 个 AI 应用 × M 个工具 = N×M 个对接 → 组合爆炸
  有了 MCP 之后：AI 实现"USB-C 母口" + 工具实现"USB-C 公口" = N+M
  MCP 是和 HTTP/USB 同级的协议层抽象
        ↓
三个角色：
  Host = 你用的 AI 应用（Claude Code/Claude Desktop/Cursor）→ 大脑
  Client = Host 内部的协议通信组件 → 看不见但一直在工作
  Server = 真正提供能力的进程（stdio 本机 / HTTP+SSE 远程）→ 你插的移动硬盘
        ↓
三种原语：
  Tools（工具）= AI 主动调用的动作 → 覆盖 90% 场景
  Resources（资源）= AI 主动读的数据 → 只读，像 URL
  Prompts（提示词模板）= 可复用提示词片段 → 用得最少
        ↓
MCP vs 传统 API：不是替代，是抽象层
  API 该长什么样还长什么样 → MCP 包一层"AI 用工具的标准语言"
  GitHub MCP Server = 翻译器：上接 MCP 协议，下调 GitHub REST API
        ↓
二、7 个基础 MCP Server 安装
  关键技术栈：
    Sequential Thinking → 推理脚手架
    Filesystem → 读写硬盘（明确授权目录）
    Playwright → 浏览器自动化（测网页/抓内容）
    Context7 → 拉最新库文档（打破模型训练截止日期）
    Chrome MCP → 接管已登录 Chrome（自带 cookie/登录态）
    Firecrawl → AI 优化的网页爬虫
    GitHub → 读写仓库（issue/PR/commit）
  安装统一命令：claude mcp add <名字> -s user -- <启动命令>
  新手易踩坑：Claude Code 是命令行优先，不是丢 JSON 配置
        ↓
三、自建最小 MCP Server（25 行 Python）
  核心流程：
    FastMCP("Server名") → 创建实例
    @mcp.tool() → 装饰普通函数 → 转为 MCP Tool（参数类型注解+docstring自动转schema）
    mcp.run() → stdio 传输启动
  调试：npx @modelcontextprotocol/inspector python server.py → 网页面板验证 Tool
  注册：claude mcp add calculator -s user -- python /绝对路径/server.py
  效果：25 行 Python = 给云端大模型接了一只它原本没有的手
        ↓
四、上架 MCP Registry（5 步）
  前提认知：
    ① Registry 只存元数据不存代码 → npm/PyPI 还是代码的家
    ② 名字用反向 DNS 防抢注 → io.github.用户名/server-name
    ③ 当前 preview 阶段，命令可能变
  5 步流程：
    1. 代码发 npm（package.json 加 mcpName 字段）
    2. 装 mcp-publisher CLI（brew install mcp-publisher）
    3. 生成 server.json（mcp-publisher init）
    4. GitHub 设备码登录（mcp-publisher login github）
    5. 上架+验证（mcp-publisher publish → curl Registry API 搜自己）
  三大坑：mcpName 拼错 / JWT 过期 / 命名空间与登录身份不匹配
        ↓
五、进阶方向
  夯实基础：官方文档 modelcontextprotocol.io + 微软 mcp-for-beginners（11模块6语言）
  生产环境：远程部署 HTTP+SSE / Server OAuth 鉴权 / 企业域名命名空间 DNS 认证
        ↓
全链路收束：Claude Code（大脑）+ Obsidian（记忆）+ Skill（章法）+ MCP（手脚）
  = 真正能干活的 AI 工作台
```

### 关键引述

> "Think of MCP like a USB-C port for AI applications." —— Anthropic 和微软给 MCP 的定义出奇一致

> Skill 本身不是锤子，MCP 才是。

> MCP 不是新模型、不是新框架，是一个协议——和 HTTP、和 USB 一个层级的东西。

> 你写的是 25 行 Python，但你做的事是：给一个跑在云端的大模型，接了一只它原本没有的手。

> Claude Code 是大脑，Obsidian 是它的记忆，Skill 是它的章法，MCP 是它的手脚。四件齐了，你手里就有一个真正能干活的 AI 工作台。

### 局限与盲区

- 本文未覆盖：各 MCP Server 之间的权限冲突和资源竞争问题；MCP 在企业环境（内网、防火墙、VPN）下的网络配置细节；MCP Server 的并发和多用户场景下的隔离策略；MCP Registry 上架后的版本管理和废弃（deprecation）机制
- 隐含假设：假设读者已安装 Claude Code 并能正常使用，网络条件支持 npm/npx 拉包（中国用户可能需要设置镜像）；假设 AI 模型能正确判断何时调用哪个 MCP Tool——实际中工具选择本身是出错点
- 可能的反例：Chrome MCP 的安装流程复杂（扩展+bridge+SSE 传输），新手可能中途放弃；自建 MCP Server 的 Calculator 例子过于简单，生产级 MCP（如带鉴权、错误处理、超时控制）需要大量额外工程；MCP 对纯写作/创意类工作流（不涉及外部工具调用）的增益有限

---

## 关联

- [[Skill小白入门教程]]
- [[ClaudeCode斜杠命令]]
- [[ClaudeSkill本质]]
- [[Agent Skill替代工具软件]]
- [[Agent最简实现原理]]
- [[Agent开发十大核心概念]]
- [[GitHub完整教程]]
