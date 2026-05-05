# How to Set Up Tmux for AI Coding Agents

| | |
|---|---
| Creator | pookie
| Published | 2026-04-26
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=nj_nVIfXRA8](https://www.youtube.com/watch?v=nj_nVIfXRA8)
| Generated | 2026-05-04

## Description

My tmux setup for working with Claude Code, OpenCode, and other AI coding agents in the terminal. A walkthrough of my .tmux.conf and the case for why tmux is essential for progamming with AI agents in 2026.
My job has shifted from writing code to instructing agents so the CLI is where I spend most of my day. tmux is the layer that makes that actually workable, persistent sessions you can detach from and reattach over SSH, parallel panes for running multiple agents at once, and a status bar to tell you when an agent is done.
I start from vanilla tmux and rebuild the config the way I actually use it day to day.
Links
- My (tmux) dotfiles: https://github.com/vossenwout/pookie-dotfiles
- Claude Code: https://claude.com/claude-code
- OpenCode: https://opencode.ai
Timestamps:
00:00 - Why tmux is essential in my CLI AI Coding workflow
02:30 - Why I prefer neovim + tmux over Cursor/VScode
06:30 - Creating vanilla tmux setup
08:35 - Intro to how tmux works
09:50 - Remapping the prefix (Ctrl+b is awkward)
11:15 - Basic tmux usage: windows and panes
12:43 - Fix colorschemes: tmux256color
13:50 - Vim keybinds for splitting / navigating panes
15:40 - Automatic window renumbering
17:30 - Set up base window index
18:50 - Make new windows start after current
19:40 - Automatic window rename to current dir
20:30 - Copy mode with vim keybinds (essential)
23:40 - How I organize my windows for AI agents
25:30 - My coding agents tmux workflow
27:00 - How sessions work and when I use them
30:00 - Styling status bar and applying Kanagawa (best colorscheme)
32:25 - SSH persistence with nested tmux sessions (advanced)
36:46 - OpenCode integration with status bar using monitor-bell (advanced)
#tmux #opencode #claudecode

> Transcript extracted from YouTube auto-generated captions (`nj_nVIfXRA8.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

In today's episode of GenZ discovers ancient unique tools, we will be looking at tmux, which is honestly one of the most important things I use in my daily job as a AI software engineer.

Yeah, it's honestly pretty crazy like that these terminal tools have become so important in my entire workflow as normally my job should be about developing AI tools, doing some data science work, creating data pipelines, etc. But the last few months um instead of writing a lot of code, I'm basically just instructing open code, closed code, these coding agents to do all the coding for me. Even things like refactoring code. Like honestly, some of these tasks are pretty easy to do, but uh when I take like 10 minutes to do them and the AI can do them in 2 minutes while I can focus on another task or instruct some more AI agents in the meantime, it honestly starting to not really make sense anymore to do a lot of these things yourself. How um yeah, how crazy as that seems as I initially also resisted this a lot and I was honestly often doing like, okay, let me just write this piece of code. Like please have me still some influence um or or really have some handwritten code in my code base, but honestly like we are getting to the stage like where this doesn't make sense anymore. So, where do we as software engineers, AI engineers, ML developers basically turn our attention to? And for me, it has become the terminal a lot because these CLI coding tools are honestly like the most powerful AI tools and future of software engineering. So, instead of trying to integrate them with my existing GUI based workflow, I basically for the past five five months have basically been living in the terminal, being getting used to neovim, which honestly I can't see myself living without it anymore.

But besides neovim for inspecting the code, also tmux has become the cornerstone of my workflow as tmux is very useful in a lot of cases where you want to spawn a lot of agents in parallel. It's also useful for example, if I have my closed code my or my open claw running on my Mac mini, I want to SSH into that Mac mini, maybe instruct it a bit, but when I then exit my SSH connection, I still want the agents to do the work I instructed it.

So, even for those use cases um the best way in my opinion to do them is through tmux. In all these terminal videos lately, you sometimes of course get these comments like, of course you can also do this with cursor and cursor has a UI, but I think I was also kind of like this 6 months ago, but to be honest, once you're used to how much more powerful keyboard driven workflows are, like simply if I can press a button and um an entire agent spawns or I split my window in two and with another button I have a new agent, like the speed with which you can do that, it's simply unbeatable. And even just because it's so much more minimal, it only shows you what you what you need to work, it's more it's it's less mentally taxing. And honestly, for me a much structured way to keep track of all my agents that are running than doing something in a GUI.

It's one of those things where you basically have to force yourself a bit for a few weeks to go through them and afterwards you come out as a better man at the at other ends. Basically, you feel like you are the chosen engineer chosen by God to instruct the keyboard and feel dopamine with every keystroke you do.

It's also one of those things where really investment of time and mastering it like the terminal, it's it was so underappreciated in my opinion. It's fully programmatic. You can chain all these cool unique tools together.

It's so much faster, doesn't take up a lot of CPU resources. If you get good at this, like you can beat a lot of the GUI workflows. So, it might be one of the things like where you as a software engineer can still differentiate differentiate yourself from the rest who are all working in this GUI world while you can instruct a lot more agents at the same time. Also like from an aesthetic perspective, would you rather work in a GUI that's five coded by 10 by 10 20 year olds in San Francisco who have billions in venture backing or would you rather work in like something that's free on your PC with just the raw LLMs without any bloat in between them?

And you can even like customize your terminal to look like these cool like like these cool Evangelion like interfaces. Like seriously, when I look at this like if you've ever watched this like all the UIs they show in the series are so are so cool. And it basically even makes me want to like this channel started with me just um even doing things like coding neural networks, fine-tuning a lot of models. But to be honest, lately it's been more of like a terminal appreciation channel because the things that inspire me a bit right now is just trying to make my terminal and my workflows look exactly like like how it looks in these scenes from Evangelion. And to be honest, isn't this the future that we want? Being able to code any free in in free software with cool visuals, having mastery over all these agents, having open source AI models running maybe in the future in your house in some kind of server. Well, at least that's the future I wish that that is going to happen. But I've already been talking a bit too much. I think some people just clicked on this video to get like an introduction or like an in-depth guide on how to work with tmux. So, yeah, let's basically get started with the with the real product. Um So, let's first maybe go to the website of tmux.

In short, what is what it can do is a terminal multiplexer which allows you to run multiple processes at the same time in your terminal and also disconnect from um detach from these sessions and then go back to them, which is a bit abstract. And I will basically go through my entire setup of tmux. I will start from scratch, go through my entire setup, show you how I use it with SSH setting SSH servers, and also show you at the end of the video I will show you an open code plugin that I made to basically really not to visualize through tmux which agents are done with their task, which in my opinion is a very good thing to have in your workflow. So, definitely stay tuned for that. But of course, the first thing you need to do is installation and I'm on macOS, so I just did brew install tmux, which if by now you should know brew. You're like, what are you even doing in the terminal macOS if you don't use brew?

So, when I'm in terminal um and I type which tmux, it should show brew.

Which version do I have?

Unknown option. Is it v?

Okay, that was not it. Like okay, after looking it up, it's tmux v. Okay, so I'm on tmux 3.6a. And this maybe shows you like tmux has a bit of a learning curve.

It does some weird things sometimes and I also see a lot of hate on it online sometimes. Like people are using Zellij a lot, but to be honest, once you get a bit used to tmux, it will work just fine. But it requires a bit of config to work well. So, I'm on tmux 3.6a. So now um let's get started. And the way to get started is yeah, you just are in your terminal and you basically launch a tmux server by typing tmux and then you will attach to that server in a session. So, you see what happened at the bottom at the bottom a a in a horizontal bar appeared. You see a first zero on the bottom left. That is basically my um my my session name because tmux it's organized a bit. At the top level you have sessions, then you have windows within those sessions, and then within those windows you have panes. So, by default um you basically have to do like a prefix to to basically target tmux and invoke command with it. And this feels a bit like playing an instrument or a piano because you have to really quickly press control B and then you can type a command which to it does something. For example, control B C I was not fast enough. Control B C it's um you see it launched a second window, window one. Control B C again, we are in window two now. You see it with a star.

I can for example open open code right here.

Then I can go back to my first window with control B one.

And you see I'm here. You can launch another open code instance right here. But the first thing before I show you anything else like control B, pressing that all the time is is extremely annoying in my opinion. I can't get used with on my keyboard and I'm also a bit struggling with it um right now. So, actually the first thing I'm going to do is I'm going to change this prefix. And um everything from tmux is basically controlled in um tmux.conf if I'm correct. Um and if this file doesn't exist, you should create it. Honestly, going to copy paste a bit because I don't want to um rewrite this all. But, the first thing I'm going to do is basically change my prefix from control B to control um space, especially because space is also my leader key in Neovim.

It works nicely together. So, um I'm going to um write that.

And then I'm going to go back to this um first window.

Um I'm going to uh close this. And what you then need to do is um load your uh tmux source file with um tmux source file and then the tmux config. Um and now I've basically once again gained my old um prefix, which makes it much um easier to move around. And I would honestly recommend changing this to something um you can hit a bit more reliably than control B. You basically already see what um tmux is used for. Like, the first level you can do is basically open a lot of um these these windows they are called. Um in one window, you might have, for example, um checked out um a a Neovim project. And to the left, you might have, for example, your open code also in that um Neovim project uh in that same project and then basically switch between them and do your coding like this. An alternative could probably be like using the Ghostty or your built-in terminal um tab functionality.

But, the thing with that like I basically need to work on a lot of different uh PCs myself. Um I'm working on Windows, for example, at my work. And on Windows, you don't, for example, have Ghostty. So, you need to learn all kind of new key binds or do a lot of different setup once again. But, tmux is supported on any um device, Linux, Windows, macOS. So, I can just take my tmux configuration, install it on there, and have the base at the exact same config on my Windows PC. But, there are a few cool things I will show which I'm sure that Ghostty or your terminal um uh emulator doesn't really offer um and tmux does. Okay. So, the second thing I need to do is basically fix my um fix my colors um a bit.

So, um the way you do this in tmux is just like um this. It just makes some color schemes appear more correctly. Um you probably can't really see it right now, but this is necessary to make your colors appear nicely because tmux is basically an extra layer in between your uh terminal uh emulator and the Neovim instance, for example. So, you need to um yeah, set a few uh envars to make sure that the uh colors are passed correctly. The next thing I often set up is I really want to keep my Vim key bindings and like all the um muscle memory I've built up. Um and I want to reapply it to tmux. I don't want to really like Vim users are pretty autistic, I got to say. Like, you want to learn one set of key binds which you spend a lot of time on and then you basically want to take them with you in every program. So, another thing I do is I basically wants to have the same way of splitting panes, uh splitting windows into panes as with Vim. So, I'm just going to bind the V to a uh vertical split and S to a horizontal split, which tmux also has some um built-in key binds for that, which I honestly um forgot. So, let's just set this up.

Um reload it.

And now let's show how these splits work. So, I can press my prefix, which is control space, and then V. And we have basically split our pane. We can also do it horizontally, vertically, or horizontally, and so on. And yeah, you can basically have a lot of different um sessions in them.

And yeah. So, one concept is windows, and then within these windows, you have panes. But, as you can see, um I'm kind of um stuck in this pane. So, how do you move uh back to your um previous pane?

Normally, you can do it with your um arrow keys. But, especially on my Happy Hacking Keyboard, pressing arrow keys is very inefficient and not nice. So, another thing I set up are basically key binds to navigate between them with Vim key bindings. And the way you do that is as follows.

So, um let's maybe in this window, let's once again um reload our source file. And now by just pressing H, J, K, L, I can move to the left, right, left, and so on, which is honestly much nicer. By the way, by pressing prefix X, um you can basically kill these panes.

It will then ask you, "Kill? Yes or no?" So, type yes. Um I'm going to leave um this split open or not. Another thing that's a bit annoying is that like, for example, if I go to um pane number uh pane to window number two, so my open code. And let's say um And by the way, the way I navigate to this, which I should have explained before, is you see that these all have an index from zero to three. And every time you create a window, um it gets the index next to the last one you add. But, the way you navigate between them is press your prefix. So, in my case, control space, and then you type number of the window you want to navigate to.

So, let's say I want to go to the open code pane, I press control space, and then two. But, let's say I now close this window.

And do you see what happens? Like, now there is like a gap between uh one and three, which is very annoying on your keyboard. Like, um it's probably not that annoying to that many people. But, to me, this is like definitely one of the worst things um that can happen during your during your day. So, we basically want to um auto renumber that.

So, I'm going to type automatically rename um windows.

So, um let's maybe now um navigate to them and um

Um create some new ones.

Um so, normally, when I go to one now, and I close it, the number two, like the Zsh, let's maybe echo something.

Hello. Um this one um should become window number one when I close this.

Okay. I'm as dumb as anyone.

Um I actually needed to load my source file first. So, let's now close it. And you can see um the pane number two uh the window number two has become window number one.

Another really important thing, at least to me, is to make sure windows start from index one because index zero is all the way to the right uh side of your um keyboard. And I basically want to um follow my keyboard layout, which starts with number one. So, I don't want a pane uh a window of index zero to appear at all. So, I also add that here. So, now um reload the source file. Um let's say I close this one. Um Yeah, maybe let's just um to show it to you, let's just um kill tmux um at all uh at all What do I say? Like, entirely?

So, you can do that Um the first option is to basically detach from the um tmux server. So, press prefix D.

Um detach from tmux. You can then with tmux ls see which uh sessions and windows you have open. But, then you can just say tmux um kill server. And now all the um there is no tmux server running. All the windows are killed. You open it again, and you can see I spawned with a window um that has index one instead of of zero. Also, another thing I have in my setup is that I want to make new windows start after the current one. So, basically, what this means is um I haven't applied this option yet. But, at the moment when I press uh I want a new window, so I press prefix C, it basically appears at the end. So, even when I'm on uh window two, if I create a new window, I'm currently at window six.

While actually, I wanted to appear next to window um two. So, I have this option. So, let's um reload my tmux.

And now when I create a new window, I should create a window three, and it should be I should be on it next to window two. So, yeah.

These are things like in my mind, these make a lot of sense, and tmux doesn't offer them out of the box, while in my opinion, it should. Another thing that makes a lot of uh sense in my mind is you can see at the moment all these windows are renamed to the process they have open. Like, for example, window one is Neovim, two is set as age. If I go to window three and type open code, you will see that it changes to open code.

But this is basically meaningless in my my daily life. I basically want to rename them to the directory I'm

Did I save this? No, I didn't save it.

Let's reload it. You will see everything is renamed to the directory I'm currently in. So if I go to for example watch fishing it changes to watch fishing. If I'm here in um What else do I have open like comfy UI LLM? You will see that my windows basically get renamed to whatever folder I'm in, which in my opinion is very nice. Another thing that's really essential when you work with terminals and I don't know if it's even possible with things like just using a raw terminal like ghost is basically a copy mode because often you have a process which prints a lot of text. You want to copy some things from it. For example, from your closed code output, you want to copy some text, paste it in your neovim. How do you do it? Like if normally you would reach for your mouse, start selecting stuff like that. But that in the terminal, every time I touch my mouse it basically makes me go out of my flow, out of my zen state. I just want to do it with my terminal. So I basically set up a copy mode and copy mode tmux has basically normal mode and um copy mode and in copy mode you can select text and copy it as you would expect. So what does this do?

I set up a couple of key binds. For example, V to select text like in in vim and then with control V I can also select a a a rectangle and with Y I can yank just like in neovim.

I also sync my clipboards with these yanks and I finally set up a a style option such that it basically the selection matches my Kanagawa theme, which is like the stylish person that I am of course. So let's reload it.

Let's maybe show it to you. For example, let's say I have a lot of text here. So now what did I what did I do? I pressed a prefix and then the left bracket.

And now I enter copy mode. I can basically copy text here from my vim com with my vim commands. I can with um What am I pressing? With with my base button, I can paste this text. But I can honestly really understand like you have like an um Let's say open code. You say um generate some random code.

And here let's open this.

I Been a while since I worked in this project. Let's say open code. Like you just asked for some advice. You can of course make modify your code. I also see like I haven't set up like a tree setter for Swift yet. So that's why you don't see any syntax highlighting. But um Now how do you copy this text? Like I just enter copy mode.

I basically want to copy this text from catches or rarity.

I have yanked it and now I can just paste it right here, which is honestly an essential flow in the terminal if you want to be really zoned in locked in in the terminal. Maybe before we go to the next option, another thing you can I do a lot is for example, you see like my name appearing in a lot of windows because that's basically what that directory is called. But during your day you get confused. You want to organize a bit what each window is and you can basically rename these windows by pressing prefix and then a comma. So now I can rename the window for example to tmux conf.

And so you basically don't forget what you're working on.

This one is for example some random work. And then you basically during your day you can switch between them as you work probably on a lot of projects at the same time. Another thing still on the window level, what you probably want or like I wanted was I wanted a way to basically swap these windows around. For example, now my tmux conf is on window one, but for example I might have some priority on some some other project which I wanted index one to really maybe order them according to priority or make matching projects appear closer together. So what I then do is basically set up a a key bind so that I can after pressing my tmux prefix and then I can basically swap windows with the left and right button. So let's show you how that works. Let's load my tmux source file.

Let's say I want to move this to the last index. So I press tmux config.

Come on. Why doesn't work?

I I didn't save it.

Come on boogie like this is an important tutorial. Millions are watching.

Tmux prefix and you can see by pressing the win by pressing left and right I can basically move it to wherever I want.

But yeah, if you're working on this yourself, a good way and also to practice it is probably to like open like split here.

Open your favorite your your favorite agent and basically ask him to code for you and you can easily swap between both panes by pressing prefix and then your neovim key binds or whatever you set up. And this is basically my preferred workflow. Sometimes I put it in a different window. Sometimes I create a different pane for it. And also a cool thing is by pressing control space like your tmux prefix and then Z, you can zoom in on a on a window and bring it to the foreground. For example, sometimes I don't want to see this cloud. So I press control space Z. It disappears. I can make it appear again with control space Z. Sometimes I just want to see the the cloud code window.

This especially becomes useful if you maybe have another um size cloud. You might have some open code window open with a different model and sometimes you want to view your open code, your cloud code, your code. Like honestly once you start doing this and then swapping around in different windows,

You can honestly see like you can't beat this with a with a gooey flow. Another thing that's useful is knowing how sessions work.

And sessions, for example, now I'm in session zero.

And a session is basically a collection of windows.

Why would you use sessions instead of windows? Well, after a while like for example, if you created like 10 windows, there aren't really any quick ways to go to them anymore. You can go to them by pressing the tmux prefix and to go to the next or um previous window, but you can't really go fast between them by pressing a number. So to be honest, after you run out of um 10 windows, there is no way to move fast between them anymore. And to honestly, I would just create a different session.

Um but besides that, sometimes you are doing so much different um work like let me close a few because it's honestly distracting me. Because sometimes you are doing some unrelated work. For example, you are coding on an AI pipeline in a project one and you you see a message on Teams like damn, I got to review someone's code got to review someone's code &gt;&gt; [laughter] &gt;&gt; [laughter] &gt;&gt; and I don't want to pollute my context here. Yeah, I basically what I do is I create a new session. And how do you do it? You press the the the the tmux prefix, then you press the colon, which allows you to type a command and then you can type new session S. And with S you can make a new. So you can for example type review Bob or something. And you can see um I'm now in it didn't completely type it.

It's types review Bob, but we will fix that later.

And you see I'm now in an entirely new session where I can create new panes, check out new projects and basically organize my work at that. But and how do you switch back?

First, you can have like tmux prefix and then like the semicolon.

So you can switch back.

That's a bit about tmux. It's kind of like playing a piano. Like you have to really get used to the keystrokes. But another way to get to that session again is by pressing tmux prefix W. And now you can basically see whatever you have open. And you also sometimes use that.

So let's maybe go back to our own own session. Um or and then team press the tmux prefix W and maybe just kill this bob session. You do that by pressing X.

It says kill session review bob. Yes.

So now we are back to our old um workflow. Another thing that's very important is of course style because you're here looking at this during 8 hours a day. So you don't want to look at some some something that annoys you. So let's customize our um team works session horizontal bar a bit so we can basically see a lot more and it fits more nicely with the theme we are using. The first thing I basically instruct is to like um make the team works bar always appear in the bottom left and make the entire background transparent so that my Kanagawa theme um comes through. So um let's do that. Team works a source file team works config and this already looks a bit nicer. We get rid of this ugly um bar and we can more um clearly see what we are doing. What I then do is um do this and we are basically going to style our window um or or uh session indicator a bit.

We will also for example remove the right status. For example it now says votes MacBook Pro and then the time and hour of day but I can already see that in the top of my Mac OS um toolbar so I don't need that so I'm just going to throw that all away. You can also see um the um session indicator like the zero is now in my Kanagawa theme already which is nice. We are also going to change like the um pane borders. For example there's no the screen border you can see uh swapping when I go to the left or the right um pane but I basically want this to appear in my Kanagawa theme style so let's rename uh the reload this so you guys can see it's now purple. And now um let's also apply some styling to the actual um window indicators.

Um basically I'm going to structure it as follows which you will quickly see.

Um so now you can see that this is basically a lot a lot cleaner. We can clearly see which uh window we are currently on um by the purple um and the

Like um doesn't this this look good?

So clean. So much better than um cursor.

And I think my first um more advanced thing I'm going to show is like um how I do um nested team works sessions um because basically let's say you are um like I have like a Mac mini server on which I want to run agents 24 hours a day. So how I go to this agent is basically go to um by the way if you want to see a bit more things about this agent I have an open claw um video uh uploaded on my uh channel as well so definitely check it out. So I SSH to it by SSH spooky. I am once again um on a this time on a Mac mini.

But here um I sometimes want to run a lot of um agents once again in parallel. So basically I basically want another team work team works session here which allows me to basically first off run agents in parallel but also allows me to detach from um from the agents because let's for example uh say I run open code here on my server. I basically um also gets um do a lot of work code me million dollar app and fast um like it's it's doing a lot of work. It will need to think a lot about this.

So um I'm basically saying keep doing work but how do you know close this and basically let it run 24 hours a day like as soon as I um close this connection I um I I basically need to um yeah first off cancel this process. So a thing you can do is um and what team works is also often meant for is basically run a team works server once again but

And here you can see there are some a bit of problems appearing because we basically have two nested um team works sessions on top. So now when I press my prefix and then see in which session in which window um in which uh team works session do I want a new window? Do I want it on my server or do I want it local? And team works itself is also confused by this.

So with the um with with the command I added right here let's maybe um reload it. Um What I can basically do is press F12 and basically disable the team works um that runs on my local PC. Only have the team works on the server and then I can keep opening um sessions right there. And the useful thing of this is basically that I can do like um do some busy work make me rich um have a session right here. And what I can basically do is like a team works detach. So like um team works prefix D.

Detached on the uh from the team works that was running on my SSH connection. I can then safely do um exit to return to my um PC toggle back my um my team works server on on my own PC.

And now I can basically close my uh laptop let my agent work on my server.

Once I wake up again I hope uh open claw or open code has made me very rich. So I do SSH spooky. I connect to my team works session again with team works attach. So attach to the running server.

Um you can see the all my uh windows are once again there and I can go to open code and see what it has done. And this is honestly like I really would I really don't know how you would do workflows like this like attach and detach from remotely running processes without team works. So besides window splitting and navigation this is also like one of the most um important use cases um I think.

And the video has already probably gone a bit on a bit too long but I really want to show you this um open clo- open code and uh cloud plugin I recently um made because often like you have a lot of um open code instances running on a lot of different um on a lot of different uh windows but you're probably working for example on another window while you have these working in the background. But how do you know when these other agents are done and when you need to check up on them? And I basically wanted them to give some kind of notification in the team works bar to basically signal to me I'm done please check on me whenever you have time. And the way to do that in team works is basically to um turn on bell mode which will basically monitor if a bell command is sent in another window besides the one you have currently open. And if a bell is sent I basically want to underscore um that session um indicator.

Pretty abstract but how does it work?

For example let's say I am on a new window um let's say um I sleep for 3 seconds and then print the bell command which is this slash A.

So um let's do that and then quickly switch to a new window.

So um let's say I'm working right here and see after 3 seconds um on window two I get a highlight like that something has happened there. So I switch to it.

And I will basically inspect the output.

Um and the notification also goes away.

So I basically wanted open code and cloud code to print this slash A whenever it was done. And you can do this with open code plugins and with cloud code hooks. So um um open code um let's go to it.

No it's in config CD open code.

The way you do this is basically make a a plugin which you can do in JavaScript.

Um and what I basically want to do is whenever um a a a open code agent is uh done running I want them to print like um the slash A commands to the current uh shell which you can invoke by doing something with this dollar command. So I basically print the um slash A in any new uh shell.

So let's say I do some I I invoke uh open code right here like who is the best YouTuber of them all?

And I And in the meantime you don't want to stare at your screen. You want to do something else. And you can see open code is done and I can see it because it has clearly notified it in my um tmux bar. I switch to it and what does it say? Mr. Beast. Who would have thought Mr. Beast is the best top YouTuber overall um in gaming Markiplier is of course also very good very good YouTubers. I definitely watch them a lot. Please go watch PewDiePie and Markiplier.

But anyways, this is this is really cool um because I often spawn a lot of agents at once. Um let's see in cloud codes, it was a bit different but I also set this up. I think cloud is just managed like this and then you have this settings.json and you can see right here I've also set up a hook with cloud.

I can honestly say cloud is a bit worse um it's it's not as nicely coded because for example um I will show you like this like um return zero immediately um and then switch to another window.

You would think like of course cloud is is already done with this command so I don't have received a notification yet and that's because for some reason um probably five coded in some way they wait for 60 seconds after the um agent is done to basically invoke your hook. I don't know why they do this. It's one of the reasons I personally like open code much better and I pray to the gods every day that I can get uh that I can use my uh cloud subscription and open codes because the models are so good but just the cloud codes the way it's coded is like yeah it's pretty clear it's five coded. Think there were some security issues that were recently leaked. Yeah, I'm just trying to talk a bit until cloud will finally um send me his command. Is he done? Is he not done?

Um I'm kind of tempted to switch to the window just to check out okay it has finally notified me after 1 minute that it's done. That's basically everything I think I need to show you about tmux.

Like always I think these videos are like 10 minutes but in the end they take me like I think it's like half an hour right now. So I hope you enjoyed the video. All the configs are in the description and definitely leave me in the comments down below what you are doing in your new five coding or five coding I I would say AI engineering setup. Are you also diving deep in the terminal as me or do you still think that you would rather use cursor? So I hope you enjoyed it and I see you in the
