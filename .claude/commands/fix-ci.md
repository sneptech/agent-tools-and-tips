# Fix CI

Diagnose and fix failing CI tests. Zero micromanagement — just point and fix.

## Context

$ARGUMENTS

If no arguments provided, run `git log --oneline -5` to find the recent push and check CI status.

## Process

1. **Identify failures** — Check CI output:
   - Use `gh run list` and `gh run view` if GitHub Actions
   - Read CI logs/output as provided
   - Parse error messages and stack traces

2. **Diagnose root cause** — For each failure:
   - Is it a real bug in the code, or a flaky test?
   - Is it an environment/dependency issue?
   - Is it a merge conflict or stale branch?
   - Did a recent change break an assumption?

3. **Fix** — Apply the minimal fix:
   - Don't over-fix: solve the failing test, not adjacent issues
   - If a test is flaky, fix the flakiness, don't just retry
   - If it's a real bug, fix the bug AND add a regression test

4. **Verify locally** — Run the same tests locally to confirm they pass

5. **Report**:

```
CI FAILURES FIXED:

[test/check name]:
  - Root cause: [what was wrong]
  - Fix: [what was changed]
  - Verified: [how you confirmed it works]

REMAINING ISSUES:
- [any failures that need different intervention]
```

Don't micromanage the fix. Understand the error, apply the surgical fix, verify, move on.
