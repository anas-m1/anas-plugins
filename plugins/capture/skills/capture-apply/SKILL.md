---
name: capture-apply
description: Write the approved memory change to disk. Use when the user invokes /capture-apply or says "yes" after reviewing a MEMORY PROPOSAL.
version: 1.0.0
allowed-tools: ["Read", "Write"]
disable-model-invocation: true
---

# Capture Apply

Find the most recent approved **MEMORY PROPOSAL** and write it to the memory directory. This is the only skill in the capture plugin that touches files.

## Process

### Step 1 — Find the proposal

Locate the most recent block bounded by `**MEMORY PROPOSAL**` and `**END MEMORY PROPOSAL**` in the conversation. If none is found, tell the user to run `/capture` first and stop.

### Step 2 — Parse the proposal

Extract from the proposal:
- **Action**: CREATE or UPDATE
- **Filename**: the target `.md` file (e.g. `feedback_terse_responses.md`)
- **File content**: everything inside `--- PROPOSED FILE ---` ... `--- END FILE ---`
- **MEMORY.md line**: the entry inside `--- MEMORY.md LINE ---` ... `--- END LINE ---`

### Step 3 — Write the memory file

Use the Write tool to write the file content to:
```
.memory/{filename}
```

in the current working directory.

- For **CREATE**: write the new file as-is.
- For **UPDATE**: overwrite the existing file with the proposed content.

### Step 4 — Update .memory/index.md

Read the current `.memory/index.md` from the current working directory.

- For **CREATE**: append the new line from the proposal to the end of the file.
- For **UPDATE**: find the existing line that references the same filename and replace it with the new line from the proposal. If no existing line is found, append it.

Write the updated `.memory/index.md` back.

### Step 5 — Confirm

Report what was done in one compact block:

```
Memory saved.

  File: {filename} ({CREATE|UPDATE})
  Type: {type}
  Index: MEMORY.md updated

"{description from the memory file frontmatter}"
```

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages.
