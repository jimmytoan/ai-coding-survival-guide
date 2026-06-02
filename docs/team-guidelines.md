# Team Guidelines for AI Coding Assistants

This is a lightweight guideline for engineering teams using AI coding assistants.

The goal is not to restrict AI usage.

The goal is to make usage effective, safe, and sustainable.

## Team principle

> Use AI to improve engineering quality and speed, not to create faster review burden.

## Encourage AI usage for low-risk work

AI assistants are useful for:

- Code explanation
- Documentation drafts
- Unit test drafts
- Error explanation
- Small refactors
- Pull request summaries
- Debugging assistance
- Learning unfamiliar code
- Generating examples
- Implementation planning

## Use extra care for high-risk work

Extra review is required for:

- Authentication
- Authorization
- Security-sensitive code
- Infrastructure changes
- Production incident fixes
- Financial calculations
- Data migration
- Privacy-sensitive workflows
- Large multi-file refactors
- Generated code that changes business logic

## Token discipline

Developers should:

- Use the right model for the task.
- Try smaller models for simple tasks.
- Use stronger models for complex or high-risk work.
- Start new chats when old context is stale.
- Compact long conversations when possible.
- Avoid dumping an entire repo unless truly needed.
- Ask for a plan before implementation.
- Keep changes small and reviewable.
- Avoid agent mode for tiny tasks.

## Suggested internal message

```text
Everyone, please be mindful of token usage when using AI coding assistants.

These tools are useful and we should continue using them, but as more providers move toward usage-based pricing, we need to be more disciplined.

A few practical habits:

- Start a new chat when the conversation becomes too large or stale.
- Compact/summarize the conversation when the tool supports it.
- Use the right model for the task.
- Try smaller/cheaper models for simple tasks like summaries, explanations, docs, basic tests, or small code transformations.
- Use stronger models for complex or higher-risk work, such as implementation planning, architecture decisions, security-sensitive logic, or production changes.
- Avoid dumping the whole repo into AI and asking it to generate a large amount of code.
- Ask for a plan first, then implement in smaller reviewable steps.

The goal is not to use less AI.

The goal is to use AI better.
```

## Suggested review expectation

AI-generated code should be reviewed like human-written code.

Reviewers should check:

- Does the change solve the actual problem?
- Is the code simple?
- Is there duplicated logic?
- Are there unnecessary abstractions?
- Are tests meaningful?
- Are edge cases covered?
- Are security assumptions valid?
- Did the assistant touch unrelated files?
- Is the diff small enough to understand?

## Suggested model policy

| Work type | Suggested model |
|---|---|
| Simple explanations | Small / cheap |
| Documentation drafts | Small / cheap |
| Basic tests | Small / cheap |
| First-pass review | Small / medium |
| Implementation planning | Strong |
| Security review | Strong |
| Architecture decisions | Strong |
| Production incident analysis | Strong |

## Suggested agent mode policy

Agent mode is allowed when:

- The task is clear.
- The scope is defined.
- The developer reviewed the plan.
- The output will be reviewed before merge.
- The task is worth the cost.

Agent mode should not be used as a replacement for understanding the problem.

## Team metrics to track

Teams can track:

- Cost per useful task
- Time saved
- Output acceptance rate
- Review effort
- Bugs introduced
- Rework required
- Model used
- Agent mode usage
- Task category

See: [Cost Observability](cost-observability.md)

## Key takeaway

AI coding assistants are now part of engineering workflow.

Treat them like engineering tools:

- Useful
- Powerful
- Not magic
- Not free
- Better with good process
