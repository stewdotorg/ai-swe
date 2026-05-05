# I Found the Vim of AI Coding Agents

| | |
|---|---
| Creator | chantastic
| Published | 2026-03-26
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=XzZgLDL0wZY](https://www.youtube.com/watch?v=XzZgLDL0wZY)
| Generated | 2026-05-04

## Description

If Claude Code is VS Code, then Pi is NeoVim — a minimal terminal coding agent harness built for extensibility. I gave it 20 minutes. My Claude Code skills worked without changes. Multi-model switching took 30 seconds.
In this first look, I install Pi, authenticate with both Anthropic and ChatGPT, configure multi-model switching, explore session trees and sharing, test cross-agent skill compatibility, and install my first community extension.
00:00 Why I Put Off Pi for a Month
01:01 How to Log In and Pick Your First Model
02:23 What "Aggressive Extensibility" Actually Means
04:47 How to Use Claude and GPT in the Same Session
06:54 How Session Trees and Sharing Work
09:03 Your Claude Code Skills Already Work in Pi
10:50 How to Install Extensions (and Run Doom)
13:17 Testing Pi's Questionnaire Extension Live
🔗 Links
Pi — https://pi.dev (also: https://shittycodingagent.ai)
Agent Skills Standard — https://agentskills.io
Pi GitHub — https://github.com/mariozechner/pi-coding-agent
👋 I'm chantastic — https://chan.dev
Twitter/X — https://x.com/chantastic
#pi #codingagent #claudecode #ai #neovim #terminal

> Transcript extracted from YouTube auto-generated captions (`XzZgLDL0wZY.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

This is Pi. Pi is a minimal terminal coding harness designed to adapt to your workflows. It can be extended with TypeScript and you can bundle and share any packages that you use to extend Pi.

As a two-decade Vim user, I've been looking for something that feels a little bit more extensible, more personal, more built by me and less controlled by somebody else. For about a month now, I've had a desire to dive into Pi, but knew that if I got into it, it would just suck my life up and I've had way too much work to do. But today I've got about 20 minutes to get this thing going and we're going to learn as much as we can in that time. Okay, so first and foremost, I have Pi installed.

I used um bun to install this, so that may be a problem. I don't know.

Um and I just I think it looks for node and I just hard alias bun to node. So with it installed, uh let's run it. Pi, nice easy two-character command, already love it. And just like Vim, we get a bunch of instructions up front. But before we can do anything, we need to log in. So we don't have any models yet.

Uh let's try logging in. I have team accounts for both Anthropic and ChatGPT, so let's try that. Okay, looks like we authenticated and we should be good to go. Hello, you there? No model selected. Okay, so maybe we go into models. Oh, perfect. Okay, can I type in? Yes, I can. So I can filter these down a little bit. Uh let's do Opus 46. Why not? Go all the way.

Hey, you there? Yes. What can I help you with? Sweet. I love this. Tell me a little bit about what you do, Pi. RTFM, bro? I see you, Pi. I like it. Okay, so the core is to read, write, edit files, run bash commands. Uh we can get help with writing code, debugging, refactoring, reviewing, and navigating code bases, etc. Okay, cool. Now this particular part is something that I'm most interested in. I've been finding that I do a lot of local scripts. That's kind of the the way that I prefer to uh build these systems, but then I want some coordinator to kind of be the glue that that runs this, gets the data, and then figures out if it did a good job, and then moves on to the next step. And from what I've heard, this is what Pi excels at. Okay, key features, sessions.

Conversations are saved with full tree-based branching, so you can revisit or branch from any point. That's nice.

What makes Pi different? Pi follows a philosophy of aggressive extensibility with minimal core. TypeScript plugins that can add custom tools, UI, skills, prompt templates. Pi packages, bundle and share all of the above via NPM or Git. There are four modes, it looks like. There's an interactive mode.

There's a print mode. One-shot answers, great for piping. Oh, that's interesting. JSON RPC and SDK, embed Pi in your own apps. Now that sounds cool as well. The philosophy is Pi doesn't dictate your workflow. No built-in subagents, plan mode, or permission pop-ups, but I can build any of those as extensions. What would you like to work on? I'd like to build up a better process for producing video content in my nix darwin config directory and user skills directory. You'll see some efforts I've made, but I think building custom Pi skills would allow me to take this further. Familiarize with what's available and let me know what you think we should work on together. Okay, while that is running, let's take a look at the documentation and see what we have access to. Now right off the bat, these look very uh Claude-y. It's nice that we can open things in external editor.

That's cool. In fact, I might change my hyper G key to this if this all works out. There is an interesting alt enter though, which allows you to queue a follow-up. I think that's because the default enter will steer. Okay, so back on our help side, uh it it it looked at a lot of our skills, a lot of things that I've built over the past couple months that are in uh quite disarray. So it has an idea of um what I've built and where it thinks we should go. So uh you've assembled a seriously impressive video production pipeline.

Thank you, Claude. It describes a handful of the tools that I've built up.

These are currently Claude skills in skills. They reference subagents and Claude code skill name invocation, which won't work in Pi. What I think we should do. Port your skills to Pi format, highest value, lowest effort. Build a video pipeline orchestrator skill, high value. Okay, let's first take a look at some of the headline features. So we can apparently select a model and we can do that mid-session, which is nice. So let me get another model in here.

Uh we'll log in again, this time with our ChatGPT account. This may help hedge us from Anthropic's war against custom harnesses. Okay, so now I should be able to change my model, as we saw before.

Now I have GPTs available and then we can shift through them with control L, it said. Yes. Control L and control P.

Okay, so control P just kind of goes down them, goes down the model list, and control L lets us select it. Now it says favorites here, so how do I mark a favorite? Cuz it's definitely just going through them right now. Based on Pi documentation, there isn't a dedicated favorite models feature. However, there are two ways to something similar. So enabled models, filter models with control P cycling, or it looks like I can uh filter for certain models. And we can also set a default model. Uh sets the model that Pi starts with.

Okay. Okay, add this config for me wherever you need it. Make codex max. Uh what is that? 54 max uh my default and add all Anthropic 46 models. Make them available. My bad. Codex 54. All right, so we're good to go.

And that was stored in Pi agents settings.json. So now as we go control P, it still goes through all of them. So maybe we need to exit, reload. Okay, it loaded with our default. That's nice.

Looks like we have a thinking level default of medium, but we can change that with shift tab. So let's try that.

Eyes here. Okay, cool. Thinking level high, extra high, off, minimal, low, and medium, which was the default. I assume we can change the default. Okay, let's hit control P again. Okay, cool. We're getting our five three and five four models, um and then our Opus 46 models, and it's cycling there. Great. Oh, cool.

Perfect. Okay, pretty painless so far.

What are some other things? So we have tree structure shareable history.

Sessions are stored as trees. Use /tree to navigate any previous point in and continue from there. So let's uh I don't know if this is complex enough to have trees, but I think we'll need to resume a session. Let's see tree for this conversation. Looks like we can go back up to any point, so even the reads, which is interesting. So I can go to my X state reading. We can branch from there. We can summarize the branch or summarize with a custom prompt. Say, let's summarize it. I think it's wild that this summary is so unexpanded by default. So extremely so. All right, so this is just a summary of this particular step where it was evaluating the skills. Now it looks like we also have the option to export or share. So let's try um see if share is like a super command of export, so let's try that right off the bat. Now it's going to share a session as GitHub. So I assume I'm going to log in. No. Okay, cool. It's just It's just available.

Sick. So the gist is where it's stored, but we're able to view it through the Pi link. Nice. Okay, so we have system prompt, available tools. This is just a review of what it does. This is very nicely styled, actually. I like this.

Okay, so we also have a minimal system prompt and extensibility let you do actual context engineering, control what goes into context window and how it's managed. So we have an agents.pi that's loaded at the startup. We have a system.md. Okay, we can replace or extend that. Compaction, skills, and templates. Um looking interesting. Okay, something I'd like to do is So let's go back to tree. Uh let's I think go all the way down. Okay, so that's where our branch is, and then I think everything beyond that is here. Yeah, let's pick up here. Uh we don't need a summary this time. Okay, how about I use my uh user skills directory in Pi? Is there a universal format that you that would make them available across agents, or am I committing to Pi? Uh what Pi functionality would I miss out on if I made them cross cross-agent compatible?

Uh yolo. Woah, this is a big one. Uh so your skills are already in great shape for cross-agent compatibility. Here's the full picture. The standard agentskills.io.

So apparently this is a format, I guess.

Recommendation, the agent skills format was originally developed by Anthropic, released as an open standard, and has been adopted by growing number of agent products. Uh the format's already what you have. Yeah, yeah, yeah. I told I told Claude to write those. Uh your skills already follow the standard. How to make your skills available everywhere, universal path. Oh, interesting. So there's a universal path, I guess, as the global standard.

Both Pi and Claude code look there, so it's the cleanest setup. Uh right now I just have them in user/skills.

So that's interesting to know, but I can move them into agents or symlink them from there. Okay, so Pi has an additional setting that allows me to link to an array of skill paths. That's really nice. Okay, what do I miss out on going cross agent versus Pi only?

Honestly, nothing. Great, perfect. The only Pi specific thing is the skill name command syntax, but that's a UX convenience. The skill files are identical either way.

Okay, what actually needs fixing? Okay, so it actually went through and evaluated some things that need fixing, which is nice. All right, clearly I'm having trouble figuring out how to get a new line. So, I'll just do it this way. Yes to all, let's go. All right, let's see if there's anything else that we can look at. We talked a little bit about steering. Enter sends a steering message delivered after the current tool interrupts remaining tools. That's probably why we weren't getting a strong result because I wasn't actually invoking tools. It was just one uh one request. So, alt enter sends it as a follow-up after all the tools have run.

Okay, so we can build extensions in TypeScript with access to tools, commands, keyboard shortcuts, events, etc. We can even run Doom. Very cool.

Let's look at packages real quick. Dang, there are a lot of packages and it looks like they're just all right inside of the repo. Fascinating. Okay, so these look like maybe just ex- natural extensions inside the library or inside the program. So, we can add permission gates. I run everything in dangerous mode anyway in Claude, so I'll just be annoyed. So, we have some to-do list tools, etc. Questionnaire. I actually do like the interview mode or interview user mode in Claude. So, let's add this questionnaire extension. See how that goes. Okay, so let's see if we can load up an extension. So, Pi extension questionnaire.ts.

Do we need to run this whole path like this? Maybe there Maybe they're in examples. So, example extensions questionnaire.ts. No. Okay, so I think I need to actually copy these into an auto discovery folder. So, let's see if we can find the ques- questionnaire tool.

Clear our terminal. Well, screw it.

Let's just download it. We'll move from our downloads folder questionnaire to

extensions. That's our auto loading directory apparently. Okay, now we open up Pi. Let's see. Ask me some questions about me to try out the questionnaire extension. Maybe it loaded and so it registers as questionnaire tool that presents an interactive UI options.

Let me use it to ask you questions about yourself. Questionnaire extension isn't loaded yet. It needs to be in Okay, I must have Oh, I probably wrote agents.

Let me set it up for you. Great. Oh, confused. Okay, so it clearly doesn't like that this is not in a folder or a directory. Okay, we learned a new command though. Reload in Pi to hot reload the extension. Once you do, I'll see the questionnaire skill. Okay, so um I think that I probably just How is this directory structured? Yeah, it just looks like coding agents examples extensions questionnaire.ts.

Okay, now can you ask me questions? I think it's thinking of questions.

Extension still not loaded. You'll need to restart Pi not just reload. Okay, cool. Let's try it. Okay, we got a fresh terminal now. So, we're going to open up Pi again. Okay, ask me questions. I want to try the questionnaire extension.

Okay, cool. Nice. Okay, pluggable. I dig it. Okay, we're going to cancel that because yeah, it looks like it's working. Okay, and our other agent is fixing up all of our skills in

Our time's done for today, but that was fun. We learned a handful of things. I'm very interested in extensions, kind of finding the right extensions, but even using that to develop my own extensions.

That'll be pretty cool. If you're using Pi, hit me up. Let me know what you're doing, what you'd like to learn more about, and we'll we'll get through this together. That's it for today. I'm Jen. See you later.
