# Model Selection

Bigger is not always better.

The best model is not always the strongest model.

The best model is the cheapest model that can reliably solve the task.

## The simple rule

> Use small models for simple tasks.  
> Use stronger models when mistakes are expensive.

## Use smaller / cheaper models for

- Summarizing code
- Explaining simple functions
- Drafting README sections
- Writing basic unit tests
- Renaming variables
- Cleaning up comments
- Generating examples
- First-pass brainstorming
- Small code transformations
- Explaining error messages
- Creating simple checklists

Example prompt:

```text
Explain this function in simple terms.

Focus only on:
- What it does
- Inputs and outputs
- Edge cases

Do not suggest refactoring yet.
```

## Use stronger models for

- Implementation planning
- Architecture decisions
- Security-sensitive logic
- Authentication
- Authorization
- Data migration
- Financial calculations
- Multi-file refactoring
- Complex debugging
- Production incident analysis
- Final review before merge

Example prompt:

```text
Create an implementation plan before writing code.

Goal:
Add token budget alerts to the AI assistant platform.

Output:
- Files likely affected
- Data model changes
- API changes
- UI changes
- Test plan
- Risks
- Questions before implementation

Do not write code yet.
```

## Why small models are underrated

Small models are often good enough for many everyday coding tasks.

They may be faster, cheaper, and less wasteful.

Good uses:

- “Explain this error.”
- “Summarize this function.”
- “Draft a unit test.”
- “Improve this comment.”
- “Convert this snippet to TypeScript.”
- “Find obvious duplicated logic.”

Do not use a large model just because it is available.

Use it because the task deserves it.

## Model choice table

| Task | Suggested model |
|---|---|
| Simple explanation | Small / cheap |
| README draft | Small / cheap |
| Basic test generation | Small / cheap |
| Simple refactor | Small / medium |
| Bug investigation | Medium / strong |
| Implementation planning | Strong |
| Architecture review | Strong |
| Security-sensitive code | Strong |
| Production incident | Strong |
| Multi-file refactor | Strong |
| Final review before merge | Strong + human review |

## Watch for model overkill

You may be overusing strong models if:

- You use the biggest model for every question.
- You use agent mode for one-line changes.
- You ask a frontier model to format markdown.
- You use an expensive model before you understand the problem.
- You use a strong model to fix issues created by a previous careless strong-model run.

## Recommended workflow

1. Start with a smaller model for understanding.
2. Use a stronger model for planning if needed.
3. Use focused implementation prompts.
4. Review output carefully.
5. Use a smaller model for cleanup if the risk is low.
6. Use human judgment for final decisions.

## Key takeaway

Strong models are valuable.

That is exactly why they should be used intentionally.
