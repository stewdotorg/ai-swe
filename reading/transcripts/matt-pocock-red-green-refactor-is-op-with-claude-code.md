# Red Green Refactor is OP With Claude Code

| | |
|---|---
| Creator | Matt Pocock
| Published | 2026-02-23
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=hYZdIwFIy-c](https://www.youtube.com/watch?v=hYZdIwFIy-c)
| Generated | 2026-05-04

## Description

Learn how to get better results from coding agents using Test Driven Development and the red-green refactor cycle. Discover why this 30-year-old software practice is perfect for AI-powered coding.
---
Resources:
My Claude TDD Skill:
https://www.aihero.dev/s/vDwUHq
Newsletter:
https://www.aihero.dev/s/oCIDpR
---
Follow Matt on Twitter:
https://twitter.com/mattpocockuk
Join the Discord:
https://aihero.dev/discord

> Transcript extracted from YouTube auto-generated captions (`hYZdIwFIy-c.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Let's talk about a ridiculously easy way to get better results from a coding agent using a software practice that's like 20 30 years old at this point. This is red green refactor or in some Willis's web blog here this is red green TDD. TDD stands for testdriven development. TDD is probably most kind of uh prolific advocate is Mr. Kent Beck here in extreme programming explained.

XP was a software practice developed in I don't know 90s 2000s and it advocated extremely aggressive use of unit tests or aggressive is maybe not the right word but everything in the software practice was built around unit tests. As Simon said the most disciplined form of TDD is test first development. You write the automated test first, confirm that they fail and then iterate on the implementation until the tests pass. And as Simon says, this turns out to be a fantastic fit for coding agents. I have definitely found this to be true. But what do the red and green here mean?

Well, red essentially means write a failing test and the CI goes red. In other words, any automated types or tests that you've got on the repo will be at that point red. You've written a failing test to verify that the thing is going to work when it's built. This might be that you're fetching something from a database using some kind of SDK and you're testing the SDK and you basically write that the fetch is going to work before you've even implemented the API method before you've even implemented the DB schema. And then once that red test is in, you then write a green implementation to make the CI go green again. In other words, all of your unit tests go, "Yep, tick. That looks good." The important thing is that this is minimal, okay? because in the next step we're then going to refactor the code that we just wrote in order to make it prettier to factor it into the shape that we want. And we get the luxury of doing that because we have already made our CI green. And so we have a set of tests that allow us to test that our refactor doesn't break anything. Now for experienced software engineers, you're probably going, okay, why does this matter more now than it did 10 minutes ago? Because red green refactor has always been an amazing way to build software. So why are we talking about it in the AI age? Well, for me, I find it really, really comforting when I see an agent doing red green refactor. Let's imagine this sequence of events where the agent writes a failing test. I can see in the CI or I can see in the agent's output that the test failed. I then see it write an implementation and it doesn't change anything about the test. Doesn't try to like fake it to make it pass or anything and the test goes green. Now, if it's a reasonable agent, it's pretty hard for it to fake that. And this means I don't end up reading a lot of the tests that are created during my red green refactor loop. I maybe skin them, especially just the titles to understand what is being tested, but I don't necessarily read all of the implementations because I've seen it go red and then go green. I feel pretty confident that it's a reasonable testing the thing that it's supposed to.

And of course, once this loop is over and it's committed to the branch, I then go and QA that chunk of work so I catch anything that the tests might have missed. And at that point, I can generally flush out any bad tests if there are any. You can find all my skills at the link below. To make this work, I have a TDD skill that I like to invoke when I'm building these features.

You can find all my skills at the link below. Now, there's stuff in here that I'm not talking about in this video, but the main focus of this is using the where is it? The red green refactor approach. So, down here, I get it to do an incremental loop for each remaining behavior. Write the next test. see that it fails and write then the minimal code to pass and then pass. The rules are that it should only do one test at a time. I find that this is a really important caveat because it means that you then don't end up with a huge splurge of tests. This is one thing that LLMs love to do which is they love to create huge horizontal layers and then they'll try to oneshot an implementation that passes all 90 of those tests. So they will do one massive file edit where they'll add 90 different tests. Now that's possible, but you do end up with a lot of crap tests in my opinion. So just getting it to focus on the thing that it's implementing at the time and then writing a single test for that.

Then writing uh the implementation to do that, another test, another implementation, another test, another implementation. You end up with tests that are really important for actually guiding the implementation. In other words, this one test at a time idea is really focused on improving the quality of your tests. Then once I'm done with this incremental loop, I then say after all tests pass, look for refactor candidates. But again, that's probably the topic for another video. So yeah, red green refactor is a thing that you have to know how to be able to do. And unsaid throughout this whole video is the fact that feedback loops matter so so much with AI. Because AI is so eager to create code and find the like fastest solution to your problem, you need to impose some back pressure on it to essentially keep it uh in a stable state. and strong types like TypeScript of course or you know like uh unit tests or things like that can really assist you in getting highquality code and so I think code quality is actually more important than ever because I mean if you've got a lowquality code base the LLM is going to replicate what it sees just like any developer out there it will be happy to play in the mud if what you have is mud. Now, if you're digging this stuff and you like what you're seeing on this channel, then you should check out my newsletter, which is where all of these videos go, and also sign up there to get my new agent skills first, too. I am putting together a Claude Code course, which is going to be very, very exciting. So, I will, of course, let you know there when it drops. Thanks for watching, and I will see you in the next
