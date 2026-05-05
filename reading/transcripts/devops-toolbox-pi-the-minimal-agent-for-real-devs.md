# Pi: The Minimal Agent for REAL Devs

| | |
|---|---
| Creator | DevOps Toolbox
| Published | 2026-04-24
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=OMFIPv8a4qA](https://www.youtube.com/watch?v=OMFIPv8a4qA)
| Generated | 2026-05-04

## Description

Junie is the only agent with 20 years of real experience: https://jb.gg/JunieCLI-devopstoolbox #ad
Massive thanks to JetBrains for their support in sponsoring this video!
---
Pi is different. It's not about a killer feature, or something that other's don't have, it's about NOT having most of these. It wins, by doing less.
- https://shittycodingagent.ai/
- https://pi.dev/
✅ Build a Second Brain With Neovim in Under 90 Minutes: https://learn.dotb.sh/courses/second-brain-neovim
✅ Zero To KNOWING Kubernetes in Under 90 Minutes:
https://learn.dotb.sh/courses/k8s-from-scratch
❗Use `devopstoolbox20` at checkout for 20% off!
⌨️ The keyboard on this video is the Dygma Defy: http://dygma.com/DEVOPSTOOLBOX
🎹 Keycaps on my Defy are made by 3DKeyCaps: https://3dkeycap.com/?ref=vxdqqmmo
⚡ Tech I use: https://kit.co/omerxx/my-battle-station

> Transcript extracted from YouTube auto-generated captions (`OMFIPv8a4qA.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Trigger warning. This video contains footage of a shiny new tool. Some viewers may find this content distressing. If that's you, rage comment without watching. If not, follow me.

What if I told you there's an agent powering the fastest growing project in GitHub's history, but almost no one had heard of it. An agent so good it's praised by Peter Steinberg, creator of OpenClaw, by one of Unity's creators, and by the creator of Flask and Ginger, who recently actually aquihired the engineer behind it. An agent so good where it ste is almost transparent. It doesn't have MCPS or plan mode. It's yellow by default and it just kicks.

Ladies and gents, meet the linest but meanest agent Pi. It's addicting to use.

Makes everything easy. Has a sea of extensions should you want them, including doing well this.

And if you give it a try, you'll find it's really hard to go back to anything else. Yes, there are many coding agents, but this one, this one's mine. I mean, not mine, mine. It's his Mario Zechner aka Bad Logic. An open- source enthusiast who had created one of the most popular gamedev libraries in the past. Mario hated every coding agent he tried from code through codec and yes, even open code didn't cut it.

&gt;&gt; I hate all the existing coding agents.

&gt;&gt; Pi's philosophy is adopt the coding agent to your needs instead of the other way around. It's stripped from fancy features. There's no MCPS, no sub agents, no plan mode, no background bash. So, if you're planning another vibe coding LinkedIn post saying you won't believe how the swarm of agents built another dashboard no one will ever need, Pi's not for you. What you can do, however, is extended in any way you like. Tools, commands, shortcuts, even 2y components. It's built for you to add things on top. With that, let's get into

Let's quickly describe the problem first, shall we? The issue started this project was the creators being fed up with bloat. In his talk, he describes Claude as this spaceship with an excessive amount of features &gt;&gt; at this feature and that feature and this feature and that feature. And eventually, Cloud Code is now a spaceship. It does so many things that you actually probably ever used like 5% of what it offers. You only know about 10% in total. and the rest the 90% that's left over that's kind of like the dark matter of AI &gt;&gt; and lastly there's zero extensibility if you want something added not an option now again open code user here but the criticism for open code itself in the same talk is quite interesting he talks about frequent session compaction making your agent dumber mid conversation silently erasing tool outputs past a certain amount of tokens and others there's also the LSP support in open code which I thought was great but apparently It's not the smartest way to iterate on code changes with an agent where it keeps getting immediate false feedback even before completing an implementation. And so Mario went and built his own extensible model agnostic very lean little agent. Shitty coding agent.ai or pi.dev for those of you who don't like the framing. The why is basically because it's lean but minimalism doesn't mean you can't do things. You can add any provider. I'll show you later how easy it is. Yes, Olama or other local providers work just as well. Amongst other few but interesting features, the context or the PI's system prompt rather is tiny. This is literally all it is. And sure, you can append, change, remove as you wish.

Yes, skills are available, including the ones you already have installed right now, which I'm hoping Doom isn't one of them, but if it is, let me know if you have this tattooed on. There's a dedicated website for packages which hold extensions of all kinds including themes prompts. But allow me before jumping in to focus yet again on what's not here. No MCPS, no sub agents, no permission pop-ups, and yes, it's yellow by default. No to the list or background bash. You want a sub agent, use T-Max or an extension for sub agents, but that's your choice. There's this awakening recently to the fact that agents aren't all this black magic. Not in the way these guys want you to think at least. I love this article by Thorston Bull who had already shown it's as simple as a 400line file and you can have your own relatively easy agent accurately describing this thing as the emperor has no clothes. Good. Off we go. NPM installed. Yes, it's JavaScript, TypeScript to be very specific because apparently that's the language for AI.

Who would have thought? Run pi and you're greeted with a beautiful unignorable warning. on your way to login. The slash list will also reveal that all your skills from cloud or open code or what have you is already available here as well. Login to your favorite provider. Mine isn't cloud despite this peak. A quick restart would reveal one reason why with a subscription I have anthropic don't care about that. And if it's not cloud code running here, the usage is built as extra. Yes, thank you Dario. Exactly the time for me to move on. You've tried AI coding agents before. They all work, but there are still few edges nobody's figured out until now. Meet Juny CLI, the first agent that actually thinks like a senior engineer, not a chatbot.

Other tools work, but Juny was built by Jet Brains. 20 years of understanding how developers actually work, baked into every suggestion, while other agents generate code that sometimes compiles.

New models are being released frequently and results vary significantly. It's the most reliable option for price quality ratio. Based on software benchmarks, the cost per problem is 10 times lower than competitors. That's not marketing.

That's verified performance. And here's the kicker. Juny isn't locked to one model. It lets you change things.

Complex refactoring. It picks the reasoning model on its own. Quick utility function. It picks the fast one.

You don't micromanage. The agent decides. But wait, there's more. Your context follows you everywhere. Start in the terminal, move to your IDE, deploy through CI/CD. Juny remembers everything. And if there's one thing that agents still don't do well is memory management. No re-expanding your codebase. No starting over autoMCP suggestions. Built-in code review for PRs test generation that actually works.

This isn't another wrapper around an API. This is a professional development tool that happens to use AI. Get started with Juny now. Link in the description.

Back to the video. For now, beside a super lean UI, as you can already feel, if for whatever reason, as people normally do, spam the context window with irrelevant questions or side quests, we can go back in time or fork the session and branch off from every single message in the tree, like so. It even helps with adding the text to the prompt in case we'd like to make changes. /tree, while not impressive in this specific session, gives you the hierarchy including commands ran while operating. While this may not seem like a big deal, having the ability to visualize and navigate through nonlinear session branches helps a lot with avoiding unnecessary side quests. These contaminate the context window, sending your agent into the dumb zone where hallucinations pile and mistakes frequency rises. At the end of the day, let's be honest, we don't know what it is that makes an agent better or worse.

I'm sure that the app your neighbor just built that solves all AI hallucinations forever is doing a fantastic job. Still, if there's one thing I think we can agree on is that context is king. Or to put it more accurately, context, both its quality, but also its size, will dictate how happy or frustrated you're going to be. Another nice addition is creating a gist of the session available both as a searchable HTML where you can follow the conversation or use the left pane to filter and see a breakdown or just get the same thing as a GitHub private gist should you want to share it from there. Also important to note a few important shortcuts to help you get started to which you can all get through the menu, but one that I only saw briefly in the UI that isn't mentioned here is sending your prompt to the editor by hitting CtrlG, which is something I find myself doing quite often for editing text properly. You can tag files to help with context management and be more accurate with responses. Similar to old cursor behavior, you're supposed to be able to use images and for whatever reason, for the life of me, I couldn't, unless the intended behavior is just pasting the path, in which case it works perfectly compared to open code. When you drag an image there, you see as if it's pasted an object, which even after reading the docs, I'm unsure whether it's just a path mask or a post can annotation.

Anyway, both work. I paste the image and it's readable. Moving on. If you're short on timac skills and must run ad hoc bash commands, you can also do that directly within the conversation using an exclamation mark. Ctrl D kills a session and it just pops back into the CLI as if you weren't even inside an interface. And I don't know why, but I love it. Pi- R will resume the session back to the same point. You can also run it with no session to not hold any memory of this session anywhere which in order to demonstrate what you can use that for. I'm going to get rid of the extra build to claude my open code zen or their new open code go subscription are both welcome here in pi. Just get an API key set it in the environment and there you have it. Cloud through open code which to be honest isn't different with the current situation but big pickle for example which is free is perfect for small queries. And yes, I know it's free because I'm the product.

Sure, I know that. And when you ask meaningless questions, a free model is a great option. While you can play with settings through the UI, they're all available through home.py/ aent settings JSON. That's also where you'll find the O credentials. Under sessions, you'll interestingly find a lot of JSON files holding these, which I remember Open Code just recently ditched in favor of SQLite for performance. and it did improve the experience with a snappier, fuzzy searchable list of conversations not yet in Pi. Now, I've been going around quite a few minutes covering what Pi has, despite the fact that everything I've shown was actually about what's between the lines, the things that it doesn't come with. There are skills, and we'll get there, but you probably noticed there was no MCP option, no other agents, special prompts, or approval gates. All of which are great stuff that you can easily add, integrate, or create on your own. But as is, it's just lin footprint, fast to respond, and easy to install everywhere.

You hit buy, there's a line, that's it.

Simple but powerful. And anyway, it's all about the model. All these agents are just a bunch of loop on top of them.

Anyway, so I did the first thing I think is required with these virtual toddlers, and it's having a file with some soft guard rails, so I don't have to repeat myself all that often. I have enough of that at home. As with any other great system, Pi got its own awesome Pi page from the community. Here's where I'm at with these things. I'm a big Open Code user. It has all the fancy features. It works great, too. But my initial curiosity with Pi was all about the linness, the absence of things rather than their presence. Overloading it with functionality just makes me wonder why would you? I can go back to open code or claude or what have you and get all the bells and whistles built by the team rather than a random unaccounted for community editions. One thing you do get out of the box though is prompt templates which for terminal users is just an alias but ready for you like adding review this code for bugs, security issues, etc. and wrap it under a slash command. That's it. If it's under prompts, it's a slash command that you can already use. It's simple, it's straightforward, and it's great to have.

Again, nothing fancy you haven't seen before. Pie packages are super easy to add if you want one of them, like the famous babysitter that should reduce hallucinations, but also slows down the process. A price I personally don't mind paying most times. So, pi install the path and that's it. The functionality is there. I can get more into babysitter another time if that's of interest, where it's relevant and where not so much. Comment if you want to hear more.

Lastly, a super fun option is the inline run with -p for process and exit gives you an ondemand query like finding a quick command for what's hogging port 8080. I can even take it a step forward.

Tell it to use some cheap model for these kinds of queries. PI list models give you the full name of everything you have available. Then once you've got it the way you like it, what comes next?

That's right. Alias, whatever. Like Q for questions or queries. And now Q high. Nice. You can keep asking your unbelievably important questions about the world and get free model answers you could have come up with on your own.

Isn't that the future? If I won't conclude here with whether I'm keeping it or not, I'll be cursed in the comments. So, as you've probably seen, pi, while really really cool and appealing, is more about what isn't there. And I know I'm repeating that for the 10th time in 10 minutes, but that's the point. And yes, I've been actually loving the linness, the simplicity, and the speed in which it works. So much so that I got a new home server. More about that later, but I made Pi run on it and do everything. Sure, I made it run on a well-designed MD file and context and instructions, which I keep making it update when necessary. And so far, it's done a pretty impressive job. I like the fact it's small. I can easily add stuff should I need them. And honestly, I don't find it extremely far from my open code usage. Even with my fancy sub agent system that I've built there, I'm doing pretty well with Temax here. adding fork sessions as needed. It's too early to say for sure, but Pi is a keeper. In the meantime, here's my full open code process used daily, both locally and for real production use. So, watch that video next. Thank you for watching. I'll
