# Cut your LLM token bill in half with these 2 simple tricks.

| | |
|---|---
| Creator | Jo Van Eyck
| Published | 2026-04-12
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=R1na--yxl1s](https://www.youtube.com/watch?v=R1na--yxl1s)
| Generated | 2026-05-04

## Description

In this video we look at ways to decrease  token burn: small, practical tweaks that shrink your context without hurting output quality.
We cover having your LLM's talk to you like a caveman. No, really. And also wrapping all CLI tool calls to clean up cruft and log vomit with the help of rtk (rust token killer). We benchmark against a real software engineering task.
The result is clear: 38% fewer tokens wasted on a typical backend engineering task.
Shoutout to the craftymaker blog for the inspiration!
📧 Stay up-to-date with my weekly agentic engineering newsletter: https://agentic-engineering-weekly.jovaneyck.be/
Timestamps
00:00 intro
01:14 caveman
03:20 caveman benchmark
06:40 newsletter
07:08 rtk
10:30 rtk benchmark
12:24 discussion
References:
* https://github.com/JuliusBrussee/caveman
* https://thecraftymaker.substack.com/p/how-to-stop-claude-code-from-burning
* https://github.com/rtk-ai/rtk

> Transcript extracted from YouTube auto-generated captions (`R1na--yxl1s.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Me write spec, you code, no mistake.

Should you be talking like a caveman to your large language models?

Sounds silly, right?

&gt;&gt; [laughter] &gt;&gt; Actually, it does make a difference. And in today's video, I want to dive into some token efficiency techniques that you can use to do more with your Claude code subscriptions basically, because as we all know, &gt;&gt; [laughter] &gt;&gt; the rates have been uh lowering, subsidized tokens have been uh are going out, and there were even some bugs on Anthropic side where the rates got capped really soon. So, I do think it kind of makes sense to start paying attention to your token usage for your wallet one and two uh if there are ways to not boil the ocean or to like lessen the impact of what we're doing here, uh that's also interesting. While I do think you should be experimenting with these tools, we're all figuring this out as we go. Uh I don't think we should be boiling the oceans while we do it if we have like surefire ways like talking like a caveman. So, yeah, let's dive into some of these approaches, and the first one, as I already said, is &gt;&gt; [snorts] &gt;&gt; this silly GitHub repo. It's silly because basically it's instructing your coding agents to talk like a caveman. That's literally all it is. If you look at the skill file, this is the only sentence that matters. Respond like a terse smart man, all technical, no fluff.

&gt;&gt; [laughter] &gt;&gt; But actually, I did think it matters for two things. I don't think this repo matters. It's super big, and it's like a one-sentence instruction, so why is there a GitHub repo?

&gt;&gt; [snorts] &gt;&gt; But uh these are some impressive statistics, right? Just a basic coding agent, a genie like Claude, uh when it normally consumes 70 tokens, if you ask it to talk like a caveman, it returns 20 tokens, which is an impressive &gt;&gt; [laughter] &gt;&gt; uh decrease. Uh and of course, if you generate code, code can be a bit terser, but yeah, you have like syntax, and stuff has to compile, but if you are uh if you are used to seeing a lot of English text in your responses, this is a great hack.

And it's actually a hack I have been utilizing myself. So, yeah, let me first show you my super secret agents.md It has not changed for a year.

&gt;&gt; [laughter] &gt;&gt; And as you can see, the first paragraph, like rule number one I give all my coding agents, is basically the caveman rule. It's less funny. It's a bit drier.

Uh you could almost say a bit more professional, but actually, I think it's good to have a bit of fun with these things, because we're living in a world with enough problems and enough things encroaching on the fun parts of our job. So, if talking like a caveman with your coding genies is helping you, please do so. But anyway, I am asking it to be very succinct, to not explain code it generates, so basically to talk like a caveman.

So, that's in there.

And today, I will not be using Claude code. I will be using GitHub Copilot CLI to give it a bit of love, basically, because it's nearly as good as Claude code.

&gt;&gt; [snorts] &gt;&gt; Uh there's like a couple of bits of pieces that I'm missing, most important one being remote control to resume sessions or to steer sessions I kicked off uh remotely while I'm walking my dog or something, but uh yeah, this allows me to or this enables me to touch grass without thinking about my agent, so maybe it's not even a bad thing. But anyway, &gt;&gt; [laughter] &gt;&gt; uh we're going to ask it to work to implement something simple uh in caveman mode, and then without caveman mode to see the difference.

And we're going to ask it to implement FizzBuzz, a coding kata. Not very impressive. We'll go to a real example later. Uh but I'm also going to ask it to work very iteratively and incrementally.

This is not how I suggest or how I would how I would I would recommend working with these agents, because this is pretty slow, and it takes Yeah, it's not the ideal way I found. I take bigger steps.

And we can go into detail in a later video, but I'm asking it to I'm specifically asking it to take small steps, so we have a couple of compile and test runs in a single implementation of this kata.

It's not important or not super important for the caveman thing, but we'll go into some other things you can do to save tokens, and there they become more important. So, yeah, I'll be reusing this prompt for all our experiments.

And let's first do caveman mode to see

Let's see the context before we start.

And as you can see, I'm using 21K tokens, and the message uh part of my token or context window is empty. So, yeah, let's implement this

And as you can see, the dev ex and the UX is really similar to some other tools. Okay, it's done, and now let's

So, it used 3% of the the message uh or it used 5.8K tokens. So, we're using uh 6,000 tokens in caveman mode. Now, let's do this again, but without caveman mode.

So, I just changed my system prompt or my agents.md file, and I got rid of the caveman part.

&gt;&gt; [laughter] &gt;&gt; A pro tip, if you like put a a version in your agents.md file, and you say like echo the version uh whenever you're done, whenever you're responding to me, yes, it takes up a couple of tokens, and yes, that's counter to today's point, but at least you see that it's picking up new versions of your prompt, so pro tip right there.

Uh so, let's see whether or not that works. Okay, it appears to be done, so let's take a look at the context window.

And as you can see, it's 7.6K, so yeah, this cost us quite a bit more tokens.

So, for this small example, we'll do a big example later, but for this small example, does talking like a caveman work? It absolutely does.

Oh Jesus, it's hard keeping up in this rapidly changing a profession, right?

&gt;&gt; [laughter] &gt;&gt; Don't worry, I got you. I got a free newsletter where I do like weekly recaps for the stuff that matters. This week, for example, we'll do a dive into like is Mythos a big deal or not, the Anthropic new models. So, if you're interested in staying up to date, but don't want to wade through the AI slop yourself, I got you covered. Look for my newsletter in the description below. All right, so talking like a caveman kind of works, which is pretty cool and pretty funny.

&gt;&gt; [laughter] &gt;&gt; Uh but I got another treat for you, another way to efficiently use tokens or to not burn your tokens.

And it actually came from one of my colleagues, the crafty maker. I'll put a link in in the description. And it [snorts] was a birthday gift. It was on my birthday, so thanks, Kristoff.

And he blogged about RTK, the Rust Token Killer &gt;&gt; [laughter] &gt;&gt; uh app. And he was stating like, "Oh, this thing cuts token costs by 80%, 99%." And those are some impressive numbers, more impressive than talking like a caveman. So, of course, I had to check it out.

And when I dug into it, uh RTK is a binary, uh and it gets hooked into your Claude coder or into your GitHub Copilot using hooks.

&gt;&gt; [snorts] &gt;&gt; And hooks are basically whenever at certain life cycle events in a coding agent like running operations, or like things like, "Okay, I'm going to use a tool now." Or events like, "I'm done." You can hook into these events and have scripts running every time this happens.

So, RTK, which is this GitHub repo, the RTK from RTK AI, the Rust Token Killer, uh yeah, so this basically is that binary and some hooks that you can wire up. I'm not going to go into the installation for GitHub Copilot, because it's pretty self-explanatory.

Right now, it's a bit buggy, but I've seen that both the GitHub Copilot CLI team and the RTK team are on it, so whenever I upload this video, the problem will probably be gone. But I set it up, and I'm going to show you the difference in the exact same example.

But before I do that, let's first dive into what's actually happening with RTK.

&gt;&gt; [clears throat] &gt;&gt; So, it's a hook that catches tool calls whenever uh my coding genie wants to run tests or do a git diff or a git log, it intercepts it and does magic. And the magic that happens is uh Let's do a git log. So, as you can see, a git log is a pretty verbose thing, and each and every line gets sent to the large language model. So, this is a lot of tokens.

What uh uh RTK does is whenever you or your agents do a command like git log or when it compiles the code to runs the tests, it prefixes it, and it rewrites the command uh to an RTK command. And it's basically just prefixing it and calling the RTK binary. But what happens is it works. It's just the same. It's an identical tool call. But look at this. Look at the conciseness of it all. That's beautiful.

And this works for a lot of tools developers use, like Git, like Python, like pip.

And if it does not yet cover tools that you heavily use and that are very verbose, it takes a couple of minutes to set up some configuration or some plugins.

And it works for every CLI-based tool after that if you configure it. So, yeah, RTK is pretty easy to set up. And now let's see the impact. Okay, now it's time for a bigger example, more in line of how I work with these tools today.

So, nowadays I just like give them a story and I have some harness in place that makes this kind of work.

&gt;&gt; [laughter] &gt;&gt; Um but this example is I've shown it before in one of my previous videos. It's a pretty big code base, but it's a well-designed code base. And I'm giving it like a complete back-end feature to implement, like implementing some new behavior. It's an e-shop website and I'm asking it to like do some non-trivial things and I have some acceptance criteria and they those will get converted into tests.

But it's all not relevant. It's It's like this is how I work nowadays and I'm going to show you the difference RTK makes. So, yeah, let's first do without RTK and let's see how big the context window gets filled for one of my single sub-agents.

Let's ask it to implement a feature and let's see how big the context window grows.

RTK, without a token killer, we are at 19,100 uh tokens in our message window, in our context window.

So, yeah, now let's run this again with RTK and let's see how big a difference it makes for realistic use cases. All right, and it's done. Let's take a look at the context window.

So, previously we had 19,000 tokens. Now we have 12,000 tokens. That is an impressive drop and this is exactly how I work. So, will I be using RTK &gt;&gt; [laughter] &gt;&gt; going forward?

Hard yes.

Okay, so these were some experiments.

Now let's take a look at the final benchmarks and the comparisons, shall we?

So, these were the benchmarks we ran, only ran them a couple of times, did not do a full eval because I do not want to boil the oceans for my YouTube videos.

&gt;&gt; [laughter] &gt;&gt; Uh but as you can see, uh the caveman on the fizzbuzz thing uh resulted in a 24% drop of tokens, output tokens, so that's not pretty bad.

And uh RTK tool setup in a realistic back-end scenario where I work mostly resulted in almost 40% drop of tokens.

So, uh yeah, those are impressive numbers.

&gt;&gt; [laughter] &gt;&gt; Uh so, yeah, I will be talking like a caveman to my coding geniuses and I will be using tools like RTK to mind my context budget.

What are you doing to consciously like navigate token economies? Uh do you have other tips? Please let me know in the comments below and I would love to see
