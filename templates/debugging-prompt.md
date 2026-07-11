# Debugging Prompt

Use this to investigate a reproducible problem before changing code.

```text
Investigate this issue. Do not edit files yet.

Problem:
[Describe the user-visible failure]

Reproduction:
1. [Step]
2. [Step]

Expected behavior:
[Describe expected behavior]

Actual behavior:
[Describe actual behavior]

Evidence:
- Exact error or log excerpt: [Paste the smallest useful excerpt]
- Frequency: [Always, intermittent, or unknown]
- First known occurrence: [Version, commit, or date if known]
- Environment: [Runtime, OS, browser, flags, or deployment]

Relevant context:
- Files: [List]
- Recent changes: [List]
- Attempts already made: [List, including results]
- Constraints: [What cannot change]

Investigation rules:
- Inspect the narrowest relevant path first.
- Separate observed facts from inferences.
- Rank hypotheses by evidence, not convenience.
- Prefer the smallest experiment that can eliminate a hypothesis.
- Explain why additional files or tools are needed before expanding scope.

Return:
1. Ranked hypotheses with supporting and contradicting evidence
2. The next diagnostic step and its expected signal
3. Most likely root cause, or what evidence is still missing
4. Minimal fix direction if the cause is supported
5. Regression tests and verification steps for that fix
```
