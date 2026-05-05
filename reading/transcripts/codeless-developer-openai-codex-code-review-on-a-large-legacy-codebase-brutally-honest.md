# OpenAI Codex Code Review on a Large Legacy Codebase (Brutally Honest)

| | |
|---|---
| Creator | Codeless Developer
| Published | 2026-02-05
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=qBh5EJH1G2c](https://www.youtube.com/watch?v=qBh5EJH1G2c)
| Generated | 2026-05-04

## Description

Can code review by OpenAI Codex catch architectural flaws in legacy code? I tested it on a real GitHub repository with a strict scoring rubric—here's why it failed at the architecture level.
I use the web-based version of OpenAI Codex, connected directly to a GitHub repository, to run a full code review—no IDE, no plugins, no VS Code required. What sounds convenient turns out to be far more revealing… and not in a good way.
This legacy .NET project has Entity Framework at its core—a classic database-centric anti-pattern. Let's see if Codex can identify it.
⏱️ Chapters
00:00 – Intro
00:33 – Guiding the AI (benchmarks, standards, and why they matter)
01:42 – The Scorecard (weights, categories, and expectations)
02:05 – Codex's Review (scores, findings, and red flags)
04:05 – My Opinion (why this review fails at the architecture level)
🧠 What This Code Review Reveals
This legacy project has already been reviewed using GitHub Copilot, making it a strong comparison case. For this Codex review, I went further by providing a strict benchmark, weighted scorecard, and explicit architectural expectations.
Despite that, Codex:
✗ Focused mostly on class-level and syntax-level issues
✗ Gave architecture 62/100 when it should be ~10/100
✗ Failed to identify database-centric anti-pattern (EF coupling in domain layer)
✗ Over-scored testing despite only having E2E tests (no unit/integration)
✗ Ignored static-heavy domain logic violations
✗ Produced a "fluffy" executive summary that only read the README
Even with a detailed weighted scorecard, the final verdict was 50/100—far too generous for this fundamentally flawed architecture.
👨‍💻 This Review Is For You If:
- You're evaluating AI coding assistants for your team
- You work with legacy codebases and need architectural insight
- You want to know if AI can replace human code reviewers (spoiler: not yet)
- You're curious about OpenAI Codex vs GitHub Copilot vs Claude
⚠️ Final Verdict on OpenAI Codex for Code Reviews
Codex is fast and convenient, especially in a browser-only workflow, but it currently struggles with:
✗ Solution-wide architectural reasoning
✗ Legacy systems with implicit coupling
✗ Meaningful scoring against strict standards
At this stage, most AI coding assistants—including Codex—are still better at local programming feedback than true architecture reviews. In my experience, other models (notably from Anthropic) are currently stronger at complex system-level analysis.
###🔗 Resources:
GitHub Repository Reviewed: [https://github.com/mpholoane-bapela/lottron2000-legacy-dotnet-framework]
PlayList: Lottron2000 Rewrite: [https://www.youtube.com/watch?v=P26t5EVz70U&list=PLphsQTGN5DbKsbob0eN5y4zQhZXUzpow2&index=1]

> Transcript extracted from YouTube auto-generated captions (`qBh5EJH1G2c.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

using open AI codeex for doing a comprehensive code review. I'm using the web- based version of codeex that is being uh pointed directly to a repository on GitHub.

This is one of my old projects that I have also used to test out other AI coding assistants such as GitHub Copilot. Now, using the web- based version is very convenient since there's no RDA needed or no plugins are needed.

There's no need to open tools such as VS Code. The code review can be initiated directly from the codeex web chat.

Here's the source code that I'm going to be code reviewing. I have it in open in Visual Studio just for clarity.

I have previously completed a strict review in GitHub copilot and it is a very good idea to provide your own coding conventions and a custom benchmark.

AI will need a lot of guidance in pointing the review to a very uh specific benchmark or standard.

So simply giving an a oneline prompt for AI telling it to just uh code review especially when the the code base is relatively large that's not going to be the best use of time architecture and how the solution is arranged. Um usually those are key items to be to be scoring and that's where AI tends to to have the most difficult time.

So here's the first lazy oneliner version of prompting codeex to complete the code review and it can point to issues only at a code level. So problems such as static classes and maybe how the data access can be improved. I did not uh it didn't give a score because there was no benchmark given and this review took just under four minutes. Now in the much more detailed code review, here's the scorecard in the template. Codeex must review this legacy code against a strict benchmark. I'm pointing or guiding the AI to very specific areas and have placed very specific ways for each section. Codeex will start the code review and about 5 minutes later the review is completed. So now just going through what what this review is saying.

Starting off with executive summary. To be honest, I feel as though AI is cheating here because on the readme file on the on the actual repository, uh, the readme file goes into detail explaining what what the code is about. So, I'm not sure if Codeex just simply glossed through that readme file and then put a little spin on it. Reading through the rest of this summary, there's too much fluff and it doesn't summarize all the main issues that it went through in in the actual review. Now moving on to the core review itself which has got different categories of scoring. The final verdict is 50 out of 100 which I feel is a little bit too high. Solution uh solution architecture is given 62 and again this this is completely wrong. My legacy code doesn't have any modern architecture.

the solution. Yes, it does have separate projects and it can give the impression of being properly structured but in fact the database is at the core of the application meaning just in modern terms this is going to be a big antiattern. So this this legacy code or code base or solution isn't really following any modern architecture or something that you could point out as being well put together. To explain this anti pattern very briefly, this entire application is directly coupled to entity framework. The domain project are indirectly coupled to this data project.

The score should be something like 10 for architecture, not 62. So 62 is a little bit too high. Then moving on, there are tests in the solution, but these are only end to- end tests and they're not actually testing the the core domain or logic. So the testing pyramid is not being followed and the actual score for testing I feel should be about a five.

Programming should also be a little bit of a fail because the domain logic is mostly static classes.

So all in all the code review I really feel this is not good. Um to be fair to open AI codeex code reviews for large solutions can be quite hard and I've seen this similar pattern for most other AI models. meaning that at this stage all of them seem to only manage programming or just reviewing a single class. They can only review at that sort of level. If you ask the AI to sort of look at look at a bigger picture, um they're not able to review the entire architecture. But I'm even more surprised even after I've provided Open AI Codeex with a very detailed guide and a scorecard, but still it managed to give this review a very very high score.

So it looks like it wasn't very strict enough. My verdict at this stage on Codeex's code review, I would say consider using a different model if you want a code review. Perhaps models from Anthropic are still in the lead when it
