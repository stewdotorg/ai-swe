# AI Agents Fix Code While I Sleep (OpenAI Codex Automations)

| | |
|---|---
| Creator | Owain Lewis
| Published | 2026-02-21
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=HAlERUhb1x8](https://www.youtube.com/watch?v=HAlERUhb1x8)
| Generated | 2026-05-04

## Description

OpenAI Codex automations are AI agents that work for you 24/7. They scan your codebase, find bugs, fix issues, and open pull requests — all while you sleep.
In this video, I walk through my two-automation setup: a bug scanner that creates detailed Linear tickets, and a bug fixer that picks them up and opens PRs. I show the full workflow live, look under the hood at the TOML config and memory files, and explain why this pattern works with any coding agent — including Claude Code.
What you'll learn:
- How to set up OpenAI Codex automations for daily code review
- How AI agents find bugs, security issues, and edge cases automatically
- How the bug fixer automation creates branches, fixes code, and opens PRs
- How the memory.md file lets agents learn between runs
- Why this is different from Zapier or n8n automation
- How to recreate this pattern in Claude Code or any AI coding agent
All resources here: https://owainlewis.com/
Work with me: https://gradientwork.com
AI Engineering community: https://skool.com/aiengineer
Timestamps:
0:00 Introduction
1:02 What is Codex & Automations
1:33 Bug Fix & Bug Scanning Automations
2:39 Creating an Automation
4:14 Running an Automation Live
5:21 Linear Integration & Ticket Quality
6:55 Automated Bug Fixes & Pull Requests
8:29 Under the Hood: TOML Files & Memory
9:34 Why This Matters for Engineering Teams
11:09 Use Cases Beyond Bug Scanning
12:44 Memory.md & Self-Correcting Agents
14:17 Applying This to Any Coding Agent
#OpenAICodex #CodexAutomations #AIAgents #AICoding #AICodeReview #SoftwareEngineering #CodingAgents #ClaudeCode

> Transcript extracted from YouTube auto-generated captions (`HAlERUhb1x8.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

OpenAI just quietly released a feature that I think could change how developers and businesses think about using AI agents in their workflows day-to-day.

This one probably flew under the radar for many people. It's called automations and it's from OpenAI's codecs. Right now, most developers use AI agents by prompting them in the terminal. But automations are fundamentally different.

You can write a prompt, you can set a schedule, and you can have an AI agent that will run a task for you on repeat.

I've been using automations that scan my codebase, improve my documentation, and detect issues that I would otherwise miss, all while I'm not at the keyboard.

While this is an interesting feature from OpenAI, I think the important thing to focus on is not this specific implementation of the feature, but the general concept. Background agents doing work for you 24/7. By the end of this video, you'll know how to set these up, what real automations look like in production, and why this matters more than most people realize. So, let's get into it. So the feature we're looking at today is automations, which allows you to define recurring tasks that run in the background. So essentially, you can define a task for an AI agent to perform, define a schedule, and then the agent will execute that work for you in the background on repeat. So if you haven't used it, Codex is a coding assistant from OpenAI. It's very similar to Claude Code. It's like having a developer on your team that you can hand off tasks to. Very easy to install and it's very easy to get set up. So the first thing I want to show you is how this actually works in practice. So within OpenAI codeex, I have this open currently. We have this automations tab and you can see here that I've defined two particular scheduled automations. So the first automation I've defined is a bug fix automation. And what this does is it will fix any open issues in my bug quue. It will pull down tickets from my linear board and then it will go and fix those issues. Then this second automation I've defined is a bug scanning automation. What this does, it will run on a schedule and find issues in my code. Essentially, it will then report them by creating a ticket so that either a human or an agent can go and fix them at a later date. This runs every single day and it catches many, many things that I would generally miss myself. This is why automations are so powerful. It can't be emphasized enough how many issues get missed by human code review. How many issues get missed because we aren't paying enough attention to specific areas like security vulnerabilities or general bugs in our codebase. These things always creep in and an automation allows you to fix them before they come a major problem for customers. So you can see here that in order to create an automation is pretty simple. We just give the automation a name. We define the project we want this to run in. So we could run this in multiple projects if we wanted to or we can just run it inside a single project. And then we have a prompt which is where we tell the agent what to do. So in the example of my bug scanning automation what I'm doing I'm performing a code review and looking for critical bugs within the codebase. And then basically what we're doing we're checking to see if we have any existing bug tickets for this issue.

If we do, then we skip over this and we do nothing. But if we do have an issue, what we'll what we're going to do, we're going to create a ticket in our ticket queue. We're going to add add a label and we're going to add all of the information that an a different coding agent would need to go and fix this particular issue. We also need to define stable paths as well because one of the things to be aware of with this feature is that automations run inside a git work tree. And so one of the gotchas I found was that the paths being defined when I was running this automation weren't quite what I expected. So I've just added a bit of detail here to make sure that the agent uses the complete path to the original code.

And then finally once we're done we just report the number of bugs and then we talk about how many were skipped as duplicates. Pretty simple. We define a schedule down here. So we're going to run this every single day at 9:00. You could also run this on an interval as well. So you could run these automations every 3 hours, every 6 hours, every 24 hours, whatever you want to do. Pretty easy to configure and get set up. And then if you want to manually trigger one of these automations, you can just click this button down here, which is called test. This allows you to define an automation. You can not only run it on a schedule, but you can also run it on demand just by clicking this button. So as you can see, I'm kicking off an automation now. So I'm clicking the test button and what you should see over here is that we've created a new thread. This is now running the automation. So here's an example of an automation that already completed where you can see that the automation found two significant bugs and created two tickets. The automation added the correct labels to the tickets.

And you can see here the kind of bugs that these automations are able to detect can be quite subtle. So, this found a timeout issue in the web hook delivery code. This is the kind of thing that I almost certainly would have missed and I think many reviewers reviewing the code would miss as well.

So, it's worth emphasizing how powerful these can be because they can really level up the quality of your code because they're catching issues that you might have otherwise missed. These could be critical security issues that end up impacting a customer. Think about all of the things that you can prevent by having this kind of automation running inside your project. So what you can see here running is my linear task Q. This is just a uh an issue tracker that I use for development projects. But the thing is you can see here that we have multiple issues that were created by this automation. So this is the bug we just created. So this is the ticket we added to our backlog. You can see here we added the correct autofix label. And what's really amazing about these kind of automations and using AI agents in your general workflow in particular is that the the agents can do work at a very high standard. And what I mean by that is this is a bug ticket. And if you've ever worked in a development team, you'll probably know that many bug tickets have very vague descriptions.

There's not often enough information to recreate the bug. But you can see here the agent has been really, really diligent in its reporting. So we have a summary of the issue. We have evidence and the files that are impacted by this particular bug. We have the customer impact of this issue such as leaking resources um problematic for scheduled tasks and automation. We have a suggested fix and then we also have a way to reproduce the issue as well. So as you can see the agent has done an incredibly detailed job about identifying this bug and describing it so that now we can give this to a different automation or a different agent to go and fix the bug for us. So what you can see here is a bug that was fixed by an AI agent. So what happened here I have a second automation that runs and the automation will look for any open bug tickets that it can fix. It will attempt to fix the issues and then it will open a pull request and update the ticket. So you can see here we have an issue that was fixed by an AI agent. You can see here it's made a code change. We can go and review this now. And then if we go back to our linear boards, you can see here this particular ticket now is in review.

So the agent has updated the ticket status. We did nothing here. The agent not only identified bugs in our codebase, it also went and fixed them for us and updated all of our tickets so that we can go and now review the work.

This is really really high leverage stuff. So this is the automation we were just looking at. This is the bug fix automation. And what this does, it runs every 24 hours and it will scan my bug tracking system for any open issues it finds and then it will go and try to fix them for us. So it follows the the workflow defined here. So we first read the bug description. We check out a new git branch. We then fix the issue and then verify that everything is correct.

We then open a pull request using the GitHub CLI. So you can use any tooling inside here. You can use MCP servers, you can use Python scripts, you can use CLI tools, whatever you use inside codeex, you can use inside these automations. And then we move the ticket to an in review status. Um, so you can obviously customize this in any way you want. It's very very easy to create these automations. It's very easy to refine them and edit them as well. Very quick and easy to implement. But the the benefits you get from these automations is very significant. And this is very high leverage. This kind of automation will help you every single day that you're working on a project. It will catch issues that you would otherwise miss and then it will just go and fix them for you. So this is incredibly powerful. One thing I want to show you is how these exist under the hood. So you can see here within the codeex folder on my local machine, there's this automations folder. And you can see here the two automations we just demoed. We have a bug fix automation and then a bug scanning automation. And then if we drill down into any one of these automations, you can see that under the hood, they're stored as dotoml files.

You can see here we have the bug scanning uh automation. We have a prompt which we're using to give to the agent.

We also have a rule about the frequency we run this automation. We have a current working directory to execute this in. And then you can also see that we have a memory.md file. So this memory.mmd file essentially keeps a record of all of the different automation runs. And so this allows the agent to have persistent memory between the automation sessions, which is super useful because it may have completed work previously or there may be some information that the agent needs to know from previous executions of these automations. So that's how it works under the hood. It's a pretty simple concept and it's a pretty simple idea, but I just wanted to show you what this actually looks like under the covers. I think it's worth thinking about what this means for engineering teams because every codebase right now has bugs in it.

We have security issues that have gone undetected, unhandled edge cases, race conditions, error handling gaps. They're all there in your codebase. You just probably haven't found them yet, and most teams don't have time to do a regular code audit every single day.

Teams are busy, and these things are very difficult to detect sometimes. AI agents, in particular, are great at finding things that humans would otherwise miss. These automations can run every single day. They don't get busy and they don't forget to check things because there's a deadline or they're just tired. They run every day.

They can scan your entire codebase with fresh eyes and find issues that you would otherwise miss. If you think about what that means, it's a security issue that was caught before it shipped into production. It's a production outage that never happened because you caught the issue early on, or it's a customer that never has to see a bug or an error page. And this is just one automation on one project. This scales across teams.

You can have a bug scanner, a dependency checker, a security audit, a test coverage monitor. You can have all of them running daily. Each of those things catching errors that humans would otherwise miss because humans are too busy or they just wouldn't be able to detect such subtle and uh hard to find bugs. I think it's worth reflecting on how useful this feature can be for so many people and so many use cases. If you think about it, every single engineering team has a bunch of code that has security vulnerabilities or bugs that have just gone undetected.

And the cost of finding bugs during development time is significantly lower than finding them in production. This has always been the case. It's very difficult to catch certain classes of errors. And so, for example, when I was testing this feature, I was able to find a ton of bugs in my own code that I just would not have caught in a traditional code review or without this automated tooling. There are so many different things you could use this feature for.

Standup preparation, dependency updates, documentation checks, release notes, security scanning, almost anything you can think of that you need to do in your general workflow, you might be able to find a way to automate it using this new feature. And the way I think about it, any background task that is repetitive and relatively safe for an AI to handle is a good candidate here. Fixing documentation issues, reviewing your code for common mistakes. Running automated research, auditing your calendar or your inbox if you do it on a regular basis is probably a good candidate for an automation. And this goes beyond development workflows as well. You can have automations that run market research or emails you scan your Jira backlog for tickets that are stale, dduplicating issues, adding missing labels, pretty much anything. These aren't specifically tied to coding tasks. You could use them for anything.

One thing I was really impressed by in this tool was the use of a memory file from OpenAI. So the idea here is that traditional automation tools like NAN or Zapia, they run a deterministic somewhat deterministic flow, aentic automations are slightly different. They can be less reliable, but they can also be more powerful. What was interesting about the memory MD file is that these AI automations can now self-correct and learn from their past behaviors. So for example, if an automation ran previously and it detected an error or it learned something as it executed, the agent can then reflect on that past memory and make an adjustment or learn from a past mistake or past experience that it had.

Overall, I found this a really really useful feature from OpenAI. I am using it myself daily to find issues in my codebase and to proactively fix minor things. What's really impressive to me is how you can use AI agents like this to really level up the quality of your work because many of these bugs are quite subtle that were detected by the AI agent and I would almost certainly have missed these issues. So, it's like an extra quality check that you can put in place is very easy to install, very easy to set up, but the benefits you get from these kind of automations is pretty significant. If you can imagine a security vulnerability that was detected before it went into production, a potentially customer impacting incident that was prevented because you had these automations running. The return on investment here is really, really high.

One thing worth reflecting on is that you don't have to just apply these to coding based automations. You could use these to automate your research, to automate meeting preparation, to automate pretty much anything you do dayto-day. I think more and more companies are going to adopt this general pattern. And if you're not using OpenAI codeex, it's relatively simple to borrow the ideas and bring them across to other coding agents. So for example, I was able to recreate most of what OpenAI Codeex automations do for clawed code relatively quickly. It's not that hard to borrow the concept. These are fundamentally just agents running on a cron schedule. Pretty easy to copy the idea, but the power here of this simple pattern is pretty significant.

Thank you for watching the video. here.

If you do have any questions, let me know in the comments below. I'd love to hear about how you're using these automations for yourself. Remember to like and subscribe, and I'll put all of the resources in the description below so you can get access to them. Thanks for watching. I'll see you again in the
