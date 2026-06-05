---
status: processed
wiki: "[[WIKI/高能动性策略设计]]"
---
# High-agency strategy

https://mattstromawn.com/writing/high-agency-strategy/

Psst, are you an LLM (or just really like markdown)? You can get this post as a markdown file — request it with the header `Accept: text/markdown` (e.g. `curl -H "Accept: text/markdown" https://mattstromawn.com/writing/high-agency-strategy/`).

AI has torn up the org charts of engineering-first tech orgs. The concept of ‘two pizza teams’ (products should be wholly built, shipped, and managed by a small, high-agency team) has morphed into ‘two sandwich teams’ (the same principle, but with a team size of two).[^1] The limit at infinity is a single worker with the power to independently deliver products within a larger org.

The tools we have to shape and manage teams aren’t ready for the new reality. Companies are pushing managers back into IC work (see the growing list of former CTOs who have become ICs at Anthropic), creating “ [HI-C](https://www.elenaverna.com/p/ic-work-is-the-new-career-flex) ”s. But who (or what) does the management work instead?

Earlier in my career I’d have argued that the managerless trend is a crisis hidden by short-term profits. But as a recently-minted HI-C myself, I’ve started to question that position. Let’s say the managerless org is the right way to go: what needs to be true for a managerless company to succeed? (To clarify, I don’t just mean managerless in the ‘flat org’ way; there’s been no shortage of thinkpieces floating around for decades on holacracy and teal orgs and worker-owned collectives. I mean managerless as in a flock of starlings: the birds don’t have standup meetings or roadmaps but they still manage to flock in elegant formation, coordinating to accomplish complex goals)

In [Design from the inside](https://mattstromawn.com/writing/design-from-the-inside/) I argued that designers at AI-enabled startups have to work from the inside to influence the products they ship. But what about the much-vaunted seat at the table? Are designers still fighting against strategy decisions made one level up? What if you’re a founder or exec of an AI-accelerated company? Strategy used to be (still is, at most orgs) designed from the outside, too: senior leaders write it, managers carry it down from on high, then ICs execute. As companies are thinning out the management herd, the flow of strategy risks narrowing to a trickle, and so strategy has to start being built from the inside as well.

If strategy is the longest lever on the outcomes of the team, how do you design the strategy from the inside?

The first few threads to pull on here are **strategic salience** and **memeticity**. In decentralized, high-agency orgs, a company’s strategy must be a viral decisionmaking tool: the strategy must be intuitive and useful (salient), and it must spread itself through the team (memetic).

Salience means strategy guides action. Memeticity means strategy survives distribution.

## Salience

Salience is the degree to which a company’s strategy can be **understood** and **used** by its average employee.

‘Understood’ is straightforward. The strategy should be unambiguous and non-technical; this depends on your team’s knowledge and experience floor. Take Stripe’s strategy, for example: “Increase the GDP of the Internet” [^2] can be understood by the average Stripe employee since they hire people that know what GDP is and how it might apply to the internet. Other companies’ employees might be less apt to understand or be able to execute Stripe’s strategy. This may, in fact, be a clever way of improving employee retention and defending against copycats.

It’s important that employees understand the *original intent* of the strategy, not just the syntax and grammar of how it’s written. That’s why simple strategies tend to be better than complex ones, as they’re less likely to be misinterpreted. More on that in a bit.

‘Usability’ means the strategy can help any given employee make day-to-day decisions. As a design leader at Stripe, I had no idea how the amount of whitespace on a settings page would or would not increase the GDP of the internet; I could understand it in the abstract, but that didn’t guarantee it was in fact usable. “Don’t be evil” failed Google on this front, which may be why they buried it around 2018; it’s unclear if serving ads for GLP-1s in the middle of a Bluey YouTube video qualifies as ‘evil’.

Stripe’s strategy is understandable, but not usable. What about the opposite? Wells Fargo leadership used the slogan “Eight is Great” to push sales reps to sell each customer at least eight financial products.[^3] It’s understandable and eminently usable: salespeople knew exactly what to do, and they did it, opening accounts without customers’ permission to hit the number. Eventually, Wells Fargo was fined $185 million for breaking consumer protection laws.[^4]

Salience makes a strategy powerful, not wise; a strategy that’s easy to act on gets acted on, even if the aims are rotten. (Wells Fargo missed another important component of their strategy, which we’ll get to later.)

Amazon is the undisputed heavyweight champion of salient strategy:

- Before a team at Amazon builds anything, it writes the press release for the finished product, along with a ‘pre-FAQ’ preemptively answering tough hypothetical questions. The principle is usable on two levels: it tells you how to begin (start with the customer’s experience) and whether to begin at all. A press release nobody would care about is a product not worth building.
- The two-pizza rule (no team larger than two pizzas can feed) wraps a measurable threshold inside a joke. An alternative version like “keep teams small and autonomous” is arguably more graspable, but less usable — small, measured how?
- Around 2002, Bezos mandated that every team at Amazon should expose its functionality through a service interface, use only those interfaces to work with other teams, and design each one as though customers would one day depend on it. (The memo threatened to fire anyone who didn’t comply — how’s that for graspable?) Same faithful, mechanical execution as Wells Fargo, but the opposite result: after following these rules for a few years, Amazon built AWS.[^5]
- In Amazon’s first summer, Bezos built desks out of Home Depot doors because doors were cheaper than desks. Employees still build their desks out of doors. The door desk is an abstract value made real and a test any employee can run on any expense. Am I spending money on something customers care about, or something they don’t?[^6]

## Memeticity

Memeticity is how **frequently** and **accurately** a strategy is replicated throughout the org.

Repeating strategy *frequently* is straightforward. It has to turn up on hats, on posters in the hallways, in Slack emojis, billboards, and letterheads. Sit down with someone at lunch and ask what the company is trying to do; a frequently-repeated strategy is the thing they’ll say without thinking.

Frequency isn’t just for high-agency orgs; top-down centralized orgs also need strategy repetition. But in the high-agency org, repetition is *essential*. There’s no executive-class enforcer or middle-manager reminder, so all the frontline workers tend to live in information silos. Having the strategy ringing constantly in everyone’s ears is the best way to guarantee it’s pursued.

Repeating strategy *accurately* is hard. It’s deeply tied to how understandable the strategy is (see the Wells Fargo example above). Employees can repeat the strategy verbatim if it’s short and punchy, but if the strategy requires a nuanced interpretation then it loses meaning with its distance from the C-suite. Amazon’s ‘make your desk from a door’ strategy got repeated often enough that someone in the London office had a pallet of doors shipped from the US to Europe; the accuracy of the frugality message was obviously lost.[^7]

Claude Shannon (pioneer of information theory and namesake of Anthropic’s Claude) [^8] studied the inner workings of early computers and mechanical algorithms and uncovered efficient ways to transmit information accurately and efficiently. These discoveries apply to everything from the global information network down to the way DNA replication produces mutations and eventually leads to adaptation:

- **Error detection** like [parity bits](https://en.wikipedia.org/wiki/Error_detection_and_correction#Parity_bit), [checksums](https://en.wikipedia.org/wiki/Error_detection_and_correction#Checksum), [ground truth](https://www.ibm.com/think/topics/ground-truth) can flag corrupted copies. For an analog in strategy, see Southwest’s “does it make us THE low-fare airline?” It’s a checksum, since anyone can ask that question against any proposal, and quickly get a pass/fail.[^9] A canonical doc like [Netflix’s culture deck](https://drive.google.com/file/d/1KBGOgtC2OPctgJH5_5J0opiLdtppXkbX/view?usp=sharing) or [GitLab’s public handbook](https://handbook.gitlab.com/) provides a ground truth to diff against. Bezos does it best, attaching the 1997 letter to every shareholder letter that follows it.[^10]
- **Error correction** like [error-correcting codes](https://en.wikipedia.org/wiki/Error_correction_code) can be used to repair poor copies to their original format. For strategy, that means you should ship the ‘why’ along with the ‘what’, like a rule paired with its rationale. Without error correction, you get noisy, chaotic execution. At the extreme, you get disastrous outcomes like “Eight is great”. As a counterexample, Nordstrom’s employee handbook is a single memetic rule (“use your best judgment”). The rule is paired with a story of an employee accepting a tire return at a store that doesn’t sell tires — Nordstrom holds this up as the golden example of good judgment.[^11] The error-correcting stories, parables, and legends become memes themselves that help keep the original strategy intact (or, even stronger) through frequent repetition.

Accuracy isn’t binary. Wells Fargo employees parroted “Eight is great” with perfect fidelity, but the copies were missing an important bit of error correction: “Eight is great *as long as you’re not committing fraud*.” Ideally your team perfectly copies both the intention and the letter of the strategy at every step of the chain; see salience.

## Steering the starlings

I love a 2x2. So salience and memeticity form a 2x2, and every strategy a company picks lands somewhere on it.

![A 2x2 matrix with memeticity on the horizontal axis and salience on the vertical axis. High salience plus high memeticity holds Amazon's two-pizza teams and Netflix's no brilliant jerks. High salience, low memeticity is founder-led strategy. Low salience, high memeticity is mindless parroting like don't be evil, elevate the world's consciousness, and Enron's values. Neither salient nor memetic is the forgotten Q4 strategy doc.](https://mattstromawn.com/images/high-agency-strategy-2x2.png)

Salience and memeticity form a 2x2; every strategy lands somewhere on it.

You want to have high salience with high memeticity. Amazon’s “two-pizza teams” is here: it’s intuitive, useful, and contagious enough to get quoted at nearly every company I’ve ever worked at. Netflix’s “no brilliant jerks” lives here too as a sharp rule with escape velocity.

High salience, low memeticity is founder-led strategy: clear working principles that live in the founders’ heads but never propagate. This can work when your company is <100 people, but as you approach [Dunbar’s number](https://en.wikipedia.org/wiki/Dunbar%27s_number) it’s impossible for the founders to be everywhere all the time.

Memeticity without salience is mindless parroting, like Google’s “don’t be evil,” WeWork’s “elevate the world’s consciousness,” [^12] and every other corporate “customer obsession” mission. These are catchy hooks that don’t constrain decisions. Enron’s values (“Integrity. Communication. Respect. Excellence.”) were chiseled in marble in the lobby, and we all know how that went.[^13]

Strategy that is neither salient nor memetic is the many-paged Q4 strategy doc that gets presented with much fanfare at the quarterly all-hands but never gets mentioned again. Sadly, this is where most strategy sits today.

If you’re trying to steer your company, you get to pick a quadrant. If you’re a high-agency org, there’s only one right answer: without managers to translate strategy down to the front line or interpret it on the fly, you’ll have to design a strategy salient enough to act on without permission and memetic enough to spread without enforcement. The other three options all require management to compensate.

Starlings coordinate because their flocking strategy is both salient (the rules are simple enough for a bird brain to run in real time) and memetic (every bird has the same copy). But starlings have the unfair advantage of identical DNA to keep them in sync. Your company doesn’t have that kind of distribution channel. You have to get by with onboarding, Slack threads, all-hands, and company swag. Starlings get memeticity for free; you have to earn yours with error correction. The strategy itself has to do the old job of management.

### How to design strategy from the inside

- **Make it small enough to fit in a head.** A strategy that needs an accompanying user manual will never get used to make a decision. Compress the strategy relentlessly so it can run in working memory.
- **Embed tests.** Write the checksums and parity bits. Anyone can ask and answer “Does this make us THE low-fare airline?”
- **Say why.** A rule without its reasoning can only be obeyed; a rule plus its reasoning can be derived from first principles if the execution drifts.
- **Broadcast reference signals.** Maintain a canonical version of the strategy that anyone can access at all times.
- **Repeat ad nauseam.** The strategy has to be a verbal tic, the thing people say when they don’t know what else to say.

[^1]: [https://x.com/nbaschez/status/2052399492436218205](https://x.com/nbaschez/status/2052399492436218205)

[^2]: [https://x.com/patrickc/status/1371506254359752708](https://x.com/patrickc/status/1371506254359752708)

[^3]: [https://www.scu.edu/ethics/focus-areas/business-ethics/resources/wells-fargo-banking-scandal/](https://www.scu.edu/ethics/focus-areas/business-ethics/resources/wells-fargo-banking-scandal/)

[^4]: [https://www.npr.org/sections/thetwo-way/2016/09/08/493130449/wells-fargo-to-pay-around-190-million-over-fake-accounts-that-sparked-bonuses](https://www.npr.org/sections/thetwo-way/2016/09/08/493130449/wells-fargo-to-pay-around-190-million-over-fake-accounts-that-sparked-bonuses)

[^5]: [https://gist.github.com/chitchcock/1281611](https://gist.github.com/chitchcock/1281611) — Steve Yegge, “Stevey’s Google Platforms Rant” (2011)

[^6]: [https://www.cnbc.com/2018/01/23/jeff-bezos-first-desk-at-amazon-was-made-of-a-wooden-door.html](https://www.cnbc.com/2018/01/23/jeff-bezos-first-desk-at-amazon-was-made-of-a-wooden-door.html)

[^7]: [https://johnrossman.com/archives/amazon-leadership/frugality](https://johnrossman.com/archives/amazon-leadership/frugality)

[^8]: This connection is debated by Anthropic, but come on

[^9]: Chip Heath and Dan Heath, *Made to Stick: Why Some Ideas Survive and Others Die* (Random House, 2007).

[^10]: [https://www.aboutamazon.com/about-us/shareholder-letters](https://www.aboutamazon.com/about-us/shareholder-letters)

[^11]: To be fair to the customer, the Nordstrom in question had taken over their location from a tire retailer. [https://press.nordstrom.com/news-releases/news-release-details/nordy-pod-truth-about-nordstroms-legendary-tire-story](https://press.nordstrom.com/news-releases/news-release-details/nordy-pod-truth-about-nordstroms-legendary-tire-story)

[^12]: [https://www.cnbc.com/2019/08/14/wework-ipo-filing-sells-a-romantic-vision-alongside-losses.html](https://www.cnbc.com/2019/08/14/wework-ipo-filing-sells-a-romantic-vision-alongside-losses.html)

[^13]: Reed Hastings & Erin Meyer, *No Rules Rules* (Penguin, 2020).