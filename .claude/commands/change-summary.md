# Change Summary

Generate a structured summary of all changes made during this session or for a specified scope. This format ensures the reviewer knows exactly what changed, what was intentionally left alone, and what risks remain.

## Target

$ARGUMENTS

If no target specified, summarize all changes in the current session.

## Process

1. Collect all modified files (via `git diff`, `git status`, or as specified)
2. For each changed file, understand the what and the why
3. Identify files that were intentionally NOT modified and explain why

## Output Format

```
CHANGES MADE:
- [file]: [what changed and why]
- [file]: [what changed and why]

THINGS I DIDN'T TOUCH:
- [file]: [intentionally left alone because...]

POTENTIAL CONCERNS:
- [any risks, things to verify, or follow-up items]

ASSUMPTIONS THAT WERE MADE:
- [any assumptions baked into these changes]

TESTING STATUS:
- [what was tested, what wasn't, what should be]
```

Be direct about problems. Quantify when possible ("this adds ~200ms latency" not "this might be slower"). Don't hide uncertainty behind confident language. If something is a risk, say so plainly.
