# Matt Pocock's Agentic Workflow — Detailed Reproduction Guide

---

## High-Level Flow

| Step | Stage | Human Role | Agent Role |
|------|-------|-------------|-------------|
| 1 | **Idea / Input** | Human-in-the-loop | — |
| 2 | **Grill Me Session** | Answer questions; reach shared understanding | Explore codebase; ask one question at a time with recommendations |
| 3 | **Write PRD** (Destination) | Review proposed modules; sanity-check module boundaries | Summarise grilling session into a PRD with user stories, implementation decisions, out-of-scope |
| 4 | **Break into Issues** (Journey) | Enforce vertical slices; review blocking relationships | Propose independently-grabbable Kanban tickets with dependency graph |
| 5 | **AFK Implementation** | Away from keyboard | Pick next unblocked AFK task; TDD cycle; run feedback loops |
| 6 | **Automated Review** | Away from keyboard | Fresh-context review against coding standards |
| 7 | **Human QA** | Human-in-the-loop | — |
| 8 | **Loop back to Kanban** | File new issues from QA findings | — |

> The boundary between "day shift" (human) and "night shift" (AFK) is **between steps 4 and 5**. Steps 1–4 are the day shift where humans plan and queue up work. Steps 5–6 are the night shift where agents execute unattended. Steps 7–8 are the handover back to humans.

**Not linear in practice**: real projects bounce between these phases — a prototype may loop back to the grilling session; QA may spawn new issues that re-enter the Kanban board; architecture improvements run in parallel.

---

## Step 1: Idea / Input

**Human responsibility:** Gather the raw material.

Inputs can be:
- A Slack message from a stakeholder
- A meeting transcript (e.g. from Gemini or similar)
- A client brief
- A bug report
- A feature request in any form

The input can be vague. A single sentence like *"retention numbers are bad, let's add gamification"* is enough to start.

**Key principle:** You do not need a fully-baked spec. The grilling session will excavate the details.

---

## Step 2: Grill Me Session

**Human role:** Active participant — answer questions, make decisions.  
**Agent role:** Explorer + relentless interviewer.

### Purpose

To reach a **shared design concept** with the AI (term from Frederick P. Brooks' *The Design of Design*). This is not about producing a document — it's about getting on the same wavelength. The conversation history *itself* becomes the valuable asset.

### How to use it

1. **Clear your context** first. Start fresh so you begin in the smart zone.
2. Invoke the grill me skill with the raw input:
   ```
   /grill-me <paste the client brief or meeting notes>
   ```
3. The agent first **explores the codebase** via a sub-agent (isolated context, returns a summary to you). This burns tokens in the sub-agent, not your context.
4. The agent then asks you **one question at a time**, each with a recommended answer. Example questions:
   - *"What actions earn points and how much?"*
   - *"Should points be retroactive for existing lesson progress?"*
   - *"What's the level progression curve?"*
5. You answer each question. You can accept the recommendation, override it, or ask for clarification.
6. Sessions typically run **20–80+ questions**. They can last an hour.

### The skill prompt (core text)

```
Interview me relentlessly about every aspect of this plan until we reach
a shared understanding. Walk down each branch of the design tree,
resolving dependencies one by one. For each question, provide your
recommended answer. Ask the questions one at a time.
```

### Tips

- **Dictate your answers** if possible — it's faster and keeps you in flow.
- If the grilling goes too long, tune the skill to include stop conditions.
- The conversation history is the asset. You'll feed it into the next step.
- If you get stuck on a question (domain knowledge gap), **pull in a teammate or domain expert** — treat it as pair/mob programming with AI as the third participant.

### Why not skip this?

Without this step, the AI eagerly jumps to producing a plan before it actually understands what you want. You end up fighting the plan instead of building the right thing. The grilling session **forces alignment before any artifact is created**.

---

## Step 3: Write the PRD (Destination Document)

**Human role:** Review proposed modules; do **not** read the full PRD.  
**Agent role:** Summarise the grilling session into a structured PRD.

### Purpose

Capture the shared design concept in a **destination document** — a description of *where we're going*. It defines the shape of the feature and provides a definition of done.

### How to use it

1. Invoke the "write a PRD" skill:
   ```
   /write-a-prd
   ```
   (Alternatively, simply say: "Write a PRD based on our conversation.")
2. The agent proposes **a set of modules it intends to modify**. **This is the part the human must review.** You are checking that the module boundaries make sense and that the agent is thinking in terms of deep modules (small interface, lots of internal logic) rather than shallow modules (many tiny files).
3. The agent produces the PRD. Typical structure:
   - **Problem statement** — what the user is facing
   - **Solution** — high-level approach
   - **User stories** (e.g. 10–20 stories)
   - **Implementation decisions** — specific choices made (technology, patterns)
   - **Testing decisions** — what and how to test
   - **Out of scope** — negative decisions and rationale (critical for definition of done)
4. **Do not read the PRD in detail.** Matt explicitly says he doesn't read these. Why? Because:
   - LLMs are excellent at summarisation — you'd just be testing that capability
   - You already achieved alignment in the grilling session
   - The PRD is a capture, not a contract

### Where to store it

- GitHub issue (Matt's preferred approach — his work repo has 744+ closed issues)
- Local markdown file in an `issues/` directory
- Whatever your team already uses

### Why this order?

Grilling first, PRD second. You *can* use "write a PRD" without grilling, but then the skill itself includes its own mini-grilling session. Starting with the dedicated grilling step gives you better alignment before the summarisation happens.

---

## Step 4: Break the PRD into Issues (Journey / Kanban Board)

**Human role:** Enforce vertical slices. Review dependency graph.  
**Agent role:** Propose independently-grabbable tickets with blocking relationships.

### Purpose

Turn the destination (PRD) into a **journey** — a set of discrete, parallelisable tasks that each fit within the smart zone (~100K tokens) and can be executed by AFK agents.

### How to use it

1. From the PRD, invoke the "PRD to issues" skill (or simply instruct: "Break this PRD into independently grabbable issues with blocking relationships.")
2. The agent proposes a **Kanban board** — a set of tickets like:
   ```
   Issue 01: Schema + gamification service     [blocked by: nothing]  [type: AFK]
   Issue 02: Streak tracking                    [blocked by: nothing]  [type: AFK]
   Issue 03: Wire points/streaks into lessons   [blocked by: 01, 02]   [type: AFK]
   Issue 04: Retroactive backfill              [blocked by: 01]       [type: AFK]
   Issue 05: Dashboard + leaderboard UI         [blocked by: 01, 02, 03, 04] [type: AFK]
   ```
3. **Human review step — this is critical.** You are checking for:

   ### The vertical slice problem (most important)
   
   AI naturally codes **horizontally** (layer by layer):
   - Phase 1: do all schema/database work
   - Phase 2: do all API/service work  
   - Phase 3: do all frontend work
   
   This is **bad** because you get zero integrated feedback until Phase 3. The agent is coding blind.
   
   Instead, enforce **vertical slices** (tracer bullets from *The Pragmatic Programmer*): each ticket should cut through all necessary layers and produce something visible and testable. A good first slice might be: *"Award points for lesson completion, visible on dashboard"* — this touches schema, service, routes, and frontend in one ticket.
   
   **How to correct:** If the first ticket is too horizontal ("create the gamification service" in isolation), push back: *"The first slice is too horizontal. I want a vertical slice that shows points on the dashboard after lesson completion."*
   
   ### Blocking relationships
   
   Check that the dependency graph is correct — this determines how much parallelisation you can achieve. Tickets with no blockers can run simultaneously. The graph should form a **directed acyclic graph (DAG)** with natural phases.

4. Store each issue as a markdown file or GitHub issue. Tag each as `AFK` or `human-in-the-loop`.

### Why Kanban over a sequential plan?

A sequential plan (Phase 1 → Phase 2 → Phase 3 → Phase 4) can only be picked up by **one agent** in a single loop. A Kanban board with blocking relationships allows **multiple agents to work in parallel** on unblocked tickets. Phase 1 might be 1 ticket, Phase 2 might be 2 parallel tickets, Phase 3 might be 1 ticket that depends on all previous.

---

## Step 5: AFK Implementation (The "Ralph Loop")

**Human role:** Away from keyboard.  
**Agent role:** Pick next AFK task, implement with TDD, run feedback loops, self-review.

### Purpose

Execute the Kanban board without human intervention. The agent works through AFK-tagged tickets one at a time (or in parallel, in advanced setups), producing commit-level outputs.

### The prompt structure

A typical Ralph prompt contains:

```
You are working on a codebase. Here are all the issues in the backlog:
<all issue markdown files>

Here are the last 5 commits for context:
<git log>

Instructions:
- Only work on AFK issues
- If all AFK tasks are complete, output "NO_MORE_TASKS"
- Prioritisation order: critical bug fixes → dev infrastructure → tracer bullets → quick wins → refactors
- For each task:
  1. Explore the repo (use sub-agents)
  2. Use TDD to complete the task (red-green-refactor)
  3. Run feedback loops (tests, type-check, lint)
  4. Commit with a descriptive message
```

### The TDD cycle (red-green-refactor)

This is Matt's **most important implementation technique**. The agent must:

1. **Red:** Write a failing test first. Isolate the smallest testable behaviour.
2. **Green:** Write the minimum implementation to make the test pass.
3. **Refactor:** Clean up the code while tests stay green.

**Why TDD matters for agents:**

- **Prevents cheating.** Without TDD, agents tend to write all the implementation first, then write weak tests that just mirror the implementation. TDD forces the test to exist *before* the code it tests.
- **Adds real tests to the codebase.** The tests tend to be good because the agent had to think about behaviour before implementation.
- **Creates tight feedback loops.** Red → green happens in seconds, not minutes.

### Feedback loops

After each TDD cycle, the agent must run:

- `npm run test` (or equivalent)
- `npm run typecheck` (TypeScript)
- `npm run lint`

The **quality of your feedback loops is the ceiling for AI code quality.** If your tests are slow, flaky, or non-existent, the agent will produce bad code. Invest here first.

### Simple execution (sequential)

A bash script (`ralph-once.sh`) that:

```bash
ISSUES=$(cat issues/*.md)
LAST_COMMITS=$(git log --oneline -5)
PROMPT=$(cat prompt.md)

claude --permission-mode accept-edits \
  --prompt "$PROMPT" \
  --var issues="$ISSUES" \
  --var commits="$LAST_COMMITS"
```

Run this once to test. The full loop wraps this in a `while` that re-runs until the agent outputs `NO_MORE_TASKS`.

### Advanced execution (parallel — Sand Castle)

Matt built a TypeScript library called **Sand Castle** for parallel execution:

1. **Planner agent** reads the Kanban board, identifies unblocked issues, selects N to run in parallel.
2. For each selected issue, **create a git worktree** in a Docker sandbox.
3. **Implementer agents** run in parallel in isolated sandboxes, each working on one issue.
4. **Reviewer agent** reviews each branch's commits (fresh context = smart zone review). Use a smarter model for review (Opus) vs implementation (Sonnet).
5. **Merger agent** merges all branches, resolves conflicts, fixes any type/test failures.

This turns a 4-phase sequential plan into a 3-wave parallel execution.

---

## Step 6: Automated Review

**Human role:** Away from keyboard.  
**Agent role:** Review code in a fresh context against coding standards.

### Purpose

Catch bugs and standards violations before human eyes see the code. Because the reviewer runs in a **fresh context** (not the same session as the implementer), it reviews in the smart zone, not the dumb zone.

### Push vs. Pull for coding standards

- **Implementer:** Coding standards should be available via **pull** (skills it can invoke when needed). Don't bloat the implementer's context.
- **Reviewer:** Coding standards should be **pushed** (included directly in the prompt). Give the reviewer both the code *and* the standards to compare against.

### Model selection

- Use a **faster/cheaper model** for implementation (e.g. Sonnet)
- Use a **smarter model** for review (e.g. Opus) — review requires more discrimination

---

## Step 7: Human QA

**Human role:** Manual testing, taste-imposition, opinion.  
**Agent role:** None.

### Purpose

This is where the human **imposes taste**. QA is not just about finding bugs — it's about ensuring the output has quality, polish, and doesn't feel like "slop." Automation without this step produces generic, soulless apps.

### What to do

- Manual testing of the end-to-end flow (login as a user, click through the feature)
- Review tests first (do they test reasonable things?), then review the implementation
- Look for module structure — is the agent producing deep modules or shallow sprawl?
- Check that the output matches the design concept from the grilling session

### Key insight: QA creates new issues

Any bug, rough edge, missing behaviour, or architectural concern found during QA should be filed as a **new issue on the Kanban board**. This feeds back into the implementation loop. The Kanban board is a living structure that grows during QA.

---

## Step 8: Loop Back

The Kanban board is not static. During QA and beyond:

- New blocking issues are added
- The agent picks them up in the next AFK cycle
- This creates a continuous "plan → implement → QA → plan" cycle

When all issues are closed and QA is satisfied, the work is ready for team review / PR.

---

## Side Processes

These run in parallel with or before the main flow.

### Prototypes (for frontend / uncertain work)

- AI can't visually evaluate frontends well (no eyes)
- Use AI to generate **throwaway prototype routes** — e.g. 3 different UI approaches for the same feature
- A human clicks through them and picks the best
- Feed the chosen prototype back into the grilling session or PRD
- Prototypes may need their own mini-PRDs

### Research (third-party libraries, APIs)

- May happen between grilling and PRD, or between PRD and issues
- Research findings feed back into the design concept

### Improve Codebase Architecture (ongoing)

- Run the "improve codebase architecture" skill periodically
- It scans the codebase and identifies **clusters of tightly-coupled modules** that should be consolidated into deep modules
- Produces candidates like: *"Quiz scoring service + related modules — currently zero tests, this is the biggest gap"*
- Deep modules = small interface, lots of internal logic, easy to wrap in a test boundary
- This is an ongoing activity, not tied to any specific feature

---

## Key Principles (Summary)

1. **Smart zone discipline.** Every task must fit in ~100K tokens. If it doesn't, split it further.
2. **Clear, don't compact.** Reset context between phases. Deterministic state beats accumulated history.
3. **Alignment over artifacts.** The grilling session is more valuable than the PRD it produces.
4. **Vertical slices, not horizontal layers.** Every ticket should produce something integrated and testable.
5. **TDD is non-negotiable for AFK work.** It's the feedback loop that keeps agents honest.
6. **Feedback loop quality = code quality ceiling.** Fast, reliable tests, types, and linting are prerequisites.
7. **Deep modules over shallow modules.** Guide the agent toward consolidated modules with small interfaces.
8. **Human taste at QA.** Automation up to implementation, human judgment at review. Don't skip this or you get slop.
9. **Own your stack.** No framework is the clear winner. Use simple, observable tools you can debug.
10. **Old books are prompt gold.** *The Pragmatic Programmer*, *Refactoring*, *The Philosophy of Software Design* — these verbalised best practices 20+ years ago in language you can drop directly into prompts.
