# Clarify

Surface and resolve confusion, inconsistencies, and ambiguities before they become bugs. Stop and think before you build on shaky ground.

## Context

$ARGUMENTS

## Process

When encountering anything unclear, inconsistent, or ambiguous:

1. **STOP** — Do not proceed with a guess
2. **Name the specific confusion** — Be precise about what is unclear
3. **Present the tradeoff or ask the clarifying question**
4. **Wait for resolution before continuing**

### What to look for:

- **Conflicting requirements**: "The spec says X but the existing code does Y"
- **Ambiguous scope**: "Does 'fix the auth' mean the login flow, the token refresh, or both?"
- **Unstated constraints**: "Should this support concurrent access? What's the expected load?"
- **Missing context**: "This references a 'user role' but I see no role system in the codebase"
- **Implicit assumptions**: "This assumes the API always returns JSON, but the error path returns HTML"

### Output format:

```
CONFUSION LOG:

1. [BLOCKING] [description of confusion]
   - I see: [what the evidence says]
   - But also: [the conflicting evidence]
   - Need: [specific question to resolve it]

2. [NON-BLOCKING] [description of uncertainty]
   - My best guess: [what I'd do if forced to decide]
   - Risk if wrong: [what breaks]
   - Need: [confirmation or correction]
```

Bad: Silently picking one interpretation and hoping it's right.
Good: "I see X in file A but Y in file B. Which takes precedence?"
