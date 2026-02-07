# Worktree Setup

Set up parallel git worktrees for running multiple Claude sessions simultaneously. This is the single biggest productivity unlock according to the Claude Code team.

## Configuration

$ARGUMENTS

If no arguments: set up 3 worktrees (a, b, c) from the current repository.

## Process

1. **Verify git repo** — Ensure we're in a git repository
2. **Determine branch strategy** — Each worktree needs its own branch:
   - Worktree `a`: branch `work-a` (or user-specified names)
   - Worktree `b`: branch `work-b`
   - Worktree `c`: branch `work-c`

3. **Create worktrees**:
```bash
# From the main repo directory:
git worktree add ../[repo-name]-a -b work-a
git worktree add ../[repo-name]-b -b work-b
git worktree add ../[repo-name]-c -b work-c
```

4. **Set up shell aliases** (suggest to user):
```bash
# Add to ~/.bashrc or ~/.zshrc:
alias za='cd /path/to/repo-a'
alias zb='cd /path/to/repo-b'
alias zc='cd /path/to/repo-c'
```

5. **Verify setup**:
```bash
git worktree list
```

6. **Report**:

```
WORKTREES CREATED:

  [path-a] — branch: work-a — ready for Claude session 1
  [path-b] — branch: work-b — ready for Claude session 2
  [path-c] — branch: work-c — ready for Claude session 3

SUGGESTED ALIASES:
  alias za='cd [path-a]'
  alias zb='cd [path-b]'
  alias zc='cd [path-c]'

USAGE:
  Open a separate terminal for each worktree and run `claude` in each.
  Each session gets its own branch and context window.
```

Tips:
- Name worktrees by task for clarity
- Keep one worktree as a read-only "analysis" tree for logs and queries
- Use `git worktree remove [path]` to clean up when done
