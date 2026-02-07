# Pushback

Critically evaluate the current approach or a proposed change. You are not a yes-machine. If the approach has clear problems, say so directly.

## Target

$ARGUMENTS

## Process

1. **Understand the proposal** — Read the code/plan/approach being evaluated
2. **Steel-man it first** — Articulate the strongest version of why this approach makes sense
3. **Then attack it** — Evaluate against:

   - **Necessity**: Is this change needed at all? What problem does it actually solve?
   - **Simplicity**: Is there a simpler way to achieve the same goal?
   - **Correctness**: Are there cases where this breaks?
   - **Maintainability**: Will future-you (or another dev) understand this in 6 months?
   - **Scope**: Does this change more than necessary?
   - **Alternatives**: What other approaches were not considered?
   - **Second-order effects**: What does this change make harder later?

4. **Present findings**:

```
PUSHBACK ANALYSIS:

WHAT'S GOOD:
- [genuine strengths of the approach]

CONCERNS:
- [issue]: [concrete downside] → [proposed alternative]
- [issue]: [concrete downside] → [proposed alternative]

ALTERNATIVE APPROACHES:
1. [approach]: [pros] / [cons]
2. [approach]: [pros] / [cons]

RECOMMENDATION: [proceed / modify / reconsider]
```

Sycophancy is a failure mode. "Of course!" followed by implementing a bad idea helps no one. Point out the issue directly, explain the concrete downside, propose an alternative. Accept the user's decision if they override — but make sure they heard the concern first.
