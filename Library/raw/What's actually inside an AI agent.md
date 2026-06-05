---
status: processed
wiki: "[[WIKI/Agent ReAct三百行实现]]"
---
# What's actually inside an AI agent: a 300~ LoC ReAct loop

2026年5月17日 23:13
https://quantumentangled.dev/viewpost/11/whats-actually-inside-an-ai-agent-a-300-loc-react-loop

I wanted to build [my own simplification of an AI Agent](https://github.com/rulyone/Simple-ReAct-Agent) — to see past the hype, and to figure out what changes when our applications start running one. I had pieces scattered around but never sat down with them.

First, I sketched it in pseudo-code, read a few of the papers that shaped what's now in production, chatted with mainstream agents to fill the gaps, and ended up with this:

![Pseudo Code](https://raw.githubusercontent.com/rulyone/Simple-ReAct-Agent/main/pseudo_code.png)

It's simple, and also a bit error-prone because I'm running a small local model instead of a Frontier one. It answers incorrectly sometimes, and a single bad step poisons the whole chain.

The point I really want to land, though, is how **Actions** can be *anything* — and how risky that is once you stop treating it as a demo. The production headlines we've all seen are exactly like this: git commits with secrets inside, `source.zip` files pushed into npm registries, agents fired off into shells they had no business being in.

An Action is any function call you wire up. If you wire up `shell.exec`, you have wired up `rm -rf` for the model, allowing it to delete your files at will.

The loop also makes something else obvious: the context window matters, because every step re-sends the entire history. Keep it small or pay for it on every iteration.

This is how it typically looks in other implementations:

![](https://quantumentangled.dev/uploads/e5cea3ba-41b9-4d9c-9ca7-4604ed4f0fc3-ctx_wnd.webp)

And even with all the workarounds — prompt caching, `/compaction`, summaries — let's be honest: it isn't in the provider's interest to help you spend fewer tokens. That's how they actually profit from this.

What we as Software Engineers should do is take context management seriously and start building custom agents for our own domains, where **Actions** can be:

- Internal API calls.
- Alerts generation.
- Database lookups.
- Web search.
- Internal process executions.

The point is that once you've written the loop yourself, every "AI assistant" stops being a black box. You start asking the right questions: what's in my context, what can my tools touch, what happens when the model is wrong, how relevant is the Prompt? (hint: very much).