---
status: processed
wiki: "[[WIKI/提示工程师到全栈AI]]"
---
# How to go from being a prompt engineer to a full stack AI engineer:

![](https://pbs.twimg.com/media/HJVQGjUWIAQvhVr.jpg)

The worst take I have read is "prompt engineering is dead", it never died, but prompt-only thinking did. So I created a course to teach you to become an AI engineer and it's designed so that a beginner and an power user will be able to read this article, learn from it, and apply it to completely reshape the way you're utilising AI today.

Prompting is still the interface between you and the model.

- Bad prompts produce bad outputs.
- Vague instructions create vague answers.
- Missing context causes the model to guess.
But prompt literacy is scratching at the surface, so yes, I've taken the time to put together and create a full course to take you through the following so that you become an AI engineer. 

![](https://pbs.twimg.com/media/HJUmzlWXQAEs900.png)

## **1: Let's change the way you use AI today.**

I want you to go from thinking: 

> I have a task.
I type the task.
The model gives me an answer.
I judge the answer by vibes.
and instead I want you to think:

> I have a task.
I define the outcome.
I decide what information is needed.
I decide what model is appropriate.
I define how the answer should be structured.
I decide whether tools or retrieval are needed.
I decide what counts as success.
I check the result against a standard.
I improve the system.
then, as an AI engineer I'd want you to think one step further, this is where you're going to level up.

Let's say you want to create an AI newsletter, you'd go and design a repeatable system to utilise:

![](https://pbs.twimg.com/media/HJUoM0AWEAkVhtn.png)

So stop thinking "what prompt should I use now?"

Start thinking "what can I do to make this output reliable every week?"

## 2: Specificity

This is the first skill I want you to learn.

A vague prompt forces the model to fill in missing details. Sometimes it guesses correctly. Often it does not. The model is not reading your mind. It is inferring intent from the words and context available to it.

Old way

> Make this better.
Better way

> Rewrite this newsletter introduction to make it sharper, more direct, and more credible. Constraints: Keep the core argument unchanged. Remove vague claims. Add a strong first sentence. Keep it under 180 words. Make it sound like a experienced operator
You get the idea, specificity is important, and should be able to answer these questions:

![](https://pbs.twimg.com/media/HJUo72-WcAgRTid.png)

3: Roles

It's still important, why do you think Karparthy uses the following prompt with everything?

Because it fucking works as a role helps define behaviour.

Here's the model he utilises: 

![](https://pbs.twimg.com/media/HJUqsRQXAAUMEtl.png)

Here's a template you can copy and paste if you want...

Roles are useful to define judgement. Okay... we're quickly learning how to prompt efficiently which is the first stage to becoming an AI engineer.  If you feel you're comfortable with this please skip past 4, 5, 6, 7, 8 (screaming kids 67 67 67 67 67 67, sorry, it's engraved in my internal monologue lmao) and go straight to 9.

## 4: Using examples

I use these yo show the model what pattern I want, and this can help with:

- structure
- tone
- reasoning pattern
- categorisation
- output shape
- level of detail
- what to include
- what to exclude
Don't just say "write like my previous posts" instead:

Giving your model examples is important as this will become a test ground to ensure it gets it right.

![](https://pbs.twimg.com/media/HJUsg-yXcAMTf4P.png)

I use this to ensure it's not a lucky output, even OpenAI have guidance frames to ensure your model gets it right.

If I ever give the model bad examples then it gets poisoned.

If I ever give the model inconsistent examples then I get shit examples.

If I ever give contradictory examples over my instructions it will get confused and fuck it all up.

That's why it's important.

## 5: Reasoning

I used to do some dumb ass shit here.

I used to say:

"think step-by-step" and "show your full reasoning".

Brother ewwwww.

Reasoning models are designed to handle harder multi-step reasoning internally so instead of saying "show me every thought" instead say "reason carefully, then give me the useful audit trail" and voila.

All you need is:

- Final answer
- Key reasoning checkpoints
- Assumptions
- Uncertainties
- How I can verify this
And (I'll paste it for you here again) this is where Karpathy's prompt comes in clutch again:

Okay... moving on :).

## 6: Output control

I use output control depending on the task I have at hand.

If I can answer "who is this for?" then I can decide on the output I want given to me. Here's a table that I use and you can steal:

![](https://pbs.twimg.com/media/HJUzKtPXwAEy4fk.png)

OpenAI’s Structured Outputs are available through function calling and JSON-schema response formats; the point is not merely to ask for “valid JSON”, but to constrain the output to a defined structure where supported. Google’s Gemini API also documents structured output support, including use alongside tools in supported models.

Here's a JSON schema as an example:

Don't confuse a cool looking output with a reliable one.

Use the output for the job at hand.

## 7: Iteration & follow up

Does your model ever give you a perfect answer straight away? Mine doesn't lol.

Old way

> Try again. Make it better.
Better way:

So when you're wanting to make changes think:

![](https://pbs.twimg.com/media/HJU0ObRXYAIc5mF.png)

## 8: Constraints

Constraints are not optional.

They are part of the task. 

Why? The model doesn't know the boundaries, they need to be defined.

I used to get this wrong all the time saying:

"Do not sound like AI" or "Do not be boring"

This is crap, this is a better framework:

Avoid:

- “In today’s fast-paced world”
- “It’s important to note”
- "it's not X, it's Y"
- vague claims without examples
- em dashes
- motivational closing lines
Here's the boundaries you should be setting:

1. Style
2. Scope
3. Evidence
4. Authority
5. Safety
6. Output
7. Time
8. Quality
Here's a template I got for you:

When you are creating a workflow these constraints become guardrails for you, here's an example:

## 9: Context engineering

Prompting asks:

> What should I say to the model?
Context engineering asks:

> What does the model need to know to do the job well?
Anthropic describes context engineering as an emerging discipline for building steerable, effective agents, with emphasis on what information is made available to the model during a task.

If I don't give it my goals, preferences, examples, source material, constraints, scoring standard, or past decisions then it's going to guess for me and fuck it all up.

This will help you (it helped me):

![](https://pbs.twimg.com/media/HJU2PRsXsAILcNI.png)

Note: Context is expensive.

Overly long unnecessary context is going to dilute attention, increase cost, and create conflicts in your work. 

More context doesn't = better context.

Ensure you follow:

## 10: Retrieval

If the answer depends on facts, the model needs sources.

This is where prompt-only reliability breaks.

You cannot solve factual uncertainty with wording alone.

Make sure you're given it good sources:

![](https://pbs.twimg.com/media/HJU3OtaXMAIb1RQ.png)

If it's finding it's own then get it to rank them:

Retrieval means the system can search or fetch information outside the model’s internal memory.

This can include:

- web search
- file search
- database search
- document retrieval
- vector search
- keyword search
- knowledge-base lookup
- citation retrieval
**Retrieval checklist:**

Before using retrieval, ask:

- What question needs external evidence?
- What sources are allowed?
- What sources are preferred?
- How recent must the information be?
- Should the model quote, cite or summarise?
- What should happen if sources conflict?
- What should happen if no source is found?
## 11: Tool use and function calling

This is where AI starts doing things instead of only saying things.

Function calling lets a model connect natural language to external tools or APIs. OpenAI describes function calling as a way for models to interface with external systems and access data outside their training data. Google’s Gemini API also documents function calling as a way for models to determine when to call functions and provide structured parameters.

Here's the tool layer:

![](https://pbs.twimg.com/media/HJU5W2rWIAQF1ti.png)

## 12: MCP and connected AI workflows

This is how I connect X, Y, and Z, and by that I mean connecting to files, databases, APIs, tools and workflows.

Model Context Protocol is an open standard for connecting AI applications to external data sources and tools. Anthropic introduced MCP in 2024 as a standard for secure, two-way connections between data sources and AI-powered tools, and the official MCP docs describe it as a way to connect AI applications to data, tools and workflows.

Here's how to understand this part:

![](https://pbs.twimg.com/media/HJU541YWEAIAtFu.png)

Here's use cases:

![](https://pbs.twimg.com/media/HJU6EotWMAMN8hO.png)

Connected context creates risk.

The official MCP security best-practices page identifies security risks, attack vectors and best practices for MCP implementations.

1. Do not connect to everything.
2. Do not trust every tool.
3. Do not allow silent high-risk actions.
4. Do not assume tool descriptions are harmless.
![](https://pbs.twimg.com/media/HJU6aFOXIAAOV8p.png)

## 13: Task chaining, workflows, and agents

You do not need an agent for everything, in fact Anthropic’s agent guidance distinguishes workflows, where LLMs and tools follow predefined paths, from agents, where the model dynamically directs its own process and tool use. Their guidance starts with simpler building blocks and progressively moves towards more autonomous agents.

1. Use a workflow when the steps are predictable.
2. Use an agent when the steps must be discovered dynamically.
Workflows are fixed sequences, and agents have more autonomy.

So how can you decide when to use an agent or a workflow?

![](https://pbs.twimg.com/media/HJU6_XrWIAEbrAs.png)

Articles on agents (bookmark these for later):

Articles on workflows (bookmark these for later):

## 14: Testing testing one two three

OpenAI’s prompt-engineering guidance recommends creating evals to monitor prompt performance, especially as prompts become more complex or model snapshots change.

Karpathy's auto research does this on autopilot so I'd learn how to set this up:

## 15: Guardrails

Oi, don't start yawning. 

OpenAI describes this as the checks that run alongside agents to check on their inputs + outputs.

![](https://pbs.twimg.com/media/HJU-VDLXYAIg1vu.png)

Copy and paste this:

Analyse it and ensure there's a guardrail layer:

1. Identify the system’s purpose.
2. Identify what actions it can take.
3. Classify the risk level of each action.
4. Decide which actions are allowed without approval.
5. Decide which actions require approval.
6. Decide which actions are forbidden.
7. Add input guardrails.
8. Add output guardrails.
9. Add tool guardrails.
10. Add data/privacy guardrails.
11. Add scope guardrails.
12. Add cost/usage guardrails.
13. Add stopping conditions.
14. Add uncertainty rules.
15. Add escalation rules.
16. Create a copy-paste guardrail policy that can be inserted into the original prompt.
You don't want your agent deleting all your emails or something dumb.

## 16: Image prompting

I believe this is an important skill set as an AI engineer. 

Especially with how useful it is for everything you do.

OpenAI’s image-generation guidance describes GPT image models as capable of generation and editing from prompts, and its prompting guidance focuses on controllability, deliverable clarity and production-oriented outputs.

That means the prompt should describe the deliverable, not just the scene.

Here's a template to follow:

![](https://pbs.twimg.com/media/HJVAyGJXEAQ71EF.png)

## 17: Multimodal prompting

I no longer give my AI only text.

I also give it:

- screenshots
- charts
- PDFs
- spreadsheets
- images
- diagrams
- audio transcripts
- UI mockups
- code files
- documents
- slide decks
I then tell the model what kind of reading to perform.

![](https://pbs.twimg.com/media/HJVBw-BWAAEJwEa.png)

## 18: Building your AI engineering stack

I am proud of this section as this is where the course comes together because the goal is to build reusable AI systems.

Now the following can be an article itself so if you're tired.

Bookmark this post. Come back to this bit later.

I can't create this article on "becoming an AI engineer" and make it a 15 minute read.

Here's the full stack in action:

![](https://pbs.twimg.com/media/HJVMm4DXMAgR_hI.jpg)

How do you get started? First ask yourself:

1. What task is this system for?
2. What outcome should it produce?
3. What context does it need?
4. What output format is useful?
5. Does it need external sources?
6. Does it need tools?
7. Should it be a workflow or an agent?
8. How will quality be evaluated?
9. What needs approval?
10. What should be logged and improved?
Once you have that you can begin...

## Step 1: define the purpose layer

The purpose layer answers one question:

> What is this AI system actually for?
Most weak AI systems fail here because the purpose is vague.

Bad purpose:

> Help me with research.
Better purpose:

> Turn messy source material into accurate, structured research briefs that I can use to write articles, make decisions or create content.
Your purpose layer should define:

![](https://pbs.twimg.com/media/HJVNRuPWYAMQSF6.png)

**System name:**
[Name of your AI system]

**Primary job:**
This system helps me [do what task] by [how it helps].

**User:**
This system is for [me / my team / my clients / my audience].

**Main output:**
The system produces [briefs / drafts / analyses / plans / reports / images / decisions / summaries].

**Success looks like:**
A good output is [accurate / useful / structured / specific / sourced / ready to publish / ready to review].

**This system does not:**
It does not [send / publish / delete / spend / make final decisions / invent sources / operate without approval].

Example purpose statements

For research:

> This system turns messy notes, articles and source material into structured research briefs with clear findings, source hierarchy, uncertainty and content angles.
For content:

> This system turns raw ideas into publishable article drafts, social posts and visual concepts while preserving my tone, standards and positioning.
For operations:

> This system turns incoming information into prioritised tasks, summaries, decisions and follow-up actions, with approval required before anything is sent or changed.
For learning:

> This system helps me learn a topic by breaking it into lessons, examples, checks for understanding and progressive exercises.
## Step 2: define the prompt layer

The prompt layer defines how the model should behave.

This is where classic prompt engineering still matters.

But it is not enough to say:

> Act as an expert.
You need to define:

- role
- job
- standards
- decision rights
- boundaries
- uncertainty behaviour
Plug-and-play prompt layer

**Role:**
You are my [role].

**Job:**
Your job is to [specific responsibility].

**Primary standard:**
Prioritise:

1. [standard 1]
2. [standard 2]
3. [standard 3]
4. [standard 4]
**You are allowed to:**

- [allowed action]
- [allowed action]
- [allowed action]
**You are not allowed to:**

- [forbidden action]
- [forbidden action]
- [forbidden action]
**When uncertain:**
If information is missing, weak or ambiguous, you must [ask / state assumptions / give conditional answer / lower confidence].

**Reasoning style:**
Think carefully before answering. Do not provide a long chain-of-thought. Give the conclusion, key reasoning checkpoints, assumptions, uncertainty and verification path.

Prompt layer template

> You are my [role].Your job is to [specific responsibility].Prioritise:[standard 1]
[standard 2]
[standard 3]
[standard 4]
You may:[allowed action]
[allowed action]
[allowed action]
You may not:[forbidden action]
[forbidden action]
[forbidden action]
If information is missing or uncertain, you must [ask / state assumptions / flag uncertainty / provide conditional answer].When explaining decisions, do not provide a long chain-of-thought. Provide the conclusion, key reasoning checkpoints, assumptions, confidence and verification path.
## Step 3: define the context layer

The context layer is what the model needs to know to do the job properly.

Most people underbuild this layer.

They ask for high-quality output while giving the model almost no useful context.

The context layer should include:

![](https://pbs.twimg.com/media/HJVNfUMXQAcUvm_.png)

Plug-and-play context layer

**Goal context:**
The goal of this task is [goal].

**User / audience context:**
This is for [person / audience / customer / reader].

**Project context:**
This belongs to [project / business / content system / research workflow].

**Source material:**
Use the following sources, notes or files: [source material].

**Preferences:**
Follow these preferences: [tone, style, depth, format, examples].

**Constraints:**
Respect these constraints: [length, scope, platform, legal, factual, technical, brand].

**Decision history:**
These decisions are already locked: [decisions].

**Known failure modes:**
Avoid: [generic output, hallucinated sources, weak examples, verbosity, overconfidence, tone drift].

Context pack template

> Goal:
[What we are trying to achieve]User / audience:
[Who this is for]Project context:
[What this task belongs to]Source material:
[Documents, notes, examples, URLs, files or data]Preferences:
[Tone, style, depth, examples, format]Constraints:
[Rules, limits, exclusions]Locked decisions:
[Decisions already made]Known failure modes:
[What usually goes wrong]
## Step 4: define the output layer

The output layer controls what the model returns.

Do not leave this vague.

Bad output instruction:

> Give me the answer.
Better output instruction:

> Return a structured answer with summary, assumptions, recommendation, risks, next steps and confidence.
The output format depends on how you will use the answer.

![](https://pbs.twimg.com/media/HJVNrHeWUAQtZjY.png)

Plug-and-play output layer

**Primary format:**
Return the output as [Markdown / table / checklist / JSON / brief / report / draft].

**Required sections:**
Include:

1. [section 1]
2. [section 2]
3. [section 3]
4. [section 4]
**Confidence:**
Include confidence level: low, medium or high.

**Uncertainty:**
Flag anything uncertain, missing or assumed.

**Actionability:**
End with clear next steps.

Markdown output template

> Return the answer in this structure:Summary
Main recommendation
Key reasoning checkpoints
Assumptions
Risks
Next steps
Confidence level

JSON output template

Use this when another system needs to parse the answer:

Use this for stricter output control:

## Step 5: define the retrieval layer

The retrieval layer controls external knowledge.

Use retrieval when the model needs information beyond the prompt.

This includes:

- current information
- private files
- company documents
- research papers
- product specs
- laws or regulations
- prices
- schedules
- uploaded files
- knowledge bases
- previous notes
Do not rely on model memory for current or source-sensitive claims.

Plug-and-play retrieval layer

**Use retrieval when:**

- information may have changed
- source verification is required
- the answer depends on files, documents or external data
- the model needs evidence
- factual accuracy matters
**Do not use retrieval when:**

- the answer can be completed from supplied context
- the task is pure rewriting
- the task is brainstorming
- the user explicitly says not to browse or search
**Source hierarchy:**
Use sources in this order:

1. [highest-authority source]
2. [second-best source]
3. [third-best source]
4. [weak signal source]
**Citation rule:**
Cite sources for claims that are current, factual, disputed or high-stakes.

**Conflict rule:**
If sources disagree, explain the disagreement and prefer the highest-authority source.

**Missing source rule:**
If no reliable source is available, say so and lower confidence.

Retrieval policy template

> Use retrieval when the answer depends on current, external, private or source-sensitive information.Source hierarchy:Official documentation or original source
Primary research or regulator
Reputable expert analysis
Major publication
Community discussion as weak signal only
If sources conflict, explain the conflict.If no reliable source is found, say so.Do not invent sources.Include citations when factual claims depend on retrieved information.
## Step 6: define the tool layer

The tool layer controls what the model can do.

A tool could be:

- web search
- file search
- calculator
- code execution
- database lookup
- calendar access
- email drafting
- document editing
- CRM lookup
- image generation
- API call
- browser automation
- spreadsheet analysis
The tool layer needs a permission model.

The question is not just:

> What tools can the model use?
The real question is:

> What tools can the model use, under what conditions, with what approval?
Plug-and-play tool layer

**Available tools:**
The system may use:

- [tool 1]
- [tool 2]
- [tool 3]
**Use tools when:**

- [condition]
- [condition]
- [condition]
**Do not use tools when:**

- [condition]
- [condition]
- [condition]
**Allowed without approval:**

- [safe action]
- [safe action]
**Requires approval:**

- [risky action]
- [risky action]
**Forbidden:**

- [forbidden action]
- [forbidden action]
**Tool failure behaviour:**
If a tool fails, the system must explain the failure, continue only if possible and lower confidence.

Tool policy template

> Available tools:[tool]
[tool]
[tool]
Use tools when:the task requires current information
the task depends on files or private data
calculation or execution is needed
an external action is required
Do not use tools when:supplied context is enough
the task is pure rewriting
the task is brainstorming
Allowed without approval:read approved sources
summarise
classify
draft
Requires approval:send
publish
delete
buy
modify records
contact people
Forbidden:expose credentials
bypass access controls
invent sources
execute destructive actions without approval

## Step 7: decide workflow or agent

This is one of the most important decisions.

Do not build an agent by default.

An agent is not automatically better than a workflow.

A workflow is better when the path is predictable.

An agent is better when the path must be discovered dynamically.

Plug-and-play workflow template

**Workflow name:**
[Name]

**Trigger:**
This workflow starts when [event / user request / schedule].

**Inputs:**

- [input 1]
- [input 2]
- [input 3]
**Steps:**

1. [step 1]
2. [step 2]
3. [step 3]
4. [step 4]
5. [step 5]
**Human checkpoints:**
Approval is required before [action].

**Output:**
The workflow produces [final output].

**Failure handling:**
If [failure], the system should [response].

Builder note: workflow pseudo-code

Plug-and-play agent template

**Agent name:**
[Name]

**Goal:**
The agent is responsible for [goal].

**Available tools:**

- [tool 1]
- [tool 2]
- [tool 3]
**Planning rule:**
The agent must create a short plan before acting.

**Action rule:**
The agent may take one tool action at a time.

**Observation rule:**
After each action, the agent must update its understanding based on the result.

**Approval rule:**
The agent must ask before [risky action].

**Stopping condition:**
The agent stops when [condition].

**Failure condition:**
The agent stops and asks for help when [condition].

**Final output:**
The agent returns [output format].

Builder note: agent loop pseudo-code

## Step 8: define the evaluation layer

The evaluation layer tells you whether the system worked.

Without evals, you are judging by vibes.

That is not good enough for repeated or serious workflows.

Plug-and-play evaluation layer

**Success criteria:**
A good output must be:

- [criterion 1]
- [criterion 2]
- [criterion 3]
- [criterion 4]
**Scoring rubric:**
Score each output from 1–5 on:

1. Accuracy
2. Completeness
3. Usefullness
4. Format adherence
5. Source quality
6. Specificity
7. Risk control
**Pass threshold:**
The output passes if it scores [score] or above.

**Automatic failure:**
The output fails automatically if it:

- invents facts
- invents sources
- ignores the required format
- performs a forbidden action
- misses the core task
- gives high confidence without evidence
## Step 9: define the guardrail layer

The guardrail layer defines what the system must not do.

Guardrails are not there to make the system polite.

They are there to make the system controlled.

Plug-and-play guardrail layer

**Allowed without approval:**

- [safe action]
- [safe action]
- [safe action]
**Requires approval:**

- [risky action]
- [risky action]
- [risky action]
**Forbidden:**

- [forbidden action]
- [forbidden action]
- [forbidden action]
**Must flag:**

- [uncertainty]
- [missing information]
- [weak source]
- [conflict]
- [risk]
**Must stop when:**

- [stop condition]
- [stop condition]
**Escalation rule:**
If the system cannot proceed safely, it must [ask user / pause / return partial answer / request more information].

Guardrail table:

![](https://pbs.twimg.com/media/HJVO5MnWEAcQDgE.png)

## Step 10: define the logging layer

If you want the system to improve, you need to know what happened.

The logging layer records:

- what task was run
- what input was used
- what prompt version was used
- what tools were called
- what sources were used
- what output was produced
- how it scored
- what failed
- what should be improved
## Step 11: define the improvement layer

The improvement layer turns one-off prompting into an evolving system.

After each use, ask:

![](https://pbs.twimg.com/media/HJVPS5JWMAgcxgd.png)

Again, I direct you to utilise auto research to support with this (I asked you to bookmark it earlier in this article wink wink):

**OKAY OKAY!**

## 19: Summary

You've made it! You're now an AI engineer who has gone from only prompting to building better systems with an AI engineer stack.

You can now use the following checklists to ensure you have become an AI engineer:

Prompt quality checklist

- Clear task
- Clear goal
- Audience defined
- Context supplied
- Constraints included
- Output format specified
- Failure modes listed
- Success criteria defined
Context quality checklist

- Goal included
- Relevant background included
- Source material supplied
- Preferences included
- Constraints included
- Irrelevant context removed
- Conflicting context resolved
Output schema checklist

- Correct format chosen
- Required fields defined
- Optional fields handled
- Empty states defined
- No extra keys allowed where needed
- Confidence included where useful
- Errors handled
Retrieval checklist

- External facts require sources
- Source hierarchy defined
- Recency requirement stated
- Conflicting sources handled
- Missing evidence flagged
- Citations included where needed
Tool-use checklist

- Tool purpose clear
- Parameters defined
- When to use tool defined
- When not to use tool defined
- Tool failure behaviour defined
- Approval rules defined
- Logs captured
Workflow vs agent checklist

Use a workflow if:

- steps are predictable
- sources are known
- tools are fixed
- risk is low
- reliability matters more than flexibility
Consider an agent if:

- steps are unknown
- tool choice is dynamic
- open-ended search is required
- iterative planning is needed
- the system can be properly guarded
Eval checklist

- Normal cases
- Messy cases
- Edge cases
- Adversarial cases
- Clarification cases
- Refusal cases
- Scoring rubric
- Failure log
- Regression testing
Guardrail checklist

- Allowed actions defined
- Approval actions defined
- Forbidden actions defined
- Sensitive data handled
- Stop conditions defined
- Escalation defined
- Tool permissions scoped
Image prompting checklist

- Goal
- Deliverable
- Canvas
- Audience
- Subject
- Composition
- Style
- Text
- Constraints
- Iteration instruction
AI engineering stack checklist

- Prompt layer
- Context layer
- Output layer
- Retrieval layer
- Tool layer
- Workflow / agent layer
- Evaluation layer
- Guardrail layer
- Logging layer
- Improvement loop
## **BOOM. BOOM. BOOM.**

## that's 3 big booms.

[📹 video](https://video.twimg.com/tweet_video/HJVJpjTW0AQeSNe.mp4)

OH, THIS TOOK ME AGES. ON A DAY WHERE IT IS 27 DEGREES.

SO PLEASE. 

IF YOU ARE THERE.

READING THIS.

LEAVE A LIKE OR A REPLY OR SOMETHING PLEASE.

I AM FED UP OF THINKING I'M JUST WRITING TO BOTS.

https://x.com/hooeem/article/2059640615344754947

— [hoeem (@hooeem)](https://x.com/hooeem/status/2059640615344754947) · 2026-05-27 22:19
