# High-Level Topics — Matt Pocock: Essential Skills for AI Coding (Workshop)

- **AI as a new paradigm still grounded in software engineering fundamentals**
  - Classic books (*Refactoring*, *The Pragmatic Programmer*, *The Design of Design*, *The Philosophy of Software Design*) remain highly relevant when working with AI

- **LLM constraints: The Smart Zone vs. Dumb Zone**
  - Attention relationships scale quadratically with tokens
  - ~100K tokens is the practical limit before quality degrades
  - Bigger context windows (200K, 1M) mostly just give you more dumb zone
  - Tasks must be sized to stay within the smart zone
  - Compacting vs. clearing context

- **LLM constraints: Amnesia / "the guy from *Memento*"**
  - Every session resets to the system prompt
  - Optimize for this deterministic reset rather than preserving history
  - Four session stages: system prompt → explore → implement → test/feedback

- **The "Grill Me" skill — achieving shared understanding (alignment)**
  - Relentless question-and-answer with the LLM before building
  - The design concept as a shared mental model (Brooks)
  - Human-in-the-loop task; cannot be automated
  - Generates conversation history as a valuable asset

- **Against "specs-to-code" / vibe coding**
  - You can't ignore the code; code is your battleground
  - You must keep a handle on the code and shape it

- **Planning documents: Destination vs. Journey**
  - **PRD** = destination document (user stories, implementation decisions, out-of-scope)
    - Don't need to read/review it deeply — LLMs are good at summarization
    - Purpose is to capture alignment, not to be a perfect spec
  - **Kanban board** = journey document (independently grabbable tickets with blocking relationships)

- **Vertical slices (tracer bullets) over horizontal layers**
  - AI naturally codes layer-by-layer (schema → API → frontend) — this is bad
  - Vertical slices give immediate integrated feedback across all layers
  - Allows continuous testing and review at each step

- **Parallelizable work via Kanban-style issues**
  - Independently grabbable tickets → directed acyclic graph
  - Multiple agents can work in parallel on unblocked tasks
  - Enables day shift (human planning) / night shift (AFK implementation)

- **AFK implementation — the "Ralph loop"**
  - Prompt-driven loop: explore repo → use TDD → run feedback loops
  - Agent picks next AFK task, implements, self-reviews
  - Docker sandboxing for safe parallel execution (Sand Castle tool)
  - Sequential loop as a starting point; parallel execution as the goal

- **Sub-agents and token management**
  - Sub-agents isolate context windows, explore deeply, return summaries
  - Real-time token monitoring is essential

- **Test-Driven Development (TDD) with AI**
  - Red-green-refactor cycle prevents AI from cheating on tests
  - Adds quality tests to the codebase
  - Feedback loops are the ceiling for AI code quality
  - Make feedback loops fast and high-quality (tests, types, linting)

- **Deep modules vs. shallow modules (codebase architecture)**
  - Shallow modules = many small files, hard to navigate, hard to test
  - Deep modules = small interfaces, large internal functionality, easy to test
  - AI unaided produces shallow codebases — you must intentionally guide toward deep modules
  - The "improve codebase architecture" skill for finding refactoring opportunities

- **Retaining mental model of the codebase**
  - Know the interfaces/shapes of modules (gray boxes), delegate implementation
  - Avoid losing sense of the codebase while moving fast

- **Human QA is where you impose taste and opinion**
  - Automation without taste produces "slop"
  - QA phase creates new issues for the Kanban board — continuous feedback loop

- **Enforcing coding standards: Push vs. Pull**
  - **Push**: instructions always sent (e.g., claude.md, rules files)
  - **Pull**: skills the agent can invoke when needed
  - Implementer uses pull; automated reviewer uses push (full coding standards)

- **Code review and automated review**
  - Separate review step with cleared context (smart zone reviewer)
  - Use smarter models for review (e.g., Opus) vs. implementation (e.g., Sonnet)
  - Expect to do more code review in the AI era — small, self-contained PRs

- **Front-end workflow with AI**
  - AI can't visually evaluate frontends well
  - Use AI to generate throwaway prototypes, then humans choose

- **Documentation decisions: Keep vs. Discard**
  - Risk of "doc rot" — stale PRDs influencing agents badly
  - Prefer to discard or mark as closed rather than keep stale docs in repo

- **Team workflow integration**
  - Idea → destination is collaborative (team grilling, prototypes, research)
  - Implementation is where individual devs can manage parallel AFK agents
  - Prototypes may need their own mini-PRDs

- **Tooling philosophy: Own your stack**
  - Avoid over-reliance on any one framework when no clear winner exists
  - Inversion of control — you must be able to observe and fix each part

- **Sand Castle — a TypeScript library for AFK agent loops**
  - Creates git worktrees, sandboxes in Docker
  - Planner → parallel implementers → reviewer → merger agent pipeline
