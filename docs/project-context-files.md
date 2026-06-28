# Project Context Files: The Tokens You Pay Before You Type

> **TL;DR** Your project rules file (CLAUDE.md, `.cursorrules`, `AGENTS.md`, copilot-instructions, etc.) is loaded into context **at the start of every session** and rides along on every turn. A bloated rules file is a tax on every message — even unrelated ones. Keep it short (aim for **under ~200 lines** of genuine essentials), and move specialized, occasional workflows out of it into on-demand mechanisms like skills or custom commands so their tokens only load when you actually use them.

This is [Context Discipline](context-discipline.md) applied to the context you do not type — the same lesson as [MCP and Tool Overhead](mcp-and-tool-overhead.md), but for your own config files.

## Why these files are different

Most context is something you add deliberately: you paste a file, you ask a question. Project rule files are different — the tool loads them **automatically, every session, for you.** That is exactly what makes them useful (consistent conventions without re-explaining) and exactly what makes them a hidden cost: every line is present whether or not this task needs it.

If your CLAUDE.md contains a 120-line "how to do a database migration" runbook, those tokens are in context when you are fixing a typo in the README. Multiply by every message in the session and by every session you ever run.

## Keep the always-on file lean

A good project file holds only what is true for **almost every task**:

- Build / test / lint commands
- Critical conventions and non-obvious gotchas
- Directory map and where things live
- Hard rules ("never touch generated files", "all money is in cents")

What does **not** belong in the always-on file:

- Step-by-step runbooks for occasional workflows (migrations, releases, incident response)
- Long style guides that a linter already enforces
- Background/history that is nice-to-know but rarely load-bearing
- Anything you would skim past on most tasks

Rule of thumb: if a section is only relevant to one kind of task, it should load only when you do that task — not on every turn.

## Move occasional workflows to on-demand context

The fix is not to delete useful instructions — it is to **load them when needed instead of always.**

- **Skills / custom commands** load on demand. A `db-migration` skill or a `/release` command carries its detailed instructions only when invoked, keeping your base context small. Moving a workflow out of CLAUDE.md and into a skill is a direct token saving on every unrelated turn.
- **Hooks** can preprocess data before the model sees it — e.g. filter a test run to only failures, or grep a log for `ERROR` and return a few lines instead of tens of thousands. The model gets the signal without the noise.

The pattern: **always-on for essentials, on-demand for specialties.**

## Two more levers worth knowing

These are not files, but they are the same idea — context and reasoning you pay for by default:

- **Plan before big changes.** Many tools have a dedicated plan mode (in Claude Code, Shift+Tab) where the agent explores and proposes an approach for approval *before* editing. Approving a plan is far cheaper than letting the agent take an expensive wrong path and then unwinding it. If the agent starts heading the wrong way, stop it early (Escape) and use `/rewind` rather than paying to compound the mistake. See [Agent Mode](agent-mode.md).
- **Right-size reasoning.** Extended thinking improves hard planning and debugging, but thinking tokens are billed as output and the default budget can be large. For simple tasks that do not need deep reasoning, lower the effort/thinking level. Spend reasoning where it changes the answer, not on formatting markdown.

## Quick audit

Once in a while, open your project file and ask:

- Is every section relevant to *most* tasks? If not, can it become a skill or command?
- Could a linter, formatter, or hook enforce this instead of prose the model re-reads every turn?
- Is the file under ~200 lines of real essentials?
- Run `/context` (Claude Code) to see how much of your window the file and tools actually take.

## Key takeaway

Anything loaded automatically is paid for repeatedly.

Keep the always-on file small; let specialized instructions load only when the task calls for them.

## Sources

- Claude Code Docs — *Manage costs effectively* (move instructions from CLAUDE.md to skills, keep under 200 lines; hooks/skills preprocessing; plan mode; extended thinking effort): <https://code.claude.com/docs/en/costs>
- Claude Code Docs — *Memory (CLAUDE.md)*: <https://code.claude.com/docs/en/memory>
- Claude Code Docs — *Skills*: <https://code.claude.com/docs/en/skills>
