# Software Development Tips

Habits that make any change safer to ship. Each one is cheap, and each pairs well with coding agents because it gives them, and you, something concrete to check. Every section ends with a **With an agent** note that says how to apply the habit when an assistant does part of the work.

For what to study, see [Engineering Fundamentals](engineering-fundamentals.md). For habits that are specific to agent tooling, see [Field Notes](field-notes.md).

## Scope and slicing

- **Ship the smallest vertical slice** that proves the idea end to end, then widen it.
- **Make the change easy, then make the change.** If the code resists, refactor first in a separate commit with no behavior change.
- **Separate refactors, behavior changes, and migrations** into different commits or PRs so each can be reviewed and reverted alone.
- **Wait for the third repetition** before abstracting. Two copies are cheaper than the wrong abstraction.
- **Delete code.** Unused code still costs reading time, test time, and review attention. Prefer boring, well-known technology unless you can name what the new tool buys.
- **Timebox spikes.** Decide the question and the stopping time first, then keep the spike code out of `main`.
- **With an agent:** Hand it one slice with acceptance criteria and explicit non-goals, and request the refactor and the behavior change as separate commits or sessions so each diff is reviewable alone. Agents invent scope when it is missing; the [Task Brief Template](../templates/task-brief-template.md) covers the inputs.

## Git history

- Commit one logical change at a time and explain *why* in the message; the diff already shows *what*.
- Keep `main` releasable. Use short-lived branches, and put unfinished work behind a flag.

```bash
git add -p                              # stage hunks, not whole files
git restore -p                          # discard selected hunks
git stash push -p                       # stash selected hunks
git commit --fixup <sha>                # mark a correction for an earlier commit
git rebase -i --autosquash <base>       # fold fixups into place before review
git log -S"needle" --oneline            # find commits that added or removed a string
git blame -w -C -C <file>               # blame ignoring whitespace and moved code
git reflog                              # recover work after a bad reset or rebase
git worktree add ../feature-x -b feature-x   # second checkout, same repository
git range-diff <old-base>..<old-tip> <new-base>..<new-tip>   # compare two versions of a branch
```

To find the commit that broke something, give git a script that exits non-zero on failure:

```bash
git bisect start <bad> <good>
git bisect run ./check.sh
git bisect reset
```

- **With an agent:** Commit and push only when you ask. Split a large agent diff into reviewable commits with `git add -p`, and use one worktree per parallel agent session. Checkpoint and rewind features in agent tools may not track files changed only by shell commands, so treat git as the real safety net and commit before a risky run.

## Debugging

- **Reproduce before you theorize.** Reduce to the smallest input that still fails, and save it as a test.
- **Change one thing at a time** and write down what you expected before you run it.
- **Bisect the space:** history with `git bisect`, input by halving, code by commenting out regions, config by diffing against a working environment.
- **Read the whole error**, including the first and last stack frames and the "caused by" chain.
- **Check assumptions with evidence**, such as a log line, an assertion, or a debugger, instead of rereading the code harder.
- **Fix the class, not the instance.** After the fix, ask where else the same mistake can occur.
- **Keep a short debugging log** of hypotheses and results. It stops you repeating experiments and makes a good handoff.
- **With an agent:** Give it the reproduction, expected versus actual behavior, and the relevant logs rather than your theory. Ask for ranked hypotheses and the cheapest experiment that separates them, and have it write the failing test before the fix. See [Debugging with AI](debugging-with-ai.md) and the [Debugging Prompt](../templates/debugging-prompt.md).

## Testing

- Test **behavior at public boundaries**, not private implementation, so refactors do not break tests.
- Make tests **deterministic**: inject the clock, seed randomness, avoid `sleep`, and isolate shared state.
- **Treat a flaky test as a bug.** Quarantine it with an owner and a deadline instead of rerunning CI until it passes.
- Use **property-based tests** for parsers, serializers, and anything with invariants.
- Add **contract tests** where services meet, and keep a few end-to-end tests for the money path.
- Keep the **local feedback loop fast**. If the focused test takes minutes, people and agents stop running it.
- Check test quality by deliberately breaking the code. If nothing fails, the test is decoration.
- **With an agent:** Have it run the existing suite first to set a baseline, then write the failing test before the fix (red/green). Review generated tests as critically as production code: look for skipped tests, weakened assertions, broad snapshot updates, over-mocking, and a test command that selects zero tests. See [Verification and Testing](verification-and-testing.md).

## Design and APIs

- **Validate at the boundary** and keep the interior simple. Make illegal states unrepresentable where the type system allows.
- **Make writes idempotent** with a request key so retries cannot double-charge or double-send.
- **Put a timeout on every network call.** Retry only idempotent work, with backoff, jitter, and a total budget.
- **Change data in compatible steps:** add the new shape, write both, backfill, read the new shape, then remove the old one.
- **Version contracts** and add fields before removing fields.
- **Give flags an owner and an expiry date.** An old flag is a hidden branch in production.
- **Paginate by default** and return stable error shapes that clients can handle.
- **Write the decision down** (context, options, choice, consequences) when it will be hard to reverse. See [Learn While Coding with AI](learning-with-ai.md).
- **With an agent:** Models imitate nearby code, so state the contract, idempotency, timeout, and compatibility rules in the task or a short instruction file instead of letting them be guessed. For a large or cross-cutting change, ask for a short plan first and review it before any edits. See [Project Context Files](project-context-files.md).

## Production and operations

- Emit **structured logs** with a correlation ID, and include enough context to answer "which request, which user class, which version."
- Track **rate, errors, and duration** per service, and alert on symptoms users feel rather than on every internal metric.
- **Deploy small and often.** Small releases localize failures; know the rollback before you deploy.
- Read configuration from the **environment** and secrets from a **secrets manager**, never from the repository. Give each workload the least privilege it needs.
- **Test restores, not only backups.** A backup you have never restored is a hope.
- Keep a **runbook** for each alert: what it means, how to check, how to mitigate, who to ask.
- Run **blameless postmortems** that end in a concrete check, alert, or test.
- **With an agent:** Keep production credentials out of its environment and give it read-only access for investigations. Require approval for deploys, migrations, and other external writes, and treat logs and tickets it reads as untrusted input that may contain instructions. See [Agent Mode](agent-mode.md) and [Security and Privacy](security-and-privacy.md).

## Performance

- **Measure first.** Profile the real workload and fix the largest cost, not the one that looks slow.
- Watch for **N+1 queries**, missing indexes (`EXPLAIN` the query), unbounded result sets, and chatty network calls.
- Add a **cache only with an invalidation story** and a metric for hit rate.
- Set a **budget** (latency, memory, cost per request) and make CI or a dashboard notice when it is exceeded.
- **With an agent:** Ask for measurements before and after the change, with the command used. Reject "this should be faster" claims that come without numbers, and have it profile before it optimizes.

## Code review

- **Authors:** keep PRs small, review your own diff first, and say in the description what changed, why, and how you checked it.
- **Reviewers:** check correctness, tests, failure paths, and security before style. Mark comments as blocking or optional, and ask a question when you are unsure.
- Google's review guidance says to approve a change once it clearly improves the code's health, even if it is not perfect. Respond quickly, because slow review pushes people toward bigger batches. See [Google's code review guide](https://google.github.io/eng-practices/review/).
- **With an agent:** Read every changed file, including tests, lockfiles, CI, and build configuration. For meaningful work, have a fresh session review the diff against the original requirement, and tell it to report correctness gaps and skip style, because a reviewer asked to find problems will always find something. Apply the extra checks in [Reviewing AI-Generated Code](reviewing-ai-generated-code.md) and never submit a diff you cannot explain.

## Dependencies

- Commit lockfiles, pin versions, and review the diff when they change.
- Ask what each new dependency replaces, who maintains it, its license, and its advisory history. See [Dependencies and API Research](dependencies-and-api-research.md).
- Update on a schedule with CI, in small batches, so one breaking upgrade is easy to find.
- **With an agent:** Never install a package the model suggested until you have verified the exact name, owner, version, and license in a primary source, because plausible names can be wrong or malicious. Review lockfile changes in the diff. See [Dependencies and API Research](dependencies-and-api-research.md) and the [Dependency Evaluation](../templates/dependency-evaluation-prompt.md) prompt.

## Shell and tooling

A small toolbox pays back daily.

```bash
rg -n "TODO|FIXME" src                  # fast code search with line numbers
jq -r 'select(.level=="error") | "\(.req) \(.msg)"' app.log   # filter JSON logs
curl -s -o /dev/null -w '%{http_code} %{time_total}s\n' "$URL" # status and latency
set -euo pipefail                       # fail fast in bash scripts
```

- Put the build, test, lint, and run commands in a `Makefile`, `justfile`, or package script so humans, CI, and agents use the same entry points.
- Use pre-commit hooks and a formatter so style never reaches review.
- Use a dev container or lockfile-based toolchain so "works on my machine" becomes reproducible.
- **With an agent:** Agents can run the same CLIs you do (`git`, `gh`, `rg`, `jq`) with no setup, and a CLI is usually leaner than an MCP server for the same job. Pipe filtered output instead of pasting whole logs, and list the canonical test and lint commands in the instruction file. See [MCP and Tool Overhead](mcp-and-tool-overhead.md).

## Documentation and communication

- A README should get a newcomer from clone to a running test in about five minutes. Try it on a clean machine.
- Document **why** and **what is surprising**. Code and tests already show how.
- Estimate in **ranges** with named risks, and update early when the risk materializes.
- Say "no" with options: what you can do by the date, what it costs, and what you would cut.
- Write for the reader who joins in six months and has none of today's context.
- **With an agent:** That reader now includes the agent, which starts every session with no memory. Record the commands, gotchas, and decisions it cannot infer from the code in a short instruction file, and keep it current. You can have an agent draft a decision record or postmortem from your notes, but verify every fact before it is published.

## Habits

- Start the day by writing the one outcome that would make it a good day.
- Take notes while you work; they become the PR description, the decision record, and the postmortem.
- Pair or ask for review on risky changes before the code is polished.
- Protect time to learn something fundamental each week. See [Engineering Fundamentals](engineering-fundamentals.md).
- **With an agent:** Write the outcome and acceptance criteria before you prompt, since that note becomes the task brief. Keep a handoff note when you switch sessions, and do some work unaided each week so you can still tell when the assistant is wrong. See the [Handoff Template](../templates/handoff-template.md).

## Key takeaway

Small changes, reproducible checks, clear decisions, and a tested way back. Those habits make both human and AI-assisted work easier to trust.
