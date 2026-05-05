# AFK Software Factory: Step-by-Step Process Recap

Based on Matt Pocock's walkthrough of his open-sourced AFK workflow using [Sandcastle](~/Code/ai-swe/tools/sandcastle/) and the [skills repo](~/Code/ai-swe/skills/mattpocock/).

---

## The Core Problem Being Solved

Running Claude Code AFK (away from keyboard) requires two things:
1. **No permission interruptions** — agents must run without prompting you
2. **Safety** — YOLO mode bypasses prompts but lets the agent do destructive things (e.g., delete your home directory, exfiltrate code)

The solution is sandboxed agents: isolated containers where the agent can do whatever it wants without touching the host system.

---

## Step 1: Install Sandcastle

**What:** Add the `@ai-hero/sandcastle` library to your project.

**Choice:** Install as a dev dependency (standard) vs. globally. Dev is preferred so the version is pinned per-project.

```bash
npm install --save-dev @ai-hero/sandcastle tsx
```

`tsx` is also needed to run TypeScript scripts directly (used later).

---

## Step 2: Run the Init Wizard

**What:** Scaffold the `.sandcastle/` directory with a Dockerfile, orchestrator, and prompt files.

```bash
npx sandcastle init
```

This is an interactive wizard with four decisions:

### Decision 1: Agent

| Option | Notes |
|---|---|
| **`claude-code`** ← Matt's choice | Anthropic's Claude Code CLI |
| `codex` | OpenAI Codex |
| `pi` | — |
| `opencode` | — |

You can mix agents later (e.g., Claude for planning, Codex for implementation) — this is just the default.

### Decision 2: Sandbox Provider

| Option | Notes |
|---|---|
| **`docker`** ← Matt's choice | Local Docker container; most common |
| `podman` | Rootless alternative to Docker |
| `vercel` | Cloud Firecracker microVMs; stronger isolation |
| `no-sandbox` | No isolation; interactive mode only, not AFK-safe |

The sandbox is the core safety mechanism. Docker is the practical default for local dev.

### Decision 3: Backlog Manager

| Option | Notes |
|---|---|
| **`github-issues`** ← Matt's choice | Agents pick up issues labeled `sandcastle` |
| `beads` | Local file-based task list; works offline, no GitHub needed |

After choosing GitHub Issues, the wizard offers to create a `sandcastle` GitHub label on your repo. Confirm this — **only issues with this label are visible to the planner agent**.

### Decision 4: Template

| Template | What it does |
|---|---|
| `blank` | Bare scaffold — write everything yourself |
| `simple-loop` | One issue at a time: implement → close |
| `sequential-reviewer` | Implement → review → next issue |
| `parallel-planner` | Plan sub-issues → implement in parallel → merge |
| **`parallel-planner-with-review`** ← Matt's choice | Plan → parallel implement → per-branch review → merge |

Matt explicitly "maxes out" here. The parallel-with-review template is the most sophisticated: multiple agents run simultaneously and a reviewer checks each branch before the merger merges them all.

**What gets scaffolded:**

```
.sandcastle/
├── Dockerfile            # Sandbox environment definition
├── main.mts              # TypeScript orchestrator script
├── plan-prompt.md        # Planner agent instructions
├── implement-prompt.md   # Implementer agent instructions
├── review-prompt.md      # Reviewer agent instructions
├── merge-prompt.md       # Merger agent instructions
├── .env.example          # Required env var template
└── .gitignore            # Excludes .env and logs/
```

---

## Step 3: Understand the Dockerfile

**What:** The Dockerfile defines what's inside the sandbox where agents will run.

The scaffolded default installs:
- System dependencies (git, curl, etc.)
- GitHub CLI (`gh`) — so agents can read/write issues
- Claude Code — the agent binary
- Renames home directory to `/home/agent`

**Choice:** This is fully editable. You can add Node runtimes, Python, databases, project-specific tools — anything your codebase needs agents to have access to. The sandbox is just a Docker container.

---

## Step 4: Build the Docker Image

**What:** Compile the Dockerfile into a runnable container image. Must happen once before any agents can run, and again after any Dockerfile change.

```bash
npx sandcastle docker build-image
```

Optional: specify a custom image name:

```bash
npx sandcastle docker build-image --image-name sandcastle:my-project
```

---

## Step 5: Configure Environment Variables

**What:** Provide credentials the agents need inside the sandbox.

```bash
cp .sandcastle/.env.example .sandcastle/.env
```

Edit `.sandcastle/.env`:

```bash
ANTHROPIC_API_KEY=sk-ant-...   # Anthropic API key
GITHUB_TOKEN=ghp_...           # GitHub token (needs `repo` scope)
```

**Choice — API key vs. Claude subscription:** Anthropic restricts subscription use for automated workflows. Matt uses an API key. If you want to use a subscription, see the guidance linked in the sandcastle repo (issue #191 as of the talk).

`.env` is gitignored by the scaffolded `.gitignore` — credentials stay off the remote.

---

## Step 6: Add an npm Script

**What:** A convenience alias so you don't have to type the full `npx tsx ...` path each run.

In `package.json`:

```json
{
  "scripts": {
    "sandcastle": "npx tsx .sandcastle/main.mts"
  }
}
```

**Choice:** This is optional — you could call `npx tsx .sandcastle/main.mts` directly. The script just makes it ergonomic.

---

## Step 7: Commit and Push

**What:** Get the `.sandcastle/` config onto the remote so that agents (which run inside Docker and clone the repo) can access it.

```bash
git add .sandcastle/ package.json package-lock.json
git commit -m "Add Sandcastle configuration"
git push
```

The `.env` file is excluded by the scaffolded `.gitignore`.

---

## Step 8: Create a GitHub Issue

**What:** Write the task you want the agents to work on, as a GitHub issue with the `sandcastle` label.

Matt's demo issue:

> **Title:** Scaffold me a basic TypeScript template in the repo
>
> **Body:** Give me a basic TypeScript application that uses vitest, uses type checking, has a very simple CLI that I can call. Use commander for the CLI. Add a CI script that does type checking and runs the tests.

**Critical:** The issue must have the `sandcastle` label. Without it the planner ignores it.

**Choice — how to write the issue:** The more specific, the better the plan the planner generates. The template's `plan-prompt.md` picks up issue title, body, and all comments.

---

## Step 9: Run the Workflow

**What:** Launch the orchestrator, which triggers all four agent phases automatically.

```bash
npm run sandcastle
```

**What happens (in order):**

| Phase | Agent | What it does |
|---|---|---|
| 1. **Plan** | Planner | Reads open `sandcastle`-labeled issues, identifies unblocked ones, outputs a JSON plan of sub-issues wrapped in `<plan>` tags |
| 2. **Implement** | N implementers (parallel) | Each takes one sub-issue, works on its own branch (`agent/task-N`), encouraged to do red-green-refactor TDD |
| 3. **Review** | N reviewers (per branch) | Diffs each branch, checks correctness, improvements, and any project standards you've added to `review-prompt.md` |
| 4. **Merge** | Merger | Takes all branches, resolves merge conflicts (the reason an agent handles this rather than a script), merges to `main`, closes the original issue with a comment |

Logs are written to `.sandcastle/logs/`. The terminal prints a `tail -f` path and supports Ctrl+click to open logs in supported terminals.

**Choice — watch vs. go AFK:** You can follow the logs in real time or leave it entirely. There are no permission prompts because everything runs inside the sandbox.

---

## Step 10: Inspect the Results

**What:** Verify what the agents produced.

- New files are committed to `main`
- The original GitHub issue is closed with a comment from the merger agent
- Logs in `.sandcastle/logs/` capture the full run for review

In Matt's demo the reviewer found the code "already clean and well structured," the merger ran type checks, merged the branch, and closed the issue. The repo gained `tsconfig.json`, `vitest.config.ts`, and CLI source files.

---

## How the Orchestrator (`main.mts`) Works

The orchestrator is plain TypeScript — not a Sandcastle DSL. The core primitive is:

```typescript
await sandcastle.run({
  agent,    // which agent binary to use
  sandbox,  // which container/provider to run inside
  prompt,   // the instruction (inline string or promptFile path)
})
```

The template wires four of these calls together:

1. **Planner run** — extracts the `<plan>` JSON from the output
2. **`Promise.all(issues.map(...))`** — implementers in parallel, one per sub-issue
3. **Reviewer runs** — one per branch that produced commits (checked by `result.commits.length > 1`)
4. **Merger run** — receives all branches and issue IDs as `promptArgs`

Because it's just TypeScript, you can change anything: swap agents per-phase, add more phases, make PRs instead of direct merges, run as a cron job, etc.

---

## Prompt File Syntax

Two special syntaxes used in prompt `.md` files:

| Syntax | What it does |
|---|---|
| `{{KEY}}` | Substituted at runtime via `promptArgs` in the `run()` call |
| `` !`command` `` | Shell command executed inside the sandbox at prompt-resolution time; stdout replaces the expression |

Example from `review-prompt.md`:

```markdown
# Diff of changes

!`git diff {{TARGET_BRANCH}}...{{SOURCE_BRANCH}}`
```

The `` !`...` `` syntax is borrowed from the Claude skills repo.

---

## Key Design Philosophy

> "Sand Castle is not opinionated here."

Sandcastle provides the primitive (`run(agent, sandbox, prompt)`). The template provides a workflow that Matt found useful. Everything is editable:

- Swap any agent for any other agent (or mix them)
- Change the sandbox provider
- Edit any prompt file
- Restructure the orchestrator entirely

The factory pattern — plan → parallel implement → review → merge — is Matt's workflow, not a Sandcastle constraint.
