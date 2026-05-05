# AI coding agents are useless on large codebases. Unless you do THIS.

| | |
|---|---
| Creator | Jo Van Eyck
| Published | 2025-08-02
| Kind | captions
| Language | en
| Source | [https://youtu.be/UqfxuQKuMo8](https://youtu.be/UqfxuQKuMo8)
| Generated | 2026-05-04

## Description

AI coding assistants not working for you because your legacy codebase is simply too big? There's a way out! In this video I share a one-click and FREE solution that will give Claude Code a 30x performance boost when working in large codebases. And it's not subagents!
References
https://github.com/oraios/serena
https://github.com/dave-hillier/refactor-mcp
Timestamps
00:00 Introduction
00:45 Serena MCP
07:05 Introducing large codebase and DIY refactor
09:33 Naive Claude code
10:36 Claude code + Refactor MCP
14:35 Conclusions
📧 Stay up-to-date with my weekly agentic engineering newsletter: https://agentic-engineering-weekly.ghost.io/

> Transcript extracted from YouTube auto-generated captions (`UqfxuQKuMo8.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

these AI coding agents, they don't work for my context. My codebase is legacy.

It's too big. Uh these tools choke on the amount of code. That's what I hear a lot nowadays. And while sometimes that is the case, I think more often than not, it's just a case of using them correctly. Uh I like to compare that to eating spaghetti with a spoon. It's technically possible, but it'll go a lot faster if you use a damn fork. So, today I want to show you what kind of tools I use. and I will show you how to 10x, 20x, or even 30x performance of your AI coding assistants without even having to resort to sub agents or other approaches that throw a lot more money at a problem. So yeah, let's dive in. So the first tool I would like to show you is called Serena. And Serena does semantic search and edit. So it's a bit smarter than text files uh and brute forcing.

Uh the official docs are here. It's on GitHub. It's open source. It's free. Uh you can run it on your machine without paying anything. And the powerful features are uh semantic code retrieval and editing tools. So it's a smarter way to uh search for code or search through code bases and to uh edit code. So let's see how Serena performs uh on the original task we did the refactoring session a couple of videos ago. So this is the same codebase at the same start. Uh I have my docs and my doc tests and I want to refactor this and I already have cloud code up and running one of my AI coding agents and I installed the Serena MCP server as you can see and I already initialized uh the system by asking it uh to uh run in Serena mode. Uh it's all in the documentation if you are curious.

Uh so there we go. We are up and running. Let me grab my prompt.

Uh, I'm going to put the prompt here and we we can discuss it in a second, but it's basically asking to refactor it.

Um, I'm giving a bit more detail than I did in my original video, like what I want to happen. And yeah, that's it. So,

In my original run, it took 12 minutes to do this myself. Uh, so now, if you know what you're doing and you have like the tools necessary, let's see how far

So, I'm asking it to both refactor and add the new behavior at the same time.

And it's going to Yeah, it's going to build a plan. That plan looks good.

And as you can see, it's using uh Serena. Now it's doing search texts, but now it's doing like semantic searches, this find symbol thing, which is a faster way, especially in large code bases. This is a really mid small codebase, this ehop on web thing. So it's not really uh that impressive, but already uh in this example, we'll see what the impact is. And as always, you can follow along uh if you want to, but I have become a bit more hands off.

Now it's implementing the subasses.

Yeah, it already introduced the factory method. Did not push them push the methods down as uh we specified in the system prompt or in the prompt.

Okay, it now starts to figure out that it has references and now it's using the semantic search as you can see, not just a grap based approach. Again, small example won't matter. Huge legacy code bases matters a lot. Okay, now it's going to TDD the new test drive the new implementation.

We're rerunning our tests and we're running the full suite. So, it's probably done if there's no compile

It's going to run the full solution build to make sure.

This is the interesting part. Let's first see what it did on the test code.

Let's see that we don't miss the the

So yeah, I see a factory method I like.

are correct and it tests for the basic behavior. So it did everything what I would expect. Uh let's take a look at the implementation.

The only thing it did not do is push down the members. I obsessed over that in my previous video.

Yeah, that's not happening here. But other than that, it looks good. It uses a lot less tokens. So your uh weekly anthropic token budget will uh be way more. You you will be able to run a lot faster and a lot longer, especially if you use these kinds of tools.

So yeah, now we're just waiting on the build, but uh we are 5 minutes in and I'm not obsessing over the code quality.

Uh, we could try that, but I first want to have it like finish the job, which would be good enough in most circumstances. And as you can see, uh, it's faster than I would ever be able to do this.

Yeah. So, it's done in 6 minutes. That's twice as fast as I would be able to do this.

Okay. So, now I'm going to ask it to push down members to see if it gets there in the same amount of time. I asked you to push down all members of the dock type to the specific subclasses that use them. Please do that now.

So if this all like hits within 12 minutes, that means that I can go do something else, attend a meeting, coach a colleague, whatever.

Uh, so yeah, that is actually pretty impressing. Impressive.

Cool. We're already in unit test run.

Let's take a look at the resulting code.

Yeah, I love it. This is exactly uh the way I would have done it.

And we needed two prompts and an MCP server. And it's faster than I'm doing it uh than I would do it by hand. So that's pretty impressive. Uh so uh that's the first MCP server I wanted to highlight. uh if you find one for your language that does either embedding based uh searches or semantic search and edit like Serena, those really speed up things. It has to do less build, fail, compile, retry um loops and the token spend is impressively lower.

So yeah, that's the first tool. I did a lot of searching on GitHub for a big net codebase because that's the language I am most familiar with and I stumbled on Umbraco which is a content management system open source and it's pretty beefy as we'll see. So, I pic uh downloaded that one, cloned that one, and then I did a line count using the clock count lines of code tool. And as you can see, there's 55,000 files. And there are 80 842,000 TypeScript files and 360,000 lines of C. So, not huge, but large enough to prove our point here today. So that is the codebase we will be taking a look at.

So first let's do a benchmark again uh so we have something to compare to. Uh first I want to do it myself then I want to do it naively with cloth and then I want to start using the power tools uh so we can compare stuff uh super scientifically of course. So I am going to start the timer and I am going to just do a simple refactoring. I'm going to rename a heavily used type in a large codebase. So, I'm going to grab it.

It's the global settings type of this Umbraco thing. And let's rename it.

And now we're just using my IDE. So, uh we are using uh writer in this case, which has pretty good refactoring support. That's why I'm name calling it.

And as you can see in a multiund,000 lines of code codebase uh this happens but it's also not immediate. Okay. So we changed 19 files. No 147 files. So that's not nothing. And what I would do myself uh after doing a refactoring I would run unit tests. So I'm also going to do that. And that's super important if you use AI coding agents. So I'm going to include that as part of the experiment.

This does both a compile of the solution or most of the projects in the solution and run the unit test suite. And yes, this takes a while. There we go. So give or take 3 minutes to do this ourselves.

That is the benchmark we will be comparing against.

So let's get this party started. And let's first do another benchmark. So we're not going to give uh clot code any forks. We're just going to have it do this with a spoon. So, no additional tools, just the basic cloth code approach. Uh, this is a huge codebase as we mentioned. So, uh, it's going to be wild.

Uh, let me grab clot. Let me grab a prompt. Uh, yeah, yep.

There we go. So the prompt is asking it to rename a heavily used file in this codebase and make sure that the unit tests still pass. So nothing too out of the ordinary. I'm going to start a timer and we'll we are going to have to fast forward through this in the video in order to not bore you to pieces. Uh so yeah, here we go. Okay, this just took way too long to capture on video. So it got there in the end. It had to rebuild the code four times and it took three whole hours. All right. Now let's see how a smarter uh claude would handle this or cloud given some smarter tools at least and the tool I will be showing you or I would like to showcase is refactor MCP. It is an MCP server. It is fornet only but if you look around for the language you're using I'm sure you'll find one that does similar things. Uh but this one is refactor MCP by Dave Hillier and it's a simple standard IO MCP server. So it's a oneliner to add this to any AI coding assistant and it provides Roslin based refactoring tools for C. So just like Serena, it's a smarter way of navigating and editing especially editing code. In this case it's based on Roslin analyzers but that's not super important. What it is important is that it provides a server that exposes tools to do refactorings for you. So you can ask it to extract a method, move method, uh rename things, save, delete, extract types, all the things you would look for in a refactoring tool, they are here. Uh so yeah, I went and installed that. Let

and let's look at the MCP servers. So that's one is running. Yeah. So, it's running and I disabled all the other ones. And then we need to do one more thing, which is tell it to uh use this MCP server for refactoring. You could add this in the claw.md file or in the system prompt somewhere.

Uh still has to find the solution.

That's my bad. And then let's grab the refactoring prompt, which is the same rename as we did.

As you can see, this takes a bit of time.

And then let's start our timer because now we are starting the refactoring

And timer is running. Prompt is fired.

Codebase is clean. So yeah, we're good to go. Yeah. So the plan looks good. No need to tweak anything. Now it should be calling the MCP server to perform the rename. Yeah. So, it's calling the

which is also pretty a heavy process.

It's a huge codebase.

but it sure beats a doom looping. So, the rename has happened. Now, it should run the tests again. Yep. Let's take a look at what happened.4 143 files were changed. That sounds

And this is where like most of the time goes.

Once uh a coding agent starts doom looping and needs to like have multiple passes of fail compiles, change on files, fail compiles. So this is where your time goes if you don't give it intelligent tools.

All right, that's the compile done.

Now we are running our unit tests.

That takes about 30 seconds.

So it should get done in 5ish minutes.

Uh and I didn't have to intervene. This was a oneshot uh prompt. So I I just put prompt and it went on and did its thing.

So imagine this in a multi-step refactor process. Uh it's totally hands off. It's not as fast as doing it yourself, but it's like autonomous, which is pretty interesting uh as a result. So yeah, this is using the Refactor MCP server.

Okay, so now to some conclusions and to share some numbers to put things in perspective.

Uh these are the two runs uh I did with uh the large codebase simple rename and the one on the 31st. The first row uh is the brute force approach, the just do it claw approach. And as you can see it takes around 28 million tokens and like the cost estimate is somewhere around 14 bucks. And if you look at what uh changed when I used the MCP server, uh you see 1 million tokens being sent back and forth and only 60 cents. So that is a factor of roughly 28 times less token spent and a factor of 20 times uh cheaper and especially if you look at the time spent running this cloud code agent in the the default way. It took 3 hours to finish. It did finish. it had to recompile I think four times. Um uh but that's nothing compared or that's a whole lot compared to the five minutes it took uh when using this MCP server.

So that's also like a 30x improvement.

So please before using sub agents take a look at this stuff. So yeah, as you could see, uh, one or two MCP servers in your tool belt will make these tools almost as fast as you yourself would be and will, uh, improve these coding agents productivity by sometimes even 20 times, which is not something that should be ignored. So if you found relevant uh, MCP servers or tr tips and tricks to help these things working in a large codebase, please shout out in the comments below. And I hope to see you
