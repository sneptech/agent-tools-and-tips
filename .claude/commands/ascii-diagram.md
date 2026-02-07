# ASCII Diagram

Draw clear ASCII diagrams of architecture, data flow, protocols, or system relationships. Useful for understanding and documenting complex systems without leaving the terminal.

## Target

$ARGUMENTS

## Process

1. **Analyze the target** — Read relevant code, configs, and documentation
2. **Identify what to diagram** — Choose the most useful view:
   - System architecture (components and connections)
   - Data flow (how data moves through the system)
   - Sequence diagram (how components interact over time)
   - State machine (states and transitions)
   - Dependency graph (what depends on what)
   - Directory/module structure

3. **Draw the diagram(s)** — Use clean ASCII art:

Example styles:
```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Client   │────▶│   API    │────▶│    DB    │
└──────────┘     └──────────┘     └──────────┘
                      │
                      ▼
                 ┌──────────┐
                 │  Cache   │
                 └──────────┘
```

```
User Request
    │
    ▼
[Auth Middleware] ──fail──▶ [401 Response]
    │
   pass
    │
    ▼
[Route Handler] ──error──▶ [Error Handler]
    │
   success
    │
    ▼
[Response]
```

4. **Annotate** — Add brief explanations for non-obvious connections
5. **Present** — Show the diagram with a legend if needed

Keep diagrams focused on one concern at a time. Multiple small diagrams are better than one sprawling mess.
