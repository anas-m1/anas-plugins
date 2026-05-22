---
name: capture
description: Draft a memory change from the current context or a specific instruction — shows exactly what will be written before touching any file. Use when the user invokes /capture with an instruction like "remember that I prefer concise responses" or "save the context of this session".
version: 1.0.0
argument-hint: <instruction — e.g. "remember I prefer dark mode" or "save last decision about auth" or just "save current context">
allowed-tools: ["Read"]
disable-model-invocation: true
---

# Capture

Analyze the instruction and current conversation, then produce a **MEMORY PROPOSAL** showing exactly what will be written to disk. Do NOT write anything yet.

## Instruction

$ARGUMENTS

## Process

### Step 1 — Read existing memory

Use the Read tool to read `.memory/index.md` in the current working directory. Also read any existing `.memory/*.md` files that seem related to the instruction, so you can detect duplicates and avoid redundancy. If `.memory/` does not exist, tell the user to run `/memory-init` first and stop.

### Step 2 — Classify the memory

Determine the memory **type** based on the instruction and content:

| Type | When to use |
|------|-------------|
| `user` | Facts about the user — role, preferences, expertise, how they like to work |
| `feedback` | Guidance about how Claude should behave — what to do, what to avoid, corrections |
| `project` | Ongoing work, decisions, goals, deadlines, context about what is being built |
| `reference` | Pointers to external resources — where things live, what tools/repos/channels to check |

### Step 3 — Determine the action

- **CREATE** a new file: if no similar memory exists
- **UPDATE** an existing file: if a closely related memory already exists — show the full updated content, not just the diff

Choose a descriptive kebab-case filename like `feedback_terse_responses.md` or `project_auth_decision.md`.

### Step 4 — Draft the memory content

Follow the exact format used by the memory system:

```markdown
---
name: {short-kebab-case-slug}
description: {one-line summary — specific enough to judge relevance at a glance}
metadata:
  type: {user|feedback|project|reference}
---

{memory body}
```

For **feedback** and **project** types, the body must include:
- The rule or fact on the first line
- `**Why:**` line explaining the reason
- `**How to apply:**` line explaining when/where this applies

For **user** type: describe the user fact in 1–3 sentences.
For **reference** type: state what the resource is and what it's used for.

Extract the content from:
- The `$ARGUMENTS` instruction (highest priority)
- Recent conversation turns (if instruction says "save context" or "save last N exchanges")
- Patterns in the current session that seem worth remembering

### Step 5 — Output the proposal

Output this exact format:

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

If the action is UPDATE, also show:
```
--- REPLACES EXISTING ---
{current content of the file being replaced}
--- END EXISTING ---
```

### Step 6 — Ask for confirmation

After the proposal block, write exactly:

> Does this look right? Say **yes** to save, run `/capture-refine <changes>` to adjust, or describe what to change and I'll update the proposal.

Stop here. Do not write any files. Do not treat the user's next message as a new capture unless they invoke /capture again. Return to normal assistant behavior after showing the proposal.
