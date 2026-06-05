---
status: processed
wiki: "[[WIKI/Agent Harness批判反思]]"
---

# thoughts on agent harness

when initially everyone had been building a workflow, it used to work well in a contained environment. it was brittle but it did its job when you are writing tests, occasionally, or often, failing in production. 

just for reminder, i will avoid technicalities here and assume u have been working on ai field long enough.

one example is langchain. why it didn't last longer and probably why we have shifted to langgraph. state, memory, persistence, and other buzzwords are interesting to use. it feels like work gets done. 

then came claude code - no one though you could do coding at terminal. well it also installs vm in your system, which helps a lot when you are running evals and also lets claude code do poc of its own given you know what you want. cool right? 

then everyone started to talk about openclaw - a rush to another gold mine. the idea was new, the product was unstable but it had the ability to do open ended tasks which are not coding. 

when i say harness, there are few problems i am always concerned about. 

coding is easy - you make sure your tools work, race condition is properly handed, and files are locked when it is being tinkered. when you ask your harness to fix a code, it can run grep commands, find the bugs and make fixes. well the fix can be in any of the file, it can be in any form. you don't care as long as it works. but that isnt how knowledge work tasks work. 

while the context matters, the quality of context doesn't matter in most of the open ended task. you can throw in anything at system prompt but as long as it works, it doesn't matter.

this is not how economically valuable knowledge work are be done in harness. context matters - the tool output the llm receives, the user intent, the size of system prompt, and how you write skills. 

so my question is, are we making harness which is economically valuable ? are the outputs produced can be used in high stake task without manual intervention ? 

we are not really there - because we love bloating. every project idea might start with good agent engineering but eventually drifts towards software engineering. you might be building a beautiful peace of software but if it isn't economically valuable, what is the point?

that's why i call harness a rich man's toy. it is fun to play with, seeing yourself burning $100 on simple tasks you could have done yourself, but then hey, my agent did that. that is butterflies and i would not deny the same. 

---

someone might want to argue on the usecases - that openclaw/hermes are different usecase than custom harness. i would disagree on that. the infra remains always remain the same, the agent layer never changes, you don't have to touch any of the code but still you might make them a valuable product worth selling. 

but then how? there are few bottlenecks. 

1. context needs to be efficient. when i talk about context, it is the skills, tool messages, mcps, but we are not there yet - because burning tokens is easier than making something efficient. O(n) or O(n^2), who cares - i got the work done. this is the current approach of the industry. 
2. you might not need to bloat with features. because of the features are not useful, not even for power users. it just looks fancy. but as a result it deprives someone with less compute to do their work using your software. what happens then? codex and claude code can absorb your features with better engineering and you are out of business. 
3. cost - while the api cost's are decreasing, it is not doing to be the same in long run. you burn vc's money to get a mrr and then you sneakingly start increasing your prices. because there is no free lunch. compute is finite and so is runway of these companies. 

---

to make my point for sensible, let me take a good-ish example. for retrieval and chunking (the classical rag), we have spend so much time building databases, rag strategies, and now we have so much infra that is costly at scale. you end up overbuilding a simple RAG pipeline. 

a lof of embedding, retrieval, and chunking problems can be solved using simpler models like RoBERTa - or you can fine tune them for usecases. there are maths that can help you do better re-ranking than pulling another re-ranking model api.  likewise a lot of structured extraction can be cost effective if you know your documents. 

if you know your game, things work out cheaply and efficiently. you dont need a million dollar infra and $3M in API credits.  

that's the problem with harness. it looks juicy on paper - for your VCs, and puts you to sleep. but that is not what it should be. there are simpler ways for solving hard problems but then nobody your pay you for that because you present a simple solution and the problem isn't looking like a mountain anymore. 

so what should be that is something simpler ? i don't know, i am not there yet. but i do believe i have a intuition on why it won't work. it might not be on paper or benchmark, "trust me bro". probably that would make it easy for you to discredit my views. 

but we will see and i would probably be right on the other side.

*for the love of the game.*

https://x.com/bythyag/article/2061335891910721929

— [kaffu (@bythyag)](https://x.com/bythyag/status/2061335891910721929) · 2026-06-01 14:35
