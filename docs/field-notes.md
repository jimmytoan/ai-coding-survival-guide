# Field Notes: Tips That Hold Up

> Last verified: **2026-10-31**. Tips are tagged **[official]** (vendor documentation for the vendor's own current models) or **[practitioner]** (blog or forum report, which is anecdote and not measurement). Re-test anything on older models or a different tool.

Models improved faster than most advice. This page separates habits worth keeping from workarounds that current models no longer need. It adds to, and does not replace, the [AI Coding Workflow](ai-coding-workflow.md) and [Prompt Patterns](prompt-patterns.md).

## Advice that has aged out

| Old habit | What the vendors now say |
|---|---|
| Manage the context window aggressively | **[official]** With 1M-token windows and automatic compaction, running out is rare. `/clear` between tasks is still worth it because a focused context answers better and costs less. |
| Add `IMPORTANT` or ALL CAPS to ignored rules | **[official]** Current models follow a plain line, and emphasis makes them over-apply it. If a rule cannot bend, make it a hook or deny rule; if it gets lost, shorten the file. |
| Write "run the tests" into every instruction file | **[official]** For GPT-6 Astra, generic reminders cause needless test runs. Grant permission for specific safe checks and put "run and fix failures" in the task definition. |
| Spell out step-by-step recipes in skills | **[official]** Detailed itineraries can constrain a strong model. Describe the goal and the constraints. |
| Long skill descriptions and monolithic skill roots | **[official]** Keep descriptions short and say when the skill applies. Make the root a minimal router to supporting docs. |
| Say `think hard` or `be thorough` | **[official]** Those are ordinary words. The effort setting controls reasoning; `ultrathink` still raises one turn. |
| Delegate to a subagent for a better answer | **[official]** The same task on a subagent is not better. A fresh *reviewer* helps because it has not seen your reasoning. |
| Describe your architecture in the instruction file | **[official]** The agent reads the repo. Record only what is not in it. |

If you still use older or smaller models, some of this habit is still useful. Test before you delete it.

## Habits that keep paying off

### Give the agent a check that returns pass or fail

**[official]** The most consistent advice across vendors. An agent stops when work looks done, so without a check you become the test suite. Put the exact test, typecheck, and lint commands, including how to run one file, in the instruction file. Ask for evidence: the command, the output, or a screenshot, not "all tests pass." See [Verification and Testing](verification-and-testing.md).

### Interview first, then spec, then a fresh session

**[official]** When you do not yet know what you want, ask: "Interview me until you could write the spec, then write it to `SPEC.md`." Build from the spec in a clean session so it starts with a written definition of done, and point the reviewer at the same file.

### Rewind instead of correcting a third time

**[official]** After two failed corrections the conversation holds two wrong attempts for the model to reread. Rewind or `/clear`, and write one prompt that folds in what you learned. **[practitioner]** Several community checklists repeat this.

### Decide what is enforced and what is advice

**[official]** Instructions in a prompt, instruction file, or skill are advice the model may decline. Hooks, deny rules, sandboxes, CI, and branch protection are enforced. If you keep adding emphasis to a line, promote it to a hook. If you have hooks for mere preferences, delete them. Keep the enforced set small, because each hook eventually fires when it should not.

### Keep the instruction file a short map

**[official]** OpenAI reports that one giant instruction file hurt results and replaced it with a short map (about 100 lines) into a `docs/` system of record. Anthropic recommends under about 200 lines and only what the agent cannot find in the code. Add a rule after an observed failure, and after the same mistake twice, ask for a retrospective and update the file. See [Project Context Files](project-context-files.md).

### Use a progress file for long work

**[official]** Anthropic describes structured note-taking outside the context window. **[practitioner]** In an OpenAI forum thread, users keep a spec, milestones with acceptance criteria, a runbook, and a live `progress.md`, then start a fresh session that reads them. Use the [Handoff Template](../templates/handoff-template.md).

### Review in a fresh context, and tell the reviewer what to skip

**[official]** A reviewer that is asked to find problems will always find something. Ask for correctness gaps against the spec and say `skip style`, or you will accumulate abstractions and tests for cases that cannot happen. See [Reviewing AI-Generated Code](reviewing-ai-generated-code.md).

### Isolate parallel work with worktrees

**[official]** One checkout per parallel session so edits cannot collide. A fresh worktree lacks gitignored files such as `.env`, so list the small ones you need in `.worktreeinclude` and keep the install command in the instruction file. OpenAI lists running live tasks on the same files without worktrees as a common mistake.

### Make the repository easy for an agent

**[practitioner]** Hacker News commenters on OpenAI's harness-engineering post report that small files help because agents load whole files, that a local environment mirroring production gives the agent a way to verify, and that CI checks for duplicate code and file length plus pre-push hooks catch drift. Turning each repeated failure into a deterministic check works better than adding prose. See [AI-Friendly Repositories](ai-friendly-repositories.md).

### Prefer a CLI to an MCP server when one exists

**[official]** If a system has a CLI (`gh`, for example), the agent can use it with no setup. Connected MCP servers add tool names and instructions to context even when unused, so turn off the ones you do not need. **[practitioner]** Engineers building their own agents repeat this and note that tool bloat creates ambiguity. See [MCP and Tool Overhead](mcp-and-tool-overhead.md).

### Write the test first, and have the agent run the suite first

**[practitioner]** Simon Willison's agentic engineering guide includes chapters on red/green TDD, "first run the tests," and agentic manual testing, where the agent drives a browser or API and records what it saw. The shared idea is that a failing test or a recorded run defines done better than a description.

### Say how much you want

**[official]** Models read requests conservatively. `be ambitious` plus a list of what "complete" includes (empty and error states, keyboard navigation) tells the agent about scope, which only you know.

## Capacity tactics

- `/clear` between unrelated tasks, and give `/compact` a focus such as "keep the API changes and the test commands." **[official]**
- Check `/usage` before changing anything. It breaks usage down by skill, subagent, plugin, and MCP server and flags long context and cache misses. **[official]**
- Remember that a `/loop`, a goal check after every turn, and subagents keep spending while you are away. **[official]**
- Send simple subagent work to the smallest model; each subagent adds its own requests. **[official]**
- Use medium or high effort for routine work and save the highest setting for hard problems. Anthropic says `max` tends to overthink ordinary tasks **[official]**, and an OpenAI forum thread gives the same advice for `xhigh` **[practitioner]**. See [Models and Plan Limits](models-and-plan-limits.md).
- Point at files instead of pasting them, and prune tools and stale output. **[official]**

## Running unattended

**[official]** Before a run goes unattended, answer five questions: does it know what done looks like, can it check its own work, what is it allowed to touch, what stops it, and where does the result go and who signs off. Use turn or spend caps, protect `main` with a branch rule the task cannot bypass, and try a script on three files before the rest. See [Agent Mode](agent-mode.md).

## Cautions and disputed claims

- **Throughput is not quality.** Commenters quoting OpenAI's post cite about 1M lines and 1,500 merged PRs in five months. The same thread notes that lines and PR counts reward bloat and says the post shows scale rather than maintainability. Count defects, rework, and review time instead.
- **Approvals get rubber-stamped, and sandboxes leak.** **[practitioner]** Commenters on a minimal-agent post argue that per-command prompts lose value after the tenth, and that a sandbox with open network access still allows exfiltration through tools like `curl`. This matches Anthropic's advice to use deny rules plus a sandbox for anything that matters.
- **Harness upkeep is real work.** **[practitioner]** Build the harness progressively, and promote a prompt to a skill and a repeated fix to a script only after it recurs.
- **Vendor guidance is vendor-specific.** The "stop doing" lists describe each vendor's current models. They do not carry over to other models or older versions.
- **AI amplifies what exists.** DORA's research on AI-assisted development describes AI as an amplifier of both strong practices and existing dysfunction. Tests, small changes, and clear ownership come first.

## Ten-minute setup for a new repository

1. List the install, run-one-test, typecheck, and lint commands in the instruction file.
2. Add deny rules for `.env` and secrets, and enable the sandbox for risky work.
3. Run the instruction-file audit your tool offers, such as `/doctor prompt-audit` in Claude Code, and delete lines the agent can infer.
4. Add one enforced check, such as a `Stop` hook or CI job, for the command that must always pass.
5. Save a `SPEC.md` prompt that asks the agent to interview you.
6. Create a `progress.md` convention for work that spans sessions.
7. Decide when to use worktrees, and add `.worktreeinclude` for the small files they need.
8. Save the review prompt with `skip style` and the original spec.

## Sources

- Anthropic — [Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- Anthropic — [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- OpenAI — [Codex best practices](https://learn.chatgpt.com/guides/best-practices)
- OpenAI — [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
- OpenAI — [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/) (the site blocked scripted fetches; claims about it come from the discussion below and search excerpts)
- Hacker News — [Discussion of the harness-engineering post](https://news.ycombinator.com/item?id=48416264)
- Hacker News — [Discussion of building a minimal coding agent](https://news.ycombinator.com/item?id=46844822)
- OpenAI Developer Community — [Tips and tricks for using Codex](https://community.openai.com/t/tips-and-tricks-for-using-codex/1373143)
- Simon Willison — [Agentic Engineering Patterns](https://simonwillison.net/guides/agentic-engineering-patterns/) (chapter list; summaries inferred from titles)
- DORA — [State of AI-assisted Software Development](https://dora.dev/research/ai)

## Key takeaway

The durable habits are a pass/fail check, a short map instead of a long manual, enforced rules for the things that cannot bend, a fresh reviewer, and isolated parallel work. Drop the workarounds your model no longer needs.
