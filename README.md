# anas-plugins

Personal Claude Code plugin marketplace.

## Install this marketplace

Add the following to `~/.claude/plugins/known_marketplaces.json`:

```json
"anas-plugins": {
  "source": {
    "source": "github",
    "repo": "anas-m1/anas-plugins"
  },
  "installLocation": "~/.claude/plugins/marketplaces/anas-plugins",
  "lastUpdated": "2026-05-22T00:00:00.000Z"
}
```

Then install any plugin with:

```
/plugin install <plugin-name>@anas-plugins
```

## Plugins

### prompt-toolkit

Three-command prompt engineering workflow for Claude Code.

| Command | Usage | What it does |
|---------|-------|--------------|
| `/prompt-craft` | `/prompt-craft <raw prompt>` | Improves your prompt for clarity, structure, and effectiveness |
| `/prompt-refine` | `/prompt-refine <instructions>` | Steers the last improved prompt based on your instructions |
| `/prompt-run` | `/prompt-run` | Executes the last improved/refined prompt |

**Install:**
```
/plugin install prompt-toolkit@anas-plugins
```

---

### capture

Save context or instructions to the project's `.memory/` folder with a preview-and-confirm step.

| Command | Usage | What it does |
|---------|-------|--------------|
| `/capture` | `/capture <instruction>` | Drafts a memory entry — shows exactly what will be written before touching any file |
| `/capture-refine` | `/capture-refine <changes>` | Adjusts the proposed memory entry before saving |
| `/capture-apply` | `/capture-apply` | Writes the approved memory entry to `.memory/` and updates the index |

**Install:**
```
/plugin install capture@anas-plugins
```

---

### memory-drive

Persist project memory to Google Drive. Draft, refine, and commit memory entries from conversation or files. Load and search memory across sessions.

| Command | Usage | What it does |
|---------|-------|--------------|
| `/memory-init` | `/memory-init` | Sets up local `.memory/` structure and a Google Drive index file |
| `/memory-draft` | `/memory-draft <instruction>` | Previews what would be saved without writing any files |
| `/memory-refine` | `/memory-refine <instructions>` | Adjusts the current draft before saving |
| `/memory-save` | `/memory-save` | Commits the draft to local disk and uploads it to Google Drive |
| `/memory-load` | `/memory-load <query>` | Searches Drive and injects matching entries as context |
| `/memory-search` | `/memory-search <query>` | Browses Drive entries with previews without loading full content |
| `/memory-sync` | `/memory-sync` | Bulk-uploads all local `.memory/` files to Google Drive |

**Requires:** Google Drive MCP (built into claude.ai).

**Install:**
```
/plugin install memory-drive@anas-plugins
```

---

### claude-setup

Scaffold a new Claude Code project — generates `CLAUDE.md`, `.claude/settings.json`, `.mcp.json`, and `.memory/` configuration in one workflow, tailored to the detected tech stack.

| Command | Usage | What it does |
|---------|-------|--------------|
| `/scaffold` | `/scaffold <project description>` | Detects the stack and drafts all four files — nothing is written yet |
| `/scaffold-refine` | `/scaffold-refine <instructions>` | Adjusts the draft (MCP servers, permissions, CLAUDE.md sections, etc.) |
| `/scaffold-apply` | `/scaffold-apply` | Creates all files from the draft with a safety check for existing files |

**Supported stacks:** Node.js, Python, Go, Rust (auto-detected from project files).

**Install:**
```
/plugin install claude-setup@anas-plugins
```
