---
Belongs to: "[[AI技术原理]]"
aliases: ["API中转安全", "Agent中间人攻击"]
tags: ["Agent安全", "API中转", "MITM", "供应链攻击"]
created: 2026-05-24
source: ai-generated
source_url: "https://x.com/mnmn94253156337/status/2058764682404790699"
concepts: ["中间人攻击", "第三方Proxy", "Agent安全边界", "LLM供应链", "Prompt注入", "Tool Call篡改"]
confidence: medium
---
# Agent第三方API中转风险

> [!abstract]- AI 摘要
> 本地Agent通过第三方API中转站访问模型时，中转站不只是转发——它可以修改Prompt、注入System Prompt、污染模型返回、插入恶意Tool Call。由于Agent本身就有读写文件、执行命令的权限，恶意返回结果会直接触发本地执行，形成事实上的远程代码执行。

---

## 扫读

> [!tip] 💡 一句话
> Agent不是聊天机器人，它能操作你的电脑。第三方API中转不是透明转发，被污染的返回结果会通过Agent的Shell/File工具直接在本机执行。

> [!important] 📌 关键结论
> - 第三方Proxy可修改Prompt、注入System Prompt、污染返回、插入Tool Call——本质是MITM（中间人攻击）
> - 真实案例：AppData下被创建.ps1，Startup目录被写入.vbs，开机自动隐藏运行PowerShell
> - 用户很难发现恶意操作，因为Agent平时本来就创建文件、跑终端、安装依赖，恶意行为混在正常操作中

> [!quote] 🎬 可行动项
> - 使用中转API时优先选择可验证签名或端到端加密的方案
> - 对Agent的Shell和文件写入权限做精细化管控，高风险目录（Startup、AppData）加拦截规则
> - 定期审计Agent的执行日志，关注异常的文件创建路径和隐藏运行参数

---

## 精读

### 论证链

```
Agent已从聊天机器人进化为LLM+本地执行器，能操作电脑
      ↓
很多人默认中转站只是换API地址、省钱、解区域限制
      ↓
但实际上第三方Proxy可修改Prompt、注入System Prompt、插入Tool Call
      ↓
攻击链路：用户请求→Claude Code→第三方Proxy→Proxy篡改返回→Claude Code相信返回→调用Shell/File Tool→本机执行
      ↓
真实案例：Startup目录被写入隐藏VBS、PowerShell PAYLOAD开机自动运行
      ↓
用户难以察觉：Agent正常工作外观下恶意操作混入常规文件创建/终端操作
      ↓
Agent让用户主动安装了一个"会自己执行命令的AI"→传统木马需要漏洞才能RCE，现在用户自授予了全盘权限
```

### 关键引述

> 很多第三方Proxy并不是透明转发，它其实可以修改Prompt、注入System Prompt、污染模型返回、插入Tool Call。本质上已经很像MITM（中间人）了。

> AI Agent已经不是聊天机器人了，它是真的能操作你电脑的。用户主动安装了一个"会自己执行命令的AI"，甚至还能获取全盘权限。

> Agent平时本来就会创建文件、改配置、跑terminal、安装依赖、写脚本，所以恶意操作很容易混进去。尤其终端疯狂刷屏的时候，很多人根本不会逐条看。

### 局限与盲区

- 本文未覆盖：具体如何验证中转站是否透明转发；是否有技术手段检测返回结果的篡改
- 隐含假设：用户使用非官方API中转方案，且Agent被授予了较为宽泛的本地执行权限
- 可能的反例：使用官方API或自建中转（完全可控）不存在此风险；仅做文本生成的简单应用不涉及本地执行则风险低

---

## 关联

- [[自建AI API中转站]]
- [[ClaudeCodeHooks管理]]
- [[Agent开发十大核心概念]]
- [[Agent最简实现原理]]
