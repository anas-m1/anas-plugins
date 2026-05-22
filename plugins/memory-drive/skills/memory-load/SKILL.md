---
name: memory-load
description: Search Google Drive for memory entries matching the query and inject their content into the conversation as context. Use when the user invokes /memory-load with a topic or keyword.
version: 1.0.0
argument-hint: <query — e.g. "auth" or "database decisions" or "last session context">
allowed-tools: ["Read", "mcp__claude_ai_Google_Drive__authenticate", "mcp__claude_ai_Google_Drive__search_files", "mcp__claude_ai_Google_Drive__read_file_content"]
disable-model-invocation: true
---

# Memory Load

Search Google Drive for memory entries matching the user's query and inject their content into the conversation so they can be used as context.

## Query

$ARGUMENTS

## Process

1. **Detect project name**: Read `.memory/.config.json` (if it exists) for the `project` field, or fall back to the basename of `$PWD`.

2. **Search Drive**: Use `mcp__claude_ai_Google_Drive__search_files` with a query that combines `{project-name}` and the user's query terms. If authentication is needed, call `mcp__claude_ai_Google_Drive__authenticate` first.

3. **Select relevant files**: Pick up to 5 files most relevant to the query. Prefer files whose names include the query terms.

4. **Read each file**: Use `mcp__claude_ai_Google_Drive__read_file_content` for each selected file.

5. **Output a LOADED MEMORY block**:

```
**LOADED MEMORY** (project: {project-name}, query: "{query}")

--- Entry 1: {filename} ({category}, {created date}) ---
{content}

--- Entry 2: {filename} ({category}, {created date}) ---
{content}

...

**END LOADED MEMORY**
```

6. After the block, state how many entries were loaded and confirm they are now available as context for this session.

If no matching files are found, tell the user and suggest running `/memory-search` with different terms or `/memory-sync` if memory hasn't been uploaded yet.

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages. The loaded memory content is now available as context.
