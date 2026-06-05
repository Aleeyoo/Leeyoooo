---
status: processed
wiki: "[[WIKI/StayInTheLoop]]"
---
# Don't stay out of the loop

The latest sentiment for how to be a better developer: get out of the programming loop. Become a shepherd of agents. You're slowing them down. Let them build the feature, make sure it works, and do everything needed to land the PR in your product.

The problem is agents do not have good opinions.

I am using them *a lot* recently, and I'm even starting to live in my coding agent OpenCode and only opening my editor when needed. I'm surprised by this. I've been skeptical that I don't need to micromanage every detail, but I've found workflows that make it work.

My editor may not always be open, but I still always look at the diff to read the code. This is why we made [a new diff viewer](https://x.com/jlongster/status/2057915791450812748) in OpenCode.

More importantly, I *always* have my product running beside my coding agent. I  can interact with it and see how the changes feel.

You know who is not staying out of the loop? Your users! Whatever you ship will be experienced by them. If you're staying out of the loop, how are guaranteeing a good experience?

A good example of this is while building the [diff viewer](https://x.com/jlongster/status/2057915791450812748), I had to solve the problem of keyboard navigation conflicting between the file tree and diff. If you press down, should the diff scroll down or should it move to the next file in the file tree? I made the file tree have a "focus state" so if you've focused that, you can navigate around it, and a key binding toggles focus state.

Initially the key for that was "f". I thought it made sense. Eventually I used the app and to focus the file tree I instinctively pressed "tab". Wait a second, of course! Tab should switch focus around, that's exactly how all web and native apps work. 

This applies to backend work too. Instead of user experience, the shape of the architecture matters. It impacts what features you're able to ship.

You could argue that eventually I would have used the app and discovered the "tab" key. A lot of times the changes aren't so simple: the later you find issues the harder they are to unwind.

You could also argue that agents give us the ability to unwind bad decisions a lot easier. So why look at the code? Just keep shifting it around until it works. But that's a lot of churn, and you might already have users depending on the bad thing, so it's hard to change. It's a lot better to just stay in the loop and make things good from the start.

https://x.com/jlongster/article/2058197974321070379

— [James Long (@jlongster)](https://x.com/jlongster/status/2058197974321070379) · 2026-05-23 22:46
