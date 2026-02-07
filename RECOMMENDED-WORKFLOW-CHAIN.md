# Recommended Post-Phase Workflow Chain

After `/gsd:execute-phase` completes, run these skill chains to verify, clean up, and capture learnings before moving on. Choose the tier that matches the risk level of the phase.

---

## Always (every phase)

```
/scope-check           # Did the agent stay on task or go renovating?
/change-summary        # What changed, what didn't, what's risky
/gsd:verify-work       # GSD's built-in goal-backward verification
/update-claude-md      # Capture anything learned for next phase
```

This is the minimum. Scope check catches agents that wandered, change summary documents the work, verify-work confirms the phase goal was actually met, and updating CLAUDE.md compounds accuracy across phases.

---

## For complex or risky phases, add:

```
/scope-check
/simplify              # Catch over-engineering while it's fresh
/dead-code-sweep       # Especially if the phase replaced/removed something
/grill-me              # Aggressive review before it gets merged
/prove-it              # Demand evidence, not just "it works"
/change-summary
/gsd:verify-work
/update-claude-md
```

---

## For algorithmic or correctness-critical phases, add:

```
/scope-check
/prove-it              # This becomes the centerpiece
/naive-first           # If optimization was involved, verify against naive oracle
/grill-me
/change-summary
/gsd:verify-work
/update-claude-md
```

---

## Execution stages (how to actually run the chain)

The chain has a specific staging order. Not everything can be parallelized — some checks are gates, others depend on earlier findings.

### Stage 1: Gate check (sequential, fast)

```
/scope-check
```

Run this **alone first**. It's the cheapest check with the biggest bang. If the agent rewrote half the codebase or went on an unsolicited renovation, you want to know before spending time and tokens on detailed quality analysis. If scope is blown, stop and fix before proceeding.

### Stage 2: Quality checks (parallel)

```
/simplify  +  /dead-code-sweep  +  /prove-it  (+  /grill-me if applicable)
```

These are independent of each other — run them all as parallel subagents. This is where you catch real problems: over-engineering, unused code, unproven claims, and logical holes.

### Stage 3: Documentation (sequential, after Stage 2)

```
/change-summary
```

Run after quality checks complete so the summary can incorporate findings ("simplify flagged X, dead-code found Y, prove-it identified Z gap"). A summary written before quality checks misses the issues they surface.

### Stage 4: Goal verification (sequential, after Stage 3)

```
/gsd:verify-work
```

GSD's goal-backward analysis. By this point you've already identified (and ideally fixed) issues from earlier stages, so verify-work confirms the cleaned-up result against the phase's success criteria.

### Stage 5: Learning capture (sequential, last)

```
/update-claude-md
```

Captures everything learned from the entire chain — implementation patterns, gotchas, bugs found, decisions made — not just the implementation itself.

---

## Persisting results (GSD integration)

**Every check in the chain must write its findings to a persistent file.** Agent output that only exists in a context window is lost on `/clear`. Follow these rules:

### Write a consolidated report

After the chain completes (or after each stage if running across context windows), write a single verification report to:

```
.planning/phases/VERIFICATION-CHAIN-P{phase}.md
```

This file should contain the findings from every check that was run, organized by check name, with clear verdicts and action items.

### Track action items

Issues found during verification must be captured durably:

- **Bugs** (must-fix): Add to the phase's action items in the verification report. If blocking, note in STATE.md blockers section.
- **Tech debt** (should-fix): Add to `.planning/todos/` or note in STATE.md pending todos.
- **Dead code**: List in the report with file:line references. Don't delete without approval — the report IS the approval request.
- **Missing test coverage**: Note gaps in the prove-it section so they can be addressed in the current or next phase.

### Update STATE.md

After the chain completes, update STATE.md to reflect:

- That verification was performed (date, which checks ran)
- Any blockers or concerns surfaced
- Whether the phase passed or needs remediation before proceeding

### Why this matters

Without persistent output:
- A `/clear` between checks loses earlier findings
- The next session's `/gsd:resume-work` has no record that verification happened
- Issues found but not tracked get silently lost
- `/gsd:verify-work` in Stage 4 can't reference findings from Stages 1-3

The verification chain is only as valuable as its paper trail.

---

## Practical shortcut

If you're moving fast and trust the phase was straightforward:

```
/scope-check && /change-summary && /gsd:verify-work && /update-claude-md
```

Four commands, under two minutes, catches the most common failure modes (scope creep, undocumented changes, goal not actually met, lost learnings).

Even with the shortcut, write findings to `.planning/phases/VERIFICATION-CHAIN-P{phase}.md`.
