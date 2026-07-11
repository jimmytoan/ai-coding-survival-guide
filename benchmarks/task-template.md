# Benchmark Task Template

Create one copy per task. Give every compared workflow the same participant-visible
section. Keep evaluator-only checks private until the run ends.

## Metadata

```text
Task ID:
Task type:
Risk/complexity: low / medium / high
Repository revision:
Environment/setup command:
Time limit:
Attempt count:
Allowed tools and permissions:
Network access: yes / no / limited
Context strategy:
```

## Participant-visible task

```text
Goal and current behavior:
[Describe]

Acceptance criteria:
- [Observable requirement]

In scope:
- [List]

Out of scope:
- [List]

Constraints:
- [List]

Required verification and handoff:
- [Checks the participant may know]
- [Expected final response]
```

## Starting artifacts

```text
Files supplied initially:
Issue/design supplied:
Known test command:
Seed data or services:
```

## Evaluator-only rubric

Do not reveal hidden checks or seeded defects during the run.

```text
Acceptance-criterion checks:
- [Criterion]: [Test or inspection method]

Hidden regression/security checks:
- [Check]

Expected scope:
- Files likely to change:
- Files that must not change:

Known valid solution properties:
- [Property, not exact implementation]

Automatic failure conditions:
- [Severe security issue, data loss, broken build, etc.]
```

## Human-only baseline

Use the same starting state and completion standard.

```text
Baseline date:
Engineer experience level:
Human implementation time:
Review and rework time:
Checks result:
Notes:
```

## Run hygiene

- Restore the frozen revision and environment before every run.
- Start a fresh conversation with no result from an earlier attempt.
- Preserve raw output, final diff, check logs, and evaluator notes.
- Record interventions and failures; do not silently repair the result.
