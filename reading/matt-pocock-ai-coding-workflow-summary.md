# Summary: "Essential Skills for AI Coding from Planning to Production" — Matt Pocock

This is an AI-generated summary of a [talk/workshop by Matt Pocock][video]
(@mattpocockuk) at an AI Engineer Europe event, ~2 hours, covering his
methodology and philosophy for software engineering workflows with AI agents.

> **Apr 24, 2026**
>
> A hands-on workshop covering the full lifecycle of AI-assisted development,
> from turning ambiguous requirements into agent-ready plans to running
> autonomous coding agents that ship production features.
> 
> You'll learn to stress-test vague briefs into structured PRDs, slice work
> into thin "tracer bullet" vertical slices, and run an AI agent with TDD.
> You'll watch it select tasks, write tests, implement code, and commit.
> You'll then refine your prompts based on where it struggles, graduate to
> fully autonomous (AFK) runs, and learn to design codebases that maximize
> agent effectiveness.
> 
> You'll walk away knowing how to:
> 
> - Turn ambiguous requirements into agent-ready issues
> - Slice work into vertical tracer bullets an agent can grab independently
> - Run AI agents human-in-the-loop and autonomously with TDD
> - Design codebase architectures that AI agents love to work in
> 
> For: Engineers ready to move beyond chat-based AI assistance and build a
> real workflow for shipping features with autonomous coding agents.
> 
> Speaker info:
> 
> <https://x.com/mattpocockuk>
> <https://www.linkedin.com/in/mapocock/>
> <https://www.youtube.com/channel/ucswg6fsbgzjbwtdf_hmlaow>

---

## CORE THESIS

**Software engineering fundamentals that work with humans also work superbly with AI.** AI is a new paradigm, but the disciplines that make human teams effective — planning, alignment, testing, feedback loops, modular design — are exactly what make AI agents effective. Classic software engineering books are a "gold mine" for crafting AI prompts because they already verbalize best practices in English.

---

## KEY CONCEPT: LLM CONSTRAINTS

### 1. The Smart Zone / Dumb Zone (credit: Dex Hy of Human Layer)
- LLMs do their **best work** at the start of a fresh context window (~≤100K tokens) — this is the **smart zone**
- Beyond ~100K tokens, attention relationships become strained quadratically and the LLM enters the **dumb zone**, making progressively worse decisions
- **Implication:** Size tasks to stay within the smart zone. Break large work into chunks that each fit in the smart zone. The 1M token context window is "just more dumb zone shipped to you" — good for retrieval, less good for coding.

### 2. LLMs Are Like the Guy from Memento
- LLMs continually forget. Every new session resets to a base state (system prompt)
- **Implication:** Optimize for this. Pocock prefers **clearing context and starting fresh** over "compacting" (summarizing past conversation). A clean start = predictable baseline. This is a feature, not a bug.

### 3. Every LLM Session Goes Through Stages
- System prompt → Exploratory phase → Implementation → Testing/Feedback
- Keep the system prompt **tiny** to maximize precious smart zone tokens

### 4. Token Visibility Is Essential
- Always know exactly how many tokens you're using. Get close to 100K? Clear and restart.

---

## THE WORKFLOW (End-to-End Pipeline)

### Phase 1: Idea → Alignment (The "Grill Me" Skill)

**The "Grill Me" skill** is the cornerstone. It instructs the AI to:
- Interview the user relentlessly about every aspect of the plan
- Ask questions one at a time to reach shared understanding
- Walk down each branch of the design tree, resolving dependencies one by one
- For each question, provide a recommended answer

**Philosophical insight:** Pocock was inspired by **Frederick P. Brooks' *The Design of Design*** — the concept of a "design concept" as a shared idea between all participants. He realized he didn't need the AI to produce a *plan* (an asset). He needed to reach a **shared understanding** (alignment). This is the difference between "specs to code" (which he rejects) and true collaboration.

**Key dynamic:** The AI eagerly wants to jump to producing a plan. Pocock wants it to slow down and ask aggressive questions instead. The goal is not a document — it's being on the same wavelength.

**Practical uses:**
- Feed in Slack messages, meeting transcripts, client briefs
- Use it for pair/mob programming — AI as relentless questioner
- Can last 40, 80, even 100 questions
- You can tune the skill's intensity if it hammers you too hard

### Phase 2: Alignment → PRD (Product Requirements Document)

- The PRD is the **destination document** — it captures where you're going
- Contains: problem statement, solution, user stories, implementation decisions, test decisions
- **Crucially:** Pocock does NOT read these PRDs. Why? Because he's already achieved shared understanding via the grilling session. Reading the PRD would only be testing "the LLM's ability to summarize" — which LLMs are already great at. Skip it.

### Phase 3: PRD → Kanban Board (Breaking into Tracer Bullets)

- Instead of a sequential multi-phase plan, break the PRD into a **kanban board** of independently grabbable issues
- Use **vertical slices** (tracer bullets), NOT horizontal layers
- **Citation:** "Tracer bullets" from *The Pragmatic Programmer* — the idea that every Nth bullet glows so you can see where you're aiming
- **Why vertical?** Horizontal coding (do all DB first, then all API, then all frontend) means no integrated feedback until the end. Vertical slices give near-instant feedback that all layers work together.
- AI naturally wants to code horizontally (layer by layer). You must direct it to code vertically.
- Each issue has blocking relationships, creating a DAG (directed acyclic graph) that enables parallelization

### Phase 4: Implementation (The "Night Shift" / AFK Agent)

This is where the human **leaves the loop**.

**Day Shift (Human):** Idea → Grill → PRD → Kanban → Review the board  
**Night Shift (AI / AFK):** Pick tasks → Implement → Auto-review → Merge

**The Ralph Loop** (named after Ralph Wiggum from The Simpsons — "make a small change toward the destination"):
- A bash script that feeds all issue files, last N commits, and a prompt into Claude Code running in permission mode
- Runs in Docker sandbox for safety
- The prompt specifies: explore repo → use TDD → complete task → run feedback loops → report

**Two types of tasks:**
- **Human-in-the-loop:** Planning, alignment, QA — humans must be present
- **AFK (Away From Keyboard):** Implementation — can be fully delegated

### Phase 5: QA & Code Review

- After implementation: human QA (manual testing) + code review
- QA is where you **impose your taste**. Automating everything produces "slop" — apps that work but lack quality, taste, and human touch
- The automated reviewer (separate, fresh context in the smart zone) catches tons of bugs
- Use Sonnet for implementation, Opus for review (review needs the "smarts")
- **Honest admission:** AI delegation means doing more code review, which "is not fun" but is "the way things are going"

### Push vs. Pull for Coding Standards
- **Push:** Always sent to the agent (e.g., CLAUDE.md instructions, reviewer standards)
- **Pull:** Available for the agent to fetch when needed (e.g., skills files in the repo)
- For implementation: let agent pull coding standards. For reviewer: push coding standards.

---

## TDD WITH AI

**TDD (Red-Green-Refactor) is essential for getting the most out of agents.** Without TDD, AI tends to:
- Cheat at tests (write implementation first, then superficial tests)
- Code in horizontal layers without integration feedback

The skill teaches AI to write a failing test first, confirm it's red, then implement. This:
- Adds good tests to the codebase naturally
- Gives AI immediate feedback
- Makes it much harder to cheat

**Critical insight:** The quality of your feedback loops is the ceiling for how good your AI can code. Bad codebase with bad tests → bad AI output. Good tests → good AI output.

---

## CODEBASE ARCHITECTURE FOR AI

**Citation:** John Ousterhout's *A Philosophy of Software Design* — **deep modules vs. shallow modules**

- **Shallow modules:** Many small files, each exporting few things, complex dependency graph. Hard for AI to navigate, hard to know where to draw test boundaries.
- **Deep modules:** Small simple interface, lots of functionality inside. Easy to test, easy for AI to work with. Wrap large test boundaries around deep modules.

**Unaided AI produces shallow-module codebases.** You must direct it toward deep modules.

**Mental model:** Design the interfaces of your modules, then delegate the implementation to AI. Modules become "gray boxes" — you know their shape and behavior, you don't need to review every line inside them. This preserves your sanity and sense of the codebase while moving fast.

**Tool:** The "improve codebase architecture" skill scans your repo and finds architectural improvement candidates — clusters of related modules that could be consolidated into deeper modules.

---

## THE PARALLELIZATION SETUP ("Sand Castle")

Pocock built a TypeScript library called **Sand Castle** for running parallel agents:
1. **Planner:** Reviews backlog, selects independent issues to work on
2. **Sandboxes:** Each issue gets its own Docker sandbox + git worktree
3. **Implementers:** Run in parallel, each working on one issue
4. **Reviewers:** Review commits from each sandbox
5. **Merger:** Takes all branches, merges them, resolves conflicts (types, tests, etc.)

This transforms sequential plans into parallel execution.

---

## META-INSIGHTS & PHILOSOPHY

1. **"Don't bite off more than you can chew"** — advice from Martin Fowler (Refactoring) and The Pragmatic Programmer, now applied to AI context windows

2. **Against "specs to code":** Pocock tried it. It sucks. You need to keep a handle on the code. The code is your *battleground*. You don't ignore the code; you shape it throughout the whole process.

3. **Against vibe coding:** Same thing — ignoring the code leads to bad outcomes.

4. **"Bad codebases make bad agents"** — garbage in, garbage out. Deeply understanding your tools (TypeScript, testing, architecture) makes you dramatically better at using AI.

5. **Own your planning stack:** When there's no clear winner among frameworks, own as much as you can. Over-relying on a framework you don't understand leads to helplessness when it breaks. Inversion of control — you should be in control.

6. **Dock rot:** Old PRDs/specs rotting in the repo will mislead agents later. Pocock tends to delete them or mark them closed. Documentation that diverges from code is worse than no documentation.

7. **The planning phase is for alignment, not optimization:** Don't optimize and optimize the PRD trying to make it perfect. The grilling session already achieved what mattered — shared understanding. The real work is in QA.

8. **Working harder than ever with AI is normal:** Most people in the room raised their hands. Delegating implementation means more code review + more planning. That's the tradeoff.

9. **The human-in-the-loop distinction is crucial:** Planning and QA require humans. Implementation is delegable.

---

## PEOPLE & WORKS CITED

1. **Dex Hy** (Human Layer) — Smart Zone / Dumb Zone concept
2. **Frederick P. Brooks** — *The Design of Design* — the "design concept" as shared understanding
3. **Martin Fowler** — *Refactoring* — keeping tasks small
4. **The Pragmatic Programmer** (Andy Hunt & Dave Thomas) — "tracer bullets" concept, "don't bite off more than you can chew"
5. **Ralph Wiggum** (The Simpsons) — namesake for the "Ralph loop" — small incremental changes toward a goal
6. **John Ousterhout** — *A Philosophy of Software Design* — deep modules vs. shallow modules
7. **Memento** (film) — analogy for LLMs continually forgetting/resetting
8. **Sand Castle** — Pocock's own TypeScript library for parallel agent orchestration (github.com/mattpocock — course video manager repo has 744 closed issues, all AI-generated PRDs + implementations)
9. **Beads Framework** (by Steve) — mentioned as another kanban/issues management approach, not evaluated

---

## PRACTICAL TAKEAWAY (If You Take One Thing)

Run the "improve codebase architecture" skill on your repo. See what happens. Deep modules with good test boundaries make everything else work better.

## LINKS
[video]: https://www.youtube.com/watch?v=-QFHIoCo-Ko&list=PLgWBLnrVaSruEA5HL8Yw0MNRfVmiuDhgJ
