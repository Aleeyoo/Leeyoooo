---
Belongs to: "[[Agent工程]]"
aliases: ["Agentic Engineer", "Agent工程最佳实践", "世界级Agent工程师"]
tags: ["Agent", "Claude Code", "Codex", "工程方法论", "上下文管理", "Agent架构"]
created: 2026-05-24
source: ai-generated
source_url: "https://x.com/systematicls/status/2028814227004395561"
concepts:
  - "上下文精简"
  - "研究-实现分离"
  - "逢迎偏差利用"
  - "契约式任务终点"
  - "规则与技能分层"
  - "全新会话每契约"
  - "极简Agent哲学"
confidence: high
---
# 世界级Agentic工程师方法论

> [!abstract]- AI 摘要
> systematicls 分享成为世界级 Agentic 工程师的核心原则：极简设置、上下文控制、利用逢迎偏差的对弈式Agent设计、规则与技能的迭代管理，以及用任务契约取代长程会话的自动化策略。

---

## 扫读

> [!tip] 💡 一句话
> 成为世界级 Agentic 工程师的秘诀不是安装更多工具，而是极致控制Agent的上下文——只注入刚好完成任务所需的信息，并用规则和技能作为IF-ELSE目录迭代优化。

> [!important] 📌 关键结论
> - 前沿公司会将真正有用的Agent方案内建到产品中（skills、memory、subagents等都是先被社区验证再被官方吸收），所以不要过度依赖外部工具
> - 上下文是一切：上下文膨胀是Agent表现恶化的首要原因，研究（research）和实现（implementation）必须分离为不同会话以保持上下文纯净
> - Agent的"逢迎偏差"既是陷阱也是武器：用对弈式多Agent设计（发现者→对抗者→裁判）可以将逢迎偏差转化为高保真输出

> [!quote] 🎬 可行动项
> - 将 CLAUDE.md 精简为逻辑化的IF-ELSE目录，只包含"在什么场景读什么规则/技能"，不要塞入具体指令
> - 为每个任务创建{TASK}_CONTRACT.md（包含测试、截图验证等终点条件），用停止钩子防止Agent提前终止
> - 当一个任务需要多项子任务时，为每份契约创建独立会话——不要用24小时长程会话

---

## 精读

### 论证链

```
核心诊断：大多数人无法最大化Agent能力的原因
      ↓
误因1：缺少正确的工具/包/harness
误因2：CLAUDE.md不够长
误因3：没有读到足够多的方法论文章
      ↓
真正原因：上下文膨胀 + 过度依赖外部依赖 + 不了解Agent设计约束
      ↓
原则1：极简即最优（Less is More）
  ① 每一代新模型都会改变最优实践 → 过早工具锁定 = 技术债
  ② 前沿公司员工（无限token + 最新模型）是最积极的Agent用户
  ③ 真正有用的方案会被官方产品吸收（skills/memory/subagents/planning）
  ④ → 不需要安装任何外部依赖做最好的工作
      ↓
原则2：上下文是一切（Context Is Everything）
  ① 上下文膨胀 = Agent被无关信息淹没 → 表现恶化
  ② 研究与实现必须分离：
    - 模糊指令"build an auth system" → Agent需研究所有方案 → 上下文污染
    - 精确指令"implement JWT + bcrypt-12 + refresh token 7d" → 上下文纯净
  ③ 当需要Agent决定实现方案时：研究会话→决策→全新上下文实现会话
      ↓
原则3：利用逢迎偏差（Sycophancy）
  ① Agent天然倾向于满足你的要求（即使需要编造）
  ② "Find me a bug" → Agent一定会找到bug（即使不存在）
  ③ 解决方案：中性提示词（"review all components, report findings"）
  ④ 进阶用法：对弈式多Agent设计
    - 发现者Agent：+1/+5/+10 积分激励 → 找到所有"可能的bug"（超集）
    - 对抗者Agent：推翻bug得积分/推翻错误倒扣 → 筛掉假bug（子集）
    - 裁判Agent：告知持有标准答案 → 客观判定 → 接近完美精度
      ↓
原则4：明确任务终点（How To End The Task）
  ① Agent知道如何开始但不知道如何结束
  ② 测试作为确定性里程碑：除非X个测试全部通过→任务未完成
  ③ 截图+验证作为新终点：实现→截图→设计/行为验证→未通过继续迭代
  ④ TASK_CONTRACT.md：整合测试+截图+其他验证条件
      ↓
原则5：长程自动化无需24小时会话
  ① 长程会话 = 必然的上下文膨胀
  ② 正确方式：每份契约一个全新会话 → 编排层管理契约队列
      ↓
迭代引擎：规则(Rules) + 技能(Skills)
  ① Rules：编码偏好（不要做X → 在CLAUDE.md中引用coding-rules.md）
  ② Skills：编码做法（怎么做X → 先让Agent研究写法→写成skill→迭代）
  ③ CLAUDE.md 只是一份IF-ELSE目录（场景→读取→执行）
  ④ 随着规则和技能增多 → 互相矛盾/上下文膨胀 → 让Agent自行"清理日"整合优化
      ↓
结论：拥有结果（Own The Outcome）——Agent不完美，但规则+技能+上下文控制的组合让你能把AI当未来的玩具严肃地使用
```

### 关键引述

> You don't need the latest agentic harnesses, you don't need to install a million packages and you absolutely do not need to feel the need to read a million things to stay competitive. In fact, your enthusiasm is likely doing more harm than good.

> Context is everything. You want to give your agents only the exact amount of information they need to do their tasks and nothing more! The better you are in control of this, the better your agents will perform.

> If something truly is ground-breaking and extended agentic use-cases in a meaningful way, it will be incorporated into the base products of the foundation companies in due time. So relax, you don't need to install anything or use any other dependencies to do your best work.

> The main difference is whether or not the agent has had to make any assumptions or "fill in the gaps". As of today, they are still atrocious at "connecting the dots".

> That's really the secret. Keep it simple, use rules and skills and CLAUDE.md as a directory and be religiously mindful about their context and their design limitations.

### 局限与盲区

- 本文未覆盖：具体项目类型的规则/技能模板示例；多人团队中Agent协作的权限模型；安全性/合规性方面的Agent使用约束；非CLI环境（IDE插件、Web界面）的Agent使用差异
- 隐含假设：读者已经是熟练开发者，有较强的技术判断力；使用的是Claude Code或Codex CLI（方法论对其他Agent工具有效但未验证）；任务可以清晰描述为契约（开放性探索任务可能不适用）
- 可能的反例：对初学者来说，现成的harness和模板可能是更快的入门路径（虽然作者认为这不利于长期成长）；某些特定领域可能有专门工具显著优于基础CLI（作者未覆盖所有垂直场景）；对弈式多Agent设计增加token成本，小任务可能得不偿失

---

## 关联

- [[Agent最简实现原理]]
- [[Harness工程控制论]]
- [[多Agent团队协作]]
- [[CLAUDE.md优化规则]]
- [[Pi极简Agent哲学]]
- [[Agent架构三省六部反思]]
- [[顶级Skill设计]]
- [[多Agent分工协作]]
- [[企业级Agent构建指南]]
- [[AgentHarness架构]]
- [[Agent长程任务评测]]
