---
name: memory-sync
description: Upload all local .memory/ files to Google Drive (bulk sync). Use when the user invokes /memory-sync to back up or push local memory entries after offline work.
version: 1.0.0
allowed-tools: ["Read", "Bash", "mcp__claude_ai_Google_Drive__authenticate", "mcp__claude_ai_Google_Drive__create_file"]
disable-model-invocation: true
---

# Memory Sync

Read all local `.memory/` files and upload them to Google Drive.

## Process

1. **Detect project name**: Read `.memory/.config.json` for the `project` field, or fall back to the basename of `$PWD`. If `.memory/` does not exist, tell the user to run `/memory-init` first and stop.

2. **Enumerate local files**: Use Bash to list all `.md` files under `.memory/` recursively, excluding `index.md`:
   ```bash
   find .memory -name "*.md" ! -name "index.md"
   ```
   Then use Read to load each file's content.

3. **Upload each file**: For every local file `.memory/{category}/{filename}.md`, use `mcp__claude_ai_Google_Drive__create_file` to upload it to Google Drive with the name:
   ```
   {project}-{category}-{filename}
   ```
   If authentication is needed, call `mcp__claude_ai_Google_Drive__authenticate` first before the first upload.

4. **Track results**:
   - Count successful uploads
   - Note any failures with the filename and error

5. **Confirm** to the user:
   - Total files found locally
   - Files successfully uploaded to Drive
   - Any failures (with filenames)
   - Remind them they can use `/memory-load` or `/memory-search` to retrieve entries in future sessions

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages.
