# Building an AI Dark Factory:  A Codebase That Writes Its Own Code, Live

| | |
|---|---
| Creator | Cole Medin
| Published | 2026-04-14
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=Xg0tNz9pICI](https://www.youtube.com/watch?v=Xg0tNz9pICI)
| Generated | 2026-05-04

## Description

What happens when you tell an AI coding agent to write a codebase, review its own PRs, merge them, and keep going - with zero human code review along the way? I'm about to find out, in public, with all of you watching.
In this livestream I'm building the Dark Factory from the ground up, powered by Archon (my open source AI coding orchestration platform). The target is a real application people can use - a RAG-powered agent platform that answers questions about my YouTube content. But I won't write a line of its code. Neither will anyone else! The factory will.
Here's what we're doing live:
- Pushing the governance files that define what the factory can and can't do
- Running the triage workflow against real GitHub issues and watching AI decide what to accept
- Running the implementation workflow and watching a pull request appear from nothing
- Running the independent validation workflow — the part designed to stop AI from gaming its own tests
- Flipping on the cron orchestrator that makes the whole thing autonomous
This is built on top of recent work from StrongDM, Spotify, and Dan Shapiro's Dark Factory concept, and it's the most ambitious thing I've tried to build on stream. Some of it will break. Some of it will be weird. But this is the direction AI coding is heading and I want to be at the forefront with you!
Check out Archon: https://github.com/coleam00/Archon

> Transcript extracted from YouTube auto-generated captions (`Xg0tNz9pICI.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

dark factory today. And I know it sounds kind of spooky, but I'll start things off by explaining what that actually is.

This is going to be a little bit more of a casual live stream. So, I did a live stream over the weekend where I dove really deep into Archon, the new open-source harness builder.

And so, I'm going to be actually be using this a lot today, but I want to just do more like a live building session. So, I'll still like be sure to explain everything that I'm doing and have a good time for Q&amp;A and stuff, but it's going to be a little bit more like you guys just get to see the inside of a process that I'm starting here with a public experiment that I'm calling the dark factory.

And so, I've got my left monitor open up here with all of your guys' comments in the chat, and then my right monitor where I have my recording software, and then I've got my screen in the middle here. So, when you see me looking around, that's what I'm doing.

But yeah, this is just going to be like a a good like two two and a half hours I'm thinking of streaming for where I'm going to be building the dark factory in public with you guys, and even opening this up fully to the public in a couple weeks here. I'll talk about that means as well, but I I need to explain first what the heck a dark factory actually is. So, I'll I'll cover the basics for you guys. I think you'll find it really really interesting.

Because this is like the peak evolution of AI coding. Not that a dark factory is going to give you the most reliable results, but it is the way to get the most control possible to your AI coding assistants when they're working on a code base. So, the idea of the dark factory, it originated actually in the late I think like 1980s, 1990s. There were some companies in China that were running physical production lines with only robots. And so, the physical location only had robots in it, so they didn't even have to have the lights on, right? There's no need to pay for electricity when there's no humans operating in the facility.

And so, Dan Shapiro, I believe he's the first one that took [clears throat] this idea of the dark factory and applied it to code bases. So, using generative AI to completely manage a code base all the way from ideation, implementation, code review, merging pull requests, handling releases, like actually shipping the code as well. That is what a dark factory is. And I think the best way to explain it is to go through his article here.

So, he put this out just at the end of January this year. And he talks about the five levels of AI coding. A lot of us are at level like three or four right now. So, I I think like seeing where you're at and then like how far we can take things with the dark factory with his analogy will like really help you understand like exactly what the heck a dark factory is.

And so, yeah, let me go and share my screen here with this blog article. I actually meant to be sharing my screen earlier, so that's my bad, but yeah, so let's take a look at this together here.

So, this is Dan Shapiro's post So, the five levels from spicy auto complete to the dark factory.

And so, level zero, this is how I started using AI to help me code.

Probably the same for a lot of you guys that came from an engineering background. You were writing code yourself before, and then you slowly start leaning on AI more and more. So, I don't know why he calls it spicy auto complete, but hey, I love it, so I'm I'm for it. So, and in this [snorts] case, the analogy that he uses throughout the different levels is our control of a vehicle.

So, at level zero, we're still driving.

You can even think of it as like stick shift, right? Like super manual. We are managing the vehicle. And so, the AI coding assistant or even just like the large language model serves as a reference tool or an enhanced search.

So, it's like a smarter stack overflow if you guys have used stack overflow in the past. And so, here the developer manually writes all the code. We're just using AI as an advisor. So, like help me with this code snippet or give me an idea for how I can implement this, but we still are the ones hands on the keyboard writing the actual code.

So, hopefully most of us aren't at this step anymore. Um I know some people still are cuz that's what what they're comfortable with, and that's totally okay. Uh but then we go into level one.

This is the coding intern. So, you can think of it like cruise control. You still have your hands on the wheel, but at least AI is managing something or like the car is managing keeping you at a certain speed like 65 mph. So, here the AI writes the unimportant or boilerplate code. So, you're still doing most of the work yourself, but for the things that you don't require much trust in the large language model, you're starting to hand it over.

That's the coding intern.

And by the way, when we get to level five, I'll talk about how I'm actually building it myself. So, we'll we'll get there, but I want to kind of give the basis for you here. So, we then get to level two, the junior developer. This is the pair programmer. So, we start to relax a little bit. We only have one hand on the wheel instead of two. So, the developer and AI trade off control.

So, there are legitimately some more complex tasks that we are delegating to the coding agent, but not all of the time. We still are the ones writing the code a lot.

And then we get to level three. And so, this is, you know, like the self-driving cars now, right? Like you got hands off the wheel, but you're still paying attention to the road. And so, the AI is generating a majority of the code base, but you're still reviewing everything that the AI does. Like you're watching the road constantly, and you're going to be nitpicky. You're going to review plans, you're going to give feedback, you're going to review the code like the pull requests before you merge it.

You're always the bottleneck for verification before progressing. That's level three. And honestly, that's where most of us are. And you know, like when I teach AI coding on my channel and in the Dynamis community, level three is actually what I generally recommend. Because this is the furthest you can push it right now and still get the most reliable results possible. So, at level four, um this is where we get into the engineering team. When we get into harnesses for longer running tasks. And so, this is where you actually get to fall asleep at the wheel. So, you let the AI run unattended for long periods handling very complex tasks. And so, you think of harnesses like the Rel loop or Anthropic's harness giving your second brain the ability to handle issues and pull requests end to end. So, here you still are going to check the final results. Like at some point, you're going to you're going to wake up and just like make sure the car is actually driving you to the right place, and you know, take your put your hands on the wheel if you need to, but for the most part, you're trusting the coding agent to handle insanely long sets of work.

So, level four, I I wouldn't say this is like the most reliable at this point. If you want to ship the most reliable production code possible, you're still at level three. Cuz you're still going to monitor everything and be the bottleneck for verification, but you're starting to really take yourself out of the loop with level four, but there's still the steering wheel, right? Like there's still the opportunity for you to step in and fix things yourself or steer the agents in a different direction in the middle of some implementation.

And then that brings us into level five.

And I love the car analogy here cuz you like you look at level four, and there's still the steering wheel, like right?

There's still the opportunity for you to have control, but in level five, your vehicle looks like this, which someday we'll get there. That'll that'll be really cool. The day that I have a vehicle looks like this. But there's no there's no steering wheel in this vehicle. There's not even the option for us to take the reins if we want. That's what a dark factory is.

So, the engineer manages the goal in the system, right? There's still some kind of console here to provide higher level direction. We're still going to write the PRDs. We might manage the some of the releases, but we're not managing the code. So, we provide plain English descriptions, but the agent defines implementation, writes code, tests, fixes bugs, and ships. That is a dark factory.

And that my friend is what we are going to be building today. I have a good amount of the system already set up because I don't want to go through all the like really boring parts with you, but I I want to work on the workflows today. Like actually build the workflows that are going to manage the entire dark factory.

Because it's not enough to just point Claude code at a GitHub repo and say manage everything. Right? We have to teach it like how do we want it to handle issues? What kinds of features are we going to build into this application? How do we want to evolve it? What are the constraints that we have? How are we going to review code, right? Like we have to create workflows to define exactly how we want to write.

So, right? So, like the engineer manages the goal in the system. I'm talking about building the system here for this vehicle.

And so, um what I'm going to be doing, and this is this is the most exciting part for me, is I'm going to be leveraging Archon workflows to manage every single part of the dark factory. So, Archon is my first ever or it is the first ever open-source harness for AI coding. First ever harness builder, sorry. So, you think about like whatever your process is right now for software development, however you work with AI coding assistants, Archon allows you to build workflows to package everything up so that you can build any on any code base.

You can invoke any workflow.

&gt;&gt; [clears throat] &gt;&gt; Excuse me. You can invoke any workflow in parallel, and you get reliable results every single time because you're taking your process and you're packaging it up. That's what Archon gives us. And so, if you're interested more how Archon works and you haven't seen my content on it recently, there's a lot that I put out on my channel recently. So, there's a couple of live streams that I did, actually one just two days ago. Um so, I did it just on Saturday last week.

And then I also have a YouTube video. My most recent YouTube video where I covered Archon. So, I'm not going to get like super deep into an introduction to Archon today because I already have this content, but I'm going to be using it as a very critical part of the workflow, of the whole system for the Dark Factory because it's going to drive everything.

I'm going to build Archon workflows to manage my issues, build Archon workflows to write the code, review the code, manage the releases. I'll talk about what that looks like when I get into my plan here. So, I have this entire markdown document that outlines my entire plan for the Dark Factory. And I took a lot of inspiration from you know other examples of Dark Factories that are already out there on the internet. And so, maybe you guys have heard of the Strong DM use case.

So, Strong DM is a company that they actually manage a production codebase with a Dark Factory. They are shipping pull requests all of the time that don't have any human review at all. And their Dark Factory is unfortunately not open source like mine is going to be for you guys to see and watch it evolve in real time.

But they did share something like a PRD.

So, they have this open source spec for building their Dark Factory. They call it the Attractor.

And so, we don't have the codebase, but we do have the plan document that I guess theoretically you can use to have your coding agent build out exactly what they have for managing their codebase with no humans involved. And so, I did actually take a lot of inspiration from the ideas here. If you guys are interested in any of this, I'm planning on putting out a YouTube video tomorrow where I'll have like a more concise overview of everything and I'll have links to this all.

I can of course put links to this in the chat too if you guys are curious. So, um I will put a link to this blog post right here.

And then um I'll put a link to this resource covering the Dark the Strong DM Dark Factory. If you guys want to read through it.

Just want to make sure I give those to you guys cuz that's a lot of like my initial research for building a Dark Factory.

And I'm super fascinated by this. So, okay, let me be really clear here. I want to uh we clear on something and then I'll explain more of that the architecture that I have for my Archon Dark Factory.

I know I already said this, but it's worth repeating. The you're not going to get the most reliable results with your coding agent when you give it this much autonomy. I highly recommend when you're doing things for real, you put yourself in the loop, at least reviewing plans and reviewing code. Those two stages of the development cycle, I would at least have those.

So, this is definitely a public experiment. I'm calling it an experiment because it might fall completely flat on its face. We'll see. I mean, I'm going to put a lot of work into trying to make it really reliable, but I have no idea how the codebase is going to end up evolving when I leave it to literally manage itself. And so, the input for the Dark Factory is going to be a GitHub issue.

So, I as a user and then I'm going to make this public so you guys can literally submit GitHub issues as well.

It's going to be so fun when I get this really running.

And the input is a GitHub issue. And so, this is either a bug that we've noticed in the platform as we've tested it as a user or it's a new feature that we're requesting the Dark Factory to add into the codebase.

So, we create a set of GitHub issues like maybe there's you know 20 that are created in the last hour or something like that. And then on a scheduled basis, I'm going to run the first Archon workflow. I'm calling it the triage workflow. Because the responsibility of this Archon workflow is to look at all of the GitHub issues.

And it's going to judge the issues against the core governance layer that I'm going to build into the the Dark Factory repository. So, we're going to have our our mission for the repo and then the different rules. And these files are going to also include the scope that we're going to allow for the codebase evolution. Like here are features that we're definitely not going to allow. Here are types of features that we are going to allow. So, basically the triage workflow is going to evaluate all of the GitHub issues that were created in the last hour, you know, like everything that hasn't been triaged yet and it's going to figure out like you know, based on our mission and rules, what issues should we you know, put in a comment and close and say like hey, this doesn't apply for X Y or Z reason or here's an issue that we're going to go into the implement step. So, triage and then we're going to have a separate Archon workflow that goes through the full implementation. So, for every single issue that we decide we are going to address, we're going to invoke Archon workflows in parallel to do the full implementation.

And so, again leaning on the beauty of Archon here, we can handle any number of issues in parallel because it each of the issues are going to be handled in a a work tree. So, we have full isolation, full copy of the codebase for each one of the issues to be worked on independently without stepping on each other's toes. So, we implement and then for every single one of the issues that we addressed, either a bug that we fixed or a feature that we built, we're going to have a separate Archon validate workflow. So, this is where we do the PR review before we do the merge into main.

And one of the things that Strong DM implemented in their Dark Factory that's very powerful is what they call the holdout pattern.

So, the holdout pattern is the idea that we don't want the bias from the implementation to go into the testing.

And this is such a prevalent problem with coding agents in general is they will if you ask it to check its own work, it's like asking a student to check their own homework. They might you know, give a little bit of feedback just to seem like they're they are being critical of themselves, but in the end their bias is going to win out and they're going to stuff the bigger problems under the rug.

And so, what we do is a handoff. After the implementation, we go through all of our testing scenarios, but we don't tell the validation agent what was just implemented. So, we basically just do regression testing over the entire system. So, that way there's no chance of bias cuz it doesn't even know like what the issue was meant to address.

&gt;&gt; [clears throat] &gt;&gt; And that sounds kind of risky and like honestly myself, I'm not fully convinced of this, but that that is Strong DM's holdout trick and we're going to try building that as well. That's why we have a separate workflow for validation and we don't just have it built into the implementation.

Now, in Archon you can just like you know, start fresh sessions between nodes. So, I guess we could package this up as one workflow and still have the holdout pattern. Um that's one of the things I'll just kind of have to explore as I'm building with you guys. Like I'm setting a lot of this up from scratch.

Like this is a live coding session. Like I said, I'm not presenting something that I've already built like I did with Archon in the last live stream.

So, yeah, uh that's pretty much the workflow here. So, I mean really it just comes down to we have the governance layer. These are pieces of context we're always going to inject into every Archon workflow. And then we have Archon workflows to handle the full orchestration here. Triage, implement, validate, and fix.

And as far as the repository that this is going to operate on, it is going to be uh this one right here. So, obviously I'm not going to use Archon to work on Archon here. I don't want to turn Archon into a Dark Factory repository. It is too important that that this doesn't just become an experiment. So, there's a separate repo, a separate project that I'm going to build using the Archon workflows in the Dark Factory.

And uh it's actually a pretty cool um application. Like this is going to be a huge value add for my YouTube channel cuz essentially it's going to be a chat platform, an agentic chat platform where you can ask it any kinds of questions around AI and then it's going to perform rag. It's going to search through my YouTube videos to give you an answer.

So, it's kind of like your own AI tutor based on my content.

And then I was also thinking about doing something for the Dynamis community. If you are in the Dynamis community, you can connect your account into this platform. So, then not only would it search over my YouTube content, but it would also search over my workshops and courses that I have in the community.

So, it'd become like the ultimate AI tutor. So, it'd be like available to everybody, but then it'd be even like a nice value add to the Dynamis as well.

So, I'm actually really excited to build this out. Um and so, if the Dark Factory experiment doesn't work for whatever reason, um and like I have faith in it, but it is very risky. Like so, if it doesn't really like build on anything that like works super super well, then I'm just going to you know, build it myself with my usual AI coding process cuz I want to have this by the end of the month for everyone.

Uh but we'll see if the Dark Factory can manage everything cuz I'm going to work hard to build that initial AI layer. Um let me actually refresh here cuz I I already created the mission factory rules in claw.md.

So, I'm going to work hard to build out the the governance [clears throat] layer, build out the Archon workflows. I want

Now, the biggest risk that we have to this Dark Factory is that I can't actually use my Anthropic subscription because I would hit my rate limits way too fast.

And if I want to make this public so anybody can create GitHub issues for the Dark Factory, I believe that would be against the Anthropic terms of service if I let other people's GitHub issues get automatically consumed into the system, cuz then it's essentially other people being able to use my Anthropic subscription, because I'm not the one starting the workflow. It's like technically another user, I think. I just want to be careful there. And I'd hit my rate limits way too fast anyway.

So, I can't actually use my Anthropic subscription for the Dark Factory.

And so, what that means is I have to pay for an API.

I have to pay per token for everything I have built in this that I'm going to build in this Dark Factory.

And unfortunately, Opus would be way too expensive.

So, I have to resort to other means. And actually, what I came up with is I'm going to use MiniMax M2.7 as the large language model driving the entire Dark Factory.

Now, um I've done a lot of research for different models. I considered Qwen 3.5 Coder. I considered GLM 5.1. I thought about maybe using like Sonnet, but Sonnet would be too expensive. Um I was maybe going to use Sonnet through OpenRouter or just the Anthropic API directly. Haiku would definitely be not powerful enough. So, I I can't really use an Anthropic model, right? Cuz like Opus and Sonnet are too expensive. Haiku is like significantly worse than MiniMax M2.7, and they're actually a pretty similar cost, I believe. Um yeah, cuz like if I go to OpenRouter here and I search for Claude Haiku 3 or 4.5, it is um $1 for every 1 million input tokens.

It's actually more expensive than um MiniMax M2.7. That This It's less than a dollar. Yeah, it's only 30 cents for every 1 million input tokens. So, it's a third of the price of Haiku and way better. So, there's no reason if I'm not going to use my Anthropic subscription, there's no reason for me to actually use an Anthropic model, because Opus and Sonnet would be way too much.

And so, yeah, I I did a lot of research and this is what I landed on. And I can always switch the entire system, but what I've done here and this is part of the the setup that I've already taken care of automat or behind the scenes is I already have a VPS spun up where I have Archon installed. I have the rag YouTube chat application cloned and and configured.

And then I have my Claude code changed there to use MiniMax M2.7 instead of Anthropic. So, if I ask like here, give me the SSH command. I'll actually show you guys. I'll SSH into the machine live with you and show you what it looks like. So,

All right.

Cuz take a look at this. If I go into Claude, I open [snorts] up Claude while I'm SSH into this machine, you can see that it's using MiniMax M2.7. And if I say, you know, like what model are you, everything looks like I'm using Claude, but or that I'm using Claude code normally, but it is actually going and and using my MiniMax API key and running on M2.7.

And by the way, if I wanted to use like GLM 5.1 instead, it would be super easy.

Like I could swap over in like 30 seconds, because all you have to do to swap the provider for Claude code is you just have to change a couple of environment variables under the hood. Like And And by the way, this session that I have open with Claude code, this is on my local computer where I'm doing some setup locally. And what I had in the session, I had it read my whole Dark Factory plan. So, like everything you see right here, it has loaded into the context currently. So, I can just ask it here, don't show any secrets, but tell me exactly how I changed Claude code to use MiniMax M2.7 instead of Anthropic for the model.

Um so, I I might actually open source my entire Dark Factory plan. Cuz as I've been setting things up behind the scenes, I've just been like documenting everything in this like pretty massive file here, including all of the Archon workflows that I'm going to build with you guys live right now. So, I have a plan initially for every single one of these. So, we'll build them today and test them and maybe get to the point where we have the whole Dark Factory running. I'm not sure if we'll get that far, cuz it's going to be quite a bit of setup. Um but yeah, you can see that uh for switching the provider for Claude code, you just have to set these environment variables. There There's quite a bit here.

So, I think it actually would be pretty useful to share this with you guys. Uh cuz there were there was a It was a little bit trickier than I thought it would be to switch the provider. Uh because then you The other thing you have to do for Archon is you have to map the environment variables for the different model types. Cuz in Archon, you know, for each of the workflows, you can specify like for this node, I want to use Sonnet. Or this node, I want to use Haiku. And so, to make the Archon workflows work out of the box without having to change these IDs, you just have to map it through environment variables. So, like when I specify Opus in Claude code or in an Archon workflow, that maps to MiniMax M2.7. Same thing with Sonnet. And then if I want to use Haiku, then it maps to MiniMax M2.7 high speed, right? So, just a faster and cheaper version of 2.7. So, I have the same effect in an Archon workflow where I can make some things a lot faster, and then other things I have the most power I possibly can.

Um like if I typically would use Opus or maybe Sonnet.

So, yeah, if you are interested in using Claude code with a different provider, it's actually quite easy to set up. So, you could have it use OpenRouter if you want to use the Anthropic models there.

Um GLM has an Anthropic compatible endpoint.

Um of course, MiniMax does as well. I think Qwen does. Does Qwen have a Claude code compatible endpoint? And if you want to run local models with Claude [snorts] code, you can use Ollama as well. So, Ollama has a direct integration with Claude code. So, it's kind of cool, because we are planning on adding Pi support this week for Archon.

So, you can run all of your workflows through Pi agents.

And but even right now, you are able to immediately switch Claude code to use any models, so that you don't necessarily have to lean on another provider, but you're not stuck to using your Anthropic subscription or the

Okay.

Um let's see.

Yeah, I don't think Qwen has Okay, but like GLM and MiniMax have the Anthropic compatible endpoints.

So, yeah, just ask Claude to do some research for you. See like which ones are compatible with Claude code, but most models you're able to use directly,

See what it says here. Yep, see yeah, GLM has a first-party Anthropic compatible endpoint. So, it's like literally their API endpoint, and then you just add Anthropic at the end. So, that that's what you hit when you want to make it compatible with Claude code.

So, pretty cool. It's It's pretty easy to set it up.

All right.

Cool. So, uh yeah, I want to get into building with you guys here.

Let me check the context. Maybe I'll spend a little bit of time to answer you guys' questions. So, I know I haven't really looked at the chat yet.

Definitely want to do that. So, I'll I'll spend a little bit of time doing that, and then we can get right into building out our factory. So, going to the diagram here, I already have the governance layer built out. And then as I'm creating the workflows, I'll show you guys what that looks like. So, right now, I mostly just need to build out the Archon workflows.

And it's important for all the Archon workflows to like actually ingest these documents here. Like it needs to know like no matter what workflow I have running, if I'm fixing something, validating something, whatever, like it has to always know the mission and factory rules, cuz that's going to be the primary guidance. And then I mean, of course, the global like the Claude.md as well.

So, yes, I am doing everything through Claude code, but I mean, you could easily make this Dark Factory do like you use Code X or OpenCoder Pi instead, but yeah, it's just what I'm using. I like using Claude code as a harness, even if I'm not using Anthropic models.

All right. Cool. So, yeah, I'll leave this up, and then I'll I'll go ahead and hit some questions from the chat here.

And the future begins. That's right.

That's what we're building. We're building the future here. I don't think that that the Dark Factory is a pattern that's reliable enough right right now, but if we refine the workflows enough, and if we have We'd probably need more powerful models as well, honestly, to like really make this production ready, but maybe Mythos is going to be the

All right.

I'm level -11 or -1, not from a tech background at all.

I mean, that's all good, cuz with how powerful AI coding assistants are right now, you can get to level three very very quickly.

And in fact, if you don't have a tech experience, you probably won't ever be level zero through level two, because that requires you to write code yourself sometimes. And so, these days when you learn how to leverage AI coding assistants and you're not coming from a technical background, you just jump right into level three.

My big recommendation with that though is to ask your coding agent a lot of questions as it's creating code for you, so that you can start to gain an understanding and like, you know, sort of become at least semi-technical

Um will this video be saved for later?

Uh 100% Lennart's, yeah. So, every single live stream on YouTube is automatically turned into a recording after. So, I don't even have to like upload it myself. And so, I think it takes a little bit of processing time, but like what we have right here, it says live right now, it'll be turned into another video just like my live stream this weekend. So, you just have to go to the live tab. It's not in the main video tab for my long-form content.

You just go over here

I still feel like I need to test things myself. I don't yet trust AI to test and uh especially ship. I'm with you there.

Yeah, like I said, level three is where it's at when you really care the most about reliability. Unless you've built some kind of level four harness that you really really have faith in, I would generally recommend sticking to level three. And then level five is like no one really knows how much this is really going to work at this point. And that's what I want to find out with you guys.

Like yeah, we have examples like strong DM, but they haven't they they've not really shared that much, right? And like a lot of it could just be like marketing hype.

So, that's what I want to figure out for you guys for reals. Like if we actually put a lot of effort into using Archon and uh building a lot of reliability through a governments layer, like how reliable can we really make it? And I'm excited to find out. We're going to do

Um you don't trust that if it's secure or more than everything is working. I mean, okay, so sorry, this is a question follow-up from the other one. But yeah, I mean, I I would say personally that like security is a big concern for AI coding assistants. They do introduce more security issues than uh seasoned engineers. Um they can valid they can fix them as well, but like first pass, they introduce more. Um and then yeah, just like generally that everything is actually working. Cuz okay, here's the other problem with AI coding assistants.

And then honestly, probably the main reason that level five might fail, it's not necessarily that the coding agent produces code that fails, but it's more that it's just not aligned with what you want to create.

And that's part of why I'm going to be focusing so much up up front on building the governments layer because I need to be very clear on the guardrails and the guidelines for the coding agent so that when it picks up an issue and implements it, uh first of all, I want to make sure the issue is actually aligned with what I want for the code base, for the evolution of it. And then also like when it does the implementation itself, it doesn't misunderstand what the issue is really getting at. Like if I open an issue because I want to improve the rag search, I don't want it to like change the front end. I want it to just like

All right.

Um I presume the dark factory can introduce bugs later down the line if proper unit testing, linting, and bug regression system is is set up correctly.

Um so, yeah, that's going to be one of the risks of the dark factory as well as there might be little problems that creep in that our validation doesn't catch and they might blow up in our face later on. That's one of the reasons human in the loop is so important for reviewing things is to catch those little issues that become a bigger problem later.

Um so, the dark factory could kind of be like boiling a frog in water, right?

Like the frog, if you put a frog in scalding hot water, it's going to immediately jump out of the pot.

And so, the analogy here is like if we have a massive issue in the code base right after implementation, the validation is going to catch that and it's going to address it. But if it's something that slips under the crack cuz it's not super apparent right away, it's like the frog that you put in warm water and then you boil it over time. And so, it stays there until it's dead.

&gt;&gt; [laughter] &gt;&gt; Right? Like that could happen to a dark factory. You have these these little issues that creep in over time. It's not big enough for the validate agent to catch it, but then they combine together to produce bigger problems or it's just an issue that compounds on itself and you're not there in the loop to find those things and correct those things.

So, it's certainly possible. And uh as much as we can, that's why it's it's important for us to build a very comprehensive validation process. Not just with unit testing and linting, but I'm going to be building in like full regression testing. Like every single time we handle an issue, I want the validation to use a browser automation tool to go through the rag chat application like just like a user would.

And um like test this whole interface and have different conversations and click on the sources that it sites and make sure that goes to the right part of a YouTube video. Like I it's got to be doing everything and constantly kind of like maintaining this list of features that it needs to test every time it's doing proper regression testing.

So, we'll get there. That's going to be probably one of the biggest challenges

All right.

Um also, it would be handy if you can suggest open source or free alternatives if available out there for the tools we're using. As a learner, those crazy bills make my heart sink. I'm with you.

I understand the pain for sure.

So, I I guess I'm curious like what tools you're referring to exactly. If you're talking about AI coding assistants, unfortunately, there's not really a a free alternative to something like Claude Code that's just as good as Claude. Like I was talking about earlier, I'm using Minimax M2.7 instead of Opus cuz it's a lot cheaper. It's still not going to be as powerful.

And if you really want like free AI coding, then my recommendation is to run a model in Claude Code through Ollama.

Like you can run Gemma 4, um Qwen 3 has some smaller good models. You're not going to get nearly as good of results as Opus though. But like it is possible to do AI coding free. You just have the expectation that um you can't do the same things that you can do with Opus or some other more powerful model like GPT-5.4 Codex in uh Codex, for example.

So.

Let's see.

Got only 1 year to experiment.

Oh, let's see. The comment above. I'm thinking of creating some kind of architecture for local businesses here so I can and provide them. So, got to be safe, easy to set up and manage for them and myself and I don't get sued.

I have I'm curious what kind of architecture you're talking about. Like if it's an agentic system or if it's um like if it's an AI agent or like for AI coding.

Gemini makes a good evaluator to run against Claude.

Yeah, that is one of the things I want to be experimenting in with Archon soon is building workflows that combine providers so that we can do like exactly what you're describing. Like Claude for implementation and then Gemini, a lot of people like using Codex for review as well just to to have a check on Claude. Um so, yeah, that's

Cool cool.

Was that Excalidraw in Obsidian?

Yes. My Excalidraw diagrams I always have in Obsidian.

So, I'll show you that really quick. If I go to the settings and go to my community plugins here, I use I just use the Excalidraw plugin by uh Zsolt. I don't know if I'm saying that [laughter] right. But yeah, I've used this for a long time now.

Very very good. Cuz I just want to have everything in my vault together.

Diagrams, research, everything. So, I

Cool.

So, it little paywalled after all. Let's see.

Right. For AI coding, if you want to get the best results, it is paywalled right now.

Um and I mean, unfortunately, it makes sense. Like these frontier models like Opus and GPT-5.4 Codex, they are expensive, man. Like these companies are burning through billions of dollars of venture capital right now just to have these models running for the world and they're just starting to like make some profits from getting it like the higher levels of enterprise agreements.

Um cuz trust me, they they don't make money off of your Anthropic subscription if you are really maxing out your rate limits for Claude Code.

Where they're really making the money is using you as the lever to get the enterprise agreements, the enterprise interest.

So, they're they're profitable, but it's it's very subsidized for our Anthropic subscriptions. And that's part of why they're um jacking down the rate limits right now. It's kind of unfortunate. It's uh I'm not I'm not able to get nearly as much out of Claude Code with my Anthropic subscription compared to even like a couple weeks ago.

Um the rate limits are are worse now,

And they took away my 1 million token Claude Code. That's another thing that I'm kind of frustrated by. Um like if I if I do slash model, apparently some people still have this. So, I don't know why it's just me, but I don't have the option to use a 1 million Opus or or uh Sonnet anymore.

So, I'd be I'd be curious if anyone else has run into this as well.

But I am stuck to 200,000 tokens again as of like just 3 days ago.

I don't know why. Or maybe it was like a week ago.

But [sighs] yeah.

Anyway.

So, yeah. I'm going to have more time to answer questions as we are waiting for the workflows to be built here.

Uh one thing I want to put in the chat quick is just a link to Archon. So, if you're interested in checking out Archon, like got some good content on my channel.

Uh got the live stream from the weekend.

And then also I have the YouTube video that I put out just 5 days ago on Archon. So, check that out if you're interested. Like I said, because I've already done so much content, I'm not going to be like hyper focused on explaining Archon. I'm going to get quick pretty quick here just into doing [snorts] a live coding session creating the Archon workflows with you guys.

Um and then also the repository that I'm using the Dark Factory to build, this is private for now.

Um well, no actually I made it public.

But, I'm not going to make it so that you can give any issue to the Dark Factory yet.

So, I'm going to start by having it only accept issues from me.

And um then I'll make it so it's available to everyone after I've like tested things for about a week is my plan. Something like that.

In fact, I I might actually want to make this repo private right now.

As I build this as a safety measure.

We'll see. I I might have to switch it private until I'm like confident that it really is only handling my own issues.

&gt;&gt; [laughter] &gt;&gt; So, we'll we'll have to do that in a little bit. But, let's start by building the Archon workflows here.

Okay.

So, in my chat, how much do I have?

Okay, I have 40% of my context used. So,

So, again this conversation that I have, it already has my full Dark Factory plan loaded.

And so, I can literally just ask it like what should I build next? Because I've been keeping a log of everything that I've created as I built it. So, like for example, one thing that I already did is I created the core governments uh doc governance documents. So, I have like my mission.md, factory rules. I can show that to you guys like a little bit of what went into that um as the workflow is building. I just want to be efficient

All right.

So, there's some things that I was doing during an event in the Dynamis community.

And then uh here's the remaining punch lists from section 14. I told you it's comprehensive. Section 14 of the plan.

Uh ordered by what makes sense to build now and save for live.

Okay, so I guess there's a couple of things that it recommended that I do before the live stream here.

But also these are going to be super quick to set up anyway.

We need to create the GitHub labels, um the issue and PR template, orchestration shell script.

Yeah, I'm going to Yeah, I'm going to have it rip through all these things in parallel. So, okay.

The GitHub labels are actually kind of interesting because everything in the Dark Factory is going to be managed through labels. So, when the triage workflow runs, it's going to basically check on these labels as a status. Like this thing is currently being implemented, right? Like don't don't send off another Archon workflow to work on this because it's already in progress.

Or worst case scenario, if it fails to implement the pull request two times in a row, I'm going to have it labeled needs human.

So, maybe I'm not making this like fully fully Dark Factory, but I do want to at least have like a small escape if I really need to address something myself.

So, I have a little bit of a system created for that.

And then uh yeah, like if I reject an issue, like this doesn't fit with our mission, or if I approve it and we're not going into implementation yet, then I'll add that label.

Um and then factory rate limit, I'm going to have some protection to make sure I don't blow through like hundreds of dollars of Minimax credits in a day.

And so, if we need to wait for the next day for the rate limits to subside that I'm going to build into the system, then we'll add this label as well. So, yeah. It's a hard even in a live stream to get like super deep into everything that I planned here. But, I hope that you can see from the the label system that I have for these GitHub issues, like there's a lot of thought that I put into this system even handling like rate limits and human escape if I really really need it.

Um so, yeah. Really like the important thing here is the GitHub issues are driving the entire Dark Factory. Because any kind of input into the system for a bug that needs to be fixed or a feature that needs to be created, the input comes in from an issue. Whether that's me creating it or the Dark Factory itself creating the issue. Cuz we do also have a sort of feedback loop here where when the validation agent runs its regression testing, if it encounters any problems that are big enough to not just be fixed right then and there, then it'll create a GitHub issue and then go through that loop and then address that.

And so, anything that it catches in regression can just be more issues for it to fix autonomously.

And uh so, yeah. I mean like hopefully doesn't mean that we'll hit infinite loops of creating issue and issue and issue after issue, but might happen.

That's part of the experiment. We don't we don't know what's going to happen or what could go wrong. Um but that's why I have protections in place to make sure that it doesn't um that that just jack me up in credits.

So, yeah. Like when I I have my balance here, I I only put like 25 bucks to start. And then I've used like, you know, 87 cents in my testing so far.

It's pretty efficient overall.

But, I'm going to make sure that I um yeah, never have like too much like I'm I'm disabling auto billing.

So, that if I run out of credits, I just have to manually uh add credits and then I'll have the system that like detects when issues weren't handled cuz of rate limit and it'll pick it back up basically.

Um okay. So, I'm going to say uh so, I'll go into my speech-to-text tool and I'll say, "I want you to create the GitHub labels, issue and PR templates, and uh the orchestration shell script and cron entry on the VPS.

So, handle all these right now.

I'm actually in the middle of the live stream now. And so, I will just explain the workflows briefly as you do these things." All right, there we go.

It still thinks that I'm not in the live stream yet. So, I'll give it an update of of where I'm actually at.

&gt;&gt; [laughter] &gt;&gt; Claude code doesn't really have a sense

who's paying for all this quota context?

I'm paying for it. It's it's coming out of pocket, which is why I'm using something very cheap like Minimax M2.7.

Yep.

Okay. So, let's see. Let's go back to the Dark Factory plan. I want to show you guys a little bit of what the Archon workflows will probably look like. So, what I have here, these are very much rough drafts of the Archon workflow. So, when I actually build them in a little bit in our stream here, they might end up looking quite different.

But, when I was doing my initial planning with Claude code creating this whole Dark Factory plan, I also had it load the primary Archon skill. So, it knows how to build workflows. It knows the different parameters, things like that, how to use the Archon CLI. So, it created the initial draft for them.

And so, there are four workflows that we need in total. We have the triage workflow. This figures out what GitHub issues we actually want to address and it handles the labeling and things like that.

And then uh we have the implementation workflow. I have to Gosh, I have to scroll a while here. We have the uh implementation Wait.

I already scrolled past it. Where'd it go? Where's the header here?

There might be some malformatting.

Oh no, here it is.

Um

Does this do the fix as well? Classify apply decisions.

Oh. I think there might be a misordering here. So, okay. Anyway, we also have the validate PR workflow. So, this is what we're going to run that we're going to do the whole like hold out pattern for validation. We're going to run this on every pull request that's created from the workflow that does the issue fix.

I thought that would be the second one.

That's why I'm confused right now.

Um I'm not sure of that. Maybe that maybe it just misordered things. So, oh oh yeah, it did misorder thing. Okay, that's kind of weird. But, anyway, this this should be the second workflow. I don't know why Claude put it in the plan in this order. But, our uh next workflow is the one to actually fix.

Um No, it No, that's not it. This is This is a workflow to fix issues that happen during PR validation. So, if there there are any problems that come up when creating the pull or when reviewing the pull request, then we run this to address things and then push a new change to update to the pull request.

Um And then we have the comprehensive test. So, this is the regression testing workflow.

And this one is going to take a lot of tokens. So, I'm planning on running this one only like once a day or once a week because it's going to look through every single possible user journey. Every single way we can use the application testing every single edge case, making sure that it works automatically.

And then for any things that don't work, it's going to create a GitHub issue.

Right. So, every time we review a pull request, we are going to do a lot of regression testing, but it's going to be a more concise version of this workflow cuz this is going to be like pretty token heavy.

And then yeah, I guess the one thing that I didn't do is I didn't create the workflow for actually fixing the issues.

And I I sorry, I remember now why that's the case. It's because there's a a default GitHub fix issue workflow in Arkon that I'm just going to use or maybe make a little bit of an adaptation for. But then for all the other workflows, they have to be created from scratch. So I apologize like Claude kind of confused me here or I forgot the planning that I did with it, but yeah, we're going to creating the workflows in a second here.

Okay.

Cool. So all three tasks are done. We created the GitHub labels and the issue templates and then we created the orchestrator. So we're we're basically going to create a cron job that runs on our VPS every so often. And whenever this job triggers, it's going to uh basically prompt Claude to use the Arkon CLI to invoke the workflows. Like, "Okay, it's time to triage our issues." Or it's time to uh yeah, see this is the default one. It's time to fix the GitHub issue or it's time to validate the pull

So we can go to the GitHub repository here and actually check this out.

So if I refresh uh well, here let's go to an issue. And if I look at the labels for the issues, you can see that we have all these labels now. Factory accepted, approved, in progress, needs fix, needs human. And if we look in the dot GitHub folder, we can see the pull request template. We want to make sure that as the dark factory is operating, it has a set standard for what goes into every single pull request description and every single issue description.

Because being as consistent as possible is one of the best ways to actually make this reliable. So we have one template for when we're filing a bug, one for when we are uh you know, requesting a feature. So if I were to actually go and open an issue right now, uh it asks me, "Is this a bug report, a feature request, or should I just create it from scratch?" And we're only going to uh allow maintainers to to do this type. So if I click on a bug report, then you can see that it automatically populates this form that's defined in the template that Claude Code just built for me. So now we have some structure.

We're enforcing certain things because we want to make sure like if I'm going to have this as a public experiment where anybody can open a GitHub issue, I I need some kind of expectations set for what information you're providing. So we have these required fields. So that way there is actually enough context for the dark factory to address the problem. And if a template's not used, then I'm just going to instruct the dark factory to automatically comment and close the issue.

So we're going to be pretty strict on

All right.

Cool. So uh now we want to actually build our workflows here.

The thing is I'm pretty low on context, so I might start a new conversation to do this.

So I'm going to just say I'm going to build the Arkon workflows in a separate context window. So just go ahead and update the plan with what we've just done here.

And then I'll go into a new Claude Code session to build the Arkon workflows.

And while this runs, I can just open up a new Claude Code and do that. So let me close out of here, open up a new Claude, and I'm going to copy the path to the

I'll just put it at the start of the

And then the other thing is uh hold on. Let me close out of this. The other thing is I want it to load the Arkon skill.

Because I want it to the Arkon skill that I have uh it gives a full reference to Claude Code uh how to build Arkon workflows and best practices for doing so.

And so I'm going to say uh read the entire dark factory plan that I gave you the path to.

We are now going to work on building the Arkon workflows. And I want to start by building the triage, the dark factory triage workflow. So I also want you to load the Arkon skill.

Um so you understand all the best practices for building Arkon workflows.

Then I want you to give me a summary of your plan for the triage workflow, all the nodes, what models we're going to use, what the prompts look like.

I and I want to just like have a conversation here iterating on the ideas for the workflow before we actually build it.

All right.

So we're going to do a little bit of a pivot loop.

If you have gone through the Agentic Coding Course in Dynalist, you know what I'm talking about. We're going to do some exploration, some planning up front, and then we're going to create the workflow and test it.

I don't even really know how we're going to test it exactly, but I'll I'll ask for its uh recommendations once we have it built. Cuz I might need to kind of like get the dark factory set up incrementally.

Or maybe I need to like really run the triage workflow on the issues I already have in the repository here and just like see what kind of labels it adds and if everything's working. And then I'd probably have to ask it to also like undo all of its work so that we can still have like a blank slate of issues that aren't aren't uh modeled with yet.

So we'll see what we have to do once it

All right.

Minimax is designed to minimax out those credits.

I hope not. We'll see.

I'm down to switch to something else like GLM if I need to.

All right.

Auto billing it messed up last month, never doing it again. Okay, that's too bad. I will keep that in mind, probably

Um use the Minimax subscription, gives good value for money. Okay, cool. Yeah.

I don't know I probably like if I really want to scale this experiment, I don't think I can use any kind of subscription cuz I'll hit rate limits, but that's good to know.

I use Minimax on Ollama, testing it now.

Okay, that's very cool.

Yeah, I mean I'm using a the biggest version of Minimax. I don't think that would really be realistically self-hosted. Like if I look up Minimax

um yeah, they don't even offer you to install this yourself. It has to run through the cloud offering in Ollama.

But I I believe if you look up like um there are self-hosted options. You must be running something self-hosted, right?

If I just look up Minimax,

Oh yeah, so maybe you are running the biggest thing.

Cuz you I guess there's like local options.

Yeah, that's massive.

140 GB for the main thing. And then if I if we were to look at a Q4 quantization, oh wait, it's still 150 GB. That doesn't seem right.

But yeah, it's a 230 billion parameter model. I'm not running that on my computer, I'll tell you that.

All right.

Am I using local models for this? Nope, I'm using Minimax. Well, I mean you can host Minimax yourself, so it's an open source model, but I'm using it through

Uh thank you very much for the donation, Jared. Appreciate it a lot. Building a dark factory because even the machines refuse to work in light mode. Right.

That's a good one. That's a very good one. And thank you for the donation, I

Um when using Arkon, is it needed to have bypass permissions for Claude Code?

Uh yeah, cuz you're running the Claude agent SDK under the hood.

You it's meant to be fire and forget, right? Like you're not supposed to babysit Arkon workflows. There's human in the loop built in, but that's a different thing. So I would use it.

Like you can limit the permissions of Claude when Arkon runs through hooks.

And if you really want to like create a settings.json to manage permissions there, you can do that as well.

But uh yeah, usually I just do like YOLO mode when I'm running Arkon workflows and then I have like hooks that prevent reading from dot envs and removing directories and working outside of my designated codebase, like the work tree

Uh we have eight H100s, soon to get H200s. That is very very cool. I'm jealous. [laughter] I do not have an H100. Uh certainly not eight of them.

That's awesome. Yeah, you're going to be able to run Minimax for sure. I think you could even run GLM.

How big is GLM?

Um

GLM is 744 billion parameters.

If you have eight H100s, though, I you would I think you'd be able to run.

Can you run GLM 5.1 on eight H100s? I think you can.

Like that should be decent. Yeah, cuz each one is 80 GB of VRAM. You're sitting pretty pretty there.

Um I mean you might still need to

12.4 tokens per second on HH eight H100s. So it's not like super super fast, but uh that's still very cool.

All right.

I have not even a 100th of an H100.

Same. Yep.

&gt;&gt; [laughter] &gt;&gt; Those are expensive.

Yeah, so yeah, 640 GB of VRAM. So yeah, you you I think you would definitely I mean, you'd want to probably quantize to get good results, but still insane.

All right.

Cool. So anyway, let's go back to our coding agent here because it came back with a summary.

Okay.

I've read the dark factory plan section 4.2 on triage specifically and the Archon workflow offering guide. Here's my proposal design for the dark factory triage before we build it. So our goal here, this is important for us to all understand, is to batch classify untriaged GitHub issues against the mission and factory.

So these core files that guide the scope of work and things we're going to actually care about and things we won't.

And then apply the labels and comments deterministically.

So Claude makes the decision, or I should say Minimax makes the decision, but then we're going to have a deterministic steps in the workflow to automatically label and comment. Just to make sure that everything works the same way every time we run the triage.

So we're going to run the orchestrator or runs when the orchestrator detects open issues with no factory label. So the orchestrator is the cron job. It runs every so often and when it sees that we have things that don't have a factory label, that means it's a new issue that our triage workflow hasn't looked at yet.

So okay, we have five nodes.

We're going to uh fetch the issues in parallel.

Fetch the rules. So we're going to read in the mission and factory rules and then take a look at the pull request list cuz that'll also help us determine what is already in flight.

Which this doesn't really make sense because I think we should be able to rely on the GitHub issues alone to figure out what's already in flight. But maybe this is just like a bit of an extra safety check, so I guess it doesn't really hurt to have it. But anyway, so layer one will do our classification. So it says Sonnet here, but remember we have it configured to route to Minimax when you we specify Opus Sonnet or Haiku in the Archon workflows. So it's going to classify each one of the issues and then we're going to have a bash step. So a deterministic step that will take in the JSON array of decisions that we have from the structured output from Claude code and we're going to loop over the JSON and then use the GitHub CLI deterministically to apply the labels and then also the comments to the issues

Okay, so we we have uh bash steps. So we're not using AI for layer zero as well, right? Like we just run the GitHub CLI to search through all the issues.

We fetch the rules.

Get the open pull requests. It says

I mean, it's fine, but I guess we can keep it.

And then classify the plan explicitly calls out scope judgment against a written mission requires nuance. Haiku often fumbles or in our case the high-speed Minimax would fumble. That makes sense. Cost is low since we run on less than 10 issues per batch in one call.

Fair enough.

Um okay, so you can see that one of the things we support in Archon workflows is defining the exact output that we require from the model. So this is like structured output with more classic agents if you guys have dealt with that before.

But the point of this here is that when our coding agent goes through the classification process, we need a standard. I'm going to keep saying this throughout our livestream here.

Everything is all about standards for reliability. We need a standard for the coding agent. It needs to communicate in the same way every single time um how we're going to label the issue.

So it's going to output an issue number, the verdict, which is going to be either accept, reject, or needs human.

It's right cuz need human it that's our fail-safe if it has failed to address the pull request multiple times.

And the priority and then the classification, bug, feature, enhancement, chore, or docs.

And then we have the prompt skeleton as well. So just telling it like when it goes through the classification what it's actually doing.

Um and then what the script is the bash script is going to look like to actually invoke the GitHub CLI to label and comment on things.

And then it's got some open questions as

Um okay.

So let's go ahead and answer these questions. So a batch size of 10 or five, let's do a batch of 10. And then help me understand like if we have actually opened up like 30 issues since last time the orchestrator ran, is it going to loop in Archon or what does that look like?

And then should triage ever label without closing on reject?

Plan says close rejected issues though with explanation.

Um yes, we should definitely close issues when we reject them. Yep.

Priority labels on need human. Currently I apply them. Useful so you can see at a glance which human review issues are

Yeah, I think that they are definite definitely worth applying priority.

Also help me understand how is this triage workflow going to know that we need a human, right? Cuz like we talked about in the plan how once the workflow or the pull request has failed twice on an issue, then we would say need human. So like what does that look like exactly?

Okay. Man, these are some tough questions. Should I include a type star label or just lean on the existing GitHub issue labels? The plan doesn't mention type labels explicitly.

I'd add them. Cheap signal for later filtering. Sure, yeah, we can add them and make sure you update the dark factory plan with that decision as well.

Duplicate detection scope. Right now the classifier sees open PRs in current uh issue batch. Should it also see recently closed issues to catch repeat reports?

Costs more context but catches more

Um I would say we don't really need this cuz if we rejected an issue before, we'll probably just reject it again. So we don't have to spend the context to look through recently closed issues.

Um do mission.md and factory_rules.md exist in the target repo yet? The answer is yes. I did actually create them. They are on the main branch.

Uh target repo path, which repo are we building this workflow into? The dark factory app repo, right? Not Dynamos engine. That is correct and actually I will give you the full path to the code

Okay.

All right, woah, that's a mouthful.

It asked a lot of questions, but okay, this is good. This is good cuz this is this is going to be a lot of work. We have our work cut out for us when we're building the system up front because every single assumption the coding agent makes is potentially going to be drastic when we're at this level of leverage creating our workflows and governance documents up front. Like we have to be very very intricate here. So I know that it can it can seem like I'm spending a lot of time on this, but man, this is important.

Because if it doesn't understand how to label things right or it's not using GitHub issues in the way that I want it to, the whole system is going to fall apart. So I got to take time. I'm really glad that it's asking me a lot of

Cool.

I'm still playing with Gemma 4 on my AMD 395 for local.

Pretty cool. Yeah, Gemma 4 is good. Like it is legitimately a an impressive model.

Uh it is I yeah, the one of the most

Um ooh, I actually haven't heard of this one. Nemo strong cascade 2.

That's cool. Man, there's so many models local models I want to try out right now.

Um woah, the benchmarks actually look pretty good.

Live code bench pro. It beats wow, it beats quant 3.5 397 billion.

And it's almost as good as Kim K 2.5 a 1 trillion parameter model. Seriously?

Interesting.

Kind of want to try this one with the the dark factory.

Okay, well anyway, I'm getting a little

All right. Yeah, that's exactly right.

Quality work up front makes for some stellar loops and that is what I'm going for.

Okay, so let me address your two help me understand questions. No looping in the Archon workflow itself. The triage workflow is not a loop node. It's a one-shot DAG that processes a single batch and exit. Here's the actual flow.

Okay, so if if we do have 30 issues opened in between orchestrator runs, orchestrator cycle one is going to fire triage workflows, process 10, and exit.

Orchestrator cycle two sees work in the high priority lanes first. Only when those are empty does it fall through to the untriaged issues exist and run triage again. Okay.

Uh so okay, this makes sense. So the 30 issue spike does drain across three orchestrator cycles. So we do have to be careful because theoretically if there's like more than 10 issues and like created every single hour, then it is going to just kind of pile up and pile up. But I think this is actually okay because I don't want That would be unfortunate if there's more than 10 created every single hour.

That's going to be so much work for it.

Um Right.

So, that makes sense. We can always change the limit if we want.

How does triage know it to needs mark or it needs to mark needs human?

You conflated two distinct escalations in the plan. I want to make sure we're on the same page cuz they're unrelated.

Flow A is triage time needs human. When this workflow handles, the issue itself is problematic at intake. The classifier marks needs human when the issue is in scope but ambiguous, architecturally risky, a product judgment call,

I actually don't like that.

I don't If I really want this to be a dark factory, I want it to either close issues or handle them. I don't want it to create this graveyard of issues that need my review when I'm not planning on actually reviewing them. So, Okay. I want to be clear here. I don't want to review issues unless there have been multiple failed attempts to address them. And so, for flow A, we should just close these issues with a comment. Like, if it's ambiguous or architecturally risky or whatever, let's just make a comment explaining that and then close

Um and then I want you to check the code base itself to see if it has dot arcan.

I believe it does.

And then Okay, one design requirement I want your okay on. For the bash apply decisions node, I'm going to have to classify I'm going to have classify write its JSON output to the artifacts directory, makes sense, as a part of the prompt instructions, then I have apply decisions read the file with jq instead of relying on classify output substitution. The reason being the reason string will contain quotes, apostrophes, new lines, and emojis and arcons auto shell quoting um into a bash script is a foot gun waiting to happen.

I mean, I guess. I don't Is that really not a problem? I'll have to look into that separately cuz that it might have just identified like a something we might want to fix in arcon. But anyway, writing to a file bypasses the whole quoting problem. It's one extra line in the prompt and a jq recursive in bash.

Sound good. Uh sure, that sounds good for me.

Okay, man, it's getting specific, but that's good. Like, I appreciate how in the weeds it is right now. That's That's what we need.

All right. Cool. So, I think this is the last thing I need and then I can

All right.

What else we got in the chat?

Thanks, man, for your shared work.

You're very welcome. It is my pleasure.

I love doing this stuff live with you guys. It's so fun. Just sharing everything.

And you know, I was a little bit um I'm going to be honest, I was like a little bit hesitant to do this live stream because it's a bit slower than how I usually roll in my videos and live streams cuz I'm I'm building something live and I definitely at the stage where I'm taking something pretty slow.

Like, we're not we're not going to have, you know, massive payoffs constantly here. Um it's a slow and steady right, it's a marathon, not a race. That's what it is when we're building a system like this up front.

That was a lot of uh information.

Okay, confirm the execution plan.

Okay, makes sense. Scaffold arcon, write the workflow, validate the workflow, fix any validation errors, and revalidate.

This is great.

Uh well, actually, yeah, I'll just say this is good.

Go ahead. The other thing I was maybe going to ask it is like, what's its plan to actually invoke the workflow? Cuz when it does the arcon validate, that's just making sure the syntax is good. So, it's sort of like linting of the workflow. It's not going to run it yet and triage issues, but once it does the build, then I'll just have a conversation with it and ask it what its

All right. Cool.

I don't think we are too low on context.

Yeah, we should be good for it to rip this whole thing. I wish I didn't only

Can't wait till the Chinese firms use missile mythos outputs to train their models so open source local can really go stratospheric.

Yeah, I mean, I'd be down for that.

So, yeah, if they would use mythos for um synthetic data generation like they used opus, that that would be powerful.

They're probably going to. Screw the lawsuits. They're probably just going to do it.

&gt;&gt; [laughter] &gt;&gt; Yep.

All right.

Can I do a session on hermies? I assume you mean Hermes, like the new like kind of like open claw alternative.

Um I would consider it. However, I'm more of a proponent of building your own second brain versus using something like Hermes or open claw.

And you know what, while we're waiting for it to build the workflow here, I think this is a good time to chat about this quick. Let me actually um Hold on. Let me open up a page in my

I think this is the right link. Yeah, here we go.

So, one of the really, really exciting things that I did quite recently in the dynamist community is I did a 4-hour boot camp teaching you how to build your own AI second brain from a scratch.

And one of the things that I cover there is how you can take inspiration from tools like open claw or Hermes without having to build run it yourself.

There are a lot of risks involved in running your own or in running a second brain that's not your own application. There's a lot of security problems with open claw and Hermes, not just in like vulnerabilities in the code base itself, but even just running something that you don't understand with permissions for your agent that you don't also don't truly understand.

So, I'm a big proponent of building your own second brain from the ground up.

And that's exactly what I cover in this course. And then I also edit it down into a more polished 3-hour version that still has like a lot of the good like Q&amp;A in it. So, that's in the community as well as the third course for dynamist.

So, if you're interested, like my second brain literally saves me 20 hours a week, like no exaggeration. It's crazy.

Like, I've been running my business for about a year and a half now. Like, I know how long it takes for me to do a lot of these things that are like partially or fully automated for me now.

And so, that's what I want for you as well. That's what I cover in the course.

So, if that's if that sounds interesting, let me actually put a link to this in the chat for for YouTube quick.

We've had a lot of people joining the community recently. It's very exciting for the second brain stuff and also because of arcon. Uh a lot of people are going to the course and they're sharing their second brain, like their own architecture and how they're molding it for their use cases cuz I one of the things I cover in the course is like, here's how you build the foundation of the second brain, but then I also talk about how you can, you know, guide it to help you build your own integrations and skills and other use cases that you have for it, even getting into it being like very proactive for you, anticipating your needs. So, yeah, people are sharing their own use cases and things. It's really cool to see.

Like, I'm learning a lot from you guys even in the community itself. So, outside of just how I'm evolving it on my own. So, very, very cool. So, yeah, wanted to call it out really quick.

Uh let's see where we are at with Claude now.

Everyone wants to sell a course.

Well, I mean, I provide a lot of value. I stand by what I what I provide there. So, yeah, I don't I don't really appreciate that the the cursing in the message there.

Um but yeah, I mean, like I it seriously, there's a lot of value that I have and a lot of work I put into

All right. Cool. So, anyway, here is what we've got for the workflow. It already built the whole thing. It's actually faster than I thought, honestly. But I guess we haven't really done any validation yet.

Um let's see. So, we have our plan updated with the little bit of changes we made to the labeling system and the changes we made to our plan for the workflow.

Um and then Okay, so we create the workflow itself.

&gt;&gt; [sighs] &gt;&gt; Few implementation notes we're flagging.

But I don't want to spend like too much

Okay, I think that's fine. What's not done yet? Orchestrator agent.

Fix GitHub issue adaptation. Okay. So, suggestion for next step. Before building the next workflow, I'd smoke test this one end to end. Create the labels in the repo, file one or two test issues, and um okay.

That actually makes sense. But here here's the thing.

Um I would love to smoke test, but I already have some issues that I have in the repository. So, maybe what we could do is we could run the workflow to triage those issues, but then just delete the labels after so we can bring us ourselves back to a blank slate. So, I want to do the full test, but I I want to like get it back to the original state before I did my testing, if that makes sense.

Uh but you can feel free to like iterate on the workflow and everything before you go back to the blank slate.

Okay.

So, yeah, I wanted to like actually run it, but not like leave a mess of of triaging and stuff. Cuz this repo that I have right here, like I want to keep it pure, right? For like when I actually kick off the dark factory, like have all the workflows built.

Okay. And then someone asked for me to share the link for what I had open up in Chrome. This is the link right here.

Um cool.

All right. Thanks, Cole. This is awesome. I appreciate it a lot. Thank you very much. Thanks, Cole. Will join Dynamis.

Thank you. I appreciate it a lot. Yeah, I'll be happy to have you in the community. Watching at 5:20 a.m. from New Zealand. Well, thank you for tuning in so early. I appreciate that a lot.

Cool.

I I know it's a reference to shooting yourself in the foot, but I've never heard the term foot gun. Am I alone?

Actually, honestly, Chris, that's a good point. So, that's in reference to what Claude mentioned earlier.

Uh I guess I haven't heard foot gun, either.

I don't know. Yeah, I've heard I've heard shooting yourself in the foot a million times. I use that expression myself a lot, but yeah, I guess that's

All right.

Uh I definitely want to join Dynamis, but just bought a house. Fair enough.

Well, congratulations, Stuart, on your new home. While I'm trying to get everything operating free and local for a first run. Then once I have output, I can pay for upgrades. Sounds good. Yeah, fair enough. Yeah, congrats on the house. That's exciting.

You're joining as well? Very cool. I appreciate it. Welcome to the Dynamis community.

Thanks, Cole. This is super cool. I appreciate it a lot. Yeah, I I appreciate you guys uh finding interest in something where it's like a little bit slower pace as I have to like really spend my time ideating and and building the system up front.

So, yeah, happy to to build this in in public, so to speak.

All right. Cool. So, let's see. The workflow is currently in

Looks good.

Um you know what I I kind of want to do, cuz I'm running this workflow for the first time, is I would love to uh view the logs in the Archon UI.

So, let me actually ask it. I I don't have it started I restarted my computer recently, so I don't have it up and running.

Start the back end and front end of Archon.

This is how easy it is, by the way. You don't You don't even have to run the commands in the terminal or run the containers or anything yourself. You

Okay.

I mean, bun run dev's not hard to remember, but I just like doing that.

&gt;&gt; [laughter] &gt;&gt; All right. So, let's head on over to

Um it looks like the back end is still

Can you monitor the back end and let me know if it's failing to start?

Not sure. I might be on a wrong branch or something. Oh, no. Okay, I think

Um Why is my workflow not showing? I have these uh These workflows from way back in the Saturday live stream that I forgot to continue are still running. That's funny. So, they're still paused, but it's not showing up.

Where did it

Oh, okay. It did actually finish.

Huge win and a Windows bug. Let me break down what happened. All three parallel fetch nodes finished. Classifier ran in 30 seconds. Decision.json was written successfully.

The fetch open PRs node paid for itself immediately. It thought that the classifiers would have accepted seven issues that are already being worked.

What broke is the JQ uh failed on Git Bash for Windows. Oh, that makes sense.

Yeah. Okay, so let's see.

Okay, so it's figuring out how to fix here.

Caught another bug.

Okay. Now it's running the workflow again.

It's cool that it's iterating. Like little little blips that it's finding, but like I mean, as long as it's able to iterate, I'm happy with it. Um yeah, okay, there we go. So, now we can see the workflow. It's not running, but the one that just failed, it uh shows up here.

So, you can take a look at the logs uh from the failure.

So, yeah, everything is working as intended in the Archon web UI. We just need to fix the syntax for the the underlying workflow itself.

Cool. So, all right. Oh, now it's running again. Okay, very good. View the logs. Very cool.

Looking good. All right.

All right.

Till foot gun means that. Yep, today I

Eric said the courses in Dynamis are standing. I'm a career dev and seen a lot of courses. Dynamis courses are exceptional. Yeah, I appreciate it very much. I I appreciate you guys like sharing that, especially after someone comes in and just like has to say something mean for no reason.

Which I mean, I got thick skin. Like it's fine. I I understand. And And by the way, like to to that person who who was a little mean, like I do get it.

Like I understand that like everyone is just trying to sell a course.

Uh and and I can see how it just feels like I'm just fitting in with that crowd. Like I I get it. It's okay. I'm not just like living in a bubble where I I think that you're saying it totally out of pocket. Like I understand. But like at the same time, like I really do believe that I'm not I'm it's not saying I'm expecting you to do this, but like if you were to actually go through the second brain course, I I feel like you would take back what you said. Like honestly, I'm just going to say that.

Um but yeah, not not like I'm thinking I'm going to change your mind or anything.

All right.

Having never built anything before, but amazed how easy this is to learn if you think logically.

That's right. Yeah. And even just like slowing down and using Claude code to help you think logically. Like break it down for me step by step. Or like help me plan this and ask me questions. Like those kinds of things are are uh how you get the most out of Claude code.

Because it it can I mean, like large language models make mistakes. They're never going to be perfect, and that's why you need to align with them. But you can have them walk you through the alignment process, cuz they do a really good job at that.

Okay.

Uh cool. So, it looks like it ran end to end, and it actually closed a lot of issues here. So, three were accepted, and then seven were rejected. Okay, interesting.

Uh well, I'm curious to dive into that now.

Cool. Smoke dev said as a part as part of as a student of the course, I agree.

Dynamis is an absolute game changer for me. Lots of value. I appreciate it a lot. Thank you very much.

Uh All right.

Cool. So,

Cool. And that was fast, by the way. It didn't take that long.

Now, we we didn't run this workflow with Minimax. Let me be clear, cuz we didn't run this on the VPS yet. But we will get there. We're just testing it right now locally. In fact, I should probably test my or check my Anthropic rate limit.

Uh oh, it's not even that bad. Okay, we're good.

Here, I'll even share that on my screen here.

Let me duplicate and bring it over.

So, this is my limits right now.

Uh look at that. We've only We've literally only used 10% so far. So, not bad, especially without bad the rate limits are. Um Wait, what is this? Daily included routine runs? This is new, like as of just today.

Included routine runs per rolling 24 hours. I actually This This is weird.

I've never seen this in the usage page before from the Claude app.

And yeah, my weekly limit, I'm at already at 50%, and it resets on Friday.

And over the past couple days, I've been doing so much testing with Minimax that I haven't even been using my Anthropic that much. Like it's crazy. I I got to like 40% over the weekend.

Yeah.

Okay. So, anyway, I was going to look at

Oh, it already undid everything.

Shoot. So, I can't actually see

Okay, so this is one of the ones that was accepted. So, we can see that the labels are removed, cuz I asked it to undo things after it's testing. But we can see that from the history here that like this one, it added the factory accepted with a priority low.

If we look at um I don't know, one that it rejected here.

Where is this one? This one, it added factory rejected. Closes not planned. It had it it an issue comment here at one point. That's another thing that it cleaned up. So, I honestly kind of wish that it did the cleanup after I told it to, but that's my fault, because I told it to do the cleanup immediately after.

We can see here that um it has taken care of all the cleanup.

All 11 issues are back open with zero labels. But anyway, the workflow actually worked extremely well.

Um okay.

What it did not touch. Yeah, that makes sense. Ready for the next step. The triage workflow is committable. Next logical builds per the plan.

Uh get help label one-time setup for the real run. Already done implicitly.

Well, it shouldn't the next step be to create an adaptation of the Archon GitHub issue fix workflow for the Dark Factory.

Cuz I I think like I don't know why it The plan wasn't really clear on like we got to actually build something for implementing the pull requests, not just validating and fixing the issues that come up. So, I'm going to try to point it in the right direction here.

Um cuz I want I think that'd be the next workflow to work on. So, we have the triage, which is going to figure out what issues do we actually want to address, and then in parallel we'll invoke the Archon workflows to you know, take it from issue to pull

Okay.

You're right, and I missed that. Of course, Claude has to be sycophantic and tell me that I'm right.

Um but I am.

The triage workflow will produce his labels, but those labels are inert until there's a working fix workflow that knows how to validate.

Uh the plan calls this out implicitly as a hard blocker. The moment a bug fix or feature lands the default bun run validate will still blow up on Python syntax. Right. So, this this is a little bit behind-the-scenes planning I was doing. Basically, the fix GitHub issue work get fix GitHub issue workflow built into Archon isn't quite specific enough for my code base. So, I just need to take this as an example. This is one of the workflows that ships by default with Archon, and I just need to tweak it to work a little bit better with my Dark Factory specifically.

So, it's going to do some research for me. So, understand the structure of the repo, um understand the command or the workflow that we're going to adapt, and then see what we have for our global rules already.

All right. Yeah, I appreciate it. Don't care for such comments. You're really delivering a lot of value for free as well. I think we all value that a lot.

Yeah, I appreciate that. And um even when I do have the community as a you know, paid thing, I I do try to give an insane amount uh for free.

Like what I'm doing right now. So, I appreciate you recognizing that. And that really is important to me. Like no matter what, I always want to just be constantly giving. That's also why Archon is fully open source. This repository has nothing hidden. I mean, the Dynamis community had early access to it and got to even help shape some of the direction for it. And that's one of the cool parts of having a community. Uh but it's it's for everyone. That's

All right.

Is there a link for the community? Uh yeah, yeah. Sorry, I'll I'll send this again here in the chat.

Um so, this this is a link to kind of like the page like showcasing the AI second brain, but you just scroll down and there's a lot of buttons here to join.

So, yeah. I appreciate that.

All right.

Let's go Sorry, I'll I'll watch the logs here.

See, a strong DM depends on a digital twin universe, which creates behavioral clones of external services like Okta, Jira, Slack to allow agents to run thousands of realistic tests.

Yeah. Okay, if you really get into StrongDM setup, it is quite impressive.

So, it certainly won't be uh building everything StrongDM has, at least for now. But also, my application is luckily a lot simpler, where I won't necessarily need to to have the same level of depth.

Right? Like simpler application means I don't have to go as deep, but I still get like the same reliability like they built. Uh but yeah, if you want to like really read into what StrongDM has built, again, they haven't open sourced their Dark Factory. But they've, you know, open sourced the PRD like I showed at the start of the stream. And then I think they have like some blog posts as well where they break down a lot of what their architecture looks like.

It's pretty cool. It's It's really

Cool.

Um routine runs. Is that analogous to OpenClaw crons?

I mean, probably analogous to some kind of cron job. I don't know exactly. Like this I've checked my Anthropic usage every single day cuz I don't want to be on top of especially the 5-hour rate limit. This This I literally like wasn't This wasn't there this morning.

Um so, yeah. I don't I don't know what it is exactly. Maybe Maybe they have something If I just like search Claude

Okay.

Um oh yeah, here we go. 47 minutes ago, we have a post on the Claude code subreddit. New Now in research preview, routines in Claude.

Configure a routine once, a prompt, a repo, your connectors, and it can run on a schedule.

Scheduled routines let you give Claude a cadence and walk away.

Um okay, I was just about to say, they already have the slash schedule in the CLI. If you've been using slash schedule in the CLI, those are routines now.

There's nothing to migrate. Okay, that makes sense. So, they're they're taking something that was has already been there, but now they're just building it into other platforms. Like I I assume that like slash schedule used to be an only CLI thing, and now it's like within Claude desktop and co-work and the Claude app.

I guess that's what it is. I I mean, it's pretty cool. So, yeah, literally it is cron jobs. Just being able to run something that runs every hour or day or whatever.

All right. They are always shipping. It is crazy.

But good for them.

Okay. Very cool. So, now uh okay, how long have we been streaming for?

Um we've been streaming for a little over an hour and a half. Okay, so I am able to stream for Wait, I got to check my calendar. I'm able to stream for 2 and 1/2 hours. And I'm I'm loving what we're building right now. So, I'm thinking I'm going to go till Yeah, I'm planning on streaming till 1:30 Central Time. So, like another hour here.

We'll see how far I get

Okay.

Shh. What are we doing here? Okay. Wow, there's there's so much output. I'm getting a little like um burnt out on the like burnt out from all of Claude's output here. It's a lot to parse through cuz it's kind of it's fairly complex what we're working on

Okay.

Reg YouTube chat's validation story is prescribed, but not wired. Okay.

Uh global rules prescribe the exact commands, but none of those dev steps are actually installed yet.

No tests, no makefile, no CI. Right.

Yeah, I haven't built that yet.

The factory should add these when the first PR touches them. I don't actually agree with that. I want to build that ahead of time.

I [laughter] love this. It's cute, but it creates a chicken and egg problem.

Oh, that's funny. I Wow, Claude Claude has some personality now, I will say.

Uh and yeah, I agree with Claude here that that is a bad decision to put in the global rules.

Um okay.

The bundled Archon fix GitHub issue workflow is mostly language agnostic.

That makes sense. So, my surgical fix is just one file.

My recommendation is option B, custom

Uh here's why. Okay. All right. Yeah, sure.

You know, so it's saying I don't need to create a workflow from scratch, and I can just use what I have as the bundled workflow.

Um Yeah. Okay.

Yes, you're right. We definitely want to bootstrap the development dependencies manually.

And then I actually do want to create a custom workflow entirely.

So, yes, I know that there's not much we have to change from the GitHub issue fix workflow, but I want to be able to evolve it separately from the default Archon workflow anyway. So, we can mostly copy it and then obviously just changing the validate command, but yeah, let's go ahead and and do it that way.

Um

And then sure, we can we can smoke test issue number 26 after.

Okay.

There we go.

Um oh crap, we have to run the compaction now. Oh, I forgot about that.

I probably should have just worked on the workflow in a separate conversation.

Um cuz now that it does a memory compaction, it has to reload its skills and stuff. So, All right. Now that you just did a memory compaction, I need you to uh read the Dark Factory plan again.

And then load the Archon skill. Just to make sure you have full context before you go into building the second workflow and um doing the smoke test on issue number 26.

Make sure you build this workflow from scratch. And then uh also, I want to use the commands folder for all Archon Dark Factory workflows. So, make sure we don't have inline prompts for the other the triage workflow we just built as well.

Um so, that's a little specific, but I just realized that um I wanted to organize my workflows a bit better than I did up front if I go do the code base now.

We have the Dark Factory triage.

Um this prompt is inline, and it's massive.

So, I would rather extract this to a command.

Right, we don't have any commands right now, but in Archon, your workflows can reference a command, which is just like a separate markdown document. Just to have a better way to organize things, so we don't have have massive ugly prompts in line in the workflow itself.

I might even want to do the same for this. This apply decisions bash is like really long.

Yeah, there's there's a lot of optimizations that I can make. Just for the sake of the live stream, I am moving decently quickly.

So certainly will be iterating on things off camera like before I make the YouTube video tomorrow that kind of like you know sums everything up for the dark factory that

I appreciate it. Haters going to hate.

Dynamis rocks and the value of your contributions Cole are frankly incalculable. I appreciate it a lot.

That means a lot.

Price of Dynamis is a lot more reasonable than most of the courses community subscriptions being offered. A lot of them sound like snake oil.

Get rich quick for thousands of dollars.

Yeah, there there unfortunately is a lot of that out there.

There there's one sort of YouTuber in particular that I don't want to call out exactly or like name drop, but he he's a respectable guy. He does a lot around second brains specifically like even before generative AI. A lot of you might know who I'm talking about.

So I'm not saying the course is going to be bad, but he like he's like running these cohorts for building your own second brain.

And he's charging $2,000 for it. It's like crazy. Like what? Like $2,000 just to like learn how to build a second brain.

Like I taught that for in a 4-hour workshop and you can just join the community for it. Like it's not $2,000.

&gt;&gt; [laughter] &gt;&gt; Yeah.

Anyway, there there are some pretty expensive things out there for sure.

All right.

Will the dark factory repo be open source after the live? So not fully but it'll be within like the next couple of weeks I will open source it. So I need some more time to validate and really polish things, but the plan is to make it public by the end of the month.

Like everything public where you can even open an issue and have it work on it for you.

Who could I be talking about?

Yeah, yeah you got some of you guys know. Some of you guys know. And and again I respect him. Well, I guess I would say I have more of a neutral opinion. I haven't gotten like too deep into his stuff. So I'm not like trying to dunk on him or anything. I'm just saying that like to me like $2,000 to join a cohort is a little ridiculous, but to each their own. I mean some some people definitely value having that kind of like cohort style.

It's yeah, to each their own.

Okay, let's see here. What else we got?

The old you're absolutely correct. Yeah, that's right.

All right.

Bit out of context, but could we build a skill to make Claude code and and anti-gravity interoperable?

You definitely could. Like if you wanted anti-gravity to invoke Claude like in headless mode, you could if you wanted to.

I think I don't know [snorts] if anti-gravity has like a CLI, but Gemini has a CLI obviously. So you could have them invoke each other.

So you could like have a workflow where it's like Claude implements the code and then it calls the Gemini CLI to do the validation. Like you could do that kind of thing 100%. Like that's something that I actually want to build like directly in the into Archon workflows.

Like being able to have different providers at different nodes for planning and implementation and

Okay, this is really interesting.

MiniMax with open code performs much faster and better than in Claude code.

Probably the context blow of all the prompting that goes under the hood in Claude code.

So that Okay, that's good to know cuz yeah, maybe I would want to change this harness to use like Pi or open code instead.

Okay.

I I will have to look into that. Like I said, there's so many ways that I can probably improve this harness with the prompting and how I organize the workflows and my mission document and my factory rules and even just the tool that I'm using under the hood. Like maybe I do want to use Pi or open code

Let's see.

One way to find out if the course or subscription you paid for justify the cost is if you ship a product that others paid for to recoup that cost. I mean yeah, exactly.

That right cuz then then it's like there's no argument there. Like if you made more money thanks to what you learned there and you built something from it then

Why does it feel like we've been here for 1 plus hours and not achieved much?

I mean that's something that I've been trying to be very transparent about here is that like we're spending a lot of time planning and defining the architecture and the system for the dark factory up front. And it it has to take time. It it has to. I can't rush it. And and to be honest like I would probably be going even slower if I wasn't in a live stream here because in the end like if I want this thing to rip through issues really quickly, if I want the dark factory to be self-sustaining and really efficient and reliable, I have to be slow up front. That's what I'm and that's kind of the the teaching lesson here honestly as well.

Yeah.

Even miracles need time.

It's a good way to put it. I would I feel like that'd be very egotistical to call this a miracle. It's certainly not.

It's just an experiment that we'll see what happens, but yeah, it's a good way

That's very cool John. I joined Dynamis two months ago today, never wrote a single line of code, have a functioning second brain.

And yeah, that's that's awesome John. I appreciate it. Once you get to that point where your second brain is up and running and saving hours and hours every week there is no better feeling. So good.

All right.

One of the basic rules of marketing is people are willing to pay for high ticket. I mean that's fair. Yeah, like this individual that I mentioned I mean maybe I could just say his name cuz I'm really not like hating on him or anything, but I just for the sake of being careful.

I'm sure there are people that get a lot of value out of it and and yeah, he he like kind of is known as like the premium person.

He's like he's been building second brains for a decade.

I mean I would like to think that like he's struggling to catch up with all the AI stuff cuz he he's more traditional in his approach. He's probably doing fine, but anyway like yeah, I'm sure there's When you when you have something high ticket, it signals value and so you do attract that kind of person that is willing to shell out and they just want to make sure like I mean I don't always agree with this, but like sometimes people just think like hey, most money means it's the best value.

Definitely isn't always true, but it does kind of scream like this is going to be the safest bet if you have the money for it. I don't know. I don't know. Kind of rambling on that, but Yeah.

Uh Dynamis is awesome. I'm part of the community since it came live. That's so cool. I appreciate you being a part of the community for so long.

And by the way, the 1-year anniversary of Dynamis is this month, April 26th.

So there going to be some some exciting things that I've got going on for that.

Some live streams I'll be doing on YouTube around the time and also some exciting events in Dynamis and some things that I am releasing as a part of the anniversary celebration.

Also just speaking of things that are going on around that time, I want to call this out really quick as well. I'm doing a This is kind of unrelated, but around the same time.

I'm doing an AI transformation workshop with a gentleman named Lior Weinstein on April 28th. So that I believe that's a Tuesday at 9:00 a.m. [snorts] Central time.

So this is going to be really cool because Lior he's like an expert at coming into a company and helping it become AI native.

And so like designing like an AI native org chart and like how to enable each team and team member with AI technologies for coding and other things like even just like you know the sales and marketing and finance team and all of that.

So he's going to like talk about that for an hour and then for an hour I'm going to talk about how to transform your organization with agentic coding and like how to transform developer teams. So we're going to kind of take team it together to give you like this full view of like how you transform companies and even yourself as an individual.

So that's going to be really cool. So that's happening at the end of this month here.

And yeah, like you can see I'm on my you know scheduled live stream page of my channel. Like this is just going to be a free workshop just [clears throat]

Cool. Yeah, big shout out to Cole. Even the content you're delivering for free every week is insane. Thanks a lot.

Yeah, you're very welcome. Always my pleasure. Man, like I've been doing YouTube for it's almost 2 years now. I started like very beginning of July 2024.

And it's just a blast. Every single video is just so fun to make and keeps me ahead of the curve on everything too. Just being a constantly in the trenches building and researching and doing what it takes to make the

What is going on now?

&gt;&gt; [laughter] &gt;&gt; Uh, sometimes it's stressful to come back and just see like it's in the middle of writing the package.json. You don't even know why.

Oh, I guess it Oh, yeah, it's setting up the developer dependencies that we talked about. Okay, we're good. We're good.

So, okay, what has it done now? I think it made the full workflow.

Yeah, okay. Dark Factory fix GitHub issue. Okay, good. So, we got our second workflow now.

All right.

Dynamis Oh, no, not that one. I need to go to the rag YouTube chat. I want to open up the second workflow.

Okay, take then this is good. So, now we have all our different commands. So, instead of the workflow just being like a bunch of massive inline prompts, uh we got it organized a lot better.

Okay, so this is our fix GitHub issue.

By the way, I'm thinking about renaming the whole uh rag YouTube chat repo to Dyna chat.

That's why it references this name a couple times.

Uh but anyway, so this workflow here, we're going to start by extracting the issue number. So, this is very much based based on the default fix GitHub issue workflow in Arkon. Then we classify the issue, bug, feature, enhancement, refactor, chore, or documentation, just like we planned in the in the Dark Factory plan. Then we research the issue.

Uh and then we either do a plan, we create a plan if it is a feature to build, or we investigate the problem if it is a bug to fix, right? Cuz issues are going to be one of the two.

Right? So, like when we classify, it's either going to be a bug, and then I know we have all these different labels, but basically all these are just feature additions, right? Like if it's a chore or a refactor or an enhancement, like all those are just like more specific versions of a feature. So, may maybe the whole like issue labeling isn't optimized here, but I think it's fine.

It's actually pretty standard to have those kinds of labels.

And then we go into implementation, then validation, create the pull request, and we review. And so, I am going to have a separate workflow to do complete pull request validation, but I still want to have the workflow it's like when the PR is created, at least have it do a little bit of review, right? So, like initial round of review here, and then it'll have a more comprehensive validate PR workflow that I'll create next. That's the plan.

Just to give it a chance to do a little bit of self-fixing before we say like, "All right, here's our pull request for for you, uh you know, next stage of Dark

So, let's go back to our coding agent

Where am I going? Okay, there we go.

Okay, status so far. So, we refactored, built the new workflow, seven new commands.

Looking good. Got our dependency set up.

Uh okay, before I kick off the smoke test, two things to confirm. Dev

Um okay, that's fine.

Yes, I want you to install the dev dependencies. And actually, I would much prefer to use UV for the Python package management, so go ahead and change that in the repository. Get everything installed and test everything.

And um and then yes, go ahead and invoke after that invoke the Arkon workflow to run the Dark Factory fix GitHub issue on

And then we're not going to have time for it in the live stream here, but um you know what I I'm almost tempted to do another live stream tomorrow instead of a YouTube video. Maybe. I probably should make a YouTube video cuz it it'll be a week.

Uh but it'd be cool to like just keep building this live more. I know that it's a lot of time, but it it's it's fun to do this, and we're getting kind of close, right? Like we have a lot of it built. We just need to finish the last couple of workflows, and then we need to bring everything onto the VPS so that we have the whole Dark Factory running autonomously, and it's not relying on my computer being on. So, we're we're getting there. We're getting there.

We we won't really be unfortunately be able to see everything running on our VPS today, but um it won't take long once we have the workflows built and validated to copy everything over cuz we already have the I think this is my directory, Dark Factory. Yeah, yeah. So, we already have the app here. So, we we already have everything like cloned and set up and verified. So, we just have to bring over the workflows and then set up the cron job that's going to run every hour to do the triaging and then the implementation and everything. Like it it should work pretty quick once we have the workflows built. And that's why I wanted to use Arkon for this because then I'm not even creating the harness myself from scratch. I'm just building Arkon workflows, and that is the harness. It's a clear example of the value of Arkon as a harness builder, which is part of the reason I wanted to do this Dark Factory, by the way, is it's just such a cool use case to show the power of Arkon. Like these workflows are defining processes that would actually take a good amount of time to architect from scratch. If we wanted to like, for example, even just having this process of triaging issues and then sending off MiniMax to handle each one of these in parallel, like that would take a lot of engineering if we didn't have Arkon as a starting point to bring in the context and handle work trees for isolation so we can build each one of them in parallel. And then having the deterministic steps to uh you know, label the issues and close issues, like that that's a lot of work, but now we're able to just rip through this like pretty quick. Like I know that it that the stream is like 2 hours now, but still like when you really think about how much we've already engineered here, like it's a lot that we've built already, and we've taken our time with

Um &gt;&gt; [laughter] &gt;&gt; Can we use Arkon and the Dark Factory as paperclip?

So, yes. You could.

Cuz each Arkon workflow could be like the the you know, individual AI employee, kind of like how you manage that with um paperclip. You could.

That'd be cool. It's kind of what I'm doing, I guess. Like each Arkon workflow you could sort of think of as a different employee. I mean, we literally have the pattern here where we're doing the about the holdout where it's like this has to run completely separately from the implementation. So, it is sort of like two different AI employees that the the Dark Factory is delegating work

Smooth as fast. Mistakes are 10x as expensive as planning time. That's right. Pays dividends take your time up

Let's see. Learned a bunch about rag from you early on. Yeah, I still cover rag somewhat, not as much anymore, but it is still important. A lot of that old content is still very relevant, too.

But yeah, I used to I definitely used to be like the rag guy back in the day, but especially when I first started my channel.

The appearance of value is very important. 100% yeah. I had a relative doing art and selling at craft shows.

She switched to art shows and made a living off of it and has pieces in museums. That is so cool. Yeah. Right, so it kind of goes back to our conversation earlier, like when you have something priced at high ticket, like if you have the good appearance of value, you can price it high, like 100%

Providing unique individual value via video platforms, cohort platforms, etc.

is the norm of the future, not the exception.

Yeah, I mean, people will crave individual attention and personalization more and more over time as AI takes it away in some parts of life, so

Maybe Cole has an AI-proof job. How many people would watch this stream if his second brain was doing it alone?

&gt;&gt; [laughter] &gt;&gt; Oh, that'll be the day. I don't I don't know if I would ever want to have my second brain run a live stream. Also, I would it wouldn't look good right now.

But I mean, there are people that have AI avatars do videos, not necessarily live streams, but even that, like it just looks bad. I I don't I don't think that it's really feasible.

that it's really feasible. &gt;&gt; [snorts] &gt;&gt; [snorts] &gt;&gt; Um unless like for some reason people are okay with it being an AI avatar cuz you're just like delivering the news or something. I know that that's usually what most AI avatar channels, they are just like AI

All right.

If you're paying 2K to learn how to use Zettelkasten, then you may need more than a second brain.

That's funny. Yeah, I don't I don't know, like you can get pretty deep with Zettelkasten. I know myself personally, I've only scratched the surface. I have actually taken inspiration from Zettelkasten a little bit for how I've organized my Obsidian vault.

But yeah, it's definitely something I I personally like I agree, I feel like you can just figure it out on your own if you have a good head on your shoulders, but again, to each their own. I'm I'm trying to like avoid getting like super opinionated on things that like I know that like there is value but yeah, it just depends how much you want to just like get the best practices right away versus like figure out yourself over time. I guess is what it

Yeah, okay. So this is exactly why I built my own get up issue fixed workflow for just now.

The default Archon commands are very no JS centric and that I think that is something we want to improve to make it a bit more language agnostic or a lot more language agnostic.

I can see arguing with itself to build my go laying app.

Yeah, so I would recommend right now. I mean just in general it's good to build your own hardness. Build your own workflows cuz then you can customize it more.

I would recommend building your own and using the ones that we have as defaults as a starting point. So you can point your coding agent to the default Archon workflows and say look at these for a best practices and even like leveraging the structure for fixing issues or creating PRDs or validating pull requests.

And like use that as a starting point and then make it specific to my validation flow or my tech stack or my architecture.

Cool.

The AI co-host.

Okay, that could actually be interesting.

If I had my second brain not like run the stream but just like be there as a peanut gallery or something more practical like kind of giving feedback in real time or even like answering some questions in the chat.

I could Okay, that could actually be kind of cool. I should think about that.

How I could have my second brain co-host a live stream with me.

That would be cool.

Hmm. Okay, I'll think about that. I will definitely think about that.

Um oh, it's still going. Jeepers.

All right.

Well, I guess Oh, yeah, cuz we had the memory compaction. So that slowed things

Okay.

&gt;&gt; [snorts] &gt;&gt; I'm I just wanted to Okay, I'll let it keep running here.

All right.

Then you don't need me anymore. No, no, no. I still I still want real people to help co-host live streams with me. So Thomas, he's the guy that I have the comment highlighted for and then also Rasmus. They've done a lot of amazing work helping me on Archon.

And I have said that like for some Archon live streams, I'd love to get them in to co-host.

And I still stand by that. There's no way if I get my second brain to help co-host with me, it would be a different kind of thing and a different sort of of value proposition for the live stream compared to having a real person like you or Rasmus co-host with me. Like I would want to do both 100%.

Yeah.

Let's see.

All right.

We got some interesting feedback here.

All right, let's take a look. Once you take this to production, it will just hallucinate success at every step. LLMs give the most plausible answer. They rationalize anything they can't reason.

This whole video is from 2025.

All right, hot take hot take but no, I appreciate the pushback.

Um because what you're saying has some validity and that's why I'm calling it an experiment. And that's also why I'm spending so much time up front planning the system and the architecture.

Because yes, one of the biggest problems with large language models right now is that they are sycophantic. They always agree with everything that we say and they have an insane amount of bias towards their own opinions. They're going to rationalize everything and there is a risk if we don't build the system right that they're going to say that this thing is ready to merge and then it's going to merge it and then it's going to move on to the next issue and we go through this loop where every single time it's producing AI slop. That is a risk.

But that's what I'm trying to engineer for here.

I'm putting so much time up front because I'm doing things like defining the holdout pattern for validation. So that I'm taking away for some of this I'm taking away from and preventing some of the sycophantic behavior. Because when we go into our regression testing, we don't even know what was just built.

And so it's just going to be a neutral judge. At least that's the goal of what we're doing here. There's still a risk to it 100% and that's why I'm very clear at the start of the stream here that when we go to this level of autonomy for our coding agents. Like going back to the five levels here, the dark factory here is not what I recommend if you want the most reliable software possible. We might get there at some point and maybe this experiment is going to be a wild success and it's going to be like whoa, dang. Actually this can get us a really good, you know, MVP for any application.

Maybe. But yeah, I'm I'm not I'm not like numb to the fact that sycophantic or sycophancy is a big problem and we have to be really careful about that.

So I I feel pretty confident in the approach that I have here. I think we are going to avoid a lot of that bias and a lot of shipping things that it says are good when they actually aren't.

It's not going to be perfect but that's also why I have other strategies. Like I have the like really deep regression testing that I'm going to run every day or every week and then create more issues to address that. I still am going to have some human in the loop for the very very end. Um where I actually want to like push things to a platform that people are using. So I'll have a little bit like I've thought about a lot of it.

So again, I appreciate the pushback.

But trust me, I'm I'm considering it all 100%.

Okay, let's go back.

Anime girl co-host. You'll never see me do that.

&gt;&gt; [laughter] &gt;&gt; It's funny though.

You have to check the parameters to themselves introduce the specific parameters to check. Yeah, extra math and type of engineering checks to achieve it.

A little lost on what you're getting at there. Maybe you could elaborate.

Uh need to drop off. Good session Cole.

I appreciate it.

Yeah.

All right.

Cole the rag guy, spicy mangoes. Yeah, spicy mangoes hasn't come up for a while but I'll have to bring it in to another video soon.

Um An LLM can't even join a a chat room without everyone knowing instantly it's an AI. I mean yes, that's true.

Yep. That's that's why I wouldn't ever have a stream of having a second brain run anything anytime soon.

Yep.

Hard to explain. Yeah, all good. Yeah, take your time.

Yeah, okay. I agree with this 100%. If an LLM hosted a show, it would be boring and bland and obviously an LLM. I mean yes, I agree.

All right.

Um cool. So okay, anyway, our coding agent is done here. So I want to come back and and give it some more attention.

So all right, we're done with all of our dev dependencies. The smoke test problem. Dark factory validate currently runs whole code base checks. If I kick off the workflow on number 26, the agent will fix the one line score bug then validate will hit 130 pre-existing errors didn't create.

Um okay.

So wow. Okay, so I guess what what happened here is we're creating the development environment for the first time. So there are a ton of issues that aren't actually related to the fix that we'd be running the workflow on. So I I

Yeah.

Let's do number one. Let's address all

Yeah, I just I mean this will take a while.

Um but I I do just want to do that cuz cuz what I might be able to do is actually create the next workflow in another Claude code session here.

I think that makes the most sense.

Okay.

So let me go back to my vault and copy the path again

So I'll say read this plan file and also load the Archon skills. You can help me build more Archon workflows.

I have just finished creating my dark factory adaptation of the fixed get up issue workflow. Now I want to work on the validate PR workflow and it's very important that this follows the holdout pattern very closely.

I just had someone in the live stream say that they don't believe in this approach and I think the holdout pattern is one of the most important things to address their concern. So let's let's focus on that.

I'm being a little silly but but yeah, I think it's it is very important that we make sure that we are taking lessons from the strong DM dark factory cuz it

Let's see. What else we got in the chat here while I wait for this to run. We'll kind of monitor these two sessions in

Um is there a way to use Quen 3.6 plus with Archon even if it's through open router?

So uh one thing I learned the hard way recently with open router is you're not actually able to use Claude code with open router for any models outside of the Anthropic ones cuz it doesn't have an an Anthropic um compatible endpoint like MiniMax and GLM.

So if you wanted to use Quen 3.6 plus I would wait until we have support for uh Pi. Pi is going to be the third provider that we add this week or next into Archon.

Because then you'll be able to use other models really easily. You're not going to be like the obviously like right now we have Claude and Codex and you're a little bit more vendor locked. You can do what I did in the VPS to point it to other providers but only if they are Anthropic compatible like MiniMax and GLM.

So there might be a way but I actually tried first before I went to MiniMax directly. I tried going through open router and it didn't work.

It only worked when I chose an Anthropic

Um okay.

It's confused here because it's trying to read my workflows in the remote machine but I haven't actually pushed it

I might need to. Hold on.

Oh no, hold on. It figured it out. We're

I need a new open source 120B.

I mean yeah, it's been a while since we had one around that size for sure.

All right.

Um since I love C what do you think about Blitzys and

Um I don't Do I love C? I haven't programmed in C for a very long time.

Actually it's been about 5 years since I programmed in C.

Uh you mean like C the programming language?

I assume cuz you're talking about C compiler. I I haven't used C in a long time. I also haven't heard about Blitzy.

I'd be interested though. Um funny story or not funny but interesting fact for you guys.

Um when I was in college I was a teacher's assistant for a C and machine learning course.

So I I got to uh not sorry, not machine learning. Goodness. A C and assembly course, machine architecture course. So I got to help students debug assembly code for hours and hours a day.

&gt;&gt; [laughter] &gt;&gt; At one point that was fun.

All right.

So all right, what did it What do we got now? Have enough context. Let me lay out what I'm building. So holdout pattern design for the dark factory validate PR.

This is the critical workflow for defending the AI writes code on supervised concern. Here's how I'll enforce strict holdout across five layers. Okay, so this is the main concern that we're addressing here with AI sycophancy in our dark factory.

Not saying this is going to be perfect but this is my first attempt for the public experiment to avoid the coding agent just always saying its work is good and merging it.

So we have a separate Archon workflow, separate artifact directory, separate work tree. There is no way that it can peer into the work of the other agent and potentially take its bias.

So we exclude comments and review. No code no coder chatter.

Uh we have our our governance files.

So we're going to read those as our rule book.

Every AI node is going to start with fresh context. So every step of the way during the validation we're not even going to build up bias in the validation itself through the different nodes.

The behavior behavioral validation command leads with a holdout rule section enumerating what it must not consider. It can't find and search for implementation plans, commit messages, coder rationale, prior review comments.

It answers exactly one question. Does the diff solve the issue body?

Very good.

And then even another holdout pattern I I was kind of referencing earlier is like when we do the full regression testing every day or every week it also is going to have no knowledge of recent feature implementations or issues that we're in the middle of addressing, right? Like it just is going to answer the the straight question of like does this application work with all the user journeys we have laid out in the mission

Good.

All right. So now it's writing everything and we'll let it go. Both both of these are still running actually.

So

Cuz I can I I just want to show off the uh mission and factory rules just like

So here's our mission document. So it it's uh decently concise, not too long but it covers um like the core of the application. What is DynaChat?

Um who is it for?

Uh patent still pending by the way. I might not keep the name either. We'll see. We'll see. Uh who is it for? The core capabilities of the applications.

This is specifically calling out what is in scope. Like if I were to create an issue to add Google OAuth to the platform the triage is going to be like oh yeah, good. Let's mark this as accepted because it's literally in the scope for the mission.md.

And then we also cover what is out of scope. What must the factory never build? Like we don't want it to add other YouTube channels. Like if someone created a GitHub issue we'd want that to be rejected because at least for now. I mean I could extend this application later but at least for now the scope of this app is for my YouTube content to be a resource for you guys.

We don't want to you know allow someone to open an issue to swap the LLM. That would be bad for my credits.

Uh we don't want to like add in payments or subscriptions. Like this is meant to stay free. We don't want to overcomplicate with a mobile app or a desktop desktop app. Like no like electron or Tauri app.

Um so yeah, it's like things that we don't want to build.

Uh hard invariants. Like things that we have to we absolutely have to keep the same so we can't allow for issues that would try to tweak the rate limiting or the authentication requirements, things like that.

Uh ways we're allowing it to evolve and then quality standards.

Right? Like that that's our mission.md.

And then I can evolve this over time.

This is sort of like my console. So like going back to the analogy here of like you have a car that doesn't even have a steering wheel. Well you're still going to have some kind of console to provide higher level directions. And so if I want to tweak the dark factory I still have the levers to pull because I can still change the factory rules or the mission.md or I can update the Archon workflows. So if I really want to and there's like a drastic issue that I just really need to fix I can make a commit myself just straight to the main branch. So I mean there's flexibility with this here.

Uh but yeah, that's And then the factory rules. This file governs how the dark factory operates on this repo. So like here here's how we handle triaging, right? Like this is more context that we it's quite important to feed into the triage workflow.

Here's how we implement things. Here are our requirements for pull requests. For example, I don't want to allow more than 500 lines cuz I want every single issue to be a small focused scope of work.

That's one of the most important things for getting reliability out of our coding agents here.

Quality gates for auto merge. Uh uh talking about the regression testing.

I want to use the agent browser

This is the Vercel agent browser CLI.

It's an open source browser automation tool that I use a lot to test my front ends especially when I'm working on Archon itself. I'm using the agent browser skill. So I want this to be built into my larger regression testing workflow. It's going to Uh protected files that it's not allowed to change, right? I don't want it to change its own governance documents cuz this is my console, my lever to pull, not its own.

Um how like rules around like when we should auto reject certain things.

How we should escalate, cost and throughput. I mean I don't need to get like super deep into everything here but uh yeah, you can see like the factory rules gets more specific. Like this is quite a few lines of code here.

How many is it in total?

Uh sorry, not code, lines of markdown. I got Even this isn't like super long but it's 320 lines cuz there's quite a bit of specificity I want to supply there to

Have I you tried using prompts that presume failure and laziness? It can rationalize that too.

Um I mean I've I've played around with that kind of thing in the past.

Um it can rationalize that too. Are you saying that like that actually doesn't work or are you saying that uh that helps it rationalize or like helps it

Let's see.

You may want to leverage all your work with rag and build graph rag as the second brain. Obsidian as a second brain doesn't come close to graph rag but Obsidian is less set up.

Yeah, I mean I've found Obsidian enough for me and like it's nice cuz it is less set up like you said but I mean, maybe at some point I will build a whole Greg graph rank system for my brain and share it.

Could be cool.

I still kind of wrestle with that might be over engineering.

Depending on, you know, how much context you're storing in your brain.

But I have that in the back of my mind

Oh, it does work. Okay, cool. Yeah, yeah. I mean, that's a good idea then.

So, like my validation process, I could have a workflow in the dark factory that says like, "Okay, I need you to assume or like not even not even tell it to assume." I just say like, "This agent was lazy. Go and figure out why it's lazy, what could be better about the implementation, and then like open up another issue or resolve it directly." All right, that I like the idea of

Could I use Karpathy's auto research to improve the dark factory in the future?

Uh tell plot twist it does not stop running and starts replication.

Right. Some Yeah, maybe it will, who knows. Um so, yeah, I could use Karpathy's auto research.

Basically, if I were to apply Karpathy's auto research, it would be a separate process that has control over actually editing these three files.

Right? Cuz like the idea behind auto research is you have a a file that kind of dictates a larger system, like a model or a repo. And then that file you have a coding agent iterate on autonomously based on lessons that are learned from a feedback loop. So, I could build Karpathy auto research to like evolve these over time.

So, like right now within the factory rules I have that that hard rule where it's not allowed to It's not allowed to change the govern governance, the constitution. But this is only for the agents that are dealing with the issues in pull requests. So, I could have a separate process that like its actual sole job is to evolve the governance over time.

It's an interesting idea. I don't know if I want to take the autonomy that far right now cuz I really want this to be the lever that like only I am pulling on currently, but that could be a a natural evolution at some point. It'd be

Cool. Yeah, I appreciate all the ideas you guys are sharing here seriously.

Like there there's so much to chew on.

That's why I love this experiment because like the world becomes your oyster for the ways that you can evolve this and the things you can build with it as well. You can really apply this to any workflow and it's just cool being able to like use Archon to guide the entire thing as well.

Um like every single line of code that's written here is written from an Archon workflow.

So, yeah. All right. Uh let's go back and see where we're at now.

Um okay.

So, we built our third workflow here.

And uh what's this one? This one is Okay, so this one is in the middle of actually testing the uh the second workflow that we built.

Cool.

So, probably have to wait a while for that to happen. I think I'm going to go ahead and call the stream here though because we'll have to wait a while until we have like the next big thing happening. So, I'll keep working on this after the stream and then I'll be making a YouTube video on this as well for tomorrow, which I'm excited about.

So, yeah. All right.

Uh let's go back to full frame here.

I appreciate all of you guys being here today. This is a very different live stream to to build and like actually take my time with something. I've not really done that on a live stream before, so yeah, I appreciate all you guys' ideas and and questions as well.

And um yeah, for anyone who didn't get a chance to get their question answered, like always feel free to comment on a video, join the Dynamis community as well cuz I'm active there every single day.

Uh maybe I'll just show that uh one more time really quick here.

I'll put a link once more in the chat here cuz yeah, if you want to build your own second brain, go through the agenda coding course, just ask any question that uh you have. Like the Dynamis community is the place for it. So, I'd love to see you there. Um otherwise, I'll I'll go back to my full frame here.

But yeah, more live streams coming as well.

Uh because I I actually enjoy doing live streams more than making YouTube videos.

I like both, of course. I love both. But I love doing live stream. I love being live with you guys.

So, yeah.

All right. So, yeah, with that, appreciate all you guys being here. Uh stay tuned for the YouTube video tomorrow on the dark factory with Archon and more live streams coming up soon.

Hope that you guys have a great rest of your day and I'll see you all around.
