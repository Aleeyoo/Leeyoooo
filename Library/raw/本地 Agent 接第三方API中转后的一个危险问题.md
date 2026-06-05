---
status: processed
wiki: "[[WIKI/Agent第三方API中转风险]]"
---

# X 上的 撸毛吃猪脚饭：“本地 Agent 接第三方API中转后的一个危险问题” / X

https://x.com/mnmn94253156337/status/2058764682404790699?s=20

很多人现在已经默认：本地Agent + 中转站也是属于日常玩法了。 发现这里其实有个被很多人忽略的风险：

> AI Agent 已经不是“聊天机器人--编码助手”了。

它是真的能操作你电脑 比如这类：

- Claude Code
- OpenCode
- Codex 这些东西本质已经是：LLM + 本地执行器。 一、很多人误以为“中转站只是转发”

最开始我也这么觉得感觉就是： 客户端 ↓ Proxy ↓ Claude/OpenAI 无非就是：

- 换个 API 地址
- 省点钱
- 解区域限制

但后面发现问题没这么简单因为很多第三方 Proxy：并不是透明转发 它其实可以：

- 修改 Prompt
- 注入 System Prompt
- 污染模型返回
- 插入 Tool Call
- 修改 Function Call

本质其实已经很像：MITM（中间人）了。

## 二、最近微信群碰到的一个真实案例

- AppData 下被创建了 .ps1
- Startup 启动目录被写入 .vbs
- 开机自动运行
- PowerShell 隐藏执行

![图像](https://pbs.twimg.com/media/HJIvwvbbYAA9PPX?format=png&name=large)

文件拿到沙箱运行最后变成这样：

![图像](https://pbs.twimg.com/media/HJIv2yUbQAA7n2h?format=png&name=large)

## 三、启动项这里其实才是重点

从图里还能看到：Startup\\我们拥有的信仰.vbs 以及：[WScript.Shell.Run](https://wscript.shell.run/) powershell ... 并且：WindowStyle Hidden也就是隐藏运行

这已经是非常经典的：

- VBS 启动项
- PS1 Payload
- Hidden PowerShell

虽然这里只是弹窗。 但这个行为模式本身：已经和传统脚本木马很接近了四、问题真正出在哪

很多人以为：中转站黑进了电脑其实并不是，真实链路更像：

用户请求 ↓ Claude Code ↓ 第三方Proxy ↓ Proxy修改返回 ↓ Claude Code相信返回 ↓ 调用Shell/File Tool ↓ 本机执行

注意：真正危险的是“返回结果”

而且用户还不容易发现 因为 Agent 平时本来就会：

- 创建文件
- 改配置
- 跑 terminal
- 安装依赖
- 写脚本 所以：恶意操作很容易混进去尤其终端疯狂刷屏的时候，很多人根本不会逐条看。 而且用户看到的是：Claude Code 在正常工作 天然警惕性就会低很多。

## 五、现在感觉 AI Agent 的边界已经有点模糊了

以前木马：

- 要漏洞
- 要RCE
- 要提权 现在：用户主动安装了一个“会自己执行命令的AI” 甚至还能获取全盘权限，这个变化其实挺大的。
- 文章转载 **\-官方论坛**

[www.52pojie.cn](https://www.52pojie.cn/)

**作者论坛账号：Lkkxi**