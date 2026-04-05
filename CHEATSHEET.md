# Claude Code — Cheat Sheet

A one-page quick reference. Print it, pin it, bookmark it.

> [Back to README](README.md) | [Full Commands Reference](COMMANDS.md) | [Exercises](examples/README.md)

---

## Essential Commands

| Do This | Type This |
|---------|-----------|
| Get help | `/help` |
| Clear conversation | `/clear` |
| Compress context | `/compact` |
| Check context usage | `/context` |
| Check cost | `/cost` |
| View changes | `/diff` |
| Switch model | `/model sonnet` or `Alt+P` |
| Toggle fast mode | `/fast` or `Alt+O` |
| Enter plan mode | `/plan` |
| Run shell command | `! npm test` |
| Quick side question | `/btw what is X?` |
| Exit | `/exit` or `Ctrl+D` |

---

## Essential Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel/stop |
| `Ctrl+D` | Exit |
| `Esc Esc` | Rewind/undo |
| `Shift+Tab` | Cycle permission mode |
| `Alt+P` | Switch model |
| `Alt+T` | Toggle thinking |
| `Alt+O` | Toggle fast mode |
| `Ctrl+B` | Background task |
| `Ctrl+R` | Search history |
| `Ctrl+G` | Edit in external editor |
| `\` + `Enter` | New line (multiline input) |

---

## File Operations

| Task | How |
|------|-----|
| Read a file | Claude uses `Read` tool automatically |
| Edit a file | Claude uses `Edit` tool (targeted replacement) |
| Create a file | Claude uses `Write` tool |
| Find files | Claude uses `Glob` (e.g., `**/*.ts`) |
| Search code | Claude uses `Grep` (regex search) |
| Run command | Claude uses `Bash` |

---

## Memory Quick Reference

| Save | Forget | View |
|------|--------|------|
| "Remember that I prefer TypeScript" | "Forget my language preference" | `/memory` |

**Types:** `user` (who you are) · `feedback` (how to work) · `project` (what's happening) · `reference` (where to look)

---

## Context Management

```
Running low on context?
  1. /compact          ← compress conversation
  2. /compact "focus on auth work"  ← compress with focus
  3. /clear            ← nuclear option: fresh start
  4. /context          ← see what's using space
```

---

## Permission Modes

| Mode | What Happens |
|------|-------------|
| `default` | Asks for risky ops |
| `acceptEdits` | Auto-approves file edits |
| `plan` | Read-only, no changes |
| `auto` | Auto-approves everything |

Cycle with `Shift+Tab`.

---

## Common Workflows

```
Debug a bug:     "There's a bug where X happens. Find and fix it."
Add a feature:   "Add a login page with email/password auth."
Review code:     "Review the changes in this PR for issues."
Refactor:        "Refactor the auth module to use middleware pattern."
Write tests:     "Write tests for the UserService class."
Explain code:    "Explain how the caching layer works."
```

---

## CLI Quick Start

```bash
claude                          # Start session
claude "fix the login bug"      # Start with prompt
claude -c                       # Continue last session
claude -r "my-session"          # Resume named session
claude -p "explain X"           # One-shot (no interaction)
claude --model opus             # Use specific model
claude -p --output-format json  # JSON output
cat file | claude -p "review"   # Pipe input
```

---

## Project Setup

```bash
claude /init                    # Create CLAUDE.md
claude /memory                  # Set up memory
claude /permissions             # Configure permissions
claude /terminal-setup          # Fix keyboard shortcuts
```

---

## Skills

| Skill | What |
|-------|------|
| `/simplify` | Review + fix code quality |
| `/loop 5m <cmd>` | Repeat every 5 min |
| `/schedule` | Cron-based agents |
| `/update-config` | Configure settings/hooks |
| `/claude-api` | Help with Claude API |

---

## Sub-Agents

| Type | Use For |
|------|---------|
| `Explore` | Fast codebase search |
| `Plan` | Architecture planning |
| `claude-code-guide` | Claude Code questions |
| `general-purpose` | Complex multi-step tasks |

---

## Golden Rules

1. **Be specific** — "Fix the null check in auth.ts line 42" > "fix the bug"
2. **Let Claude read first** — Don't paste huge files, let it use tools
3. **Use CLAUDE.md** — Put project rules there, not in every prompt
4. **Use memory** — Save preferences once, not every session
5. **Compact often** — Don't wait until context runs out
6. **Name sessions** — `/rename auth-refactor` so you can resume later
