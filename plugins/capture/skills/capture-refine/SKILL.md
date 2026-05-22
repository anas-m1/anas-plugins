---
name: capture-refine
description: Adjust the most recently proposed memory change before applying it. Use when the user invokes /capture-refine with instructions about what to change in the MEMORY PROPOSAL.
version: 1.0.0
argument-hint: <changes — e.g. "change type to feedback" or "make the description more specific" or "add a Why line">
disable-model-invocation: true
---

# Capture Refine

Find the most recent **MEMORY PROPOSAL** in the conversation and update it according to the instructions. Do NOT write any files.

## Changes Requested

$ARGUMENTS

## Process

1. **Locate** the most recent block bounded by `**MEMORY PROPOSAL**` and `**END MEMORY PROPOSAL**` in the conversation.

2. **Apply** the requested changes. Common refinements:
   - Change memory type (user / feedback / project / reference)
   - Rename the file
   - Rewrite or expand the body content
   - Add or fix the `**Why:**` or `**How to apply:**` lines
   - Make the description more specific or shorter
   - Change CREATE to UPDATE (or vice versa) if the action is wrong
   - Fix the MEMORY.md line to be more descriptive

3. **Output** the full updated proposal in the same exact format — all sections, even unchanged ones:

```
**MEMORY PROPOSAL**

Action: {CREATE|UPDATE} {filename}
Type: {user|feedback|project|reference}

--- PROPOSED FILE ---
{full file content including frontmatter}
--- END FILE ---

--- MEMORY.md LINE ---
- [{Title}]({filename}) — {one-line hook, under 120 chars}
--- END LINE ---

**END MEMORY PROPOSAL**
```

Include the `--- REPLACES EXISTING ---` section if the action is UPDATE.

4. After the proposal, show a concise bullet list of what changed.

5. End with:

> Does this look right? Say **yes** to save, or run `/capture-refine <changes>` again to keep adjusting.

Stop here. Do not write any files. Return to normal assistant behavior after showing the updated proposal.
