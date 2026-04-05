# Claude Course

**The complete learning resource for Claude Code** — Anthropic's agentic coding tool.

Whether you're just getting started or looking to master advanced features, this repo covers everything: commands, skills, context management, tools, memory, hooks, prompting techniques, real-world workflows, and hands-on exercises.

---

## Who Is This For?

- **Beginners** who just installed Claude Code and want to learn the basics
- **Intermediate users** who want to unlock productivity with skills, memory, and automation
- **Advanced users** who want to master sub-agents, hooks, MCP servers, and CI/CD integration
- **Teams** looking for a shared reference to onboard members onto Claude Code

---

## How to Use This Repo

Start with this README for an overview, then dive into the detailed guides:

| Start Here | What You'll Learn |
|------------|-------------------|
| **[Commands Reference](COMMANDS.md)** | Every slash command, keyboard shortcut, CLI flag, and vim binding |
| **[Cheat Sheet](CHEATSHEET.md)** | One-page quick reference — print it or pin it |

### Deep-Dive Guides

| Guide | What It Covers |
|-------|---------------|
| **[Skills](guides/SKILLS.md)** | What skills are, every built-in skill explained with examples |
| **[Context System](guides/CONTEXT.md)** | How context works — 5 layers, management, best practices |
| **[Tools](guides/TOOLS.md)** | Every tool (Read, Edit, Bash, Agent, etc.), permissions, MCP |
| **[Memory System](guides/MEMORY-SYSTEM.md)** | 4 memory types, file format, lifecycle, what to save vs. skip |
| **[Hooks & Automation](guides/HOOKS-AND-AUTOMATION.md)** | Hooks, MCP servers, plugins, automation patterns |
| **[Prompting Tips](guides/PROMPTING-TIPS.md)** | How to write effective prompts — patterns, templates, anti-patterns |
| **[Workflows](guides/WORKFLOWS.md)** | 12 step-by-step workflows (debugging, features, reviews, DevOps) |
| **[Settings & Config](guides/SETTINGS-CONFIG.md)** | Settings files, permissions, hooks config, environment variables |
| **[Git & GitHub](guides/GIT-AND-GITHUB.md)** | Setup, commits, branches, PRs, issues, CI/CD, permissions, troubleshooting |

### Practice & Templates

| Resource | Description |
|----------|-------------|
| **[Exercises](examples/README.md)** | 12 hands-on exercises from beginner to advanced |
| **[CLAUDE.md Template](templates/CLAUDE.md)** | Basic starter template for any project |
| **[Project Starter Kit](project-starter/)** | **Ready-to-use config files** — copy into any project and fill in the blanks |

#### What's in the Project Starter Kit

| File | What It Does |
|------|-------------|
| [`CLAUDE.md`](project-starter/CLAUDE.md) | Project instructions Claude reads every session (tech stack, conventions, rules) |
| [`IDENTITY.md`](project-starter/IDENTITY.md) | Tell Claude who you are, your role, experience, coding style, preferences |
| [`GIT-RULES.md`](project-starter/GIT-RULES.md) | How Claude handles git — commit style, branch rules, PR format, safety rules |
| [`settings.json`](project-starter/settings.json) | Permissions and hooks — copy to `.claude/settings.json` |

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

**Prerequisites:**
- Node.js 18+
- An Anthropic account (Pro, Max, or Team plan, or API key)

**First steps after install:**
```bash
claude              # Start a session
/init               # Create a CLAUDE.md for your project
/terminal-setup     # Configure keyboard shortcuts
/doctor             # Verify everything works
```

---

## Core Commands

These are built-in CLI commands you can use inside a Claude Code session:

| Command | Description |
|---------|-------------|
| `/help` | Get help with using Claude Code |
| `/clear` | Clear conversation context |
| `/compact` | Compress conversation to save context space |
| `/context` | Visualize context usage |
| `/diff` | View changes Claude has made |
| `/fast` | Toggle fast mode (same model, faster output) |
| `/cost` | Show token usage and cost for the session |
| `/model` | Switch between models (Opus, Sonnet, Haiku) |
| `/plan` | Enter plan mode (read-only analysis) |
| `/init` | Initialize a CLAUDE.md file in the project |
| `/memory` | Edit Claude's memory files |
| `/permissions` | View and manage tool permissions |
| `/doctor` | Diagnose installation issues |
| `/quit` | Exit the session |
| `! <command>` | Run a shell command directly from the prompt |

> **Full reference:** See [COMMANDS.md](COMMANDS.md) for every command, shortcut, and flag.

---

## Slash Commands & Skills

Slash commands invoke **skills** — specialized routines Claude Code can execute:

| Slash Command | Description |
|---------------|-------------|
| `/simplify` | Review changed code for reuse, quality, and efficiency |
| `/loop <interval> <command>` | Run a command on a recurring interval (e.g., `/loop 5m /foo`) |
| `/schedule` | Create, update, list, or run scheduled remote agents (cron-based) |
| `/update-config` | Configure Claude Code settings via `settings.json` |
| `/keybindings-help` | Customize keyboard shortcuts and keybindings |
| `/claude-api` | Get help building apps with the Claude API or Anthropic SDK |

> **Deep dive:** See [guides/SKILLS.md](guides/SKILLS.md) for detailed explanation of each skill.

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

- Claude maintains context throughout a session
- When context approaches limits, older messages are automatically compressed
- Use `/compact` to manually compress context
- Use `/clear` to reset context entirely

> **Deep dive:** See [guides/CONTEXT.md](guides/CONTEXT.md) for context layers, flow diagram, and management tips.

---

## Tools Available to Claude

Claude Code has access to a set of built-in tools:

| Tool | Purpose |
|------|---------|
| **Read** | Read files from the filesystem (code, images, PDFs, notebooks) |
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

> **Deep dive:** See [guides/TOOLS.md](guides/TOOLS.md) for how each tool works, permission system, and tool call flow.

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

### Quick Usage

```
Remember that I prefer TypeScript over JavaScript     # saves user memory
Remember to always run tests before committing         # saves feedback memory
Forget my language preference                          # removes a memory
/memory                                                # view and edit memories
```

> **Deep dive:** See [guides/MEMORY-SYSTEM.md](guides/MEMORY-SYSTEM.md) for file format, lifecycle, and best practices.

---

## Hooks & Automation

Hooks are shell commands that execute in response to Claude Code events (like tool calls). Configure them in `settings.json` to automate behaviors:

- **Pre-tool hooks** — Run before a tool executes (validate, block, log)
- **Post-tool hooks** — Run after a tool executes (format, test, notify)
- **Prompt-submit hooks** — Run when you submit a prompt

Use `/update-config` to set up hooks, or see [guides/HOOKS-AND-AUTOMATION.md](guides/HOOKS-AND-AUTOMATION.md) for patterns and JSON examples.

---

## MCP Servers

MCP (Model Context Protocol) servers extend Claude Code with additional tools and capabilities. They allow Claude to interact with external services like:

- IDEs (VS Code, JetBrains)
- Databases
- APIs
- Custom tools

> **Deep dive:** See [guides/HOOKS-AND-AUTOMATION.md](guides/HOOKS-AND-AUTOMATION.md) for MCP configuration and management.

---

## Models

Claude Code is powered by the latest Claude model family:

| Model | ID | Best For |
|-------|----|----------|
| **Claude Opus 4.6** | `claude-opus-4-6` | Complex reasoning, large codebases |
| **Claude Sonnet 4.6** | `claude-sonnet-4-6` | Balanced speed and quality |
| **Claude Haiku 4.5** | `claude-haiku-4-5-20251001` | Fast, lightweight tasks |

- Fast mode (`/fast`) uses the same model with faster output — it does NOT switch models
- Switch models mid-session with `/model sonnet` or `Alt+P`

---

## Tips and Best Practices

1. **Be specific** — Clear, detailed prompts produce better results than vague ones
2. **Use CLAUDE.md** — Put project conventions there so Claude follows them every session
3. **Use memory** — Tell Claude to remember your preferences so you don't repeat yourself
4. **Use sub-agents** — For complex tasks, Claude can launch parallel agents to work faster
5. **Read before editing** — Claude should always read a file before modifying it
6. **Use `/compact`** — When conversations get long, compress to save context
7. **Use `! command`** — Run shell commands directly from the prompt when needed
8. **Check permissions** — Use `/permissions` to control what tools Claude can auto-execute
9. **Use plans for big tasks** — Ask Claude to plan before implementing complex features
10. **Report issues** — File bugs at https://github.com/anthropics/claude-code/issues

> **More tips:** See [guides/PROMPTING-TIPS.md](guides/PROMPTING-TIPS.md) for prompt patterns, templates, and anti-patterns.

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
│   ├── SETTINGS-CONFIG.md            # Settings, permissions, hooks configuration
│   └── GIT-AND-GITHUB.md             # Git/GitHub setup, commits, PRs, CI/CD
├── examples/
│   └── README.md                      # Hands-on exercises from beginner to advanced
├── templates/
│   └── CLAUDE.md                      # Basic starter CLAUDE.md template
└── project-starter/                   # COPY THESE INTO YOUR PROJECTS
    ├── README.md                      # Setup instructions
    ├── CLAUDE.md                      # Project instructions (fill in the blanks)
    ├── IDENTITY.md                    # Your role, style, preferences
    ├── GIT-RULES.md                   # Commit, branch, PR, and safety rules
    └── settings.json                  # Permissions and hooks → .claude/settings.json
```

---

## Suggested Learning Path

| Step | What to Do | Time |
|------|-----------|------|
| 1 | Read this README for the overview | 10 min |
| 2 | Skim the [Cheat Sheet](CHEATSHEET.md) | 5 min |
| 3 | Try [Exercises 1-3](examples/README.md) (Beginner) | 15 min |
| 4 | Read [Context System](guides/CONTEXT.md) | 10 min |
| 5 | Read [Prompting Tips](guides/PROMPTING-TIPS.md) | 10 min |
| 6 | Try [Exercises 4-7](examples/README.md) (Intermediate) | 20 min |
| 7 | Read [Tools](guides/TOOLS.md) and [Memory](guides/MEMORY-SYSTEM.md) | 15 min |
| 8 | Read [Workflows](guides/WORKFLOWS.md) | 10 min |
| 9 | Try [Exercises 8-12](examples/README.md) (Advanced) | 30 min |
| 10 | Set up [hooks and config](guides/SETTINGS-CONFIG.md) for your projects | 15 min |

---

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Claude Code GitHub](https://github.com/anthropics/claude-code)
- [Anthropic API Docs](https://docs.anthropic.com)
- [Report Issues](https://github.com/anthropics/claude-code/issues)

---

## Contributing

Found something missing or incorrect? Contributions are welcome:
1. Fork this repo
2. Create a branch for your changes
3. Submit a pull request

---

*This project is for learning purposes. Not affiliated with Anthropic.*
