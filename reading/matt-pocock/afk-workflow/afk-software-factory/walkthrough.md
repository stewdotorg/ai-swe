# Getting Started: Parallel Planner With Review (Matt's Demo Flow)

This guide walks through setting up Sandcastle in an existing repo to replicate the exact workflow Matt Pocock demonstrated: a **parallel planner with review** that picks up GitHub issues, plans sub-tasks, implements them on separate branches, reviews each one, and merges everything back to `main` — all AFK.

We'll follow Matt's exact choices: Claude Code as the agent, Docker as the sandbox provider, and GitHub Issues as the backlog manager.

**Prerequisites:**
- [Git](https://git-scm.com/)
- [Docker Desktop](https://www.docker.com/) installed and running
- [Node.js](https://nodejs.org/) (18+ recommended)
- A GitHub repo (with a remote configured) that you can push to and create issues on

---

## Step-by-Step Walkthrough

### Step 1: Install Sandcastle

```bash
npm install --save-dev @ai-hero/sandcastle
```

This adds `@ai-hero/sandcastle` to your `devDependencies`. You'll also need `tsx` to run TypeScript scripts directly:

```bash
npm install --save-dev tsx
```

---

### Step 2: Initialize Sandcastle

```bash
npx sandcastle init
```

This is an interactive wizard. For Matt's exact flow, choose:

| Prompt | Choice |
|---|---|
| **Select an agent** | `claude-code` |
| **Select a sandbox provider** | `docker` |
| **Select a backlog manager** | `github-issues` |
| **Select a template** | `parallel-planner-with-review` |

After selecting the backlog manager, you'll be asked to **create a `sandcastle` GitHub label**. Confirm this — the label is how the planner knows which issues to pick up. Only issues with this label will be considered by the agent.

**What gets scaffolded:**

```
.sandcastle/
├── Dockerfile          # Docker image definition for the sandbox
├── main.mts            # TypeScript orchestrator (or main.ts if package.json has "type": "module")
├── plan-prompt.md      # Prompt for the planner agent
├── implement-prompt.md # Prompt for implementer agents
├── review-prompt.md    # Prompt for reviewer agents
├── merge-prompt.md     # Prompt for the merger agent
├── .env.example        # Template for required environment variables
└── .gitignore          # Ignores .env and logs/
```

> **Note:** If your `package.json` has `"type": "module"`, the orchestrator file is named `main.ts` instead of `main.mts`. The content is identical.

---

### Step 3: Understand the Scaffolded Files

Before running, let's look at what each file does.

#### Dockerfile

This defines the sandbox environment. The scaffolded version installs:

- System dependencies (git, curl, etc.)
- **GitHub CLI** (`gh`) — needed so agents can read issues, add comments, and close them
- **Claude Code** — the agent binary
- Renames the home directory to `/home/agent`

You can customize this to install anything your project needs (language runtimes, package managers, databases, etc.). More on that in the [customization section](#customizing-the-dockerfile).

#### prompt files

These are markdown files that instruct each agent. They contain:
- **`{{KEY}}` placeholders** — substituted at runtime via `promptArgs`
- **`` !`command` `` shell expressions** — the command runs inside the sandbox and its stdout replaces the block

Here's what each prompt does in the scaffolded template:

| File | Agent Role | What It Does |
|---|---|---|
| `plan-prompt.md` | Planner | Reads all open issues labeled `sandcastle`, grabs their comments, identifies unblocked ones, and outputs a JSON plan of sub-issues wrapped in `<plan>` tags |
| `implement-prompt.md` | Implementer | Works on one sub-issue at a time, on a specific branch. Encourages red-green-refactor TDD |
| `review-prompt.md` | Reviewer | Diffs the branch against the target, reviews for correctness, improvements, and adherence to project standards |
| `merge-prompt.md` | Merger | Takes all completed branches, resolves merge conflicts, merges back to main, and closes the original issue with a comment |

#### main.mts (the orchestrator)

This is the TypeScript script that orchestrates the entire flow:

1. **Planner** — runs `sandcastle.run()` with `plan-prompt.md`, extracts the JSON plan
2. **Implementers** — for each sub-issue, spawns a separate `sandcastle.run()` on its own branch (these run in parallel)
3. **Reviewers** — for each implementation that produced commits, runs a reviewer agent on that branch
4. **Merger** — takes all completed branches and merges them back to `main`

Because this is just TypeScript code, you can edit it freely to change the flow, add steps, or change how branches are handled.

---

### Step 4: Set Environment Variables

```bash
cp .sandcastle/.env.example .sandcastle/.env
```

Edit `.sandcastle/.env` and fill in:

```bash
ANTHROPIC_API_KEY=sk-ant-...   # Your Anthropic API key
GITHUB_TOKEN=ghp_...           # GitHub personal access token (needs repo scope)
```

**About the API key vs subscription:** Anthropic is "a little bit funny" about people using subscriptions for automated agent workflows. If you want to use your Claude subscription instead of an API key, see [sandcastle issue #191](https://github.com/mattpocock/sandcastle/issues/191) for up-to-date advice. For this walkthrough, use an API key.

> **GitHub token scopes:** The token needs at minimum `repo` scope (to read/write issues, create branches, push commits) if your repo is private; for public repos, `public_repo` is sufficient. Create one at https://github.com/settings/tokens.

---

### Step 5: Build the Docker Image

```bash
npx sandcastle docker build-image
```

This builds the Docker image from `.sandcastle/Dockerfile`. The image name defaults to `sandcastle:<repo-directory-name>`. You can specify a custom name with `--image-name`:

```bash
npx sandcastle docker build-image --image-name sandcastle:my-project
```

Wait for the build to complete. This needs to happen once (and again whenever you modify the Dockerfile).

---

### Step 6: Add an npm Script

Add a convenience script to your `package.json`:

```json
{
  "scripts": {
    "sandcastle": "npx tsx .sandcastle/main.mts"
  }
}
```

(If your scaffold produced `main.ts` instead of `main.mts`, use that filename.)

---

### Step 7: Commit and Push

Sandcastle needs a clean git state and a remote to push branches to. Commit the `.sandcastle/` directory (but **not** `.sandcastle/.env` — it's gitignored) and push:

```bash
git add .sandcastle/ package.json package-lock.json
git commit -m "Add Sandcastle configuration"
git push
```

---

### Step 8: Create a GitHub Issue

Go to your repo on GitHub and create a new issue. For Matt's demo, the issue was:

> **Title:** Scaffold me a basic TypeScript template in the repo
>
> **Body:**
> Give me a basic TypeScript application that uses vitest, uses type checking, has a very simple CLI that I can call. Use commander for the CLI. Add a CI script that does type checking and runs the tests.

**Crucially, add the `sandcastle` label** to this issue. Without it, the planner won't pick it up.

---

### Step 9: Run the Workflow

```bash
npm run sandcastle
```

You'll immediately see output. The planner agent kicks off first:

```
[sandcastle:planner] Setting up sandbox...
[sandcastle:planner] Running planner agent on Docker...
```

Sandcastle logs each run to `.sandcastle/logs/`. The terminal prints a `tail -f` command you can use to follow the logs in real time. You can also **Ctrl+click** the log path (in supported terminals) to open it.

**What happens in sequence:**

1. **Planner** reads the open `sandcastle`-labeled issue, determines it's unblocked, and outputs a JSON plan with sub-issues (e.g., "Set up TypeScript config", "Install vitest", "Scaffold CLI with commander", "Add CI script")
2. **Implementers** spawn in parallel — each in its own sandbox on its own branch (`agent/task-1`, `agent/task-2`, etc.). You'll see them running bash commands, installing deps, writing tests first (red), then implementing (green), then refactoring
3. **Reviewers** run on each branch that produced commits — they diff the changes and check for correctness, style, and project standards
4. **Merger** takes all the branches, resolves any conflicts, merges back to `main`, and closes the original GitHub issue with a comment

**While it's running, you can:**
- Watch the logs in real time
- Go get a cup of tea
- Work on something else

The entire flow runs without any permission prompts, because everything is sandboxed inside the Docker container.

---

### Step 10: Check the Results

Once complete:

- Your repo has new files on `main` (the TypeScript scaffold)
- The GitHub issue is closed with a comment from the agent
- Logs are available in `.sandcastle/logs/` for review

---

## Customization: Alternative Choices and How to Adapt

Every choice in the walkthrough above is swappable. Here's how to customize each layer.

### Choosing a Different Agent

During `sandcastle init`, you can select `codex`, `pi`, or `opencode` instead of `claude-code`. To change after init, edit `main.mts` — replace the agent import and usage:

```typescript
// Claude Code (default in this walkthrough)
import { run, claudeCode } from "@ai-hero/sandcastle";
const agent = claudeCode("claude-sonnet-4-6");

// Codex
import { run, codex } from "@ai-hero/sandcastle";
const agent = codex("gpt-5.4");

// pi
import { run, pi } from "@ai-hero/sandcastle";
const agent = pi("claude-sonnet-4-6");

// OpenCode
import { run, opencode } from "@ai-hero/sandcastle";
const agent = opencode("opencode/big-pickle");
```

You can even mix agents in the same workflow — use Claude Code for planning, Codex for implementation, and a different model for review. That's the power of the agnostic design:

```typescript
// Adversarial review: different agent reviews the work
const planResult = await run({
  agent: claudeCode("claude-opus-4-6"),
  sandbox: docker(),
  promptFile: ".sandcastle/plan-prompt.md",
});

const implResult = await run({
  agent: codex("gpt-5.4"),
  sandbox: docker(),
  promptFile: ".sandcastle/implement-prompt.md",
  promptArgs: { ISSUE_TITLE: "Fix bug #42", TASK_ID: "42" },
});

const reviewResult = await run({
  agent: claudeCode("claude-sonnet-4-6"),
  sandbox: docker(),
  promptFile: ".sandcastle/review-prompt.md",
});
```

Agent-specific options:

```typescript
// Claude Code: reasoning effort
claudeCode("claude-opus-4-6", { effort: "high" })

// Claude Code: disable session capture (e.g. for sensitive repos)
claudeCode("claude-opus-4-6", { captureSessions: false })

// Codex: reasoning effort
codex("gpt-5.4", { effort: "xhigh" })
```

---

### Choosing a Different Sandbox Provider

| Provider | Import | Best For |
|---|---|---|
| Docker | `@ai-hero/sandcastle/sandboxes/docker` | Local dev, most common |
| Podman | `@ai-hero/sandcastle/sandboxes/podman` | Rootless container runtime |
| Vercel | `@ai-hero/sandcastle/sandboxes/vercel` | Cloud Firecracker microVMs |
| No-sandbox | `@ai-hero/sandcastle/sandboxes/no-sandbox` | Interactive mode only (not AFK) |

Swap the import in `main.mts`:

```typescript
// Docker (walkthrough default)
import { docker } from "@ai-hero/sandcastle/sandboxes/docker";

// Podman
import { podman } from "@ai-hero/sandcastle/sandboxes/podman";

// Vercel (Firecracker microVMs — stronger isolation)
import { vercel } from "@ai-hero/sandcastle/sandboxes/vercel";

// Usage is identical
await run({
  agent: claudeCode("claude-sonnet-4-6"),
  sandbox: podman(),  // or vercel(), or docker()
  prompt: "...",
});
```

**Provider-specific options:**

```typescript
docker({
  imageName: "sandcastle:my-project",
  // Mount host directories (e.g. npm cache for faster installs)
  mounts: [
    { hostPath: "~/.npm", sandboxPath: "/home/agent/.npm", readonly: true },
  ],
  // Provider-level env vars
  env: { DOCKER_SPECIFIC: "value" },
  // Attach to Docker networks
  network: "my-network",
})
```

---

### Choosing a Different Backlog Manager

During init, you can choose **Beads** (local file-based tasks) instead of GitHub Issues. This is useful for offline work or repos without GitHub. If you need a different tracker (Linear, Jira, etc.), you can implement your own prompt logic — the prompt files are just markdown. Your team writes how tasks are discovered.

---

### Choosing a Different Template

The five templates, from simplest to most sophisticated:

| Template | What It Does | Best For |
|---|---|---|
| `blank` | Bare scaffold — write your own prompt and orchestrator | Total control, unique workflows |
| `simple-loop` | Picks GitHub issues one by one, implements, closes | Single-task fire-and-forget |
| `sequential-reviewer` | Implements one issue, then reviews it, then next | Quality-critical work where review matters |
| `parallel-planner` | Plans sub-issues, implements in parallel, merges | Speed — no review step |
| `parallel-planner-with-review` | Plans, parallel implements, per-branch review, merges | Speed + quality (Matt's choice) |

If you've already initialized with one template and want another, re-run `sandcastle init` with `--template`:

```bash
npx sandcastle init --template blank
```

(Note: `sandcastle init` errors if `.sandcastle/` already exists. Move or delete the existing one first.)

---

### Customizing the Dockerfile

The Dockerfile is yours to modify. Common additions:

```dockerfile
# Add Node.js (for a Node project)
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs

# Add Python (for a Python project)
RUN apt-get install -y python3 python3-pip

# Add project-specific tooling
RUN npm install -g pnpm

# Pre-install dependencies to speed up sandbox startup
# (these would then be available immediately)
```

After modifying, rebuild:

```bash
npx sandcastle docker build-image
```

---

### Customizing Branch Strategies

The branch strategy controls where agent changes land. The walkthrough uses the template's default — typically `branch` (each implementer gets `agent/task-{id}`). You can change this per-`run()` call:

```typescript
// Agent writes directly to the host working directory (no worktree)
await run({
  branchStrategy: { type: "head" },
  // ...
});

// Temp branch auto-merged to HEAD on completion
await run({
  branchStrategy: { type: "merge-to-head" },
  // ...
});

// Explicitly named branch (the template's pattern)
await run({
  branchStrategy: { type: "branch", branch: "agent/fix-42" },
  // ...
});
```

> **Note:** `head` strategy is not supported for isolated sandbox providers (Vercel).

---

### Customizing Prompt Files

The scaffolded prompts are starting points — edit them freely.

**Add project-specific coding standards** to `review-prompt.md`. Matt's scaffold includes a placeholder:

```markdown
## Coding Standards
<!-- Fill this in with your project standards -->
- No `any` types
- Prefer pure functions
- Tests must be colocated with source files
```

**Add dynamic context** with `` !`command` ``:

```markdown
# Current state of the repo

!`git log --oneline -20`

# Open issues

!`gh issue list --state open --label sandcastle --json number,title,body,comments,labels --limit 20`

# Test results before starting

!`npx vitest run --reporter=verbose 2>&1 || true`
```

**Use `{{KEY}}` placeholders** for runtime substitution via `promptArgs`:

```typescript
await run({
  promptFile: ".sandcastle/implement-prompt.md",
  promptArgs: {
    ISSUE_TITLE: "Fix login bug",
    ISSUE_NUMBER: "42",
    PRIORITY: "high",
  },
});
```

In the prompt:

```markdown
Work on: {{ISSUE_TITLE}} (#{{ISSUE_NUMBER}})
Priority: {{PRIORITY}}
```

**Built-in prompt arguments** are always available without passing them:

```markdown
You are working on branch {{SOURCE_BRANCH}}.
When diffing, compare against {{TARGET_BRANCH}}.
```

---

### Using `createSandbox()` for Multi-Run Workflows

Instead of one-shot `run()`, use `createSandbox()` when you need the same agent (or multiple agents) to work in the same container:

```typescript
import { createSandbox, claudeCode } from "@ai-hero/sandcastle";
import { docker } from "@ai-hero/sandcastle/sandboxes/docker";

await using sandbox = await createSandbox({
  branch: "agent/fix-42",
  sandbox: docker(),
  hooks: {
    sandbox: { onSandboxReady: [{ command: "npm install" }] },
  },
});

// Step 1: implement
const implResult = await sandbox.run({
  agent: claudeCode("claude-opus-4-6"),
  promptFile: ".sandcastle/implement.md",
  maxIterations: 5,
});

// Step 2: review on the same branch, same container
const reviewResult = await sandbox.run({
  agent: claudeCode("claude-sonnet-4-6"),
  prompt: "Review the changes and fix any issues.",
});

// Auto-cleanup via `await using`
```

This is more efficient than repeated `run()` calls because the container stays alive and dependencies remain installed.

---

### Using `createWorktree()` for Interactive-First Workflows

When you want to explore interactively before going AFK:

```typescript
import { createWorktree, claudeCode } from "@ai-hero/sandcastle";
import { docker } from "@ai-hero/sandcastle/sandboxes/docker";

await using wt = await createWorktree({
  branchStrategy: { type: "branch", branch: "agent/fix-42" },
});

// Interactive session — defaults to no sandbox, permission prompts active
await wt.interactive({
  agent: claudeCode("claude-opus-4-6"),
  prompt: "Explore the codebase and understand the bug.",
});

// Then go AFK in the same worktree
const result = await wt.run({
  agent: claudeCode("claude-sonnet-4-6"),
  sandbox: docker(),
  prompt: "Fix the bug we just explored.",
  maxIterations: 3,
});
```

---

### Configuring Iteration Control

Control how long each agent runs:

```typescript
await run({
  maxIterations: 10,           // Maximum agent loops (default: 1)
  idleTimeoutSeconds: 300,     // Timeout if agent is silent for 5 min (default: 600)
  completionSignal: "DONE",    // Agent emits "DONE" to stop early
  // Or multiple signals:
  completionSignal: ["TASK_COMPLETE", "TASK_ABORTED"],
});
```

---

### Adding Lifecycle Hooks

Run setup commands before the agent starts:

```typescript
await run({
  hooks: {
    host: {
      onWorktreeReady: [
        { command: "cp .env.example .env" },
        { command: "echo 'Worktree ready'" },
      ],
      onSandboxReady: [
        { command: "echo 'Sandbox ready'" },
      ],
    },
    sandbox: {
      onSandboxReady: [
        { command: "npm install" },
        { command: "npx prisma generate" },
      ],
    },
  },
});
```

**Execution order:** `copyToWorktree` → `host.onWorktreeReady` (sequential) → sandbox created → `host.onSandboxReady` + `sandbox.onSandboxReady` (parallel).

---

### Changing Logging Behavior

```typescript
// File logging (default) — writes to .sandcastle/logs/
await run({
  logging: { type: "file", path: ".sandcastle/logs/my-run.log" },
});

// Terminal mode — spinners and styled output directly in the terminal
await run({
  logging: { type: "stdout" },
});

// Forward agent events to your own observability system
await run({
  logging: {
    type: "file",
    path: ".sandcastle/logs/my-run.log",
    onAgentStreamEvent: (event) => {
      // event = { type: "text" | "toolCall", iteration, timestamp, ... }
      myLogger.info(event);
    },
  },
});
```

---

### Resuming a Prior Session

If an agent session was interrupted, resume it:

```typescript
await run({
  agent: claudeCode("claude-opus-4-6"),
  sandbox: docker(),
  prompt: "Continue where you left off",
  resumeSession: "abc-123-def",  // Claude Code session ID
  maxIterations: 1,              // Required: cannot be > 1 with resume
});
```

Session JSONL files are automatically captured to `~/.claude/projects/` after each iteration.

---

### Writing a Custom Sandbox Provider

If Docker/Podman/Vercel don't fit, implement your own. There are two kinds:

**Bind-mount provider** (your environment can mount host directories):
```typescript
import { createBindMountSandboxProvider } from "@ai-hero/sandcastle";

const myProvider = createBindMountSandboxProvider({
  // ... implementation
});
```

**Isolated provider** (separate filesystem, e.g., a cloud VM):
```typescript
import { createIsolatedSandboxProvider } from "@ai-hero/sandcastle";

const myProvider = createIsolatedSandboxProvider({
  // ... implementation must handle copyIn, copyFileOut, extractCommits
});
```

---

### Rolling Your Own Orchestrator

The scaffolded `main.mts` is a starting point. Because Sandcastle is just a TypeScript library, you can build entirely custom workflows:

- **Competitive implementation** — spawn 3 agents on the same task, pick the best result
- **Blended review** — reviewer takes all branches and creates a hybrid solution
- **PR-based flow** — create actual GitHub PRs instead of merging directly
- **CI integration** — run Sandcastle as a GitHub Actions job
- **Scheduled agents** — run via cron to pick up new issues nightly

Everything is composable because the primitive is just `sandcastle.run({ agent, sandbox, prompt })`.
