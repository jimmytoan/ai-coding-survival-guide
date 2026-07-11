# Verification Prompt

Use this after implementation or before handoff. Verification should challenge
the change, not repeat the implementation summary.

```text
Verify this change against the task brief. Do not edit files.

Task brief and acceptance criteria:
[Paste or link]

Change to verify:
- Diff/base revision: [Reference]
- Files: [List]

Claimed checks:
- [Command and reported result]

Environment constraints:
- [Unavailable service, missing credentials, network restriction, etc.]

Verification process:
1. Inspect the actual diff and map each acceptance criterion to code and evidence.
2. Check likely failure paths, regressions, compatibility, and unintended scope.
3. Run the smallest relevant automated checks that the environment allows.
4. Never infer that a check passed. Mark it not run if there is no direct result.
5. Do not use network access, mutate external systems, or perform destructive
   actions without explicit approval.

Return:
- Verdict: verified / partially verified / not verified
- Acceptance criteria: pass / fail / not demonstrated, with evidence for each
- Commands run: exact command, result, and relevant failure excerpt
- Manual checks still required
- Regressions or risks found, with file locations
- Confidence and what would increase it
```
