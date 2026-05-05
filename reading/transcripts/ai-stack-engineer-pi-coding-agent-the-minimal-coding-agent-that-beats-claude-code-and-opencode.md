# Pi Coding Agent: The Minimal Coding Agent That Beats Claude Code and OpenCode

| | |
|---|---
| Creator | AI Stack Engineer
| Published | 2026-05-03
| Kind | captions
| Language | en
| Source | [https://www.youtube.com/watch?v=8yac_swVw8I](https://www.youtube.com/watch?v=8yac_swVw8I)
| Generated | 2026-05-04

## Description

Pi Coding Agent is the open source terminal coding tool built by Mario Zechner that's quietly winning over senior engineers.
🔗 My Links:
📩 Contact for collaboration: amish.sohaill@gmail.com
🎓 Learn Coding & AI: https://scrimba.com/?via=u440dbee (20% off)
🔗 Video Links:
Pi Docs: https://pi.dev/docs/latest
Pi GitHub: https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent
Pi Models: https://pi.dev/models
#PiCodingAgent #ClaudeCode #AIcoding #GitHubCopilot #OpenSource #DeveloperTools #AIagents #CodingTools #TerminalApps #VibeCoding

> Transcript extracted from YouTube auto-generated captions (`8yac_swVw8I.en.vtt`) and cleaned with `vtclean.py`. Timestamps have been removed; paragraph breaks follow sentence boundaries (or pauses > 1.5 s). Minor transcription artifacts from the automatic speech recognition may remain.

## Transcript

Pi is one of those rare tools that gets popular by doing less. It's a coding agent built by Mario Zechner, the same guy behind GDX library. And what's interesting is the fastest growing terminal coding agent right now is not the one with sub-agents, plan mode, or fancy MCP integrations.

It's this minimal little thing that ships with only four tools: read, write, edit, and bash. That's it. Mario built it because he was sick of Claude code becoming bloated. He noticed something a lot of us felt but didn't really say out loud. Every new feature added to these big agents made the behavior less predictable. Bugs piled up. Context got eaten silently. And you'd end up with a tool that does 50 things, but you only use five of them. So, instead of fighting it, he built his own, stripped to the bone, yellow by default. No permission prompts, no background bash, no swarm of sub-agents, just the model and a tiny harness around it. And honestly, I didn't think I'd switch.

I've been on open code for a while. I had my templates, my sub-agents, my whole setup. But after running Pi for a couple of weeks, I'm not going back. So, let me start with installation. Pi is on NPM. So, one command and you are done.

You type Pi, hit enter, and you're in.

The first thing you'll notice is the interface is small, like really small.

There's no banner, no ASCII art, no login wall blocking you, just a prompt and a footer telling you the model, the working directory, and the token usage.

That's the whole UI. Now for the provider part. This is where it gets fun. Pi supports almost everything: Anthropic, OpenAI, Google, DeepSeek, Cerebrus, Grok, Open Router, Bedrock, Azure, Vercel AI Gateway, even local stuff like Ollama or LM Studio. But the one I want to use today is GitHub Copilot. A lot of us already pay for Copilot Pro or have it through work. So, why not get more out of it? Pi can route your requests through your existing Copilot subscription, which means you can use models like GPT 5.1, Claude Sonnet, or Gemini without paying extra.

Setup is simple. Inside Pi, you type /login. A list shows up with the providers that support OAuth. Pick GitHub Copilot. It asks you to press enter for github.com or type a Copilot Enterprise domain if your company uses one. Most of you will just hit enter. It opens your browser, you authorize, and you're back in the terminal. The token is saved and Pi will refresh it automatically when it expires. After login, I run /model or hit control L to switch. I'll pick Claude Sonnet through Copilot for this test. The footer at the bottom updates and shows the active model. From here, anything I type just goes to the model. Every conversation is saved as a JSONL file.

You can resume any session with pi -r and pick from a list. You can also fork a session at any point. So, if I made a side trip into something dumb and want to roll back without losing my main thread, I just fork from the message I want and branch off. The session tree is real. It's not linear like most other agents. This is huge for keeping the context clean.

The second thing is skills. Pi reads skill.md files just like Claude code does. So, if you already have skills set up in your tilde/ .claude folder, Pi finds them. Same with project-level skills.

They show up at startup in the header.

There's also a community skills repo called pi-skills with stuff like a babysitter skill that double-checks the agent's work, a sub-agent skill, and others. To install one, you do pi install and the path. That's the whole thing.

The third is the agents.md file. You drop one in your project root and Pi loads it as context. I keep mine short.

Things like run npm run check after edits, do not touch migrations, keep responses tight, whatever rules I want.

If I change the file, I run /reload and it picks up the new version.

There's also bash escape. If I just want to run a quick shell command without leaving the agent, I type an exclamation mark and the command, like exclamation mark git status. It runs and shows the output inline.

And if I want to edit my prompt in a real editor instead of the terminal box, control G opens it in my system editor.

Small thing, but I use it constantly.

Okay, now the actual test. I want to see how Pi performs on a creative front-end task with a Copilot model behind it. So, my prompt is going to be straightforward. I'm going to tell it to build a 3D Rubik's Cube using three.js in a single HTML file. It needs to be interactive. You should be able to click and drag to rotate the whole cube, and the faces should be turnable with mouse drags, too. Keep it self-contained, no build step. I want to open the file in a browser and have it work. I paste the prompt in. Pi starts thinking. The thinking indicator shows in the editor border, which I find kind of nice because it's not in your face.

It reads the directory first using ls, sees there's nothing there, and then starts writing. The bash tool is what it uses to actually create the file. It pipes the HTML content through cat and writes it to disk. No fancy file edit tool needed for a fresh file. You can see how minimal the loop is.

The model decides what tool, Pi runs it.

The output comes back, and the loop continues. After about 90 seconds, it finishes. The file is on disk. I open it in Chrome. There's a black canvas with a 3x3 Rubik's Cube floating in the middle.

Six colors, white on top, yellow on bottom, red, orange, blue, green on the sides. I drag with my mouse and the whole cube spins, smooth. Then I click on a face and try to drag a row, and it actually rotates a single layer. The animation is decent, not perfect. The rotation snaps back to a 90° multiple when I let go, which is exactly what you'd want. What I want to highlight here is not that the cube works. Plenty of agents could build that. What I want to highlight is how quiet the experience was. No pop-ups asking me to approve every bash command. No plan mode making me confirm a 10-step roadmap before writing a single line. No sub-agent spinning up another model to second-guess the first. Just a model, a few tools, and a clean session file I can fork later if I want to try a different approach.

If I compare this to Claude code, the difference is mostly philosophical.

Claude code has hooks, plan mode, MCP servers, skills, sub-agents, the whole spaceship. It's powerful, but it's also opinionated, and you can't really turn most of it off. Open code sits in the middle. It has LSP support, an SST-style design, and decent extensibility. But it auto-compacts your context aggressively, and that has bitten me on long sessions where it summarized away things I needed. Pi just doesn't compact unless you tell it to. Your session is your session. Now, there's a trade-off. Pi gives you nothing out of the box beyond the basics. If you want a sub-agent system, you bring it. If you want LSP feedback, you write an extension or use a skill. If you want web search, you wire it up. The good news is the extension API is basically just TypeScript with event hooks. Session start, tool call, tool result, message added. You write a few lines, drop it in tilde/ .pi/agent/ extensions, and it loads. Peter Steinberger built Open Claw on top of it. The fastest growing thing on GitHub right now in this space is built on Pi.

And the reason is the same reason I'm sticking with it. Less stuff means less to break.

So, would I recommend it to you? If you like fiddling, yes. If you want a polished out-of-the-box experience with planning, sub-agents, and a bunch of integrations pre-installed, probably not. Pi is for people who want to own their setup. If that sounds like you, install it tonight, hook up your Copilot subscription, write a small agents.md, and just see how it feels. You can always go back to Claude code or Open code if it's not for you. But I think a lot of you are going to stay. That's it for this one. The link to the docs and the GitHub repo are in the description.
