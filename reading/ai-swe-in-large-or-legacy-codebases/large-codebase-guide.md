# Guide: Using AI Coding Agents on Large and Legacy Codebases

A synthesis of observed patterns and practical techniques drawn from practitioners working in production codebases ranging from tens of thousands to tens of millions of lines.

---

## The Core Problem

AI coding agents have no memory. Every session, they arrive in your codebase as a new employee who has never seen it before. In a small, well-structured codebase, this is manageable. In a large or legacy codebase it creates three compounding problems:

1. **Orientation failure** — the agent doesn't know where things are, what the architecture is, or how components relate.
2. **Context exhaustion** — the agent burns its context window on search and discovery instead of on the actual task.
3. **Doom loops** — without the right tools, the agent falls back to read-file/grep/compile/fail/retry cycles that can take hours and cost 20–30× more tokens than necessary.

All of the techniques below address one or more of these three problems.

---

## 1. Prepare the Codebase for AI Navigation

The biggest lever is the codebase itself. Software quality matters *more* with AI, not less.

### 1.1 Match the file system to the mental map

AI cannot infer groupings that exist only in a developer's head. If you have a "video editor service," "authentication module," and "CRUD forms," those should be visible directories with clean boundaries — not scattered files that can freely import each other. Make the architecture legible to a new hire on day one.

### 1.2 Use deep modules with explicit interfaces

The "deep module" pattern (from John Ousterhout's *A Philosophy of Software Design*) is especially valuable for AI work:
- Each module has a small, documented public interface.
- The implementation is hidden behind it.
- When the AI encounters a module, it reads the interface, understands what the module does, and doesn't need to read the implementation unless it's modifying it.

This is "progressive disclosure of complexity." The agent navigates at the interface level and only descends into implementations when necessary. Modules with shallow, leaky interfaces force the agent to read everything.

### 1.3 Enforce module boundaries

In dynamically-typed or permissive languages (JavaScript/TypeScript), nothing prevents cross-module imports by default. Use linting rules, package-level exports, or barrel files to enforce boundaries. An agent that can import anything from anywhere will do exactly that.

### 1.4 Invest in test coverage before involving AI

Tests are the AI's feedback loop. Without them, the agent has no reliable signal about whether its changes are correct. This leads to more compile/run cycles and larger contexts. A well-tested codebase lets the agent run tests and get deterministic feedback quickly.

---

## 2. Establish Orientation Before Coding

Don't send the agent straight to a task. Invest in orientation first — it pays back many times over.

### 2.1 Generate an architecture map

For a codebase the agent (or you) hasn't seen before, start with a high-level orientation query:
- *"Summarize this codebase: main components, how they interact, what each major module is responsible for."*

This single pass gives the agent (and you) the map that a senior engineer would carry in their head. For very large codebases, you may need to do this module by module, building up a persistent architecture document.

### 2.2 Build and maintain architecture documentation

A persistent `ARCHITECTURE.md` (or equivalent) that describes:
- Major modules and their responsibilities
- Key interfaces between modules
- Known anti-patterns or technical debt areas
- The "why" behind non-obvious decisions

This document is loaded at the start of every agent session. It prevents the agent from re-discovering what you already know, and prevents it from following the bad patterns that exist in the legacy code.

### 2.3 Combine code analysis with existing documentation

When available, cross-reference AI-generated architecture summaries with any existing diagrams, ADRs, or design docs. The intersection ("this file handles the computation described in this diagram section") is much more useful than either alone.

---

## 3. Provide Organizational Context, Not Just Code

For any non-trivial task, *why* the code is the way it is matters as much as what it does. Without organizational context, agents will:
- Recreate mistakes that were tried and rejected previously
- Remove backwards-compatibility code whose reasons aren't obvious from the code
- Follow the wrong patterns (often the oldest ones, which are most prevalent in the codebase)

### 3.1 The iceberg problem

What's visible in the code is the tip: what compiles and runs. What's invisible: original intent, rejected approaches, incident learnings, decisions made in Slack, code that was deleted. Build processes for making the invisible visible.

**Practically:**
- Use ADR (Architecture Decision Record) files to capture *why* decisions were made, not just what was decided.
- Keep a `DECISIONS.md` or similar for non-obvious choices.
- When briefing the agent on a task, include relevant history: "We tried approach X in 2023 and it failed because Y."

### 3.2 Access ≠ Understanding

Wiring up MCP tools for all your data sources (GitHub, Slack, Notion, JIRA) does not give the agent organizational understanding. It just gives it access to a firehose of data. The agent will "satisfy search" — find the first plausible-looking answer and stop — missing the real signal buried elsewhere.

What actually helps is *distilled* context: the key decisions, the relevant history, the patterns your team follows, pre-loaded before the agent starts.

### 3.3 Seed context at the start of planning

The highest ROI moment for context delivery is *before the agent starts planning*. A well-briefed planning phase prevents rework in the implementation phase. Give the agent:
- The task/ticket
- The relevant area of the codebase and its architecture
- Patterns your team follows (and explicitly: patterns it should NOT follow, even if they appear in the existing code)
- Known constraints: dependencies you don't want added, interfaces you don't want changed

---

## 4. Use Language-Server-Grade Tools, Not Text Search

This is the single most impactful mechanical improvement for large codebases.

### 4.1 The naive approach and its cost

Without the right tools, an agent navigates a large codebase by grepping and reading files. For a task like renaming a heavily-used type across an 840K-line codebase, this approach:
- Takes 3+ hours
- Costs ~28 million tokens
- Requires 4+ compile cycles before getting it right

### 4.2 Use semantic/LSP-powered MCP servers

Language Server Protocol (LSP) tools understand your code as a structured program — they know what "rename this symbol" means across all its references, respecting language semantics. MCP servers that expose LSP capabilities give the agent:
- **Semantic search**: "find all uses of this interface," not grep for a string
- **Symbol-aware editing**: insert a method at the right location in a class, not at a line number
- **Refactoring operations**: rename, extract method, move method — executed by the language server, not by the agent doing text manipulation

**Key tools:**
- **Serena** (open-source, 30+ languages, LSP-based): semantic retrieval and editing via MCP. Use this as a general-purpose tool across languages.
- **Refactor MCP** (Dave Hillier, .NET/C#): Roslyn-based refactoring operations via MCP. Language-specific but extremely powerful.
- For other languages: search for MCP servers that expose LSP, Roslyn, or semantic analysis capabilities. The pattern transfers.

**Result with Refactor MCP on the same 840K-line rename task**: 5 minutes, 1 million tokens — a 28× token reduction and 36× time reduction.

### 4.3 Reduce tool output noise

CLI tools (git log, test runners, compilers) produce verbose output that fills context windows with noise. Use hooks to compress this output before it reaches the agent.

**RTK (Rust Token Killer)**: intercepts CLI tool calls and rewrites them to produce compact, structured output. ~38% token reduction on realistic backend tasks. Works by prefixing commands through a binary; configurable for any CLI tool.

---

## 5. Manage the Context Window Deliberately

The context window is your most constrained resource. Everything about agent quality in a large codebase comes down to what you put in it.

### 5.1 The "dumb zone"

Context window quality degrades around 40–60% fill. At 60%+ context, you get statistically worse instruction-following and reasoning. As a default:
- Keep below 40% for complex tasks where correctness is critical
- Keep below 60% as a hard ceiling
- Start a new session (pointing at static artifact files) rather than continuing in a degraded context

### 5.2 Static artifacts beat auto-compaction

When important context (architecture notes, design decisions, plan artifacts) lives in files on disk, you can start a fresh session and reload exactly what matters. Auto-compaction by the agent is unpredictable — it may discard something important. Put everything that matters into named files and reference them explicitly at the start of each session.

### 5.3 Instruction budget

Models reliably follow ~150–200 instructions. A CLAUDE.md + system prompt + MCP tool descriptions + planning prompt can easily exceed this, causing random non-adherence. Keep individual prompts under 40 instructions. Split complex workflows across multiple prompts.

### 5.4 Keep agent responses terse

Instruct the agent to be concise: "respond like a terse, technical expert — no prose explanations of what the code does, no summaries of what you just did." This reduces output tokens by ~24% without affecting code quality. Add this to your CLAUDE.md as the first rule.

---

## 6. Separate Research from Planning from Implementation

This is the structural technique that prevents the most common failure mode: the agent making architectural decisions it shouldn't be making, or following the wrong patterns because it inferred intent from incomplete research.

### 6.1 Research: compress facts, not opinions

Research is a scan of the codebase to understand *what is true today*. It should be purely objective — how does authentication work, what does this endpoint do, trace the data flow for X.

**Key technique**: run research in a context window that does NOT know what you're building. If the agent knows you're building a new feature, it will inject opinions and implementation ideas into the research. Generate your research questions in one session, execute them in a fresh session with no task context.

### 6.2 Design: align before writing code

After research, generate a design discussion artifact (~200 lines) that captures:
- Current state
- Desired end state
- Patterns found in the codebase (including which patterns to follow and which to avoid)
- Open questions for the human

Review and correct this document before the agent writes any code. This is where you catch wrong assumptions at the cheapest point in the process.

### 6.3 Structure outline: phases, not implementations

After the design is agreed, generate a structure outline — the phases of work, the method signatures and types that will be introduced, the order of changes. This is the "C header file" of the plan: enough to see what the agent is thinking without committing to the implementation. ~2 pages, not 8.

Review the structure outline instead of the full plan. If anything is wrong, correct it here.

### 6.4 Vertical plans, not horizontal

AI naturally writes horizontal plans: do all the database changes, then all the service layer, then all the API, then all the frontend. This creates long stretches with no testable checkpoints.

Insist on **vertical plans**: each phase is a thin slice through all layers, testable at the end of that phase. Same total code, but you catch failures at phase 2 instead of after 2,000 lines of code.

### 6.5 Specs as execution documents

For any non-trivial task, write a spec before coding. A spec (distinct from a PRD or architecture doc) is an execution document for the agent:
- What to build and why (brief)
- Explicit constraints (what libraries NOT to use, what patterns NOT to follow, what is out of scope)
- Task breakdown with verification steps

Explicitly listing what is *out of scope* is as important as what's in scope — agents are eager and will implement things you didn't ask for.

---

## 7. Use Multi-Agent Patterns for Large Tasks

Large legacy codebases often require more context than fits cleanly in a single agent session. Multi-agent patterns distribute this problem.

### 7.1 Domain-specialized sub-agents

Give each agent a restricted domain: one agent for the frontend, one for the backend API, one for the database layer. Each agent's context window contains only what's relevant to its domain. Benefits:
- Smaller, more focused context windows
- Better adherence to domain-specific patterns
- Parallel execution where tasks are independent

### 7.2 Orchestrator/worker separation

Orchestrators read key context files, generate plans and prompts, and delegate to workers. They do not write code. Workers execute within their domain. This keeps the orchestrator's context lean (high-level) and the worker's context focused (domain-specific).

### 7.3 Persistent agent memory

For agents that work on the same codebase repeatedly, maintain a "mental model" file per agent — what it has worked on, what patterns it has observed, what decisions have been made. This is externalized memory that doesn't require refilling the context window every session.

---

## 8. Keep Humans in the Loop at the Right Moments

### 8.1 Review design artifacts, not plans or code alone

Reviewing the design discussion document (200 lines) and structure outline (2 pages) gives more leverage than reviewing the full plan (1,000 lines) or the final code. You catch wrong assumptions before they become code.

### 8.2 Read the code

Plans differ from implementations. Auto-generated code should be read and understood. This is not optional for production systems. The goal is 2–3× productivity, not 10× with code you don't understand. Reading code is the professional obligation, not reviewing plans.

### 8.3 Correct early, not late

The cheapest time to fix a misunderstanding is during design. The second cheapest is during the structure outline. The most expensive is after implementation. Push corrections as early as possible in the workflow.

---

## Quick Reference: Checklist for a New Large-Codebase AI Task

**Before the task:**
- [ ] Architecture documentation exists and is current
- [ ] CLAUDE.md includes known anti-patterns and "do not follow" patterns from the legacy code
- [ ] Relevant MCP servers installed (Serena or language-specific LSP tool; RTK for output compression)
- [ ] Agent configured for terse responses

**Research phase:**
- [ ] Generate research questions in one session (without knowing the task)
- [ ] Execute research in a fresh session (to avoid opinion contamination)
- [ ] Research output is factual (what is true today), not prescriptive

**Design phase:**
- [ ] Generate design discussion artifact (~200 lines)
- [ ] Review: correct wrong assumptions about patterns, dependencies, and approach
- [ ] Explicitly state which patterns NOT to follow

**Structure phase:**
- [ ] Generate structure outline (phases, method signatures, types)
- [ ] Verify the plan is vertical (thin slices through all layers, testable at each phase)
- [ ] Review with team if touching code owned by others

**Implementation:**
- [ ] Agent operates with focused context (<40% fill)
- [ ] Agent uses LSP/semantic tools for navigation and refactoring (not grep/text)
- [ ] Tests run after each phase
- [ ] Read the code at each commit

**Context management:**
- [ ] Everything important is in named files on disk (not in the context window)
- [ ] Start fresh sessions rather than compacting a degraded context
- [ ] Context < 40% for critical tasks; hard limit at 60%

---

## Tools Summary

| Tool | Purpose | Language |
|------|---------|----------|
| [Serena](https://github.com/oraios/serena) | LSP-based semantic search and editing | 30+ languages |
| [Refactor MCP](https://github.com/dave-hillier/refactor-mcp) | Roslyn-based refactoring operations | .NET/C# |
| [RTK](https://github.com/rtk-ai/rtk) | Compresses CLI tool output (git, compilers, test runners) | Any |
| Unblocked | Full context engine: organizational memory, expert graph, conflict resolution | Enterprise |

---

## Recommended Reading

- Ousterhout, *A Philosophy of Software Design* — deep modules, progressive disclosure
- Horthy, [12-Factor Agents](https://github.com/humanlayer/12-factor-agents) — context engineering principles for reliable agents
- Horthy, *Everything We Got Wrong About RPI* — CRISPY methodology for research/plan/implement
