# Claude Course

A learning project for mastering Claude Code — Anthropic's CLI tool, its features, skills, context system, and commands.

---

## Table of Contents

- [What is Claude Code](#what-is-claude-code)
- [Installation](#installation)
- [Core Commands](#core-commands)
- [Slash Commands](#slash-commands)
- [Skills](#skills)
- [Context System](#context-system)
- [Tools Available to Claude](#tools-available-to-claude)
- [Memory System](#memory-system)
- [Hooks](#hooks)
- [MCP Servers](#mcp-servers)
- [Models](#models)
- [Tips and Best Practices](#tips-and-best-practices)

---

## What is Claude Code

Claude Code is Anthropic's official agentic coding tool. It runs in your terminal (CLI), desktop app, web app (claude.ai/code), and IDE extensions (VS Code, JetBrains). It can read, write, and search your codebase, run shell commands, and manage complex multi-step software engineering tasks.

---

## Installation

```bash
# Install via npm
npm install -g @anthropic-ai/claude-code

# Run it
claude
```

---

## Core Commands

These are built-in CLI commands you can use inside a Claude Code session:

| Command | Description |
|---------|-------------|
| `/help` | Get help with using Claude Code |
| `/clear` | Clear conversation context |
| `/compact` | Compress conversation to save context space |
| `/fast` | Toggle fast mode (same model, faster output) |
| `/cost` | Show token usage and cost for the session |
| `/quit` | Exit the session |
| `/init` | Initialize a CLAUDE.md file in the project |
| `/memory` | Edit Claude's memory files |
| `/permissions` | View and manage tool permissions |
| `/status` | Show session status |
| `/terminal-setup` | Set up terminal integration |
| `! <command>` | Run a shell command directly from the prompt |

---

## Slash Commands

Slash commands invoke **skills** — specialized routines Claude Code can execute:

| Slash Command | Description |
|---------------|-------------|
| `/commit` | Create a well-formatted git commit |
| `/review-pr` | Review a pull request |
| `/simplify` | Review changed code for reuse, quality, and efficiency |
| `/loop <interval> <command>` | Run a command on a recurring interval (e.g., `/loop 5m /foo`) |
| `/schedule` | Create, update, list, or run scheduled remote agents (cron-based) |
| `/update-config` | Configure Claude Code settings via `settings.json` |
| `/keybindings-help` | Customize keyboard shortcuts and keybindings |
| `/claude-api` | Get help building apps with the Claude API or Anthropic SDK |

---

## Skills

Skills are specialized capabilities that Claude Code can invoke. They provide domain-specific knowledge and workflows.

### Built-in Skills

- **update-config** — Configure the Claude Code harness via `settings.json`. Use for automated behaviors ("from now on when X", "each time X").
- **keybindings-help** — Customize keyboard shortcuts, rebind keys, add chord bindings.
- **simplify** — Reviews changed code for reuse, quality, and efficiency, then fixes issues.
- **loop** — Run a prompt or slash command on a recurring interval.
- **schedule** — Create and manage scheduled remote agents (cron-based triggers).
- **claude-api** — Help building apps with the Claude API, Anthropic SDK, or Agent SDK.

---

## Context System

Claude Code uses several layers of context to understand your project:

### CLAUDE.md Files

`CLAUDE.md` is a special file Claude reads automatically. Use it to provide:
- Project-specific instructions
- Coding conventions and style guides
- Build and test commands
- Architecture notes

```
# Locations where CLAUDE.md can live:
./CLAUDE.md              # Project root (most common)
./src/CLAUDE.md          # Subdirectory-specific
~/.claude/CLAUDE.md      # Global (applies to all projects)
```

### Conversation Context

- Claude maintains context throughout a session.
- When context approaches limits, older messages are automatically compressed.
- Use `/compact` to manually compress context.
- Use `/clear` to reset context entirely.

### Working Directory

Claude is always aware of the current working directory and uses it as the root for file operations, searches, and git commands.

---

## Tools Available to Claude

Claude Code has access to a set of built-in tools:

| Tool | Purpose |
|------|---------|
| **Read** | Read files from the filesystem |
| **Write** | Create new files |
| **Edit** | Make targeted edits to existing files |
| **Glob** | Find files by name/pattern (e.g., `**/*.ts`) |
| **Grep** | Search file contents using regex |
| **Bash** | Execute shell commands |
| **Agent** | Launch sub-agents for complex, parallel tasks |
| **WebSearch** | Search the web |
| **WebFetch** | Fetch content from URLs |
| **NotebookEdit** | Edit Jupyter notebooks |
| **TaskCreate/TaskUpdate** | Manage tasks and track progress |

### Agent Sub-types

The Agent tool can launch specialized sub-agents:

- **general-purpose** — Multi-step tasks, code search, research
- **Explore** — Fast codebase exploration (quick / medium / very thorough)
- **Plan** — Software architect for designing implementation plans
- **claude-code-guide** — Answer questions about Claude Code features, API, SDK

---

## Memory System

Claude Code has a persistent, file-based memory system that carries knowledge between conversations.

### Memory Types

| Type | Purpose |
|------|---------|
| **user** | Who you are, your role, preferences, expertise |
| **feedback** | How you want Claude to work (corrections and confirmations) |
| **project** | Ongoing work context, goals, decisions, deadlines |
| **reference** | Pointers to external resources (Linear, Slack, Grafana, etc.) |

### How Memory Works

- Memories are stored as Markdown files with frontmatter
- An index file (`MEMORY.md`) tracks all memories
- Claude reads relevant memories at the start of conversations
- Ask Claude to "remember X" to save something, or "forget X" to remove it

---

## Hooks

Hooks are shell commands that execute in response to Claude Code events (like tool calls). Configure them in `settings.json` to automate behaviors:

- **Pre-tool hooks** — Run before a tool executes
- **Post-tool hooks** — Run after a tool executes
- **Prompt-submit hooks** — Run when you submit a prompt

Use `/update-config` to set up hooks.

---

## MCP Servers

MCP (Model Context Protocol) servers extend Claude Code with additional tools and capabilities. They allow Claude to interact with external services like:

- IDEs (VS Code, JetBrains)
- Databases
- APIs
- Custom tools

---

## Models

Claude Code is powered by the latest Claude model family:

| Model | ID |
|-------|----|
| **Claude Opus 4.6** | `claude-opus-4-6` |
| **Claude Sonnet 4.6** | `claude-sonnet-4-6` |
| **Claude Haiku 4.5** | `claude-haiku-4-5-20251001` |

- Fast mode (`/fast`) uses the same model with faster output — it does NOT switch models.
- When building AI apps, default to the latest and most capable models.

---

## Tips and Best Practices

1. **Be specific** — Clear, detailed prompts produce better results than vague ones.
2. **Use CLAUDE.md** — Put project conventions there so Claude follows them every session.
3. **Use memory** — Tell Claude to remember your preferences so you don't repeat yourself.
4. **Use sub-agents** — For complex tasks, Claude can launch parallel agents to work faster.
5. **Read before editing** — Claude should always read a file before modifying it.
6. **Use `/compact`** — When conversations get long, compress to save context.
7. **Use `! command`** — Run shell commands directly from the prompt when needed.
8. **Check permissions** — Use `/permissions` to control what tools Claude can auto-execute.
9. **Use plans for big tasks** — Ask Claude to plan before implementing complex features.
10. **Report issues** — File bugs at https://github.com/anthropics/claude-code/issues

---

## Project Structure

```
claude-course/
├── README.md                          # Overview of Claude Code (this file)
├── COMMANDS.md                        # Complete commands, shortcuts, and CLI flags reference
├── CHEATSHEET.md                      # One-page quick reference card
├── guides/
│   ├── SKILLS.md                      # Skills system — what they are and how to use each
│   ├── CONTEXT.md                     # Context system — layers, management, best practices
│   ├── TOOLS.md                       # All tools — Read, Edit, Bash, Agent, etc.
│   ├── MEMORY-SYSTEM.md              # Memory types, storage, lifecycle, management
│   ├── HOOKS-AND-AUTOMATION.md       # Hooks, MCP servers, plugins, automation patterns
│   ├── PROMPTING-TIPS.md             # How to write effective prompts
│   ├── WORKFLOWS.md                  # Step-by-step common workflows
│   └── SETTINGS-CONFIG.md            # Settings, permissions, hooks configuration
├── examples/
│   └── README.md                      # Hands-on exercises from beginner to advanced
└── templates/
    └── CLAUDE.md                      # Starter CLAUDE.md template for your projects
```

---

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Anthropic API Docs](https://docs.anthropic.com)

---

*This project is for learning purposes.*
