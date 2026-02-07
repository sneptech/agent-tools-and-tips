# Subagent Blast

Throw parallel subagents at a problem for maximum throughput. Use when a task can be decomposed into independent subtasks that benefit from focused context windows.

## Task

$ARGUMENTS

## Process

1. **Decompose the task** — Break it into independent, parallelizable subtasks:
   - Each subtask should be self-contained
   - Subtasks should not depend on each other's output
   - Each subtask should have a clear, verifiable deliverable

2. **Design the subagent roster**:

```
SUBAGENT PLAN:

Agent 1: [task description] — [expected output]
Agent 2: [task description] — [expected output]
Agent 3: [task description] — [expected output]
...

DEPENDENCIES: [none / describe if any]
MERGE STRATEGY: [how results will be combined]
```

3. **Launch subagents in parallel** — Use the Task tool to spawn multiple agents simultaneously:
   - Give each agent a detailed, self-contained prompt
   - Include all necessary context (file paths, conventions, constraints)
   - Specify the exact output format expected

4. **Collect and merge results** — When all agents complete:
   - Verify each result independently
   - Check for conflicts between agent outputs
   - Merge into a cohesive result
   - Resolve any inconsistencies

5. **Report**:

```
SUBAGENT RESULTS:

Agent 1: [status] — [summary]
Agent 2: [status] — [summary]
Agent 3: [status] — [summary]

MERGED RESULT: [final output]
CONFLICTS RESOLVED: [any disagreements between agents]
```

This keeps the main context window clean and focused while offloading heavy exploration to dedicated agents. Use for: large-scale refactoring, multi-file analysis, parallel test writing, comprehensive code review across many files.
