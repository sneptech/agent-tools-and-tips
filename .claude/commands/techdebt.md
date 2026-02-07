# Tech Debt Scan

Scan the codebase (or specified area) for tech debt: duplicated code, inconsistent patterns, outdated practices, and accumulated cruft.

## Target

$ARGUMENTS

If no target specified, scan the entire project.

## Process

Use subagents to parallelize the scan across these categories:

### 1. Duplicated Code
Search for repeated logic, copy-pasted blocks, and near-identical functions. Flag anything that appears 3+ times.

### 2. Inconsistent Patterns
Look for the same thing done multiple ways:
- Mixed error handling strategies
- Inconsistent naming conventions
- Multiple patterns for the same concern (e.g., some files use pattern A, others use pattern B)

### 3. TODO/FIXME/HACK Inventory
Collect all TODO, FIXME, HACK, XXX, and WORKAROUND comments. Categorize by severity.

### 4. Dependency Concerns
- Outdated or deprecated dependencies
- Unused dependencies
- Dependencies with known vulnerabilities (if tooling available)

### 5. Code Smells
- Functions longer than 50 lines
- Files longer than 500 lines
- Deep nesting (4+ levels)
- God objects/functions doing too many things

## Output

```
TECH DEBT REPORT:

CRITICAL (blocks future work):
- [issue] — [location] — [suggested fix]

HIGH (should fix soon):
- [issue] — [location] — [suggested fix]

MEDIUM (fix when touching this area):
- [issue] — [location] — [suggested fix]

LOW (nice to have):
- [issue] — [location] — [suggested fix]

DUPLICATED CODE:
- [pattern] found in [N] locations: [list files]

STATS:
- TODOs: [count]
- FIXMEs: [count]
- HACKs: [count]
```

Run this at the end of every session to keep debt from accumulating.
