# Explain Visual

Generate a visual HTML presentation that explains unfamiliar code, architecture, or protocols. Makes surprisingly good slides for understanding complex systems.

## Target

$ARGUMENTS

## Process

1. **Analyze the target** — Read and deeply understand the code/system/protocol
2. **Identify key concepts** — Break it into digestible chunks:
   - High-level architecture
   - Data flow
   - Key abstractions and their relationships
   - Important state transitions
   - Entry points and exit points

3. **Generate an HTML file** — Create a self-contained HTML presentation with:
   - Clean, readable styling (dark theme, monospace for code)
   - One concept per section/slide
   - ASCII or SVG diagrams for architecture
   - Syntax-highlighted code snippets for key parts
   - Annotations explaining the "why" not just the "what"
   - Navigation between sections
   - A summary/cheat-sheet at the end

4. **Write the file** — Save as `explanation-[topic].html` in the project root

The presentation should be understandable by someone who has never seen this codebase before. Focus on building intuition, not exhaustive documentation. Use diagrams liberally — a picture is worth a thousand lines of prose.
