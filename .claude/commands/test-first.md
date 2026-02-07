# Test First

Implement using strict TDD: write the failing test first, then implement until it passes.

## Task

$ARGUMENTS

## Process

1. **Understand the requirement** — Reframe the task as a success criterion:
   "I understand the goal is [success state]. I'll work toward that. Correct?"

2. **Write the test(s)** — Before touching any implementation:
   - Write test(s) that define exactly what success looks like
   - Tests should cover the happy path AND important edge cases
   - Run the tests to confirm they fail for the right reason
   - Show the tests to the user

3. **Implement the minimum** — Write the simplest code that makes the tests pass:
   - No premature optimization
   - No speculative generalization
   - Just make the tests green

4. **Verify** — Run the full test suite:
   - New tests pass
   - Existing tests still pass
   - No regressions

5. **Show both** — Present the test and implementation together:

```
TEST:
[test code with explanation of what it verifies]

IMPLEMENTATION:
[implementation code]

RESULTS:
- New tests: [pass/fail]
- Existing tests: [pass/fail]
- Coverage delta: [if available]
```

Tests are your loop condition. Use them. If a test fails, loop on the implementation — don't modify the test to make it pass (unless the test itself is wrong).
