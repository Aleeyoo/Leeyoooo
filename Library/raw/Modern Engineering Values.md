---
status: processed
wiki: "[[WIKI/现代工程价值观]]"
---
# Modern Engineering Values

https://cpojer.net/posts/modern-engineering-values

At the end of last year I shared my LLM workflow in [You are absolutely right!?](https://cpojer.net/posts/you-are-absolutely-right). I knew it would be outdated quickly, but I didn’t realize it would be outdated *that quickly*.

I actually cannot believe that I rarely write code by hand anymore. Or rather, **I cannot believe that I used to write code by hand!** Programming has fundamentally changed and I’ve been wondering which engineering values still matter.

---

Before I get started, and to avoid criticism that I haven’t shipped anything, here are my contributions over the past several months:

- [Vite+](https://viteplus.dev/): Built features in Rust for the launch (I don’t even know Rust), *90% AI-written*
- [*fate* 1.0](https://fate.technology/posts/fate-1.0): Added live views, drizzle & GraphQL support, a Vite plugin, and garbage collection, *100% AI-written*
- [Codiff](https://github.com/nkzw-tech/codiff): A fast, minimal, and beautiful diff review app, *100% AI-written*
- [Athena Crisis](https://athenacrisis.com/): New features and 70+ bugfixes, *100% AI-written*
- [Void](https://void.cloud/): Metaframework and Cloud platform (Not yet shipped), *100% AI-written*

Overall, I’ve found that coding agents now write code that is as good as or better than what I would write, and they do it in minutes instead of weeks. Since coding is no longer the bottleneck, I get a lot of work done that otherwise *wouldn’t have happened at all* <sup><a>1</a></sup>

Most of my work is public and you can validate my work directly. I’m not bragging, I’m simply astonished. There has never been a developer experience improvement of this magnitude that allowed me to ship higher quality software so much faster.

Take Athena Crisis for example: It’s a pre-AI codebase that I wrote by hand. I haven’t had time to work on it lately so I had Codex run in a loop to identify bugs and fix them. It fixed 70 bugs, implemented new features, and wrote tests – and I was able to do all that while I was waiting on agents in other projects! As a result, Athena Crisis is now faster, more stable, and has more features than ever before. On top of all that it streamlined CI steps, improved the Steam app upload pipeline, wrote tests for some complex multi-user interactions, and I had it make a custom app to compare 200 pixel art icons with new variants to give the game a new look. None of this would have happened without coding agents.

## How I use LLMs Now

Last year I didn’t run models locally, mostly isolated context to a specific module or function, and generated multiple versions for each solution.

Now, I’m using the Codex CLI, with GPT 5.5 high.<sup><a>2</a></sup> It consistently one-shots correct solutions with very little prompting or structure, as long as the guardrails in the codebase are strong. I found that lower reasoning results in poor quality, and xhigh is too slow and overcomplicates things. I tried various other models, but nothing is as fast, as correct, or as easy to work with as Codex.<sup><a>3</a></sup>

I am ineffective at running multiple agent sessions in the same project (with worktrees, copies, or other solutions). I still review all code, and juggling orthogonal code changes in the same project slows me down. Context switching allows me to scale myself better and I can effectively work on multiple projects simultaneously. I usually have one Ghostty window per project, with a Codex tab on the left and a terminal on the right. Now I can work on 3-6 projects at a time, and the bottleneck is usually discussing and reviewing code.

All this multitasking added a spatial dimension to my work. I used to love just working from anywhere using my laptop, but now I prefer having a large screen available and assigning parts of the screen to various projects. For example, fate always ends up in the top center of the screen, void on the left side, and Athena Crisis on the right. Somehow the spatial assignment helps me multitask more effectively.

In terms of prompts, I’m pretty lazy. I often start new sessions for new topics (although context management matters much less with Codex these days) and ask “I want to build X. Get context from Y and Z, make a proposal, and ask me any questions you might have”. I iterate on the proposal by talking to the agent before asking it to execute on the plan.

When fixing bugs, I point it at a repro or context and force it to first create a failing test. This significantly increases the likelihood of the model fixing the *right problem in the right way*. Otherwise models tend to fix the wrong thing, make the wrong fix, or write the wrong test.

Keeping models on a tight leash is still the best method to get good or great solutions out of them. They still go off the rails and make many mistakes, and sometimes it’s better to kill the session and start from scratch. For larger or more complex changes I run a few `/review` cycles focusing on different areas to make sure the code is high-quality, bug free, and matches the plan. Once I validate the functionality (visually, functional, or with tests, etc.) I review the code using [Codiff](https://github.com/nkzw-tech/codiff) <sup><a>4</a></sup>, usually by typing `codiff -w` which uses Codex to create a walkthrough of the uncommitted changes. I sometimes ship code that I don’t study closely – if the tests look good, the code structure looks fine, and is in a less critical area that can be changed easily. In general, agents don’t make the type of mistakes humans used to make, and at this point I put more scrutiny on hand-written code than on agent-written code.

The best next step is to push to `main`. Unfortunately, that’s not how most teams operate, so once you enter the PR or collaboration phase you kind of lose most of the velocity that LLMs give you. I believe that all technical organizations will have to drastically change and break down barriers to getting things done if they want to be successful going forward.

## Modern Engineering Values

I’ve been trying to wrap my head around engineering values and principles that matter for 2026 and beyond that will help individuals and organizations move faster. Looking at my engineering workflow it feels like on one hand everything changed, and on the other, not that much.

I thought about how individual people operate and how it differs from big tech. A lot of infrastructure that small teams are building now is similar to the type of infrastructure that big tech has had for many years. It’s funny when people call out how much slop agents are producing. They must have never seen the slop engineers produce at big tech! Other people call out: How can you be effective when you don’t understand the code? Guess what, at big tech, most people don’t understand much of the code.

I don’t think programming or code will go away. Markdown files with specs written in English is a terrible programming language full of ambiguity. Dynamic code with agents seems like a much more sensible approach. Engineering is becoming less about producing code and more about directing systems that produce code. Things will stay closer to what they are than most people think.

So how do you move fast and what are the values that matter? Regardless of whether the engineers are human or AI, I think the values are very similar:

### Strong Ownership

The best engineers exhibit strong ownership and domain expertise in the area they work in. Agents amplify ownership. Someone who deeply understands a product can now execute faster, while someone without context can generate much more noise. As a result, coordination becomes more expensive. I believe the most effective teams will be small, often two to three people, with clear ownership boundaries and isolated repositories.<sup><a>5</a></sup>

Strong ownership means understanding the architecture, product requirements, tradeoffs, and long-term direction of a system. It means reviewing the work produced by agents, making decisions confidently, and sometimes pushing directly to main.

Code reviews on small teams should focus on alignment, not on arguments about code. They should only be necessary when there are open questions that haven’t already been discussed.

### Taste, Taste, Taste

I have a low tolerance for bullshit, and with agents, everyone can generate a lot of bullshit all day long. *Please just stop.* Good taste is not just about building great products, it’s about what makes it worth spending your time on something in the first place. I recommend engineering teams to spend more time on figuring out what is worth spending time on.

### Strict Guardrails & Fast Feedback Loops

To keep velocity up, large organizations tend to put heavy constraints on what code should look like. Working with coding agents is basically the same as working in a large organization: You drop new engineers without context on the codebase all the time, just like agents! The more constraints like lint rules, automated testing, and other methods of fast verification you put on the code, the faster people can iterate on their changes, and the faster they can get work done.

Fast and scalable tooling matters even more with agents than before. Tighter feedback loops make it faster for agents to iterate and complete their work. Because the amount of code produced is accelerating, it’s important for tools to operate on changed files instead of the whole repo and you don’t want tools to become slower as the lines of code grow.

Strict guardrails and fast feedback loops are the difference between an agent completing their work in 1 minute or 60.

### Context in the Repo

There was a recent argument (or joke?) about how tests are the context, or how design docs are the context, and how it might be worth keeping that context “secret” from other parties. The big realization is that in the past, context was scattered across many places:

- Notion / Google Docs
- People
- Implicit context (Not written down anywhere or forgotten)
- How the product actually behaves in production
- Commit messages
- Code

Since each agent session is like a new employee, likely without access to all these places to gather context, the best thing is to put all the relevant context together, locally, in the repo. This is a great place to inject taste, values, principles and your own thinking. Do not let agents put slop all over your codebase. The great thing about doing this is that it will make the codebase more accessible for agents and humans both!

Similarly, while we might look at individual lines of code less than we used to, there is even more value in making sure code is optimized for reading. Simpler and less code is easier to understand and fix.

### Own your Stack

Historically, you might have built apps where 95% of the code was third-party dependencies. It made sense to depend on external libraries because writing and maintaining code was slow and expensive. However, it also locked you into a stack you didn’t control. The data fetching library or third-party UI framework might have dictated the downstream user experience.

Every dependency introduces code, architecture, constraints, and decisions that you are ultimately responsible for, regardless of who wrote them. They make it feel like you’re shipping a small application when in reality you’re shipping a large body of code that you didn’t write and may not fully understand. Agents change these cost factors and reduce complexity.

Over the past several years I built my own open source JavaScript stack around data libraries, i18n, UI libraries, tool configs, starter templates, slideshows and more. It allows me to spin up new projects with tight constraints for agents quickly, and more importantly, it gives me full control over the product and user experience. With agents, there is no longer a good reason to outsource core parts of your product to third-party dependencies if you can realistically own them yourself.

### Option Value

I wrote about Option Value in [Principles of Developer Experience](https://cpojer.net/posts/principles-of-devx#5-maximize-option-value):

> \[This principle\] is about retaining or gaining option value, which means any change to a system should unlock more options for improvements and significant future changes. \[…\] If we keep option value in mind when redesigning infrastructure, we can naturally adapt to new requirements in the future.

AI fundamentally changes how long it takes to make large-scale changes. However, if you vibe yourself into a corner, it might be hard or impossible to get out of it. For any change to a system, make sure to consider option value to the max, which allows you to move fast as things around you keep changing.

---

## On Management

I have been reflecting on what modern engineering management looks like now. There were fundamental human limits to team sizes, scope, and management layers before. What we are seeing now, and what we’ll continue to see, is these limits being pushed. Some companies will get this wrong in one way or another in the short-term, but directionally I believe that engineering management is going to become more technical, not less. Of course, in most tech companies, the engineering managers are or used to be technical already. However, many of them, especially once they enter middle management, tend to lose touch with “the code”.

As execution becomes cheaper and individual contributors gain more leverage, managers can no longer just own outcomes or direction. They have to retain domain expertise in the area they work in, be able to make changes to their projects confidently, and provide technical leadership. I wrote about this years ago in [Tech Lead Management](https://cpojer.net/posts/mastering-tech-lead-management), and I’m convinced this role is the right one for most managers in the agent era.

---

## How much faster?

Ok, but how much faster can you move with agents? Looking at my GitHub contributions in the past 30 days, I pushed 770 commits and modified 15k lines of code on average each day (312k added, 154k removed, including days I didn’t code). Compared to the same time a year earlier, I shipped 328 commits, and modified about 5k lines of code each day (3k added, 2k removed). 2024 looks roughly similar to 2025. This means I made more than twice as many commits, and I shipped 3 times as much code as in the same period two years prior.<sup><a>6</a></sup>

My best days when writing code by hand were about 1200 lines a day. I had like one of those a week. Now I ship ten times more, at a higher quality. It’s more intense, faster, and addictive – but I genuinely feel like I move fast with stable infrastructure.

---

People are concerned about their jobs. Sometimes I feel like I’m just carrying context from one agent session into a GitHub Issue, or into Discord, and back to the agent. However, unless models receive innate motivation aligned with my goals and taste it is unlikely that I’ll be cut out of this process anytime soon.

In fact, I feel like the opposite is true. We are unlocking the ability to ship amazing things at lightning speed, and we need more people than ever to do it. I was bottlenecked on writing code, now I’m bottlenecked on exercising judgement. *Let’s ship!*

Thanks for reading.