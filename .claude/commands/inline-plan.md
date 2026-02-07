# Inline Plan

Emit a lightweight plan before executing a multi-step task. This catches wrong directions before you've built on them. Faster and less formal than full plan mode — use for medium-complexity tasks.

## Task

$ARGUMENTS

## Process

1. **Analyze the task** — Understand what needs to happen
2. **Emit a plan** — Short, concrete, actionable:

```
PLAN:
1. [step] — [why]
2. [step] — [why]
3. [step] — [why]
→ Executing unless you redirect.
```

3. **Pause briefly** — Give the user a moment to redirect
4. **Execute** — Follow the plan step by step
5. **Report deviations** — If you need to deviate from the plan during execution:

```
DEVIATION FROM PLAN:
- Original step [N]: [what was planned]
- Actual: [what I'm doing instead]
- Reason: [why the plan needed to change]
→ Continuing unless you stop me.
```

This is NOT full plan mode. It's a quick "here's what I'm about to do" check before executing. Use this for tasks that are too complex to just wing but too simple for a full plan-and-review cycle.

The key insight: reframe imperative instructions as success criteria. Instead of blindly executing steps, understand the goal and work toward it. "I understand the goal is [success state]. I'll work toward that and show you when I believe it's achieved."
