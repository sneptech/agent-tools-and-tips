# Plan and Review

Two-phase planning: first write a detailed implementation plan, then critically review it as a staff engineer would.

## Phase 1: Plan

Enter plan mode. Thoroughly explore the codebase and produce a detailed implementation plan for the following:

$ARGUMENTS

The plan must include:
- **Goal**: What success looks like (declarative, not imperative)
- **Files to touch**: Every file that will be created or modified
- **Approach**: Step-by-step implementation with rationale for each step
- **Assumptions**: Explicitly list every assumption you are making
- **Risks**: Anything that could go wrong or needs verification

## Phase 2: Staff Engineer Review

Now switch roles. You are a skeptical staff engineer reviewing this plan. Evaluate it against these criteria:

1. **Simplicity**: Can this be done in fewer steps or fewer lines? Are abstractions earning their complexity?
2. **Scope discipline**: Does the plan touch only what is necessary? Flag any scope creep.
3. **Correctness**: Are there edge cases, race conditions, or failure modes not addressed?
4. **Assumptions**: Challenge every listed assumption. Are any wrong or unverified?
5. **Missing pieces**: What did the plan forget? Tests? Error handling at boundaries? Migration steps?

Format your review as:

```
STAFF REVIEW:
- APPROVED / NEEDS REVISION

STRENGTHS:
- [what's good about this plan]

CONCERNS:
- [specific issues with severity: blocker / should-fix / nit]

SUGGESTED CHANGES:
- [concrete modifications to the plan]
```

If the review identifies blockers, revise the plan and re-review until it passes. Present the final approved plan to the user before proceeding.
