# Claude Code — Skills Guide

> [Back to README](../README.md) | [Commands](../COMMANDS.md) | [Context](CONTEXT.md) | [Tools](TOOLS.md)

Skills are specialized routines that extend Claude Code's capabilities. They are invoked via slash commands and provide domain-specific workflows.

---

## What Are Skills?

Skills are pre-built capabilities that Claude Code can execute. Unlike regular conversation, skills follow structured workflows designed for specific tasks. They have access to tools, templates, and domain knowledge.

Think of skills as **expert modes** — when you invoke a skill, Claude switches into a focused workflow optimized for that task.

---

## Built-in Skills

### /simplify

**What it does:** Reviews changed code for reuse, quality, and efficiency, then automatically fixes issues found.

**When to use:**
- After writing or modifying code
- When you want a quick quality check
- To catch redundancy, dead code, or inefficient patterns

**Example:**
```
/simplify
```
Claude will review recent changes and suggest/apply improvements.

---

### /loop

**What it does:** Runs a prompt or slash command on a recurring interval.

**When to use:**
- Monitoring a build or deployment
- Polling for status changes
- Running periodic checks (e.g., test suite, linting)

**Syntax:**
```
/loop <interval> <command>
```

**Examples:**
```
/loop 5m /cost              # Check cost every 5 minutes
/loop 10m ! npm test        # Run tests every 10 minutes
/loop 2m check deploy status # Ask Claude to check something every 2 minutes
```

Default interval is 10 minutes if not specified.

---

### /schedule

**What it does:** Creates, updates, lists, or runs scheduled remote agents (triggers) that execute on a cron schedule.

**When to use:**
- Setting up recurring automated tasks
- Creating cron jobs for Claude Code
- Managing scheduled agents that run in the cloud

**Examples:**
```
/schedule                           # List existing schedules
/schedule "Run tests every morning" # Create a new schedule
```

This creates **remote agents** that run on Anthropic's infrastructure on your defined schedule.

---

### /update-config

**What it does:** Configures Claude Code settings via `settings.json`, including automated behaviors (hooks).

**When to use:**
- Setting up automated behaviors ("from now on when X happens, do Y")
- Configuring hooks that run before/after tool calls
- Modifying Claude Code's runtime settings

**Important:** Automated behaviors require **hooks** configured in `settings.json`. Claude doesn't run these — the harness does. So if you want something to happen automatically, it must be a hook.

**Example:**
```
/update-config
```
Then describe the behavior you want.

---

### /keybindings-help

**What it does:** Helps customize keyboard shortcuts, rebind keys, and add chord bindings.

**When to use:**
- When you want to rebind keys (e.g., change submit key)
- Adding chord shortcuts (multi-key combinations)
- Modifying `~/.claude/keybindings.json`

**Example:**
```
/keybindings-help
```
Then describe what you want to customize.

---

### /claude-api

**What it does:** Helps build applications using the Claude API, Anthropic SDK, or Agent SDK.

**When to use:**
- Writing code that imports `anthropic` or `@anthropic-ai/sdk`
- Building apps that call the Claude API
- Working with the Agent SDK (`claude_agent_sdk`)
- Need help with tool use, streaming, or other API features

**When NOT to use:**
- Working with OpenAI or other AI SDKs (this skill is Anthropic-specific)

**Example:**
```
/claude-api
```
Then ask your API question.

---

## How Skills Work Internally

1. You type `/skill-name` in the prompt
2. Claude Code expands this into a full prompt with specialized instructions
3. The skill may have access to specific tools and context
4. Claude executes the skill's workflow and returns results

Skills are different from regular commands:
- **Commands** (like `/clear`, `/cost`) are built-in CLI operations
- **Skills** (like `/simplify`, `/loop`) are AI-powered workflows with specialized prompts

---

## Custom Skills

Skills can be extended through:

### Plugins
Use `/plugin` to install community or custom plugins that add new skills.

### MCP Prompts
MCP servers can expose prompts that become available as commands:
```
/mcp__<server>__<prompt>
```
These are dynamically discovered from connected MCP servers.

---

## Tips for Using Skills

1. **Use `/skills`** to see all currently available skills
2. **Skills are context-aware** — they can read your project, git state, and recent changes
3. **Combine skills** — use `/loop` to repeatedly run other skills
4. **Skills respect permissions** — they follow your permission mode settings
5. **Skills can be disabled** — use `--disable-slash-commands` flag if needed
