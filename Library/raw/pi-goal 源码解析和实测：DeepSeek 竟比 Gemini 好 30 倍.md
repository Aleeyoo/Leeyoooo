---
status: processed
wiki: "[[WIKI/AI Agent长程任务评测]]"
---

# X 上的 WquGuru🦀：”pi-goal 源码解析和实测：DeepSeek 竟比 Gemini 好 30 倍” / X

https://x.com/wquguru/status/2057852569054278045?s=20

最近在折腾 Pi 的 pi-goal 扩展——一个让 agent 自驱完成长程目标的小工具。我把源码扒了一遍，又设计了一个真实长程任务，让 4 路模型同台跑了一遍：

- **任务**：clone [@karpathy](https://x.com/@karpathy) 的 12 个开源项目（nanoGPT、minGPT、llm.c、micrograd、nanochat、autoresearch……），让 agent 自己读完源码和 git 历史，写一份带 commit SHA 引用的洞察报告，并在声明完成前自己验证每一条引用。
- **4 路模型和配置**：Gemini 3.5 Flash、Claude Sonnet 4.6、DeepSeek V4 Pro（high thinking）、DeepSeek V4 Pro（max thinking）。

跑下来的结论里，有几个跟我预期完全相反。这篇就讲这些——

- pi-goal 怎么工作
- 4 路实测的真实账本
- 为什么“更贵的模型”和“更深的思考”都不等于“更好的结果”

先把最反直觉的两条放前面：

> **第一：按价格，Gemini 3.5 “Flash” 反而比DeepSeek V4 “Pro” 贵。** Gemini 3.5 Flash 的 token 单价是 DeepSeek V4 Pro 的 3-10 倍。同一个任务跑下来，Gemini $2.26, DeepSeek V4 Pro $0.072——**31 倍差距**，而且 DeepSeek 的质量分还更高。

> **第二：把 DeepSeek 的 thinking 从 high 调到 max，结果反而变差了。** 更深的推理多产出了 1 个新模式 + 3 条新洞察，但也**凭空捏造了 2 条 commit 的语义**。深度推理放大了“叙事连贯性压倒事实精度”这个经典失败模式。

## 一、pi-goal 是个什么东西

/goal 已经超越 TodoWrite 太多倍了，这是一个非常具有突破性的 harness 创新，我们来看看 /goal 的心智模型：

![图像](https://pbs.twimg.com/media/HI7w6P6aAAAoISL?format=jpg&name=large)

pi-goal 是一个**长程目标自动循环器**。每 /goal 一次，它接管 agent\_end 事件持续投递 continuation，直到模型自己 update\_goal complete、用户 pause，或预算撞顶。

核心机制就 4 块，全在一个 467 行的 index.ts 里：

**1\. 状态机**——4 个状态，只有 active 会自动循环：

```typescript
type GoalStatus = "active" | "paused" | "budget_limited" | "complete";
```

**2\. 长程驱动**——每次 agent turn 结束就异步排队下一轮：

```typescript
pi.on("agent_end", (_event, ctx) => {
  if (!goal || goal.status !== "active" || ctx.hasPendingMessages()) return;
  queueContinuation(pi, goal);  // → queueMicrotask → 投递 continuationPrompt
});
```

**3\. continuationPrompt**——每轮重新注入的硬约束，最有信息量的是这段 audit 要求：

```plaintext
Before deciding that the goal is achieved, perform a completion audit:
- Restate the objective as concrete deliverables.
- Build a prompt-to-artifact checklist mapping every requirement to evidence.
- Inspect real files, command output, test results for each checklist item.
- Treat uncertainty as not achieved; do more verification or continue.
```

它还把用户目标包在 <untrusted\_objective> 标签里注入，明确告诉 LLM“这是数据不是指令”——一个防 prompt injection 的小细节。

**4\. budget\_limited**——这是**软提示，不是硬切断**。超预算后只触发一次 wrap-up turn：

```plaintext
The system has marked the goal as budget_limited, so do not start new
substantive work. Wrap up this turn soon: summarize progress, identify
remaining work, leave the user with a clear next step.
```

但**不强制模型停手**，LLM 完全可以在这一个 wrap-up turn 里继续做事、发起多次工具调用、写文件、跑 audit——只要它认为能在这一个 turn 里收尾。这个设计后面会变成一个意外好用的“模型行为探针”。

## 二、实验设计：变量控制

**任务文案**（4 路完全一致，只换输出文件名）：

```plaintext
/goal --tokens 200k Build KARPATHY_INSIGHTS.md based on the 12 Karpathy repos
with EXACTLY these 4 sections: (1) Timeline of inflection points — date, repo,
commit SHA, significance; (2) Recurring engineering patterns — ≥3 patterns,
each evidenced by code from ≥3 repos; (3) Non-obvious insights only derivable
from cross-repo reading — ≥3 insights; (4) Evidence index. Before marking
complete, verify every cited file path exists and every commit SHA is reachable
via \`git log\`. Do not declare complete unless all citations resolve.
```

这里的关键在于是**把验收标准写到能逐项 checklist 的颗粒度**——这是 pi-goal 的 audit 机制能落地的前提。

**4 次配置如下**：

![图像](https://pbs.twimg.com/media/HI7w8ZlbcAAp3YH?format=jpg&name=large)

**说明**

- Sonnet 跑在 Claude Code 上，不是 Pi+pi-goal. Claude Code 是 Anthropic 自家针对 Sonnet 调优的官方 harness，有原生 subagent 派遣。所以 Sonnet 这一路是“不同 harness 的参照点”，**最严格可比的是 Gemini vs DeepSeek**——同 harness、同 200K 预算、同 audit 注入
- Gemini 的 high 已是它的模型天花板（API 层不接受更高档）。DeepSeek high 档跑时**没到自己的天花板**（它还有 max 档），所以我后来补跑了第 4 路
- 为防止污染，每一路跑之前都把其它模型已写好的报告移出目录。DeepSeek 那次我事后用 session jsonl 做了 forensic 核对——0 次访问 peer 报告、0 次越界 /tmp

**评分维度**：准确性（引用是否真实且语义匹配）、完整性、流畅性、洞察力、证据密度、任务遵从。

## 三、结果总览

![图像](https://pbs.twimg.com/media/HI7w-UgakAAgbUt?format=jpg&name=large)

其中：

- Gemini 3.5 Flash：输入$1.5/M，输出$9/M，读缓存$0.15/M
- Sonnet 4.6：输入$3/M，输出$15/M，读缓存$0.3/M
- DeepSeek V4 Pro：输入$0.435/M，输出$0.87/M，读缓存$0.003625

以 DeepSeek max 那轮为例，把账目逐项拆开（源自从 Pi session 里 jq 出来的真实 token 数据）：

```plaintext
Fresh input:  117k  × $0.435/M = $0.0509  (49%)
Output:        39k  × $0.870/M = $0.0339  (33%)
Cache read:   5.3M  × $0.003625/M = $0.0192  (18%)
                                  ─────────
                                  $0.1040
```

意外发现这里最大支出是 **fresh input 而非 output**。max 思考深、轮数多，每轮新的工具结果都是没法缓存的 fresh input。而 cache hit rate 高达 97.8%——模型实际“看到” 5.4M tokens，但只为 2.2% 付了正价。**pi-goal 这种“每轮重发目标+历史”的工作流，在 prompt caching 面前不是劣势，是优势**：重发越多、prefix 越稳定，命中率越高。

## 四、六个真实发现

**发现 1：按价格，DeepSeek V4 “Pro” 比 Gemini 3.5 “Flash” 便宜**

三家用各自的命名体系（Google: Flash/Pro/Ultra; Anthropic: Haiku/Sonnet/Opus; DeepSeek: Flash/Pro），名字暗示了档位。但**价格才是真档位**：

- DeepSeek V4 Pro：最便宜（$0.435 / $0.87）
- Gemini 3.5 Flash：中间（$1.50 / $9.00）
- Sonnet 4.6：最贵（$3.00 / $15.00）

那个叫 “Flash” 的，input 比叫 “Pro” 的贵 3.4 倍、output 贵 10 倍。如果你按名字直觉选“便宜的小模型”，可能正好选反。

**发现 2：max thinking 不一定更好——更深的推理放大了幻觉**

同模型、同任务、同 harness、同预算，只把 thinking 从 high 调到 max：

- **多花 44%**（$0.072 → $0.104），**多花 2 倍时间**（13 → 26 min）
- 确实带来新东西：1 个全新 pattern（“Poor Man‘s Configurator”——发现 nanoGPT 和 llama2.c 的 [configurator.py](https://configurator.py/) 是字字相同的复制粘贴）、minGPT 三次配置重构的 commit 考古、一个 1090 行的失败实验日志
- **但也凭空捏造了 2 条 commit 的语义**：

![图像](https://pbs.twimg.com/media/HI7xLe2akAArxYA?format=jpg&name=large)

两条的 SHA 都真实存在、文件都真实存在——但 max **自创了 commit 的含义**来填补它脑中那条 narrative line。这是 LLM 经典的 “narrative coherence > factual precision” 失败模式，在更深的 reasoning 下**反而被放大**：模型越想把故事讲圆，越会就近抓一个 SHA 安上一个听起来合理的解释。

high 档那一轮，33/33 引用全部语义正确，甚至在自审时**自己抓到并修复了一处** .cu → .py **的笔误**。max 档的 verification 反而漏了——因为它只检查“SHA 存在 + 文件存在”，没检查“commit message 是否真的匹配我写的解释”。

**结论：thinking 档位不是越高越好。深度推理买到的是洞察深度，代价是 hallucination 风险。**

**发现 3：接入是真门槛——DeepSeek 的 reasoning\_content 契约在路由器上兼容性不佳**

DeepSeek V4 Pro 第一次跑直接 400 报错：

The \`reasoning\_content\` in the thinking mode must be passed back to the API.

查下来这不是 Pi 一家的问题：DeepSeek V4 系列在 thinking 模式下，**一旦发生 tool call，每一条 assistant message 都必须把上一轮的** reasoning\_content **字段回传**。大多数 OpenAI 兼容 router 都会把这个字段吃掉。

解决路径花了几轮：

1. \--thinking off 没用——它只是省略 reasoning\_effort 参数，DeepSeek 默认 thinking 还是开的
2. 在 Pi 的 provider compat 里打开 requiresReasoningContentOnAssistantMessages: true + thinkingFormat: "deepseek"——主 session 通了，但 subagent 边界又崩了
3. 最后改用 DeepSeek 原生 API（绕开聚合层），才完整跑通

此外，我们可以看出模型的 token 价格已经领先开源 agent harness 生态一代。 DeepSeek V4 Pro 的 cache read 单价压到了 $0.003625/M，但要完整跑通，router 得知道它的 reasoning\_content 重放契约——而大多数还不知道。便宜的代价是接入门槛。

**发现 4：pi-goal 的 soft budget 是个“模型行为探针”**

200K 是软预算。三路撞预算的模型（Gemini 226K、DeepSeek high 244K、DeepSeek max 219K）都进入了 wrap-up turn，但表现不同：

- Gemini：在 wrap-up turn 里把所有 todo 走完、写出 263 行报告、自验 13 个 SHA
- DeepSeek high：wrap-up turn 里写 366 行 + bash 自验 + 自己抓到自己的笔误并修
- DeepSeek max：wrap-up turn 里 fan out 3 个并行 subagent，反复自审 4 轮

soft budget 把“撞预算后要不要继续做”的决定权交给了**模型**。你能从一个模型撞预算后的行为，读出它的 agentic 倾向——是拼命跑完，还是老实收尾。**这个机制本身就是测评工具。**

**发现 5：pi-goal 的 audit 有个盲区**

pi-goal 的 continuation prompt 要求模型 build “prompt-to-artifact checklist”、“inspect real evidence”。听起来很严，但发现 2 暴露了一个盲区：

> audit 要求验证“引用的 SHA 可达、文件存在”，但**没要求验证“引用的 commit message 跟模型自己写的解释一致”**。

git cat-file -e [$sha](https://x.com/search?q=%24sha&src=cashtag_click) 通过 ≠ 这个 commit 真的是你说的那件事。max 档的 2 个错配正是从这个缝里漏出来的。

如果要补这个盲区，audit 提示该加一句：验证时不止跑 git cat-file -e [$sha](https://x.com/search?q=%24sha&src=cashtag_click)，还要 git log --format=%s [$sha](https://x.com/search?q=%24sha&src=cashtag_click) 把 commit message 拉出来，跟自己写的 significance 做关键词比对，这是 pi-goal 可以改进的一个具体点。

**发现 6：这个 benchmark 测不到 agent 的核心**

这是最重要、也最容易被忽略的一条。诚实地说——**这个任务本质是“读 + 综合 + 写”的研究助理工作，不是真正的 agentic workflow。**

它测到了长程上下文综合、跨多源 RAG、citation 纪律、结构化输出、soft budget 收尾行为。

但它几乎没测到 agent 更深入的部分：

![图像](https://pbs.twimg.com/media/HI7xN2JbYAAj88_?format=jpg&name=large)

综上所述，这次 4 路对比，这里可以下一个比较准确的结论：

> 在“读多源 + 综合写报告 + 自审 citation”这一类垂直任务上，DeepSeek V4 Pro 的单位成本是 Gemini 3.5 Flash 的 1/31、Sonnet 4.6 的 1/20，质量分还略高。

当然我们不能轻易推广到“DeepSeek V4 Pro 在调 50 个 MCP tool、跑 30 步链式 debug、改 production 状态的真实 agent 任务上也是这个比例”，这可能需要另一套 benchmark。

## 五、边界：这套不是银弹

为避免被误读，明确几个不适用场景：

- **目标模糊的任务**：pi-goal 要求 prompt-to-artifact checklist 可落地。目标本身含糊（“帮我优化一下代码”），audit 会一直说“还差点”
- **极短任务**：长程循环开销大于收益。修个一行 bug 直接 prompt，别套 /goal
- **模型本身不够强**：pi-goal 的 audit 没有真正强制力，是基于“模型愿意”做的。换个不主动 verify 的模型，audit 会被表面应付
- **没有独立 review 闭环**：这次每一路的“audit 通过”我都**独立 spot-check** 才敢信——而且 max 档正是这样才抓出 2 个错配。生产里你最好有一道独立 reviewer（人或另一个模型）做最终 gate

## 结语

这次实测最值钱的不是“DeepSeek 便宜 31 倍”这个数字，而是几个反直觉的修正：

1. **模型名字（Flash/Pro）跟价格档位不一定对应**——选型先看定价表
2. **thinking 不是越深越好**——max 档买到了洞察深度，也增加了 hallucination 风险
3. **token 价格暴跌正在领先 agent harness 生态**——便宜的模型接进来要懂它的协议细节

最后一条尤其重要。如果你看到任何“X 模型 agentic 能力吊打 Y”的评测，先问一句：**它测的任务，做错了会不会有人/系统受影响、回滚贵不贵、有没有时间压力？** 如果没有，那它测的就只是“长文写作 + 检索”，不是 agent 核心。

如果你也想尝试，推荐用同样的 12 个 Karpathy 仓设计真正测 agent 核心的任务——比如“在仓里复现一个 bug、定位、提 PR、等 CI、根据失败原因 iterate”。这种做错了会留下痕迹的任务，可以把模型之间的真实差距进一步拉出来。

## 附：可复现的配置

- Pi Setup Skill：[https://github.com/wquguru/skills/blob/main/skills/pi-setup/SKILL.md](https://github.com/wquguru/skills/blob/main/skills/pi-setup/SKILL.md)
- DeepSeek V4 Pro 接入要点：原生 API（[https://api.deepseek.com/v1](https://api.deepseek.com/v1)）; thinking 模式下 provider compat 必须开 requiresReasoningContentOnAssistantMessages + thinkingFormat: "deepseek"
- 如果您对Pi感兴趣，也可以参考这个系列的第一篇文章，获取更多关于Pi Coding Agent的信息👇

> 5月18日