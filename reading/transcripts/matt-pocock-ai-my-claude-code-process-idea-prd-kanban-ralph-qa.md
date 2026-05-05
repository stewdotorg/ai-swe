# My Claude Code Process: Idea, PRD, Kanban, Ralph, QA

| | |
|---|---
| Creator | Matt Pocock AI
| Published | 2026-02-23
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=WHt67PZjHtk](https://www.youtube.com/watch?v=WHt67PZjHtk)
| Generated | 2026-05-04

## Description

Learn how to organize AI agent workflows using GitHub Issues as a Kanban board, PRD skills, and vertical slice task decomposition for efficient autonomous coding.
0:00 Introduction
1:19 GitHub Issues
2:10 Skills
2:36 Demo
Getting started with Ralph:
https://www.aihero.dev/getting-started-with-ralph
AI Hero Newsletter:
https://aihero.dev/newsletter
Follow Matt on Twitter
https://twitter.com/mattpocockuk
Join the Discord:
https://aihero.dev/discord

> Transcript extracted from YouTube auto-generated captions (`WHt67PZjHtk.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Hello friends. I wanted to give you an update as to where my Ralph experiments have got to. Specifically, I've been debating the value of basically whether you include an implementation plan or something like it before you initiate your Ralph loop. Essentially, the process goes like this. You've got an idea in your head somewhere and you turn that into a prd file like this. The question is is do you then just take Ralph like this and just run Ralph based on the raw PRD or do you do something else and that's something else if I just rearrange everything like this would be to kind of make some kind of some sort of plan document like this where you then run Ralph based on that plan. I was resistant to this for a while because I didn't want to impose any extra work on the dev because if you think about it, you've got this kind of red line here which is basically everything above this is what the dev needs to do. And of course the dev then needs to the stuff that comes after Ralph too. So that ends up in the dev's domain again. But recently I've gone back to sort of having some kind of like planning stage between the PRD and Ralph. And for those who don't know, Ralph is essentially a loop that just sort of runs again and again on some kind of plan or a PRD to try to get the codebase to that state.

So you can think of Ralph as like an autonomous AI agent just running in a loop until it reaches the desired state.

Now I've put these down as markdown files here, but actually I end up putting these in as GitHub issues. So we have essentially a PRD GitHub issue and then the kind of setup that I've landed on is instead of having one monolithic plan here is to actually break down the PRD into a bunch of different GitHub issues. These different GitHub issues kind of act like a canban board basically where you have all of the things you need to do in order to complete the PRD in order to kind of complete the sprint. You have relationships between the issues too. So like this task is being blocked by this task. And so this takes some pressure off the Ralph loop at the bottom here because all the agent needs to do is just find the next unblocked task sort of see the link to the PRD to understand the entire picture and then it's good to go. This also means that you can effectively parallelize too because you just get two agents to work on the like canon board and just pick any unblocked tasks. Now the way I create all of these assets here is via skills. So for instance, between the idea and the GitHub issue, I have a write a PRD skill. Then between the PRD and the Cambon board, I have a PRD to issues skill. And then to run the autonomous AI agent, I just have a bash script, which is Ralph.sh. These skills are becoming like an essential part of my workflow.

And the way that they compose together and allow for different workflows in different repos is just beautiful. So let's take a look at a feature I'm working on now. We just added this PRD here to create a new video from selected clips and sections inside a video editor that I'm working on. This contains a problem statement. This contains the solution. Contains a bunch of user stories and contains a bunch of implementation decisions. This is a really rich document. We're also doing testing here too. So describes how to test things. Describes everything that you might need to uh kind of implement this. What it doesn't do is give you an actual implementation with step-by-step tasks. So I've done that by running my skill and creating all of these different issues. So for instance, this one just creates the first kind of copy mode here. And this is blocked by nothing. Can start immediately. And it even says which user stories it addresses in the original PRD. Again, if we keep zooming through here, we'll go to 198, which is the next one it created. This is blocked by the other one. So it's blocked by uh 197. And this simply addresses uh user story 15 cut clips from original video. So again the scope of all of these is quite small and I've also instructed it to make sure it creates vertical slices here. What I mean by that is if we think about systems having layers here where you have let's say a database and an API or a front end or you probably have multiple many many layers inside one of these deployable units. The thing that LLMs love to do is they love to code horizontally. They love to get all of the database work done first then they'll do the API stuff and then they'll do the front end. Now, that ends up being miserable because you don't get any decent feedback until you've really started phase three. So, I prefer using a traceabulit style approach where you prompt the LLM to use vertical slices to actually create features across all integration layers. That means it can get feedback on its approach much earlier kind of within phase one that all the integration layers actually work together. So, that's a big part of my prompting for the PRD to issues skill here. It really needs to make sure that each issue does a really strong vertical slice instead of just kind of like creating a horizontal slice of garbage.

So that's where I'm at right now. You can check out all of these skills inside Matt PCO skills up here and of course use npx skills add to add them if you like. I'm not exactly maintaining this repository in a responsible way. This is just literally a repository that's pointed at my uh local skills directory and so any changes that I make there I do tend to push up. So, if you want the latest thing that I'm using, then it will be here. But if you do want to keep up with everything I'm doing, then the AI Hero newsletter is the place. I'm posting new agent skills up there. I'm posting tips about how to use claw code and all sorts of AI agents. AI coding is here to stay and it is a lot of fun. And this is the place where I explore all of those cool things. Thanks for watching.

I'll put all the links below that you might need if you want to follow along.

So, thanks again and I will see you in the next one. and I will see you in the
