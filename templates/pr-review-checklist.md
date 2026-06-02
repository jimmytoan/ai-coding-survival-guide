# PR Review Checklist for AI-Generated Code

Use this when reviewing code produced with AI assistance.

## Scope

- [ ] Does the PR solve the stated problem?
- [ ] Are unrelated files untouched?
- [ ] Is the diff small enough to review?
- [ ] Did the AI introduce unnecessary abstraction?

## Correctness

- [ ] Are edge cases handled?
- [ ] Are errors handled clearly?
- [ ] Are behavior changes intentional?
- [ ] Are assumptions documented?

## Code quality

- [ ] Is the code simple?
- [ ] Is there duplicated logic?
- [ ] Is there redundant code?
- [ ] Are names clear?
- [ ] Is the implementation consistent with existing patterns?

## Tests

- [ ] Are tests added or updated?
- [ ] Do tests cover important edge cases?
- [ ] Are tests meaningful, not just snapshot noise?
- [ ] Did the assistant update tests to match wrong behavior?

## Security

- [ ] Does the change affect authentication?
- [ ] Does the change affect authorization?
- [ ] Does it expose sensitive data?
- [ ] Does it weaken validation?
- [ ] Does it introduce unsafe dependencies?

## Final check

- [ ] Would I be comfortable owning this code?
- [ ] Can I explain the change without relying on “AI wrote it”?
