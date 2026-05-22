---
name: scaffold
description: Draft a full project scaffold — CLAUDE.md, .claude/settings.json, .mcp.json, and .memory/ config — based on the project description. Use when the user invokes /scaffold with a brief description of what they are building.
version: 1.0.0
argument-hint: <project description — e.g. "a REST API in FastAPI with Postgres" or "a Next.js SaaS app with Stripe">
allowed-tools: ["Bash"]
disable-model-invocation: true
---

# Project Scaffold

Analyze the project description and existing directory, then produce a **SCAFFOLD DRAFT** showing every file that will be created. Do NOT write any files yet.

## Project Description

$ARGUMENTS

## Process

### Step 1 — Detect context

1. Use Bash to run `ls -la` in the current directory to see what already exists (package.json, pyproject.toml, requirements.txt, Cargo.toml, go.mod, etc.).
2. Combine that with `$ARGUMENTS` to determine:
   - **Project name**: basename of `$PWD` (or extract from description)
   - **Tech stack**: Node.js / Python / Go / Rust / other — and the specific framework (Next.js, FastAPI, Express, etc.)
   - **Services**: database (Postgres, MongoDB, SQLite, Redis), auth, third-party APIs
   - **Project type**: web app, REST API, CLI tool, library, fullstack

### Step 2 — Build each file

Generate the content for four files, tailored to the detected stack:

**A. `CLAUDE.md`** — a concise root file that uses `@` includes to pull in sub-documents from `.claude/`. Keep `CLAUDE.md` itself short; the detail lives in the referenced files.

```markdown
# {Project Name}

{One paragraph: what it does, who it's for, key constraints or non-goals.}

## Memory

@.claude/memory/MEMORY.md

## Stack & Commands

@.claude/stack.md

## Architecture & Conventions

@.claude/architecture.md
```

Then generate the referenced sub-documents:

**`.claude/stack.md`** — tech stack and commands:

```markdown
## Tech Stack
{List: language + version, framework, DB, key libraries, package manager}

## Commands
- Dev: `{command}`
- Test: `{command}`
- Lint: `{command}`
- Build/check: `{command}`
- DB migrations (if applicable): `{command}`
```

**`.claude/architecture.md`** — layout, patterns, conventions, and guardrails:

```markdown
## Repository Layout
- `{dir}/` — {purpose}
- {repeat for each key directory}

## Key Patterns
- {e.g. "all DB access via repository layer", "API routes in src/routes/"}

## Conventions
- {naming conventions, component structure, error handling pattern}
- {where to put new files, how to name tests}
- {any stack-specific idioms to follow}

## Do NOT
- Commit secrets, .env files, or credentials
- Edit migration files directly — always generate new ones
- Skip writing tests for new features
- {add any stack-specific cautions}
```

**`.claude/memory/MEMORY.md`** — empty index to start:

```markdown
# Memory Index — {project-name}

| Date | Category | Title | File |
|------|----------|-------|------|
```

**B. `.claude/settings.json`** — permissions pre-approved for the stack, plus a lint hook:

Node.js example:
```json
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(npx *)",
      "Bash(node *)",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git add *)",
      "Bash(git log *)",
      "Bash(git commit *)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [{ "type": "command", "command": "npm run lint --silent 2>&1 | tail -5" }]
      }
    ]
  }
}
```

Python example uses `python *`, `pip *`, `uv *`, `pytest *`; hook: `ruff check . 2>&1 | tail -5`
Go example uses `go *`; hook: `go vet ./... 2>&1 | tail -5`
Rust example uses `cargo *`; hook: `cargo clippy 2>&1 | tail -5`

Always include the git commands. Add DB CLI commands (psql, mongosh) if a database is detected.

**C. `.mcp.json`** — only servers relevant to this project:

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}" }
    }
  }
}
```

Add a Postgres server (`@modelcontextprotocol/server-postgres`) if Postgres is detected.
Add a filesystem server if the project reads/writes lots of local files outside `$PWD`.
Omit Google Drive — that's handled by the built-in claude.ai MCP.
Only include servers that are actually useful for this project.

**E. `.claude/memory/.config.json`** — memory-drive configuration:

```json
{
  "project": "{project-name}",
  "initialized": "{ISO-8601 timestamp}",
  "drive_folder": "claude-memory/{project-name}"
}
```

### Step 3 — Output the draft

Output the entire draft in this exact format (Claude and /scaffold-apply use the delimiters to parse it):

```
**SCAFFOLD DRAFT**
Project: {project-name} | Stack: {stack summary}

--- FILE: CLAUDE.md ---
{full content}
--- END FILE ---

--- FILE: .claude/settings.json ---
{full content}
--- END FILE ---

--- FILE: .mcp.json ---
{full content}
--- END FILE ---

--- FILE: .memory/.config.json ---
{full content}
--- END FILE ---
**END SCAFFOLD DRAFT**
```

After the draft, show a brief summary:
- Stack detected and why
- MCP servers included and why
- Permissions granted and the lint hook used
- Anything the user should fill in manually (placeholder values, missing info)

Remind the user:
- `/scaffold-refine <instruction>` to adjust anything
- `/scaffold-apply` to create all files

Stop here. Do not ask follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages. Do not treat future input as scaffold instructions unless the user invokes /scaffold again.
