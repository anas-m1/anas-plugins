---
name: memory-draft
description: Preview what would be saved to memory based on the user's instruction — outputs a MEMORY DRAFT block without writing any files. Use when the user invokes /memory-draft with an instruction like "save context of last 2 exchanges" or "save the auth decision".
version: 1.0.0
argument-hint: <instruction — e.g. "save the last decision about auth" or "save context of last 3 exchanges">
allowed-tools: ["Read"]
disable-model-invocation: true
---

# Memory Draft

Analyze the user's instruction and produce a **preview** of what would be saved to memory. Do NOT write any files.

## Instruction

$ARGUMENTS

## Process

1. **Parse the instruction** to understand:
   - **Source**: conversation history (e.g. "last 2 exchanges"), a specific file (e.g. "from CLAUDE.md"), or a manual note (freeform text in the instruction)
   - **Category**: infer from content:
     - `decisions` — architectural choices, technology picks, design decisions
     - `context` — session summaries, background, what was discussed
     - `notes` — reminders, tasks, freeform observations

2. **Extract the content**:
   - For conversation source: summarize or excerpt the relevant turns
   - For file source: read the file and extract the relevant section
   - For manual note: use the instruction text itself as the content

3. **Generate the draft** and output it in this exact format:

```
**MEMORY DRAFT**
---
project: {basename of $PWD}
created: {current ISO-8601 timestamp}
category: {decisions|context|notes}
tags: [{comma-separated inferred tags}]
source: {conversation|file|manual}
---

{content — written in clear, concise prose or bullet points. Include enough context to be useful when read in a future session with no other background.}

**END MEMORY DRAFT**
```

4. **After the block**, show a brief bullet list:
   - What was captured
   - Why this category was chosen
   - Suggested tags if any were inferred

5. **Do NOT write any files.** This is a preview only.

Remind the user they can:
- Run `/memory-refine <instructions>` to adjust the draft
- Run `/memory-save` to commit it to local disk and Google Drive

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages. Do not treat future input as a new draft unless the user invokes /memory-draft again.
