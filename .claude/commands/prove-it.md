# Prove It

Don't just claim changes work — prove it. Diff behavior between the current state and the previous state to demonstrate correctness.

## Target

$ARGUMENTS

## Process

1. **Identify what changed** — Read the diff and understand the behavioral implications

2. **Define proof criteria** — What specific behaviors need to be verified?
   - Existing behavior preserved (no regressions)
   - New behavior works as specified
   - Edge cases handled

3. **Gather evidence** — Use any available method:
   - Run the test suite and show results
   - Write a quick script that exercises the changed behavior
   - Compare output between main/base branch and current branch
   - Show before/after for key scenarios
   - Check logs, metrics, or other observables

4. **Present proof**:

```
CLAIM: [what the changes are supposed to do]

EVIDENCE:
1. [test/scenario]: [expected] → [actual] ✓/✗
2. [test/scenario]: [expected] → [actual] ✓/✗
3. [test/scenario]: [expected] → [actual] ✓/✗

REGRESSION CHECK:
- [existing test suite]: [pass/fail]
- [specific concern]: [verified how]

VERDICT: [proven/unproven/partially proven]

GAPS:
- [anything that couldn't be verified and why]
```

If you can't prove it works, say so. Don't hide uncertainty behind confident language. Flag what would need to be true for this to be correct.
