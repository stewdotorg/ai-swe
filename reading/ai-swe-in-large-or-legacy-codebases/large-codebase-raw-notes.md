# Raw Notes: AI Coding Agents in Large/Legacy Codebases

Observations extracted from relevant YouTube transcripts, organized by source.

---

## Jo Van Eyck — "AI coding agents are useless on large codebases. Unless you do THIS."
Source: `jo-van-eyck-ai-coding-agents-are-useless-on-large-codebases-unless-you-do-this.md`

- Without the right tools, brute-force Claude Code on a ~840K-line .NET codebase took **3 hours and ~28M tokens** to complete a rename refactor. With an MCP-based refactoring tool, the same task took **5 minutes and ~1M tokens** (28x fewer tokens, 36x faster).
- The root problem is that naive agents resort to repeated read-file / grep / compile / fail / retry loops ("doom looping"). These loops burn enormous tokens because every compile error and stack trace goes into the context window.
- **Serena MCP**: An open-source MCP server that provides semantic code retrieval and editing. Instead of scanning whole files with grep, it uses embedding-based and semantic search — "find symbol," "find references," etc. This dramatically reduces the number of tokens needed to navigate a large codebase.
- **Refactor MCP** (by Dave Hillier, .NET-specific but concept is general): Exposes Roslyn-based refactoring tools to the agent — rename, extract method, move method, etc. The agent calls one tool and the IDE-grade refactoring engine does the work correctly across all 143+ files in one shot, with no compile loops.
- Key insight: For any language, look for an MCP server that provides *language-server-grade* operations (semantic search, symbol resolution, refactoring). This is the difference between an agent using an IDE versus using grep.
- Token cost scales directly with number of compile/test cycles. Giving the agent the right tool to do a rename in one shot (rather than doing it textually across hundreds of files) is the core lever.
- Recommend: before reaching for sub-agents or bigger models, first ask whether there's an MCP server that provides smarter code navigation/editing for your language.

---

## Jo Van Eyck — "Cut your LLM token bill in half with these 2 simple tricks"
Source: `jo-van-eyck-cut-your-llm-token-bill-in-half-with-these-2-simple-tricks.md`

- **"Caveman" mode**: Instructing the agent to be terse — "respond like a terse smart man, all technical, no fluff" — cuts output tokens by ~24% in small examples. The agent generates ~20 tokens instead of ~70 for the same reasoning. Put this in your CLAUDE.md / agents.md as rule #1.
- **RTK (Rust Token Killer)**: A hook-based binary that intercepts CLI tool calls (git log, test runners, compiler output) and compresses their output before it reaches the LLM's context window. Typical verbose `git log` → compact structured summary. ~38% token reduction on a realistic backend engineering task. Works by wrapping CLI commands; configurable for any tool.
- Takeaway: Two independent dimensions of token waste — (a) verbose LLM prose responses and (b) verbose tool output fed back into context. Both are controllable without changing the model or the task.
- Hooks in Claude Code (and other agents) allow you to intercept tool calls and rewrite/clean them before they land in context. This is a systematic place to apply compression.

---

## Looking Glass Universe — "How I used AI to understand a huge codebase"
Source: `looking-glass-universe-how-i-used-ai-to-understand-a-huge-codebase.md`

- Problem: navigating a large unfamiliar codebase (OpenFold) by chasing references file-by-file is overwhelming. Every function points to another file.
- ChatGPT (smaller context) could only analyze one file at a time and would *guess* about functions in other files based on their names alone — unreliable.
- Solution: Upload the *entire codebase* to Claude (large context window). Ask "give me a summary of this entire codebase — main components, what I need to pay attention to." This gave the first clear picture of the overall architecture.
- Key insight: getting the overview first (architecture map) is more valuable than reading files sequentially. It lets you navigate *non-linearly* — you know which parts are important and which to skip.
- Combining the AI-generated architecture summary with an existing architectural diagram (from the paper/docs) let her triangulate: "this module handles this part of the pipeline." Having two representations together was much more useful than either alone.
- This is an "orientation" use case: use large-context AI to generate the mental map that a senior dev would have, before going into details.

---

## AI Fieldbook — "Agentic AI for Code #1 - Legacy Code Documentation for the Enterprise"
Source: `ai-fieldbook-agentic-ai-for-code-1-legacy-code-documentation-for-the-enterprise.md`

- Enterprise scenario: tens of millions of lines of code, built over 10–15 years, multiple layers (frontend GUI, application layer, stored procedures), minimal documentation, original authors gone.
- **Context Lakehouse** approach: ingest everything into a unified context store — source code, tests, documentation, APM/telemetry data, incident tickets. The AI needs *all* of this to understand why the code behaves as it does, not just the code itself.
- Current IDE tools (with normal context windows) cannot handle tens of millions of lines. You need a different architecture: chunk the codebase, interrelate chunks, query them.
- The AI agent processes chunks systematically, documenting what each section does at configurable levels of detail. Accuracy is ~80–90%; the remaining 10–20% needs human review (especially code that changed direction over 15 years).
- **What documentation alone can unlock:**
  1. Business context diagrams showing how all components interrelate
  2. Forward-looking refactoring recommendations (e.g., add API layers, RBAC — things that didn't exist in the '90s)
  3. Traced test cases: every test case traces back to specific lines of legacy code
  4. PRD generation for refactoring work, distributable to other teams
  5. Code translation/modernization into new languages
- Human in the loop is essential at the specification/review stage — AI is not perfect, especially for code that evolved over many years with conflicting patterns.
- Key insight: The output of the documentation phase becomes the *context* for all subsequent AI work on the codebase — you're building a persistent knowledge layer, not just asking one-off questions.

---

## Matt Pocock — "Your codebase is NOT ready for AI (here's how to fix it)"
Source: `matt-pocock-your-codebase-is-not-ready-for-ai-here-s-how-to-fix-it.md`

- AI is like "the guy from Memento" — no memory, no prior experience with your codebase. Every session it starts from zero. The codebase's structure is the only orientation it has.
- A web of shallow, tightly interconnected modules (where any module can import from any other) is the worst possible structure for AI. The AI sees a jumbled mass of dependencies with no natural groupings.
- **Deep modules** (from John Ousterhout's "A Philosophy of Software Design"): lots of implementation hidden behind a simple public interface. This creates natural seams.
  - The *interface* of a module is the AI's entry point — it can read the interface, understand what the module does, and trust it without reading the implementation.
  - This is "progressive disclosure of complexity": interface at the top, implementation details accessible when needed but not required.
- **Benefits of deep modules for AI:**
  1. **Navigability**: AI can browse module interfaces on the file system and know what each service does without reading all the code.
  2. **Reduced cognitive load**: The engineer only needs to think about 7–8 "lumps" (modules) and their interfaces, not hundreds of interconnected files.
  3. **Testability**: Well-bounded modules are easy to test with clear behavior contracts.
- Gray-box modules: the engineer controls the interface and the tests; AI manages the implementation inside.
- Think of every spawned AI session as a "new starter joining your codebase." Design for new starters → design for AI.
- The file system *must* reflect the mental map of the codebase. If groupings exist only in the developer's head but not in the directory structure, the AI can't see them.
- Software quality (modularity, testability, clear interfaces) matters *more* with AI, not less, because AI has no accumulated organizational knowledge.

---

## AI Engineer / Peter Werry (Unblocked) — "Mergeable by default: Building the context engine to save time and tokens"
Source: `ai-engineer-mergeable-by-default-building-the-context-engine-to-save-time-and-tokens-peter-werry-unblocked.md`

- **Context engine definition**: "The art of supplying all the context you need and most importantly *none* of the context you don't need, in a highly optimized way, so the agent executes the task in a streamlined way aligned with your organization's best practices."
- Problem with naive RAG / MCP wiring: access ≠ understanding. Wiring up all your tools to an agent doesn't make it understand the *reasons* decisions were made, what was tried and rejected, or historical context buried in Slack/incidents.
- **"Satisfaction of search"** (from radiology): agents stop searching when they find something that looks like the answer, missing more important context elsewhere (e.g., in a past incident, a Slack conversation). This is a real failure mode, not just a metaphor.
- The "iceberg" of what matters: code that compiles is the visible tip; what's beneath the waterline (original intent, rejected approaches, deleted code) is invisible without a context engine.
- Key components of a proper context engine:
  1. **Unified system context**: builds relationships between data sources (PRs, docs, Slack, incidents, code). Not just linkages but *why decisions were made*.
  2. **Conflict resolution**: when docs say one thing and Slack says another, the engine needs to surface (not hide) conflicts it can't resolve. Recency alone doesn't work; code is often the source of truth but not always (e.g., when a refactor is in progress).
  3. **Targeted retrieval + personal relevance**: pull context relevant to the task and the person. Bias toward repos the engineer works in most. Avoid polluting context with unrelated org data.
  4. **Social/expert graph**: understand who the experts are for each area of the code. Use expert contributions to weight conflicting information. "Bottling the expert" — distilling an expert's past decisions, PR comments, and Slack conversations into context to guide the agent.
  5. **Access controls**: don't surface private channel / restricted repo content to unauthorized agents.
- Hard lessons:
  - Optimizing for *access* (wiring up MCP tools) rather than *understanding* doesn't work.
  - Caching context engine answers leads to stale/regressive results.
  - Bigger context windows don't solve understanding — even with 1M tokens, the model struggles to reason across conflicting sources.
- **Performance comparison** (single large task): Without context engine: 2.5 hours, 21M tokens. With context engine: 25 minutes, 10M tokens.
- Context engine is most valuable during **planning phase** — biggest ROI per token. Also valuable at review, incident triage, and ticket enrichment.
- Key quote: "AI-generated code should feel like it was written by someone who has been on the team for 20 years."
- ~90% of agent execution time is context collection, not code writing. Output tokens (code generation) dominate latency. The smarter your context delivery, the more the agent's outputs stay tight.

---

## MLOps.community / Dexter Horthy — "Everything We Got Wrong About Research-Plan-Implement"
Source: `mlops-community-everything-we-got-wrong-about-research-plan-implement-dexter-horthy.md`

- AI is great for low-complexity greenfield tasks; notoriously poor for high-complexity brownfield (legacy) tasks.
- **Do not outsource the thinking.** The engineer must stay in the loop — not just approving the plan but shaping it.
- **"The Dumb Zone"**: context windows degrade in quality around 40–60% fill. Rule of thumb for less experienced users: keep context below 40%. Experienced users can push to 60% situationally. Do not use built-in compaction if you can avoid it — put state into static assets/files instead, so you can resume without quality degradation.
- **Good research ≠ opinionated research**: research should compress *facts about how the code works today*, not opinions about how to implement the new feature. Separating the two is critical.
  - Technique: generate research questions in one context window, then execute research in a *separate* context window with no knowledge of what's being built. This prevents the model from injecting implementation bias into the research.
- **Instruction budget**: models can reliably follow ~150–200 instructions. A 85-instruction monolithic planning prompt + Claude.md + system prompt + MCP tool descriptions can easily exceed this, causing random non-adherence. Fix: split across multiple smaller prompts, each with <40 instructions.
- **Horizontal vs. vertical plans**: models love to write horizontal plans (do all DB, then all services, then all API, then all frontend). This creates long stretches with no testable checkpoints. Push for *vertical plans* — each phase is a thin slice through all layers, testable at each step. Same amount of code, but failures are caught earlier.
- **Design discussion artifact** (~200 lines): before writing the plan, generate a document that brain-dumps the agent's current understanding: current state, desired end state, patterns found, open questions. The human reviews and corrects this before the agent writes code. 200 lines >> 1000-line plan in terms of review leverage.
- **Structure outline** (C header file analogy): a high-level overview of phases, types, and method signatures — enough to see what the agent is thinking without committing to the implementation. Humans review this instead of the full plan.
- Don't read the plan; read the code. Reading plans is redundant because the code will differ from the plan anyway.
- **CRISPY** (evolution of RPI): Questions → Research → Design → Structure → Plan → Work tree → Implement → PR.
- No magic words. If a workflow requires "magic words" to produce good output, fix the workflow.
- Recommended context strategy: "everything important goes into static assets" (files on disk). This way, auto-compaction doesn't destroy your context; you can always resume from those files.

---

## Codeless Developer — "OpenAI Codex Code Review on a Large Legacy Codebase (Brutally Honest)"
Source: `codeless-developer-openai-codex-code-review-on-a-large-legacy-codebase-brutally-honest.md`

- AI coding assistants (including Codex and Copilot) struggle with *architectural* understanding of large codebases. They review at the class/method level but miss solution-wide anti-patterns.
- In the test case, a database-centric anti-pattern (everything coupled to Entity Framework in the domain layer) went undetected; architecture scored 62/100 when it should be ~10/100.
- AI will often just summarize the README rather than doing deep structural analysis.
- Providing a detailed weighted scorecard benchmark helps, but models still over-score flawed architecture.
- Key finding: at this stage, all AI coding tools are better at *local* (file/class-level) feedback than *architectural* feedback. Do not use AI alone for architectural code review of legacy systems — human expertise is still required for this.
- Implication: if you're using AI to navigate or modify a legacy codebase, you can't assume it understands the architectural anti-patterns. You need to *tell it* what the architecture is and what the anti-patterns are.

---

## WorldofAI — "Serena: Ultimate AI Coding System..."
Source: `worldofai-serena-ultimate-ai-coding-system-ends-vibe-coding-100x-better-than-vibe-coding-bmad-alternative.md`

- **Serena MCP** is an open-source MCP server providing IDE-level intelligence to coding agents via the Language Server Protocol (LSP).
- Supports 30+ programming languages.
- Key capabilities: find symbol, find references, insert method at correct location, refactor across files — all using semantic/LSP understanding rather than grep/text search.
- Concrete demo: asked to "find all user management code in both JavaScript and Python files" — Serena used LSP-powered symbol analysis to find relevant classes, functions, and modules across the codebase, rather than reading every file.
- Result: more accurate, fewer tokens, fewer hallucinations about what exists where.
- Serena is complementary to spec/planning frameworks (BMAD, SpecKit, OpenSpec) — those help with planning and task structure; Serena handles the actual code editing with IDE-grade accuracy.
- Critical distinction: Serena gives the *agent* IDE-level code intelligence. The agent behaves like it's using an IDE — it jumps to symbols, finds references, and inserts code in the right place — instead of brute-forcing with grep and string matching.

---

## Owain Lewis — "How I Code With AI Agents (Spec-Driven Development)"
Source: `owain-lewis-how-i-code-with-ai-agents-spec-driven-development.md`

- **Spec-driven development**: write a specification document *before* coding. The spec is a contract between you and the agent about what will be built.
- A spec for an agent includes: what and why, constraints (what NOT to build, what dependencies NOT to add), explicit task breakdown with verification steps.
- Key benefit: the agent doesn't have to guess at implementation decisions. Without a spec, agents are "eager" — they'll implement things you didn't want, in ways you didn't intend.
- Spec vs. PRD vs. tech design doc: PRD is for humans/stakeholders; tech design doc is for engineers; spec (as used here) is an *execution document for agents* — more prescriptive, includes constraints and task steps.
- Tasks in a spec should mirror good engineering practice: implement X, verify with tests, commit. This allows incremental, reviewable progress across multiple sessions.
- Allows resuming across sessions: implement task 1, commit, come back later with a fresh Claude Code session, implement task 2. The spec persists on disk.
- Iterate on the spec as you go — it's not waterfall. If task 1 reveals something unexpected, update the spec before proceeding.
- Explicitly listing *out-of-scope* items is as important as listing what's in scope, because agents are eager to add things.
- Working incrementally (one task at a time) produces higher quality and easier review than handing the agent a full feature and reviewing 10,000 lines at the end.

---

## Dexter Horthy — "12-Factor Agents: Patterns of Reliable LLM Applications"
Source: `ai-engineer-12-factor-agents-patterns-of-reliable-llm-applications-dex-horthy-humanlayer.md`

- LLMs are pure functions: tokens in, tokens out. Everything about agent quality reduces to *what you put in the context window*.
- Context engineering is about getting the right tokens in, not just more tokens.
- **Own your context window**: don't blindly add everything. Summarize errors, don't dump full stack traces. Clear stale error context after a retry succeeds.
- **Small, focused agents**: don't build one giant agent loop. Build small agents with 3–10 steps and clear responsibilities, embedded in a mostly deterministic workflow. This gives manageable context windows and clear boundaries.
- **Control flow belongs in code, not prompts**: if you can express a workflow as an if statement or a DAG, do it. Reserve the LLM for the parts that genuinely require natural language understanding.
- **Stateless reducers**: agents shouldn't hold state internally; state lives in your system. This makes pause/resume trivial and keeps context windows small.
- Longer context windows degrade results. Always prefer tighter, more targeted context over "let's just stuff everything in."
- The "right" amount of context is task-dependent, not a fixed number.

---

## IndyDevDan — "My Pi Agent Teams. Claude Code Leak SIGNAL. Harness Engineering."
Source: `indydevdan-my-pi-agent-teams-claude-code-leak-signal-harness-engineering.md`

- The agent *harness* — the deterministic code, prompt infrastructure, tool wiring, and orchestration around the LLM — is what drives quality, not the model alone.
- **Specialization as a context management strategy**: in multi-agent systems, giving each agent a restricted *domain* (front-end only, back-end only, DB migrations only) is a key technique for keeping context windows focused and relevant. "One agent, one purpose, one focused context window."
- "Mental model" files per agent: each specialized agent maintains its own persistent notes file (what it's worked on, what patterns it's observed). This acts as externalized agent memory, reducing the need to rediscover context each session.
- Orchestrators delegate; workers execute. Orchestrators only read key files and produce prompts — they don't touch the codebase. This separation keeps orchestrator context clean and high-level.
- The agent harness lets you solve *problem classes*, not just one-off tasks. Once built, the harness encodes patterns and workflows that work consistently.
