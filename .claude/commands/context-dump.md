# Context Dump

Gather context from multiple sources into a single, digestible summary. Useful at the start of a session or when picking up an unfamiliar task.

## Sources

$ARGUMENTS

## Process

1. **Gather from all available sources** — Depending on what's specified or available:
   - **Git**: Recent commits, branches, open PRs, recent diffs
   - **Issues**: Open issues, bug reports, feature requests (via `gh` CLI)
   - **CI/CD**: Recent build status, failing tests
   - **Code**: Key files, recent changes, CLAUDE.md, README
   - **Slack/Chat**: If MCP available, recent relevant threads
   - **Project management**: If MCP available, current sprint items
   - **Logs**: Recent error logs, deployment logs

2. **Synthesize into a briefing**:

```
CONTEXT BRIEFING — [date]

PROJECT STATE:
- Branch: [current branch]
- Last commit: [hash] [message] [time ago]
- Clean/dirty: [working tree status]

RECENT ACTIVITY (last 7 days):
- [N] commits by [authors]
- Key changes: [summary of significant commits]

OPEN ITEMS:
- PRs: [list with status]
- Issues: [high priority items]
- CI: [passing/failing — details if failing]

CURRENT TASK CONTEXT:
- [relevant context for what you're about to work on]

KEY FILES TO KNOW ABOUT:
- [file]: [why it matters for current work]

LANDMINES:
- [anything surprising, broken, or in-progress that could trip you up]
```

This replaces the need to manually check 5 different dashboards. One command, full situational awareness.
