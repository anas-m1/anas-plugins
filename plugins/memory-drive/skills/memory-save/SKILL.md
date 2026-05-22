---
name: memory-save
description: Commit the most recently drafted memory entry to local .memory/ and Google Drive. Use when the user invokes /memory-save to persist the current MEMORY DRAFT.
version: 1.0.0
allowed-tools: ["Bash", "Write", "mcp__claude_ai_Google_Drive__authenticate", "mcp__claude_ai_Google_Drive__create_file"]
disable-model-invocation: true
---

# Memory Save

Find the most recent **MEMORY DRAFT** block in the conversation and save it — to local disk and Google Drive.

## Process

1. **Locate** the most recent block bounded by `**MEMORY DRAFT**` and `**END MEMORY DRAFT**` in the conversation history. If none is found, tell the user to run `/memory-draft` first and stop.

2. **Parse** the draft:
   - Extract `project`, `created`, `category`, `tags`, `source` from the frontmatter
   - Extract the content body

3. **Generate filename**: `{YYYY-MM-DD}-{slug}.md` where `{slug}` is the first 5 significant words of the content, lowercased and hyphenated (skip articles like a/an/the).

4. **Write locally**: Use the Write tool to save the full draft content (frontmatter + body) to:
   ```
   .memory/{category}/{filename}.md
   ```
   If the `.memory/{category}/` directory does not exist, create it with Bash first.

5. **Update local index**: Append a row to `.memory/index.md`:
   ```
   | {YYYY-MM-DD} | {category} | {first line of content} | {category}/{filename}.md |
   ```

6. **Upload to Drive**: Use `mcp__claude_ai_Google_Drive__create_file` to create a file named:
   ```
   {project}-{category}-{filename}
   ```
   with the full draft content (same as local file). If authentication is needed, call `mcp__claude_ai_Google_Drive__authenticate` first.

7. **Confirm** to the user:
   - Local path: `.memory/{category}/{filename}.md`
   - Drive file ID (from the create_file response)
   - Number of entries now in `.memory/index.md`

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages.
