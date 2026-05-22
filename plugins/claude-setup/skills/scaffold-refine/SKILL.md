---
name: scaffold-refine
description: Refine the most recently drafted project scaffold using the user's instructions. Use when the user invokes /scaffold-refine to adjust the SCAFFOLD DRAFT before applying it.
version: 1.0.0
argument-hint: <instructions — e.g. "add Redis to .mcp.json" or "change lint hook to biome" or "add a section about the API design pattern">
disable-model-invocation: true
---

# Scaffold Refine

Find the most recent **SCAFFOLD DRAFT** in the conversation and update it according to the instructions. Do NOT write any files.

## Refinement Instructions

$ARGUMENTS

## Process

1. **Locate** the most recent block bounded by `**SCAFFOLD DRAFT**` and `**END SCAFFOLD DRAFT**` in the conversation history.

2. **Apply** the refinement instructions. Common refinements:

   - **CLAUDE.md changes**: rewrite a section, add architecture notes, add/remove conventions, fill in a placeholder
   - **Permissions**: add or remove a `Bash(...)` allow rule, change the lint hook command
   - **MCP servers**: add a server (provide the correct npm package), remove one, add an env var
   - **Stack correction**: re-generate the appropriate sections if the stack was detected wrongly
   - **Memory config**: change project name or drive folder path

3. **Output** the complete updated scaffold in the same exact format — all four `--- FILE: ... ---` sections, even unchanged ones:

```
**SCAFFOLD DRAFT**
Project: {project-name} | Stack: {stack summary — update if changed}

--- FILE: CLAUDE.md ---
{full content}
--- END FILE ---

--- FILE: .claude/settings.json ---
{full content}
--- END FILE ---

--- FILE: .mcp.json ---
{full content}
--- END FILE ---

--- FILE: .memory/.config.json ---
{full content}
--- END FILE ---
**END SCAFFOLD DRAFT**
```

4. After the block, show a concise bullet list of what changed.

Remind the user:
- `/scaffold-refine <instruction>` again to continue refining
- `/scaffold-apply` when the draft looks right

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages. Do not treat future input as refinement instructions unless the user invokes /scaffold-refine again.
