---
Belongs to: "[[Agent工程]]"
aliases: ["Agent Sandbox Architecture", "Control Plane Pattern", "Agent沙箱架构", "零秘密沙箱"]
tags: ["Agent基础设施", "沙箱", "控制平面", "Unikraft", "微VM", "安全隔离", "Browser Use"]
created: 2026-06-06
source: ai-generated
source_url: "https://x.com/larsencc/article/2027225210412470668"
concepts: ["Isolate the Tool vs Isolate the Agent", "Control Plane Proxy Architecture", "Zero-Secret Sandbox", "Unikraft micro-VM", "Bytecode-Only Execution", "Presigned URL File Sync", "LLM History as Platform State", "Gateway Protocol Abstraction"]
confidence: high
---

# Agent沙箱隔离与控制平面

> [!abstract]- AI 摘要
> Browser Use 团队运行数百万 Web Agent 后总结的沙箱架构演进：从 AWS Lambda 起步，经历"隔离工具"和"隔离 Agent"两种模式的对比选择，最终采用控制平面（Control Plane）架构——Agent 完全运行在零秘密的 Unikraft 微 VM 中，所有外部通信（LLM 调用、文件存储、计费）通过控制平面代理完成。核心理念：你的 Agent 应该"没有值得偷的东西，也没有值得保存的状态"。

---

## 扫读

> [!tip] 💡 一句话
> Agent 沙箱架构有两种模式：隔离工具（Agent 在你的基础设施上，危险操作在沙箱中执行）和隔离 Agent（整个 Agent 在沙箱中，通过控制平面代理一切外部通信）——Browser Use 从模式一迁移到模式二，核心理由是"Agent 应该是可丢弃的：无秘密可窃取，无状态需保存，可随时杀死重启"。

> [!important] 📌 关键结论
> - 两种隔离模式的本质差异是信任边界的位置：模式一信任 Agent 代码但隔离其工具调用，模式二将整个 Agent 视为不可信——后者更安全但多一跳网络延迟
> - 控制平面（Control Plane）是模式二的核心：一个无状态 FastAPI 服务，持有所有真实凭证，作为 Agent 与外部世界之间的唯一代理——LLM 调用、文件存储、计费全部经过它
> - 沙箱内 Agent 只接收三个环境变量（SESSION_TOKEN、CONTROL_PLANE_URL、SESSION_ID），读入变量后立即从 os.environ 中删除——即使 Agent 被攻破也没有可窃取的凭证
> - 同一镜像在生产（Unikraft 微 VM <1s 启动、scale-to-zero）和开发/评测（Docker）中运行，通过 sandbox_mode 配置切换——保证环境一致性
> - 会话历史由控制平面在数据库中持有（而非沙箱），使 Agent 成为真正无状态的可丢弃单元——杀死并重建后对话从断点继续

> [!quote] 🎬 可行动项
> - 诊断你的 Agent 系统当前处于哪种模式：Agent 进程自己能访问数据库凭证或 API key 吗？如果是，考虑引入控制平面代理层
> - 实现"零秘密沙箱"：Agent 需要的每个外部服务都通过控制平面代理，凭证只存在于控制平面内存中，沙箱仅持有一个短期会话令牌
> - 采用 Gateway 协议抽象：Agent 代码面向接口编程（AgentGateway Protocol），生产用 ControlPlaneGateway、开发用 DirectGateway，Agent 自身不知道后端差异
> - 确保沙箱镜像在开发、评测和生产环境完全一致——消除"在我机器上能跑"的沙箱行为差异
> - 将会话历史从 Agent 运行时中剥离到平台层（数据库），实现 Agent 的完全无状态化

---

## 精读

### 论证链

```
起点：Browser Use 运行数百万 Web Agent 的经验
  第一代架构：AWS Lambda 上运行仅浏览器 Agent → 每次调用隔离、即时扩展、无秘密担忧
  架构演进动因：增加了代码执行能力（Agent 可编写运行 Python、执行 Shell 命令）
  → 代码执行放进独立沙箱作为工具调用（安全OK，代码在沙箱而非后端）
  BUT：Agent 循环和 REST API 仍在同一后端进程中
    → 重新部署杀死所有运行中的 Agent / 内存密集型 Agent 拖慢 API
    → 两种根本不同的工作负载共享同一进程
        ↓
两种隔离模式：
  模式一（Isolate the Tool）：
    Agent 在你的基础设施上运行
    危险操作（代码执行、终端访问）在独立沙箱中执行
    Agent 通过 HTTP 调用沙箱 → 代码运行在无泄露风险的环境中
  模式二（Isolate the Agent）：
    整个 Agent 在零秘密沙箱中运行
    通过持有所有凭证的控制平面与外部世界通信
    Agent 成为可丢弃的——无秘密、无状态、可随时杀死/重启/独立扩展
  Browser Use 的路径：从模式一到模式二
        ↓
控制平面架构详解：
  定义：无状态 FastAPI 服务，作为 Agent 与外部世界之间的唯一代理
  每次沙箱请求携带 Bearer: {session_token}
  控制平面按令牌查找会话 → 验证活跃状态 → 以真实凭证执行操作
        ↓
控制平面的三类代理职责：
  ① LLM 代理：
    沙箱只发送新消息 → 控制平面在数据库中持有完整对话历史
    → 每次调用时重建完整上下文转发给 LLM 提供商
    → 沙箱无状态：杀死后重建，对话从断点继续
  ② 文件同步（预签名 URL）：
    沙箱监控 /workspace 中的文件变更
    → POST /presigned-urls 请求上传 URL（控制平面生成 scope 限定到会话的 S3 预签名 URL）
    → 沙箱直接用预签名 URL 上传到 S3
    → 下载同理反向操作
    → 沙箱从未持有 AWS 凭证，只拿到临时限定 URL
  ③ 计费与成本上限：控制平面在每次 LLM 调用时执行成本限制和计费，沙箱只关注任务
        ↓
沙箱层安全加固：
  ① Bytecode-Only 执行：Docker 构建时编译所有 Python 源码为 .pyc → 删除所有 .py 文件
     Agent 框架代码作为 root 加载到内存 → 源码加载后消失
  ② 权限降级：入口点以 root 启动（读取 root 拥有的字节码）→ 立即 setuid/setgid 到 sandbox 用户
  ③ 环境变量剥离：三个变量读入 Python 变量后从 os.environ 删除
     → Agent 检查环境变量时只剩空列表
     → Token 在沙箱所属 VPC 外本就无效（VM 私有 VPC，仅允许与控制平面通信）
        ↓
生产/开发一致性：
  同一容器镜像在三种环境运行：
    生产：Unikraft 微 VM（<1s 启动、scale-to-zero、跨多 metro 分布避免单点瓶颈）
    开发/评测：Docker 容器（相同镜像、相同入口点、相同控制平面协议）
  通过 sandbox_mode: 'docker' | 'ukc' 配置切换
        ↓
Gateway 协议抽象：
  Agent 代码面向 AgentGateway Protocol 接口编程：
    invoke_llm() / persist_messages()
  生产实现：ControlPlaneGateway（HTTP → 控制平面）
  开发/评测实现：DirectGateway（直接调 LLM，历史存内存）
  → Agent 代码不感知后端差异——同一接口，不同后端，统一行为
        ↓
扩展性：每个层次按自身瓶颈独立扩展
  控制平面：无状态 → 加实例 + 负载均衡 → 基于 CPU 利用率自动扩展（ECS Fargate）
  沙箱：每个会话独立 VM → Unikraft 跨 metro 调度
  成本：闲置 VM 自动挂起（scale-to-zero），查询间隙几乎零成本
        ↓
核心理念：你的 Agent 应该没有值得偷的东西，也没有值得保存的状态
  取舍：每次操作额外一跳网络延迟 + 三个服务而非一个
  实际影响：延迟对 LLM 响应时间而言是噪音级，运维复杂度是运维团队已掌握的范畴
```

### 关键引述

> "Your agent should have nothing worth stealing and nothing worth preserving."

> "The agent becomes disposable. No secrets to steal, no state to preserve, you can kill it, restart it, scale it independently. The control plane holds the truth."

> "The control plane is stateless: validate the token, do the work, return the result. Need more agents? Spin up more sandboxes. Need more throughput? Add control plane instances behind a load balancer. Each layer scales based on its own bottleneck."

> "The tradeoff is an extra network hop on every operation and three services to deploy instead of one. In practice the latency is noise compared to LLM response times."

### 局限与盲区

- **本文未覆盖**：控制平面成为单点故障的风险——虽然无状态可水平扩展，但令牌验证依赖的会话数据库仍然是状态存储的集中依赖点，其故障会导致所有 Agent 无法工作。预签名 URL 文件同步模式下大文件（数 GB）的上传性能和可靠性。多 Agent 共享同一会话历史的场景——如果分支 Agent 各自产生不同对话路径，控制平面如何合并或选择。
- **隐含假设**：假设 Agent 的所有外部通信都可以通过 HTTP 代理完成——对于需要持久 WebSocket 连接或流式协议（如实时浏览器自动化中的 CDP）的场景，控制平面代理模式可能引入协议转换复杂度。假设 Unikraft 微 VM 的 scale-to-zero 恢复延迟对用户可接受——对于需要即时响应的同步交互场景，冷启动延迟可能影响用户体验。假设所有 LLM 提供商的 API 都可以通过统一代理层抽象——不同提供商的流式响应、工具调用格式、token 计数方式差异可能迫使控制平面引入提供商特化逻辑。
- **可能的反例**：对于低安全需求的内部工具 Agent（如仅访问公开 API 的数据分析 Agent），三服务架构（Agent 沙箱 + 控制平面 + 文件同步）是过度设计——直接在 Agent 进程中持有只读凭证可能更经济。对于需要极低延迟的实时交互 Agent（如语音助手 <200ms 响应 SLA），额外的网络跳在 LLM 延迟之上叠加可能突破延迟预算。Bytecode-only 执行增加了调试难度——线上 Agent 行为异常时无法直接查看源码，需要反向工程 .pyc。
- **生态限定**：Unikraft 微 VM 的选择依赖特定云提供商关系（Unikraft Cloud on AWS bare metal），对使用 GCP/Azure 或其他平台的团队不可直接复制。Docker 容器在本地开发中运行良好，但安全加固措施（字节码编译、权限降级）可能不适用于所有 Agent 框架的语言生态（如 Node.js/TypeScript 的 Agent 框架）。

---

## 关联

- [[云端Agent沙箱基础设施]] —— CREAO 团队的云端 Agent 基础设施实践，与本文共享"零凭证沙箱 + 外部控制层"的核心理念，但 CREAO 侧重慢变/快变代码分离，本文侧重两种隔离模式的权衡
- [[AgentHarness架构]] —— Agent 运行时底座设计，控制平面是该文 Harness 在云端的工程实现
- [[Harness工程全景]] —— "模型是推理引擎，Harness 是环境"的理念在本文中体现为"沙箱是 Agent 的身体，控制平面是 Agent 的感官和记忆"
- [[企业级Agent构建指南]] —— 企业 Agent 的上下文隔离原则与本文沙箱隔离逻辑一致
- [[Agent构建21个惨痛教训]] —— 第 17 条"成本追踪"和第 19 条"不要信任 Agent 的输出"与本文的控制平面计费和沙箱零信任设计对应
- 所属项目：[[Agent工程]]
