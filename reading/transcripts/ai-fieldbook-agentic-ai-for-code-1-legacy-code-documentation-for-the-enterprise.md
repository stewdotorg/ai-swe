# Agentic AI for Code #1 - Legacy Code Documentation for the Enterprise

| | |
|---|---
| Creator | Ai Fieldbook
| Published | 2025-09-30
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=GN7ijr18v6Y](https://www.youtube.com/watch?v=GN7ijr18v6Y)
| Generated | 2026-05-04

## Description

Modernizing Legacy Code with AI Agents | A Comprehensive Demo
In this episode, we delve into the complex world of legacy code modernization with the help of AI agents. Our client, one of the largest global retailers, faced the challenge of poorly documented legacy code built in the late '90s and early 2000s. To tackle this, we introduce an AI-driven approach to understand, document, and refactor the old code base. Key topics include the ingestion of code and documentation into a context lakehouse, detailed code analysis, generation of business context diagrams, forward-looking recommendations for refactoring, and the creation of comprehensive test cases and product requirement documents. Join us for an in-depth demo showcasing the transformative power of AI in updating legacy systems.
00:00 Introduction to Legacy Code Modernization
00:08 Client's Legacy Code Challenges
01:09 AI Agents for Code Refactoring
01:39 Demo: Legacy Platform Modernization
02:38 Building the Context Lakehouse
04:02 AI's Code Analysis and Documentation
07:11 Generating Test Cases and PRDs
09:58 Translating and Modernizing Code
10:37 Conclusion and Next Steps

> Transcript extracted from YouTube auto-generated captions (`GN7ijr18v6Y.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Hey everybody, welcome back. Today we're going to talk about another story and this is around legacy code modernization. The client was a very large retailer, one of the largest global retailers and they had a problem.

Their legacy code was poorly documented.

Nobody really understood how it all worked and because of a variety of reasons, they actually were now required by local outside of the US to rebuild their codebase in that environment so that they could run their stores in country on that toolkit rather than running their stores in country on technology that was built in the problem was all of this technology was built back in the late '9s, early 2000s.

Very little documentation and very few of those people are still left and the architecture and the structure and the business context of those applications were not well understood. We just know that they run. So, we suggested, how about we use some AI agents to go through and understand what's going on inside of that legacy code and then help you refactor that into a knowledge base and a broader set of potential for refactoring those in other languages.

Let's dive into it.

We'll get into the legacy platform modernization piece as well as the knowledge management piece in this first demo.

So you'll see here we've trimmed it down vastly. We're talking about tens of millions of lines of code and we're talking about something that was built over 10 15 years and the code sits in the front-end guey. It sits in the application layer. where it sits in stored procedures and there's a whole host of components that interact to make these applications work and all of this had to be really well understood so that it could be rebuilt in the local language.

So the first thing that you'll see that the AI agent does is it pulls in all of the codebase that is available. It also pulls in any documentation you might have on the codebase and you can also add lots of other things. So we ingest it all into what we call the context lakehouse and the context lakehouse pulls in everything about the application. So it could be the code itself, it could be the tests that you run on the code, it could be any documentation that you have, it can be telemetry data, it can be APM data that comes in, it could be tickets in terms of major incidents that you had with that application. So feeding in everything that you can into this context lake so that the AI gets a really detailed context of what that application is all about.

Now, a lot of people have tried to do this using the current IDE tools, and they're realizing that the context windows of the LLMs as well as the effective size of code bases that the current idees can handle is not large enough to deal with tens of millions of lines of code. If you're doing 10,000 lines of code, then definitely that's your best tool. So what we do in this context lake is interrelate all of these different components so that you have a clear understanding of what the application is, where it has errors, how it's being used, what parts of it get used, all of those pieces that you wish you knew about every one of your about every one of your applications. And so once we've ingested this content into the context lakehouse, what we then do is we're able to analyze it to do a variety of different things. In this case, the AI has a little ID that it uses to read through all the codebase and actually understand what's going on in the code and document what's going on in the code at whatever level of detail you wish. So in this demo, we'll just take you through a high level on a few different chunks of code.

So you'll see here what the AI will do in the IDE is it will go through the chunks of code. It'll go into multiple folders, go through chunks of code, and start documenting what it thinks that the code is actually doing. And it's pretty good at it. It's right about 80 to 90% of the time. The other 10 to 20% of the time it gets a little confused because you have a variety of different code that's in there that was built over 15 years. And so you may have things taking different directions inside of the code. And so that's where you require the human intervention to actually look at the specification or the requirements that are being created and say, "Yeah, this is something that we built in 1990 and then we stopped use using it in in 2000." And so that's the level of detail and granularity that the AI can get into. And you'll see here it's going through just systematically step by step. We're going to get this done in a few minutes, but typically it will run for an entire day to go through and get this content laid out. And then as it goes through and really understands what's going on here, we're just looking at the Java code in the application layer, but it can also process stored procedures. It will also look at JavaScript and other languages in the front-end guey and put everything together in a nice context where you'll be able to see how all these different components interrelate. So that's the second thing it does once it's understood everything that's going on in the code. It will then put together a business context diagram where you really have a good sense of how all these different components interrelate.

And then the third thing it does is it actually is thinking about the future.

And it says given that you've built this in the '90s, there might actually not be a perfect solution here that we're trying to replicate. So there are other things that we didn't do in the '9s like API layers, like role-based access control at the application level. So those types of components, if you're going to refactor the code, you're going to want to build in. So it actually has a forward-looking component that says this is what you've done. Here's how it all interrelates, but then if you are to refactor, this is what you should do.

And so then now that you've got the whole thing documented, there's a whole bunch of things you can do, right? You can generate test cases. So you'll see here that every single piece of documentation that we created relates back to lines of code in the legacy in the legacy system.

And so now you can generate test cases that you can actually trace back from what we're generating from a test case perspective all the way through to the lines of code that that we're triggering that in the legacy codebase. And the AI can do a variety of different things which a lot of people are familiar with.

It will generate the requirements. It will generate the test cases. is it'll lay out all the different componentry behind the test cases. It can also generate test scripts and in a future video we'll go into the QA agent which is actually doing that generating test scripts out of the business requirements and keeping them evergreen. So here you'll see a couple of things that this is doing. First full traceability from the test case through the requirement to the legacy code. The second is it's generating unit tests. It's generating system tests. It's generating integration tests. All the componentry you need to make sure that any changes in this code, you can then validate. And then the third thing it's doing is it's prioritizing those things and saying to get optimal code coverage, you should do all of these. But if you don't have the resources, then at least do the highs and you've got a full heat map of what kind of test cases you should be running. And you'll also see where it can it rationalizes and addresses multiple requirements with a single test case.

And so now you've got these things, you've got the requirements, you've got a documentation of the code, you've got test cases. Now you can do all kinds of other things like generate a PRD.

So if you in fact need to refactor the code, you can generate a detailed product requirement document and be able to share it with other development communities in your organization who can then go work on different parts and pieces of the code as you refactor it.

The other thing you can do with the PRD is you can actually take that and use modern code generation tools to generate that code. Especially if you're refactoring small components of it, a lot of the IDs are very very good. If you're going to generate and modernize at scale, then we have our own agent that can actually process tens of millions of lines of code and generate that at a very high level of accuracy using this PRD. And again like I described in this case the folks needed to send this to their colleagues but in the local language so that their colleagues could actually build this out. And so you'll see here what we've done is actually built a mechanism to get to a translation that's very clear and very clean. Again it's not perfect. LLMs are never perfect. And what we've done is taken several steps to reduce the hallucination that happens across the board. But you always want a human in the loop to review these pieces of content that the AI is generating.

And so now you get the idea here of we've built this context lakehouse that really understands the depth of the application from a code perspective, from a documentation perspective, from a product requirements perspective, from a testing perspective, and if you had fed in your incident data from an incident perspective, where it's strong, where it's weak, all those kind of things. And now what you can do, and I'll maybe break this out into a separate video because this one's gotten pretty long already. But what you can now do is you can put a knowledgebased interface over the top of this context lakehouse and you can ask it all kinds of different questions around generate some requirements for me or just a shoulder tap question that you might ask somebody who's anme on this application.

So, we'll pause there and we'll go into
