---
name: memory-refine
description: Refine the most recently drafted memory entry using the user's instructions. Use when the user invokes /memory-refine with steering instructions like "change category to decisions" or "add tag authentication".
version: 1.0.0
argument-hint: <instructions — e.g. "add tag auth, change category to decisions, trim to 3 bullets">
disable-model-invocation: true
---

# Memory Refine

Find the most recent **MEMORY DRAFT** block in the conversation and refine it according to the user's instructions.

## Refinement Instructions

$ARGUMENTS

## Process

1. **Locate** the most recent block bounded by `**MEMORY DRAFT**` and `**END MEMORY DRAFT**` in the conversation history.

2. **Apply** the refinement instructions. Common refinements:
   - Change `category` (decisions / context / notes)
   - Add or remove `tags`
   - Rewrite or trim the content body
   - Change `source` field
   - Adjust tone, detail level, or structure of the content

3. **Output** the updated draft in the same exact format:

```
**MEMORY DRAFT**
---
project: {same as before unless changed}
created: {same timestamp as before}
category: {updated category}
tags: [{updated tags}]
source: {same or updated}
---

{updated content}

**END MEMORY DRAFT**
```

4. **After the block**, show a concise bullet list of what changed.

5. **Do NOT write any files.** This is still a preview.

Remind the user they can:
- Run `/memory-refine <instructions>` again to continue refining
- Run `/memory-save` to commit the draft

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages. Do not treat future input as refinement instructions unless the user invokes /memory-refine again.
