# Pi Coding Agent: The Unrestricted Alternative to Claude Code

| | |
|---|---
| Creator | Zero Code Devs
| Published | 2026-03-13
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=KMTcJX89jFI](https://www.youtube.com/watch?v=KMTcJX89jFI)
| Generated | 2026-05-04

## Description

The Pi Coding Agent by Mario Zechner is the backbone of Openclaw — and if Claude Code is VS Code, then Pi is Neovim. No restrictions. Any model. Fully customizable with TypeScript.
In this video, I show you exactly how I use it across Windows, Mac, and Linux — including my cross-platform config setup synced via OneDrive, my WSL + tmux workflow, and the custom sub-agent teams I've built to handle real tasks.
We walk through two examples:
- Generating a Readme file using a Documenter sub-agent
- Building a full, accessible firefly website in just two prompts using a specialized web team (Scout → UI Designer → Greenfield Web → Conventions Analyst → Builder → Reviewer → WCAG Auditor)
The whole thing — planning, building, reviewing, and fixing accessibility issues — handled automatically.
🔗 Pi Coding Agent: https://pi.dev
📺 Indie Dev Dan's Pi Agent series: https://www.youtube.com/@indydevdan
💻 Indie Dev Dan's GitHub repo: https://github.com/disler/pi-vs-claude-code
— — —
⏱️ Timestamps
0:00 - What is the Pi Coding Agent?
0:36 - Cross-platform setup (Windows/Mac/Linux)
1:31 - Getting into WSL, tmux & Pi
1:46 - My custom sub-agent setup
3:26 - Demo: Creating a Readme file
3:57 - Switching to a website-building team
4:53 - Building a full website in 2 prompts
6:32 - Final result walkthrough
7:25 - Recap

> Transcript extracted from YouTube auto-generated captions (`KMTcJX89jFI.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

My harness of choice is the pi coding agent by Mario Zner. If you're not familiar, the pi coding agent is the core behind openclaw.

Think of it like clawed code without restrictions.

If cloud code is vs code, then the pi coding agent is neoim.

Use it with almost any model you want.

And with typescript, you can make it look however you want and you can make it do whatever you want. I'm not going to tell you how to install it. For more on that, check out their website at pi.dev. This video is about how I use it. Now, I normally jump between Windows, Mac, and Linux on a daily basis. So, there are a few things I need in order to stay productive on all three platforms.

I don't want to have to rebuild my Pi configs on every system I use.

Thankfully, PI allows you to specify where you want to store your configs.

For me, that's a folder on my one drive that's synced between all my platforms.

It could just as easily be a Dropbox or iCloud or your cloud storage of choice.

It could even be a folder containing a git repo, which I also do because I'm constantly adding, removing, and changing TypeScript files, and I like being able to roll things back. Besides that, I need a terminal to work from. On Mac and Linux, those are already taken care of. On Windows, I use WSL with Ubuntu in order to avoid needing to

I'll also need Node.js because that's how you install the PI coding agent and its plugins.

And finally, I'll need T-Mox installed just so I can have some flexibility with the terminal windows.

Okay, so in this example here, we're just going to start in an empty folder.

I'm going to create a new folder and

Then I'm going to rightclick on the folder and open it in the terminal.

Now since I'm on Windows, uh I want to get into WSL so that I don't have to

And now that I'm in WSL, I want to get

And now that I'm in T-Mux, I want to get into Pi.

Now, if you have Pi installed, yours is not going to look like this because I've made some customizations.

When I was learning about Pi, I watched Indie Debb Dan's video series on it right here about PI agents. He has a GitHub repo based on that video and in there he has different extensions that he's made and I've taken his agent- team extension and I've made some modifications to it. So basically what that extension lets you do is create a bunch of sub aents and assign them to teams and then you use those specific teams to complete a specific task.

So, as you can see, my sub agents here.

I have a scout that can read folders and files. I have a browser which can search the internet. I have a documenter which creates readme files. I have a reviewer that makes sure things are accurately done. I have a negotiator which makes sure we're making the right decisions.

And I have an agent builder which lets me build more custom agents. So, for this example, we're just going to create a readme file um and have it be based on

Because I told it to create a file, it's dispatching the document sub agent. And on the bottom of the screen, you can see everything that the document is doing.

Okay. So, as you can see, it's done now.

And it didn't even have to search the internet. It already knew what Firefly was and it created the markdown file for us. So, this team of sub agents is not very good at building websites. So, I'm going to switch to one specifically made

As you can see, this team is a little different. It has a scout so that it can read files. It has a conventions analyst which if there's existing code already, it'll evaluate that code and decide how it should write its new code. It has a planner to create a plan on how to make a website. It has green field web, which is basically going to tell it to create websites the way I like to create websites. It has a UI designer to help it make good uh layout decisions. It finally has a builder to create everything, a reviewer to make sure we're staying on task, and even a W keg auditor to make sure our website is accessible. So now I'll just tell it to

So the first thing it'll do is send the

Now that that's done, it'll call up the UI designer to come up with a look for

Once the design is done, it'll kick off the Greenfield web sub agent, which would basically bootstrap a website the

With the base website complete, it's going to kick off the conventions analyst to see how it should code the

Now that it's done observing how I coded everything, it's going to dispatch the builder, which will actually go through and build the website.

After the reviewer agent did a quick check of how things are supposed to be, it kicked it back to the builder to fix

Now that the builder is done, we'll kick it off to the W CAG auditor to perform

As expected, the accessibility auditor found some issues and the builder went

So, there it is. Everything is complete.

Now, we can just go ahead and test it by

That'll open it up in Visual Studio

Let's go ahead and install all the packages it needs and finally run it.

As you can see, it gave us a beautiful website using fancy fonts. It tells us

talks about the genre,

talks about some of the best episodes,

And there you go. And as you can see that uh the whole process just took two prompts. the one prompt asking it to make a markdown file and the second

It went through and it took care of everything it needed to figure out which agent it needed to do what and it even went back and checked its work and made changes as needed. So that's how I use
