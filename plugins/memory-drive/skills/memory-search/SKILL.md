---
name: memory-search
description: Search Google Drive for memory entries matching the query and list results with previews — without loading full content. Use when the user invokes /memory-search to browse available memory.
version: 1.0.0
argument-hint: <query — e.g. "auth" or "architecture" or "decisions">
allowed-tools: ["Read", "mcp__claude_ai_Google_Drive__authenticate", "mcp__claude_ai_Google_Drive__search_files", "mcp__claude_ai_Google_Drive__get_file_metadata"]
disable-model-invocation: true
---

# Memory Search

Search Google Drive for memory entries and show a browsable list of results.

## Query

$ARGUMENTS

## Process

1. **Detect project name**: Read `.memory/.config.json` (if it exists) for the `project` field, or fall back to the basename of `$PWD`.

2. **Search Drive**: Use `mcp__claude_ai_Google_Drive__search_files` with the project name and user query. If authentication is needed, call `mcp__claude_ai_Google_Drive__authenticate` first.

3. **For each result**, use `mcp__claude_ai_Google_Drive__get_file_metadata` to get filename and modified date if not already available.

4. **Display results** in a clean list:

```
Memory search results for "{query}" — project: {project-name}

1. {filename}
   Category: {decisions|context|notes}  |  Date: {YYYY-MM-DD}
   Preview: {first 2 lines of content or file description}

2. {filename}
   ...

Found {N} entries.
```

5. After the list, tell the user they can run `/memory-load {query}` to inject the full content of matching entries into the conversation.

If no results are found:
- Suggest trying broader search terms
- Suggest `/memory-sync` to upload any local `.memory/` files that haven't been pushed to Drive yet

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages.
