# Essential Skills for AI Coding: From Planning to Production
## Matt Pocock — Workshop Transcript (Reformatted)

---

Okay, folks, we're at capacity. Let's kick off. I don't want you waiting here for 25 more minutes before some arbitrary deadline. So, welcome. My name is Matt. I'm a teacher and I suppose now I teach AI. We have a link up here if you've not already been to this, which has the exercises for the stuff we're going to do today. This is going to be around two hours, so we might just sort of kick off two hours from now. Is that right, Mike?

Yeah. Perfect. And the theory behind this talk, or at least the thesis under which I've been operating for the last six months or so, is that we all think that AI is a new paradigm, right? AI is obviously changing a lot of things. You guys are obviously interested in this and that's why you've come to this talk. And I feel that when we talk about AI being a new paradigm, we forget that actually software engineering fundamentals — the stuff that's really crucial to working with humans — also works super well with AI. And this is what my keynote is on tomorrow. I'm going to be fleshing that out a lot more. And in this workshop, I'm hopefully going to be able to direct your attention to those things and hopefully show you that I'm right — but we'll see.

Can I get a quick heads-up first? How many of you have ever coded with AI? Raise your hand if you've ever coded with AI. Perfect. Okay. Keep your hand raised. Let's all share those armpits with the world. How many of you code every day with AI? Cool. Okay. Keep your hand raised if you've ever been frustrated with AI. Okay. Very good. You can put your hands down. Thank you for that show of obedience, I really appreciate that.

We are also being live-streamed to the Gilgood room as well. I've not — did we send someone up to the Gilgood room to just check they're okay? Don't know. But I see you. And there is a way that you can participate, which is we have a Q&A. We're going to be doing — I have a sort of hatred of Q&As because they're not very democratic. Mostly the most talkative people get to participate and share. And so we're going to be going through this QA here. So why do we have to wait till 3:45? The room is packed. The doors are closed. I 100% agree. And so if you want to ask a question, I would like you to pile into this async and then we can vote on each other's questions and hopefully get the best question surfaced for the entire room to enjoy.

---

So I want to talk about first the kind of weird constraints that LLMs have, and those weird constraints are sort of what we have to base a lot of our work around. There's a guy called Dex Hy who runs a company called Human Layer, and he came up with this idea that when you're working with LLMs, they have a smart zone and a dumb zone.

When you're first working with an LLM and you just started a new conversation, you start from nothing. That's when the LLM is going to do its best work, because in that situation the attention relationships are the least strained. Every time you add a token to an LLM, it's kind of like you're adding a team to a football league. Think of the number of matches that get added every time you add a team to a football league — it scales quadratically. And that's because you have attention relationships going from essentially each token to the other that are positional and based on the meaning of the individual token.

This means that by around — I would say around 100K is my new marker for this, because it doesn't matter whether you're using a 1 million context window or 200K — it's always going to be about this. It starts to just get dumber. So as you continually keep adding stuff to the same context window, it just gets dumber and dumber until it's making kind of stupid decisions. Raise your hand if that feels familiar to you. Yeah. Cool.

So this means that we kind of want to size our tasks in a way that sticks within the smart zone. We don't want the AI to bite off more than it can chew. And this goes back to old advice — Martin Fowler in *Refactoring*, the Pragmatic Programmer talks about this. Don't bite off more than you can chew. Keep your tasks small so that you as a developer, a human developer, don't freak out and don't start going into the dumb zone.

But how do you tackle big tasks? How do you take a large task — like cloning a company or doing something crazy — and how do you break it into small tasks so they all fit into the smart zone? One way you could do it, which is what the AI companies maybe want you to do or the natural way of doing it, is just keep going and going and going. You end up in the dumb zone, charging you tons of tokens per request. You then compact back down — we'll talk about compacting properly in a minute. And you keep going, keep going, keep going, compact back down, keep going. And I think that doesn't really work very well, because the more sediment builds up — we'll talk about that in a minute.

So the theory here then, and this is what I was doing for a while, is I would use these kind of multi-phase plans where I would say, okay, we have this large task. Let's break it down into small sections so that we can chunk it up and do each little bit of work in the smart zone. Raise your hand if you've ever used a multi-phase plan before. Yeah, really common practice, right? This is kind of how we've been doing it. Certainly, this is how I was doing it up until December last year.

And any developer worth their salt will look at this and go, "This is a loop, right? We've just got phase one, phase two, phase three, phase four. Why don't we just have phase N?" Phase N where we essentially just say, okay, we have a plan operating in the background and then we just loop over the top of it and go through until it's complete. And this is where — raise your hand if you've heard of Ralph Wiggum as a software practice. Okay, cool. Raise your hand if you've not heard of Ralph Wiggum as a software practice. Actually, that's more like it. So there's this idea called Ralph Wiggum, which is essentially: all you need to do is specify the end of the journey. You create a PRD — a product requirements document — to say, okay, let's describe where we're going, and then you just say to the AI, "Just make a small change that gets us closer and closer to there." And Ralph works okay, but I prefer a little bit more structure. That's kind of where we got to in terms of thinking about the smart zone.

---

Another weird constraint of LLMs: LLMs are kind of like the guy from *Memento*, right? They just continually forget. They just keep resetting back to the base state.

Let me pull up this diagram. Let's say another concept I want you to have is that every session with an LLM goes through the same stages. You have first of all the system prompt — this gray box is essentially the stuff that's always in your context. You want this to be as small as possible, because if you have a ton of stuff in here — if you have 250K tokens, like I've seen people put in there — then you're just going to go straight into the dumb zone without even being able to do anything. So you want this to be tiny.

You then go into a kind of exploratory phase — this blue is sort of where the coding agent is going out and exploring the codebase. Then you go into implementation and then you go into testing and making sure that it works, running your feedback loops. Raise your hand if that feels familiar based on what you've done. Yep. Sort of the main cornerstones of any session. And when you clear the context, you go right back to the system prompt — poof, you go right back there. So you delete everything that's come before.

Raise your hand if you've heard of compacting as well. Yeah. There are some people who've not heard of compacting. So let's just quickly show what that means. For instance, I've just been having a chat with my LLM. I've been talking about a thing that I want to build. I'm using Claude Code for this session, but you don't need to use Claude Code — in fact, it's often nice not to use Claude Code. So I've been having a chat with the LLM, just sort of planning out what I'm going to do next. It's asking me a bunch of questions, and I highly recommend you do this.

There's this tiny little status line that tells me how many tokens I'm using — the exact number of tokens I'm using. I have an article on my website AI Hero if you want to copy this. This is essential information on every coding session, because you need to know exactly how many tokens you're using so that you know how close you are to the dumb zone. Absolutely essential.

So I've got two options: I can either clear and go back to nothing, or I can compact. And when I compact, it's going to squeeze all of that conversation — which admittedly isn't very much — into a much smaller space. In diagram terms, this looks like you take all the information from the session and you essentially create a history out of it — a written record of what happened.

And devs love compacting for some reason, but I hate it. I much prefer my AI to behave like the guy from *Memento*, because this state is always the same. Always the same. Every time you do it, you clear and you go back to the beginning. And so if you're able to do that and you're able to optimize for that, then you're in a great spot. So that's kind of the two things I want you to think about with LLMs — the two constraints that we're working with. They have a smart zone and a dumb zone, and they're like the guy from *Memento*.

---

So let's take a look at the first exercise. While I'm doing this, the way I want this to work is I'm going to show you how — I'm going to be walking through it up here, and I want you folks to be tapping away and doing things as well. So that was just a little lecture bit. Let's now actually get and do some coding. For anyone who arrived late or anyone in the Gilgood room, go to this link up here to see the exercises and clone the repo. You absolutely do not have to — you can just watch me do it if you fancy it. But let's go there myself and let's see what exercises await us.

Essentially, I've built a course management platform — a kind of CMS for instructors and for students — and this is what we're going to be building a feature in. So I'm going to take you from essentially the idea for the feature all the way up to building a PRD for the feature, all the way up to implementing the feature. And hopefully you can take inspiration from this process and use it in your own work. So let's kick off.

---

We're going to start by using a skill which is very close to my heart. It's the "grill me" skill. And this grill me skill is wonderfully small, wonderfully tiny. And it helps prevent one of — I think — the main issues when you're working with an AI, which is misalignment.

The silent idea that I'm talking against here, that I'm arguing against, is the specs-to-code movement. Has anyone heard of the specs-to-code movement? Raise your hand. It's not really a movement, I suppose it's just sort of people saying "specs to code." What it is is people say, okay, you can write a program or you want to build an app. The best way to build that app is to take some specifications — so to write some sort of document — and then turn that document into code. So just turn it into code. How do you do that? You pass it to AI. If there's something wrong with the resulting code, you don't look at the code — you look back at the specs, you change the specs, and you sort of just keep going like this.

This is kind of like vibe coding by another name, where you're essentially ignoring the code. You don't need to worry about the code. You just sort of keep editing the specs and eventually you just keep going. And I tried this. I really tried it, and it sucks. It doesn't work, because you need to keep a handle on the code. You need to understand what's in it. You need to shape it, because the code is your battleground.

So this again is where we're going. Let's get some exercises. What I'd like you to do is go to this page — the grill me skill. And inside the repo here, we have a Slack message from our pal. It's in clientbrief.mmd — a Slack message from Sarah Chin. (For some reason, Claude always chooses Sarah Chen as the name, I don't know why.) It's saying that in Cadence, our course platform, our retention numbers are not great. Students sign up, do a few lessons, then they drop off. "I'd love to add some gamification to the platform." And so, when you're presented with an idea like this, you need to find some way of turning it into reality. Let's say Sarah Chen is your client. You're on a tight budget. You need to get this done fast. How do you go and do it?

Raise your hand if you would enter plan mode when you're doing this. Anyone a big user of plan mode? Yep. Let's actually shout out quickly — any other ideas about what you would do with this? Or raise your hand — what would be your first port of call? Let's imagine that Sarah Chen's gone on holiday. You have no idea — she's just posted this thing. You need to action it before you go.

Well, my first protocol is I go for this particular skill. I'm going to clear my context. I'm going to get rid of you — you don't need to be there. And I'm going to say, I'm going to invoke a skill, which is the grill me skill. Let's quickly check. Raise your hands if you don't know what this is. Cool. Let me be more specific. Raise your hands if you don't know what I'm doing here when I do a forward slash and then type something. Anyone kind of understand what that is? I'm invoking a skill. I'm invoking the grill me skill. And what I'm going to do is I'm going to say "grill me" and I'm going to pass in the client brief.

So now the LLM really has only a couple of things here. It just has the skill and it has the description of what I want to do. And this is virtually how I start every piece of work with AI. And while it's exploring the codebase, I'm just going to show you what the grill me skill does.

So this is inside the repo — you can check it out. It's extremely short. "Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies one by one. For each question, provide your recommended answer. Ask the questions one at a time." What this does — and what I noticed when I was working with AI, especially in plan mode actually — is it would really eagerly try to produce a plan for me. It would say, "Okay, I think I've got enough. I'm just going to produce a plan."

And what I found was that I was really trying to find the words for what I wanted instead of that. And Frederick P. Brooks, in *The Design of Design*, he has a great quote talking about the design concept. When you're working on something new with someone, when you're all trying to build something together, then there's this shared idea that's shared between all participants, and that is the design concept. And that's what I realized I needed with Claude. I needed to reach a shared understanding. I didn't need an asset; I didn't need a plan. I needed to be on the same wavelength as the AI — as my agent. And this is an extremely effective way of doing it.

So hopefully — there we go. Nice. It has done its exploration. First of all, it's invoked a sub-agent which spent 93.7K tokens on Opus. And it's asked me the first question. Cool. We can see that even though the sub-agent burned a ton of tokens, I haven't actually increased my token usage that much. Raise your hand if you don't know what sub-agents are. It's an important question. Everyone kind of clear what sub-agents are? Okay, I'll give a brief definition: this sub-agents thing here, the explore sub-agent, has essentially gone and called another LLM which has an isolated context window, and then that LLM has reported a summary back. So a sub-agent is kind of like a delegation. You're delegating a task to a sub-agent. It goes eagerly, does all the thing, explores a ton of stuff, and then just drip feeds the important stuff back up to the orchestrator agent, to the parent agent.

So hopefully you guys have seen the same thing. It's done an explore. And we now have our first question: "Points economy. What actions earn points and how much?" At this point, you can ask it questions to deepen your understanding of the repo. I obviously know this repo really well because I wrote it, but you might not know what's going on. So let's say my recommendation: keep it simple, two point sources to start. What's so nice about this is that not only does it give us a question that kind of aligns us here, we get a recommendation too. And often what I'll find is the AI's recommendations are really good. And so I'll just say, "Skip video watch events, they're noisy and gameable." I agree. Sarah's asked us to keep lessons as the bread and butter. Yeah, looks good, pal.

Now, what I usually do is I usually dictate to the AI. I'm usually actually chatting to the AI instead of typing, but this is a relatively new laptop and I couldn't get my dictation software working on it because Windows is crap.

"So should points be retroactive? There are existing lessons progress records. We're completing out timestamps." This is a really nasty question, right? Should we actually go back and backfill all of the lesson progress events? This is a kind of question that you need to be aligned on if you're going to fulfill the feature properly. This is not something I considered, and Sarah Chen certainly didn't consider. Do I want it to be retroactive? Let's actually do a vote inside here. Should we go back and backfill all the records? Raise your hand if you think we should backfill all the records. Raise your hand if you think we shouldn't backfill all the records. There are a lot of fence-sitters in the room. I'm going to say, you know, this is the kind of discussion you're sort of having with the AI. You're getting further aligned.

Notice, too, how I'm able to keep in the loop here with AI. It's pinging me these questions pretty quickly. I'm not having to go off and check Twitter or something. "Levels. What's the progression curve?" Yeah, that looks about right. Yes. So hopefully you should be able to work through this with the AI and essentially try to reach an alignment. And this grill me skill — this can last a long time. I've had it ask me 40 questions. I've had it ask me 80 questions. I've had some people it asks a hundred questions to — literally you're sat there for an hour chatting to the AI. And what you end up with is essentially this conversation history that works really nicely and works really nicely as an asset of the design concept that you're creating.

This can also function like this: you can have a meeting with someone who's maybe a domain expert. Maybe I have a meeting with Sarah. I feed that meeting transcript into — I don't know — Gemini meetings or whatever you guys are using. You take that, you feed it into a grilling session, and you grill through the assumptions that you didn't have. So this ends up being a really nice way of just taking inputs from the world and then turning and validating them.

So, okay. Let's see. I really want to get to the end of this, but I also don't want to just be sat here talking to the AI in front of you for a thousand days. So, I tell you what — while you guys sort of have a little fiddle with this locally, let's start a little Q&A session now.

---

**Q&A Session 1**

**Q: Have I tried SpecKit, OpenSpec, or TaskMaster instead of the grill me skill? Do I find them more verbose or a structured alternative?**

This is a great question. So there are a ton of different frameworks out there that allow you to build up this planning process for you. I personally believe that at this stage, when there's no clear winner, when there's no kind of one true way, and when things are changing all the time, you need to own as much of your planning stack as you possibly can. What I've noticed with a lot of my students is they tend to overuse a certain stack. They get into trouble, and because they don't own the stack and they don't have observability over the whole thing, they just go, "This isn't working. This sucks." Whereas if you have control over the whole thing, then at least you know how to fix it, or potentially know how to fix it. So even though I'm giving you a stack, I believe in inversion of control and you should be in control of the stack.

**Q: Many of the questions asked by the grill me skill are not necessarily appropriate for a developer — rather a PO. In larger teams, who should use it?**

Raise your hand if you've ever done pair programming. Anyone ever done pair programming? Right. Put your hands down and raise your hand again if you've ever done a pair programming session with an AI. Right. How did it go? Was it good? You enjoy it? I think pair programming sessions with AI is a great idea because you've got a third person in the room who will relentlessly quiz you and ask you questions. If you don't know the answer, it should be you, the domain expert, and the AI in the same room. If you have a question about implementation, it should be you, a fellow developer, and the AI in the same room. You can be working through these questions in your team. And I think actually we're going to look at implementation in a bit and we're going to see how you can make implementation so much faster. But I think the really crucial decisions — the ones you need humans for — you actually need a lot of humans, and it doesn't really matter how many humans are in there. You can actually throw a bunch — like a kind of mob programming with AI essentially.

**Q: What's my favorite meta-prompting tool?**

I think I kind of answered that.

**Q: How do I use the conversation as an asset after the grill me session?**

Well, we're going to get there.

---

Okay, so I really want to speed this up sort of artificially. Someone just said, "Okay, Ralph loop this." But this is crucial because I can't loop over this. I think of there as being two types of tasks in the AI age: you have human-in-the-loop tasks, where a human needs to sit there and do it — which is this, we are the human in the loop with multiple humans in the loop — and there are AFK tasks, tasks where the human can be away from the keyboard and it doesn't matter. Implementation, as we'll see, can be turned into an AFK task. But planning — this alignment phase — has to be human-in-the-loop. Has to be. So I've got to do it, unfortunately.

I don't know. Give me a long list of all your recommendations. I'm running a workshop right now, so I artificially need you to pull more weight. So let's see what it does. Let's answer a couple more questions while it's doing its thing.

**Q: What is my opinion on PMs or other...**

I'm going to return to this later. I think I'm going to leave this unanswered. A bit of mystery.

**Q: I notice I'm not using the "ask user questions" UI for grill me. Why?**

There's a specific UI that you can bring up in Claude Code — ask me a question using the "ask user question" tool. And this UI is just sort of broken in Claude and I really hate it. You notice I'm using Claude, but I don't like Claude very much. Like, you really are free with this method to choose any system you like. And this is what the UI looks like. It's very pleasing when you first encounter it, but then you realize it is actually broken in a ton of different ways.

---

All right, what did it come back with? Oh, blimey. Oh, no. So, while this is doing its thing, let me do some teaching in the meantime.

The plan here is that we take our grill me skill and we need to essentially find some way of turning it into a destination. We need to figure out the shape of this — that's what we're doing, figuring out the shape of the tasks during the grilling session. And in order to turn it into a bunch of actionable actions for the AI, we essentially need to figure out the destination. We need to know where we're going. We need to know the shape of this entire thing.

So I think of there as being two essential documents that we need. We need a document that documents the destination, and we need something to document the journey. In other words, we need a document that's going to figure out what this even looks like in all of its user stories and figure out a definition of done. And then we need to figure out what the split looks like. So that's where we're going to go next, once we finish with the grilling session.

Yeah, it looks great. Fantastic. I love it. It answered 22 of its own questions. There you go. That's quite representative of what a grilling session looks like.

So at this point now I have used 25K tokens and all of that — or loads of that stuff — is gold. I want to keep that around. I've got 25K great tokens there. And what I want to do is summarize it in some kind of destination document. So this is the next bit: we're going to write a product requirements document. And the product requirements document, or the PRD, is essentially — that's its function, it's the destination document. And it sort of doesn't matter what shape it is. I've got a shape that I prefer and that I quite like, but you can just choose your own shape or whatever your company uses. And all we're really doing is summarizing the design concept that we have so far.

So let's try this. I'm going to initiate this. I'm going to say, zoom all the way to the bottom. All I'm going to do is just say, "Write a PRD." And we can take a look at that skill now. This skill does a few things. It first asks the user for a long, detailed description of the problem. You can use "write a PRD" without grilling first, but I just like to grill first and then write the PRD afterwards. Then you can get it to explore the repo, which we've kind of already done. Then we get it to interview the user relentlessly — so have a kind of grilling session again. And then we start putting together a PRD template.

So this is available in the repo if you want to check it out. And essentially this is what it looks like: we've got some problem statements — the problem the user is facing — the solution to the problem, and a set of user stories. And these user stories sort of define what this is. You guys have probably seen things like this if you've been a developer at all. Cucumber is a language you can use to write these in, or we just write them ourselves essentially. Then we have a list of implementation decisions that were made and a list of, crucially, testing decisions too.

So I'm going to run this. And so it's finished its thing. Ah, Windows. Let me close the thing. Thank you. I don't know why I bought a Windows laptop. I think I just like the challenge. So the first thing that it's going to give me are a set of proposed modules it wants to modify.

Now there's a deep reason why I'm thinking about this. At this stage we have an idea. We've spec'd out the idea. We've reached an understanding of what we're trying to do. And then we need to start thinking about the code, because this is not specs to code. This is not where we're ignoring the code. We actually keep the code in mind throughout the whole process. And the way I like to do this is I like to just think about a set of proposed modules to modify. We're going to return to this idea of continually designing your system and keeping your system in mind. So it's saying, "Recommend test for the gamification service is the only deep module with meaningful logic. These modules look right." Yeah, that's good. And it's going to ping out a PRD.

Now for ease of setup, I've got it so that it creates a set of issues locally. It's just going to create a PRD inside this issues directory. But the way I usually do it, and you can check this out yourself, is you can go to what I consider my work repo, which is github.com/mattpocock/course-video-manager. And in here, this is essentially an app that I use all the time to record my videos and things like this. I think I've recorded like a thousand videos in here or something nuts. And you can see here that it's got 744 closed issues. And this is essentially all of the PRDs and all of the implementation issues that I've put into here. So this is how I usually like to do it.

So that's what I'm doing. There we go. I'm just going to say yes and get that issue out. Let's see. It is inside here. So we got the problem statement: people sign up for courses. The solution. The user stories — 18 user stories, looks nice. Some implementation decisions, level thresholds, etc. This is enough information. We've kind of clarified where we're going and what we're doing. So that's what we do. We essentially have a grilling session and we've created an asset out of it.

Now, raise your hand. Should I be reviewing this document? Raise your hand if you think I should be reviewing the document. Yeah, I don't look at these. I don't look at these. The reason I don't look at these is because what am I testing at this point? What am I — when I read it, what am I testing? What are the failure modes I'm trying to test for? I know that LLMs are great at summarization, because they are — they're really good at summarization. I have reached the same wavelength as the LLM, right? Using the grill me skill, we have a shared design concept. So if I have a shared design concept, all I'm doing is I'm just essentially checking the LLM's ability to summarize.

---

Let's have a Q&A because I can feel you guys are itching for it. And then I think we might have like a five-minute comfort break just to rest my voice and so you can catch up with the exercises for a minute, if that's all right.

**Q&A Session 2**

**Q: If I don't like Claude Code, which one do I actually like?**

Have you ever heard the phrase, "Democracy is the worst way to run a country apart from all the other ways"? That's how I feel about Claude Code.

**Q: What are your thoughts on developers needing to very deeply understand TypeScript now that "fix the TS, make no mistakes" exists?**

I don't understand the phrasing of this but I think I understand the meaning, which is that I believe that code is very important — and this is going to feed through the whole session — and that bad codebases make bad agents. If you have a garbage codebase, you're going to get garbage out of the agent that's working in that codebase. We'll talk more about that in a bit. And so I think understanding these tools very deeply, understanding code deeply, is going to make you a much, much better developer and get more out of AI.

**Q: Now that we have 1 million tokens available, do we ever actually want to take advantage of that? I've noticed that the dumb zone has become less dumb lately.**

Okay, great question. This goes back to our earlier discussion. I recorded my Claude Code course using a 200K context window, and on the day that I launched the course, they announced the 1 million context window. My take on this is that what Claude Code did is they essentially just shipped a lot more dumb zone to you. Now, this is good for tasks where you want to retrieve things from a large context window — if you want to pass five copies of *War and Peace* to it and find out all the things. It's good for retrieval. It's less good for coding. So I consider that about 100K at the moment is the smart zone. The smart zone will get bigger, and that will be a really nice improvement.

---

So folks, we're going to take like a five-minute comfort break if that's all right, just for my voice and so maybe you can have a little move around or something or grab a drink. I can just notice some sleepy eyes and I want to make sure that we're awake for the next bit if that's all right. So we'll take five minutes and I'll see you back here then.

---

All right. So we have our PRD — which I'm not going to read — a kind of destination document. Let's quickly scan for any good questions before we zoom ahead.

**Q: Rediscovering the role of software engineer in today's world — top three disciplines you recommend.**

Taekwondo is good. I've heard — I have no idea how to answer this question. Thank you for asking it though. Plumbing is a good one. Yeah. I don't know if that's a discipline. The plumbers I've hired are not usually very disciplined.

---

So, okay, we now have our destination. How do we actually get to our destination? We have a sort of vague PRD. How do we split it so that we don't put things into the dumb zone? In other words, we have our large task — how do we split it into a multi-phase plan? Well, probably what you would do at this point is you would say, "Okay, Claude, give me a multi-phase plan that gets me to this destination." That sort of makes sense. This is what we've been doing before, but I have a better way of doing it now, which is that I like creating a Kanban board out of this.

Raise your hand if you don't know what a Kanban board is. Cool. A Kanban board is essentially just a set of tickets that you put on the wall that have blocking relationships to each other. This is how we've worked as developers for a long time, really since agile came around. And what it does — we can see it here — it has proposed that we split this setup into five different tasks. Here we have the first one, which is the schema and the gamification service. Yeah, that looks pretty good. This is blocked by nothing. And we can even see here that it's given it a type of AFK, too. Remember I talked about human-in-the-loop and AFK earlier. This is an AFK task — something we can just pass off to an agent to do its thing. Streak tracking — okay, that looks good. Then wire points and streaks into lessons and quiz completion — this is blocked by one and two. Retroactive backfill — this is blocked only by one. And then this one here is blocked by all of the tasks.

Now you could say, why don't we just hand this generation of the issues over to the AI? Why do I need to be involved here? Because it's given us quite a good selection. Why do I need to review this and figure out what's next? My take here is that this is really cheap to do — very quick to do once I've done the PRD. And I can immediately see some issues here.

There's a really, really important technique when you're figuring out what the shape of this journey should look like, and it comes from a classic idea from *The Pragmatic Programmer* called tracer bullets or vertical slices. It's really transformed the way I think about actually getting AI to pick its own tasks.

Systems have layers, right? There are layers in your system. These might be different deployable units. You might have a database that lives somewhere, an API that lives close to the database but in a separate bit, a front end that lives somewhere totally different like a CDN. Or within these deployable units, you might have different layers — for instance, in the codebase we're working in, we have a ton of different services: a quiz service, a team service, user service, coupon service, course service. And these services have dependencies on each other, so they're kind of like individual layers.

What I noticed is that AI loves to code horizontally — it loves to code layer by layer. In phase one, it will do all of the database stuff, all of the schema. Then it will go into phase two and do all of the API stuff. Then it will add the front end on top of that. Can anyone tell me what's wrong with that picture? Why is that not a good thing to do? Raise your hand if you have an answer.

Exactly — you don't get feedback on your work until you've really started or completed phase three. You're not actually testing that all the layers work together. You haven't got an integrated system that you can test against. And so instead you need to think about vertical slices — thin slices of functionality that cross all of the layers that you need to. And this is a much better way to work, and a much better way for the AI to work too, because it means at the end of phase one, or during phase one, it can get feedback on its entire flow.

So what this means to me is, inside the PRD-to-issues skill, I've got: "Break a PRD into independently grabbable issues using vertical slices." Tracer bullets — by the way, when you're an anti-aircraft gunner looking up in the sky at night, if you're just shooting normal bullets you have no idea what you're firing at. You see the plane but you don't see where your bullets are going. Tracer bullets have a tiny bit of phosphorescence attached so that every sixth bullet or so, you actually see a line in the sky. You have feedback on where you're aiming. So the idea here is that we increase our level of feedback and get near-instant feedback on what we're building, because without that the AI is coding blind until it reaches the later phases.

So what I see here is that even though I've told it to do vertical slices, it's proposing to create the gamification service first on its own. That's just one slice there, and that feels like a horizontal slice. What I want to see in the first vertical slice especially is schema changes, some new service being created, and a minimal representation of that on the front end. I want it to go through the vertical slices, not just the horizontal. Does that make sense? So I'm going to give the AI a rollicking. I'm not going to waste tokens just memeing. "The first slice is too horizontal" — I'll start with that and see if it picks it up.

I think having that — what I really like about going back to those old books is that we are really trying to verbalize best software practices in English. And these 20-year-old books have already done that. It's an absolute gold mine if you want to throw that into prompts. But even with that, it's not going to do a perfect job each time. So: "Award points for lesson completion, visible on dashboard." Yes, that's a beautiful vertical slice because it's definitely a big chunk of stuff. It's doing a lot of stories there, but we're going to see something visible at the end, and the AI will then just be able to add to that. You see why that's preferable to the first one. Cool. Looks great.

So, we're getting closer now. Anyone following along at home — you're not at home, but you get the idea. We'll hopefully see the same thing too and start developing the same instincts. Let's open up for questions just while I'm creating these GitHub issues — or not GitHub issues, local issues.

---

**Q&A Session 3**

**Q: When will I stop using Windows?**

Never.

**Q: How does AI decide when to stop grilling? Because AI can ask incessantly. Can we have a smarter way to decide the stop point?**

Yeah, it does tend to — those grilling sessions can be super intense. And the thing about these skills is you can tune them if you want to. If you feel like the AI is just absolutely hammering you, then you can just tell it to pull back a little bit or get it to do stop points and that kind of thing. So if that's a failure mode that you run into a lot, then you just change the skill.

**Q: Do I still use "be extremely concise, sacrifice grammar for the sake of concision"?**

There was a tip that I gave folks five months ago, which was to basically increase the readability of your plans. When you're using plan mode, you can put it in your claude.md and say, "When talking to me, sacrifice grammar for the sake of concision." This prompt was really useful to me when I was reading the plans because it meant the plans would come out very concise, really nice, easy to read. But I've since dropped this idea in preference to a grilling session, because what I noticed was I didn't want to read the plans — I wanted to get on the same wavelength as the LLM. I wanted it to ask aggressive questions to me. And when I stopped reading the plans, I stopped needing them to be concise. So I think of the plans, the destination document, as the end state — and I don't need that end state to be concise.

**Q: What do I think will be the outcome of the Mexican standoff of future roles of PMs and other roles converging?**

I have no idea. I'm not a pundit. I have no idea.

---

So, after a couple of approvals, we should end up with a set of issues. Now, these issues that we're creating — they're designed to be independently grabbable, which means that this Kanban board ends up looking like a set of tickets with independent blocking relationships. This one needs to be done before this one, this one needs to be done before this one. And let's say we got another one over here — this one needs to be done before this one. This means that you can start to parallelize. You can start to get agents working at the same time on these tasks, because yeah, this one needs to be done first, and then these two can be grabbed at the same time by independent agents.

Raise your hand if you've done any kind of parallelization work with agents. Cool. So this allows you to turn those plans into optimally — into directed acyclic graphs essentially — where you have three phases: phase one you do this one, phase two you do the two below it, and phase three you do this third one and add it on. And when you think about it, this is a relatively simple plan, but you could have many different plans operating all at once. It means that you can do really nice parallelization, and we'll talk more about that in a bit. But that's why I prefer a Kanban board setup like this to a sequential plan — because a sequential plan can really only be picked up by one agent. This plan here is really only one loop — only one agent can work on these because we have numbered phases and they're not parallelizable. Does that make sense?

So, we've got our issues. Stop asking me for — oh no, it's creating them on GitHub. I really don't want that. You fool. Create them in local markdown files instead, referencing the local version. So once we get to this point, we have a bunch of issues locally that we can start looping over and implementing. And it's at this point that the human leaves the loop.

So far, let me pull up a proper overview of this flow. We have taken an idea and we've grilled ourselves about the idea. We can skip over research and prototype, but we've turned that into a PRD, into a destination document. We've then turned that PRD into a Kanban board. And all of those steps are human-reviewed. And now at the implementation stage, we step back and we let an agent — or multiple agents — work through that Kanban board.

Now, what this means is that yeah, we've spent a lot of time planning here, but it means we've queued up a lot of work for the agent. We can think of this as kind of like the day shift and the night shift. This is the day shift for the human — planning everything, getting all the stuff ready. And then once we kick it over to the night shift, the AI can just work AFK.

But what does that look like? If we head to the last exercise — running your AFK agent. I've called this "Ralph" really, because it is essentially a Ralph loop. And this prompt here — I want to walk through this really closely.

The first thing it's doing is we're essentially going to run Claude and encourage it to work completely AFK. I'll show you what the script for this looks like. Inside once.sh in the repo, it's essentially just a bash script where we grab all of the issues — which are inside markdown files — and we cap them into a local variable. So that issues variable contains all of the issues in our entire backlog. Then we grab the last five commits — I'll explain why in a minute. And then we grab the prompt and we just run Claude Code with permission mode except edits, and just pass it all of the information. That's what a very, very simple version of this sort of loop looks like. And of course this is not a loop — this is just running it once.

The loop is in the AFK version, which is a fair bit more complicated. The crucial part here is we're running it in a Docker sandbox as well, but I don't want you to install Docker on your laptops because we're just going to tank the conference Wi-Fi. So I am going to demo this to you, but you won't need to run this yourself. We're just running one version of the thing that we're going to loop again and again. This is kind of like the human-in-the-loop version. And running this again and again is essential because you're going to see what the agent does and see how it ends up working. And any tuning that you need to add to the prompt, you can do that.

Let's go to the prompt. Local issue files are being passed in. You're going to work on the AFK issues only — that makes sense. If all AFK tasks are complete, output this "no more tasks" thing. What we're doing here is essentially running a backlog — curating a backlog that our AFK agent is going to pick up. That's the purpose of all of this setup, from the beginning all the way to this Kanban board. We're just essentially creating a backlog of tasks for the night shift to pick up. And the night shift — this sort of Ralph prompt here — it's got its own idea about what a good task looks like. I did talk about parallelization, and I will show you this later, but this is essentially a sequential loop here. We're just going to run one coding agent at a time. This is a good way to get your feet wet.

So it's prioritizing critical bug fixes, development infrastructure, then tracer bullets, then polishing quick wins and refactors. And then we just have a very simple kind of instruction on how to complete the task: explore the repo, use TDD to complete the task — I'll get to that later. And we then run some feedback loops.

So let's just try this and see what happens. It's created the issue files — we should be good to go. I'm going to cancel out of this, clear, and I'm going to run ralph-once.sh. And you can feel free, if you're following along, to do the same thing. So we can see it's just running Claude inside here with the prompt and with all of the issues that have been passed in. And while it's doing its thing, you probably have some questions about this setup and about the decisions I've made to essentially delegate all of my coding to AI.

---

**Q&A Session 4**

**Q: How do you retain negative decisions — things you decided against and rationale — when persisting the results from the grilling session?**

That's a very simple answer: in the "write a PRD" section, there is a section at the bottom for things that are out of scope — the things we're not going to tackle in this PRD, which is very important for giving a definition of done.

**Q: How to deal with agents producing more code than we can review? How to properly parallelize and use multiple agents in a separate way?**

There's two questions there. Raise your hand if you feel like you're doing more code review now than you used to. Yeah, definitely. I don't think there's a way to avoid this. If we delegate all of our coding to agents, you notice that the implementation here is really the only AFK bit. We then also need to QA the work and code review the work. And if we are running these loops where it's essentially going to implement four issues in one, it's hard to pair that with the dictum that you should keep pull requests small and self-contained. Small self-contained pull requests means you're needing to do fewer loops or shorter loops — or maybe you do a big stack of PRs, but that seems horrible as well. That's still just more separated code to review. I don't honestly know what the answer to this is yet. I think we just need to be ready to be doing more code review, essentially — which is not fun. That's not a fun thing to say. I don't feel good saying that, but I do think it's probably the way things are going. It's a great question.

**Q: What's my front-end workflow?**

(I'm going to answer that in a minute.)

---

Can we grab a couple of questions from the room as well? Let's not do the mic, but raise your hand if you've got a question for me immediately.

**Audience Q: The approach looks very linear — from an idea to QA. Of course, the real world is a lot more messy. You have all these ideas in parallel, and while you're working on something, something else comes in. How do you deal with the messiness? How do you feedback?**

Great question. So the question was: if this all looks great if you're a solo developer, but actually how do you implement this in a team? How do you gather team feedback on this? My answer to that is that if you have an idea up there, essentially the journey from the idea to the destination is something you need to figure out with the team. All of this stuff up here — this is kind of like team stuff, you know what I mean? So if you have an idea and you do a grilling session on it and you have a question that you don't know how to answer, then you need to loop in your team as we described before. Then you might need to go, "Okay, we just need to build a prototype of this. We need to actually hash this out. We need something that the domain experts can fiddle with." Oh, okay. We might need to integrate a third-party library into this. We might need to do some research. We might need to ping this back and forth and find a third-party service. We might need to go back with the information that we gathered there to the idea phase. So all the way up to the sort of PRD and the journey, that's something you need to involve your team with. That's something where these assets are going to be shared and argued over, and you're going to have requests for comment on them, and that loop is going to just keep grinding and grinding until you figure out where you're going. Once you figure out where you're going, then you can start doing the Kanban board and the implementation. But this is super arguable and you'll be bouncing back and forth between the phases. Does that make sense?

**Follow-up: Would you not need a PRD for your prototype as well?**

Let's just quickly talk about prototypes for a second. There was a question about how you make this work for front-end — because front-end is really sensitive to human eyes. You need human eyes looking at the front end all the time to make sure it looks good. AI doesn't really have any eyes. It can look at code, but front-end is multimodal. My experiences with trying to plug AI into — let's say agent browser or Playwright MCP — to give it tools to allow it to look through a front end and sort of look at images... in my experience, it's not very good at that yet. And it can't create a nice front-end in a mature codebase. It can sort of spit one out. But what it can do is: you say, "I want some ideas on how this front-end might look. Give me three prototypes that I can click between in a throwaway route, so I can decide which one looks best." And you take the asset of that prototype and you then feed it back into the grilling session, or you get feedback on it, etc. The prototype is just — you know, it's messy. It's supposed to give you feedback early on in the process. So that's a great way of working with front-end code, a great way of looking at software architecture in general. Let's go one more question.

**Audience Q: In your system, how do you integrate respecting an architecture design with API contracts and fitting with a larger system, security constraints — all kinds of constraints like that?**

Yeah, there's a lot in that question. The question was: how do you conform with existing architecture? How do you make it conform to the code standards of your codebase, or architecture design, API security rules that constrain your designs? I'm going to answer that in a bit if that's okay.

---

So hopefully we have started to get some stuff cooking. It's just pinging on the explore phase here. Tempted to just start running it AFK. Maybe I will, maybe I won't. What it's essentially doing is exploring the repo. It's going to then start implementing based on what we wanted. Let's actually have one more question just while it's running.

**Audience Q: Why do you not get AI to QA its own code?**

Now of course you absolutely can. And I think while it's cooking here — okay, it's got a clear picture of the codebase. It's assessing the issues. It's doing issue 02 as the next task. I'm going to show you that in a bit. I think you definitely should do an automated review step as part of implementation. So you have your implementation. You should then, because tokens are pretty cheap and AI is actually really good at reviewing stuff, get it to review its own code before you then QA it. I found that catches a ton of different bugs.

And the way that works is — let me do a little diagram. If you have an implementation that's used up a bunch of tokens in the smart zone, if you get it to try to do its reviewing, it's going to be doing the reviewing in the dumb zone. And so the reviewer will be dumber than the thing that actually implemented it. Whereas, if you clear the context, then you're essentially going to be able to just review in the smart zone, which is where you want to be.

Let's see how our implementation is doing. Okay, good. It's generating a migration. That looks pretty nice. We're getting some code spitting out. And while I'm sort of like — aha, here we go: TDD.

---

Let's talk about TDD and then I think we'll have another little break. TDD — I've found — is absolutely essential for getting the most out of agents. Raise your hand if you know what TDD is. Cool. TDD is test-driven development. What it's essentially doing is something called red-green-refactor. And if you look in the codebase, you'll be able to find a skill which really describes how to do red-green-refactor and teaches the AI how to do it.

So what it's doing is it's writing a failing test first. It's saying, "Okay, I've broken down the idea of what I'm doing and I'm just going to write a single test that fails, and then I need to make the implementation pass." I have found that, first of all, this adds tests to the codebase — and it tends to add good tests to the codebase.

So we've got this kind of gamification service. It looks like it's using some existing stuff to create a test database. Test fails because the module doesn't exist yet. Okay, we've confirmed red. And then it goes and hopefully runs it and it passes. I found that — raise your hand if you've ever had AI write bad tests. Yeah, it tends to try to cheat at the tests because it's sort of doing it in layers. It will do the entire implementation and then it will do the entire test layer just below it.

I'm just going to say yes, you're allowed to use npx vitest. And using this technique, it generally is a lot harder to cheat, because it's sort of instrumenting the code before it's then writing the code. So I find that TDD is so, so good for places where you can pull it off. And in fact, it's so good that I sort of warp my whole technique around getting TDD to work better.

I can see some drooping eyes. It is so hot in here. You can imagine how hot it is up here. Let's take another five-minute comfort break. Let's come back at quarter to — I think have a nice generous one. And we'll be back in about six, seven minutes and I'll talk about how I think about modules, think about constructing a codebase to make this possible. I've just been sort of fiddling with the AI here and we have ended up with a commit. So we have something to test. Issue number two is complete. Here's what was done — this is kind of what it looks like when a Ralph loop completes: you end up with a little summary. And we have now something we can QA, because we did the feedback loops — because we did the tracer bullets — because we said, "Okay, give us something reviewable at the end of this." We can immediately go and QA it. Now, there's nothing less exciting than...

...watching someone else QA something, but hopefully we can have a little play. Let's just check that it works at all. In fact, before I go there, I just want to sort of work through what just happened, which is: we see that it's created some stuff on the dashboard and it then ran the feedback loops. So it then ran the tests and the types.

Now TDD is obviously really important, and it's really important because these feedback loops are essential to get AI to produce anything reasonable — because without this, AI is totally coding blind. You have to — if your codebase doesn't have feedback loops, you're never ever ever going to get decent output out of AI. And often what you'll find is that the quality of your feedback loops influences how good your AI can code. Essentially, that is the ceiling. So if you're getting bad outputs from your AI, you often need to increase the quality of your feedback loops. We'll talk about how to do that in a minute.

So it ran npm run test, npm run type check. It got one type error and needed to fix it with a nice bit of TypeScript magic. Very good. Yeah. Typo — level thresholds, number. Okay. You see why I stopped teaching TypeScript — because AI just knows everything now.

And it ran the tests and it passed and it's looking good. So we now end up with 284 tests in this repo. Pretty good. I do find front-end really hard to test here. We're essentially just testing the service. So we've created a gamification service, and then we have a test for that service. You can see the service and the test itself.

Now, if I was doing code review here, I would first go to review the tests, make sure the tests were testing reasonable things, and then go and review the code itself, just to make sure that it's not doing anything too crazy. The essential thing is I need to actually look at the dashboard. I'm going to log in as a student. Log in as Emma Wilson. Head into courses. Let's say I've got an Introduction to TypeScript. Continue learning. Yes, I completed this lesson.

Something went wrong. I imagine it's because I don't have — SQLite error. I don't have the right table. So I need a table "point events." Point events is a strange table name — I'm not sure quite what it was thinking there. Let's suspend. Let's run npm db migrate or push, I think. Can't remember which one it was, but you kind of get the idea. I'm not going to subject you to watching me do QA because it's so dull. But at this point, I would essentially go back in. I would open the project back up.

And this is a crucial moment. And it's so important to QA it manually here — because QA is how I then impose my opinions back onto the codebase, how I impose my taste. What you'll often find is that there are teams out there who are trying to automate everything — like every part of this process. And they will tend to — if you try to automate the creation of the idea, automate the QA, automate the research, automate the prototype, you end up with apps that, I feel, just lack taste and are bad. Maybe they just don't work, or they don't even work as intended, or there's just no taste. You need a human touch when you're building this stuff, because without that you just end up with slop. And we are not producing slop here. We're trying to produce high-quality stuff, and so that's what the QA is for.

---

So I'm going to do two things in this final section. I'm first going to tell you how — there's probably a question in your mind here, which is: let's say I have a codebase that I'm working on and it's a bad codebase. It's a codebase that's really complicated, that AI just never does good work in, and maybe actually most humans that go into that codebase don't do good work. How do I improve that codebase?

In his book *The Philosophy of Software Design*, John Ousterhout talks about the ideal type of module. Let's imagine that you have a codebase that looks like this. Each of these blocks here are individual files. And these files export things from them — you know, they have things that you pull from the files that you then use in other things. And so you might have these weird dependencies where this file over here might rely on this file or might rely on that file.

Now, if these files are small and they don't export many things, then John would call these shallow modules — essentially lots and lots of small chunks. Now this is hard for the AI to navigate, because it doesn't really understand the dependencies between everything. It can't work out where everything is. It has to sort of manually track through the entire graph and go, "Okay, this relies on this one, relies on this one, this one relies on this one."

And it's then also hard to test this as well, because where do you draw your test boundaries here? Do you test each module individually? Like, just literally draw a test boundary around each one. No, don't do that. Around this one and then maybe another test boundary around the next one and then the next one — or should you sort of do big groups of it? Should you say, "Okay, we're going to test all of these related modules together and just sort of hope and pray that they work"?

Now this means that if I think that bad tests mostly look like that — where the AI essentially tries to wrap every tiny function in its own test boundary and then just test that those individually work. But what that does is it means that when, let's say, this module over here calls those two, so it depends on both of these — then this module might misorder the functions or there might be stuff inside that poor module that's worth testing on its own. And if you then wrap this in a test boundary, what do you do? Do you mock the other two modules? How does that work?

So actually figuring out how to build a codebase that is easy to test is essential here, because if our codebase is easy to test, then our feedback loops are going to be better and the AI is going to do better work in our codebase. Does that make sense?

So what does a good codebase look like? Well, not like that. It looks like this — where you have what John Ousterhout calls deep modules. Modules that have a little interface on them that expose a small simple interface but that have a lot of functionality inside them.

Now what this means is that these are easy to test because you — let's say that there's a dependency between this one and this one. Then what you do is you just wrap a big test boundary around that one module — around this one up here. And you're going to catch a lot of good stuff, because there's lots of functionality that you're testing, and really the caller, the person calling the module, is going to have a simple interface to work from. So it's not too tricky. That makes sense. Deep modules versus shallow modules. This shallow version is bad. This deep version is good.

And what I find is that unaided — if you don't watch AI carefully, it's going to produce a codebase that looks like the shallow one. So you need to be really, really careful when you're directing it. And that's why, if we look inside the PRD — it's specifically saying, "Okay, this gamification service is a new deep module which we're going to test around. It's going to have this particular interface." And it's going to have — we're modifying the progress service too. We're modifying the lesson route, modifying the dashboard routes, etc. So I'm being really specific about the modules that I'm editing, and I'm making sure that I keep that module map in my mind at all times throughout the planning and then throughout the implementation. Very, very useful.

It's useful for one other reason, too. Not only does it make your app more testable, but you get to do a little mental trick.

---

Let me get a question from you guys. Raise your hands if you feel like you're working harder than ever before with AI. Yeah. Raise your hands if you feel like you know your codebase less well than you used to. Yeah. This is a real thing — because we're moving fast, because we're delegating more things, we end up losing a sense of our codebase. And if we lose the sense of our codebase, we're not going to be able to improve it. And we're essentially delegating the shape of it to AI. I don't think that's good.

But then how do we make it so that we can move fast while still keeping enough space in our brains? I think that this is a way to do it, because what you're doing here is not only are you thinking about creating big shapes in your codebase — big services. What I think you should do is design the interface for these modules, but then delegate the implementation.

In other words, these modules can become like gray boxes where you just need to know the shape of them. You need to know what they do and sort of how they behave, but you can delegate the implementation of those modules. I found this is really nice. I don't necessarily need to code-review everything inside that module. I don't necessarily need to know everything of what it's doing. I just need to know that it behaves a certain way under certain conditions and that it does its thing. So it's kind of like, okay, I've got a big overview of my codebase and I understand kind of the shapes inside it, understand what the interfaces all do, but I can delegate what's inside. I found that has been a really nice way to retain my sense of the codebase while preserving my sanity. Make sense?

And so you might ask, how do I take a codebase that looks like the shallow one and then turn it into a codebase that looks like the deep one? How do I deepen the modules? Well, we have a skill called "improve codebase architecture." Nice and direct.

Let's run it. What this skill is going to do is essentially just do a scan of our codebase and look for what's available here. And feel free to run this yourself if you're running the exercises. And it's exploring the architecture — essentially how to work within this codebase — and it's going to attempt to find places to deepen the modules. Pretty simple.

One really cool thing that it found here is part of my course video manager app — a video editor built in the browser, which is really hardcore. It's a decent bit of engineering. And I wanted a way that I could wrap the entire front-end all the way to the back-end in like a single big module, so that I could test the fact that I press something on the front-end and it goes all the way to the back-end. And so I found a way — essentially by using a kind of discriminated union between the two types — I was able to use this skill to essentially have a huge great big module that just tested from the outside, or was testable from the outside. This video editor infrastructure — and it meant that AI could see the entire flow, could act on the entire flow, and test on the entire flow. And honestly, it was just night and day in terms of the ability of AI to actually make changes, because AI working on a video editor is pretty brutal if you don't give it good tests.

So that is — honestly, if you take one thing away from today, just try running this skill on your repo and see what happens.

---

**Q&A Session 5**

**Q: Do I keep the markdown plans and issues for later reference?**

Let's say that you have a great idea, you turn it into a PRD, and you then implement that PRD and the PRD is essentially done. Raise your hand if you keep that information in the repo — so you turn it into a markdown file. Raise your hand if you want to keep that around. Cool. And raise your hand if you don't want to keep it around — if you want to get rid of it as soon as possible. Yeah. This is a question that doesn't have a clear answer.

What I'm really scared of with any documentation decision is this: let's say that we have a PRD for this gamification system. We keep it in the repo. We go on, go on, go on. Let's say a month later, we want some edits to the gamification system. And we go in with Claude and it finds this old PRD and says, "Yes, I found the original documentation for the PRD system." Well, it turns out that the actual code has changed so much from the original PRD that it's almost unrecognizable. The names of things have changed. The file structure has changed. Even the requirements may have changed. We might have actually tested it with users. This is doc rot — where the documentation for something is rotting away in your repo and influencing Claude badly, or Claude agents badly. So I tend to not keep it around. I tend to get rid of it. And for me, because my setup uses GitHub issues, I just mark it as closed. It can fetch it if it wants to, but it's got a visual indicator that it's done. So I tend to prefer ditching these.

**Q: Should I, in the early planning stage, be trying to optimize the plan?**

This is something I actually see a lot of people doing, and it's a really good idea. So when you — let's go back to the phases. Let's say that you have all of these phases here and you get to the point where you've figured out everything with the LLM. You understand where you're going. You've created this sort of journey-destination document here. How do you then — should you then try to optimize and optimize and optimize that PRD until it's the perfect PRD you can possibly imagine? I don't think there's a lot of value in that, because I think the journey is really just sort of a hint of where you want to go. And the place that you need to be putting the work is in QA. And you can sort of do that AFK, I suppose, but in my experience you're not going to get a lot of juice out of it. The thing that really matters is getting alignment with the AI, which you do in the grilling session initially.

---

**Audience Q: How do you get it to code the way you want it to code — so by the time you get to code review, it's at least familiar, uses the libraries you wanted to use?**

We had this question before actually, which was like: how do you enforce your coding standards on the agent? Essentially, how do you get it to code how you want it to code? Now, there's essentially two different ways of doing it. You've got push and you've got pull.

What do I mean by push and pull? Push is where you push instructions to the LLM. So you say, okay, if you put something in claude.md — "talk like a pirate" — that instruction is always going to be sent to the agent. So that is a push action. You're pushing tokens to it. Pull is where you give the agent an opportunity to pull more information. That's, for instance, like skills — a skill is something that can sit in the repo and it has a little description header that says, "Okay, agent, you may pull this when you want to."

My current thinking about code review and about coding standards looks like this: when you have an implementer, you want the coding standards to be available via pull. If it has a question, you want it to be able to sort of answer it. But if you then have an automated reviewer afterwards, then you want it to push. You want to push that information to the reviewer. You want to say, "These are our coding standards. Make sure that this code follows them." So if you have skills for instance, then you want to push that stuff to the reviewer so the reviewer has both the code that's written and the coding standards to compare to. Hopefully that answers your question.


And I can show you an automated version of this as well. I recently spent maybe a week or so building this thing called Sand Castle. And Sand Castle — I was sort of unhappy with the options out there for running agents AFK. And what this does is it's essentially a TypeScript library for running these loops. So you have a run function that creates a work tree, sandboxes it in a Docker container, and then allows you to run a prompt inside there. And in that work tree then it's just a git branch and you have that code and you can then merge it later.

There are some really, really nice ways of viewing this, and it essentially allows you to run these kind of automated loops and allows you to parallelize across multiple different agents really simply. So I'll go into my sand castle file, go into main.ts here, and let's just walk through this.

So this is kind of like — I showed you a version of the Ralph loop earlier. This is where we take it from sequential into parallel. We have here, first of all, a planner that takes in — it has a plan prompt here — that looks at the backlog and chooses a certain number of issues to work on in parallel. Remember I showed you that Kanban board where it had all the blocking relationships? It works out all of the phases. So this one will say, "Okay, let's say we have..." — you can ignore all this glue code here. This is essentially just a set of issues — GitHub issues with a title and with a branch for you to work on.

And then for each issue, we create a sandbox, and then we run an implement in that sandbox, passing in the issue number, issue title, and the branch. This is like the loop that we ran just before. Then if it created some commits, we then review those commits. This is essentially the loop.

What do we do with those commits? We pass those into a merger agent which takes in a merge prompt, takes in the branches that were created, takes in the issues, and it just merges them in. If there are any issues with the merge — you know, with the types and tests and that kind of thing — it solves them. And this has been my flow for quite a while now for working on most projects. It works super, super well. And yeah, I recommend you check out Sand Castle if you want to learn more.

And to answer your question properly: in the reviewer, I would push the coding standards; in the implementer, I would allow it to pull. And I'm actually using Sonnet for implementation and Opus for reviewing, because I consider reviewing — I need the smarts for that.

---

Okay, where are we at? We're sort of zooming everywhere in this talk because I'm kind of having to run things in parallel. So let's go back to the improved codebase architecture. It has finally finished running and it's found a bunch of architectural improvement candidates. So it's got essentially a cluster of different modules that are all kind of related that could probably be tested as a unit. Got number one: the quiz scoring service. There's some reordering logic extraction as well. It has arguments for why they're coupled and it has a dependency category as well. So local, substitutable in SQLite, within-memory test DB. Quiz scoring service currently has zero tests — this is the biggest gap. So this is what it looks like when we come back from improved codebase architecture.

---

So we have nominally kind of 17 minutes left. I don't know about you, but I'm knackered. Let me kind of sum up for you, because I think we're sort of reaching the end of our stamina. I'm going to be available for the full time if you want to come and ask me questions.

So this is essentially the flow. Throughout this whole process, we're bearing in mind the shape of our codebase. This is not a specs-to-code compiler. This is not an AI that's just sort of churning out code. We are being very intentional with the kind of modules and the shape of the codebase that we want. We are making sure that we are as aligned as possible by using the grilling session — by really hammering out our idea. We're not over-indexing into the PRD. We're not trying to read every part of it. We're not thinking too much about it even. We're then just turning that into a set of parallelizable issues which can be worked on by agents in parallel. We implement it, and we QA and code review the hell out of it, and then keep going back to that implementation.

One thing I didn't really mention is that in the QA phase, what the QA phase is for is creating more issues for that Kanban board. So while it's implementing even, you can be QAing the stuff and going back, adding more issues. And the Kanban board just allows you to add blocking issues sort of infinitely, really. And then once that's all done, once you've got code that you're happy with, once you've got work that you're happy with, then you can share it with your team and you can get a full review. So this is kind of like — once you get here, this is kind of one developer, or maybe a couple of developers, sort of managing this. And then it's kind of up to you to figure out how to merge it back in.

Of course, all of this can be customized by you. This is just something that I have found works. I'm not trying to sell you on a kind of approach here. What I recommend, if you take one thing away from this session, is that you should head to Amazon and just buy a ton of those old books, because I mean, I just found it so enlightening reading them. Pre-AI writing is always really fun to read anyway, and I just — on every single page I found that there was something useful and something interesting to read.

So thank you so much. Thank you for putting up with the heat. Hopefully your body temperatures will reset soon. Thank you very much.

