# Building Pi, and what makes self-modifying software so fascinating

| | |
|---|---
| Creator | The Pragmatic Engineer
| Published | 2026-04-29
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=n5f51gtuGHE](https://www.youtube.com/watch?v=n5f51gtuGHE)
| Generated | 2026-05-04

## Description

Mario Zechner is the creator of Pi, a minimalist, self-modifying AI coding agent, that is the foundation upon which OpenClaw (created by Peter Steinberger) is built. Meanwhile, Armin Ronacher is the creator of Flask, and a longtime user of Pi. The pair are also friends.
I sat down with Mario and Armin for the latest episode of the Pragmatic Engineer Podcast for an interesting conversation about AI and their reservations about it – even though both are heavily invested in building AI-powered tools.
Mario explains why he built Pi, and gives his take on why it has become so popular. Armin walks us through how he uses AI tools, including building a game with Pi, and why he always puts human judgment firmly at the heart of his approach.
We cover the risks of over-automation, the limits of agentic workflows, and why strong engineers with informed judgment still matter. We also get into the challenges of working with code written by non-engineers, and whether open source can withstand a tidal wave of agent-generated code.
—
*Brought to you by our season partners:*
• Statsig — ⁠ The unified platform for flags, analytics, experiments, and more. http://statsig.com/pragmatic
• Sonar – The makers of SonarQube, the industry standard for automated code review. https://www.sonarsource.com/pragmatic/?utm_medium=paid&utm_source=pragmaticengineer&ut[…]egory=Paid&s_source=Paid%20Other&s_origin=pragmaticengineer
• WorkOS – Everything you need to make your app enterprise ready. https://workos.com/
—
*The Pragmatic Engineer deepdives relevant for this episode:*
• The impact of AI on software engineers in 2026: key trends https://newsletter.pragmaticengineer.com/p/the-impact-of-ai-on-software-engineers-2026
• Cycles of disruption in the tech industry https://newsletter.pragmaticengineer.com/p/cycles-of-disruption-in-the-tech
• The AI engineering stack https://newsletter.pragmaticengineer.com/p/the-ai-engineering-stack
• The creator of OpenClaw: "I ship code that I don't read" https://newsletter.pragmaticengineer.com/p/the-creator-of-clawd-i-ship-code
• What is inference engineering? Deepdive https://newsletter.pragmaticengineer.com/p/what-is-inference-engineering
—
*Where to find Mario Zechner:*
• X: https://x.com/badlogicgames
• LinkedIn: https://www.linkedin.com/in/mariozechner
• Website: https://mariozechner.at
*Where to find Armin Ronacher:*
• X: https://x.com/mitsuhiko
• LinkedIn: https://www.linkedin.com/in/arminronacher
• Website: https://mitsuhiko.at
• Blog: https://lucumr.pocoo.org
—
*In this episode, we cover:*
(00:00) Intro
(07:30) How Mario, Armin, and Peter Steinberger met
(15:15) How 30 dev teams use AI agents: learnings
(21:50) The importance of judgment
(24:26) Challenges when non-engineers write code
(28:30) Downsides of over-automation
(32:18) Pi
(48:09) OpenClaw + Pi
(50:54) “Clankers”
(57:32) Open source and AI
(1:00:22) Complexity as the enemy
(1:02:50) Building an AI-native startup
(1:11:52) “Slow the F down”
(1:16:40) MCPs vs. CLI
(1:25:03) Predictions and staying up to date
—
See the transcript and other references from the episode at https://newsletter.pragmaticengineer.com/podcast
—
Production and marketing by https://penname.co/.

> Transcript extracted from YouTube auto-generated captions (`n5f51gtuGHE.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Can we start with the backstory of why you decided to build Pi? I personally like simple tools that are stable that I can rely on even if they have non-deterministic parts. So you can ask Pi to modify itself. Pi doesn't have MCP. People just ask Pi to build MCP support into PI.

&gt;&gt; Non-engineers participating in engineering process is a thing now.

&gt;&gt; You might have a PM who wants to try out a feature without wasting time of an engineer. Now you can do that. The problem is that people are now so focused on everybody can do everything now that they forget that you still need a process to guard rail all of that.

&gt;&gt; But you just recently wrote we all need to slow the f down.

&gt;&gt; All the companies claiming that all of their code is now written by agents.

Yes, we know the quality is garbage. We feel it in our bones when we use your product. It's garbage. Basically, I

What if I told you that one of the most influential AI coding agents of 2026 was built by a single developer in Austria who got frustrated with existing AI coding agents. This is Pi, a minimalist self-modifiable coding agent which has quietly become the engine behind the wildly popular personal AI assistant OpenClaw. Mario Zner is the creator of Pi and joining him today is Armen Roner, the creator of Flask and now an early adopter and contributor to Pi. In today's episode, we cover the backstory of Pi and why self-modifying software is much easier to do with AI agents. What Armen learned interviewing 30 plus engineering teams about how AI agents are changing how they work and why software quality feels like it's trending down, the case against MCP and why CLI are becoming so popular and many more. If you want to hear from two very grounded voices in the industry honestly talk about what's working and what isn't and why we need to slow down as an industry, this episode is for you. This episode is presented by Statsig, the unified platform for flags, analytics, experiments, and more. This episode is brought to you by Work OS. Engineers love to build. Today's episode will be a great example of this. We'll get into why and how PI was built from the ground up. But when you're shipping a product, some problems are better solved with trusted infrastructure built for scale.

Enterprise features like SAML, directory sync, and audit logs are some of those.

Work gives you APIs to add them in days, not in months. Ship faster without reinventing the wheel. And now, let's get into the episode.

&gt;&gt; Mario and Armen, it's so good to have you here on the podcast.

&gt;&gt; Thanks for having us.

&gt;&gt; Thank you.

&gt;&gt; So, as a kickoff, Mario, how did you get into tech and eventually into building AI stuff?

&gt;&gt; Oh, well, that's a long story. How much time do we have?

&gt;&gt; So, I'm a kid of the '9s, actually.

and uh got my first PC at 909 96. And the trigger for that was that I loved computer games. We were kind of working poor, so we couldn't afford any of the Game Boy and NES, Super NES stuff. But I had an uncle who had an Amigga 500 and I would go to his place every second day and just play games there. And eventually my my parents told me if you work uh you can save up and buy yourself a computer. And in reality, my dad would do um what's he called? Schwartz of it.

&gt;&gt; Well, you're not necessarily paying the taxes on your &gt;&gt; Yeah. So, he would do his normal he would do his normal job and after his normal job, they would go fix cars and work at construction sites and &gt;&gt; Yeah. It's very common in Europe. Like I know everyone did that.

&gt;&gt; And after two or three years or so, they they just said, "It's time." And took me to a computer shop in the nearby big city and bought me a 486. And that's how it started basically.

&gt;&gt; Pentium 486.

&gt;&gt; Yeah. an Intel 486DX40 MHz with turbo button and that's where I started and I've always been into games a lot um which also led to graphics programming and through sheer luck I got a job while I was studying at university at um applied science organization who was doing NLP stuff um machine learning applied machine learning basically taking research results and trying to stuff them into industry applications And that's where I learned the ropes of machine learning. That was all before deep learning became a thing. And I actually quit that kind of domain in 2010 111ish because I joined a startup in San Francisco.

And then later came back and joined another startup with two friends in in in Sweden where we did uh an ahead of time compiler for jaw bite code to iOS that got sold.

And since then I have a little bit more time and I've always kept up with machine learning stuff because obviously super interesting. Uh and yeah and then GPT happened and that's the story.

&gt;&gt; Yeah. And here we are. And then Armen where were your roots?

&gt;&gt; So my roots are definitely not working poor but I because my parents ran an architectural office where they kind of adopted computers for cat drawing. My first computer was like old computers that they recycled. So, my first computer, even though I'm younger, was in three. I'm &gt;&gt; so sorry for you.

&gt;&gt; And and so, basically, none of the computers that I ever had were capable of playing computer games properly. Um, because one, they used Windows NT, which at the time didn't do anything. So, you had to sort of like build your way through it. And like the only way you could actually get them to run was because before it didn't know yet how to get the Windows 95 or like Windows 311.

Um that was like before it booted into either one of those you could boot it into DOSs like really old DOSS games at a time when you could already get better stuff but but because it was sort of this kind of thing I started toying around with quick basic a lot um with to Pascal I bought a bunch of books on that um and I that that was my roots of of of learning how these things work and it just I wasn't ever really good at this but I found it really interesting like this this idea of like No, for sure.

Like I like I &gt;&gt; We call it a tabler in Germans.

&gt;&gt; No, I I swear to you like I was when I when I started dabbling with this, I just really sucked. But like over time, you like if you keep doing this, you get better. Um and then in um 2002 or three, I I used I used to use Deli a lot, which was like a visual version of of Turbo Pascal.

&gt;&gt; Yeah. And in 2002 or 2003 someone uh also showed me because I I I' I've got this idea like I want to use Linux and then um I del I didn't work on Linux and then I found Python and through that I started doing some Pyth programming and there was a YUbuntu just came out in 204 and that was a venturebacked vehicle but it they created all this like local community. So there was this like Ubuntu association. I together with a bunch of friends we started the German Ubuntu foundation uh not that foundation association um and we ran this online community called YUbuntu users for four or five years and we and because Yubuntu was popular the community grew and then scaling problems came so like that's how I got into web development um and then for building this I just I wanted to build a templating engine a web library all of this and then eventually I bundled that together and made this flask frame work which got very popular and even nowadays still is a thing that clankers like to spit out.

&gt;&gt; That's hilarious.

&gt;&gt; Um h but I I left it and then in uh in 2013 14 or so I worked on computer games for a couple of years in London but then afterwards I went back to open source and I I worked on Century for 10 years and then left in April last year to try something new.

&gt;&gt; So both of you are originally from Austria. In fact you right now live in Austria as well right? you were doing games, you were working at Sentry, you also did games before. Then the third person who's not in the room but was on this podcast just before is Peter Stainberger also from Austria. Great that the two of you meet where the three of you meet cuz uh I I've I've recently seen a bunch of photos especially before Open Claw and Pi started you hanging out uh the three of you experimenting playing with AI.

&gt;&gt; I think the two of us met on on the internet right on Reddit.

It depends because I definitely met you once when I was at university.

&gt;&gt; All right.

&gt;&gt; So, but you didn't recognize me that time and I was useless.

&gt;&gt; I was already famous.

&gt;&gt; Um, but yeah, we sort of abstractly met on the internet, &gt;&gt; but eventually we met up in Vienna. Um, we were screaming a lot at each other, but uh on the internet, but uh in a in a very cute kind of way, in a very non-confrontational kind of way. And even though we might not think alike in all areas of of of our lives, uh it was a cultured exchange, I would say. So that was nice. Uh and Peter I like 60° of Peter Steinberger basically. Um I was working at an office in my town and the company that gave me free office space in exchange for being like a mentor to the CEO had some kind of business dealings with Peter's company, PSPDF Kid.

&gt;&gt; PSPDF Kid. Yeah. um and eventually came to the office in Gratz and I think that's where we met the first time and then also the same year we met at the conference in Istanbul and just hung out for an entire night and that's basically where it all started. Nice. And then how did the both of you go from being skeptical about AI when these tools came out and again both of you have to at that point and by 2022 you've been doing a decade plus of building complex software in different domains. what was your first reaction to it and then eventually how did you kind kind of come across to the side of like well this thing is actually really interesting &gt;&gt; so for me it was I think in 2022 I think co-pilot um GitHub copilot came out before TPT &gt;&gt; yes in 2021 &gt;&gt; yeah and through my previous startup stuff I was working with Ned Friedman and Miguel Daza from Samarine because they acquired the company &gt;&gt; with Samarine yeah they acquired the company I talked about earlier the Java compiler thing I I knew Ned Freriedman from our early startup stuff and eventually moved to GitHub and then was in my DMs in 2022 I think and asked if I wanted to have access to GitHub copilot the tap tap tap autocomplete thingy and I was like I I don't really care I don't think this is going anywhere and he's like no man it's the future got to try it it's the future so I tried it and it was absolutely horrible but yeah after after when GPT came out and especially when when they started providing API access I did a lot of projects just figuring out what works and what doesn't work not necessarily in the coding space but eventually once they had tool calling that's when they became very interesting or function calling as openi called it back then um but it took until 200 I would say 24 end of 24 October or so for that to actually be useful and that's where the coding agents also became kind of interesting and then 2025 um the cloud code team came out with with cloud code and that introduced the chanting search. So basically just give the agent a way to plow through your file system and read all your files and it made the whole difference actually &gt;&gt; like all the things that came before like cursor with indexing and and any based stuff and and all of that that just went away and I know that the CEO of uh Chroma is probably mad at me for saying this but that that was the difference that it didn't it it wasn't like a dense and sparse search thing that the agent could could go through it was just give it access to your files.

That that was it for me. That's where it clicked for me.

&gt;&gt; I think my path was kind of similar. Um because I think Copilot came out quite a bit earlier, but I know that um there was a program at GitHub that gave you early access to Copilot at the time. Um I think it was like this maintainers group or something where I still was in I got the feeling for Copilot that this will actually be really interesting.

um but not in any way in which it is now because I felt like oh I am in open source for such a long time and now they're doing like training in open source data. It's like there is something at the very least this will be controversial. I I didn't think about like it being productive. I felt like oh this is going to be um is going to be like a controversial thing with like training open source data and and and I was I remember for like a time was like I was trying to probe it like really um &gt;&gt; whether there's flask in there. Then I I was trying to probe it like really adversarial. So one of the things that I I probed on is like I probed on like will it retail GPL code and I remember at one point I got it to um spit out the uh &gt;&gt; carax inverse &gt;&gt; carax inverse square root function which is was very easy because also it was had a very specific name. So like was very easy to get the recall but I also found like you can you can sort of tab in a certain way then it would then continue putting like license text on top of it.

It was completely wrong. it like came from an open source GPL drop of of of Doom originally I think. Um and so it was like it would have been GBL code if it would have done that but it actually attributed like MIT license from a random dude and I was like oh like Mr.

Cop that's the wrong thing and that tweeted at the time got really really popular and then sort of people started like sharing with me like because I was at a time not really exposed to how much actual AI progress was being made in those labs. Yeah, &gt;&gt; like I I didn't come from this AI space or ML space. So like I was I learned about a university and like oh there's AI winter and then nothing happens. But through this tweet and some other things I like other than I like I re recognized that there was something there like there's there's actually CEOs in certain companies are convinced this will get off and that's how I started like paying attention to it and I was essentially I was trying all kinds of stuff with the API like can you do like bug fixing things I got really interested in it but it didn't at all feel like the world is going to change until um cloud code &gt;&gt; and you also changed your stance on the whole oh my god this spitting out open source code. It it memorized.

&gt;&gt; So, because like my like my shtick for many years now has been that I really I'm like a I want people to share stuff like I I think like human progress comes from like building on top of each other and I I'm a huge supporter of the fact that in the US you basically take knowledge from one company to another company that then no competes like I I like this pirate kind of approach to sharing.

&gt;&gt; Yeah. spread of knowledge.

&gt;&gt; Yeah. And so like I I was like my optimal version is like copyrights don't exist in a way or like very very like limited kind of version of this. I was like I really didn't care that it spits out GPL code and doesn't attribute like I was like oh maybe this will just completely destroy copyrights and like for me that was like oh this is I like if if that's the outcome of like I'm I'm fine with it. So it was but it was it was an interesting kind of thing in the beginning that it sort of like it sort of creates this license violation like I want to see like what what chaos will emerge from it and so far I think mostly what has emerged from it is like a strong belief now that like the the system in place for copyrights has some presumptions assumptions in the US about how it's supposed to work and we're all kind of like ignoring that right now because we want to create the mess first and then reeregulate probably because like at least in theory a lot of the things that we're producing right now are probably by historic readings of the copyright interpretation actually not copyrightable.

&gt;&gt; Yeah, that that's an interesting one.

But speaking of jumping jumping to today so an interesting thing that you did recently, we talked about it just before is as part of your new startup is building things on top of agents and you talked to about 30 different engineering teams saying hey how are you using agents inside of your company inside of your team? What did you learn from large companies to startups?

&gt;&gt; I think the the a bunch of learnings entirely unsurprising is that whenever people had vacation, there was more time spent on um trying these tools &gt;&gt; and and just to be clear like you talk with like folks at the likes of like Meta Startups.

&gt;&gt; Yeah.

&gt;&gt; Like like a bunch bunch of different people, right? So a bunch of different people from like different like European dinosaurs like &gt;&gt; are you pointing at me?

&gt;&gt; Well I mean like the European dinosaur would be someone like cementss.

&gt;&gt; Yeah.

&gt;&gt; Or I also talked to to companies which are sort of in a critical space. And what I mean like when adoption happens when people have vacation is that like when when your CEO or your tech lead comes and says like you got to use cursor now you got to use cloud code now is actually you don't get it in a way because you you need to actually spend some time on like there's a there's a it's like a two to three week kind of thing until it really clicks on you and so I I always felt like with the people that I knew like I had a lot of free time like I I left the company in April until October. I was like, I can dive into this and I I like this is like how does nobody get this and &gt;&gt; a catnip for all of &gt;&gt; it was it was crazy catnip. I didn't sleep much all of this. But but what happened within the company seemingly is that when there was like Thanksgiving, there was um for the Europeans a lot of it was over summer and then at Christmas a lot of people sort of and they also get free credits during those times and so like more and more people get &gt;&gt; Oh, you mean the the companies often give you generous &gt;&gt; so and so more and more people went into this and and especially after Christmas I would guess like in more than half the companies I talked to after Christmas it really exploded um and and it it exploded in And so in all the ways we would expect it where like all of the sudden the quality drops and and and and it doesn't necessarily drop because like people want to make worse code but because it actually takes some effort to to stay within this and we we have seen this in the startup ecosystem already in the summer last year like if you if you pay attention to like the the YC startups a lot of them some of them have their stuff on GitHub or for some period of time on gith when you can look at it and like at the time because of like plan MD files checked in and like everything attributed to CLA. So like that vibe coding kind of thing was was for like prototypes and whatever like that built that out. that was already out there to see. But then gradually a small version of this has like been code bases with a little bit of vibe slop on top. And an interesting sort of part of this was like how engineering teams and companies are now responding to that um with all kinds of like different findings. But but a lot of it has been challenge to review PRs. They're getting larger and larger and they're becoming like more psychological &gt;&gt; and engineers specifically are having a hard time keeping up with the the longer PRs that they're more frequent.

&gt;&gt; Yeah. And there also there a lot of the code in those PRs is how an engineer wouldn't do it because as an engineer you sort of get a really bad feeling committing certain code because you think of your future self and the agent really does not care. This is I I will retell this story over and over, but like I I worked uh for an Xbox One game at the time um right around the Xbox One launch. So that was like a fixed date.

It has to release on that day. So I worked on um uh the Halo Master Chief Collection and there was a game where you had like a matchmaking component and you had to like start this thing and whatever. And it was like it was an all hands-on deck kind of situation where people had to go in and unslop the humanmade slop that was the matchmaker.

And it was like it was it was a system with like way too many states. We call it an emergence state machine because it was like 16 bulls on one massive thing and like in theory there were only six valid states but in reality it was a geometric explosion of possible states.

And that's how a centi code feels like where it really should only be like a very clearly defined system but in all reality they're like oh we can config doesn't load let's catch it down and load the default config. So instead of actually failing, it now recovers. But now your code is way more complex than it should be because instead of failing properly, it is now recovering and entering these many more failure states.

And that makes it much harder to work with this code because you can also not really ask the agent to refactor applications like oh yeah, this could be possible. So we need to maintain this variant. I think it's kind of even worse than what you described about your humanmade complex system because there are moments of brilliance in agents where they spit out perfectly fine simple code exactly the amount and type of code you didn't need for that specific thing and you as the steering engineer looking at that like wow this is amazing I can just sit back and not care because it's obviously doing the thing like two minutes later you have another agent running in this window and it spits out the worst horrible garbage suppose but you might not notice because now you have fallen into automation bias and think your your your agent is doing the job well. Do you think this might be our bit of a human bias because because you know like typically like onboarding a new engineer uh you have when you join a new grad you review their code and if it's terrible code you will review the next one thoroughly until they get to the point that oh it writes the code that I do and then it typically takes you know 6 months or a year or something like that but then you know I can trust this person. Yes, but you don't have anything like that with agents. Like agents don't learn. You can put as much stuff in the agents and do you build a memory system, but that's not the same type of learning than u a human does.

Obviously, humans are failable as well.

No, no, no matter. But they have some capability of learning &gt;&gt; and retaining that learning, right?

&gt;&gt; Yes. And they also feel pain. And I think that's one of the defining things about humans. It's kind of ties back to what you said. Eventually, if the pain gets too big, you as a human are incent incentivized to fix the cause of your pain. And in a codebase, the cause is usually terrible interfaces, terrible complexity that you want to get rid of because you can no longer maintain that system. Isn't this why just holding on to that, you know, like senior engineers are always in demand because from the CEO sees a senior engineer as like they just get it done, but in reality, a senior engineer, most senior engineers who are effective, they've had battle scars. They've been burned. They felt the pain. You they saw what happened when they left tech def spiral. So they now make all these decisions that they know they they will help avoid. And of course through this uh progress goes faster. I personally think and your mileage may vary. But a a good engineer is an engineer that says no a lot and I don't need this a lot.

&gt;&gt; Mhm.

&gt;&gt; Because that keeps complexity down. If you're using agents, the exact opposite happens. You say yes, I want this and that. I want this and I want this and I want this because I don't have to type it myself. I don't have to think about it. I just give the little machine a prompt and it will spit out something that kind of looks like the thing I wanted. Good enough. And that's where all the problems start.

&gt;&gt; And one thing that I also think is like good engineering is all about knowing the trade-offs that you have to make.

And there's sometimes the right solution is actually if you were to sort of like sit at university and learn about it, you kind of learn that you shouldn't be doing this in a way. I think Kell Henderson had this once where he said like you you do the dumbest solution first until it doesn't work anymore because the the actual problem is there's so much stuff that you need to do that if you actually do the right solution the correct solutions all of this it is it you're creating the kind of complexity that kills you at scale and the engineer learns that but also like if you if if you don't have that battle scar it's actually very hard for you to argue correctly because it is it is this learning process that gives you the authority to then convince other engineers in the engineering org that you should be doing it this way. That is part of it. You learn that. But the other thing is also that the agents give you now world knowledge access. And one of the other things that I learned through interviewing engineering teams now is that the senior person says no knowing something and then 48 hours later the junior comes by and said like I talked to the agent and I already had this inkling but now I have all the evidence of why we shouldn't be doing it this way because like previously you really didn't have that readymade access to &gt;&gt; someone who can tell you a senior off.

&gt;&gt; Yeah. Ex and and this this is creates other stresses now that were previously like not every team has that because it's like people going to the doctor with a jetp print out and saying this is what the machine said you better do that. Is it fair to say that we are based on what you're seeing and talking we might face a thing where it's very hard for experienced engineers to it's harder just for them to say no uh in spite of the product manager or a junior engineer saying &gt;&gt; it's much worse because the product management comes in sends pull requests and autom should them &gt;&gt; yeah that's another thing like non-engineers participating in engineering processes is is a thing now &gt;&gt; ask how that works Ask him how does it work?

&gt;&gt; How how does it work Armen?

&gt;&gt; Well, it's hard because if because on the one hand like it's well intended, right? If someone who's like &gt;&gt; what what is your experience? Is this your your company talking with other other people?

&gt;&gt; So, first of all, like like we have a little bit of this air like we're small and so um like like my co-ounder for instance sometimes has like a pork on the website. I talked to people that have that at scale where like the marketing team all of a sudden does stuff on a website and and the sales team like creates ever more elaborate like sales demos that sort of land up on a GitHub or partially at this one one of the most funniest one was like where the sales demo built a feature that didn't exist but nobody noticed right so this this is all like this is new right because like previously none of that happened.

&gt;&gt; But I think it's empowering like if your entire &gt;&gt; empowering it's like there's a good thing to it in too &gt;&gt; if your entire or if everybody in your or can participate in in in in the creation of software in some form right previously people couldn't do that like you had a designer who could figure something out in Figma but they might not be able to kind of put it into a clickable dummy demo whatever or you might have a PM who who wants to try out a feature without kind of wasting time of an engineer. Now you can do that. The problem is that people are now so focused on everybody can do everything now that they forget that you still need a process to kind of guard rail off all of that.

&gt;&gt; And the integration part is the hard thing. It's like that Peter gave this idea of like the prompt request, but I'm actually really warming up to this idea like once you've demonstrated it, &gt;&gt; I no longer need your code. And and just just to recap, the prompt request was him saying that he doesn't like to get pull requests. and said he would rather see the prompt because he will run the prompt or he will tweak it and it will generate it in the style that &gt;&gt; for me it's less about like I want to see the prompt as it like what is it supposed to be doing and now that we understand because like actually in many ways I think like the interesting part is like often you don't really fully know what you wanted to do in the first place and so like the act of creating clarifies what you really want to do and so like that part is highly valuable often the approach and the code that comes out of it is not what an engineer there with sufficient seniority would have done. So it's not like I want your prompt so that I can reclank my clanker so that it does it slightly better, but more like now that we know what we wanted to build, it's probably faster for me to start.

&gt;&gt; Yeah. And I also kind of disagree with Peter on I just need your prompt. I actually value seeing a terrible implementation of something. Um like if I get a pull request and most of the pull requests we get on the Pi repository are made by agents without a lot of human touch. let's say then I immediately know okay this is going to be garbage but it's valuable garbage um because someone has put in at least a minimum amount of thought instructing their agent to create this pull request and I get to see how a shitty implementation of what they wanted to build looks like and I get to I I don't need to waste my own time on trying that out. So somebody else tried it out already that the naive dumb agent do the thing do no mistakes uh version and that saves me time. I'm not saying I like pull requests by agents because they they're terrible and they autoc close them now, &gt;&gt; but they have value. It's it's not just a prompt. It's uh on an exponential rate sigmoid eventually always because ser dynamics, but uh I think we're going to find out way earlier than in previous cycles that this is a bad idea.

&gt;&gt; That's good news. What I think is going to be interesting and I don't know the answer to this but uh I read this fascinating retelling of the British industrial revolution and how it it changed the textile industry.

&gt;&gt; The industrial reind industrial. Yeah.

&gt;&gt; Yeah. And so the the the the the general thesis on that article was like every time something at the head of the pipeline got optimized. It created an incentive downstream of the whole thing to create something right. is like in the beginning like if you can weave the thing faster then eventually you need to have Garn that can be weaved at faster speeds then eventually you need to everything sort of turned a bottleneck all the way down and like ultimately the biggest bottleneck in the entire thing turned out to be what I think like is actually the the next bottleneck we're hitting in engineering which is like at one point you made a shirt and if you didn't like the shirt you went back to the person that made it and they fix it up for you and so the actual thing was like if if the shirt is bad nobody cares about anywhere who destroyed fur in the process. Is it just going to get a new one? Right? Like the the responsibility actually went from anyone in this chain to the entire factory as a whole doesn't have to care responsibility anymore because we have we've commoditized the whole thing so much that that you don't you don't have to do this. And if you take the engineering approach of it, it's like a pretty significant part of running a company and running a service is like running it reliably. And so you have these postmortems on incidents to figure out like what went wrong in the process &gt;&gt; and you go back and fix the the shirt.

&gt;&gt; Yeah. And and and the thing is like we we we we are running all on this idea that every engineer that sort of is in this creation process that ultimately let up carries some responsibility and that we're going to that person and not saying like to blame that person but like to figure out like why why did you do wrong here? And so like if you do if like the machine now produces stuff at like 10 times the speed the responsibility thing does not scale in the same way because a machine cannot yet be responsible. And I don't actually know if there is a future where you can abstract away human failure so much in in how we run engineering that now the entire company now no longer cares about who signed off on a poll request or something like that. We that we automate it in the same way I think as we are sort of automating t-shirt creation. I I just don't yet see that. But &gt;&gt; so here's the thing. I think one thing we software engineers or or IT people underestimate is just how freaking complex the world is and how much human squishiness is in each little nook and and granny and and corner, right? So we we thinking, oh, we could we were now able to automate that thing. Uh now we can automate everything like every bit of knowledge work. But but we as software engineers are so bad at becoming domain experts that we don't see all the non-machine parts that go into a workflow and we running through the same fallacy here again. We we seeing models doing incredible things.

I'm not disputing that. Like this is for me this is like w basically all my research in the 2000s is now null and void because transformers can do all the things. But we are overextending that to to everything like we always do in software like like we did in edtech.

Yeah. We have tablets in classrooms now.

Sure. Now it's solved. Education is solved because we have now computers. Um &gt;&gt; well, in fact, I've heard I don't know which country it was, but they're now rolling back in Sweden. They're they're taking the tablets out from the classroom.

&gt;&gt; It turns out if you do some scientific investigations into the tactics and effects on pupils, if you do just throw a bunch of tablets into a classroom, close it and hope for the best. Turns out the best is terrible.

Um, so yeah, I'm that for me I think the biggest takeaway from the past two to three years is the hype is terrible because it's dehumanizes everything and I want to not be part of that circus.

&gt;&gt; Well, speaking of not wanting to be part of the circus, let's talk about Pi, which is a which is a very popular &gt;&gt; Let me get my clown nose &gt;&gt; and also minimalist coding agent. C can we start with the the backstory of why you decided to build PI at a time where there were already uh agent harnesses around right because they were

&gt;&gt; Tell me more.

&gt;&gt; Yeah, sure. I so I I was a a believer in cloud code just because they kind of created that whole genre through the invention of a gentic search. I mean inventions. There were precursors to that and shows of giants and so on, but they were the first that packaged it up in a really compelling package. And at the time that fit my workflow really well. It was simply it was predictive.

So the LLM um uristic nature or stoastic nature of of being kind of unpredictable, but everything around the LLM was kind of nice and tidy and easy to understand.

&gt;&gt; So were you were a happy user of claw code, right?

&gt;&gt; I was super happy. I was proitizing it.

But eventually the team started dog fooding and getting more and more tokens I guess and kind of increased velocity and team size. And with that came more features and much much much more bucks.

And I personally like simple tools that are stable um that I can rely on even if they have non-deterministic parts. But all the deterministic parts should be as stable as possible. And that was just not the experience with cloud code around summer 2025.

&gt;&gt; Mhm. So I kind of soured on that real hard.

&gt;&gt; Was it was it bugs? Was it unexpected behavior?

&gt;&gt; Like so they take away your control of the context. They would inject stuff behind your back which is bad. And then your workflows that used to work stop working because there's now a system reminder that you don't even see in the UI. Um that will modify the behavior of the model. They would also do this to the system prompt. I I I reverse engineered. I mean, I wouldn't call opening an offiscated JavaScript file and unoffiscating it reverse engineering coming from a more low-level background, but I reverse engineered cloud code during the summer of 2025 and build a little service where I can track the progression or evolution of the system prop and tool definitions in cloud code and it's like every release it was like messing with stuff. CC history. Mario.at if you want to see that. And uh yeah, that that just messed with my workflows and I don't appreciate that. If I commit to a development tool, I want it to be a stable, reliable thing like a hammer. I don't want my hammer to break at a different spot every day.

&gt;&gt; Yeah, &gt;&gt; that's terrible. So, that's what happened with Claude. But again, I'm this is not like I'm not roasting the team. I think they're some of them are really nice people I got to know on the internet. They're just dog fooding and that's perfectly fine. We need somebody who like goes the the full velocity kind of way. I But I don't want to work with a tool like that.

&gt;&gt; Yep.

&gt;&gt; Because I can't get work done.

&gt;&gt; It sounds like the move fast and break things. to break things was not for you.

&gt;&gt; No.

&gt;&gt; And uh then I looked into alternatives and AMP and Droid came out around that time. I think &gt;&gt; pretty early in 2025.

&gt;&gt; I don't remember.

&gt;&gt; Was early was very early. I think they they sort of spun off from the same experience of taking because I think AMP was around when Clo came out. I'm pretty sure around that time. Yeah.

&gt;&gt; Yeah. In any case, I looked into those harnesses and they were super good. um they were just super expensive as well because none of them could basically use what made cloud code enticing on top of it being a cool tool um the subscription and that works in an enterprise setting where you're paying by token anyways um but it doesn't work for the small tinkerer in the garage while I'm not a small tinkerer in the garage in the financial sense anymore I kind of still relate to that community and I would like to use my subscription with something so I looked into open source alternatives and found open code. But while that kind of wipes me with my OSS roots, um it too did stuff to the context I didn't appreciate behind my back. Um pruning tool results after a certain amount of uh uh tool result token output or asking an LSB server after every single edit the model makes uh if there is an error. Yes, there will be an error because the model isn't done yet with its work. So the code doesn't compile. So the LSP server will &gt;&gt; so like reaching out to LSP. The language um &gt;&gt; language server protocol server. Yes. So um when you go into VS code and you type some TypeScript you have like in the bottom some error diagnostics and that comes from an LSP server for TypeScript &gt;&gt; and Open Code runs an LSP server on your behalf in the background and feeds the model with uh diagnostics from that server on every edit. We as programmers how do we work right? We go into one or more more files. We added line after line after line and only then look at the errors that resulted from that. In open code's case or in other harnesses cases that also support LSP, the model calls an edit tool to change lines and they would inject the diagnostics after every edit call and that's just not smart because now you're confusing the model with you have an error, you have an error, you have an error on the model like yeah I know I know I'm not done yet. Oh, it's not Yeah, it's not great.

Anyways, TLDDR uh open code wasn't for me um either. It was also I had to fork it to modify it which I don't think should be necessary. So then I just thought how hard can it be? I built my own little thing.

&gt;&gt; And then your own little thing is pretty minimalistic. What does it use? What's the basics of of PI?

&gt;&gt; The basics of PI are um my own abstraction over all the LM provider APIs because I didn't like the VCEL SDK, the VCELI SDK for various reasons. Armen kind of wrote a blog post eventually about that as well. It's obviously good to use. Lots of people use it. It just didn't fit my old man um sense of abstraction.

&gt;&gt; But this is the beauty of software and open especially open source. You can build your own always.

&gt;&gt; Yeah. And now with agents you can even do it faster and produce terrible complex software. No. So I built an abstraction over that. Then I built a little abstraction for generalized agent loop with tool calling and streaming all of that. I built a bespoke little tool that doesn't flicker or not a lot. And then I tied that all together into a coding agent that looks like clot code or codeex or whatever you have. Um that's it. And the extent ability comes from the fact that this minimal core has so many hook points uh that you can basically hook into with a simple TypeScript module um that gets loaded into the same node process and that allows you to do things like provide the LLM with custom tools uh do your own compaction implementation uh fully revamp the TUI itself. You can modify everything in the TUI. So if you have a special &gt;&gt; the terminal UI exactly &gt;&gt; if you want the TUI to behave differently for a specific workflow you have like say you're non techy uh you can change the toy to become whatever you need as a non techy and I have a couple of nonty friends that did that because they don't need to know how to build this they can just ask pi to build it and pi will modify itself oh so this is a thing right so you can ask pi to modify itself because of the extension points and it can write code that extends itself and it's trivial, but it's a big unlock.

&gt;&gt; Is this what you meant when you said that? For open code, you needed to fork it to to modify it. It doesn't have this.

&gt;&gt; It does have a plug-in system, but there's not a lot of extension points and was very rigid. I think they changed it recently. I think it's much more open now. Um I I haven't kept up with it, but might be better now.

&gt;&gt; So, I guess Pi Star has this very minimalistic thing. As I understand the the tools it has is read, write, &gt;&gt; read, write, edit, bash. It's all you need. That's it. And and then you can actually like start to make it your own like okay like at at what are examples that people would add. Pi doesn't have MCP. People just ask Pi to build MCP support into PI. Pi doesn't have a plan mode. Armening goes and my plan mode must be fantastic bespoke and super.

&gt;&gt; I don't have a plan mode.

&gt;&gt; Yeah. But he has like five implementations of a plan mode until he realized plan mode is entirely useless.

Other people just like messing with the UI and making it their own, like a different visual style of the editor box where you enter your prompts, stuff like trivial stuff, more cosmetic stuff. Um, other people have re-triggered it for fullblown RL environment for open weights models where they use pi as the agent that does that part of the RL execution environment. So it's you can do anything really. What drew me to it beyond like actually using the library abstraction was was in fact the the custom tools part because um one moment for me was um over Christmas again like many people had some time and I tried to build other things and I and Peter was talking to me in in November that he's like vibing without looking at code more or less. I don't know exactly how he said like but like he's like he can do this now like okay I I want to build a thing where I don't look at the code. I wanted it to not look like slop. I wanted I wanted a version of it where like afterwards like even though I don't really look at the code it should look like what I would have written and so and I wanted to make a game and so then I I basically started the whole experience with like a just basic pie like we want to build a game but actually before we build a game I want you to set up the codebase in a way that you can validate the changes that you're making but also I can see them like like a like a twoprong kind of approach like I wanted to be in the loop but also O have the agent be able to validate itself and and what what sort of emerged out of that was well first of all like it built itself some debugging tools into the game so it can make screenshots and like run a simulation and sort of dump out state and read it again but also pi can can show images in a TUI and and and I added so a bunch of like I talked with the twanker to figure out like what would be interesting things to do but we we ended up having like a all the screenshots I can tap through quickly in the UI or I can pull Alo this great feature it can reverse to an earlier state in the conversation and then it can branch within the conversation to build a bunch of stuff around that because like these the sessions especially with screenshots and it become very token inefficient very quickly. It was actually one of the other things that pi was rather quickly rather good at was having a lot of screenshots in it &gt;&gt; because open claw people had a lot of screenshots in their chats and open claw is using pi. Yeah. So yeah, &gt;&gt; but but having this like it it felt really magical for me to actually treat the problem as I don't know what the right way of engineering here is but very clearly part of it is like I should be in the loop so we can figure out like how to specifically for the problem at hand do that and and it turned out like for web project and computer games and some of the other things I tried they're kind of different but very many of them are sort of come down to a similar thing where The agent interacts now with my program and it should do the most optimal way and I want to interact with it in conjunction with it interacting with the program and the entire experience should be as little confusing as possible to both me as a human and to the agent. And I found it very very fascinating just to see how that emerges where like your tool all of a sudden when you launch it in this program looks and feels different than if you launch it in the other program.

&gt;&gt; I really like this point. Arma made just a few seconds ago that AI works best when the engineer stays in the loop and the system can actually validate what changed. And this is a great time to mention our season sponsor Sonar. AI can now generate code faster than you can verify it. Sonar, the makers of Sonar Cube, sees this leading to serious gap in verification. With the rise of coding agents autonomously writing code, verification is no longer a nice to have. While the latest coding models are extremely intelligent, they also are errorprone and they don't fully understand your code base and your context or your objectives. This is why verification must be mandatory in agentic workflows. Sonarq provides a zerorust multi-layered approach to code verification that is consistent and repeatable. It analyzes semantic syntax, data flows, and architectural boundaries at agent speed acting as a critical trust and verification layer before any code reaches production. Covering 40 plus languages and 7500 issue types, Sonar Cube is the most comprehensive code verification platform available.

And with easy integration via MCP, CLI, and hooks, it fits right into your existing AI tool chain. Let agents move fast and have Sonar Cube as the independent multi-layered verification for safe, reliable, and auditable agentic development. Head to sonarsource.com/pragmatic to start verifying your agentic workflow today. I'd also like to talk about our presenting sponsor, Statsig. Static build a unified platform that enables both experimentation and continuous shipping. Built-in experimentation means that every roll out automatically becomes a learning opportunity with proper statistical analysis showing you exactly how features impact your metrics. Feature flags let you ship continuously with confidence. And because it's all in one platform with the same product data, teams across your organization can collaborate and make datadriven decisions. To learn more, head to stats.com/pragmatic.

With this, let's get back to the episode and to the topic of general versus purpose-made tools.

&gt;&gt; Yeah, I mean, I spend a lot of my youth on construction sites to earn money. And you don't use a hammer for all your problems at a construction site. You have a screwdriver, you have your hammer, you have your drill, you have whatever. And I think in engineering, it's kind of the same. Um, I'm not using the same tool for every task I do as an engineer. So now, if I use an agent, I don't want a general agent for every task, per se. I want a specialized thing where I know the performance will be topnotch for that specific task because we built the harness in the way that the agent can be most effective at this this task just because of the construction of the way the the harness is constructed and that's what I wanted to enable with PI. That said, I'm probably the person that has the least amount of modifications in Pi. I have like two extensions that I use and they're trivial. They're basically just if you see a URL that looks like a GitHub issue or pull request thing, pull down the details via the GitHub API and display me a small little widget on top of the editor that gives me the issue title, the author account, uh, and a link to the issue. That's basically all I do.

&gt;&gt; Well, but it might work for you as a minimalist.

&gt;&gt; Yeah, I mean, that's how I work on the on the Pyon repository because I might have two or three of of sessions open in which I process an issue or pull request. That way I I remember what s what the session was about.

&gt;&gt; But sounds like you also made your Pi for that for working on the Pi monor repo a specific one. And if you if you were working on a if you went back to building games, you'd probably have a I never thought of the fact that you might want a different harness for a different task. I guess we just kind of assume that most developers you work on your main thing at work. You might have a side project and just experime experiment with whatever. But this this I wonder if this is a new new thing that we we could never have. We could never have custom tools for a project. That that just sounds crazy, you know. Here's here's the like my intuition is this. I think where we are going is software that modifies itself on behalf of the users's wishes and needs and the agents can do that now if you give them enough rope to modify themselves. And I think with Pi that is my first foray into this kind of selfmodifiable malleable thing.

um just for the coding agent sector but I think this this this actually can be extended to all kind of knowledge work to a degree for specific tests within the broader set of knowledge work obviously the humanization and so on you know but yeah um the next plan here is actually to have an alternative user interface to the TUI because the TUI is obviously limited and the best alternative stack is obviously the web because it works everywhere and can do anything so once I have that built out that then it really becomes was interesting because then you're not limited anymore to the line based rendering of a terminal. Now you can do really really interesting stuff. And so yeah, we'll see how that works out.

&gt;&gt; And one reason that I learned about Pi before I I knew that it was this minimalist interface is how OpenClaw is using Pi. How did that come? And we were hanging out and and reviewing each other's blog posts and and just throwing ideas at each other. And in October, I started building out Pi and Peter started be building out V relay, his little WhatsApp assistant, so to speak.

&gt;&gt; Oh, that's how it started.

&gt;&gt; Yeah. And he was in search of a a gent or copy. I think it started out by him taking Pi and cloning it and calling it towel and then modifying it. But eventually he got tired of having to maintain that. So he just said, I'm going to use your stuff. And that's how it ended up being. Pi wouldn't have compaction if it weren't for open call.

&gt;&gt; No, &gt;&gt; I specifically built that because Peter was crying in the in chat and I need compaction. Okay, you get compaction, but I'm going to tell all my users, don't use compaction. It's bad for you.

Yeah, but that's I guess the beauty of of building on top of open software one another, right?

&gt;&gt; I mean, it has pros and cons. Yes, I'm now get to enjoy all the openclaw instances that think bugs in open claw are actually pi bugs. So they autonomously send me a gazillion issues and pull requests without the users probably even knowing and I get to deal with that in my open source. So that's not that's a negative side effect.

&gt;&gt; Well, so you're you're really on the receiving end of this I guess.

&gt;&gt; I mean just just like open call itself is which is much more exposed to this problem. I mean they have tens of thousands of issues now and there's no way they can get a good uh grip on that.

But but how are you dealing with the fact that you now have open claw just AI autonomously opening uh things on your repo as a maintainer? Do you build tools to battle this and try to close them out or &gt;&gt; build a tool for open claw ones which embeds issue and pull requests into a 3D space so I can see the clusters of similar things that agents would have sent to the repository and then I can bulk select things and close them out in in &gt;&gt; Oh really? So you actually have a 3D like visualization.

&gt;&gt; Yeah. open for context at I think it's less crazy now but end of December to I think midFebruary I mean it was exploding obviously but like this explosion almost like directly translated to I I I was on this repo refreshing pull request and the number went up &gt;&gt; we we actually tried to contri contribute and help out Peter a little bit but I immediately gave up &gt;&gt; I didn't know how to do anything useful there was looking at this I was This is a type of software engineing I'm just not used to.

&gt;&gt; Yeah, I I I would fix two things and spend an hour on them and then five minutes after I committed and pushed it, some clanker comes along and just reverts my fixes and this is not how I &gt;&gt; Okay. Can we talk about the name of the name Clanker?

&gt;&gt; Oh, sure. Um, so Clone Wars, Star Wars, I I actually never watched it. Um, but uh kids of friends of mine watched it a lot while we were visiting them. So I kind of through osmosis got the lore and there is an army of robot robots and the Jedi would call them clankers or people who call them clankers because when they move they clank clank clank. Yeah, that's the origin of that. Yeah. So an AI a droid. Yeah. Exactly. Yeah. But coming back to the how do you deal with the influx of agentic pull requests and issues? I just auto close every pull request. A human agent doesn't matter.

Um, what I do is if I haven't had contact with you previously, my GitHub workflow knows about this because if you had, you're in a file in my Git repository, your account name. So if you're not in there and you send me a pull request, your pull request gets autoc closed.

&gt;&gt; Mhm.

&gt;&gt; And then my little workflow posts a comment under your pull request that says, "Hey, thanks so much for contributing. Really appreciate it.

Could you please open an issue in a human voice? uh no longer than a screen's worth of text and uh if I like it I type looks good to me and then that account name gets put into the file and the next time they send a pull request they pass and it turns out agents don't see the comment my GitHub workflow posts underneath the pull requests. So this is a great filter for filtering out agents and keeping the humans safe more or less from &gt;&gt; this this is interesting. I wonder if this might be the like an unavoidable future where like we just need we need a way to separate is this coming from a a human with an intent or an AI.

&gt;&gt; I don't necessarily care if if if it were actually a good PR then if it came from a machine it's it's it's actually fineish. I think what's interesting in PI is like and and open CL even more so is like it it accumulates pull requests well actually there was no intentionality behind it at all and so the the person that dispatched the machine didn't actually care that much about it &gt;&gt; but didn't even know about it &gt;&gt; or didn't even know about it and I've done open source for many years and there was also there was a there was a big difference between someone send a pull request up or like an issue and like hey please fix this but actually didn't care enough to even reply to questions anymore. Like this not uncommon. And then you don't actually have to fix that, but you have to close it out because like maybe it's it's still useful input, but like it clearly that person wasn't caring enough. And with the pull request, it's even worse now because they come in so quickly that many of them cannot be merged anyways without manual resolution of the conflict. And there's a there's a lack of back pressure mechanism because even I as a human if I see there's like 500 pull requests open like I probably will not contribute to this thing now because at the worst I will make it worse. Yeah.

&gt;&gt; And and I think previously in open source you had the people who would just send issues and be very entitled and say you're the worst person on the planet if you don't fix my little issue. But that's fine that can be handled. And pull requests were kind of special because it needed a human to invest quite a bit of time to produce them and you don't have that anymore. You just have people, oh this this should be easy. Uh agent, please do this thing.

Make no mistake, send it to this repository and that's just not going to happen. So basically what we need are bottlenecks. I'm not necessarily I don't necessarily need human verification or a verification that you're human. I just need a bottleneck that allows me to process the amount of incoming things as a human because in order for Pi to not dtoriate into a pile of garbage, I still believe that it needs me and other capable people reviewing at least the important code and for that I need bottlenecks because otherwise I can't deal with.

&gt;&gt; It's it's a second law of thermodynamics, right? It's like everything degrades towards chaos and you have to put extra energy in to to keep it away from this uh from this outcome and we don't see and feel like the pain of the codebase anymore if we stop looking at it and people don't feel the pain or like they feel no restraint anymore and and it's the issues are also interesting because on the one hand it is something great about someone doing an investigation and sending you a description of that that can be good and can be bad, but they look very similar.

Like it takes quite a bit of energy to tell apart a good and a bad AI generated issue request. And unfortunately, like most of them are not great, but some of them are actually good and that's also kind of it's weird like all of it is weird. I I really don't know what the future of open source is in many ways because like the a lot of open source really worked because people piled out on hard problems and so they congregated around it and said like now we need to have a good database so we're going to put all this energy on building a good database and the the value of open source came from there's some hard problems and we're going to our energy together and we're trying to figure out how to solve it and and now it feels like open source is all about like growing stuff up. What what really grinded me so mad was people particularly like a lot of Atlantic engineering right now is like building more stuff for Atic engineering. So it's like it's yuborous or yuborous or what I call it and and I I see this tweet and it's like oh I solved problem XYC and here is my solution for it and you click on this thing it's like it's 48 hours old that person probably never used the thing that they built. I would like to suggest to the viewership to look at Arvin's GitHub account over the last year and what happened there.

&gt;&gt; Yeah, I built a lot of the stuff, but I don't then go on Twitter say like, "Hey, I solved the problem, right?" is like I I have a [&nbsp;__&nbsp;] ton of VIP slop on my GitHub account and I wish I could mark it differently because like maybe there's some utility in it, but unless you're going to actually have that codebase still be there a year, a year and a half from now and someone is still using it, the utility of that is actually not validated in a way. And there's so many markers and and metrics you can look at now for GitHub that really demonstrate this this explosive growth of it.

But if you were to then maybe find some other number to see like how many of the things that are being created are actually turning into like really fundamental pieces that can sustain open source communities that can that can actually deliver this value that scales amazingly. We haven't actually created many VIP engineered projects that have become that. But I I like how you mentioned energy and how open- source always worked. If we just think preai again, let's say Linux, the most successful or or widely used open source project, it has both an energy and a structure. You know, people come in with intent that they want to add something.

They have a process where it goes through. There's human trust at every level. There's a little pyramid and in the end it all goes back. Each change request goes up one level and in the end Lionus uh does the cut. But there's a lot of energy. There's a lot of intent.

uh there &gt;&gt; there's a lot of humans &gt;&gt; there there's a lot of humans and it was always about human energy and now we suddenly have this AI which it's just tokens right now they're who knows how much they're subsidized or or not or it's just machines doing and then suddenly you know they create plausible things that that look like human energy and it's hard to differentiate and suddenly just like there was this wrench &gt;&gt; actually disagree I don't think a lot has changed to open source &gt;&gt; okay &gt;&gt; um &gt;&gt; the volume has changed &gt;&gt; no yes uh But that's just a number. Uh the the amount of as you said the amount of actually useful and maintained projects has probably not changed a lot.

&gt;&gt; So you're saying that the ones that were there, they're still useful and maintained.

&gt;&gt; Not even the ones that were there, there might I mean there's a specific rate of new open source project that survive longer than two weeks.

&gt;&gt; Mhm.

&gt;&gt; That's always been the case, right?

&gt;&gt; Mhm.

&gt;&gt; So now we just have more projects that die after two days than before. But we still have the same amount of projects that will have a long-term viability just because there are humans that actually care to maintain the thing over a long time. Build a community of humans that support the entire thing. Build an ecosystem around the entire open source project. That makes &gt;&gt; you say not you're not believer into mold book.

&gt;&gt; No, I mean good job meta putting that up. Super useful. Um, no. I I I think at the end of the day we we're kind of freaking out when we don't actually need to because apart from the fact that I personally can now generate code faster speed of light for me building an open source project and that entails not just the code but the community around it the spirit around it the ecosystem around it nothing changed um what changed is mechanical parts I I need the bottlenecks to deal with the influx of exponentially growing uh agents pull requests whatever um GitHub itself is under immense pressure Because now it's not just humans hammering their infra, it's now billions or millions of Open Claw instances hammering their infra.

&gt;&gt; Yeah, &gt;&gt; everybody complains about GitHub going down. I actually think they're doing a pretty good job. Like that's a lot of traffic that's coming their way since basically Christmas. It's basically open call. So yeah, I I I would be a little bit more optimistic. We're just indeed messing around and finding outstage at the moment and everybody wants tokens to be a KPI just like lines of code used to be a KPI. We've seen this speaking around of of things that don't change and messing around and finding out. You you wrote a a tweet or or you wrote somewhere that your biggest enemy is complexity. It's also your agent's biggest enemy. Can we talk about that?

&gt;&gt; Very simple. If I have a 600 lines of code code bis and my agent can at best be affecting effective up to a context window size of around 200,000 tokens, how much of the code can the agency see?

A third, right? Great. Um, if you manage to get all the relevant code for a task into that context window, you're probably okay. Although that is a separate project an information retrieval pro uh problem which is not solved and which agentic search also doesn't solve that is does are you sure that the agent finds all the relevant code it needs to find to to fulfill a thing that's also where all the garbage code comes from because it doesn't see all the thing it needs to see in this case let's assume the best case information retrieval is solved everything fits into a context agent does a good job okay that's not the reality we're living in because now the agent spit out so much code they themselves cannot possibly read into their context on a new task anymore. You know what what I mean?

&gt;&gt; Yep. They develop their own context window.

&gt;&gt; Yeah. Exactly. The complexity they add is their own worst enemy because eventually the code base will be so big and so complicated and so interconnected um that the agent has absolutely no way on a technical level to ingest all the context it needs to do the new task. And I would like to point out that the agent has learned all of this garbage from the internet and from us because on the internet there's all our old code. While there are some pearls, uh there's also a lot of swine.

um because we have a gazillion GitHub projects from the olden days where we just tried out things and because instances like Linux or any other really well-maintained and well-ritten open source project are minuscule in compared to all the rest of the garbage and a machine learning model will kind of converge towards not the well simplified to the mean right and what is the mean then it's it's not the handful comparatively of excellently engineered projects it's all the garbage on the internet all the cargo culting all the trend type of the day kind of stuff and that's what we get when we let the agents do all the things for us.

&gt;&gt; Yeah. So we have this problem of things are getting more complex which slows agents down which will in fact impact quality which we were just talking about but Armen now that you're you're building your own startup you two of you're building your startup now how are are you and you're working with agents right and they they will have these things how are you dealing with generating code building products balancing quality tech complexity &gt;&gt; I'm dealing with that &gt;&gt; badly look I think that &gt;&gt; we're coping. We're not dealing.

&gt;&gt; I don't know if I wrote this in the blog. I definitely have it on my slides for for the for a conference here. It was um I enjoyed the time from from April to about October immensely because it felt like I can do so much but also like there was no heightened expectation like the world has not yet gotten used to this idea that everything has to now also move at 10 times the speed. And there was a there was a moment of time where I felt like like we we worked in this vibe tunnel thing in the beginning and it was like it felt so much fun because like I I have time now to play with the kids and I just prompted a little bit on my phone and like it felt &gt;&gt; VIP tunnel was where you could set up with your phone talking with your machine where it wasn't that easy &gt;&gt; terminal basically.

&gt;&gt; Yeah. And it's not that we did much with it, but like I it it it had this like happy vibe and like I know that I spent too much time on the computer, but like it didn't I didn't feel any pressure.

But now it's like this like we're collectively feeling like everything has to ship faster. It has to like iterate faster like the the the baseline that we want to achieve in terms of fidelity and everything has to be higher. And so now it feels very stressful &gt;&gt; even in your own startup.

&gt;&gt; Yeah. Because like to some degree you cannot like &gt;&gt; you can be the most stoic person in the world and it's still going to get at you in a way that I'm slowly learning to work with my own emotions in a way on on like on dealing with this. is I I find it very very hard in a way to because like I was I was used to things working a certain way and and I I I knew how I do some stuff and and then I fell a little bit too much in the trap of like giving into the machine and actually doing things in a way that I normally wouldn't have done things &gt;&gt; that you regret.

&gt;&gt; It's definitely a gentic regret.

&gt;&gt; Gentic regret. Yeah. And so like the quite frankly the answer is like I I I feel like now with a little bit of power of hindsight um learned some things that I wish I would have learned probably in November.

&gt;&gt; Tell us.

&gt;&gt; Well I mean like a lot of it is like really the recognition that if you there's no back channel to the to me or to any other engineer when under normal circumstances there was a back channel.

was this this this feeling of like things are not quite right in the codebase like there was this now the change is harder and like the complexity like do you sort of see then the complexity of the pull request getting higher but like if you rubber stamp it then like what's what's the back channel there and so like this this mechanism this back pressure this friction in the codebase you don't feel when you work with the agent &gt;&gt; I think there's a way to kind of measure it and um like if I scan through my sessions on a project from start to current date. I think the frequency of curse words increases because the agent starts messing up more because it itself cannot deal with the complexity of the add to the project. And I would be actually really interested in whether this measurable because I feel it uh in most of my projects now it occurs a lot more.

&gt;&gt; But you you mentioned friction in the software. You didn't say tech depth. You didn't say complexity. What what what is this friction? cuz I I don't remember us talking about this pre- AI at all.

&gt;&gt; So, I found this ironically kind of funny and it it's kind of sad, but so I will not name any names, but uh there was a what I what I assumed was an incident related at least in part to achieve engineering on on a company uh where they they shipped out a configuration change that ultimately result in a security issue and look things happen. But the link that I saw on this had the social preview of that company's tagline and the tagline was ship without friction. And that g that get me really gave me pause because like you I I know as an engineer like we used to talk about like you got to get rid of like all the things in the way so that you feel happy shipping stuff. But there there always were changes where you really wanted to think like do you want to drop the database like do you want to merge this migration which might take a table lock that could potentially take you down. Right? is like there's there's this this moments every once in a while where you really you were really supposed to think and and you and people created checklists or people created um like like mechanical gates that would where you would have to confirm something like there's there's certain things that we used to put particularly if you run a SAS company did you put stuff in so to slow things down or and in in some of the best engineering teams in order to mature a service you have to define an SLO you have to define um like yeah expectations and like if if your service is supposed to be critical but like there's some other stuff that unlocks on this sort of tree of of requirements that you and and and like a lot of engineers be like a this is also this bureaucracy but like the reality is like if you do this correctly then it saves you time and it like it makes you happier. You're not waking up at 3:00 in the morning like all of this is useful.

&gt;&gt; It's like friction injected to deliberately slow things down. I guess the easiest example in any decentsized company you have services based on tier based on criticality. the highest tier uh software now needs to have let's say two or three code reviews or an approval from a director to do a configuration change which again all slows down but it's kind of like we know this is on purpose like by adding this friction we want you to think do I want to push through this friction in terms of time invested or effort or having to justify things etc. It makes you think about do I really want to add this to the codebase if I know that the end effect will be that it has to go through this entire chain of arteries. Um, so it can be coming back to saying no to yourself to avoid pain going through that process &gt;&gt; and then taking on the pain when you know that you have the comm you have the &gt;&gt; the backing you have the confidence as well, right? Like so typically when it's a higher friction thing, let's say a tier one service or highest tier service where a director have to sign off. When you're a new joiner on the first day and you don't know the context, you probably know that that's a pretty large ask and you'll probably socialize, get get by in from a from an experience and to say like, oh, this is the right thing, you'll go with them, right? Back to human dynamics a little bit.

&gt;&gt; I think the the the thing is like there's a there's a there's a very delicate balance in the whole thing because like you don't want the friction to be just an accident of having created bad developer experience, right? But some things look the same and and but they but they were deliberate but they maybe were not sufficiently documented but but there's this feeling now like get rid of all the friction so that the agent can be very autonomous so that he can run many of them simultaneously. A lot of it comes from that. Like I like it the these things are actually rather slow and the only real time saving that you get from it is parallelism and so somewhere there is is this trap. I feel like a little bit more experienced now in managing the trap but I I don't have the solution for that either. And I I will not like say that here's an example codebase where I felt like really really great about the stuff that I built except for pre-existing libraries from before aentic days where where I still feel like strong emotional attachment to them and much more careful about doing them than than any of the code that we other than pi to which I don't have access.

&gt;&gt; Oh no, there's there's still no right access. Um, there there's a lot of sloppin pie, but I try to avoid it in the in the bits and pieces where I know that's important code like we have an HTML export functionality where it takes the current session and just spits out an HTML file that you can then host on GitHub and whatever. I have not looked at a single line of code for that function. I don't care if it's broken, if it looks right when it comes out. But then then there's the the agent loop itself or the the extension loading mechanism and all of that stuff and that's important and the way I deal with ensuring that that has or at least trying to ensure that it has high quality is I refactor mercilessly because that pulls me into the codebase.

I need to understand what I want to change structurally not just line per line and syntactically or whatever. I need to understand what's going on to do a good refactor. and and doing that every now and then like I'm doing now at the moment prompted by wanting to add a new feature that's currently not possible with the current architecture being in the code is the one thing that keeps the codebase quality high and the complexity low but that's against the industry wisdom of burning as many token maxing basically yeah that's that's that's an interesting one happening but you just recently wrote on on the same theme a blog post called we all need to slow the f down can we rehash some of the thinking and what triggered you So just put it out there. Okay. So the basic gist is okay, your agent can now spit out 10 times more code a day than you can, but it also means it spits out 10 times more boooos errors. Even if it has half your error rate, then okay, it's not 10 times more. It's five times more. It is still more than you would spit out. So the rate of deterioration in your codebase has now increased. And now go dark factory. Now take a 100 agents that do this to your codebase.

What's the end result of that? So that's the first problem, right? And you need some way to review all of that code that now gets generated to fix all the boooos. But you can't as a human because as a human you're used to spitting out 1.5k lock a day and that's about the limit that you can actually review well right agent spits out 10 times that no chance you can review that. And not not all of the code by the agent might be important like the HTML export thing, right? But even if the agent spits out three to 5k a day, you have no way of reviewing that in any meaningful sense.

And then if you do the the armies, yeah, I mean, and then the armies, this is interesting. So you call it the dark factory. The idea being that tens or hundreds or thousands of agents, you give them a spec, they go and they break it up, they they organize themselves, they like the mayor and all all that jazz. They have the qual the QA agent.

They have the you know you give them roles. You give them context and then you give them enormous amounts of tokens and spend. And the idea is or the hope is that your software will be done in &gt;&gt; Oh, there will be something will be done. I definitely something's going to be done. First your purse and then uh No. Yeah, sure. More power to the people that make that work. I can't make it work. And the reason I think I can't make it work is because I still care about the quality of of my product. And I don't care if it's built by by hand or by agent. I just want the quality to be good. Both in terms of how easy it is to maintain it and add new stuff to it on an developer side and on the user side.

All the companies claiming that all of their code is not written by by agents.

Yes, we know quality is garbage. We feel it in our bones when we use your products. It's garbage.

&gt;&gt; U so I don't want that. And yeah, basically I think people need to turn around and say, "Hey, what what are we even doing here?" Um, we have these wonderful machines now that can take away so much pain from us by doing the stuff we hate doing and doing that really well. Why don't we start by giving us some more free time to work on the interesting bits and delegating the stuff we know they can do to them large on large like like uh across the entire organization. find all the things that annoy the out of you and have the Asians automate that for you. And then you suddenly have time to think about what do we actually want to build? What do our users need? And if we decide to build the thing, then we can pull in the agents again and say, and we're going to polish the out of that because now we have the time and the means and the tools to do an excellent job. But that's not how we're working. We we we build an army of agents and install beats and uh make a big spec that hopefully will result in something crazy. But here's the thing. We we talked about where did the agents learn the knowledge from, right? The internet. So garbage to mediocre. Now if you write a spec, &gt;&gt; what what's the best possible spec you can have? The best possible spec is well you define exactly how it should work.

You give it test cases.

&gt;&gt; Best possible spec spec is the software itself. Oh, I see what you mean. So, yes.

&gt;&gt; Okay. You write a spec that's not the software itself. So, that means there's a lot of blanks that need filling in.

&gt;&gt; Yes. What do you think is the agent going to fill those planks in?

&gt;&gt; Well, most likely from, you know, stuff it from his training data.

&gt;&gt; Yeah. And we already identified what the quality of that training data is, right?

Garbage to mere. Well, and even even before AI, don't forget like Stack Overflow had a really big criticism because there was this thing of like well you control C controlV from Stack Overflow and oftent times there will be some answers where the first answer was either not correct or not correct in many cases. Reax for email was a good one. You emailed Reax for email. First page was Stack Overflow. Everyone just copied the first solution and I think underneath number three it was said it missed a bunch of cases.

&gt;&gt; Yeah. But but here's the thing though. I I'm I'm not saying agents or humans are better. They're clearly not. But agents also don't solve that problem. And if you then don't let just one agent that's already 10 times more productive as you do the thing that it's bad at and that you as a human are bad at, but a hundred of those, what do you think is the outcome?

&gt;&gt; Yeah, it's just very simple math. Let's talk about another controversial topic.

MCP versus CLI.

&gt;&gt; Oh, it's it's it's it's coming up. And you know, right now I'm hearing a lot of people really going for CLI is the future. And I think I'm sitting with two of them. But also MC MCPS are also really popular inside of large companies, especially when you talk with a bunch of people working at at large companies. It seems MCPs have found a real product market fit inside of larger enterprises.

&gt;&gt; Despite what people might think, I don't actually hate MCP quite as much.

&gt;&gt; Seems Oh. Oh, wait. We have it on recording.

&gt;&gt; Yeah. No, we don't deal in absolutes.

We're in CIF. So I my fundamental challenge with MCP is that I think first of all the spec is very complex I think for for it but it's like this is this is just generally how specs happen to be.

So it's a bit the the the core of its time. So there's an inherent complexity in it. But if you if you were to say like okay so what is it really doing at the end of the day it's it's authentication and it's sort of invoking some stuff and MCP even theoretically there's structured responses but MCP for the most part is run some stuff put stuff back in the context and then work with it. So it fills your concept very quickly. And there's uh Cloudflare has this code mod MCP which is like in principle I really like. I have an MCP um for testing which uh is a JavaScript interpreter that gives me access to the Google API. And between an MCP like this and a skill, there's not a huge difference because the skill also needs to be in a system prompt. So that defines it. But the the agents are just very very very very good at running code and MCP is not quite running code. It's basically rag is like input in and do some stuff and maybe some state transition at the model also doesn't see but it is in that sense just in it's a hard problem to solve but it does solve off it solves a whole bunch of things. Um I want it to work. I just still don't get it to work like I wish it could work and I my my suspicion is still the glue is is has to be code execution but because MCP servers are largely not defined in a way that the model actually understands them. I haven't found ways to compose MCP tools reliably. I I found ways to make the MCP itself be composable by having the MCP be one tool run code, but I haven't found ways to then orchestrate larger ones. I want it to work and I think it has found its niche and I don't think it's going to go away.

&gt;&gt; I I think it's just a victim of its own success really. when when the whole thing started, I think it was in October 2024, it was more or less a solution to get external services into uh consumerf facing chat apps.

&gt;&gt; Yeah.

&gt;&gt; Connect your emails, connect your one drive, connect your whatever &gt;&gt; pretty much. And then IDs also took it over because it was convenient, the cursors, the wind surfs.

&gt;&gt; Yeah. But I think the origin was basically the consumer side, not not the developer side. And I think that's a totally great use case. I don't want my mom to having mess around with code generation or whatever to invoke some API or call some API and so on.

perfectly fine use case and then developer side also picked it up and thought oh this is a great way to provide tools to my LLM tools as in in the system prompt somewhere there is if you want to call this tool provide this JSON payload and you get this thing back right and that kind of felt right at the time because if you read a tropics uh documentation um they would say our models can deal with about 30 to 40 tools in the context and even that wasn't the case like at 12 20 they would just break down but doesn't matter. Um but but there was still like a yeah this can work if you kind of keep it small and contained and very specific to your use case. And then people started building MCP servers that would just basically map an entire open API spec into a gazillion tools.

&gt;&gt; Yeah.

&gt;&gt; And that's where it all fell apart. So that's the first problem. Very bad MCP servers from big corporations that thought we need this now. What's the the fastest thing we can build? I just push the open API spec of our APIs through this thing and make it an MCP server.

That's garbage. The second problem is that it's inherently non-composable. Um if you want to combine a tool out the MCP tool outputs of two different servers, they need to go through the context the the model itself needs to do the data transformation the the the the yeah the composition of of multiple pieces of data fetched through &gt;&gt; and compared to this with a CLI. It's a pipe, right?

&gt;&gt; Exactly. the the model only sees the end result and it is it is super free in how it massages that data and that's also the idea behind code mode basically it's a hack it's basically okay we now have MCP we know it doesn't work for this specific use case where you have multiple sources of true data and you want to combine them but don't kind of pull that through the context so let's build code mode and code mode is basically we take all the MCP servers you expose that as functions in Typescript and then the the model can actually just write some code that calls the MCP service and then does the composition in the code. It's it's like how many interactions do we want here?

We can just let the mod write the code.

We we don't need the MCP server. And then the third part is David from Sentry is a big proponent of MCP because it's off the off thing. And honestly, that's again for me super valid. But the model itself kind of doesn't make sense anymore. I think that there's a there's a world for MCP2 which is ironically maybe based more on there's a company called stainless which basically generates SDKs out of uh open specs and I I'm I'm really warming up to the idea of like maybe it is an MCP is entirely based on off plus uh like uh &gt;&gt; libraries &gt;&gt; libraries or or or like directly like HTTP request against offsp specs because if you compose it together there. And I think like one of the things that's also like kind of underappreciated and and you sort of as you see I think if you see pi do its stuff because it's kind of transparent of the tool calls that it does. It's kind of magical at times like how creative agents get at large outputs. Like for instance pi when it when it runs a program in bash and it produces too many lines of code actually only reads I don't know what the the cut off is but it reads the first couple and like oh if you want the rest of the file it's 20 megabytes large and it's in this file. And then the agent is like, "Oh, 20 m, that's too much. I'm going to grab on the file, right?" And it they they get really ingenious in in how they're interacting with it. And like, and MCP takes that away. The question is like, how would how would you define MCP in a way where it wouldn't take that away, right? where it still has all of that magic and and and capability and and and I don't really know the answer because I think it's hard but off need solving and and composibility needs solving and and I think there's a there's a bright future of of that kind of stuff and and also like what Mario said if coding agent wouldn't have become so popular then the idea of code generation code running for like non-code related problems probably wouldn't have taken off quite as much too. Um, but like the most capable personal agents, Open CL being a good example of it, they're just coding agents hidden from you. And then that just naturally some random person who is not a programmer is going to say, how am I going to do this? And the model doesn't say like install this MCP. The model says like, okay, I can write a Python script that does it. And so you naturally have this in the sort of the crazy space, you have the adoption of of more code execution. in the compliant enterprise space you you don't have that there's a different path that &gt;&gt; and I personally don't think that models are going going anywhere else other than code generation going forward for any kind of a aentic task I think that's that's mostly a function of there being a lot of training data for code generation and code generation being a very easy means to control computers uh so I don't see a different paradigm there coming out of the model labs anytime soon so I I think taking that as the assumption where the future is going, we just need to figure out how to make code generation kind of work within an enterprise setting with off and all of the other enterprisy things that entails. So, so let's do a fun trying to predict a year out which is hard but in in 2027 knowing some of these basics just again from first principles where do you think these coding agents might be and the software engineering workflow might be you know basically this is just like again speculation we know we cannot predict the future but where do you think that there'll be a lot of focus in the coming year and we might in an optimistic case see some results in in tools and how we work and what's working what's not working.

&gt;&gt; I have no idea. I honestly have no idea.

I I could make up something that's probably not going to happen. I I think the self-mability thing is obviously something I believe in. I I think we will see more of that and and see &gt;&gt; so like self mutable software.

&gt;&gt; Yeah.

&gt;&gt; Yeah. including the tools themselves with with which we built the software and I think that will expand I not only to the tech sector but also to non- tech applications of agentic u tools my is it dog years with your time seven is that how it works so that that's that's basically the model I have right now of like how this stuff works it's like when you ask me like what's going to be in in a year it's like seven years right and to me that makes it incredibly hard to have any sort of predictions about the future because like it's still not one year maybe now it's a one year from like people starting to using cloud code but it feels like it is much much longer much more more time behind and more time has passed and and I think like right now the the closest that I can imagine is going to be like we we we know that code execution and code generation and like this harnessing around it this is this is going to be it because reinforcement learning gets more of that data and my my my strong hypothesis is that as more and more people are starting to wake up to this you can do interesting things with agents there will be a societal recognition also of how much more dependent you are on basically two companies and I think we'll have a conversation about that part uh we should have a conversation about that particular as Europeans um because we don't really have these labs over here and so I hope we have that conversation but like my my best guess is that we'll wake up to the fact that we are now I engineering teams already now telling me that they have code bases that they think they couldn't maintain anymore without the machine. My my guess is that one of those companies will be public and &gt;&gt; and all of a sudden &gt;&gt; and it will be expensive and I think that might actually dominate or at least become a conversation that's much bigger than the question of are you using pi or using cloud code or or something like this. I I also see a we we've seen this with was it myths the new cloud model oh no spot uh the new uh GPD model they will only give this to select partners so now we are seeing uh a split in who can get the best intelligence &gt;&gt; yep &gt;&gt; or the perceived best intelligence &gt;&gt; it'll be interesting dynamics so both of you are working on AI on popular AI tools you're building a startup that of course you're using AI and it it's also around agents. How do you both keep up to date? I've just seen things and it's not as easy to get me on a hype train as it used to be, but that comes with age.

It's definitely easier not being in San Francisco because I I think that just drives me crazy. Like I I hear so many things from my peers over there and that's just like, yeah, I'm not going to go to San Francisco. Thank you.

&gt;&gt; So having a peaceful environment around you where it's not all about tech might be helpful.

&gt;&gt; It helps having a kid. It helps just going outside, climbing trees, going ice skating, and then looking back at what you did just half an hour ago and be like, why would I do that? That's just stupid. I'm into the detriment of maybe people that are trying to stay in contact with me. I I got very good at not muting notifications, not reading emails, and and that has in part become necessary, I think, over over the last year or so. But this like it actually turns out that passage of time sometimes clarifies stuff a lot because like if it's really necessary it's going to going to reach you again. Like I have an unhealthy Twitter addiction which I'm not particularly proud of. Um but in in terms of source of like interesting things that is still a thing but I I try to now sort of consume it in a form of if it's really really important it will stay in the discourse for quite a while and I just wait it out. Um and if it's if it's there until like 3 weeks after it originally happened then probably something to it and and and I don't need this three week head start necessarily but it is honestly it's really hard. It is really hard to to deal with this because there's a there's a genuine excitement in it and I feel like my my my 20 more than 20 years of experience in that space of software engineering doesn't it tells me a lot of stuff but at the same time it hits you in certain ways where you felt like there will be grounding and there will be something to build on and a strong foundation and now it feels like well seemingly everybody else doesn't care about that foundation anymore so maybe you don't need the foundation and for quite a a while it sort of it works and and that is sort of weird and and &gt;&gt; I kind of feel like since we've been funemployed in 2025 when all this started that we had like a head start like I see all the excitement the two of us and Peter had in April last year &gt;&gt; has &gt;&gt; nobody else no no but nobody else at the time has kind of shared that excitement uh that much and then the Christmas break came and now everybody else has that excitement that we had in April right so Now they are learning groups.

Now they are catnipping themselves to immeasurable amounts of lost sleep uh and at terrible code bases. Um and I think it will self-correct because it's not sustainable.

&gt;&gt; Yeah, we we we did see this as as well.

I did a deep dive in the parametric engineer at early March when a lot of people who were very excited in January about all and they started to use the new models what it can do. They went all in at work or on side projects. In about 2 months time, a lot of them were like, "Hang on, it introduced all this complexity. It has these things. I'm not going as fast as I thought I would be, etc." So, I guess there's just a natural thing where you you have a time, anything new, right? A job, anything.

You have a honeymoon period where you've got the blinders on, which you should, by the way. And then you start to realize and maybe overcorrect, but but there's a natural thing where it in general like it just takes time to see the outcome of your decisions.

&gt;&gt; Yeah. So, so I'm not worried about all the dark factory and all the software is dead and sus is dead and all that. I generally believe this is just part of the hype machine and that will selfcorrect.

&gt;&gt; Yeah. As closing, what's a book that you would recommend and why?

&gt;&gt; Code by Pet Salt.

&gt;&gt; Classic. I just love it. It's just such a great read. It's also for non techies and it's the first thing I recommend if anybody asks me what's your job and pointing at that it's like it has much less to do with computers than you think.

&gt;&gt; And I read recently breakneck uh which I unfortunately forgot the author of um it sort of goes a little bit into an exploration of like how China works and how maybe Europe and and the US are different. and I found it at least um thoughtprovoking.

&gt;&gt; Well, Mario and Armen, thanks a lot for for this conversation. It was great to have it in person. Thanks for having us.

&gt;&gt; Thank you.

&gt;&gt; This was a really fun conversation.

Thanks to Mario and Armen. The idea of self-motifiable software really grew on me. Mario said how Pi doesn't have MCP support, plan mode, and many other features that devs would want from it, but you can build it into his own code.

So far, it's working. Pi is popular because it modifies itself. I wonder if and when this concept of self-modifying software thanks to AI will spread outside of just the dev tool. I also liked how we talked about the observation that agents don't feel pain but humans do. When a codebase gets too complex the human engineer feels the issues this creates and this tech depth is what pushes refactors and rewrites.

But agents simply do not do this. They just keep adding to the complexity. And in a codebase where devs regularly feel the pain of the codebase and do something about it, the quality will probably be also better. And finally, the MCP versus the CLI discussion, this was a good one. MCP is more about offering tools for AI through context and CLIs allow piping one tool after the other. Both Mario and Armen are more of the fans of the CLI, but in all fairness, MCP has its use cases, for example, inside larger companies. The right tool for the right job. Do check out the show notes below for related theatic engine deep dives that go even deeper into related topics. If you've enjoyed the podcast, please do subscribe on your favorite podcast platform and on YouTube. A special thank you if you also leave a rating for the show. Thanks and
