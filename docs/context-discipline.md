# Context Discipline

Context discipline means giving the AI assistant enough information to solve the task, but not so much that it wastes tokens, gets distracted, or creates unnecessary work.

This is one of the most important skills for effective AI coding.

## Why context matters

AI assistants use context to understand your code and your intent.

But context has trade-offs:

- More context can improve accuracy.
- Too much context can increase cost.
- Irrelevant context can confuse the assistant.
- Long conversations can carry outdated assumptions.
- Large workspace scans can burn tokens quickly.

The goal is not to starve the model.

The goal is to feed it the right context.

## Simple rule

> If old context is no longer helping, it is probably costing you.

## Start a new chat when needed

Start a new chat when:

- You are switching to a different feature.
- The old conversation contains outdated decisions.
- The assistant keeps referencing old context.
- The chat has become slow.
- The topic has changed.
- You need a small answer but the conversation is huge.

A fresh chat often produces cleaner answers.

## Compact long conversations

If your tool supports compaction or summarization, use it.

But timing matters: compact while the session is still active (the cache is warm), not after a long idle break. See [Compaction and the Prompt Cache](compact-and-caching.md) for why a cold cache can cost ~10× more.

A useful compacted summary should include:

- Current goal
- Important decisions
- Relevant files
- Known constraints
- Open questions
- Next step

Example:

```text
We are implementing budget alerts for AI assistant usage.

Relevant files:
- usage-tracker.ts
- billing-service.ts
- model-router.ts

Decisions:
- Warn at 70%.
- Warn again at 90%.
- Block non-admin users at 100%.
- Admins can override.
- Usage events must be auditable.

Next step:
Design API and database changes before writing code.
```

## Be specific about files

Bad:

```text
Review the whole repo.
```

Better:

```text
Review only these files:
- src/auth/session.ts
- src/api/login.ts
- src/api/refresh-token.ts

Problem:
Users are logged out after token refresh.

Do not edit files yet.
First list likely causes.
```

## Ask before scanning large context

Add this instruction to your AI coding workflow:

```text
Before scanning many files or using the whole workspace as context, tell me what files you need and why.
```

## Use context in layers

A good pattern:

1. Give a short problem statement.
2. Provide relevant files only.
3. Ask for analysis.
4. Ask for a plan.
5. Approve the plan.
6. Ask for a small implementation.
7. Review the diff.

Avoid jumping directly from “here is everything” to “write code.”

## Context checklist

Before asking AI:

- What problem am I solving?
- Which files are actually relevant?
- Do I need a plan or code?
- Is this chat still clean?
- Can a smaller model solve this?
- Should I ask AI to inspect first before editing?
- What output format do I want?

## Good prompt

```text
I am debugging a login issue.

Relevant files:
- src/auth/session.ts
- src/api/login.ts
- src/api/refresh-token.ts

Problem:
Users are logged out after token refresh.

Please inspect only these files first.

Output:
- Likely causes
- Evidence from the code
- Recommended next step

Do not write code yet.
```

## Key takeaway

Context is not free.

Use it intentionally.
