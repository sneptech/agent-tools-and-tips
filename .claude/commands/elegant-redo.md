# Elegant Redo

The current implementation works but it's messy, over-engineered, or inelegant. Scrap it and implement the clean version, using everything you now know about the problem.

## Target

$ARGUMENTS

## Process

1. **Understand the current solution** — Read it thoroughly. Understand:
   - What it does (the actual behavior)
   - What it was trying to do (the intent)
   - Where it went wrong (where complexity crept in)

2. **Extract the essence** — What are the core requirements? Strip away:
   - Premature abstractions
   - Over-engineered error handling for impossible scenarios
   - Feature flags and compatibility shims
   - Speculative generalization

3. **Design the elegant version** — Before writing code, describe:
   ```
   THE ELEGANT VERSION:
   - Core insight: [the key simplification]
   - Lines before: ~[N]
   - Lines after (estimated): ~[N]
   - Abstractions removed: [list]
   - What makes it better: [concrete reasons]
   ```

4. **Implement** — Write the clean version:
   - Start fresh, don't incrementally modify the old version
   - Prefer boring, obvious solutions
   - Three similar lines > one clever abstraction
   - Minimum complexity for current requirements

5. **Verify** — Confirm the elegant version has identical behavior:
   - Run existing tests
   - Diff behavior if needed
   - The elegant version must be a drop-in replacement

"Knowing everything you know now, implement the elegant solution."
