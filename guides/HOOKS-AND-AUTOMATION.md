# Claude Code — Hooks & Automation

> [Back to README](../README.md) | [Settings](SETTINGS-CONFIG.md) | [Skills](SKILLS.md) | [Workflows](WORKFLOWS.md)

Hooks let you run shell commands automatically in response to Claude Code events.

---

## What Are Hooks?

Hooks are shell commands that execute at specific points during Claude Code's operation. They're configured in `settings.json` and run by the Claude Code harness (not by Claude itself).

Think of them as **triggers**: when event X happens, automatically run command Y.

---

## Types of Hooks

### Pre-Tool Hooks

Run **before** a tool executes.

**Use cases:**
- Validate inputs before a command runs
- Block certain operations
- Log what Claude is about to do
- Run a linter before file writes

### Post-Tool Hooks

Run **after** a tool completes.

**Use cases:**
- Auto-format code after edits
- Run tests after file changes
- Send notifications when tasks complete
- Log tool results

### Prompt-Submit Hooks

Run **when you submit a prompt**.

**Use cases:**
- Pre-process your input
- Add context automatically
- Validate prompts

---

## Configuring Hooks

Use the `/update-config` skill to set up hooks:

```
/update-config
```

Then describe the behavior you want:
- "After every file edit, run prettier on the file"
- "Before any git push, run the test suite"
- "When I submit a prompt, log it to a file"

Hooks are stored in `settings.json`:
- Project: `.claude/settings.json`
- User: `~/.claude/settings.json`

---

## Hook Behavior

- Hooks run as shell commands in the project directory
- If a pre-tool hook fails (non-zero exit), the tool call is **blocked**
- Hook output is fed back to Claude as feedback
- Hooks run synchronously — Claude waits for them to complete

---

## Examples

### Auto-format on edit
"After every Edit tool call, run `prettier --write` on the edited file"

### Lint before commit
"Before any `git commit` Bash command, run `npm run lint`"

### Notify on completion
"After any Bash command that takes more than 30 seconds, send a desktop notification"

---

## Hooks vs. CLAUDE.md Instructions

| Feature | Hooks | CLAUDE.md |
|---------|-------|-----------|
| **Execution** | Automatic (harness runs them) | Claude follows instructions voluntarily |
| **Reliability** | Guaranteed to run | Claude might forget or skip |
| **Scope** | Specific tool events | General behavior guidance |
| **Format** | Shell commands | Natural language |

Use hooks for things that **must** happen. Use CLAUDE.md for things that **should** happen.

---

## MCP Servers

MCP (Model Context Protocol) extends Claude Code with external tools and services.

### What MCP Does

MCP servers add new tools to Claude's toolkit. When connected, Claude can use them like built-in tools.

### Common MCP Integrations

| Integration | What It Provides |
|-------------|-----------------|
| **IDE (VS Code, JetBrains)** | Execute code, get diagnostics, navigate code |
| **Databases** | Query and modify data directly |
| **APIs** | Interact with external services |
| **Custom tools** | Anything you build as an MCP server |

### Managing MCP

```
/mcp                           # Interactive MCP manager
claude --mcp-config ./mcp.json # Load MCP config from file
claude --strict-mcp-config     # Only use specified MCP servers
```

### MCP Commands as Slash Commands

MCP servers can expose prompts that become slash commands:
```
/mcp__<server-name>__<prompt-name>
```
These are discovered automatically when the server connects.

---

## Plugins

Plugins extend Claude Code with additional skills and capabilities.

### Managing Plugins

```
/plugin              # Install, enable, disable, update plugins
/reload-plugins      # Reload without restarting
```

### Plugin Directories

```bash
claude --plugin-dir ./my-plugins    # Load plugins from custom directory
```

---

## Automation Patterns

### Pattern 1: Quality Gate
Set up hooks so Claude can't commit code that doesn't pass tests:
```
Pre-hook on Bash(git commit *) → run test suite → block if tests fail
```

### Pattern 2: Continuous Formatting
Auto-format every file Claude edits:
```
Post-hook on Edit → run formatter on the file
```

### Pattern 3: Scheduled Tasks
Use `/schedule` to run agents on a cron schedule:
```
/schedule "Every morning at 9am, check for dependency updates"
```

### Pattern 4: Recurring Monitoring
Use `/loop` to poll during a session:
```
/loop 5m check if the build is still passing
```
