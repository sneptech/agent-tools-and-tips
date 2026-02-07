# Scope Check

Verify that recent changes stay within the scope of the original task. Surgical precision, not unsolicited renovation.

## Target

$ARGUMENTS

## Process

1. Determine the original task/intent from the user's request or recent conversation
2. Review all modified files (use `git diff` if available, otherwise review recent changes)
3. For each change, classify it:

```
SCOPE AUDIT:

IN SCOPE:
- [file:line]: [change description] — directly serves the task

OUT OF SCOPE:
- [file:line]: [change description] — [why this exceeds the task]

WARNINGS:
- Comments removed that weren't part of the task
- Code "cleaned up" orthogonal to the task
- Adjacent systems refactored as side effects
- Unused code deleted without explicit approval
- Style changes unrelated to the task
```

4. If out-of-scope changes are found:
   - Flag each one
   - Recommend reverting or ask for explicit approval
   - Explain what the minimal in-scope change would look like

The goal is to touch only what you're asked to touch. Do NOT remove comments you don't understand, "clean up" unrelated code, refactor adjacent systems, or delete code that seems unused without approval.
