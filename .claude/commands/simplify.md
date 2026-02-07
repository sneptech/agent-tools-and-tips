# Simplify

Audit code for unnecessary complexity and over-engineering. The goal is to find where fewer lines, fewer abstractions, or a more boring/obvious solution would be better.

## Target

$ARGUMENTS

## Process

1. Read the target files thoroughly
2. For each function, class, or module evaluate:
   - **Line count**: Can this be done in fewer lines without sacrificing readability?
   - **Abstraction audit**: Is every abstraction earning its complexity? Would a senior dev say "why didn't you just..."?
   - **Premature generalization**: Is code designed for hypothetical future requirements instead of current needs?
   - **Clever tricks**: Is there cleverness that a comment can't justify?
   - **Helper/utility bloat**: Are there helpers or utilities for one-time operations?
   - **Feature flags / compatibility shims**: Are there backwards-compatibility hacks when the code could just be changed?

3. Present findings:

```
SIMPLIFICATION OPPORTUNITIES:

[file:line] — [severity: high/medium/low]
  Current: [what it does now]
  Simpler: [what it could be]
  Savings: [lines removed / abstractions eliminated]

VERDICT: [X lines could become Y lines, Z abstractions can be removed]
```

4. Ask user which simplifications to apply before making changes

Remember: three similar lines of code is better than a premature abstraction. The right amount of complexity is the minimum needed for the current task. If 1000 lines exist and 100 would suffice, that's a failure.
