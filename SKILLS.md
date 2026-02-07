# AI Agent Skills Reference

Skills derived from **Boris Cherny** (Claude Code creator/team lead) and **Andrej Karpathy** (senior software engineering principles). Designed for use with Claude Code and compatible with orchestrators like GSD.

All skills are installed at `.claude/commands/` and invoked as `/command-name [arguments]`.

---

## Quick Reference

| Command | Source | One-liner |
|---------|--------|-----------|
| `/plan-and-review` | Boris | Write a plan then review it as a staff engineer before executing |
| `/surface-assumptions` | Karpathy | Explicitly state all assumptions before writing any code |
| `/clarify` | Karpathy | Surface confusion, inconsistencies, and ambiguities before they become bugs |
| `/pushback` | Karpathy | Critically evaluate an approach — don't be a yes-machine |
| `/inline-plan` | Karpathy | Emit a lightweight plan before executing a multi-step task |
| `/simplify` | Karpathy | Audit code for unnecessary complexity and over-engineering |
| `/scope-check` | Karpathy | Verify recent changes stay within the scope of the original task |
| `/dead-code-sweep` | Karpathy | Find unreachable/unused code after a refactor |
| `/techdebt` | Boris | Scan codebase for duplicated code, inconsistencies, and cruft |
| `/change-summary` | Karpathy | Generate structured summary of all changes with risks and concerns |
| `/test-first` | Karpathy | Strict TDD: write the failing test, then implement until it passes |
| `/naive-first` | Karpathy | Implement the obviously-correct naive version before optimizing |
| `/prove-it` | Boris | Diff behavior between branches to prove changes are correct |
| `/grill-me` | Boris | Aggressive code review — challenge changes before they ship |
| `/fix-ci` | Boris | Diagnose and fix failing CI tests with zero micromanagement |
| `/elegant-redo` | Boris | Scrap a messy implementation and redo it cleanly |
| `/explain-visual` | Boris | Generate a visual HTML presentation explaining unfamiliar code |
| `/ascii-diagram` | Boris | Draw ASCII diagrams of architecture, data flow, or protocols |
| `/update-claude-md` | Boris | Update CLAUDE.md with learnings from the current session |
| `/subagent-blast` | Boris | Decompose a task and throw parallel subagents at it |
| `/worktree-setup` | Boris | Set up parallel git worktrees for multiple Claude sessions |
| `/context-dump` | Boris | Gather context from git, CI, issues, and code into one briefing |

---

## Detailed Skill Documentation

---

### `/plan-and-review`

**Source**: Boris Cherny — *"Start every complex task in plan mode. One person has one Claude write the plan, then they spin up a second Claude to review it as a staff engineer."*

**Purpose**: Two-phase planning process. Phase 1 writes a detailed implementation plan. Phase 2 switches to a skeptical staff engineer persona who reviews the plan for simplicity, scope discipline, correctness, assumptions, and missing pieces. The plan is revised until the review passes.

**When to use**: Before any complex implementation task. Especially valuable when the task involves multiple files, architectural decisions, or unclear requirements.

**Example**:
```
/plan-and-review Add WebSocket support for real-time notifications in the dashboard
```

Claude will:
1. Explore the codebase and produce a detailed plan (files to touch, approach, assumptions, risks)
2. Switch to staff engineer mode and critique the plan
3. Revise until approved, then present the final plan for user sign-off

**GSD integration**: Use before `/gsd:execute-phase` to get a vetted plan. The staff review catches scope creep and over-engineering before a single line is written.

---

### `/surface-assumptions`

**Source**: Karpathy — *"Before implementing anything non-trivial, explicitly state your assumptions. Never silently fill in ambiguous requirements."*

**Purpose**: Forces the agent to stop and enumerate every assumption, inconsistency, and decision point before writing code. Prevents the most common failure mode: making wrong assumptions and running with them unchecked.

**When to use**: At the start of any non-trivial task, especially when requirements are vague, specs are incomplete, or you're working in an unfamiliar codebase.

**Example**:
```
/surface-assumptions Implement user role-based access control for the API endpoints
```

Claude will:
1. Read relevant code and identify all ambiguities
2. List assumptions with reasoning ("I assume roles are stored in the JWT because...")
3. Flag any conflicting information found in the codebase
4. Present decision points with tradeoffs
5. Wait for user confirmation before proceeding

**GSD integration**: Run this before `/gsd:plan-phase`. The assumptions document becomes input for planning, ensuring the plan is built on verified ground.

---

### `/clarify`

**Source**: Karpathy — *"When you encounter inconsistencies, conflicting requirements, or unclear specifications: STOP. Do not proceed with a guess."*

**Purpose**: A structured protocol for handling confusion. Instead of silently picking an interpretation and hoping it's right, the agent stops, names the exact confusion, and asks for resolution.

**When to use**: Whenever you (or an agent) encounter something that doesn't add up — conflicting code patterns, ambiguous scope, missing context, implicit assumptions.

**Example**:
```
/clarify The auth middleware uses JWT tokens but the session store uses cookies — which is the intended auth mechanism?
```

Claude will:
1. Investigate the specific confusion
2. Gather evidence from the codebase
3. Present a structured confusion log with blocking vs non-blocking items
4. For non-blocking items, state its best guess and what breaks if wrong
5. Wait for resolution on blocking items

**GSD integration**: Subagents spawned by `/gsd:execute-phase` should invoke this behavior whenever they hit ambiguity rather than guessing and producing wrong output.

---

### `/pushback`

**Source**: Karpathy — *"You are not a yes-machine. Sycophancy is a failure mode. 'Of course!' followed by implementing a bad idea helps no one."*

**Purpose**: Critically evaluate a proposed approach. Steel-man it first (articulate why it might be good), then attack it on necessity, simplicity, correctness, maintainability, scope, alternatives, and second-order effects.

**When to use**: When you suspect an approach has problems, when evaluating a PR, when a user proposes something and you want a sanity check, or when you want to consider alternatives before committing.

**Example**:
```
/pushback The proposal to add a Redis cache layer in front of the database for user profiles
```

Claude will:
1. Articulate the strongest case for the approach
2. Then challenge it on every axis (is it needed? is there something simpler? what breaks?)
3. Propose concrete alternatives with pros/cons
4. Give a recommendation: proceed, modify, or reconsider
5. Accept the user's final decision — but ensure the concerns were heard

**GSD integration**: Use during `/gsd:discuss-phase` or `/gsd:list-phase-assumptions` to pressure-test approaches before committing to a plan.

---

### `/inline-plan`

**Source**: Karpathy — *"For multi-step tasks, emit a lightweight plan before executing. This catches wrong directions before you've built on them."*

**Purpose**: A quick, informal plan for medium-complexity tasks. Not full plan mode — just a "here's what I'm about to do" check. Also reframes imperative instructions as success criteria, enabling the agent to loop and problem-solve rather than blindly follow steps.

**When to use**: Tasks too complex to wing but too simple for a full plan-and-review cycle. 3-7 step tasks where you want a quick sanity check.

**Example**:
```
/inline-plan Migrate the user model from Sequelize to Prisma
```

Claude will:
1. Emit a numbered plan with rationale for each step
2. Pause for the user to redirect
3. Execute, reporting any deviations from the plan as they happen

**GSD integration**: Use within `/gsd:quick` for tasks that need a plan but don't warrant the full `/gsd:plan-phase` ceremony.

---

### `/simplify`

**Source**: Karpathy — *"Your natural tendency is to overcomplicate. Actively resist it. If you build 1000 lines and 100 would suffice, you have failed."*

**Purpose**: Audit code for unnecessary complexity. Checks for bloated abstractions, premature generalization, clever tricks, helper functions for one-time operations, and backwards-compatibility shims. The rule: three similar lines of code is better than a premature abstraction.

**When to use**: After implementation (as a self-review), when inheriting unfamiliar code, or as a periodic hygiene pass.

**Example**:
```
/simplify src/services/notification-manager.ts
```

Claude will:
1. Read the target files
2. Evaluate every function/class for line count, abstraction value, and premature generalization
3. Present a list of simplification opportunities with severity and estimated savings
4. Ask which ones to apply before making changes

**GSD integration**: Run after `/gsd:execute-phase` completes. Catches over-engineering that crept in during implementation.

---

### `/scope-check`

**Source**: Karpathy — *"Touch only what you're asked to touch. Your job is surgical precision, not unsolicited renovation."*

**Purpose**: Reviews recent changes and classifies each one as in-scope or out-of-scope relative to the original task. Catches removed comments, "cleaned up" unrelated code, refactored adjacent systems, and deleted code that seemed unused.

**When to use**: Before committing, after a large implementation session, or when reviewing someone else's (or an agent's) work.

**Example**:
```
/scope-check Review the changes from the last 3 commits against the original task of "add pagination to the users endpoint"
```

Claude will:
1. Determine the original task intent
2. Review all modified files via git diff
3. Classify each change as in-scope, out-of-scope, or warning
4. Recommend reverting out-of-scope changes

**GSD integration**: Run as a verification step after `/gsd:execute-phase` or before `/gsd:verify-work`. Ensures agents didn't go on an unsolicited renovation spree.

---

### `/dead-code-sweep`

**Source**: Karpathy — *"After refactoring: identify code that is now unreachable. List it explicitly. Ask before deleting. Don't leave corpses."*

**Purpose**: After a refactor or implementation change, trace the impact and find functions, imports, variables, types, files, and config entries that are now dead. Verify they're truly dead, then present findings for approval before deletion.

**When to use**: After any significant refactor, after removing a feature, or after migrating between patterns/frameworks.

**Example**:
```
/dead-code-sweep Check for dead code after removing the legacy payment processor
```

Claude will:
1. Analyze recent changes and trace their impact
2. Search for orphaned functions, unused imports, unreferenced types, etc.
3. Verify each finding (grep to confirm zero references)
4. Present confirmed dead code and possibly-dead code separately
5. Ask for per-item approval before deleting anything

**GSD integration**: Run after `/gsd:execute-phase` when the phase involved removing or replacing functionality.

---

### `/techdebt`

**Source**: Boris Cherny — *"Build a /techdebt slash command and run it at the end of every session to find and kill duplicated code."*

**Purpose**: Comprehensive tech debt scan using parallel subagents. Covers duplicated code, inconsistent patterns, TODO/FIXME inventory, dependency concerns, and code smells (long functions, deep nesting, god objects).

**When to use**: At the end of every session (habit), when onboarding to a new codebase, or when planning a cleanup sprint.

**Example**:
```
/techdebt src/
```

Claude will:
1. Spawn subagents to scan in parallel across 5 categories
2. Aggregate findings into a prioritized report (critical/high/medium/low)
3. List all duplicated code patterns with file locations
4. Provide counts of TODOs, FIXMEs, and HACKs

**GSD integration**: Use to generate input for `/gsd:new-milestone` when planning a tech debt reduction milestone. The report maps directly to phases.

---

### `/change-summary`

**Source**: Karpathy — *"After any modification, summarize: what changed and why, what you didn't touch and why, and potential concerns."*

**Purpose**: Structured post-implementation report. Documents what changed, what was intentionally left alone, risks, assumptions baked in, and testing status. Ensures nothing falls through the cracks during review.

**When to use**: After completing any implementation task, before creating a PR, or when handing off work.

**Example**:
```
/change-summary
```

Claude will:
1. Collect all modified files from git diff/status
2. Document each change with the why, not just the what
3. Explicitly list files that were intentionally NOT modified
4. Flag risks and untested areas
5. List assumptions that were baked into the changes

**GSD integration**: Run at the end of `/gsd:execute-phase` to produce the handoff document. Feeds directly into `/gsd:verify-work`.

---

### `/test-first`

**Source**: Karpathy — *"When implementing non-trivial logic: write the test that defines success, implement until the test passes, show both. Tests are your loop condition."*

**Purpose**: Strict TDD workflow. Reframes the task as a success criterion, writes tests first, confirms they fail for the right reason, implements the minimum to make them pass, then verifies no regressions.

**When to use**: For any non-trivial logic, bug fixes (write the test that would have caught it), or when correctness matters more than speed.

**Example**:
```
/test-first Implement rate limiting: max 100 requests per minute per API key, returning 429 when exceeded
```

Claude will:
1. Reframe as success criteria and confirm understanding
2. Write tests covering happy path and edge cases (101st request, different API keys, minute rollover)
3. Run tests to confirm they fail
4. Implement the minimum code to make them pass
5. Run the full test suite to verify no regressions
6. Present test + implementation together

**GSD integration**: Use as the implementation strategy within `/gsd:execute-phase` for phases where correctness is critical.

---

### `/naive-first`

**Source**: Karpathy — *"For algorithmic work: first implement the obviously-correct naive version, verify correctness, then optimize while preserving behavior. Never skip step 1."*

**Purpose**: Correctness-first development. Implement the simplest version that is obviously correct, lock in correctness with tests, then optimize. The naive version becomes a correctness oracle for the optimized version.

**When to use**: Any algorithmic or complex logic work — data transformations, search algorithms, batch processing, calculations.

**Example**:
```
/naive-first Implement a function that finds the shortest path between two nodes in our dependency graph
```

Claude will:
1. Implement the simplest correct version (e.g., BFS with no optimization)
2. Write comprehensive tests and verify all pass
3. Optimize only if needed (e.g., A* with heuristic)
4. Run the same tests against both versions to verify identical behavior
5. Present both with complexity analysis

**GSD integration**: Use during `/gsd:execute-phase` for algorithmically complex tasks. The naive version provides a built-in verification mechanism.

---

### `/prove-it`

**Source**: Boris Cherny — *"Say 'Prove to me this works' and have Claude diff behavior between main and your feature branch."*

**Purpose**: Don't claim changes work — prove it with evidence. Run tests, write verification scripts, compare output between branches, check logs. Present a structured proof with claims, evidence, and gaps.

**When to use**: Before merging, when reviewing someone's "it works on my machine" claim, or when changes are subtle enough that eyeballing isn't sufficient.

**Example**:
```
/prove-it Prove the new caching layer doesn't change any API response bodies
```

Claude will:
1. Identify what behaviors need verification
2. Gather evidence: run tests, diff branch outputs, write verification scripts
3. Present a structured proof (claim, evidence per scenario, pass/fail)
4. Explicitly flag anything that couldn't be verified and why

**GSD integration**: Use as the verification mechanism in `/gsd:verify-work`. Replaces "did it work?" with "here's proof it works."

---

### `/grill-me`

**Source**: Boris Cherny — *"Say 'Grill me on these changes and don't make a PR until I pass your test.' Make Claude be your reviewer."*

**Purpose**: Aggressive, no-mercy code review. Reads all changes and challenges every aspect: correctness, edge cases, security, performance, naming, simplicity, test coverage, error handling, and hidden assumptions. Does NOT approve unless genuinely satisfied.

**When to use**: Before creating a PR, after a complex implementation, or when you want confidence that your changes are solid.

**Example**:
```
/grill-me
```

Claude will:
1. Read all uncommitted/recently committed changes
2. Understand the intent
3. Grill on every axis with specific, pointed questions
4. Format as a formal review: blockers, should-fix, questions, nits
5. Give an honest verdict: APPROVE, REQUEST CHANGES, or NEEDS DISCUSSION

**GSD integration**: Run after `/gsd:execute-phase` and before `/gsd:verify-work`. Acts as a synthetic peer review step.

---

### `/fix-ci`

**Source**: Boris Cherny — *"Just say 'Go fix the failing CI tests.' Don't micromanage how."*

**Purpose**: Autonomous CI diagnosis and repair. Reads CI output, identifies root causes (real bugs vs flaky tests vs environment issues), applies minimal surgical fixes, and verifies locally.

**When to use**: When CI is red and you don't want to debug it manually. Especially useful for test flakiness, dependency issues, and merge-related breakage.

**Example**:
```
/fix-ci The GitHub Actions build is failing on the test-integration job
```

Claude will:
1. Pull CI logs (via `gh` CLI or as provided)
2. Parse error messages and stack traces
3. Diagnose root cause for each failure
4. Apply minimal fixes (fix the bug, not adjacent issues)
5. Run locally to verify
6. Report what was wrong and what was fixed

**GSD integration**: Use when `/gsd:execute-phase` produces code that breaks CI. Can be run autonomously by a subagent.

---

### `/elegant-redo`

**Source**: Boris Cherny — *"After a mediocre fix, say: 'Knowing everything you know now, scrap this and implement the elegant solution.'"*

**Purpose**: Start over with the benefit of hindsight. The first implementation taught you about the problem; now implement the clean version. Extract the core requirements, strip away accidental complexity, and write it fresh.

**When to use**: When an implementation works but feels wrong — too complex, too many abstractions, too many lines for what it does.

**Example**:
```
/elegant-redo src/services/billing-calculator.ts — it works but it's 400 lines and should be 80
```

Claude will:
1. Read and understand the current implementation
2. Extract the core requirements (what it actually needs to do)
3. Design the clean version and estimate the reduction
4. Implement from scratch (not incremental modification)
5. Verify identical behavior via tests

**GSD integration**: Use after `/gsd:verify-work` identifies working but messy code. Can be a follow-up phase.

---

### `/explain-visual`

**Source**: Boris Cherny — *"Have Claude generate a visual HTML presentation explaining unfamiliar code. It makes surprisingly good slides!"*

**Purpose**: Creates a self-contained HTML presentation that explains code, architecture, or protocols visually. Includes diagrams, syntax-highlighted code snippets, annotations, and navigation.

**When to use**: Onboarding to a new codebase, explaining a system to a teammate, documenting complex architecture, or learning an unfamiliar protocol.

**Example**:
```
/explain-visual How the authentication and authorization system works in this codebase
```

Claude will:
1. Deep-read the relevant code
2. Break it into digestible concepts
3. Generate a polished HTML file with dark theme, diagrams, annotated code, and navigation
4. Save as `explanation-[topic].html`

**GSD integration**: Use during `/gsd:discuss-phase` or `/gsd:research-phase` to build understanding before planning.

---

### `/ascii-diagram`

**Source**: Boris Cherny — *"Ask Claude to draw ASCII diagrams of new protocols and codebases to help you understand them."*

**Purpose**: Quick ASCII art diagrams of architecture, data flow, sequence interactions, state machines, or dependency graphs. Stays in the terminal — no context switching needed.

**When to use**: When you need to understand or communicate system structure quickly. During planning, debugging, or documentation.

**Example**:
```
/ascii-diagram The request lifecycle from API gateway through auth, routing, handlers, and database
```

Claude will:
1. Analyze the relevant code
2. Choose the most useful diagram type (architecture, data flow, sequence, state machine)
3. Draw clean box-and-arrow ASCII art
4. Annotate non-obvious connections

**GSD integration**: Use during `/gsd:plan-phase` to visualize the system before designing changes.

---

### `/update-claude-md`

**Source**: Boris Cherny — *"After every correction, end with: 'Update your CLAUDE.md so you don't make that mistake again.' Claude is eerily good at writing rules for itself."*

**Purpose**: Reviews the current session for corrections, mistakes, and learnings, then drafts precise updates to CLAUDE.md. Rules are framed as instructions to future-self with the WHY included.

**When to use**: After every session where something was learned, after a correction, after discovering a project quirk.

**Example**:
```
/update-claude-md I had to correct you three times about using the legacy API — add a rule about that
```

Claude will:
1. Review the session for all corrections and learnings
2. Read the existing CLAUDE.md
3. Draft concise, specific rules with reasoning
4. Present the diff for approval
5. Apply only after user confirms

**GSD integration**: Run at the end of every `/gsd:execute-phase` or `/gsd:verify-work` session. The accumulating CLAUDE.md makes every subsequent phase more accurate.

---

### `/subagent-blast`

**Source**: Boris Cherny — *"Append 'use subagents' to any request where you want Claude to throw more compute at the problem."*

**Purpose**: Decompose a task into independent subtasks and run them in parallel via subagents. Each subagent gets a focused context window and clear deliverable. Results are merged and conflicts resolved.

**When to use**: Large-scale refactoring, multi-file analysis, parallel test writing, comprehensive code review, or any task that's embarrassingly parallel.

**Example**:
```
/subagent-blast Review all 12 API endpoint handlers for security vulnerabilities, performance issues, and missing input validation
```

Claude will:
1. Decompose into independent subtasks (e.g., one agent per 3-4 endpoints)
2. Design the merge strategy
3. Launch subagents in parallel
4. Collect results, check for conflicts
5. Present merged findings

**GSD integration**: This IS how GSD works internally. Use this skill when you want the same parallelism for ad-hoc tasks outside the GSD ceremony.

---

### `/worktree-setup`

**Source**: Boris Cherny — *"Spin up 3-5 git worktrees at once, each running its own Claude session in parallel. It's the single biggest productivity unlock."*

**Purpose**: Creates parallel git worktrees so you can run multiple Claude Code sessions simultaneously on different tasks, all in the same repo.

**When to use**: At the start of a work session when you have multiple independent tasks, or when you want an "analysis" worktree for read-only investigation.

**Example**:
```
/worktree-setup 3 worktrees: auth-refactor, api-pagination, fix-dashboard
```

Claude will:
1. Create worktrees with named branches
2. Suggest shell aliases for quick navigation
3. Verify the setup
4. Explain how to run Claude in each

**GSD integration**: Set up worktrees before running multiple `/gsd:execute-phase` commands in parallel across different phases.

---

### `/context-dump`

**Source**: Boris Cherny — *"Set up a slash command that syncs 7 days of activity into one context dump."*

**Purpose**: Gathers context from all available sources (git, CI, issues, code, logs) into a single situational briefing. Replaces manually checking 5 different dashboards.

**When to use**: At the start of any session, when picking up unfamiliar work, or when resuming after time away.

**Example**:
```
/context-dump What's the state of the billing-service repo?
```

Claude will:
1. Check git status, recent commits, branches
2. Check CI status via `gh`
3. List open PRs and high-priority issues
4. Identify key files and potential landmines
5. Present everything as a concise briefing

**GSD integration**: Use before `/gsd:resume-work` or `/gsd:progress` to get full situational awareness before deciding what to work on.

---

## Workflow Recipes

### Recipe: Full Implementation Cycle

```
/context-dump                    # Understand the current state
/surface-assumptions <task>      # Clarify before committing
/plan-and-review <task>          # Plan with staff review
/test-first <task>               # Implement with TDD
/scope-check                     # Verify no scope creep
/simplify                        # Check for over-engineering
/dead-code-sweep                 # Clean up after yourself
/change-summary                  # Document what was done
/grill-me                        # Final review
/update-claude-md                # Capture learnings
```

### Recipe: Quick Bug Fix

```
/clarify <bug description>       # Make sure you understand the bug
/inline-plan <fix>               # Quick plan
/test-first <fix>                # Write regression test, then fix
/prove-it                        # Prove it's fixed
/change-summary                  # Document
```

### Recipe: Codebase Onboarding

```
/context-dump                    # Get the lay of the land
/ascii-diagram <system>          # Visualize architecture
/explain-visual <key area>       # Deep dive on important parts
/techdebt                        # Understand what needs work
```

### Recipe: Code Review

```
/grill-me                        # Aggressive review
/scope-check                     # Verify scope discipline
/prove-it                        # Demand proof it works
/simplify                        # Check for over-engineering
```

### Recipe: Refactor

```
/surface-assumptions <refactor>  # Clarify the goal
/plan-and-review <refactor>      # Plan the approach
/naive-first <new approach>      # Get correctness first
/dead-code-sweep                 # Clean up the old code
/prove-it                        # Prove behavior is preserved
```

---

## Source Attribution

**Boris Cherny** — Creator and team lead of Claude Code. Tips sourced from team practices for parallel workflows, plan mode, CLAUDE.md maintenance, custom skills, bug fixing, prompting techniques, subagent usage, and learning patterns.

**Andrej Karpathy** — Senior software engineering principles for agentic coding workflows. Covers assumption surfacing, confusion management, pushback culture, simplicity enforcement, scope discipline, dead code hygiene, test-first development, naive-then-optimize patterns, inline planning, and structured change reporting.
