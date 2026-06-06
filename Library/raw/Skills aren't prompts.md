# Skills aren't prompts

Most "agent skills" shipping today are prompts in a trench coat. A paragraph of instructions, a confident name, and a hope that the model reads it at the right moment. It's a prompt you filed under a new word.

I spent the last few months building agent skills for the web at @steeldotdev, together with the team. We split five of them between us. @junhssss took steel-browser, @daneo_w took steel-developer, @nibzard built steel-skill-creator, and I own the two nobody asks about until something breaks: session-debugging and reliability. Here's what we learned, and where I think most of the conversation is wrong.

## A skill is operational code

> A skill has activation semantics: when to fire, which tools to use, what to return, and what to do when it blocks. Those four are the contract. If your SKILL.md is a paragraph of vibes, you wrote a prompt and gave it a folder.
## The hard part is when it fires

We learned this the dumb way. We loaded one broad browser skill and watched the agent reach for a browser while it was writing SDK code. The skill understood browsers fine. It picked the wrong moment.

So we made every skill narrow enough to know when *not* to fire. The narrow description is the product. It decides whether the right skill shows up at all. Most skills that fail in the wild fail right here.

## The handoff is the system

A failed run shouldn't dead-end inside one skill that shrugs. In our set it routes: browser hits a wall, hands the session to debugging; debugging gathers the evidence, hands the diagnosis to reliability; reliability proposes the fix. One skill is a tool. A set that knows when to hand off is a system. The seams are where the thinking went.

Two opinions I'll actually argue about.

## Debugging: stop guessing

Most people debug an agent by re-reading the prompt and squinting at it until it feels different. That's prayer.

A real session leaves evidence: logs, raw agent logs, semantic traces, a replay you can scrub, the network calls that failed. Start there. Build the timeline. Let the proof tell you what broke. The debugging skill doesn't speculate. It collects, redacts the secrets, and hands you a diagnosis with the smallest step that would confirm it. If I can't point at the evidence, all I have is a guess.

## Reliability: there is no magic bypass

Anyone selling a guaranteed way past bot detection is selling you a story. You'll find out which at 2am, when it stops working.

Reliability is a ladder. You climb from the cheapest rung. First question: is this even anti-bot, or is it a login behaving like a login? Preserve the evidence. Use a profile when identity matters. Slow down and lower concurrency before you reach for anything fancier. Add proxies when the evidence asks for them, not before. CAPTCHA solving comes last. Half the "blocks" I get handed are a login state nobody persisted. The fix that holds is almost always the boring one.

## Don't write skills, compile them

This is the part I'm proudest of, and it isn't mine alone. A browser trace already contains the skill: the selectors, the wait points, the values that got typed. Instead of asking someone to remember all that and type it up, we run the task twice and diff the runs to find what varied. That's the parameter. The skill gets written from what happened in the browser.

What a model remembers about a flow and what the browser actually did are two different documents. Only one of them is true.

## A score lies

Before we shipped, we tuned these skills against evals, and learned how easily a number lies. One candidate ranked top tier while firing commands that don't exist. Tuned to win the validation set, it cleared barely half its held-out tasks.

Same lesson as debugging: measure the run, not the prose. A skill that reads well and a skill that works are two different things. The only way to tell them apart is to watch the run.

## Why most skills fail

I learned this one from Niko, and it stuck: most skills fail for one of three reasons. Too broad, too static, or disconnected from what actually ran. The first two you fix by writing less. The third is the one people skip. A skill that never looks at a real run is guessing, same as the engineer who debugs by re-reading the prompt. Failed runs are training data for the next version.

## Where I landed

Skills are operating procedures. Narrow, so they fire at the right moment. Evidence-backed, so you can tell when they're wrong. Built to hand off, because real work never stays in one mode.

Install one. Run it on something real. Then break it and send me the failed session. That's the part I actually want.

@steeldotdev  @hussufo @acornsandnuts @daneo_w @junhssss @nibzard

https://x.com/0xbosta/article/2062899239810678836

— [nars (@0xbosta)](https://x.com/0xbosta/status/2062899239810678836) · 2026-06-05 22:08
