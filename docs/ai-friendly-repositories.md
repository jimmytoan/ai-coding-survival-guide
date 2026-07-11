# Make a Repository AI-Friendly

AI assistants work best in repositories that are also friendly to new human contributors: clear boundaries, deterministic commands, fast feedback, and documented invariants.

The goal is not to describe every detail to the model. It is to make the correct path easy to discover and verify.

## Provide a reliable entry point

A repository should make these questions cheap to answer:

- What does this project do?
- How do I set it up?
- Where are the important components?
- How do I build, test, lint, and type-check it?
- Which files are generated or must not be edited?
- What are the critical architectural and domain rules?
- What constitutes a complete change?

Use the README for human orientation and a concise project instruction file for agent-specific working agreements. See [Project Context Files](project-context-files.md).

## Make commands deterministic

Prefer a small, documented command surface:

```text
make setup
make test-unit
make test-integration
make lint
make check
```

The exact tool does not matter. The properties do:

- Commands work from a documented directory.
- Versions and dependencies are pinned appropriately.
- CI and local development run equivalent checks.
- Test selection is predictable.
- Failures return nonzero and expose useful diagnostics.
- A quiet or focused mode exists for large output.

If the canonical command requires tribal knowledge, an agent will guess—and so will a new teammate.

## Put rules at the right layer

Use prose for judgment and automation for mechanical enforcement.

| Need | Better home |
|---|---|
| Formatting | Formatter |
| Import order and simple style | Linter |
| Type contracts | Type checker or schema |
| Required tests | CI |
| Dependency policy | Package tooling or policy check |
| Generated-file protection | Generator/check script |
| Architectural invariant | Tests plus concise documentation |
| Task-specific constraint | Issue or prompt |
| Durable agent convention | Project instruction file |

Do not spend context repeatedly telling an agent what a tool can enforce deterministically.

## Document invariants, not history lessons

High-value repository instructions include statements such as:

- All money values are integer minor units.
- Authorization is enforced in service methods, not controllers.
- Migrations must remain compatible with the previous application version.
- `api/generated/` is produced by `make generate`; never edit it directly.
- Tests must not make live network calls.

Include the reason when it prevents a plausible mistake. Move long runbooks and occasional workflows to on-demand documents or skills.

## Keep architecture discoverable

Useful artifacts include:

- A short directory map
- Service and ownership boundaries
- Public interfaces and schemas
- Architecture decision records for surprising choices
- Data-flow or threat-boundary diagrams where prose is insufficient
- Examples of one well-implemented feature
- Links from code to the source of truth for generated interfaces

Prefer current, small documents over a large architecture wiki nobody trusts.

## Build fast feedback loops

Agents improve when they can test hypotheses quickly:

- Focused unit and contract tests
- Hermetic fixtures and seed data
- Local substitutes for external services
- Type checking and linting that finish quickly
- Clear error messages with relevant context
- Reproduction scripts for common failure modes
- Preview environments or scripted UI checks

A slow, flaky suite encourages speculative coding and fewer verification cycles. Track flaky tests as defects rather than teaching the assistant to retry them.

## Make boundaries machine-checkable

Where practical, encode contracts through:

- Types and schemas
- Database constraints
- API contract tests
- Module-boundary checks
- Security and dependency scanning
- Secret detection
- Migration validation
- Golden fixtures for stable formats
- Performance budgets for critical paths

The model can then use those checks as objective feedback instead of interpreting a paragraph of preferences.

## Preserve a clean change surface

Help both agents and reviewers by:

- Keeping generated artifacts separate and reproducible
- Excluding build output and vendor directories from normal search
- Avoiding unrelated formatting churn
- Providing small example data rather than production dumps
- Keeping commits and pull requests scoped
- Stating how to handle a dirty working tree
- Making ownership and approval boundaries visible

## Add an agent-ready task template

Issues that contain goal, behavior, scope, acceptance criteria, and verification are easier for any coding agent to execute safely.

Start with the [Task Brief](../templates/task-brief-template.md). For recurring tasks, turn the workflow into a script, command, or on-demand skill rather than copying an ever-growing prompt.

## Repository audit

- [ ] Setup succeeds from documented steps
- [ ] Build, test, lint, and type-check commands are explicit
- [ ] A focused test command exists
- [ ] CI matches local verification
- [ ] Generated and vendored files are identified
- [ ] Critical domain and security invariants are documented
- [ ] Mechanical rules are enforced by tools
- [ ] Project instructions are concise and scoped
- [ ] Architecture and ownership boundaries are discoverable
- [ ] Test fixtures do not require sensitive production data
- [ ] Errors and logs support diagnosis without leaking secrets
- [ ] Contributor tasks include acceptance and verification criteria

## Key takeaway

The best “AI optimization” is often ordinary repository engineering: make intent visible, feedback fast, and correctness executable.
