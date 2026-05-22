---
name: scaffold-apply
description: Create all files from the most recently drafted project scaffold — CLAUDE.md, .claude/settings.json, .mcp.json, and .memory/ structure. Use when the user invokes /scaffold-apply to commit the SCAFFOLD DRAFT to disk.
version: 1.0.0
allowed-tools: ["Bash", "Write", "mcp__claude_ai_Google_Drive__authenticate", "mcp__claude_ai_Google_Drive__create_file"]
disable-model-invocation: true
---

# Scaffold Apply

Find the most recent **SCAFFOLD DRAFT** in the conversation and create every file it defines.

## Process

### Step 1 — Locate the draft

Find the most recent block bounded by `**SCAFFOLD DRAFT**` and `**END SCAFFOLD DRAFT**`. If none is found, tell the user to run `/scaffold` first and stop.

### Step 2 — Safety check

Before creating files, list which files already exist using Bash:
```
ls -la CLAUDE.md .mcp.json .claude/settings.json .claude/stack.md .claude/architecture.md .claude/memory/MEMORY.md .claude/memory/.config.json 2>/dev/null
```

If any file already exists, list them and ask the user to confirm overwrite before continuing. Wait for confirmation — do not proceed if any target file already exists without explicit user approval.

### Step 3 — Create files

Parse each `--- FILE: {path} ---` ... `--- END FILE ---` section from the draft and write its content to disk. First, create all required directories:

```bash
mkdir -p .claude/memory
```

Then write each file using the Write tool:

1. **`CLAUDE.md`** — project root.
2. **`.claude/stack.md`** — tech stack and commands reference.
3. **`.claude/architecture.md`** — layout, patterns, conventions, guardrails.
4. **`.claude/memory/MEMORY.md`** — memory index.
5. **`.claude/settings.json`** — permissions and hooks.
6. **`.mcp.json`** — project root.
7. **`.claude/memory/.config.json`** — memory-drive config.

After writing files, attempt to create a Google Drive index using `mcp__claude_ai_Google_Drive__create_file` (authenticate first if needed). If Drive is unavailable, skip and note it.

### Step 4 — Add to .gitignore

Check if `.gitignore` exists. If it does, append these lines if not already present:
```
.env
.env.*
```
If `.gitignore` doesn't exist, create it with those two lines.

### Step 5 — Confirm

Report a clean summary:

```
Project scaffold applied for: {project-name}

Created:
  ✓ CLAUDE.md
  ✓ .claude/stack.md
  ✓ .claude/architecture.md
  ✓ .claude/memory/MEMORY.md
  ✓ .claude/settings.json
  ✓ .mcp.json
  ✓ .claude/memory/.config.json
  ✓ .gitignore updated
  ✓ Google Drive index created  [or: skipped — Drive unavailable]

Next steps:
  1. Fill in placeholder values in .claude/stack.md and .claude/architecture.md
  2. Set env vars referenced in .mcp.json (e.g. GITHUB_TOKEN, DATABASE_URL)
  3. Run /project-init to scaffold the actual project directory structure
```

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages.
