# Claude Code — Context System

> [Back to README](../README.md) | [Memory System](MEMORY-SYSTEM.md) | [Tools](TOOLS.md) | [Settings](SETTINGS-CONFIG.md)

Understanding how Claude Code gathers, manages, and uses context is key to getting the best results.

---

## What Is Context?

Context is everything Claude knows during a conversation:
- The system prompt (built-in instructions)
- Your project files (read on demand)
- CLAUDE.md files (loaded automatically)
- Memory files (loaded automatically)
- Conversation history (messages back and forth)
- Tool results (output from commands, file reads, searches)

Claude has a **context window** — a maximum amount of information it can hold at once. Managing this well is critical for long sessions.

---

## Context Layers

### Layer 1: System Prompt

The foundation. This includes:
- Claude's built-in behavior instructions
- Available tools and their descriptions
- Permission rules
- Current date, model info, platform details

You can customize this with CLI flags:
```bash
claude --system-prompt "You are a Python expert"
claude --append-system-prompt "Always use TypeScript"
```

### Layer 2: CLAUDE.md Files

These are **automatically loaded** at the start of every conversation. They provide persistent project instructions.

**Where they can live:**

| Location | Scope | Example |
|----------|-------|---------|
| `~/.claude/CLAUDE.md` | Global (all projects) | Personal preferences, coding style |
| `./CLAUDE.md` | Project root | Build commands, architecture, conventions |
| `./src/CLAUDE.md` | Subdirectory | Module-specific instructions |
| `./.claude/CLAUDE.md` | Project (hidden) | Same as root, but hidden |

**What to put in CLAUDE.md:**
```markdown
# Project: My App

## Build & Test
- Run tests: `npm test`
- Build: `npm run build`
- Lint: `npm run lint`

## Conventions
- Use TypeScript strict mode
- Prefer functional components
- Error messages should be user-friendly

## Architecture
- Frontend: React + Vite
- Backend: Express + PostgreSQL
- Auth: JWT tokens
```

**Key points:**
- Claude reads these automatically — you don't need to mention them
- More specific CLAUDE.md files (subdirectory) supplement broader ones (root)
- Keep them concise — they consume context every conversation
- Use `/init` to create one, `/memory` to edit

### Layer 3: Memory Files

Persistent memories stored in `~/.claude/projects/<project>/memory/`. These carry knowledge between conversations.

**Types of memories:**

| Type | What It Stores |
|------|---------------|
| `user` | Your role, preferences, expertise |
| `feedback` | How you want Claude to work (corrections + confirmations) |
| `project` | Ongoing work context, goals, decisions |
| `reference` | Pointers to external resources |

Memories are indexed in `MEMORY.md` and loaded automatically. See [MEMORY.md](./MEMORY-SYSTEM.md) for full details.

### Layer 4: Conversation History

Everything said in the current session:
- Your messages
- Claude's responses
- Tool calls and their results
- Errors and outputs

This grows over time and is the main consumer of context.

### Layer 5: Tool Results

When Claude reads files, runs commands, or searches code, the results are added to context. Large outputs can consume significant context.

---

## Managing Context

### Check Context Usage

```
/context
```
Shows a colored grid visualization of what's consuming your context window, with optimization suggestions.

### Compress Context

```
/compact
/compact "focus on the authentication changes"
```
Compresses conversation history while preserving key information. You can provide instructions to focus the compression.

### Clear Context

```
/clear
```
Completely resets the conversation. Use when you're switching to a completely different task.

### Automatic Compression

When context approaches the limit, Claude Code automatically compresses older messages. You don't lose the conversation — it's summarized.

---

## Context Best Practices

### Do

1. **Use CLAUDE.md** for info Claude needs every session (build commands, conventions)
2. **Use `/compact`** regularly during long sessions
3. **Be specific** in prompts — vague prompts cause Claude to read more files
4. **Use `/context`** to monitor usage
5. **Break large tasks** into smaller conversations
6. **Name sessions** (`/rename`) so you can resume later with `/resume`

### Don't

1. **Don't paste huge files** into the chat — let Claude read them with tools
2. **Don't repeat instructions** that are in CLAUDE.md
3. **Don't keep old context** around when switching tasks — use `/clear`
4. **Don't over-fill CLAUDE.md** — keep it concise and high-value

---

## How Claude Reads Your Project

Claude does NOT read your entire codebase at once. It reads files **on demand** using tools:

| Tool | What It Does | Context Cost |
|------|-------------|-------------|
| `Read` | Reads a specific file | Adds file content to context |
| `Glob` | Finds files by pattern | Adds file paths (small) |
| `Grep` | Searches file contents | Adds matching lines |
| `Bash` | Runs commands | Adds command output |

This means Claude is efficient — it only loads what it needs. But each read adds to context, so longer sessions accumulate more.

---

## Context Flow Diagram

```
┌─────────────────────────────────────────────┐
│              Context Window                  │
│                                              │
│  ┌──────────────┐  ┌────────────────────┐   │
│  │ System Prompt │  │   CLAUDE.md Files  │   │
│  │  (built-in)   │  │ (auto-loaded)      │   │
│  └──────────────┘  └────────────────────┘   │
│                                              │
│  ┌──────────────┐  ┌────────────────────┐   │
│  │Memory Files   │  │ Conversation       │   │
│  │(auto-loaded)  │  │ History            │   │
│  └──────────────┘  │ (grows over time)   │   │
│                     │                    │   │
│  ┌──────────────┐  │  - Your messages    │   │
│  │ Tool Results  │  │  - Claude replies   │   │
│  │ (on demand)   │  │  - Tool outputs     │   │
│  │               │  │                    │   │
│  │ - File reads  │  └────────────────────┘   │
│  │ - Grep results│                           │
│  │ - Bash output │                           │
│  └──────────────┘                            │
└─────────────────────────────────────────────┘
```

---

## Working Directory

Claude is always aware of its **working directory** — the folder where it operates. This determines:
- Where file paths are resolved
- Which CLAUDE.md files are loaded
- Which git repo is active
- Which memory project folder is used

Add additional directories with:
```
/add-dir ../other-project
```
or
```bash
claude --add-dir ../apps ../lib
```
