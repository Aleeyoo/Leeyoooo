---
status: processed
wiki: "[[WIKI/AI辅助设计方法]]"
---
# How to Design using AI (Builder's Guide)

![](https://pbs.twimg.com/media/HJaz6F5bIAAXvlF.jpg)

I built a personal Notion this weekend.

Not a clone. The parts of Notion I actually use, stripped of everything I don't. 

No teammates. No comments. No notification feed. Just my tasks, journal, habits, bookmarks, and pages. One screen. My data. My machine.

> If you don't want to read the entire article, get the Github repo from below and check the product work through. 
**The whole thing runs on two tools doing two jobs:**
1. **Moonchild AI does the design (brief through final visual)**
2. **Cursor and Codex do the code. The rest is incidental.**
[📹 video](https://video.twimg.com/amplify_video/2059923863027458048/vid/avc1/698x360/35KnU1hyiPmm4RlQ.mp4?tag=27)

t's real. State persists. Dark mode works. Keyboard shortcuts work. You can use it at

> TLDR : github.com/codejunkie99/cove
That's the whole post. The rest is the how.

## The mental model

> TLDR; If you don't wanna read that long just watch this video on how I designed the frontend 
[📹 video](https://video.twimg.com/amplify_video/2059798225381625856/vid/avc1/480x270/nyRMwqbCsNSMoVPB.mp4?tag=27)

** Keep reading on my Design process using AI**

Three principles. Get these right and the rest is mechanical. Get them wrong and no tool saves you.

**1. Design the flow before you design the pixels.** The pixel is the last thing you decide. Before the pixel is the screen. Before the screen is the state. Before the state is the action. Before the action is the person, and what they were trying to do when they arrived.

**2. The prompt is the brief.** A one-line prompt is a one-line brief. You would never accept a one-line brief from a human designer you were paying. Don't accept it from an AI one. Write 500 words. Tell it the audience, the constraint, the feeling, the references you admire, the references you refuse to look like.

**3. The design system lives in the repo.** Not in Figma. Not in a Notion page. In a markdown file checked in next to the code, edited in the same pull requests, committed in the same history. Anything else drifts.

Save these as a file in your project. I keep mine as principles.md at the root of every new repo. Open a fresh project, paste the template, edit one line at a time until each principle is one sentence you'd defend in an argument.

Commit that file. Read it before you start. The principles aren't decorative. They're the gate every later phase passes through.

## **The workflow at a glance**

Eight phases. Each one has an artifact. Each artifact is a markdown file you keep. The whole project sits in one folder.

That's the structure. Hold it in your head.

## Phase 1: Write the brief

## TLDR : if you wanna skip reading the detailed principle here is a shortcut I follow

![](https://pbs.twimg.com/media/HJXhaiYb0AAeFu_.jpg)

This is the only part where you really have to think. Sit down with a notebook. Answer five questions before you touch a model.

1. **Who is this for?** A specific person. Not "knowledge workers." A specific person with a specific problem on a specific Tuesday morning.
2. **What's the one job?** The single thing the app does well. Not five. One.
3. **What does it feel like?** Two or three adjectives. Calm. Sharp. Warm. Whatever. Specific.
4. **What does it look like nothing else?** Two or three apps you'd be proud to be compared to.
5. **What does it refuse to look like?** Two or three apps you'd be insulted to be confused with

**Examples, so you have something to compare against:**

- *Task management.* "Every app I've tried treats my task list like a project plan. I don't run projects. I run mornings. I want to see four things, mark three of them done, and close the tab."
- *Journaling.* "I want to write 200 words a day. Every journal app I've tried makes it a forty-five-step ceremony. I want to open the app, write, and not see a streak counter that makes me anxious."
- *Learning.* "I read papers and forget them in a week. I don't want a flashcard system. I want a one-screen surface where my notes from this week are searchable and the rest is out of sight."
Notice what these have in common. Specific person. Specific failure of the existing market. Specific shape of the right answer. No "AI-powered." No "for teams." No vagueness.

This step took me twenty minutes. Most of that was crossing things out.

Now you have a brief. Time to find the vocabulary

## Phase 2: Find your design vocabulary

Every good designer carries a vocabulary. Not in words. In references. They know what "Stripe-feeling" means. They know what "Linear's restraint" means. They know what "Notion early days" means versus "Notion now." Those aren't moods. They're specific, decomposable design languages. You can teach one to a model in twenty minutes if you're willing to do the work.

The trick: don't describe the aesthetic you want. Make the model describe it for you, by feeding it references and asking for a decomposition.

I do this in the design tool. Open the project, attach the brief, paste this prompt, drop in the URLs.

For Cove I dropped thirteen URLs into it and asked it to find the throughline. The list, exactly as I pasted it: stats.design, hapy.design, michael.fm, prefolio.framer.website, harshith.com, mohdesignz.framer.website/#work, tommkv.me, ramx.in, dhairya.dev, lakshb.dev, legions.dev, chanhdai.com, pqoqubbw.dev.

Not famous products. Mostly personal sites and developer-built micro-tools.

1. Why those instead of Notion or Linear or Stripe? Because famous products have been picked apart by every model in existence. The vocabulary you get back is regurgitated. "Notion-like."
2. "Linear-clean." "Stripe-elegant." Words the model has seen a million times that mean nothing specific. Less-known reference sites force the model to actually look at pixels, because it has nothing else to lean on.
3. Moonchild came back with the throughline in one paragraph. Zinc and dark palettes with subtle grain. GestSans-style sans and monospace typography. Modular card-based layouts. 
4. Minimal Mac-native chrome. Soft shadows. Frosted UI details. And the line that became the entire product positioning: *"a crafted, quiet interface rather than a loud SaaS dashboard."*
5. Seven words. I committed that sentence verbatim as the opening line of the brief. Every later decision was a referendum against it. If something I was about to ship would have felt loud, performative, or SaaS-like, it didn't ship.
That extracted vocabulary became the *language* I used in every later phase. Instead of saying "make it minimal," I could say "zinc neutrals, mono labels on metadata, Mac-native chrome, no SaaS density." Specific. Decomposable. Testable.

Save as design-library.md. Commit it. This file is reusable across projects. You'll build a personal library of these over time, one per aesthetic you care about.

## Phase 3: Design the flow

This is the phase nobody does. It's why most AI-generated apps look great and feel wrong.

A flow is not a screen. A flow is a path. Where the user enters, what they do, what state changes, where they end up. Before any visual decision, you describe every flow in plain English. If you can't name a flow, you can't build it. If you name it but it sounds tedious, the product is wrong before you've drawn a single rectangle.

In the same  project, with design-library.md and the brief attached as persistent context, I paste the flow prompt and let it run.

![](https://video.twimg.com/amplify_video/2059802231902466049/pl/ZrNwhYzXInpRGwIc.m3u8?tag=27)

The first pass came back in two minutes. Three core flows: Daily Check-In, Quick Capture, Command Palette. Six screens. Twenty-one states. Eight transitions. Four open questions, all of which were real, all of which I had to answer before continuing.

The first draft was also slightly wrong. It had invented a "team sharing" flow nowhere in the brief. I deleted it and re-ran. The tools aren't magic. They overshoot. You correct them.

![](https://pbs.twimg.com/media/HJXj2KoaMAAPZHB.jpg)

![](https://pbs.twimg.com/media/HJXj5O2agAAImy5.jpg)

Two non-obvious things about this phase.

First, *the open questions are the highest-value part of the output*. They're the places your brief was vague, surfaced by a system that doesn't have the social courtesy to pretend it understood. Answer them. Re-run the prompt. The flow.md you commit should have zero open questions left.

Second, *you can't design a screen until you know what flows pass through it*. The dashboard exists because three flows converge on it. The command palette exists because two flows accelerate by skipping screens. The architecture of your UI is just the geometry of your flows. Get the flows right, the UI is mostly forced.

Save as flow.md. Commit it.

## Phase 4: Generate the PRD

TLDR; this is the in-action part of the process

![](https://video.twimg.com/amplify_video/2059804273559601152/pl/K40uRvcjLb07N0GT.m3u8?tag=27)

This is the phase most people skip. It's why most AI-generated apps feel like a beautiful skin over no spine.

The PRD is the contract. It names every feature, every data type, every state variant, every acceptance criterion. Once it's written, every later decision (visual design, code, deploy) has something to be tested against. Skip it and you're building on vibes.

Same project, same thread, third prompt.

The "Out of Scope" requirement is non-negotiable. It's the single highest-leverage section of any spec. The model will happily expand your product into a Swiss Army knife if you let it. Forcing it to name what it isn't forces a point of view.

Cove's spec.md came back with twelve features, seven data types, five user flows, twenty-six state variants, and an "Out of Scope" that explicitly rejected calendar integration, team features, and AI assistance inside the app. I kept all three rejections.

A representative slice

## Phase 5: Generate the final design

Now the visual question. The PRD is fixed. The vocabulary is fixed. The flow is fixed. The design has to satisfy all three.

Same Moonchild project. The trick at this phase: don't ask Moonchild for one design. Ask for three.

I ask for three first-pass design directions, each fully formed enough to react to. The instruction is explicit: don't converge. Give me three different bets so I can see what the design space looks like before I commit to a corner of it.

The reason this works in Moonchild: Moonchild renders. Within minutes I can see real palettes on real components, in light and in dark, and react to them visually instead of imagining them from a spec.

For Cove the three directions came back as:

- **Direction 1: dark three-panel editor.** A dense workspace in the spirit of Linear and stats.design. Clean left sidebar, central block-editor canvas, right-side info panel, zinc palette, blue active states, monospace labels. The "serious tool for serious work" direction.
- **Direction 2: Today-view widget dashboard.** A daily command-center page. Current time and greeting, focus task card, habit tracker, journal streak heatmap, quick note, words-this-week chart. Scannable cards for daily planning. The "open the app, see your day" direction.
- **Direction 3: light editorial editor.** A warmer, lighter writing surface. White or stone canvas, orange accent system, wide margins, large typography scale, floating toolbar. Optimized for deep writing sessions. The "this is where the words happen" direction.
![](https://pbs.twimg.com/media/HJXqHZcbwAAaUhf.jpg)

I didn't pick one.

![](https://pbs.twimg.com/media/HJXqbldacAARFCp.jpg)

That decision generated four canonical app states: Today × Editor × Light × Dark. Those four screens became the source of truth for every later design decision in the project. Once the four states were locked, everything downstream was just instantiation.

1. The remix move is the part most builders skip. They ask for one direction, iterate it to death, and end up with a polished version of the model's first guess. Ask for three and then explicitly synthesize across them. 
2. The act of articulating *what*you preferred in each is what surfaces your point of view. You can defend the design later because you've already defended every move out loud.
3. About sixty percent of what Moonchild produced in the first pass survived. The other forty percent got cut, rewritten, or replaced. But having something rendered to react to was faster than designing from a blank canvas by an order of magnitude.
4. Once the four states felt right, I asked it to write them up as style.md: a single markdown file with the color palette, type ramp, layout grid, every component and its states, and a voice section.

## **How to Iterate
**

**TLDR; take a look on the exact prompt to get the style.md **

[📹 video](https://video.twimg.com/amplify_video/2059813235692552192/vid/avc1/480x270/qYid2dQdRCmOel5R.mp4?tag=27)

**A representative slice of the resulting style.md:**

Each iteration was specific. Each iteration named the failure mode. The model is a mirror. Give it precise critique, get precise improvement.

Save as style.md. Commit it. The design is locked.

## Phase 6: Design the backend

Most AI-built apps die here. The model produces a beautiful frontend that's wired to nothing. The builder bolts persistence on as an afterthought. By then the data layer is contaminating the UI layer and you're rewriting half the codebase.

Avoid that by designing the backend as its own phase. Same workflow: prompt, markdown, review, commit.

For Cove, the backend is deliberately tiny. No server. No database. Everything lives in localStorage, with cross-tab sync. The architecture diagram, as it appears in architecture.md:

Three rules made this work. All three came from the architecture prompt below.

1. **All state lives in App.tsx.** No Context, no Redux, no Zustand. Props threaded down. One component tree with one source of truth.
2. **One hook for persistence.** useLocalStorage<T>(key, defaultValue) handles read, write, validation, and cross-tab sync. Every persistent field uses it. There is no other path to localStorage.
3. **CSS variables, not Tailwind dark: variants.** Theming is a single set of custom properties in :root and .dark. Components read variables. They don't know about themes.
The prompt that produced **architecture.md:**

The "invariants" section is the part to obsess over. For Cove they read:

1. App.tsx is the only file that owns mutable state.
2. Every persistent field uses useLocalStorage. There is no direct localStorage access elsewhere.
3. Theming is read-only at the component level. Components consume CSS variables, they don't toggle classes.
4. The command palette is the only component allowed to know about all views simultaneously.
Those four sentences are worth more than a thousand lines of documentation. They're the contract any future change has to respect. Name your invariants and you can review your own pull requests.

Save as architecture.md. Commit it.

## Phase 7: Build with Cursor and Codex

Now the magic. Five markdown files exist. The agents get them as input. The agents write the app.

This phase forks into two tools, by design.

**Cursor for building.** Cursor's agent mode reads style.md directly through MCP wired to the Moonchild project, so when I ask it to build a feature it doesn't have to be told what the FAB looks like or what the type ramp is. It reads the design system and complies. 

[📹 video](https://video.twimg.com/amplify_video/2059905216879984640/vid/avc1/1280x720/Z7eOLLkNZP8Utu5B.mp4?tag=27)

[📹 video](https://video.twimg.com/amplify_video/2059904088108900352/vid/avc1/430x270/YMxLLaLHXzKxfbNd.mp4?tag=27)

That's how the entire backend got coded: Cursor pulling from style.md and architecture.md in the same agent loop, building one feature at a time. Scaffold first, then state layer, then components smallest to largest. I review after each one.

1. **Codex for reviewing.** Once Cursor ships a feature and the file compiles, I hand it to Codex as an independent reviewer. A second agent that didn't write the code. The job isn't to write more code. 
2. It's to catch the bugs Cursor's bias missed. Type errors that slipped through. Accessibility regressions. Edge cases the spec named that the implementation forgot. Codex flags. Cursor fixes. The bug rate on what ships is dramatically lower than what either tool produces alone.
3. Building and reviewing are different mindsets. The building agent wants to ship. The reviewing agent wants to find faults. 
4. Put one model in both seats and "ship" usually wins, which is how generated code piles up silent bugs. Two agents fix this by being literally two agents.
The prompt that goes into Cursor:

Cursor took about an hour. Then Codex did a review pass and flagged six issues. Cursor fixed five. The sixth I fixed by hand because it was a microcopy line and I wanted the wording mine.

![](https://video.twimg.com/amplify_video/2059908839441625088/pl/yymDuw6TM3XJWurx.m3u8?tag=27)

## Phase 8: Design the landing page

The landing page is its own product. Treat it that way.

Same workflow, smaller scope. New Moonchild project. New brief. New design-library (the landing page should look related to the app but not identical). New PRD scoped to "what does this page do?" (sign up? read? clone the repo?). New style. Then build it the same way, Cursor and Codex, with the landing-page markdown files as input.

The thing landing pages need that apps don't: a preview loop that's tight enough to feel typographic decisions, because every word fights for the fold.

![](https://video.twimg.com/amplify_video/2059911452006432768/pl/3Mzbm7f8uIlqX5xv.m3u8?tag=27)

The iteration prompt I use most often when working on the landing:

The "options to consider, pick one and defend it" pattern is the most useful iteration shape I've found. It gives the model degrees of freedom without letting it hedge. You get a single committed change with a reason behind it.

## Build these skills for yourself

A "skill" is a saved instruction you can invoke by name. Think of them as named prompts. The right set turns ad-hoc prompting into a repeatable practice.

These are the skills I have for this workflow. Steal them.

- **brief.** Five answers in, full creative brief out. The input to every later skill.
- **design-library.** Takes references (URLs, screenshots, recordings) and produces design-library.md. Run once per aesthetic. Reusable across projects.
- **flow-from-brief.** Takes a brief and design-library.md and produces flow.md. Forces open questions to surface.
- **generate-prd.** Takes the brief + design-library + flow and produces spec.md. The contract.
- **generate-style.** Takes everything upstream and produces style.md. Same section order every time so the build phase can rely on it.
- **generate-architecture.** Takes spec + flow and produces architecture.md. Includes the state diagram, the invariants, the extension points.
- **build-from-md.** Takes all six markdown files and produces a scaffolded project. Same stack, same conventions, every time.
- **review-pass.** A Codex prompt template that reviews a feature against the spec and flags everything that drifts.
My first project took a weekend. The next one took an afternoon. By the fifth I was through the whole flow in under an hour, because every step had been reduced to invoking a skill.

## What to do when it goes wrong

It will go wrong. Here's the triage.

**The output is generic.** Your brief was generic. Add more specificity to the "refuse to look like" section. Vagueness in, vagueness out.

1. **The design system contradicts itself.** Common. The model defined Text, Muted as #9ca3af in the table and then used #a1a1aa in three component specs. Ask it to audit itself: *"Read style.md. List every hex value used. Flag any defined twice with different values."* It'll catch it.
2. **The code doesn't match the spec.** The agent inferred something the spec didn't say. Update the spec. Re-run the relevant phase. Don't patch the code. The spec is the source of truth, the code is downstream.
3. **The flow has too many steps.** A flow over five steps is two flows or a bad product. Split it or cut it.
4. **The app feels off but you can't say why.** Use it for two days. Note specifically what feels off, with timestamps. *"Tuesday morning, I tried to mark a task done and clicked the row instead of the checkbox. The row didn't respond. Annoying."* Specific complaints generate specific fixes. Vague disappointment generates nothing.
5. **Nothing the agent produces feels like yours.** The real failure mode. The only one that's hard to fix. The model can only return what's in your brief. If your brief was a list of products you admire with no point of view of your own, the output will be a competent average of those products. The fix isn't a better prompt. The fix is a stronger opinion

## What stays yours

Be specific about what *you* are doing in this workflow, so you can tell whether you're doing it well.

- The choice of accent color, #fb5607, was mine. The model offered six. I picked one. I cannot explain why. It felt right. That is taste, and the model does not have it.
- The decision that tasks should toggle when you click anywhere on the row was mine. The spec didn't say it. I noticed it was bothering me the third time, and I added it.
- The voice ("Good morning." instead of "Hey there!") was mine. The model writes either, with equal fluency.
- The decision to *cut* features (to refuse the weather widget, the Pomodoro timer, the calendar integration) was mine. The model always offers you more. Saying no is the job.
- This is the actual work in 2026. Not draftsmanship. Judgment.
The AI gives you a thousand reasonable options. You have to know which one is right. That knowledge doesn't come from a tool. It comes from caring, paying attention, building things, using them, hating them, using better things, and slowly developing a sense for what *good* feels like in your particular domain.

There is no shortcut. The tools won't give you one. The tools just remove every other excuse.

## The Steve Jobs quote everyone misquotes

*"Design is not just what it looks like and feels like. Design is how it works."*

People quote that line to sound thoughtful. They miss what Jobs actually meant. He wasn't praising thoroughness. He was naming a hierarchy.

The look and the feel are *downstream* of the function. If you design how it works correctly, the look and feel mostly fall out for free. If you design the look first, the function will fight you forever, and the thing will never feel right no matter how much polish you throw at it.

AI tools, used badly, tempt you to do exactly the wrong thing. They are spectacular at producing the look. They will give you a beautiful component library before you've decided what the components are *for*.

Resist that. The order in this guide is not arbitrary. Brief, then vocabulary, then flow, then PRD, then design, then architecture, then code. Why before how. Function before form. Always.

Taste, in the end, is refusing to ship the median.

The tools collapsed every other cost. They cannot collapse that one.

## Build the thing

The gap between *wanting a thing to exist* and *the thing existing* has collapsed to roughly the time it takes you to articulate what you want.

If you can describe it, you can have it. If you can't describe it (if you don't know what you want clearly enough to write the brief), then no amount of tooling will save you. That was always true. It's just suddenly the only thing that's true.

So: write the five answers. Write the brief. Run the prompts in this guide, in order. Save the markdown files. Hand them to Cursor. Let Codex review. Deploy.

Then build the next thing in a third of the time, because the skills compound.

The tools are gifts you didn't earn. What you do with them is the only thing that's still yours.

Build the thing.

## Appendix: the stack

Two tools do the work. The rest is incidental.

- **Moonchild.** Design. Everything from the brief through the final visual: design-library.md, flow.md, spec.md, style.md, architecture.md. One project, one thread, six artifacts.
- **Cursor and Codex.** Code. Cursor builds. Codex reviews. Cursor reads style.md and the rest of the design files directly, so the whole backend got coded without me re-explaining a single component.
Everything else is incidental:

- **Apple Notes / Notion.** For drafting the brief.
- **GitHub + GitHub Pages.** Repo and deploy. Free until you outgrow it.
- **The things AI is still bad at.** I hand-edit easings, microcopy, and any line that has to *feel* a specific way. The tools propose. I dispose.
The hourly leverage is absurd.

https://x.com/Av1dlive/article/2060035425574764708

— [Avid (@Av1dlive)](https://x.com/Av1dlive/status/2060035425574764708) · 2026-05-29 00:28
