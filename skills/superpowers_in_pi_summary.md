# Superpowers in Pi: Overview & Adaptation Guide

## The 14 Skills, by Role

```
PLANNING LAYER
├── brainstorming          "Before any creative work" — explore intent → design doc
├── writing-plans          Design doc → bite-sized implementation plan
│
EXECUTION LAYER
├── subagent-driven-dev     Execute plan: fresh subagent per task + 2-stage review
├── executing-plans         Same, but in a separate session (no subagents)
├── dispatching-parallel    Multiple independent failures → parallel agents
│
QUALITY LAYER
├── test-driven-dev         RED-GREEN-REFACTOR — no code without failing test
├── systematic-debugging    4-phase root cause investigation before any fix
├── requesting-code-review  Dispatch reviewer subagent against git SHA range
├── receiving-code-review   Verify feedback, push back when wrong, no performative agreement
│
WRAP-UP LAYER
├── verification-b4-completion  Run commands, read output, THEN claim success
├── finishing-a-branch      4 options: merge/PR/keep/discard + worktree cleanup
├── using-git-worktrees     Isolate feature work in git worktrees
│
META LAYER
├── using-superpowers       How skills work, priority rules, red flags
└── writing-skills          TDD for process docs — test skills with subagents
```

## How Skills Reference Each Other

```
brainstorming ──▶ writing-plans ──▶ subagent-driven-dev ──▶ finishing-a-branch
                        │                │
                        └──▶ executing-plans ──▶ finishing-a-branch

All implementers: tdd, systematic-debugging, verification-b4-completion
subagent-driven-dev: requesting-code-review (per task)
```

## Pi Adaptation: What Works & What Doesn't

| Feature | Status in Pi | Notes |
|---------|-------------|-------|
| **Skill loading** | ✅ Native | Pi auto-discovers `SKILL.md` files, injects into system prompt |
| **`Skill` tool** | ⚠️ Different | No `Skill`/`Task` tool. I use `read` on SKILL.md files directly |
| **`Task` (subagents)** | ❌ Missing | Pi has no subagent dispatch. This affects 4+ skills |
| **`TodoWrite`** | ❌ Missing | Pi has no structured task tracking tool |
| **Checklist items** (`- [ ]`) | ⚠️ Manual | I track these in my own thinking or plan files |
| **`read`/`write`/`edit`/`bash`** | ✅ Same | File and command tools work identically |
| **Git operations** | ✅ Same | `bash` tool handles all git commands |
| **TDD cycle** | ✅ Works | All verification commands run via `bash` |
| **`docs/superpowers/` paths** | ✅ Works | File writes go to same location |
| **`.worktrees/` conventions** | ✅ Works | Git worktrees function normally |

## Critical Gap: Subagents

The following skills **heavily depend on subagent dispatch** (`Task` tool in Claude Code):

| Skill | Subagent Dependency | Severity |
|-------|-------------------|----------|
| **subagent-driven-dev** | Core mechanic — dispatch implementer + 2 reviewers per task | 🔴 Unusable as written |
| **dispatching-parallel-agents** | Core mechanic — dispatch one agent per failure domain | 🔴 Unusable as written |
| **requesting-code-review** | Dispatches `code-reviewer.md` subagent | 🟡 Degraded (can inline review) |
| **writing-skills** | Tests skills by dispatching subagents without the skill | 🟡 Degraded |
| **executing-plans** | Explicitly notes "quality much higher with subagents" | 🟢 Works inline |

**executing-plans** is the designated fallback when subagents aren't available. It explicitly says:
> *"Superpowers works much better with access to subagents... If subagents are available, use subagent-driven-development instead of this skill."*

## Practical Adaptation for Pi

### The Golden Path (No Subagents Needed)

```
brainstorming → writing-plans → executing-plans → finishing-a-branch
```

All four work in pi. The "inline execution" approach (executing-plans) is the natural fit.

### What You Lose

- No parallel work on independent tasks/failures
- No automated per-task spec review + code quality review (review becomes inline/manual)
- No skill-testing workflow (writing-skills needs subagents to pressure-test)

### What Works Fine

- TDD, debugging, verification, git worktrees, branch finishing — all tool-agnostic
- brainstorming and writing-plans — pure conversation + file writing
- receiving-code-review — pure behavioral discipline

### Tool Mapping Quick Reference

| Claude Code / Copilot | Pi Equivalent |
|------------------------|---------------|
| `Skill` tool | Loaded automatically into system prompt; use `read` to refresh |
| `Task` tool (subagents) | Not available — use inline execution instead |
| `TodoWrite` tool | Track checklists inline in conversation or in plan files |
| File tools (`Read`, `Write`, `Edit`, `Bash`) | Same tools in pi (`read`, `write`, `edit`, `bash`) |
| `gh` CLI for PRs/issues | Same — available via `bash` |
