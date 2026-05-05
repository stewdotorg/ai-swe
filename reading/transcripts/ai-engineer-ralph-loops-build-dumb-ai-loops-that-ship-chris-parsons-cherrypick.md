# Ralph Loops: Build Dumb AI Loops That Ship — Chris Parsons, Cherrypick

| | |
|---|---
| Creator | AI Engineer
| Published | 2026-05-04
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=2TLXsxkz0zI](https://www.youtube.com/watch?v=2TLXsxkz0zI)
| Generated | 2026-05-04

## Description

Dumb loops beat clever workflows. Most teams building with AI agents reach for multi-agent orchestration, planning graphs, and elaborate tool chains. Then they spend months debugging them. A single loop that processes one ticket at a time, evaluates its own output, and improves on the next run will outperform all of it.
In this hands-on workshop you will build three things. First, a working Ralph Loop that processes real tickets end-to-end. Second, a synthetic feedback loop so you can test and iterate locally without waiting on production data. Third, a self-improving cycle where the loop's output quality gets better with every run without you touching the prompt.
Speaker info:
- https://x.com/chrismdp
- https://www.linkedin.com/in/chrisparsons/
- https://github.com/chrismdp

> Transcript extracted from YouTube auto-generated captions (`2TLXsxkz0zI.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Welcome. So, this workshop is on Ralph loops. Uh, hands up here who knows what a Ralph loop is. That's almost everyone.

I'm guessing that the other folks who came in were just here because they thought that sounded weird or or maybe looking for a quiet place to work. I don't know, but you're very welcome. So, what we're going to do today, this is a two-hour workshop. We're going to uh if you could just kind of um make a little bit of space if you need to as people are coming in, that would be really helpful. Thank you. This is a two-hour workshop. We're actually going to build Ralph Loops together. We're going to do this together on our own laptops in order to uh to make some stuff happen and get some things done. So, so it's not just about theory. This is a very practical thing. So, if you've got a laptop, you're welcome to get it out in a second. We're actually going to try and do this ourselves. Uh, so, uh, I I have a few slides, but not many. Most of this is going to be live demos and kind of interaction, um, points as well. Um, and the idea is at the end of this, you should be able to leave with something that works that we'll uh, we'll apply it to a kind of toy codebase uh, just for fun uh, to create a Pomodoro timer. But uh but hopefully the idea is that you'll be able to use this on your real work when we get uh when we get done. So another show of hands. Okay. Who is using Claude code or codeex specifically to write code? Hands up. Oh, quite a lot of people specifically to write code.

Okay. Who is using it to write all their code? Who is no longer writing any code?

That's quite a lot of people. Look around for a minute. That that is a huge change. If I'd asked a bunch of programmers six months ago who was not writing any more code, you'd get a very different answer. Uh so next question, who is using either claude code or codeex? Um and I'll include cursor here as well in their non-coding work.

Okay, quite a lot of you. What about for all your normal non-coding work?

Okay. Interesting. Interesting. So can you just see the future um in the room?

Right. There's a few people who are starting but we're still on that journey for sure. And who has built Ralph Loops before? Last show of hands. One or two people. Okay, great. I'm going to be looking to you for all the answers. Um, so that's great. So, uh, just a little bit about me. My name is Chris Parsons.

Um, these days I spend most of my time trying to help, uh, teams like the team I used to run figure out what on earth to do with AI mostly. So, uh, I'm a CTO by background. I've done a couple of BC back startups and scaleups and uh this has taken uh me and my friends by storm rather and we are trying to still all figure it out professionally together in terms of how to help our teams adopt and use AI. So that's what I do for a living these days. I have about 30 years or so of building software professionally been the CEO of an agency. I've done a lot of agile consulting remember that uh and that kind of training as well back in the day. And uh and funnily enough all of those it's a whole another talk. all of those uh principles and practices that we taught for years and no one really listened were are still very much applicable um to AI. So, so there we go.

Um so these days I'm actually running Ralph loops all the time, 24 hours a day to get my work done. So I'm I'm using them to write my emails. I'm using them to check my calendar. I'm using them to write content and newsletters. I'm using them to help me do my client work. So I'm using them in absolutely everything.

I also use them for code which is what we're focusing on today but they are very much applicable to every part of our lives. So by the end of the day the idea is that you will be in the position where you can do that too. Uh so this is how I used to work with AI until quite recently. Uh you probably can't see that very well. This is an N10 workflow uh that I used in order to create my weekly newsletter. It took me uh probably a week to write let alone actually test and debug. Uh, it's got a huge number of different things in here. This this is like a a featured article flow which would read various different articles from my blog and figure out whether I posted it before, summarize it using AI and put it there. And then there's another one for grabbing uh links that I'd posted into a particular list and it kind of did a bit of commentary on that.

It was really quite complicated and and difficult to run and maintain. And it kind of worked okay, except that 2 p.m.

on a Monday, pretty much every Monday, I would get the dreaded notification from NA10 that my workflow had failed. And I was just like, "Oh, no." And then I'd have to go in and figure out in here what whatever had broken and try and run it and fix it. Now, this is nothing against NA10. NA10 is a really cool tool and it can do some really cool things.

And I'd never have been able to orchestrate AI in the way that I was doing before without a tool like N810.

despite being a coder, it's just so much easier to manage in here. And you can manage all the API keys really easily, and it's it's a nice tool to to stick things together, but it was so brittle to use at this kind of level of complexity. And I didn't get a huge amount of value out of it. Um, and then every time I fixed it, I would do something else. And so, honestly, it was probably easier for me to just write the newsletter than it would have been to maintain the thing that wrote the newsletter. And I probably had a slightly better newsletter. Uh, so this wasn't great. Um but but this was to my mind a few months ago the really the only way to use AI. You had to kind of orchestrate it and manage it, give it the right data and kind of handle all the context. And I thought that this was the future of automation, but it isn't really the future of automation. The future of automation is a lot more like something like this uh running in clawed code. So this is obviously not the actual skill, but um I have now in clawed code a skill that writes newsletters for me and it has all of those instructions. In fact, I copied and pasted the NA10 JSON code from there into claw code and said, "Write a skill based on this flow, and it did a great job." Um, and then what it does is it goes through and does all of the things.

But what's interesting about that is how does claw code work? Well, it reads the first thing, it decides on the next step, and then it reads the next bit, and then it decides on the next step, and it kind of works through over some minutes to actually write and produce the newsletter that I was writing. And it's the same for code. It's the same for anything that we want to build using claw code. Claw kind of just takes care of it. You describe the kinds of things you want and it does it. Now what's interesting is that clawed code fundamentally is running on a loop, isn't it? It just reads the skill, calls a tool, goes back to the beginning, reads the skill again, calls a tool, calls a tool, and then at some point it figures out that it's done and it stops and it gives you your newsletter in whatever form you want it in. Um, so what's interesting is that this ships much better, more coherent newsletters than the previous uh the previous workflow. I still have to change them, write them, screw around with them, but but they are uh they are a much better first draft than they ever were. And I haven't really touched this skill. All I really do with this skill is I say at the end of a newsletter writing process, please just update the skill with anything you can figure out from the session you should have done differently. And it makes the odd tweak here and there. Um so that's a loop that is this kind of form of working in loops with AI. So so agents that originally start in workflows where you have quite complicated orchestration that looks a bit like something hellish like this end up in quite a simple loop perhaps with a bit better context. Now this didn't work for the longest time but it's now beginning to work with the latest models and by latest model I really mean GPT 5.8. X really GPT 5.12 onwards uh and Claude Opus 4.6 or Sonnet 4.6 upwards.

So those models uh started emerging around about uh the end of November. Uh I have no idea about Mythos by the way.

I I've spoken to people who've used it and they say it's it's it's good, but it's mostly marketing, but we'll see. Uh but um but yes, uh we'll see what that where that takes us. Maybe we won't even need skills. Maybe we'll just say write a newsletter and it'll do it. Who knows?

Um, but what what I'm trying to my my point is is that rather than using complicated workflows, we're actually using skills and loops much more in context and loops. And any kind of agent that we run is in some way a loop already. Okay. And this kind of powerful looping construct is something that you can more generally apply. Um so what happens when we take loops a little bit further? So um the first stage um is this idea or the first idea came from Jeffrey Huntley uh a little while ago you know ancient times in AI which means probably about last June and he said basically what we should do is whenever we finish using an AI to do anything we should just try the thing again in some way we should just uh just give it exactly the same prompt and see what happens see what it does again uh and it sounds it sounds a bit stupid and it's based on. Does anyone know where this story comes from? Who knows why it's called a Ralph loop? Like two people.

It's called a Ralph loop because of Ralph Wigum, which is a Simpsons character who basically says um the uh he just tries the same thing over and over and over again and eventually it works. Um and it's all it is really. All all that a Ralph loop is is uh build this thing or do this thing inside a prompt. Then the AI goes away and does the thing and then it finishes and says, "Okay, I've done the thing." And then it says, "Okay, great. Go and build this thing." And you know, and do all of the things I said to build this thing. And it goes, "Oh, okay. I'll do it again." And and the the groundbreaking nature of what that meant was that the uh AI would often review its code and realize it had missed something in some way, right? So, it figured out that uh there wasn't it wasn't quite finished. And this is quite a common problem with AI coding tools last year. It wasn't quite done. it didn't quite get to the end and therefore it go oh yeah I should have fixed that bit and then does it again and then and then when it stops and says right I'm definitely finished now 100% it's done it's finished and then what do you do you give it another prompt again say go away and build the feature it's like I've built the feature and tries and looks again oh yeah there was actually this tiny thing that I should have done I really am now finished and so on and so on so you can kind of see the utility of kind of going through that loop um where you just build the feature and just then ask it to build the feature and then you ask it to build the feature um So, so that's kind of the first stage of RF loops. And what I'd like us to do is I'd like us to try that. So, firstly, I'm going to uh do a bit of live coding. Hold on to your hats. Uh we'll see how that goes. And uh we're going to try and do that process using claude code to see where that takes us. So,

Sorry, just takes a moment.

Okay, great.

Uh, this one, can everybody see that?

Okay, can people see that in the back?

Okay, do you want me to increase the size? Got thumbs up. Great. Okay, so this is a piece of code that I vibe coded in about three minutes last night.

So, it's not good, but we'll that's the whole point. We're going to fix it. Um, so it is literally a Pomodoro timer, and you can see how it works. So, if I go to Python and type Pomodoro start, woohoo, we've got a Pomodoro timer. Uh, that's all it does. It literally just does start. There is no way of finding out whether it's finished or complete or anything like that, but that's what we're going to change. Um, the other cool thing which is very important for any self-respecting vibecoded AI project is that it has tests. So, look, it's got a test.

There's one test and the check to see whether it starts. So, that's great. So, if we just have a quick look, and you'll have to forgive me if you're not a Vim fan because I am. Um, we although I hardly use it now, it's quite sad, you know, 20 years of muscle memory just gone. But, um, but yes, so all it does is it literally just runs a start command and then it saves in your Pomodoro uh Pomodoro in your home directory. It saves the time in which you started. Really, really simple. So, this is a very simple, quite straightforward project. The difference is that it has a new folder with different things in it. And these are tickets. So there are a whole bunch of ways in which we could improve this pomodoro timer. Um and the first ticket is it would be really nice to know how long is left on our pomodoro rather than just starting it. Um so I' what I've done is I've created a very simple ticket system to allow us to just kind of capture some changes. These are not u this was oneshotted. So I have no idea if these tickets are actually good. In fact, I haven't looked at some of them.

So we'll see how that goes. But um but the idea is that uh we can use these in order to start building a loop of work in order to get something done. So um what I'm going to do to start with is I'm going to start Claude and I'm literally going to say write the first ticket. So bear with me while Claude fires up.

In some ways I'm quite glad they didn't actually release Mythos yesterday because I think I don't think it would be working today if they did. Uh that is really not working, is it? That's

&gt;&gt; There's a problem with Wi-Fi.

&gt;&gt; Oh, okay. I mean, that could cause some problems to my talk, but we'll have to see how that goes. I don't have one of those fancy new um Macs that allow you to run. No, this has actually locked up my computer. Can you believe that? It was working literally 10 minutes ago.

Uh let me just have a look. Uh bear with me while I debug my machine.

&gt;&gt; Um there is um I've got I think I'm on a different Wi-Fi, so it should uh No. Yeah, the Wi-Fi has gone down.

fun &gt;&gt; tethering time &gt;&gt; might have to be. Okay, hang on a second while I tether to my phone, which I think has decent 5G, so we should be good. Okay, let's see if that's any

Okay, let's try again. Uh, not that one.

Um, cool. So, uh, code Pomodoro workshop. Okay, is that big enough?

&gt;&gt; Okay, good. Let's try this Claude via the power of 5G. Look at that. It works.

Fantastic. Okay. So, um, what I'm going to do is we have, as I said, a very, very simple, stupid Pomodoro timer. And we're going to implement a ticket. So, what I'm going to do is I'm going to say implement this ticket. So, it's in doc tickets 001.

Great. And I'm just going to say that.

See what happens. So, what's going to do is going to read the ticket, which I showed you briefly earlier. It's it's um very straightforward. And all it does is it implements a status to see how far we've got. Um and then what I would like it to do, what I'm going to do after this is I'm then going to say when it's done because it will literally be two files. It's not going to be difficult for it to do. And then I'm going to say, you know, implement it again and see what happens.

So, there's a few different ways that you can do this. Um and there is no kind of one set way of doing a Ralph loop.

It's really about the concept, not about anything else. Great. So, it's done the ticket. If I just quickly do a quick get diff, you can see that what it did is it added a status command. I think when I had a show of hands earlier, most of you are coders. So, hopefully this is not tricky to follow. And then we've got a new test. Look at that. We've added a test. It didn't even ask it to. It added a test. Oh my gosh, what is the world coming to? So, um, now what I'm going to do is I'm literally going to say the same thing. Now, a year ago, this would have been really important step because it would have definitely missed something. Um whereas now it's like you already done it. It's fine. Right? So Opus is now much better at at noticing when things are done. Now a traditional Ralph flute would just keep doing this, right? And it would keep going with this kind of implement this ticket, implement this ticket, implement this ticket. And um and this is kind of boring and it's not really going to do very much else.

Um and at some point actually when I tried this earlier, uh it's interesting.

It's done something different. It actually noticed that what it should have done is it actually should have updated the status to done. So the process worked that didn't work earlier.

Uh so that's great. So it's actually noticed something that it didn't do. So there you can see the fundamental early principle of early Ralph loops, right?

The idea that you can just zoom through and and do something and it will find things eventually that it missed.

Because it missed that. I'm just going to try once more, but I don't think it will come up with anything else. Um like I said, latest models really don't need this step in quite the same way. Um they they tend to just kind of get it done.

In fact, this time it's just like, um, oh, if you're running a Ralph loop that picks up the next ticket.

Oh, that's hilarious. It's literally giving away my presentation. Um, that's fantastic. Okay. Um, what I'd like us to do is I think as a starting point, the other thing you can do is you can just kill the context and then you can do the same thing again and you can say implement doc tickets 001. And I can't bother to spell it. It'll find it.

Um, and then, um, so now, uh, what I'm doing is basically doing the same thing, but without it knowing about the previous context. So, it' be quite interesting to see what it does with this. I'm assuming it'll find assuming it found the ticket. Yeah, it did find the ticket. That was easy enough for it to do. It's just running the test to make sure that they work. And it all passes. Okay. So, it's happy. So, some people when we when we first started using Ralph Loops is that they weren't doing it within the same status. And uh there was an early claw code plugin that just on the stop hook which is which is what runs right now when it stops running it would just do the same command again. And so rather like me just typing the same thing in each time but that didn't really work very well because it didn't get very far. Whereas uh now what's um more useful or or what people started doing was just running claude code in a kind of loop. So they would do something like while true um and then do claude um implement ticket 001, right? And then done. And then that will just go through. Uh not quite actually because I didn't do claude P, but effectively that's that's what they were doing. Um oh no, now I've really screwed it up. I really shouldn't have press hit enter on that, should I? Okay, there we go. So that's but that's effective what people were doing. The dumbest Ralph loop is literally that just a while loop and it just goes through and implements stuff super super easy. Um so um what is the next step in a RA loop? Well, in fact what we're going to do now is I'm going to get you to get to that point and then I'm going to take questions from other folks. So um let me just switch to back to here. I'm hoping. There we go.

Yeah, great. Um, so what I'd like you to do if you could crack open your laptops and um, grab the code from here. So, um, it's just on my GitHub uh, as Pomodoro workshop. You should be able to find that quite easily. Um, and you saw how I ran it. It's very simple. Uh, you might need to set up Python in your machine.

So, there's hopefully that won't be too hard or you just run bare Python.

Pomodoro.py will give you the command you can type and then it's um, just a unit test thing to run the test pomodoro. Super easy. And then in step four, I want you to fire up Clawed Code or Codeex and I want you to try and build that ticket and make sure that it's working. Um, and that will be a great starting point. Um, but don't build any more tickets yet. Don't don't let it take you too far. And then, uh, if you are really used to that and that is just like a literal no-brainer for you, try in codeex, try in something else. Try in maybe try setting up something similar in one of your own projects. Um, so something different.

So, I'm I'm going to take questions now while people are typing away on that.

I'm going to give you maybe a few minutes just to get that set up and then we'll kind of move on to the next step.

Does anyone have any questions or comments or thoughts? I have a microphone here.

Uh if people would like to ask anything.

there we go.

&gt;&gt; It should come on in a second.

&gt;&gt; Hopefully, the guys at the back are It may not be on. Is it on?

&gt;&gt; Yeah, you could shout and I repeat.

Yeah, that could work.

&gt;&gt; Is it? Can I just check it's on first?

&gt;&gt; Yeah, it looks on. That's weird. Shout and I repeat. Anyway, oh, there we go.

There we go.

&gt;&gt; Great.

&gt;&gt; Um, I've I've played a bit around with the BMAD method, which I don't know whether you've seen that. Um he's got a guy a guy who's basically written a whole load of skills and commands for following a full agile process from &gt;&gt; you know and he's got an agent for build it test it &gt;&gt; you know everything and and I I guess &gt;&gt; so um have have you done anything where sort of using this kind of Ralph loop process you go through that cycle you go through the full software development life cycle of each stage and then Yes.

Um I I might get as far as that at the end. Um but yes, I have tried some of that stuff. Um it's really interesting and it answer it asks some really very good questions both around context and and actually the value of the work. It's really interesting. So yeah, we'll talk about maybe that a bit more at the end.

So ask again if I haven't got to it, just ask the same question again and we'll get there in a raffle.

&gt;&gt; Okay. Thank you.

&gt;&gt; Um anybody else got any questions?

How are people getting on with setting that up? Have has anyone managed to set it up? You know, kind of wave at me if you've managed to get it running. Great.

Great start. Has anyone managed to implement the first ticket? Hooray. A few people. You got a question?

&gt;&gt; Oh, go. You've done it. Great. Fab.

Yeah, I probably should have given different directions for asking a question versus having finished. That's great. So, a few people have got started. Fantastic. Great. So, you can probably tell where this is going. And if you were paying attention to the live demo, you'll already know the answer.

I'll grab the mic from you so you don't have to keep holding that. Thank you.

Um, but yes, you don't have to just stop at one ticket. Now, is Matt PCO happen to be in the room? I know that he's doing the workshop after this one. He is the person I got this from. So, if he's watching the video, thank you, Matt. Uh, this was a this was a a revelation to me back in sort of September last year.

Posted a brilliant YouTube video just about exactly how to kind of take Ralph loops to the next level because when they first came out, I spotted it on the internet. I played with it and I was like, "Yeah, this is fine. It's kind of cool. It kind of spots things that AI can do where where it's missed things and it can maybe do a slightly better job of things, but it's not going to change everything that I do. Um, and then the answer is actually it does change the entire way that I work and approach code now. So, um, I guess the the really interesting thing is not how do I make sure Claude has finished this one thing. It's what happens if I point this kind of loop at a whole pile of things to do, right? What happens when we point at a whole list of things? Now, I tried this. I I wrote a blog post about this, which was a bit depressing because it just showed abject failure in the entire post to be honest, but it was more about I tried what I tried to do is I tried to get um Claude, I think it was Claude at the time, to break up a big project into a lot of different tickets.

And then I got it to break down all of those tickets into smaller tickets. And then I got it to to figure out what all the dependencies were between those tickets and write them all down really carefully. And then I got it to figure out how I it could use like a ton of different agents. Sorry. Did you have a

Uh one, two, one, two. Okay. I have a problem with I had a problem with Wi-Fi and I didn't do the clone.

&gt;&gt; Oh, I'm sorry. Go to this one.

&gt;&gt; Yeah. Yeah. Couple of minutes. Yeah.

Thank you.

&gt;&gt; Yeah, that's fine. I'll leave it on there. That's fine. Um has everyone appreciated the slide. Okay, good. Okay, there we go.

&gt;&gt; Um so, um these slides, by the way, are created using a slide skill using Nano Banana Pro. It's absolutely incredible at making slides. Um, I didn't I haven't apart from like the tiniest thing like adding a QR code, I haven't added any text to these slides. They're just flat images in Google Slides. Um, they're actually it's incredible at making slides. Um, what was I saying? Yeah. So, uh, so the idea of just creating uh a a Ralph loop to just do one thing seemed a bit pointless and and it was just working around a few limitations.

If you point this loop on a whole pile of work, then it becomes incredibly uh powerful. And as I was saying before, I created this huge complex dependency graph with a whole ton of different tickets about how I was going to build this really complex system for me. And then I got I fired up like six or seven parallel agents and I was like, "Right, you do one stream and you do that stream and you pick up this ticket." And it just failed horribly because the the system just couldn't figure out what had been done and what hadn't been done.

Picked up there was lots of contention between tickets like well I can't do anything until you until I get that share ticket done so I'm going to do it.

And another Claude was like well I can't do anything until that share tickets done so I'm going to do that too. And then they both implemented the same thing and it was a huge mess. So that was really very depressing and I wrote a whole thing about it and I was basically like it's impossible to orchestrate large numbers of agents you know you just can't do it which was obviously nonsense but um that's how I felt at the time and what was interesting was that what I'd done effectively was recreate the waterfall processes that were seen in some of the worst companies back when I was starting to code for myself in the 90s where people would write requirements documents that you had to stagger to carry into the the requirements meetings.

uh where the the entire project was specified up front with all the intricate dependencies handed to the development team and then given two years to build. I can see some perhaps um slightly more seasoned people in the room sort of nodding and smiling at me when when they hear me talk about this but yeah I thankfully managed to avoid working on any of those teams but I some of my friends did and it was absolutely awful. And what I had done is basically I had given that to Claude to do. I'd given that waterfall process to Claude to to um to organize and figure out as as it went, which was really bad. So no wonder it didn't work. If humans can't do that, how was how is AI supposed to do any better? Um however, if instead of saying with all of your tickets, right, the first one is the most important, then this one, and then you should do this one, but don't think about this one until you've done that one. Instead of doing that, I'm going to go back for just a minute. Are we all good with this slide? By the way, does anyone still need the slide? Okay. Okay, we're good.

Right. Uh instead of doing that, you can just run a loop where you say something like, "Hey, just pick the next most important ticket." It's as simple as that. Just figure out here are all the tickets. Just figure out what is the most important next one to run. Okay? You don't have to worry about the dependencies. You don't have to figure it out yourself. the AI is quite capable of looking at all of them, figuring out the dependencies on the fly based on what's just been done and figuring out what the next most important thing to do is. That's actually quite easy for an AI to do. The one thing it cannot do so easily is manage that process in parallel. But to be honest, when we're running these kind of loops, if you're running them continually, the bottleneck is usually not the number of agents. It's usually you just keeping up with the AI just doing things over and over again. So let's forget parallelism just for a minute and just start with a loop. See, if you can keep up with an AI, just one AI that's running continuously, you're fine. Don't worry about parallelism just yet. Don't worry about gas town, any of that stuff just yet. As impressive as those projects are, you can just start with a simple loop. It is okay. So, um, what I'd like you to do again at this point is I'm going to quickly show how this works for those of you um, who don't have your laptops, but then I'd like you to just try it on your

great. So if I go back to Claude, in fact, I'll go back to Vim first and look at the tickets folder. So I've got a whole bunch of tickets here. I've got a status command, I've got a stop command, I've got custom durations. Never use that anyway. I use um other things like you know labels and all of that. Um I could try and figure out the dependencies myself but I really just don't need to. I can just simply go into Claude and say implement the next most important ticket using uh TDD

from doc tickets.

um commit

So, it's now reading a whole bunch of tickets. As you can see, it's read number one, two, and three, and it's decided that the next one is number two.

It's just going to do it. That's great.

So, it's going to work on it. Um, now the interesting thing now is that once this is finishes, now it's using TDD, so it read the test first. Uh, when it finishes, hopefully, yeah, it's marked it as done. Very good. Remembered that time and then it should commit. Let's see if

&gt;&gt; Sorry.

&gt;&gt; No, this is a brand new session.

Although I think it probably had the working directory from the previous one still. So um in fact I think it has. So what it hasn't done is committed those atomically which is definitely something I could improve in my prompt but we can cover that in a minute. Um so then hopefully it's just going to do that and then it's going to finish.

Yeah, great. It's done it. Fantastic.

Now what I can do is I can either do that again uh as a row loop or I can just restart a new session. Just do the same thing and this time it will pick something else and then it will keep working. Now you can imagine that if I put this inside a while loop then it should in theory work through all of the tickets in some way. Now whether what I get at the end is actually what I want is a whole different question. Um but it will definitely get a lot of work done in a row. So I'd love you to try it. So, if you're if you've got the um app working on your computers, um see if you can get it to work through just as many tickets as you want to within that amount of time. Um so, it should be able to just carry on. See if you can get it to to actually um uh maybe write a little bash script um just like I've done where you do a while, true, and then claude. I'll show you how to do that briefly. In fact, if I just quickly get reset so it can start just from the beginning. Um, and in fact, I'm actually

hard head. There we go. Great. Yeah, that's the right place to start. Um, so, uh, if you can get it to do that, then that's great. The other thing that you can do is instead of using claude like this, you can do claw-p. Can you all see that?

Okay. By the way, I'm not sure how I can make that. Um, &gt;&gt; yeah. Yeah, clear is a good one. Yeah.

clear claude. There we go. So, what we can do is actually use claude.p like this. And you can get it to output by just doing stream JSON or something like that.

What's it called? Something like stream JSON.

Um, hang on a second.

I think they've removed it. That's annoying. Never mind.

which won't see any output.

So there's nothing to stop you setting it up like this and then you can just do that.

&gt;&gt; You got to set up CL to have full permission so it doesn't properly.

&gt;&gt; Yes. So the only way that this works is if you want to run this properly and for it to not stop, you have to be quite selective about the permissions that you give it. Um so the question was presumably you have to run claw with full permissions for this to work. Yes.

Yes, you do. Um &gt;&gt; it depends on what you're doing. Yeah, if you're working in a little sandbox project like this, the chances of it going elsewhere to find stuff out is is very small. Um, I have a project called lockbox and the sole purpose of that is to try and stop it doing stupid stuff um by why when it reads untrusted tokens which could potentially send it off track, it um it basically just prevents any kind of file system access or anything after that. So, there are ways of of kind of managing it. Um, so what this is doing in the background is you can't actually see it doing anything um because I don't have that that output mode, but you can figure out basically that to to run this in in some kind of script. And if I in fact if I quit that um hopefully it will stop.

There we go. No, let's just keep going. Sorry. Clearly clearly a more productionready Ralph loop would not look like this. But you can see it's done a bunch of work. So if I go to here, you can see it's already started on the status command. Um, and it's just working through that at the moment. So it started at the beginning again. It was just working. So um, there's a few things to be aware of here. One is that feedback is really really important with Ralph loops. Um, you know, you need to be able to have it run uh um in a uh in a way that you can tell what it's doing and how it's doing it. So this kind of super basic one that I've given you there isn't very good.

That's not one that I would recommend running in production. Um, equally, you need to um figure out exactly what the prompt is for Ralph. And that's a really, really important point. And I think what I'd like you to do when you're trying this is yes, it's going to be building a bunch of tickets in a row, but equally, it's going to be um uh it's going to be doing them in a way that you don't like. Um so for example if we go if I'm if I'm running this test which is literally just implement the next most important thing uh let's start with this one um I would probably do something like um run simplify which is a really useful skill from Claude from the anthropic team uh when finished and ensure you refactor to reduce duplication you can imagine Imagine that you could create quite a complicated um skill for this. And I'll show you I'll show you my kind of actual skill for this at the end. But as you're kind of working through this, do try and figure out um if there's ways that you can improve what it's doing. So give it a go, let it make a decision, and then let it write some code and then read the code and think, okay, what could I have actually improved about that process?

And then reset everything and then improve the prompt after that. um have a go at that and see how that works.

Whilst people are kind of working through that on their machines, I'm happy to take questions.

&gt;&gt; Have you used the skills like superpowers? Uh &gt;&gt; which one? Sorry.

&gt;&gt; Superpowers one.

&gt;&gt; Uh the superpowers one. I what I did is I pointed Claude at the entire repository and said figure out anything that isn't currently in my skill set and um implement them for me with my own context and that worked quite well. So I haven't used those ones particularly but I've basically rip them off.

&gt;&gt; That's great. Um then because I use superpowers a lot and then I just give tasks like this and then ask it to like &gt;&gt; run multiple agents in the background.

Yeah. And have you done that is what I &gt;&gt; Yeah. Yeah. So what you can do is there is an agent teams version which I think I've got turned off in this particular instance of cla code but what can happen is you can get uh claude to um use uh sub aents within team. So in fact I think I might be able to turn that on if I can find the agent teams. There it is.

Claude code experimental agent teams. So if I grab that and then run Claude with that on then you should be able to say use an agent team to implement the dock tickets in this repo or something like this. And I don't this isn't actually running within T-Max. So um so actually maybe this won't work. So I might just try this again.

Bear with me a second. So if I grab that and then paste that there. And then grab that and then run T-Max.

And then run that in theory. In theory, um this should uh start pulling up other agents and it and because like I said trying to orchestrate uh Ralph loops or orchestrate agents myself to try and organize all of the dependencies and complexities is actually really really difficult to do. But um what you can do is just give the job to Claude to do and it does a much better job of managing that for it. So as you can see it's already decided to print out the entire thing, the file name and the the ticket for each and it's got a whole bunch there. So it's actually decided that they're all sequential. So therefore it should run it should run none of them in parallel which is kind of interesting.

Um and then it should in theory start an implementation agent. Let's see if it's going to uh I think it's just running as a sub agent. Never mind. I'm not sure that's going to work. Never mind. If you can get it to do it, then let me know. But but but basically, it's an experimental feature that only came out a few weeks ago that allow um it to kind of start sub agents include as well um in order to do things.

Any other questions while people are working through that? Yeah.

&gt;&gt; You said you built a rough with automation, right? So what was the feedback criteria? So like what decides if it's a good newspaper article like a website framework but what good looks like if that's dynamic if that's static do you put that in the cloud MD file?

&gt;&gt; Yeah great question how does that how does that work? So, so um the question is just to to repeat the first half of that is when I used the NA10 workflow in order to build a Ralph flute for a newsletter creator, uh how did I know how did how did the agent know what good was when it come?

&gt;&gt; How did I define good? Okay, great question. So, so in terms of newsletters, I had already been writing my newsletter manually. So, I knew roughly what I wanted it to read like and sound like. I also did a bunch of research um using a research skill which was something like which I built which is something like um great newsletters and I've also did things like that. I also said things like um this is a fantastic written new in fact I'm just going to this is not what I do I actually do this is a fantastic newsletter that I've written or that I've read somewhere could you please figure out why this is so good and what are the kind of editorial principles that go went into this newsletter for it to to work really really well and then I would just paste that into paste the newsletter in get it to figure out what what was good about the newsletter and then and then I would check it and then I would say yes there's still is an element of human taste here. You can't entirely get away with that. Having said that, I do also have a simulate audience skill which um basically uses a whole bunch of different personas uh for different clients or or prospective clients that I work with. And then I would run it run the finished newsletter through that and say run all of these in parallel um and then once you have finished that figure out ways that I can improve this newsletter or newsletter skill in order to do that. So there's a number of different ways you can do that. the kind of audience simulation is super experimental, but it's actually really effective and and often will will surface insights I just hadn't thought of. Um I'm u my my personality as I'm a bit slightly all over the place, slightly kind of the way that I talk and and communicate um often my clients are not like that. So um I tend to barge people with information and sometimes my skill will say okay there's a lot of ideas in this Chris you just need to focus on one main point that makes sense and I'm like that's so helpful. So um so yes what's quite helpful and interesting is that you can use AI to give feedback on AI like that. The great thing about this particular um project that we're writing this little pomodoro thing that people are writing is that it's a command line tool and it's really simple to know whether it works. So it's perfect for a Ralph loop and in fact these little tools that we build for ourselves like for example the newsletter skill perfect for this kind of loop. I I will often say, "I want to improve this skill. Could you please back and forth and and write the content, then use another agent to read the content, decide if it's any good, come up with things to improve, then send that back in and just run that as a loop. Um there's a really cool um I wasn't going to tell you about this until the end, but I'll tell you now." Um there's a really cool uh feature inside code called loop where is instead of um creating, in fact, I'll start this in a new session. Instead of just doing this thing where you have to create your own while loop, you can say loop every minute um build the next ticket from doc tickets basically. And then what will happen is um the loop will set up a um a kind of almost like a a repeat timer. And as you see it's it's got a cron create tool which for the uninitiated just means do something every minute. This is what those five stars mean. And um what it's going to do is it will literally just build the next ticket. It when it finishes it will then check the chron again, build the next ticket. When it finishes it will check the chron again and keep going. So that's great for working through a bunch of tickets, but it isn't just applied to a set of things that you've got from before. If you think about it, I'll just leave that running up there. You could have a loop that does something like this. loop every one hour check linear for new bug reports from test.

Um, just leave that running and um, you're going to get you're going to annoy your testing team. But but anyway, the point is is that you can run these kinds of loops in order to get work done in a in a in a quite an interesting, I guess, dynamic way. Even though it's quite a simple loop, it's just find the next thing, do the next thing. If you think about it, heck of a lot of our work is just loops. If we're software developers, what do we do? We look at the backlog. We pick the top thing from the backlog. We pull it over to, you know, in progress. We assign it to ourselves. We check on the architecture.

We figure out whether there's other contexts we need. We uh look at the change. We we make the change. We submit a PR. We um uh you know wait for reviews. We uh comment on the reviews.

We reject the reviews. We implement the changes occasionally. We submit the PR.

We merge the PR. We then go through the release process. Then we start again, pick up the next ticket, and so on. That is a loop. It's quite a complicated one.

Um like we were talking about just a minute ago, but but it is still a loop.

It is possible to get an AI to run that entire loop. there's no reason not to.

Um, and that's effectively what's happening here when when um you can set up in fact you would never actually write this. You would much more likely to write something like this where you'd say every one hour uh linear bug finding and you'd have a skill that encoded all of those um those chunks of information that I just gave it in a way that would work for you and your particular team. Does everyone does anyone not know what skills are before I go any further? No. I think pretty much almost everyone knows what skills are.

If you haven't figured out what a skill is yet, then then this is your homework.

Go and understand how skills work. They are the best uh way that we have at the moment of packaging up useful little parcels of context and scripts and moving them to different places or or creating different things. So, for example, I mean, I have about 50 of them that I've written. Um, and then they just do lots of different things. The great thing about skills is that you can pull them into your context whenever you need them. So, for example, and I'll just do this. I could say, "Do you know how to create images using uh nano banana?" And I can ask um the AI the question, and the answer is, well, I could kind of look this up um but it actually knows that I have an image skill for this, funny enough. But if you hadn't got one, it wouldn't know. But if I then do images and say uh how do you create images? Give me the step by step.

Then and what it's going to do is it's going to pull in that images skill. Um and then it tells me exactly how it does it. And I've actually written in fact I will make that bigger so you can see I've actually written a script within that skill that actually does the generation for me. Um, so it's codified the process of doing that and it just picks whichever um, uh, model it wants to and it gives it content and I have these specific templates that I use in order to create specific Nanabanana um, skills. Nanaban is brilliant. Um, this is how I created the presentation that you're looking at. I have a slide skill and an image skill that work in tandem in order to create these presentations.

Cool. So let's see what the other thing has done. As you can see, it's already on ticket six. Um, the great thing about Ralphs is you just keep working and keep talking about something else and it's done a whole ton of stuff here. And it's just stopped at this point, but in a second, um, hopefully if we just wait, it will start the whole process again.

There we go. It's got the scheduled task to run and it's going again. Um, so you can just you you can just leave claw code sessions uh running with these kind of loops in them. They last about three days, so you do have to keep refreshing them. Um, but you can you can just do that and keep it running even before you get to a more complicated write a script that wraps claw to do a thing and all of those kinds of things. Um, any other

Anyone got anything interesting or surprising out of their Ralph loop? Has anyone tried this on their real work yet? This would be the interesting thing. Yeah. How what was your experience? Have you still got the mic?

&gt;&gt; Yeah.

&gt;&gt; Yeah. Yeah.

I just made a screenshot Ralph loop for a website context engineing framework.

So Claude just takes the screenshots and then looks at the layout because it has problems with geometric like spacing.

&gt;&gt; It works well.

&gt;&gt; Nice. Cool. So you're actually using Claude Claude screenshotting to to get feedback. Yeah.

&gt;&gt; Yeah. That's pretty advanced. Not many people are doing that. The um people are trying to use um uh like um playright and things like that as well to take screenshots and the claudin chrome plugin that comes with claude as well.

can use that in order to get it to drive Chrome and then take screenshots of what's going on. I've had mixed success with that because it it's quite a complex thing for it to manage. But but for just basic screenshots, it works really well. For my um images and content uh that I write, um I when it runs those images skills, it will always look at the images first to see whether there's any kind of a weird AI garble text or whatever and it will reject them without even showing me if there's a problem with an image. Um was there another question or comment? Yeah, there's a question just back here. Can you just pass the mic? Is that okay?

&gt;&gt; Um I think it's it's close to a question that has been already asked. Um because I'm not quite familiar with Rough Loops.

If I ask the agent to implement task one that has already been implemented, would it actually check the quality of what was implemented or only check if it was done or not or marked or not? Yeah, it very much depends on what you set it up for. So, so there's no kind of magic to a Ralph loop. It's just a loop. So, this loop that I'm running at the moment, in fact, I probably should just say loop stop. Otherwise, it's going to keep going. Use my quotota. I think I think you can just stop like that. We've got a quite a fully featured pomodoro setup now.

Come on. Time to stop. Um, so it depends on it entirely depends on what you write. So, if you um if you go through to what was the loop that I set up? I think it was this one. I just said build the next ticket. That's very ambiguous and not very helpful. So, it might not actually finish it. It may just decide to build it and not ship it. It may not actually be very helpful. So, what's more interesting is if you go to um let me probably the easiest thing to do. If I load my my Ralph skill, this is my actual skill that I use um for Ralph loops. So um and what I'm doing actually this is slightly out of date. Um but the one that I've got here is actually using a doc changes folder.

Uh you can see that there, but um I using a doc tickets in this example, but I've changed it on the the latest one.

Um but um but ultimately you don't have to use a ticketing system like a flat file in a in a um in the GitHub repository. You could use beads which is Steve Yaki's version of of this kind of approach which is quite cool. I've used it. Um you could use linear, you could use Jira, you could you know as long as you can get access to it from the AI, you can use whatever uh ticketing system you want. uh for this for the purposes of this exercise that you're working through I tend just to use uh flat files because they just work you know it's not you don't really need anything sophisticated um in the same way um the uh the Ralph loop is entirely what you make it in terms of its effectiveness so so for example um in this particular one um I've given it a proper kind of role in the sense that you are one engineer in a relay team do exactly one change then drop the context and stop start Again, that's the idea. So, for this one, it's designed to be run in a shell script where it has an entirely fresh context each time because I didn't want the context to pollute each time. These days, I care much less about that because context is so much larger than it used to be. But, um, but when I wrote this, that was very important. As you can see, it's specifically for code that doesn't need human review before shipping. Um and then basically um it tells you about when work should go in there. Uh what the right time for the tool is read the claw.md change the format. This is the format of a ticket.

Um this is all of the rationale. These are the different status values. Um I ask it to check git state um to make sure that it hasn't got a working directory. Um it's also got you know recovery states. So if it crashed, it knows that if there's a a dirty working tree, but the tests are passing, but you're probably done, um, but you might not be, so just double check. If the tests are failing, um, then it's probably just mid-flight, but broken, so you should probably just throw it away or or just treat it differently. So, so you can you can imagine that this was built up over time of trying to trying to get this working, trying to understand what the user wants, make sure test passing is not enough, verify the actual behavior works, run things in parallel, mark it done, blah blah blah blah blah. There's an awful lot going on. Um, so with a real Ralph loop, you want to be building up over time for your specific project exactly how to check something is working, exactly how to run the test in your particular framework and dialect, how you submit things to the test team, how you want to uh comment on particular changes, what style you want to use, whether you want to um pull off the thing that feels most obvious to you or the thing that's highest priority or a mixture of both depending on how you're feeling that day. whatever whatever you want needs to be coded in it. So when you're writing a Ralph loop, um I'll show you a link to grab this one at the end, but there's no need to use just mine or or something else. Just start with mine and then say fix this for my project and and and allow it to change and morph and evolve.

Couple of questions. So um just come forward. Thank you.

&gt;&gt; Hi. Um thanks for the talk. Um could you expand a bit more on the topic of sandboxing because that would be the thing uh stopping me from running this.

Uh &gt;&gt; yeah, it makes sense. Yeah. Yeah, absolutely. So there's a number of different ways to sandbox this. For this particular small project, I'm not doing that. Um most of my work happens on a VPS which is away from my main machine.

It has a few keys on it that are specific to what I want it to do. And um it can access developer tools. Uh a lot of them it can only access them readonly. Um, it can also access my email, but again, it has quite strict fine grain claw permissions for not sending emails because that's quite important. It I don't let it ever send an email. I only ever let it draft them.

So, um, so I use a combination of uh positioning the code physically, not physically, but like away from the machine on a different machine on a VPS.

Uh, I use clawed permissions for that as well. The permission system is a bit broken, but it it mostly works. Um, I'm trying you to build lockbox to make it even better. Um I use um what else do I do? Uh so the keys that I use are separate keys. So um the AI has it access to its own keys which I don't use for my other stuff. So I can I can see the kind of audit trail of what it's done. So there's a number of different ways of doing it. Um if you want to just run things simply on your own machine, there's Docker sandbox which is quite cool. It's a new feature in Docker which just allows you to do Docker sandbox cla and a run claw within that sandbox. Um, so you can kind of isolate it within a specific container. That's quite powerful because it allows you to only change things within that specific place in the file system. The challenge with that is that it can still leak data from one of your systems to another of your systems. Uh there's a thing called the lethal trifecta. I don't know if you've heard of that. Simon Wilson uh coined it. um an idea that if you have untrusted tokens, uh internet access and access to secret important data you don't want to lose, you're going to lose that data. Basically, that's that's the the um the bottom line of it. Uh so you have to kind of minimize the amount of times that those things collide in the same context. Um so yeah, lots to say about security and sandboxing specifically. Um, I tend to run I don't run with dangerously skip permissions.

Uh, but I do run with with um a number of things turned on by default, but not everything basically. And you kind of have to go through and figure out what what your risk profile is and how much you care about those things. And certainly as you're giving, the main the main things to to read up if you're interested is to read up about the lethal trifecta if you didn't already weren't already aware of it. And um kind of be thoughtful about how much power and permission you're giving to your agents, especially if you're using something like OpenClaw, which is unfortunately insecure by default. Um I know that they've been doing a huge amount of work um on OpenClaw to make it more secure, but it is still a challenge for those kinds of agents. They do have access to a lot of things. Any other questions? Yeah.

&gt;&gt; Uh yeah, you had a validation step in the loop.

&gt;&gt; Uh this might be anecdotal evidence, but as soon as I changed mine to use sub agents here now for the validation step, it started finding things.

&gt;&gt; Ah, interesting.

&gt;&gt; Whereas as long as you're doing the validation in the same step with the same context, it just pats itself on the back and like Yeah, it &gt;&gt; that's a really that's a really good point. Um there's definitely uh confirmation bias going on with agents where they're like, "Oh yeah, of course I wrote it fine. It was fine. I checked it a minute ago." Uh yeah, using sub agents is really powerful because a sub agent starts with only a small chunk of context. It doesn't start with the full context, right? So so you can get much more uh power from it. So as a good example from this particular project, um a really useful um skill which I mentioned earlier is simplify.

Simplifies a claw coding bundled skill and what it does is it will look at the most recent changes and it will run three sub aents to try and figure out what whether your code should improve.

So you you can see here what it's doing.

So hopefully this will run these will load and it will probably find a bunch of problems.

&gt;&gt; Great presentation. Thank you. Uh did you try open spec or combined with openspec or any other spec driven? No, I'm I if I'm honest, I'm not a huge fan of spec driven development. I know that's that's um controversial and I'll qualify that. I'm I worry that spec driven development is taking us at the extreme is taking us back to the bad old days of waterfall where we would specify the entire or try and overspecify a project. Even these little set of tickets I'm not that comfortable with. I feel like spec should be much more iterative um and things that we can see it's already fixing a bunch of things that's quite cool. So it found just to finish off that point it found a bunch of issues there um and it's got some fixes. Uh yes so specs um I like just in time specs. I like the idea of building or thinking through what you're trying to build creating some kind of plan and claw code and then executing it. That's fine. I'm happy about that and I think that's a useful step. What I worry about is a I I worry about things like Kira where they've codified that into the to the tool. I worry that that will almost fossilize that that one approach with with AI that works today but may not work again when mythos eventually comes out. Right? So I worry that the tools are being too quick to jump to a specific structure of work that may not be the right thing in the future. So I'm I'm cautious about that. Um, I think it is obvious. It's a it's a truism that AI needs more context in order to do well.

So, we should try and give it more context. But I think the idea of overdoing that and and oversp specking a project is one to be careful of as well as overstructuring our process based on what we know about agents today because then we'll end up with working on with a a new kind of AIdriven process that worked best with agents that came out in 2025 or 2026. You know, when we'll still be using that in 2030 and that'll be a pointless waste of time. So those are the kind of concerns I have with it.

&gt;&gt; Yeah, great talk. So uh you mentioned that you don't like spec driven and you use Ralph. So basically there is no human in the loop. So the question arises does clo actually need you there.

Where where is your input there?

&gt;&gt; Great question. Um so I've been thinking about this quite a lot recently and having a bit of an existential crisis. I don't know about anyone else. Um but um but yes, what what value am I adding here to this thing? Um certainly not with writing a Pomodoro timer. I'm not sure I'm adding much value at all. I mean, I literally said I oneshotted those specs and and there was no point there at all. I'm not saying that I don't I don't like planning out a system. What I'm what I'm interested in at this point and I don't have the answers is the fact that AI often will pick better specs and and write better specs than I can write and will often have a better idea of the kinds of direction my software should go in than I necessarily will have. So I like the idea of actually having Ralph loops that create other Ralph loops um potentially or having Ralph loops that track whole um uh customer engagements or even whole startups. So, I have a a skill that I'm working on. Should I show this? I'm going to show it. It'll be fine. What could go wrong? Um, which is called startup.

It's pretty ambitious, but the idea is that it should um basically guide a product through an entire startup framework. Um, so it is it is meant to be run as a loop. The idea is that it Oh, I see you all taking pictures now.

Now I'm owning this thing.

&gt;&gt; Damn it. Um but with great thanks to Ash Maru who writes some brilliant stuff on this. I should say that for the for the for the tape. Um so really really helpful um to me. So I built this out of um basically all of the cool books I I've read about stuff. So I I'm I'm a startup founder, co-founder CTO. So so this is a near and dear to my heart. And what I'm trying to do here is I'm trying to give the AI enough context such that it could run my startup for me and potentially figure out what the next most important thing to work is and then do that in a loop, you know, and then it then there's a big outer loop that runs that says, "Okay, well, what's the next most important thing to do? Let's do that." Um, so it doesn't work, but it it's it's interesting and it's getting somewhere and um it will often um the first thing it does I don't think I've got it to show. Um but um Oh yeah, I will show it because it is hilarious.

Hang on just a second. Um, there's a it I asked it how it was doing uh on one of its loops and it produced a a startup update deck as an investor memo which was I didn't even ask it to do this.

I'll show you the the demo. Hang on a second. Um because it's absolutely brilliant.

Uh let's see if I can just show this window.

There we go. Air skills startup update.

Um, and so yeah, it said, "Yeah, I need to give him an update." So what I did is it said basically this is how far we've got. These are the problems nobody has solved. This is what we know that's real. These are the number of this is a skills management tool that I'm working on in the background. Um, this is these are all the kind of issues. And it came up with all of this cool stuff that could go into an investor deck. Um, to be honest, that's not bad. It's it's not a bad I think it's actually the GitHub for AI skills, but there we go. And um, you know, who's going to pay for this thing? How much will they pay? Those numbers are definitely not right. But um but what's interesting is that it decided that it wanted to do this uh and figure out all of these numbers based on based on this um which is I think was hilarious and it was quite proud of this deck to be honest and I had to kind of be like hang on a minute we haven't you there's some serious thinking you need to do before you kind of go to that um will will or pay for skills government would your all pay for skills governance great question um not sure yet so anyway um the reason for showing that is more to kind of point out that that AI can do a heck of a lot and and it doesn't do startups well yet, but that's probably down to my skill file, not down to the um agent itself. I I have a feeling that there are an awful lot of things that potentially will be um will be loops in the future. Um I only got that far on my slides. Oh my gosh. Um hang on a second.

Um so we've done that. We've done that.

If you are still if you're not just listening to me and are still working on this demo, I've got a couple of challenges for you if you'd like to do this. One is is um you could try upgrading your ticket format. Um if you like the raw markdown file, the doc tickets is fine. If you wanted to just type bd install or or install beads, it's super easy to do that. And you wanted to kind of get Ralph loop to work with your beads, try that out. See if that works. You know, no there's no pressure on you to achieve anything in this little folder. Uh beads is great because it only works within your folder. So um and it just installs a little tool. So it's quite a useful thing to to to try it on. So if you wanted to try a different ticket format or you wanted to kind of move this into your main project and connect your your Ralph loop to your um ticketing system to see how that feels. Yeah, maybe not submit tickets yet, but you know, you potentially could you could try that and see where that takes you. Um so that's an option. The other is is the skill. Uh you you're going to need to keep upgrading and working on your loop. Uh the loop basically contains all of the knowhow about how you as a person will go through that. You can take it all the way through from do the next ticket and you can take it all the way up to do the next step in the world dominating startup you're trying to build or whatever it is. Um you know it works for all of those kind of things. What's super interesting about this is that I'm I'm more and more convinced that everything in fact is a loop. Maybe maybe as an engineer I'm definitely on a loop on and a lot of the work that I do.

Uh maybe as a project manager or or project manager I'm on a loop. Maybe as a CEO I'm on a loop. Who knows? Um maybe uh certainly uh a lot of the kind of cadences that I work on um run in loops too. So um I have a skill and if you're running an open claw bot, you're doing a similar thing uh that that just runs a heartbeat every 15 minutes. Um, it just, um, on my, uh, VPS, it just fires up Claude, checks a few things, checks my calendar, see if I've got anything happening, and, um, sends me telegram messages. Um, maybe that's on a loop, and that is, it's definitely on a loop.

It's 15 minutes. Um, I have a worker loop, which I'll show you in a minute.

And I have a morning loop where every morning at at 6:00 a.m. it comes up with a full briefing of my day, figures out exactly what I should be doing, um, and, uh, just gives me all the information that I need that's happened overnight, all the emails that come in, all of that stuff. Um the the worker loop is particularly interesting because it basically I'm not sure I can actually show this. Uh let's see if I can find find something that I can show.

Um

No, the reason I can't is because it's got a bunch of client information in it.

So I can't show you that. But um what I can show you is for example this screen I can show you. So if I quickly switch to this

so this is an app I'll make that slightly bigger. This is now how I run my worker loop. So this is an app that I wrote um uh to manage projects. So I don't have tickets inside my my work vault. I have project files and each of the projects is a is a set of work that I need to do. And then every so often I have a I basically vibe coded a cambban system and a worker will pick up and do the next step on the project. So if the next step on the project is writing an email because it has a an overview or it's checking things or uh it's producing the slides for my project, it will do the next step. So this for example is the uh workshop uh prep spec uh uh project that it's working on. And it's it's got a bunch of front matter that is just looking like that. Um and um it's got some questions for me. I haven't updated this. it needs to be updated. It's got the context. It's got a decision trail of things that it's done and why it's done them. Um and so basically for every different thing that's happening um it it just is figuring out the next one. Um it's also got um notes on um other talks that I might be giving um that didn't happen in the end. Um it's got feedback from a previous workshop I did on a similar topic um which you can click on.

Actually, I can't click on that. There's a bug. But um it will basically show the notes from a feedback session. So so this project pulls everything together from all of the context you can find and then does the next step in a loop. So you can run everything in a loop. Uh you can run all of your work in a loop. When I wake up in the morning, normally I have about 15 or 16 draft emails where people have got back to me and it's had a go at replying them replying to them.

I always have to edit them. They're always okay. But um but it definitely has a go at at getting getting on with trying to schedule some of my work. I have very specific rules about what it can and can't do. My basic rule is is this reversible without embarrassment to me. And um if the answer is no, don't do it. Um and but just make a little note in the project and hand it back to me.

Um so sending emails is not allowed to do. Creating a slide deck like for example this one that's reversible. It doesn't cause me any embarrassment. So it just gone on and did it um and gave it to me. Uh for example, uh it doesn't post on LinkedIn for me. It doesn't um it doesn't send emails. It doesn't send messages. It uh but it does it does get everything ready for me to review. To your point earlier, which is a very long answer to your question, um it um it has caused me to genuinely question what I'm good at and what I'm here for. Quite a lot of the time I've got to a point where I'm just the email person who just checks emails and send check emails, send check. That doesn't sound like a a proper job. That doesn't feel good. So, so therefore, what does that mean for my work? And I've had to make a conscious decision. Which bits of my work do I want to do and which bits of my work don't I want to do. I don't want to be the email reviewer, but I do want to be the strategist. I do want to be helping organizations think through what on earth is going on with AI and how to kind of fix it for their organization.

Now, I could get AI to do a bad first draft, but I don't want to be reviewing AI's draft. I actually want to be doing that thinking myself. So therefore, I basically said, don't do any of that work. I want to do that work. just give me all the information I need and I'll do the work because I enjoy that work and I'm good at it. So, um, AI can do all of the rubbish work, but it can't and it shouldn't do the work that I'm uniquely good at. But because everything is a loop and Ralph, this is getting so existential because Ralph loops are are so um really everything and can be used for everything. We have to start asking hard questions about which of our bits of work we actually want to do. What do we want to do um out of this work? It's not just about what AI can do or can't do anymore.

Um, yes, there's a question. There's like loads of questions, but let's go at the back. I think your hand was up first. I think the um chat's coming with

&gt;&gt; Well, we just for the recording, it's really helpful. Thank you. So, with the open-ended tasks, yeah, how do you think about sort of when to stop? So, do you set KPIs up at the beginning or how do how do you how do you know when it's done?

&gt;&gt; Yeah, great question. Um, I ask it to so again this is this comes down to if I

uh this one it comes down to upgrading your loop and you have to basically tell it when to stop. So, uh I don't just have one Ralph loop file that works for all of those different loops that I showed you earlier. Um, I have different ones for each. And for example, the worker says, "When you get to a point where you've either running out of context or you've got to a point where there's an irreversible thing to do, then I want you to stop and and report." And what report means is in this case update the project file, which is just a file in a repository with what where you've got to and in a way that where you present it to me for review. So, I'm working quite hard at the moment about about that kind of presentation step because I I definitely don't want to be reviewing a diff and just reading text I find really difficult um just because there's often a huge wall of it and it's hard to pause. So, I'm getting it to start giving me things step by step in slide format. I'm trying to get it to that interface I showed you before. You can kind of see a little bit how I'm trying to get it to show things in different places in different ways uh to to for it to um uh to get to uh what I need to uniquely do next. I suppose so it so to the answer your question it depends on what you're doing. I think the most important bit is that you figure out what the edges are for yourself and and and have that real you know moment of you know what do I actually want to be doing? How do I want to be involved in this work here? not just uh you know AI is helping me do my work and a companion to me it's much more now which bits do I not even need to know about you know uh next question there's one here there's a mic just at the front somewhere I think yeah great

&gt;&gt; uh hi so since you brought up this topic of our involvement right so at what stage we really get involved I mean you mentioned you don't really review the diff um I guess the most important part of our work now is creating the tickets, right? I mean, first identifying what the most useful feature to implement is and then &gt;&gt; describing in a way that you foresee all the different edge cases or um just explain it the best way possible so that the outcome is what you desired in the first place.

&gt;&gt; Yeah.

&gt;&gt; What's your process of creating this tickets? Yeah, great question. And uh &gt;&gt; so like like my concern is sometimes you don't really know yourself until you start implementing. I mean the way we used to do it, right? So during the development process you encounter certain um different cases where you need like a custom logic you need to it's just difficult to foresee these things from the get go and do you like iteratively improve the tickets and you reimplement them or what's your process?

&gt;&gt; Yeah great question. So um in terms of how I work there are two modes of work that is done on my behalf. One is the fully automatic work that we're talking about here where the I would just get something done. I don't need when I've got a decent spec that I trust and and there's a way of feeding back so that the AI knows that it's good. I don't need to be involved in that work. That can just happen. For every other piece of work I do, I have a uh I I work on it with and in Claude code. So um I uh with that um system I showed you earlier, um in fact, I'll go back to it so I can show you. Um

With this one, at the very bottom of any of these kind of projects that it's running for me, there's a little thing here, which I this is just a vioded app that I've written for me. Nobody else has access to this. Um, it has a little vCP command which if I take that and I type this into a terminal window. Um, so if I go back to this one for example, I think this is okay.

yeah, I can't I can't easily show that just because my internet is not going to be able to connect to my VPS. But the point is is that I'm able to go back to um,

The point is is that I'm able to um type that in and just paste that into my VPS.

What that does is it starts a new clawed code session. That session grabs knows where to find that project and it pulls in all of the project context. So rather like loading a skill, it loads it as it loads the project. It reads the entire project file and knows where everything else is. At that point, it's loaded in everything it needs in order to supercharge that session with me and then we work together on it. So if I'm for example to your point about specking tickets, I'd have a ticket I don't know probably a project for that particular feature and then I would say okay load in everything you know about that and it would load them all in and then we would work back and forth on specking out those tickets and then the output would be um whatever I needed to get done in order to get that done. So if I'm working on something that where I I want to usefully and uniquely do that myself that's when I would jump into a project with claw code. And when I say do it myself, I don't mean typing it myself or I don't mean doing all the thinking. I normally mean I get Claude to interview me to ask questions so that I can uh give it the information it needs to formulate to do the writing because I don't like doing the typing, but I get it to pull the information out of me in order to to get that work done. So those are the two modes. It's the the back and forth iterating and then it's the it just the automatic stuff. And I should also point out that I don't I don't like reading diffs, but ultimately that's the only way that you can review code. Um when I'm when I'm reading a newsletter item, I don't want to read the diff. I want to read the newsletter. Whereas if I'm reviewing code that's important um that is for other people, then yes, I read the diffs. I don't like doing it.

Nobody likes reading diffs, but I I check to make sure it's working. And I will I can't see myself not doing that for a while, especially not with security um like conscious code. Maybe with mythos or just delegate it. That'd be nice. Any other questions?

Yes, one here.

&gt;&gt; Um, how do you deal with context rot?

So, for example, uh your example where you have a loop and it takes one task after the other. Is it the same cloud code session that takes all those tasks?

&gt;&gt; We'll have to experiment with that with the with the slash loop command. Yes, it is. Um, it's the same session. Um, I when you run it as a kind of while loop outside claw, then it's a different session. um you have different trade-offs uh with that. With the same session, you have all the context of the previous tickets and the previous changes. That might be useful. In practice, I've not found that so useful because it can just pull the files as it goes. Um if you're not typing anything into to the session, you're not really adding anything to that. So, there's nothing really in there that's useful.

So, I tended I've tended in the past to prefer starting a fresh context for each new session. Um, but the loop is very the slash loop is very easy to run and it just works and especially opus is very very good at long context retrieval. So it's less much much less of an issue.

&gt;&gt; Okay.

&gt;&gt; Uh, sorry. Yeah, there's a there's one at the back as well. Is there a microphone as well? Okay, great.

&gt;&gt; Um, are you reviewing sessions that are done by by your loops or or are you just reviewing diffs on on the GitHub? Great question.

&gt;&gt; I don't allow any of my workers to close a project. Um, so uh I I would always say if you think you're done, tell me what's finished and I will close I will I will close that off. Um, so it could it could be that there's a a big list of completed things that I need to check off check off for myself, but I I want to be that kind of final step of verification. The reason that I've added that is because I worry that I'll miss something. There's a thing uh that someone coined recently called cognitive debt uh which is the idea of just not being up to speed with everything that your codebase can do or or all of the code in your codebase. And that that worries me. So so I tend to want to to at least understand how the code fits together and and how or how the piece of work that I'm working on fits together.

So I don't let AI get away with just putting something, you know, out of my sight without me having a chance to look at it. Otherwise, I feel like I'd lose track of what's happening.

Yeah, because I I mean uh for example, I I'm using sessions to to track uh tickets.

&gt;&gt; So, so instead of reviewing the code or diffs in the code, I'm just reviewing what the particular session was doing &gt;&gt; and I even have like a marking system which which session is on which status.

&gt;&gt; Yeah.

&gt;&gt; Is there any way how how you do it similarly?

&gt;&gt; Um similar. Yeah, I think the sessions and the status I I I think that can work. Um, I I haven't tended to use sessions like that. What I've tended to do with sessions is I get Claude to every night go through all of the previous sessions that I've run that day across all the machines I run. Claude, it saves them all into a JSON file for me. And then I get it to uh both figure out how my system could improve uh and also just what I did so that I haven't I don't forget um what happened. And so it writes a little paragraph for how much I did. And um and I use that in order to uh to kind of track work, but it's not quite the same as one ticket per session. I quite like the idea of having like one context per session. I think that's quite a nice idea and one sorry one um like by per unit of work. I I just haven't made that work. But that's &gt;&gt; I I found it really useful because then then I can go back to the particular session when the particular thinking was happening.

&gt;&gt; Yes. And I do do that for sometimes when I've got a project that's running over multiple sessions, I can go back to the previous session. Um, instead of uh the VCP command I showed you, I could type VCR and it'll do the same thing. But um in practice though I I like the discipline of it having to pick up again because it mean if it has to pick up again from a fresh context it means that all of the information that was in that session has actually been codified into other places that any claude code session or human could find which means that you end up with a a much more um I guess richer kind of repository of knowledge that you're working in. So, so there's a question mark around if if session if you if sessions are truly not ephemeral and you've got them as a store, are they accessible uh as future context? If you treat them as ephemeral and make sure you capture everything within them into your repository anyway or into documentation files or whatever, I think that could be more powerful. So, worth thinking through for sure.

Any other questions? Feels like we've come a long way from just write this ticket, but there we go. It's good. It's all good. Yeah, &gt;&gt; thank you so much for the talk. I have a questions. It seems like uh in the loop some of the steps might not be necessary like you might go to the code and then find nothing there. Y &gt;&gt; and would you consider to optimize it somehow or you just let the token burn?

&gt;&gt; No, just burn the tokens. They're not that expensive. Depends what you're doing. Um I think we're at the point I should this is a whole another thing. Um we're basically in the era of free tokens right now. Um, you know, I I have a max 20 subscription. Um, I definitely use more than the average person probably who is is paying for one of those. Um, so I I think at this point I would optimize for for freeing your own time up as opposed to optimizing for for burning a few more tokens. I don't think tokens will ever get that expensive. I think that the frontier models potentially will be very expensive, but we have really good uh cheaper or freer alternatives just around the corner. Not quite as good for the latest kind of work that we're trying to do, but they're really really good. So, um you know, I think um uh there was at least one that just came, the GLM one that just came out looks really promising. Um that's the ZAI one. I think it just came out this week. Really, really interesting. I'm still running Claude, but that won't necessarily always be the case. So, I think I I would just burn them. I would, like I said at the very beginning, um you know, I I spent a long time doing the whole optimization thing where I was doing this, if you weren't here at the beginning, this thing, you know, I spent a lot of time trying to to screw around with with all of this, but but ultimately, I just now let it run.

It's much simpler. I do get quite close to the end of my max subscription sometimes, though. I'm slight slightly nervous about what that means. I have to figure out how to get another account.

200.

&gt;&gt; Yeah, max to the $200 a month one. Yeah.

Yeah, I get pretty close to that every week. I'm quite I'm about 80% now.

Getting the jitters. Um yeah, you had a question. Do you want to bring the mic

&gt;&gt; I'm not looking at that anymore.

&gt;&gt; Hello.

Is this uh I wanted to ask you about Thank you so much for the presentation.

Yeah, sure.

&gt;&gt; About fine-tuning for for the prompt, &gt;&gt; do you version it? Do you have data sets that you use to fine-tune your entire loop?

&gt;&gt; Uh, so in terms of um versioning the Ralph loop specifically, like the prompt for the Ralph loop.

&gt;&gt; Yeah.

&gt;&gt; So I use skills for that. So um as I pointed out before um uh everything like that goes into the skill and I get Claude to write the skill for me. Um, and that saves in either your your docu skills folder within your project or it goes into your home directory under doclaude skills. Uh, I use GitHub to version all of those for myself. I don't think git is the right skills format for this long term. I think we need a new thing. Hence trying to build skills in fact um which is this idea of trying to um make skills much more portable and sharable within teams which I'm trying to figure out. So yes, I do um I do version control them and I treat them as quite important code and I don't actually I do share some of them uh but I don't share all of them routinely because they there it's a lot of my own IP in there and actually a lot of my customers IP is in there too.

&gt;&gt; Yeah, the question was more with regards to the performance of the prompts. Mh.

&gt;&gt; So um you were saying that in the beginning you as you go along you improve the prompts as you go along and but are you versioning the that going along and are you versioning the the performance of the prompt overall?

&gt;&gt; So when you say prompt do you mean the um the skill itself that I'm using?

&gt;&gt; Yes. Yes. Yes. Yes.

&gt;&gt; Yes. So yes. So the skill so the prompt lives within the skill. So when I type slashbug tracking or slashalph uh that that is the prompt that that um that gets written by claude um and managed by claude which means that the um uh that whole file is is is the prompt and therefore that is version controlled. So I I always I have a git running within that setup and then every time I change it I update um update. But you but but you remain subjective how you mention you have improved. Let's say that you have your data set will be an issue. I say that and the expected uh output would be the new feature added to the repo.

&gt;&gt; Yeah.

&gt;&gt; So you could &gt;&gt; is it more how do I evaluate whether it's any good or how &gt;&gt; Yeah, exactly. How do you know you're actually improving?

&gt;&gt; I see. So how do you know if you're improving? That's a really good question. Um I do um stress test my skills. So with other skills uh and I say you know is this skill any good?

Could you improve it? Could you write it? Um, I do I I spend quite a lot of my time tinkering with my system and my skills, probably more than I should. Um, I think, um, it's a bit subjective at the moment.

What I haven't done, and this would be a really good exercise, is to try running, um, blind testing where you would run a set of tickets with one skill and a set tickets with another. Ultimately, because Claude is non-deterministic anyway, I think there's a high level of variability with any of those kinds of tests. So, it's it's really difficult to think about how to to construct a useful test in that way to know whether you're actually improving or not. Um, in general, the more context you give um into your prompt, the better it will do up until a point which isn't very easy and obvious to figure out where it becomes worse.

So, um it's about kind of balancing that ultimately. But yeah, I haven't done a kind of objective improvement process. A great question though. It's a question just behind you.

How are you version controlling the skills?

&gt;&gt; I'm using GitHub at the moment. Um I do have a a product that I'm trying to build which is I mean this thing here.

So if you want my skill by the way that's what how you get it. Um it's a project called air skills which you saw a brief preview of earlier from my my uh slide deck that my agent put together for me. But the idea is that um you uh can package and manage those skills as a unit. So you can create skills for your organization, you can create skill bundles. you can um create a skill set for your org that works for different teams within your org. Um and then that all gets versioned and updated for you without everyone having to learn how to use git and github. That's the idea. Um it is a real pain at the moment. I found it really really difficult to manage.

Just for myself even just putting a skill on GitHub. Uh you know I can't imagine anyone from there that's quite a lot of friction for for a coder like me.

It's I can't imagine non-coders using that. So so yeah trying to trying to build this. Um, so yeah, run that command on your machine. You'll have my

&gt;&gt; Um, sorry, there's a question just here first and then go next. Yeah.

&gt;&gt; Um, how do you, you sort of touched on this a little bit, but sort of around the edges. How do you do knowledge management? So, I guess, you know, I use Claude for why is my VPN not working? And then I learn something and I want to record that and then I'm like I've got some a meeting with somebody with a transcription and I have that somewhere else and I've got a bit of code that I'm writing and all all I've got all of these different contexts but they're sort of very disorganized. Do do you have a way of thinking about how you organize all of that?

&gt;&gt; Yeah. So I have a code directory and I have a vault directory and those are the two directories I work in. So the code directory contains a few different projects um that I work in a more classic way. The vault directory is where I do all of my other work and and frankly I I mostly start working in there even if I'm working on code and just tell it where the code is. Uh because the vault contains several thousand files with all of the different stuff that I have picked up learn um worked on with Claude over the last several years. Well, not with Claude for that long, but you know what I mean. Um, I started with uh Obsidian a long time ago and I've been working on that vault for for a long time. Um, and and with Claude now it just works on that for me.

So when I do some research on how to fix my VPN or whatever it is, it just saves a file in there. I have some specific rules for how to kind of structure and manage that. Um, if you're interested more, I haven't written a lot about this, but I know Andre Kapathy's just written about it using LM as a wiki.

That's a great article if you haven't seen it already. Um I know there's a Minjovich actually funnily enough has done a thing on me palace yesterday.

That's another version of this. Um uh you can you can use that too. There's lots of different systems for that that out there. The best way to get started is it's markdown files in a file system and use it like a wiki. So run obsidian in one window and um claude in the other and just kind of work with it and and save things as you go. And so do you have an agent that then structures and puts those fault into folders or something like that?

&gt;&gt; Yes. So it depends on your method. I use the zetlecast approach which is the one where you have one note per thought. So any thought of all just goes into a flat folder. Then I have a slash projects uh thing which has all of the projects that you saw um including one for this presentation. Um which which is my kind of unit of work for an agent that we work on together. It does a lot and then I do some and then it does some. Uh I have um transcripts in there. all of the the calls that I've ever recorded go in there. Um, and I use a tool called Leanne, uh, which is a command line embeddings tool. So, it basically just runs embeddings across the entire all of the text in the repository, all of the transcripts, all of the links I've ever saved, including all of the content.

It's huge. Um, and um, then it can find things usefully and easily in there. Um, so you you the best time to start that is today because it just takes years to put together.

I need to write more about that. Any other questions? Yes, one here.

&gt;&gt; You said you had friction while versioning your skills. I've been using skills only for last month, so I'm not aware of this friction. Can you explain what the friction is?

&gt;&gt; Um, I can I'm sure there are Has anyone here had had any kind of friction with my kind of managing and using skills yet? Has anybody else? Yeah, quite a few different people. So, yeah, it's it's a it's an emerging thing. It's not surprising you haven't experienced it yet if you're not using it for very long. What I found is that if you're just using them on your own, creating a file of skills and managing them is is quite straightforward. Putting them in GitHub is quite straightforward. It's sim links and and GitHub repository.

It's fine. What where it becomes difficult is how you share that. So, how would you share a skill? Okay. Well, if you want to use MPX skills, you have to then put it in its own GitHub repository. That feels quite heavy weight just for one skill. I'd have to have 50 of them in order to share all my skills. So, that doesn't really work.

Then it's more like, okay, if I don't want to do that, I just do I just send them the skill file? Do I send them a zip file? I mean, I can't think of a better way of doing it. Um, do I have to have a subm module in my skills folder for every single Git repository I share a skill with? It just doesn't make any sense. So, I think I think the idea of Claude has got some stuff in there around plug-in marketplaces where you can have a plugin which has a bunch of skills. That's the best way, but then you're versioning the plug-in, not the skills. So, that's probably the most seamless way. it just it just doesn't work that well. Also, there's a challenge around if you if somebody contributes to your skill, do you want their changes or not? It will depend on what the contribution is. Are they local to just them or are they um uh changes that could be generally incorporated and that depends on the skill and depends on them. So, you have to then manage that.

So, do you run a backlog for each skill where you have tickets to improve the skills? Do you see do you see the kind of I think these are all unsolved problems. I'm trying to my contribution is trying to solve some of those. But these these are big problems we haven't figured out yet.

Um other questions?

There's one right at the back. Um if there's a mic that would be amazing.

One, two, one. Okay, it's working. Uh my question is about how we can uh scale up this approach with Ralph loop but like for the actual production team like I I don't know three engineers how to coordinate how to cooperate do you have any idea how we can organize it? Do you have any experience?

&gt;&gt; That's a big question. How so just to make sure I've understood it. How do you scale this up so that you can coordinate whole teams using this kind of looping approach?

&gt;&gt; Is that Yeah.

with all the tickets and the skills.

Yeah.

&gt;&gt; 100%. Yeah, it's difficult. I think the teams that are where I've seen this work well is where they are proactive about updating tickets. The great thing is if you connect your ticketing system to the AI, it's really good at updating it. So, you should definitely do that. Um, make sure that you claim the ticket and move it into the doing column before it starts work. Um, and make sure that somebody else hasn't just done that before you start work. Do you see what I'm saying? That's really important to avoid contention. Those have always been issues with with bigger teams. Um, just in the same way, a couple of controversial things. Just in the same way that Ralph loops work really well by just doing one thing in a loop and quite sequentially. Um, you know, it could well be that the coordination overhead in our teams is caused by the fact we've got too many people in our teams and maybe we should have smaller teams and just more of them, right? So maybe maybe if you're trying to get 10 people to coordinate and using AI and Ralph loops and all of that, that's just not going to work. maybe you need three and maybe that's the way to to kind of run that project and then you split it down and then you have another the other seven people doing something else or or whatever. Does that make sense? So, so making the problem go away is the first step and making sure that you're already using your coordination mechanisms is the second step. Um and then just try it and and figure out what what the bottlenecks are. Be really, you know, be really good at retrospectives uh with this stuff. I think retrospectives and teams are often pretty anemic. It's like what should we do less of? What should we do more of? That's just a recipe for the same, more of the same. Um, and just changing tiny increments, which can be good, but ultimately this requires a radical rethink. So, be really conscious in making sure that your retrospectives are changing actual things about how you actually work or or have space to try.

Let's just try using a RA flip on all of our work for a week and see what happens, you know? And if it doesn't work after two days, that's fine, you know. And then if you are someone here who's in a leadership capacity and is able to sponsor that kind of work, this is what it means to try and move to AI.

If you want to transform your team, you are going to have to sponsor these kind of experiments and be okay with failure because so I was speaking to the leaders in here for a minute because it's going to be messy and it's going to it's going to fail a lot. But if you want real transformation, that's the only way to get it. You've got to give um your team space to try a whole bunch of different things. So give them air cover. Um so yeah it's it that's a big and complicated question. Um I think if you're able to and have the agency to just try it and see where it gets to.

There's a whole um uh separate thing called uh theory of constraints which I haven't talked about at all which is the idea that within any team in any system there is always a bottleneck. There's always one bottleneck that's the big bottleneck. If you don't work on that one bottleneck, all of the other work that you might do to optimize and improve the system is pointless and actually probably counterproductive. So this is why some teams when using AI tools and using advanced AI tools like Ralph Loops, which is just, you know, AI or you know what we're doing now, but just on steroids, some teams when they implement it actually go slower. Some teams go amazingly fast, some teams go slow. Why is that? it's because they're not working on the constraint. The constraint in those teams might be the review process. If you or the release process, if you release your code once a month, um, and you're shipping 200 PRs, not 20 in that release, how do you think that's going to go, right? You know, it's not going to go well. So, that's why teams go slower because what they need to do is fix their release process, not their coding speed. Um, so always fix the thing that is the biggest bottleneck first. Then figure out where the bottleneck moves in the system and that's not predictable. It's random. So you have to figure that out. Then move and fix the next thing in the system.

For more on that, read the gold by Elio Goldrat from 1984, no less. It's an amazing It's amazing book. Um, one more.

Is there another question down here? Is there another mic? Where's the mic?

&gt;&gt; I have a mic.

&gt;&gt; Oh, you've got a mic. Great. Keep going.

Um since you were asking about like talking about the constraints part this reminded me like I'm part of the AI team and we have an NI team and they write a lot of microservices and it's in different repositories.

&gt;&gt; Okay.

&gt;&gt; How do you deal with uh like coding now since is it like one big monor repo or is it like small small repos?

&gt;&gt; Um you you have to try it different ways and see. Um, I don't think that the the GitHub or sorry, Git architecture, whether it's many repos or one repo really matters. You can always start your AI in um a folder above all of your other repos and just get it to work. It does a great job of that. So, that's okay. I think the bigger question is what are the coordination patterns within your teams and your services?

Who's responsible for what? And how does that change with with AI? I think that's a more interesting challenge. The main reason I'm asking this was because like some of the microservices depend on others &gt;&gt; and then you have to release one of them you need to release a tag and then updating another and that's just &gt;&gt; yeah I think what AI will do is it will expose all of the places in which that process is inefficient because it will do everything faster which means that you if you're seeing those bottlenecks where you are getting dependencies between your microservatives guess what that's your biggest bottleneck um therefore you fix it. So how do you fix that bottleneck? Well, you might try atomic release system or you might build something that using claude that using a ra loop that um figures out a way of um uh coordinating releases across multiple repos more successfully. I don't know.

But that's what you do. That's where you work. Don't work on anything else until that's fixed. If that's the bottleneck.

&gt;&gt; Yeah, there's a question here. Do you want to pass the mic?

&gt;&gt; Yes.

&gt;&gt; I should say I'm kind of at the end of the content. There's if you which probably was was clear half an hour ago.

Um, the only other thing I I mean I had a Q&amp;A slide. If you are leaving, you're welcome to leave, but you're welcome to save for more questions. I would really appreciate some feedback though. So, this this QR code is the only thing that I manually added to these slides. Um, if you could just um fill that in, that would be lovely and amazing. Thank you.

It's literally only three minutes, four questions. Um, it just helps me to improve and make sure that I do a good job of these workshops going forward.

Um, that's also my LinkedIn. I post a lot of content on there. Do um do connect with me. you do um in case you mean some of this stuff, disagree with me. I love disagreement. I love it when people say, "Surely Chris, that's nuts.

You shouldn't be doing that." Love those kind of comments. Uh because it really helps me to think and improve, which is what I love to do and because all because the Ralph Loops is doing all my other work. So, got no got nothing else to do.

Uh great. Thank you. Um so, I just wanted to put that up there. Very happy to continue answering questions though.

Um but if people wanted to drift away, then that might be a good time. Go for it. Um, my question is regarding multi- aent orchestration tools. My I'm curious if you've tried things like CVG's Gasttown or yes, &gt;&gt; there's another guy who does like MCP agent mail.

&gt;&gt; Yeah, there's some really cool and interesting stuff. I still think we're in the wild west literally with Gasttown and things like that, but we we don't really know how how that's going to go.

I have tried Gasttown. I couldn't really get it to work, but it was pretty early on. Uh for me I feel like the the agent orchestration side of things is I I think that we over complicate things by assuming that they need to be in parallel. I quite like the idea of just starting with a loop to start with. I don't feel the need to um have my AI um do lots of things at once before I can get just get it to to to do one thing.

Well, it kind of goes back to the theory of constraints thing again. Um, I don't think speed of, you know, number of tokens per second is the bottleneck. I think that it's our ability to specify what we want and review what the AI has done. So, so if that's my bottleneck, I don't want to introduce more agents. So, I haven't spent lots of time with those tools kind of for that reason. I feel like they're solving a problem that not many people have yet.

&gt;&gt; Yeah. So, so &gt;&gt; hello. Yeah.

&gt;&gt; Yeah. Not just for speed, but for example, I don't know if you've experimented with MCP agent mail. So agents can uh lock files and speak to each other so they don't step on each other's toes and you can use different like cloud opus and codecs work on the same project so you get different brains working on the same project.

&gt;&gt; Nice. Yeah. No, I haven't I think I've heard of it but I haven't tried it.

Sounds like a super interesting idea rather like someone mentioned earlier about sub aents trying to look at things from a different perspective. I've had a lot of value for with doing that. I mentioned earlier my simulate audience um approach which takes um ultimately the way the way that it works by the way is it takes like um uh transcripts and also survey responses on my website and it it creates personas and has those personas think differently in parallel sub aents to take fresh looks at content from different perspectives. Um so that whole idea of having it um having two different things and two different models as well in that instance is a super interesting one. I think we'll see a lot more of that. I can see a lot of value in it. We definitely know that a they're pretty good at agreeing with themselves and you often get better a better contrarian take if you throw away the context and look at it again. So great principle. I think the the tooling is still super early which we all know but they're interesting ideas for sure.

&gt;&gt; Yeah, just keep going. They all turn on.

&gt;&gt; So great talk by the way. Thank you. Um how much importance do you put on? So in terms of phases of how you develop you're spending a lot of time building out the context, creating the tickets and you have a system to run them in sequence gets pushed up. How much do you focus or emphasize on CI/CD running automated tests linting? Um does that give you the confidence to reduce the amount of code you're reviewing?

&gt;&gt; Absolutely. Uh well yes and no. It depends what the code is. I think firstly I think CI/CD good testing is absolutely essential. Linting and all of those things. If you want an AI to do a good job for you, why wouldn't you give it those tools to to help it do a good job for you, right? Just the same way that humans do much better when they have linting and CI/CD and good tests.

It's exactly the same. It's the same with clean code bases. You know, um you know, it's worth doing all of that work to make an AI do well. So, so that does give me more confidence in what I'm doing. The challenge is if the AI writes the tests then and also writes the code then there's a good chance that it's got something wrong about what you're trying to build. So often it doesn't make it doesn't make kind of obvious mistakes.

The things that it gets wrong is it just completely misunderstands a feature, builds and says, "Yep, that's fine." And then ships it and then I'm like, "Oh my goodness, I don't I don't quite let it ship all the things." Only only with pre-release projects do I do that. But um but yes um it does give me confidence in in knowing that the thing uh is is I guess uh functionally acceptable for release or releasable. What I I still want to read the diffs because I still don't trust an AI with security. Um so um I don't know if I've lost uh lost the screen. There we go. Thank you. Um I think my computer went to sleep. I I I just don't quite trust the AI not to lose my customers data and I just won't compromise on that. So I will read the disc because I don't want to be responsible for that. Um it doesn't feel uh it doesn't feel like it feels like there are some problems for which you can trust AI fully like for example um linting uh testing. There are some problems which you can't really trust AI just because it's not responsible to do so. So maybe specific changes around security. If you're running production database migrations, you should probably check that they worked before running them in production. Um, and there are some that are a bit more fuzzy and hazy.

So, uh, UI testing is quite interesting early. You know, the idea of having a a great feedback mechanism. If you're able to get an AI to click through your um project to check that it works, that's really powerful. It works 50% of the time in my experience, but it can be quite useful at least to have a first go at it. Uh having good endto-end tests is actually really helpful if you're using playright or something like that which it maintains for um for the skills project I showed you. Uh I have some very comprehensive end toend tests that set up full file systems of skills and get the AI to create two different um personas with running each you know a publisher and a creator and it just checks all the files are in the right places and that's really really useful um for those kind of full end to end tests. Um equally um uh if you're able to to build those kind of feedback mechanisms and give the AI a chance to um to really know whether it's done well that that's a brilliant place to be. And so I'm always looking to figure out how if an AI could tell whether something was good or not rather than me. And when I'm able to take myself out of that loop, it just massively improves the whole process. It's not always possible or desirable, but as much as I can, I do.

You are designing the feedback uh process though you're you're &gt;&gt; you're deciding on the criteria and then letting AI execute on top of it.

&gt;&gt; Yes. Um if if I'm working in a team of one, yes. If I am working with a product owner or product manager u or a designer, I'm really interested in in utilizing their skills and testers as well to to figure out ways to design those pro. This is what they this is the value that they bring to these processes, right? is how just in the same way that coders are thinking how could we avoid doing the typing ourselves now uh what about um if you're a product manager how do you um get the AI to do the easy stuff so that you don't have to do it you know how do you go through that process um same with testers uh super interesting area of research &gt;&gt; two things that come to mind on this have you what works well for in a small team what works what worked well for me is setting up adversarial reviews where you have spec The dev agent goes develops a reviewer that does an adversarial review. You pass that context back. The dev agent iterates generally catches a lot of things increases the amount of confidence I have to ship it. But even with that, the with the rate at which you can create specs and how much code you actually have to review, I end up being the bottleneck in the review process still.

&gt;&gt; Yeah. Have you I'm always I'm always a bottleneck. I have 30 different things that I need to now review that my AI has done overnight or something and I'm just like oh my gosh, you know, and and the challenge for me is that a lot of that is not work that I should be doing. The only reason that it's given it to me is because I can't trust the AI with it, but any human could do that kind of work. So I'm now like, do I hire humans to just do the boring work? Is that ethical? You know, this is kind of an interest really interesting questions um for us to think through, but you're right. If we're able to design system, I love your adversarial point to to builds on something else somebody else was saying. Um if we're able to do that and design these systems such that we don't have to be in the loop, I think that is better for all of us because I don't just want to give a human a terrible job.

&gt;&gt; Till then, we're employed. So, &gt;&gt; yeah, I guess.

&gt;&gt; Thank you.

Should we call it there? Folks, it's been a pleasure hanging out with you.
