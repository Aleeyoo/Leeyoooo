---
Belongs to: "[[Agent工程]]"
aliases: ["Cloud Agent Infrastructure", "Agent Sandbox Design", "云端Agent基础设施", "Agent沙箱架构"]
tags: ["Agent基础设施", "沙箱", "云端Agent", "安全", "CREAO", "热更新", "凭证隔离"]
created: 2026-06-06
source: ai-generated
source_url: "https://x.com/intuitiveml/article/2062699747224568212"
concepts: ["Cloud vs Desktop信任边界", "沙箱快照冻结", "原子热替换", "所有权变更节奏分离", "零凭证沙箱", "短期JWT按运行签发", "API Bridge模式", "单执行管线统一调度"]
confidence: high
---

# 云端Agent沙箱基础设施

> [!abstract]- AI 摘要
> CREAO 团队从零构建云端 Agent 基础设施的实战经验：桌面 Agent 框架假设一个用户、一台机器、一个可信边界，而云端 Agent 运行在共享硬件上、被陌生人触发、执行对抗性代码。两条核心教训——分离慢变状态与快变代码（原子热替换 Runner）、将凭证永远挡在沙箱外（API Bridge + 短期 JWT），解决了云端 Agent 的可靠性、安全性和多触发面统一调度问题。

---

## 扫读

> [!tip] 💡 一句话
> 桌面 Agent 框架给你的每项免费保证（持久化、身份、网络安全、重试），在云端都必须重建为显式系统——核心原则是冻结用户状态、热更新平台代码、凭证永生不入沙箱、一条执行管线服务所有调用者。

> [!important] 📌 关键结论
> - 桌面与云端的根本差异不是"多了几台服务器"，而是信任边界的彻底翻转：桌面 Agent 作为用户进程运行，云端 Agent 作为受隔离的不可信代码运行在共享硬件上
> - 用户环境（慢变）与平台 Runner（快变）的耦合是核心矛盾——冻结整个沙箱快照保证了可复现性，但每次平台部署都会触发"保留冻结环境还是更新 Runner"的二选一困境
> - 解决方案借鉴操作系统：内核热更新不影响用户主目录。将沙箱启动流程拆为两步——从用户快照启动（不动用户环境），然后原子热替换 Runner（约 300ms），更新失败则回滚重建
> - 凭证隔离的两层防线：IP 白名单（仅沙箱宿主内网可访问 API Bridge）+ 按运行签发的短期 JWT（scope 限定用户/应用/会话，随运行过期），即使沙箱被攻破攻击者也只能继承一个随运行死亡的有限令牌
> - 统一执行管线：同一套 executeAgent 函数处理 UI 点击、定时调度和 API 调用三种触发源——新增触发面只是路由变更而非架构变更

> [!quote] 🎬 可行动项
> - 审计你的 Agent 系统：凭证是否曾出现在沙箱环境变量或文件中？如果是，引入 API Bridge 模式迁移至 host-side 凭证注入
> - 区分你的系统中"用户控制的慢变部分"和"平台控制的快变部分"——如果它们当前耦合在同一镜像中，按所有权边界拆分为独立层次
> - 为 Runner 部署实现原子热替换：staging → 验证 → 原子 swap → 清除缓存，确保没有"半升级"状态进入运行
> - 统一触发面：检查你的 Agent 执行路径是否对人工触发、定时调度、API 调用使用了不同的代码路径——合并为单一 executeAgent 入口
> - 为多触发面（调度、API、手动）设计统一的计费和可观测性管道，确保新增触发面时不需要重建这些系统

---

## 精读

### 论证链

```
问题定义：桌面 Agent 框架的隐含假设在云端全部失效
  桌面假设：单用户、单机、单进程、笔记本电脑开着、本地文件系统、
           API key 在环境变量、终端关闭 Agent 死亡、出问题用户重试、
           pip install 直接装到用户 Python
  云端现实：沙箱冷启动、共享硬件、触发者未知（定时/HTTP/另一Agent）、
           用户可能在睡觉、沙箱内代码可能对抗性、文件系统需跨部署存活、
           凭证不能和 Agent 同处、每个保证都需重建为显式系统
        ↓
Lesson 1: 分离慢变（用户环境）与快变（平台 Runner）
  问题来源：桌面中用户环境和 Agent 运行时是同一物、同一更新节奏、同一控制者
  云端的矛盾：
    用户环境需要冻结（上周的 pip install 今天解析到不同版本 → 冻结快照保证可复现）
    平台 Runner 需要高频部署（每天多次）
    但两者打包在同一镜像中 → 每次部署触发二选一：保留冻结环境（用旧 Runner）还是更新 Runner（丢弃用户环境）
  初始方案（粗暴）：启动时检查版本 → 不匹配就丢弃快照重新从模板启动
    → 问题：9:00 AM 的 cron job 不应因为 8:55 的部署丢失环境
  最终方案（借鉴 OS 内核更新）：
    ① 从用户冻结快照启动（不动用户环境）
    ② 新 Runner 写入临时目录
    ③ node --check 验证语法
    ④ 原子 swap：解锁旧 Runner → 复制新 Runner → chattr +i 重新锁定 → 隐藏 chattr 二进制
    ⑤ 清除 V8 编译缓存（确保新代码加载）
    ⑥ 任何步骤失败 → 杀死沙箱重试 → 不存在"半升级"状态运行 Agent
    ⑦ 成功运行后重新快照，将新 Runner 固化到用户镜像中
  核心诊断问题：任何持久化构件，问"谁控制这个构件的变更节奏？"——如果用户和平台共同拥有，迟早要还耦合债
        ↓
Lesson 2: 凭证永驻沙箱外
  桌面信任模型：Agent 以用户身份运行、用用户密钥、在用户机器上、对用户网络
  云端安全模型：假设沙箱内代码已被攻破（对抗性 prompt → LLM 生成的代码 → 任意行为）
  铁律：沙箱内永远不存放任何长生命周期凭证
  实现——API Bridge 模式：
    沙箱内 Agent 需要调用认证服务 → 发送本地 HTTP 请求到沙箱外的 API Bridge
    → Bridge 在 host 侧附加 OAuth token → 转发调用 → 响应回到沙箱但 token 从未进入沙箱
  两层防线：
    ① IP 白名单：Bridge 仅接受沙箱宿主内网 IP → 开发机/泄露 URL/公网请求在网络层被丢弃
    ② 短期 JWT：沙箱启动时平台签发 scope = (用户, 应用, 会话) + 运行窗口有效期
       沙箱每次调用携带 JWT → Bridge 验签+检查过期 → 解析用户存储凭证附加
       沙箱被攻破 → 攻击者只能拿到随运行死亡、scope 受限的令牌，没有主凭证可窃取
  该 Bridge 同时负责计费扣减、日志、指标的外传 → 唯一跨越沙箱边界的接口
        ↓
底层模式：四条属性不需例外
  ① 状态驻留沙箱，用户不改则永久冻结
  ② 代码可热替换，独立于状态
  ③ 凭证驻留 host 侧，永不进入 Agent
  ④ 一条执行管线服务所有调用者（人/调度器/另一软件）
  底层哲学：Agent 是一个具有自然语言接口的函数——实现归用户，触发面/运行时/安全边界归平台
```

### 关键引述

> "Most agent frameworks today assume a desktop. One user, one machine, one process. The agent runs while the laptop is open, writes to a local filesystem, holds API keys in environment variables, and dies when the terminal closes. Cloud agent infrastructure has none of those luxuries."

> "The rule we hold is simple. No long-lived credential ever lives inside the sandbox."

> "An agent on a laptop is bound to the laptop. An agent in the cloud is a function the rest of your stack can call. The user writes it once. The platform makes it survive deployments, run safely on shared hardware, and accept callers the user never anticipated."

> "For anything you persist in a cloud platform, ask: who controls the cadence of change on this artifact? If the user and the platform both own it, you will eventually pay for the coupling. Split the artifact along the ownership boundary and let each side update on its own clock."

> "One executeAgent function handles UI clicks, scheduled runs, and API calls. The billing system, the credit deduction logs, the observability signals — all identical regardless of whether a human clicked Run, a cron fired, or a script called the API."

### 局限与盲区

- **本文未覆盖**：API Bridge 模式在高频调用场景下的性能开销（每请求多一跳网络往返 + JWT 验签）；沙箱快照的存储成本（每个用户保留一份冻结镜像的规模效应和冷启动延迟）；多租户沙箱间的隔离强度等级（进程级、容器级、VM 级，不同等级的安全/成本平衡点）。平台 Runner 热更新成功但 Agent 自身代码不兼容新 Runner 的场景如何处理（例如 Runner API 变更导致用户自定义脚本报错）。
- **隐含假设**：假设用户的 Agent 行为可以通过短期 JWT 的 scope 完全限定——但 scope 只能限制 API 访问范围，无法限制 Agent 在沙箱内部的恶意行为（挖矿、DDoS 攻击、数据外泄）。假设用户接受"环境冻结直到手动编辑"的模式——对于需要实时数据或动态依赖的场景（如股票分析），冻结快照可能导致运行时数据过期。假设沙箱内代码的对抗性行为仅限于尝试窃取凭证——未讨论代码层面的其他攻击面（如通过 Bridge 的 side-channel 攻击、滥用合法 API 进行数据拖取）。
- **可能的反例**：对于内部团队使用的 Agent 平台（非公开多租户、所有代码来自受信开发者），零凭证沙箱的安全投入可能是过度设计——简单的环境变量注入已足够。对于无状态 Agent（如纯推理、不需文件系统持久化），快照冻结机制增加了不必要的复杂度。原子热替换的 300ms 开销在超低延迟场景（如实时代理响应 <100ms SLA）中可能不可接受。
- **生态限定**：文章基于 CREAO 的平台架构，对自建云端 Agent 基础设施的团队有参考价值，但对于选择托管平台（如 Anthropic API、OpenAI Agents SDK）的用户，这些基础设施层已被平台封装。Runner 热替换方案的 chattr +i 细节是 Linux 特化实现，不适用于 Windows 或 Serverless（无文件系统持久化）环境。

---

## 关联

- [[Harness工程全景]] —— 云端沙箱是 Agent Harness 在云基础设施层的延伸，"仓库即 system of record"的思路与本文化"状态驻留沙箱"异曲同工
- [[企业级Agent构建指南]] —— 企业 Agent 的上下文隔离、子 Agent 独立上下文窗口与本文沙箱隔离逻辑一致
- [[AgentHarness架构]] —— Agent 运行时底座设计，本文提供了云端的 Harness 运行时实现模式
- [[编排税]] —— 云端 Agent 的多触发面统一管线降低了"每增加一种触发方式就增加一套系统"的编排税
- 所属项目：[[Agent工程]]
