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
  "lastUpdated": "2026-05-21T00:00:00.000Z"
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
