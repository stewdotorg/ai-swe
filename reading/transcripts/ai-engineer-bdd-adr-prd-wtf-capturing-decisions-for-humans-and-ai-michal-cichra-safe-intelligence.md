# BDD, ADR, PRD, WTF: Capturing Decisions for Humans and AI Alike

**Speaker:** Michal Cichra, Safe Intelligence  
**Event:** AI Engineer (YouTube @aiDotEngineer)  
**Date:** 2026-06-03  
**Length:** 12:49  
**Views:** 12,467 | **Likes:** 314  
**URL:** https://www.youtube.com/watch?v=504PvfXou5Y

## Description

"One thing harder than reading AI code is reading AI tests." Mikuel from Safe Intelligence argues spec driven development leaves a loop open: you have a markdown spec, but how do you know the product actually behaves that way? His answer is Cucumber, nearly forgotten and suddenly useful again. Executable, human readable BDD scenarios connect directly to PRDs and critical user journeys and close the gap between what the spec says and what the tests verify.

The rest of the talk is enforcement. ADRs capture not just what the rules are but why; agents rejected at commit time get linked back to the document and iterate. Module import linting makes N+1 queries structurally impossible: rendering templates cannot touch the database, E2E tests cannot import any module that could. His sessions run 20 to 50 context compacts. The agent stays on track because the rules live in git hooks and CI, not in the prompt.

## Transcript

Hi, I'm Michal. Welcome to capturing decisions for humans and AI alike.

Yesterday with a team from Safe Intelligence, we have released Spec 27, a new product to test agents. Before that, I was in Microsoft, Red Hat, and spent 10 years working on a single product. The consistency problems we face with AI and the story of capturing decisions show up in every product I have seen. And these notes are distilled from the experience. And you can find me at the booth.

So, BDD, PRD, ADR, like that's a lot of acronyms. Why does any of that matter?

So, let's unpack it from the end. You probably know this story. I hope it's not an urban legend, but scientists put five monkeys in a cage with bananas on a ladder. Then gave them a cold shower every time a monkey tried to get a banana. Other monkeys beat up the poor fellow. Then they replaced the monkeys one by one and none of the originals remained. And yet, they have beaten up every monkey that tried to climb the ladder not knowing why.

So, humans and LLMs, they suffer from the same trait. Limited context. People forget. LLMs context compact. Humans leave. LLMs have no memory.

After a while of operating a product, the team starts asking, "Why do we have this flow? Why is this goal of this feature? Why is this code shaped like that? Why? Where does this belong?" And you might not have the founding engineer available to answer. And these problems show in every org. Maybe with AI much sooner than they used to.

### ADR — Architecture Decision Record

ADR is architecture decision record. It records why you do something and how you enforce it or how you want to do that. And you can cover examples by reference docs and code snippets.

For example, we split code in layers to prevent N+1 queries. We enforce that split by linting imports in modules. And we also enforce reading from database returns plain shapes instead of ORM objects, so we cannot make these queries and to prevent duplication. And also linting it by module imports. And another like 50 ADRs that define architecture of the product.

There is not a single format that you need to use. It's just a concept. It's a text, so there is no specific way how to enforce it. You still need a tool to enforce it. But the tool will tell you that this is the rule. Why are you doing this? And how are you supposed to fix it? Then the agent will go and try to find this document why this reason exists and more information about how to fix it. Also, you can define like which files it actually concerns to, like is it some Python files or some folders? And how you actually enforce it.

### PRD — Product Requirements Document

PRD is a product requirements document. That's something lighter when you're building a feature, you describe why that thing exists and what problems it solves. And how user goes through the app to actually interact with it. What's the journey through the application. It can be very light. It doesn't need to be really long and exhaustive like a massive document. You can just capture why, the problem, and the goal, and the journey that connects them. And it's not just for the agents, but also for you 6 weeks from now when you forget why you did that.

### BDD — Behavior-Driven Development

Now, BDD. It's behavior-driven development. You have probably seen spec-driven development lately, but if you practiced it, you might have suffered the same thing as me. How do you validate that the product actually adheres to the spec? It's a markdown document, you describe how it's supposed to work, but how do you know it actually works like that? One thing harder than reading an AI code is reading AI tests.

So, what if you had an intermediate layer that actually describes how the product behaves in a human language? And BDD is not new and shiny, but it can be executable and readable.

So, enter Cucumber. It's almost forgotten, suddenly useful again. It's definitely easier to review than your average tests. You can connect scenarios directly to your PRDs and critical user journeys. It can be readable, executable, and it closes the loop that a spec-driven development leaves open.

These rules, these specs are later parsed by steps and they are executed as code. But what you can do is that you can actually write and read these. And you can review these. And you can understand these. The language is on you. It doesn't need to be enforced. Like there are multiple ways how to write these features. And that's it, but they describe how you're supposed to go through the application, why this thing exists, and how it runs. And similarly, they can refer back to all the documents that you have about why things exist.

### Design Systems for Consistent UIs

And as a bonus, making consistent UIs with agents is just another level of hard. Like design system and pattern library are the way to build consistent UIs. That was the way before AI and it is the way now. So, you document your language. You say, for example, a primary button is this and that. It is blue. It has this shape. It has this color and it's this size. And you say your rules. You say, "We will have only one primary button visible on a page at any point in time." And then you can enforce these rules. Similarly, you define components and patterns.

So, for example, if you have multiple colors of these buttons and multiple states, you define components and you define previews and you demonstrate how they work and you create snippets of previews, so you can actually see them. And the agents can see them. And then you can go and review and like do these actually adhere to the principles that I have? Do they adhere to the visuals? And then you reuse them. As with code, you build these from the ground up from small pieces into bigger ones. You compose them and you reuse them. Otherwise, it's chaos like with the code.

### The Enforcement Loop

So, cool. These are cool ideas, but how do I actually enforce this? So, my team and agents stick with it. How do I keep it consistent? Well, with the loop.

You probably have heard about closing the loop, reinforcement loop, the harness. How to remind the agent that there are rules and how to follow them. So, our loop is simple. It is git hooks, skills, CI, and linters, and a bunch of other checks.

Agent's goal is to deliver a pull request and to do that, they need to use git. So, we use git hooks to run predefined tasks and these tasks are later executed on CI. They are the same tasks that are executed as hooks. If, for example, agents would get lazy and not want to execute them or skip them, then they get caught. And we include linting, formatting, type checking, code duplication, architecture checks, document linting, everything that's possible.

There was a time where code reviews were about style and tabs and spaces and there is no space for that anymore. All these things are not for discussion. They are rules and they are enforced and they are automated because there is no space for discussion about these anymore. It's more about the high-level concepts.

**What you cannot find, you cannot enforce.**

For example, we enforce architecture of the product and of the code. We separate modules and their imports, so what you can use from where. For example, our end-to-end BDD test suite cannot access database. So, we forbid from accessing any module that could access database and basically force the module to iterate without database and really use only the browser features of the application. Similarly, in the product itself, we enforce we cannot talk to database from rendering templates. So, we know that there are no N+1 queries ever. We just define ways to prevent these problems from happening ever. You cannot keep finding them. You need to prevent them entirely.

Then the agent tries to commit and push it and they get feedback on the commit and get rejected and they get linked back to the document and they go read it and fix it. And iterate.

### Skills as Loop Focus

So, there are some drawbacks. Oh, sorry. It's not drawbacks. So, this loop is generic. This loop where they do some work, they push it and they get feedback and they iterate. But the loop can be multiple things, right? Like sometimes you're working on a product feature, sometimes you're working on a UI, sometimes you're working on more back-end-ish things. So that loop is the same, but what changes is the focus of the loop.

We have different skills. There is ADR that whenever there is an ADR mentioned, the agent will look up ADRs, how to operate with them. How to find code that's affected by these ADRs. For PRD the same. For UI loop, we actually skip bunch of checks and rather force it to iterate in a browser quickly. And test skill that actually identifies tests to run based on code coverage and file changes. So we run just a focused part of the suite and not the entire suite. And some goal execution that actually keeps decisions that the model makes so we can review them later. But all of these provide focus in the loop, but the loop still stays the same.

### Context Compacts Are Fine

There are drawbacks. It is very context heavy. Like you can run out of half of the context in starting the research. But I have no fear of context compacts. Like this actually for last half year actually works, I think. So in my sessions there are 20-50 context compacts and it's okay. Because the important things survive and the agent will always look them up again. And that's the goal anyway, right? Like you want to have multiple hour sessions with a clear goal that agent can operate autonomously with the rules that you define.

### Summary

There are decisions that you can record. There are parts of the product that you can describe why these exist. There is Cucumber or BDD that can have executable specifications that you can actually read and review, understand. Design systems can help you to build consistent UI from components. And again, enforce it that for example, there are no inline styles anywhere else. And you employ harness to loop it all together.

May the spec be with you.
