# Update CLAUDE.md

Update the project's CLAUDE.md with learnings from the current session. Claude is eerily good at writing rules for itself — use that.

## Context

$ARGUMENTS

If no arguments provided, review the current session for any corrections, mistakes, or learnings.

## Process

1. **Identify what was learned** — Review the session for:
   - Corrections the user made ("no, do it this way...")
   - Mistakes that were caught
   - Patterns that worked well
   - Project-specific conventions discovered
   - Build/test/deploy quirks
   - Things that wasted time due to lack of context

2. **Read existing CLAUDE.md** — If it exists, understand what's already documented

3. **Draft updates** — For each learning, write a concise rule:
   - Frame as an instruction to your future self
   - Be specific, not vague ("use pytest-asyncio for async tests" not "handle async properly")
   - Include the WHY so the rule isn't blindly followed in wrong contexts
   - Group related rules together

4. **Present changes** — Show the diff:

```
PROPOSED CLAUDE.md UPDATES:

NEW RULES:
- [rule]: [why]

MODIFIED RULES:
- [old rule] → [new rule]: [why changed]

REMOVED RULES:
- [rule]: [why no longer relevant]
```

5. **Apply after approval** — Only update after user confirms

Ruthlessly edit over time. A CLAUDE.md that's too long gets ignored. Keep it focused on things that actually prevent mistakes.
