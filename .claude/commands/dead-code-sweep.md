# Dead Code Sweep

After a refactor or implementation change, identify code that is now unreachable or unused. Don't leave corpses. Don't delete without asking.

## Target

$ARGUMENTS

## Process

1. Analyze the recent changes (via `git diff` or specified files)
2. Trace the impact of those changes across the codebase:
   - Functions/methods that are no longer called
   - Imports that are no longer used
   - Variables/constants that are no longer referenced
   - Types/interfaces that are no longer implemented
   - Files that are no longer imported anywhere
   - CSS classes/styles no longer referenced
   - Config entries no longer read
   - Test helpers for removed functionality

3. For each piece of dead code found, verify it's truly dead (use grep/search to confirm no references)

4. Present findings:

```
DEAD CODE IDENTIFIED:

Confirmed dead (zero references):
- [file:line]: [symbol] — was used by [removed thing]
- [file:line]: [symbol] — was used by [removed thing]

Possibly dead (needs verification):
- [file:line]: [symbol] — [why it might still be used]

→ Should I remove the confirmed dead code? (Y/N for each)
```

Do not delete anything without explicit approval. List it, explain it, ask permission.
