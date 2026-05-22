---
name: memory-init
description: Initialize project memory storage in the current directory and Google Drive. Use when the user invokes /memory-init to set up memory for this project.
version: 1.0.0
allowed-tools: ["Bash", "Write", "mcp__claude_ai_Google_Drive__authenticate", "mcp__claude_ai_Google_Drive__create_file"]
disable-model-invocation: true
---

# Memory Init

Set up memory storage for the current project — both locally and in Google Drive.

## Steps

1. **Detect project name**: Use the basename of the current working directory (`$PWD`) as `{project-name}`.

2. **Create local structure**: Use Bash to create these directories and files:
   ```
   .memory/
   .memory/decisions/
   .memory/context/
   .memory/notes/
   ```
   Then create `.memory/index.md` with this content (replace `{project-name}` and `{date}`):
   ```markdown
   # Memory Index — {project-name}
   
   | Date | Category | Title | File |
   |------|----------|-------|------|
   ```
   Then create `.memory/.config.json` with:
   ```json
   {
     "project": "{project-name}",
     "initialized": "{ISO-8601 timestamp}",
     "drive_folder": "claude-memory/{project-name}"
   }
   ```

3. **Create Drive index**: Use `mcp__claude_ai_Google_Drive__create_file` to create a file named `claude-memory-{project-name}-index.md` in Google Drive with this content:
   ```markdown
   # Memory Index — {project-name}
   
   Initialized: {ISO-8601 timestamp}
   
   | Date | Category | Title | Filename |
   |------|----------|-------|----------|
   ```
   If authentication is needed, call `mcp__claude_ai_Google_Drive__authenticate` first.

4. **Confirm to the user**:
   - Local path: `.memory/` in the current directory
   - Drive file: the file ID returned by the create_file call
   - Tell the user the workflow: `/memory-draft` → `/memory-refine` → `/memory-save`

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages.
