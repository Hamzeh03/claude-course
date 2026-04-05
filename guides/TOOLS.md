# Claude Code — Tools Reference

> [Back to README](../README.md) | [Commands](../COMMANDS.md) | [Context](CONTEXT.md) | [Settings](SETTINGS-CONFIG.md)

Tools are the actions Claude Code can take. Every file read, edit, search, and command is a tool call.

---

## Core Tools

### Read

**What:** Reads a file from the filesystem.

**Capabilities:**
- Read any file type (code, text, config, etc.)
- Read images (PNG, JPG) — Claude sees them visually
- Read PDFs (use `pages` param for large PDFs, max 20 pages per request)
- Read Jupyter notebooks (.ipynb) — shows all cells with outputs
- Read specific line ranges with `offset` and `limit`

**When Claude uses it:**
- Before editing any file (required — Claude must read before editing)
- When you ask about code in a specific file
- To understand existing implementations

**Example use cases:**
- "Read the auth middleware" → Claude reads the file
- "What does this function do?" → Claude reads the relevant file
- "Review this screenshot" → Claude reads the image file

---

### Write

**What:** Creates a new file or completely rewrites an existing one.

**When Claude uses it:**
- Creating brand new files
- Complete rewrites where Edit would be impractical

**Important:** Claude prefers `Edit` over `Write` for existing files. Write overwrites everything.

---

### Edit

**What:** Makes targeted string replacements in files.

**When Claude uses it:**
- Modifying existing code (the most common editing operation)
- Fixing bugs, adding features, refactoring
- Renaming variables (`replace_all` mode)

**How it works:**
- Finds an exact string match (`old_string`)
- Replaces it with new content (`new_string`)
- The match must be unique in the file (or use `replace_all`)

**Why Edit over Write:**
- Smaller changes = easier to review
- Shows exactly what changed (like a diff)
- Less risk of accidentally overwriting unrelated code

---

### Glob

**What:** Finds files by name/pattern.

**When Claude uses it:**
- Finding files: `**/*.tsx`, `src/components/**/*.test.ts`
- Locating config files: `**/tsconfig.json`
- Discovering project structure

**Replaces:** `find`, `ls` commands

---

### Grep

**What:** Searches file contents using regex.

**When Claude uses it:**
- Finding where a function is defined or used
- Searching for patterns: `function\s+\w+`, `import.*from`
- Finding TODOs, errors, specific strings

**Output modes:**
- `files_with_matches` — just file paths (default)
- `content` — matching lines with context
- `count` — match counts per file

**Replaces:** `grep`, `rg` commands

---

### Bash

**What:** Executes shell commands.

**When Claude uses it:**
- Running tests: `npm test`
- Git operations: `git status`, `git diff`
- Installing packages: `npm install`
- Building projects: `make`, `cargo build`
- Any command that needs shell execution

**Important:** Claude prefers dedicated tools over Bash equivalents:
- `Read` instead of `cat`
- `Edit` instead of `sed`
- `Grep` instead of `grep`
- `Glob` instead of `find`

Bash is reserved for commands that truly need shell execution.

---

### Agent

**What:** Launches sub-agents for complex, parallel tasks.

**Sub-agent types:**

| Type | What It Does | When to Use |
|------|-------------|-------------|
| `general-purpose` | Multi-step research, code search, task execution | Complex tasks that need multiple steps |
| `Explore` | Fast codebase exploration | Finding files, searching code, answering codebase questions |
| `Plan` | Software architecture planning | Designing implementation strategies, identifying trade-offs |
| `claude-code-guide` | Claude Code feature expert | Questions about Claude Code itself, API, SDK |

**Thoroughness levels for Explore:**
- `quick` — basic searches
- `medium` — moderate exploration
- `very thorough` — comprehensive analysis across multiple locations

**Key features:**
- Agents can run in parallel (multiple agents at once)
- Agents can run in background (`run_in_background`)
- Agents can use isolated git worktrees (`isolation: "worktree"`)
- Each agent has its own context — doesn't consume parent context

---

### WebSearch

**What:** Searches the web for information.

**When Claude uses it:**
- Looking up documentation
- Finding solutions to errors
- Researching libraries or APIs

---

### WebFetch

**What:** Fetches content from a URL.

**When Claude uses it:**
- Reading documentation pages
- Fetching API responses
- Getting content from links you provide

---

### NotebookEdit

**What:** Edits Jupyter notebook cells.

**When Claude uses it:**
- Modifying .ipynb files
- Adding/editing/removing notebook cells

---

### TaskCreate / TaskUpdate / TaskGet / TaskList

**What:** Creates and manages tasks to track work progress.

**When Claude uses it:**
- Breaking complex work into steps
- Tracking progress on multi-step tasks
- Showing you what's done and what's remaining

---

## Permission System

Every tool call goes through the permission system:

### Permission Modes

| Mode | Behavior |
|------|----------|
| `default` | Ask for approval on risky operations |
| `acceptEdits` | Auto-approve file edits, ask for others |
| `plan` | Read-only — Claude can only plan, not execute |
| `auto` | Auto-approve everything |
| `bypassPermissions` | Skip all permission prompts (dangerous) |

Switch modes with `Shift+Tab` or `/permissions`.

### Permission Rules

Configure which tools run automatically:
```
/permissions
```

Rules can be:
- **Allow** — runs without asking (e.g., `Read`, `Glob`)
- **Ask** — prompts you each time
- **Deny** — blocked entirely

You can set granular rules like:
- `Bash(git *)` — allow all git commands
- `Bash(npm test)` — allow only npm test

---

## Tool Call Flow

```
You send a message
        │
        ▼
Claude decides which tool(s) to use
        │
        ▼
Permission check
   ┌────┴────┐
   │         │
 Allowed   Needs approval
   │         │
   │     You approve/deny
   │         │
   ▼         ▼
Tool executes
        │
        ▼
Result added to context
        │
        ▼
Claude processes result
        │
        ▼
Response or next tool call
```

---

## MCP Tools

MCP (Model Context Protocol) servers add custom tools. When connected, their tools appear alongside built-in tools.

Common MCP integrations:
- **IDE tools** — execute code, get diagnostics from VS Code/JetBrains
- **Database tools** — query databases directly
- **API tools** — interact with external services
- **Custom tools** — anything you build

Manage MCP connections with `/mcp`.

---

## Tips

1. **Claude chains tools** — a single request might involve Read → Grep → Edit → Bash
2. **Parallel tool calls** — Claude can call multiple independent tools at once
3. **Background tasks** — long-running Bash commands can run in the background
4. **Tool results consume context** — large outputs (big files, verbose commands) use up context space
5. **Agent tool isolates context** — sub-agents don't add their full work to your context, only the summary
