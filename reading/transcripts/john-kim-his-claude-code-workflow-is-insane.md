# His Claude Code Workflow Is Insane

| | |
|---|---
| Creator | John Kim
| Published | 2026-02-21
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=WpQZlKiy3zo](https://www.youtube.com/watch?v=WpQZlKiy3zo)
| Generated | 2026-05-04

## Description

I met Boris Cherny, the creator of Claude Code, and asked him about his personal setup. He told me he'd posted about it publicly. So I went through every tip in that thread, showed what it actually looks like in practice, and shared my own thoughts on what's worth stealing.
Check out my newsletter where I write about AI and give away some of my personal Claude Code workflows: https://getpushtoprod.substack.com/
📽 Videos You Might Ike
How I use Claude Code (Meta Staff Engineer Tips) https://youtu.be/mZzhfPle9QU?si=vT3htDZl8wkp3bBy
Claude Code Workflows That Will 10x Your Productivity
https://youtu.be/yZvDo_n12ns?si=0KaQMKmaQD5BykC-
⏱️ TIMESTAMPS
0:00 - Who is Boris Cherny and why this matters
0:19 - Boris's philosophy: "My setup might be surprisingly vanilla"
1:31 - Tip 1: Run 5 Claude instances in parallel
3:10 - Tip 2: 5-10 web Claudes + the /teleport feature
5:00 - Tip 3: Opus with extended thinking, always
5:54 - Tip 4: Share your CLAUDE.md across the whole team
7:08 - Tip 5: Add Claude to your code reviews (GitHub Action)
8:06 - Tip 6: Plan mode first, then auto-accept
9:52 - Tip 7: Slash commands for every inner loop workflow
12:00 - Tip 8: Custom subagents (Code Simplifier, Verify App)
14:00 - Tip 9: Post tool use hooks for auto-formatting
14:30 - Tip 10: /permissions instead of --dangerously-skip
15:44 - Tip 11: MCP for Slack, BigQuery, Sentry
17:14 - Tip 12: Long running tasks and background agents
19:14 - Tip 13: Give Claude a feedback loop (2-3x quality)
📲 CONNECT WITH ME
► LinkedIn → https://www.linkedin.com/in/jonnykvids
► Threads →  https://www.threads.net/@jonnykvids
Need a study buddy? Check out my free app Anime Pomodoro App
https://apps.apple.com/us/app/anime-pomodoro/id6749815827
👍 Like, Subscribe & Hit the Bell for more software engineering insights!
#ai #claudecode #claude

> Transcript extracted from YouTube auto-generated captions (`WpQZlKiy3zo.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Well, hello there. So, back in January, the creator of Claude Code, Boris Chenry, he actually posted about his entire workflow. And I actually had a chance to meet with him and ask him a bunch of questions. And I personally found his personal workflow very enlightening. And I've been trying to replicate a lot of his workflows directly into mine. So, today I thought it would be fun to go over this chain of thread posts that he made and also kind of show you physically what it actually looks like in practice. Now, I have a few local projects. They're nothing really in terms of complexity as my projects for work, but I thought it would actually work pretty well to kind of showcase some of these and kind of like interact with some of them right here. So, as usual, I created a little slide deck. So, let's get right into it.

So, this is Boris Henry. For each of these, I'm just going to read it out really quick to show you each of the tips. Hi, I'm Boris and I created Cloud Code. Lots of people have asked how I use Cloud Code. So, I wanted to show off my setup a bit. My setup might be surprisingly vanilla. Cloud code works great out of the box. So, I personally don't customize it much. There is no one correct way to use cloud code. We intentionally build it in a way that you can use it, customize it, and hack it however you like. Each person on claw code team uses it very differently. So, here goes. I really like how he's set up here that claw code can be customized. I personally have been customizing the hell out of it and at some point I'll probably share my own personal setup as well, but I really like how it starts out here. So, let's get to the first tip. The first tip is around running clouds in parallel. I run five clouds in parallel in my terminal. I numbered my tabs one through five and use system notifications to know when a cloud needs input. So, look at this. I will open up my terminal.

All right. So it looks like what he does is he creates a claude like this and then he has one that is just for like just a terminal for bash. So it looks like this is his setup and then he has multiple tabs like this and then he probably replicates this claw tab system for all all of them. Right now I actually work very similarly like this.

I'll have one instance here, one instance there. I'm using like hotkeys for it terms is command 2, command 3, command 4 to switch between and then you could switch in between the tabs by doing command left bracket or right bracket. Now one additional tip here that I like to do to keep things like in my mental context is to like rename it.

So this could be like local one um or maybe the project that I'm working on local this is push to prod app for example. And here maybe instead of this uh I I work on my anime pomodoro app for example. And because I did that I would name this local anime ma pomodoro. Now you could imagine that you would have like ssh or some kind of on demand stuff as well. And this is exactly what I do at work. So that's tip number one. It also looks like he uses light mode. I am not going to switch my dark mode to light mode. Okay. Tip number two is 5 to 10 web clots. So I also run 5 to 10 clots on claw.ai/code in parallel with my local clots. As I code in my terminal, I will often hand off local sessions to web using and or manually kick off sessions in Chrome.

And sometimes I will teleport back and forth. I also start a few sessions from my phone from the Claude iOS app every morning and throughout the day and check in on them later. So this is claw.ai.

And I actually haven't done the teleport feature, but let's try the teleport. I don't know if it will work, but for my push to prod app, let's work on some of the printify integrations. Give me a plan. So, it looks like it's whatever sessions are on here. It just maintains here. But let me see if I can do a slash teleport. I'm just going to try can you can you push to the web teleport? I'm just going to try this.

Okay, this is taking a long time. I'm just going to cancel this and then uh create to-dos for printed by integrations. Okay, so it looks like it's creating a backend task. Look at this right here. So you just hit N like this and then um it creates a session and then you can basically open the session like so and it's going right here. That's really cool. I actually haven't been using this but I can imagine doing this especially when I'm like ready to go off for the day or something like that.

And it's synced to your phone. If you look at this it's synced. So and so you can start new sessions here. So I actually didn't know about this but this is actually mind-blowing to me.

That's awesome. I'm going to start using this all the time. Okay, next tip. Tip number three is Opus with thinking always. I use Opus 4.5 with thinking for everything. It's the best coding model I've ever used. And even though it's bigger and slower than Snonnet since you have to steer it less and it's better at tool use, it's almost always faster than using a smaller model in the end. Now, he posted this before Opus 4.6 six came out. So I wonder what he's thinking about it now because Opus is like known to be a lot slower. So one thing you can do is you can switch though like this using /mod and I actually also just default to 4.6. They got rid of ultra think which used to have this little like animation thing but and if you have like just multiple like sessions of cloud code going then it doesn't matter if it's that slow in my opinion. All right, tip number four is share your cloud.md. One cloud.mmd for the whole team checked into the git update multiple times a week. Okay, our team shares a single claw.md for the cloud code repo. We check it into git and the whole team contributes multiple times a week. Any anytime we see claude do it do something incorrectly, we add it to the cloud. MD. So claude knows not to do it next. Our team maintains their own cloud.MDS. It's it is each team's job to keep theirs up to date. Now, I actually asked Boris what his personal cloud MD is and he said there's basically like two lines and it just points to um using the claw.md and so so the whole idea is that they're doing compound engineering and then context engineering right within the codebase. I have a video coming up around this on agentic engineering but but yeah it's very simple. I also am doing this now where for example um like this pomodoro app I have the claw.md directly here and it's kind of like things about my codebase for iOS the navigation flow the design patterns and things like that a lot of this stuff you can just create using slashinit but yeah like this is like very straightforward stuff all right tip number five is add claude in code reviews so during code review I will often tag add claude on my co-workers PRs and add something to cloud.mmd as part of the PR. We use claude code GitHub action/install GitHub action for this. It's our version of at Dan Shippers compound engineering now.

So I actually kind of set this up so you can see that like right there Claude's there in this workflow. Um let me just try making a quick PR. write me a quick PR on some of these to-dos and push it as a separate branch. So the whole idea is that you should theoretically once this pushes do an at and then do a review. Now obviously this is like not a good example that I'm doing because it's just to-dos but theoretically the the whole idea is you develop a system so that it automatically uses cloud to

Tip number six is plan mode first. Most sessions start in plan mode. Shift tap twice. If my goal is to write a pull request, I use plan mode and go back and forth with Claude until I like his plan.

From there, I switch into auto accept edits mode and Claude can usually oneshot it. A good plan is really important. So, I I also do plan mode all the time as well. So, for example, here I want to create a new live activity for my anime Pomodoro app where it keeps tracks of the timer in my like live activity and also dynamic island. All right, there's something definitely wrong with this UI here, but it's going.

So, I I I really like plan mode um because it's a way to really analyze and deep dive into your codebase before you actually make any changes. Someone asked me recently, why do you even have multiple cloud instances? What are you doing? A lot of times if I'm not using git work trees or like some way to do multiple coding executions, I'm actually doing a lot of deep dives using plan mode for like different parts of the codebase. So maybe I have like some kind of performance optimization project I want to do on media viewer. Maybe I want to do look into app init or app start.

Whatever I'm trying to investigate, I actually use plan mode. And also for like investigating SVS or investigating like random bugs, I'll also start with plan mode to start investigating. And plan mode is essentially a way for you to create prompts, like gigantic prompts, like really good prompts. And I talk about context engineering and things like that, but plan mode is where you do the orchestration to be able to bring all the context that you need into that cloud instance so that the agent can execute correctly. All right, tip number seven, slashcomands for everything. I use slashcomands for every inner loop workflow that I do many times a day. This saves me from repeated prompting and makes it so Claude can use these workflows, too. Commands are checked into the git and live include/comands.

For example, Claude and I use a

command every day. The command uses inline bash to premputee git status to make the command run quickly and avoid back and forth with the model. Okay, so if you ever wonder how you want to make a slash command, it's it's very very easy to do. So let's just go here and say I want to create a commit- push-r skill. And this skill needs to format my code and then add everything that I just did in the session and then commit the code with a good commit message. I always want to push it as a PR rather than directly as main so that I can review the code online. Something like that. And these days actually slash commands and skills are basically the same. So Claude will usually create the same thing. Now, you know, the thing about making these videos is that it just takes a long time because Claude takes a long time to run. I probably should have ran this on Sonnet, but it's okay. Opus is fine. But you can see here that it's exploring the skills first so that it's not remaking the same thing.

And I love the fact that Claw Code and Codeex, they ask questions rather than just assuming. And I think this is like a big change between like I I don't know. I haven't used cursor in a bit.

Maybe I should give it a try. But I remember it it just like kind of did stuff without and always just made assumptions which and it always turned out like not great. All right, it decided to name it / ship which is fine to me as well. But yeah, that's how you create skills. It's very easy. Remember all you need to do to make skills or slashcomands or sub aents or install MCPs is just leverage cloud code to ask you to do it. All right, tip number eight from Boris is custom sub agents. I use a few sub aents regularly. Code simplifier simplifies the code after claude is done working. Verify app has detailed instructions for testing cla code end to end and so on. Similar to slash commands, I think sub aents is automating the most common workflows that I do for most PRs. Now, one thing about sub aents I want to say is that it's really to protect the context. Sub agents are really useful if you want to do like side effects or if you want to just have the result of that like cloud instance running. You don't really care how it got there, but you just want the output. That's really good for sub aents. There's a new thing just that just came out with Opus 4.6 called agent swarm or agent teams. And I'll probably do a video on this, but it's the idea of sub agents that have actually different roles and they share context with each other and there's like a master orchestrator. I think I in my other video I showed what a sub agent is, but I should have some sub agents here.

Agents you can see here. I actually have I created the same ones that uh Boris made cuz I I I thought it was very interesting. But yeah, so I could just say right here in another instance, can you run my code architecture sub agent to verify the integrity of my build or integrity of my app? I don't know about you guys, but I run out of RAM all the time at work. I think one, my codebase is extremely large and then I I also have like five or so cloud instances running all the time. Um, I had to give up running Android Studio completely or like I I just have my terminal up. I don't even have Chrome up sometimes when I'm running out of memory. So yeah, [laughter] so you can see that it's running two agents here. Let's do control O to expand it. and just see what it's doing.

Yeah, code architecture is reviewing. I don't know. All of this stuff is so cool to me. [laughter] Okay, so while that's all going, let's go back. All right, post tool use hooks.

I don't know why it's hard to say, but we use a post tool use hook to format cloud codes. Cloud usually generates well formatted code out of the box, and the hook handles the last 10% to avoid formatting errors in CI later. This one, there's not much to show. You can create one just saying post tool use and it's very similar to like pre-commit hooks.

It's it's it's nothing new here.

All right. Slash permissions and not dangerously skip. Number 10. I don't use dangerously skip permissions. Instead, I use slash permissions to preallow common bash commands that I know are safe in my environment to avoid unnecessary permission prompts. Most of these are checked intocloud/ settings.json and shared with the team.

This is what I talk about like in compound engineering where this you need to like start putting things like this into the codebase and share with the team. A a team needs to be like AI native first and everyone has to kind of start agreeing on some of the these like ways that we need to work and that's how you really move quickly. But yeah, so quickly showing you guys you can do slashpermissions here and then yeah slashpermissions. There's a bunch of things here and I I will just say you can update this manually, right? Can you update my permissions list so that all bash scripts are okay to run?

For example, you could you could do that. Like I keep saying this, but don't manage your clock code instance manually. You know, just have clock code do it. And then after you do this, you could always push all of this directly into the codebase. You see, it's uh change trying to change the settings right now. All right. While that's going, tip number 11. MCP for all tools.

Slack, Bitquery, Century. Claude uses your tools, not your codebase. 11. Cloud Code uses all my tools for me. It often searches and posts to Slack via the MCP server. Run big queries questions to answer analytic questions using a BigQuery CLI, grabs error logs from Sentry, and Slack MCP configurations, is checked into our MCP JSON, and shared with Teams. Now, I I always say be careful with MCPS because they're known to blow up your uh like context window.

Only leverage it when you need it for very specific things. If you see what these are, these are very like outside of the norm of regular coding and even like Slack, right? like this is so we we have this integration similar to our internal communication channel and I use this as a with a combination of our like PR reviewing thing where I'll just pull all of my context from the PRs read the diffs that I wrote this week and then I'll have it summarize that and then like send status reports to like specific threads that I want. So using cloud code to manage your work besides just coding is like a whole like big thing. Now, there's not much to show here, but for MCPS, you should always just kind of be careful because I I think like MCP is also another place where prompt injection is going to start happening. Same thing with downloading plugins and stuff like that. I think just be careful when you're doing these and just always have cloud code. Review the MCP before you install it. Okay, for number 12, longunning tasks. For very long running tasks, I will either prompt Claude to verify his work with the background agent when it's done. Use agent stop hook to do that more deterministic or use Ralph Wigan plug-in. I will also use either permission mode equals don't ask or dangerously skip in sandbox to avoid permission prompts for the sessions so Claude isn't blocked on me. So now I don't really use dangerously skip too often or even Ralph again. I tried Ralph Wigan. Ralph is this, right? So this is the Ralph Wigan loop. Basically, you just have a task, you have the agent, you have the file system, and then it just keeps looping until it's finished.

I don't really like this because I feel like there's it's too I don't know.

there's just so much uh work that needs to happen for you to have like a spec that's like so perfect that you can just have something like Ralph build the whole thing. But honestly, for side projects and things like that, I might just spec the whole thing out and I don't really care too much. And it's it's okay to let it run uh overnight or whatever. And and it's okay. And the whole idea is like you do it a few times maybe and then get a different result every single time.

and and then you kind of test out if it's good and then you can throw it away and then start engineering. I think there are times when it's actually pretty good. I think the hype around it has died quite a bit. But yeah, like if if you want to use it, I'm not going to show how to use it here, but you just install the plugin that I showed you earlier and then just start installing it. But to do dangerously skip permissions, it's very easy to do that.

So for example, when you're here, um let's exit here. When you do claude, you can do you just do claude dangerously skip permissions like that. And then now when you ask claw to run something, it won't ask you any permissions. Basically, it's it's okay. 13. A final tip, probably the most important thing to get good results out of Claude code. Give Claude a way to verify his work. If Claude has that feedback loop, it will three two to 3x the quality of final results. Claw tests every change I land in claw.ai/code using the Chrome extensions. It opens the browsers, tests the UI, and iterates until it works. So, let's give this a try. Can you run an end toend integration test navigating the website from the homepage to the detail page?

When you click on a poster, make sure the title is the same. Okay, so it just launched the website. I'm going to I'm going to try to show it. Okay, there you go. Check that out. Okay, so it navigated to the main website. All right, so now it went into the detail page and it's clicking on stuff. You can see that it's like taking screenshots and stuff. It's very cool. And this is kind of this agentic validation thing that I'm talking about in one of my other videos. And look, it passes. It like tells you what it did. Very, very cool.

And this is like a different form of N2N, right? It's not it's I didn't like write an end to end test per se, right?

It just kind of knew how to do it and navigate. You can do really a lot of interesting things here. So that's Boris Shenley's workflow. I hope you guys enjoy this. I think one of the really interesting things is that if you think about what your work might have looked like 6 months ago and then you compare it to like Boris's workflow, it's probably like night and day, right? We were still manually coding at that point. But now we have like multiple um clock code instances just going at all times.

Multiple clock code instances and you're just like playing Starcraft over here, you know, uh basically saying, "Hey, file this PR over here." And then and then you go to the next one and then you go down and go, "Oh, update my like backlog task or fix this bug." And you switch over. Now, I know probably this is context switching wise, mentally is probably the bottleneck at this point.

And these little quick prompts that I'm doing is obviously not ideal, but you should really challenge yourself to try to like take some of the learnings here, try it yourself. Like one thing I'm going to do this week is definitely figure out how to do the teleport things more often. Obviously, I can't do it for my work because we don't have SLT teleport yet. for my personal products.

I could just see myself doing this and I want to build like a entire agentic validation on GitHub so that I can just do clock ho sessions on my phone and actually code you know I did a video around kind of trying to do this with happy but it in that workflow it's very hard to test the flow bec but with GitHub actions if if it can actually navigate the web and send me like a GIF of like what it did then it makes me feel a lot more safe and and I just know that it's validated itself using screenshots and stuff like that. But yeah, I hope you guys enjoyed this video on Boris Chenry's clock code setup and go and check out my other claw code videos and my engineering videos. And
