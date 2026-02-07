# Surface Assumptions

Before implementing anything, explicitly surface all assumptions about the task at hand.

## Task

Analyze the following request and surface every assumption before writing a single line of code:

$ARGUMENTS

## Process

1. Read all relevant files and understand the current state of the codebase
2. Identify every ambiguity, unstated requirement, and implicit assumption
3. Present assumptions in this format:

```
ASSUMPTIONS I'M MAKING:
1. [assumption] — [why I think this is true]
2. [assumption] — [why I think this is true]
3. [assumption] — [why I think this is true]
→ Correct me now or I'll proceed with these.
```

4. If you encounter conflicting information between files, flag it:

```
INCONSISTENCY DETECTED:
- File A says: [X]
- File B says: [Y]
→ Which takes precedence?
```

5. For each non-obvious design decision, present the tradeoff:

```
DECISION POINT: [description]
- Option A: [approach] — Pro: [benefit], Con: [cost]
- Option B: [approach] — Pro: [benefit], Con: [cost]
→ Which direction?
```

Do NOT proceed with implementation until the user has confirmed or corrected your assumptions. Never silently fill in ambiguous requirements. The most common failure mode is making wrong assumptions and running with them unchecked.
