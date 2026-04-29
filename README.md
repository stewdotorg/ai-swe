# Research on AI-aided software engineering

Identifying promising tooling, process and wisdom for working with agentic AI

A collection of useful (and potentially useful) resources as my colleagues and I
build out our software engineering skillset and approach in the age of AI agents.

## Recomendations

### Matt Pocock's impressive agentic software engineering workflow, with tooling

#### The Process

Matt Pocock's talk ["Essential Skills for AI Coding from Planning to Production"][pocock-talk]
is a 2-hour deep-dive into his methodology for shipping production features
with autonomous AI agents. An excellent, detailed AI-generated summary lives in
[`reading/matt-pocock-ai-coding-workflow-summary.md`](reading/matt-pocock-ai-coding-workflow-summary.md).

It is the most impressive, fully-fleshed-out agentic workflow recipe I have
seen to date. It is fully open and easy to implement with his [`sandcastle`](tools/sandcastle)
tool and a couple of his key [skills](skills/mattpocock/README.md), such as
the famous [`grill-me`](skills/mattpocock/grill-me).

This workflow enables AFK agent development, in parallel where possible, while
retaining full human stewardship of and responsibility for software and
product design decisions.

#### The Skills

The repo includes Pocock's [agent skills](skills/mattpocock/README.md) as a
submodule — ready to be dropped into any coding agent that supports them.

#### The Tooling

##### Sandcastle

[Sandcastle](tools/sandcastle/README.md) is a TypeScript library for
orchestrating AI coding agents inside isolated Docker, Podman, or
Vercel sandboxes. It handles git worktrees, branch strategies, and merging so
you can run agents in parallel — each on its own branch, each in its own
container — then merge the results.

## Setup

### How to use this repo - git submodules

This repo references several submodules that serve as read-only research material
and AI context. No development or contribution to the submodules is expected.

**Cloning (first time):**

```bash
git clone --recurse-submodules git@github.com:stew/ai-swe.git
```

If you already cloned without `--recurse-submodules`, run:

```bash
git submodule update --init --recursive
```

**Updating all submodules to latest:**

```bash
git submodule update --remote --recursive
```

This fetches the latest commits from each submodule's tracked branch. If you
want to lock back to the pinned commits stored in this repo, omit `--remote`.

**Why submodules?**
These external repos evolve independently. Submodules pin them at known-good
revisions so that AI tools and readers get a consistent snapshot. When there's
new material worth surfacing, bump the pins deliberately.

[pocock-talk]: https://www.youtube.com/watch?v=-QFHIoCo-Ko&list=PLgWBLnrVaSruEA5HL8Yw0MNRfVmiuDhgJ
