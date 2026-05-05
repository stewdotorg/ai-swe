# Your local LLM is 10x slower than it should be

| | |
|---|---
| Creator | Alex Ziskind
| Published | 2026-01-31
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=L9QZ97y9Exg](https://www.youtube.com/watch?v=L9QZ97y9Exg)
| Generated | 2026-05-04

## Description

Here’s the one change that took mine from ~120 tok/s to 1,200+ without a new GPU.
TryHackMe just launched Cyber Security 101 (SEC1) — and for a limited time you can get 40% off with my link: https://tryhackme.com/alexsec1
Use code ALEXSEC1 for 40% off the exam fee + 3 months of Premium access.
🛒 Gear Links 🛒
🪛🪛Highly rated precision driver kit: https://amzn.to/4fkMVfg
💻☕ Favorite 15" display with magnet: https://amzn.to/3zD1DhQ
🎧⚡ Great 40Gbps T4 enclosure: https://amzn.to/3JNwBGW
🛠️🚀 My nvme ssd: https://amzn.to/3YLEySo
📦🎮 My gear: https://www.amazon.com/shop/alexziskind
🎥 Related Videos 🎥
🧳🧰 Mini PC portable setup - https://youtu.be/4RYmsrarOSw
🍎💻 Dev setup on Mac - https://youtu.be/KiKUN4i1SeU
💸🧠 Cheap mini runs a 70B LLM 🤯 - https://youtu.be/xyKEQjUzfAk
🧪🔥 RAM torture test on Mac - https://youtu.be/l3zIwPgan7M
🍏⚡ FREE Local LLMs on Apple Silicon | FAST! - https://youtu.be/bp2eev21Qfo
🧠📉 REALITY vs Apple’s Memory Claims | vs RTX4090m - https://youtu.be/fdvzQAWXU7A
🧬🐍 Set up Conda - https://youtu.be/2Acht_5_HTo
⚡💥 Thunderbolt 5 BREAKS Apple’s Upcharge - https://youtu.be/nHqrvxcRc7o
🧠🚀 INSANE Machine Learning on Neural Engine - https://youtu.be/Y2FOUg_jo7k
🧱🖥️ Mac Mini Cluster - https://youtu.be/GBR6pHZ68Ho
* 🛠️ Developer productivity Playlist - https://www.youtube.com/playlist?list=PLPwbI_iIX3aQCRdFGM7j4TY_7STfv2aXX
🔗 AI for Coding Playlist: 📚 - https://www.youtube.com/playlist?list=PLPwbI_iIX3aSlUmRtYPfbQHt4n0YaX0qw
GitHub repo for this code: https://github.com/alexziskind1/llama-throughput-lab
— — — — — — — — —
❤️ SUBSCRIBE TO MY YOUTUBE CHANNEL 📺
Click here to subscribe: https://www.youtube.com/@AZisk?sub_confirmation=1
— — — — — — — — —
Join this channel to get access to perks:
https://www.youtube.com/channel/UCajiMK_CY9icRhLepS8_3ug/join
— — — — — — — — —
📱 ALEX on X: https://x.com/digitalix
#macstudio     #gdxspark    #llamacpp

> Transcript extracted from YouTube auto-generated captions (`L9QZ97y9Exg.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

You're probably familiar with Olama, right? This is what it looks like. You can launch it. You can talk to it. Never mind that we're using Quen 34B. That's just standardized thing that I'm [music] doing. And the prompt doesn't matter what it is. 100 tokens per second is what Olama gives us. Lama runs on Llama CPP. It has a little bit of overhead, but you can also run Llama CPP directly and query it remotely, which is what software developers are going to be interested in if they're going to be using it as a code assistant, for example, or with agents. Hold on a minute. And that's going to be key here in a moment. I'm going to start up Llama Server pointing to the same model by the way. And Llama Server has this nice user interface that we can also say hello to.

And this one gives us tokens per second.

124. So a little bit better. So that's chat. We're chatting with one model, one request at a time. And it's not even how code assistants [music] work because code assistance requires a lot of different chats going on at the same time. What if I query this remotely here? I wrote a little script.

Basically, this is from my race to 1 million episode, but this one would do a thousand. And I'm going to be communicating from my laptop here to my Mac Studio that's running Llama Server.

Boom. And there it goes. And now I'm twiddling my thumbs. Boom. Boom. Boom.

Look at that. 120 tokens per second.

Very close numbers, right? You know who you don't want to twiddle their thumbs?

Your agents that are working away at your code base. Sometimes tens of them or hundreds of them. And we're even going to increase our target tokens to 1,000 so we have a little bit of room to walk around. Boom. I'm going to watch this in real time. Check this out. This is going to be insane. [laughter] What? 826 tokens per second from Llama CPP. And you might think, "Oh, okay.

&gt;&gt; I've seen this trick before, right, &gt;&gt; on my channel probably." Uh, concurrency is set to 128. Well, I'm about to show you a new trick because even with concurrency 128, without that trick, you still get 231 tokens per second, which is still better, but we're limited to what Llama CPP is capable of just by itself. It can't handle as many concurrent connections as something like VLM, for example. And I've got other videos to show that. So, what am I doing? Well, I wrote this little launcher. This is actually inspired by Donato Capitella's distributed launcher.

I recently featured that in a video about this thing. Separate video. You can watch that if you want. But here we're just running this on one machine.

So I created this launcher which allows you to tweak a few knobs that Llama CPP provides for tweaking along with sweeping and exploring what your machine is capable of. For example, I have this running on the Mac Studio, but what if you have a Mac Mini for example? Well, you can run this on there. If you have a Windows laptop or another Linux laptop or an Nvidia one, it doesn't matter because Llama CPP will run on all those.

And this launcher is going to run on any of those as well because it's just Python. And it'll find the best combination for you to run Llama CPP as a server while twisting those knobs. I probably shouldn't say that on YouTube.

Um, somebody might complain.

&gt;&gt; FBI, open up.

Some sensitive soul out there. Let me show you how this works. First, we've got some tests. We can do a single request. Select your model, select where you're running it, and run. And now, this will just query Llama CPP, get your tokens per second for that configuration, which is 127, very close to what we've seen. You can do concurrent requests. There we go.

Concurrent requests, count 128, concurrency 128, total tokens, average request tokens per second, and then throughput. So, we got a throughput 240 tokens per second on the same model. By the way, all this stuff is documented in the repository. If you're getting into cyber right now, there's an awkward gap between I watch the videos and I can handle real situations. And the gap is exactly what this is for. This is try hackme where you learn cyber by doing it. Interactive labs real scenarios all browserbased with a global community of over 6 million people. They just launched an entry-level search called cyber security 101 also known as sec 1.

Quick tour only. No exam spoilers. The big idea here is applied fundamentals.

You train hands-on, then sew one checks you can actually perform the basics in realistic situations, not just memorized definitions. This is for students going for their first cyber role, career switchers, and interns or junior folks like 0 to2 years who want proof they can actually do the fundamentals. High level, it touches what you'll actually run into. operating systems, networking, web-based security basics, a little bit of blue team fundamentals, a little bit of red team fundamentals, and malware analysis at a beginner level. And this is what makes it different. A lot of beginner searchs feel like vocabulary quizzes. SEC 1 is trying to be a hands-on checkpoint instead. When you finish, you get an instant result, a digital certificate, and a sharable badge. If you're getting started in cyber security and want a hands-on way to prove your fundamentals, check out Try HackMe CyberC 101 certification using the link below. And if you use my code for launch, nonpremium users get 40% off their certification plus 3 months of free premium subscription link below. All this stuff is documented in the repository. It's called Llama Throughput Lab. There's other tests in here, but there's one really interesting one and it's called Full Sweep. This takes a while. There's 308 different combinations of how many instances of Llama server you start. That's the key here. Hint hint. So, you run multiple instances of Llama server to be able to reply because a machine that's as powerful as this has a lot of memory these days. This one has 64 gigs of VRAM or it's called unified memory in Apple silicon terms. This Mac Studio has 512.

But that's not the limiting factor when it comes to running these. The limiting factor here is the compute on the GPU.

We're running all this on the GPU because we don't want to do it on the CPU. It's going to be extra slow. That's why we're using the GPU here. And you can only do so much when you're running multiple llama servers at the same time.

You're no longer limited by memory, obviously, unless [music] you're running huge models, which you can do here. You can run on a 512 GB machine. You can run like four llama 70 billion parameter models at the same time in four instances of llama server. So those are instances. Then parallel llama server exposes a parameter called parallel. And this has been documented pretty well on the GitHub repository under Llama CPP by the creator of Llama CPP himself, Georgio Ganov. Another knob you can twist.

All right, maybe I should stop it with that. Um, somebody's gonna get mad. I'm not twisting any knob. Concurrency is another uh parameter you can alter and that's what I showed before in my videos where we can send multiple requests to Llama server. Why do we care about that?

Well, if you're just doing chat, then by all means go ahead and chat and wait for it to finish. But a lot of times we want to go beyond that. Imagine a folder with thousands of photographs that you want to analyze or a video with millions of frames. You don't want to be sitting there and waiting for one frame at a time to be analyzed, do you? Or imagine another scenario where you have a bunch of agents orchestrated together. Some are doing planning, some are doing implementation. You want them working at the same time. So that's where concurrency comes in. Getting back to the sweep, this will do a full sweep. So instances, parallel, and concurrencies, and it'll find out where the optimal spot is on your computer, whatever computer you're using. You can select that test sweeps, and then click on run selected test, and off it goes. There it goes. Uh, just keep in mind that this will take a while. Also, on a slower system, it'll take a real long while. So right now we're on four out of 308 different combinations. But as it's sweeping, you can see the numbers coming in. 128 tokens per second, 193, 263, 314. So you can see how these different parameters are affecting that output. If I go to my results folder here, I did a full sweep on this Mac Studio, of course. Now, I should probably create a nice looking chart, but this is also on GitHub and open source, so feel free to muck with it if you want to. Look at these numbers. 1,226 tokens per second.

That's with 16 instances. In other words, I started Llama server 16 times.

Parallel flag set to 64. 1,024 concurrency. Now, keep in mind this is just a benchmark. If you want to do real world cases, this will only give you some guidance and point you in the right direction, but always make sure you actually test your real world scenarios.

And finally, let's discuss how I actually run this. Well, I run it by using this command right here. It's an executable Python command. Run llama tests. Boom. Not very creative, I know.

And here you can select the test, select the model. Boom. And there's one more thing, which is this number six here.

It's number six for now, but it's probably going to change. Configure and run round robin. Well, this is the thing that's actually going to start up multiple servers for you. You can do it manually, of course, but that's going to be not fun. Uh, so here you can configure how many instances you want to start. Let's say I want to start up 16.

Parallelism, that's llama servers parameter. This is going to be the starting port of the first llama server.

And that is going to plus one for each one of the servers you start up. So this is going to go up to 9,15. And then finally, this is the key right here, Engine X. This is the thing you should not be afraid of. I was for a long time, but this is really cool because it's a little server that sits in front of your llama servers that you talk to. It's just pass through basically, but it has the nice little capability of doing roundroin, which means imagine you have llama servers running on each one of these things. The first request comes in, it goes here. The second request comes in, send it here, and so on. And it's going to round robin these requests so that they're not all handled by one server. So, let's set that up. I like to use 8,000 because it's easy. Now, host, this is local host if you're running locally, but you can set it to 000 if you're going to be quering this remotely. And then you have start round robin servers and stop roundroin servers. Let's try that. Now, here's Mac Studio and there's activity monitor. and you'll see 16 happy little llama servers running there waiting to handle requests. Now, you might be familiar with Open Web UI. I actually made a tutorial, a whole tutorial setting this up with Olama at the time, but Open Web UI can work with a bunch of different servers. So, I pointed it at my EngineX endpoint, which is basically the IP address and port 8000 like we configured it. Let's do a new chat. Hello. Boom.

And there it is. It's responding. Now, I didn't exactly configure this to handle long stories, which is why a short prompt works like say hello because it doesn't have to think for a long time.

But the thinking is actually part of inference. It's actually generating tokens while it's thinking. So, that's using up tokens. And I set a maximum of tokens that need to be generated, which you can adjust if you need to. What's the capital of France? Say hi to me in Espanol. And they're doing this pretty much at the same time. So, go on, go check this out. Play with Llama Throughput Lab and see what you think.

let me know. You can open issues, do pull requests, and so on. If you like this video, you're probably going to like the video I did about this framework cluster. That's going to be right over here. Thanks for watching,
