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

Before creating files, list which files already exist using Bash (`ls -la CLAUDE.md .claude/settings.json .mcp.json .memory/.config.json 2>/dev/null`).

If any file already exists, list them and ask the user to confirm overwrite before continuing. Wait for confirmation — do not proceed if any target file already exists without explicit user approval.

### Step 3 — Create files

Parse each `--- FILE: {path} ---` ... `--- END FILE ---` section from the draft and write its content to disk:

1. **`CLAUDE.md`** — use the Write tool to create it at the project root.

2. **`.claude/settings.json`** — use Bash to create the `.claude/` directory if it does not exist (`mkdir -p .claude`), then use the Write tool to write `.claude/settings.json`.

3. **`.mcp.json`** — use the Write tool to create it at the project root.

4. **`.memory/` structure** — use Bash to create directories:
   ```
   mkdir -p .memory/decisions .memory/context .memory/notes
   ```
   Then write `.memory/.config.json` from the draft content.
   Then create `.memory/index.md` with this content (fill in project name and date):
   ```markdown
   # Memory Index — {project-name}

   | Date | Category | Title | File |
   |------|----------|-------|------|
   ```

5. **Google Drive index** — use `mcp__claude_ai_Google_Drive__create_file` to create `claude-memory-{project-name}-index.md` in Drive with the same index content. If authentication is needed, call `mcp__claude_ai_Google_Drive__authenticate` first. If Drive is unavailable, skip this step and note it — the user can run `/memory-init` later.

### Step 4 — Add to .gitignore

Check if `.gitignore` exists. If it does, append these lines if not already present:
```
.env
.env.*
.memory/
```
If `.gitignore` doesn't exist, create it with those three lines.

### Step 5 — Confirm

Report a clean summary:

```
Project scaffold applied for: {project-name}

Created:
  ✓ CLAUDE.md
  ✓ .claude/settings.json
  ✓ .mcp.json
  ✓ .memory/ (decisions/, context/, notes/, .config.json, index.md)
  ✓ .memory/ folder created in Google Drive  [or: skipped — run /memory-init to set up Drive]
  ✓ .gitignore updated

Next steps:
  1. Review CLAUDE.md and fill in any placeholders
  2. Set env vars referenced in .mcp.json (e.g. GITHUB_TOKEN)
  3. Use /memory-draft → /memory-save to persist key decisions as you work
```

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages.
