---
status: processed
wiki: "[[WIKI/Agent构建21个惨痛教训]]"
---

# 21 painful mistakes I made building AI agents so you don't have to

These are all my lessons from 3 months of building Hermes and OpenClaw agents every day, and the exact traps I fell into so you don't waste countless credits and hours on the learning curve.

---

## 1. Don't build one giant agent

Build a crew of specialized agents with clear ownership. One bloated agent becomes harder to debug, route, and trust. Singular agents with specific tasks are scalable and easier to manage.

[This is an example](https://x.com/gkisokay/status/2043594876881969386) of how I structure multiple agents to execute one agentic workflow:

---

## 2. Don't build an agent that focuses on output first

Build a research agent first. It becomes the input intelligence layer that feeds every other agent. Everything it gathers from the world becomes your training data to refine your builds over time.

Here's my guide to building your own research agent.

---

## 3. Don't confuse scraping with research

Raw links, feeds, and summaries are not enough. Agents need structured, verified, source-backed information they can act on.

[This is an example of how my research agent routes new context to my other agents.](https://x.com/gkisokay/status/2051294697579249809)

---

## 4. Don't let research die in a doc

The research agent should route findings into workflows: coding, content, marketing, competitive intel, consulting signals, and product ideas.

[Here I shared how my research agent routes context to each relevant agen](https://x.com/gkisokay/status/2051474784706560096)t.

---

## 5. Don't let autonomous workflows run blind

Use a supervisor or runtime monitor to watch intended flow vs. actual flow, catch failures, and patch issues mid-run.

[This is a prompt you can use to have Codex monitor your agentic workflows and fix what breaks until they run as intended.](https://x.com/gkisokay/status/2045064220740301094)

---

## 6. Don't run OpenClaw solo for too long

I lost weeks debugging OpenClaw instead of building. Make Hermes the supervisor early. It has better UX, persistent memory, and can automatically pull ClawHub skills.

Below is my setup guide on how to set up Hermes to babysit your OpenClaw.

---

## 7. Don't auto-build before you auto-think

A self-building system first needs a self-thinking layer that notices friction, failed runs, missing tools, and recurring bottlenecks.

Below is a guide to setting up my Auto-think and Auto-build mechanism.

---

## 8. Don't give agents vague goals

Define what "done" means. Add acceptance criteria, recovery logic, deduplication, and clear success checks.

Strongly reinforce these across your self-builds and autonomy layers.

---

## 9. Don't build from loose plans

Building plans without nailing down the details ends in a failed or mediocre product. Before agents implement, force them to clarify requirements, edge cases, dependencies, acceptance criteria, and what "good" looks like.

---

## 10. Don't accept agent output without proof

Make agents test, verify, cite, or demonstrate. Trust should come from evidence, not confidence.

---

## 11. Don't scale autonomous loops without cost tracking

My agents now log the exact cost per run. That becomes critical once things start running 24/7.

[This is an example of one of my agents running a cost analysis for a single run.](https://x.com/gkisokay/status/2039903226187968843)

---

## 12. Don't use frontier models for everything

Use local or cheap models for scanning, summaries, brainstorming, and low-risk review. Save frontier models for planning, debugging, and hard reasoning.

Here is my most recent master LLM cheat sheet so you can decide which jobs require which model.

---

## 13. Don't depend on one model/provider

Model diversity is resilience. It protects you from downtime, restrictions, pricing changes, and sudden quality drops.

I personally use a mix of GPT-5.5, Minimax for tool calling, and local Qwen models.

---

## 14. Don't ignore local LLMs

Local models are the always-on layer for 24/7 background cognition. Your RAM/VRAM tier decides what work you can run cheaply.

I highly recommend playing with local LLMs. I created cheat sheets for all models that run from 8GB to 512GB devices.

---

## 15. Don't let your agents go stale

Run weekly audits after updates to tools, models, MCPs, and workflows. 

Agents decay if you do not maintain them.

---

## 16. Don't build content agents with only voice replication

Voice makes it sound like you. Taste, thesis, proof, and forbidden-pattern files make it think like you.

[Here's an example of how I approach a content agent.](https://x.com/gkisokay/status/2052713875997626567)

---

## 17. Don't think the model is the product

The system around the model comprises research, routing, memory, supervision, feedback loops, and self-improvement.

It's totally on you to build what's important, iterate until it works exactly the way you need it to, and do not move on until it's complete.

---

## 18. Don't use expensive tools

Do everything in your power to keep your agent as cheap as possible. Especially when it comes to research, you need to figure out how to save money from data providers.

I recommend using Grok for research, and here is my setup guide for using it.

---

## 19. Don't chase AGI before reliability

The magic comes after boring infrastructure like clean inputs, clear handoffs, monitoring, recovery, evals, and cost control.

It takes time to make sure these are all in place, and you have to believe that most people are unwilling to stick through the boring times to build something meaningful.

---

## 20. Don't use someone else's setup for your own

Aside from the security threats, you are going to learn so much more from borrowing ideas and adding to your own all by yourself.

Your agent is yours, so make sure it works for you. No one else's setups are going to build your use-case the way you need it.

---

## 21. Don't be afraid to share what you're building

Building in public is a magical thing. The truth is that no one really knows what's best, and it's up to you to figure it out and connect with others on your journey along the way.

If you work in isolation, you will get isolated results. Build in public and reap the benefits of your new community.

---

Thanks for reading. I hope this helps you on your journey.

If you want to build with people who care about these kinds of systems, join my free [Discord community for AI builders](https://discord.gg/TQTYPpp2fb). 

We are sharing setup ideas, agent workflows, guardrails, and experiments that move the stack forward.

If you are a business curious about how to implement AI, please check out [gkisokay.com](https://gkisokay.com/) to see how I can help.

And also remember to follow [@gkisokay](https://x.com/@gkisokay) for more AI fun.

https://x.com/gkisokay/article/2059986597391823286

— [Graeme (@gkisokay)](https://x.com/gkisokay/status/2059986597391823286) · 2026-05-28 21:14
