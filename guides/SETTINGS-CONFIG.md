# Claude Code — Settings & Configuration

Everything about configuring Claude Code to work the way you want.

---

## Settings Locations

Claude Code reads settings from multiple locations, in order of priority:

| Location | Scope | File |
|----------|-------|------|
| **CLI flags** | Session only | Command-line arguments |
| **Local project** | This machine + this project | `.claude/settings.local.json` |
| **Project** | Everyone on this project (committed) | `.claude/settings.json` |
| **User** | All your projects | `~/.claude/settings.json` |

Higher priority overrides lower. CLI flags always win.

---

## Accessing Settings

### Interactive

```
/config
```
or
```
/settings
```

Opens the settings UI where you can change theme, model, editor mode, and more.

### Via Skill

```
/update-config
```

Then describe what you want to change. Best for setting up hooks and automated behaviors.

### Via CLI Flag

```bash
claude --settings ./custom-settings.json
claude --setting-sources user,project
```

---

## Key Settings

### Permission Mode

Controls how Claude handles tool approval.

| Mode | Behavior | Best For |
|------|----------|---------|
| `default` | Asks before risky operations | Normal development |
| `acceptEdits` | Auto-approves file edits | Trusted editing sessions |
| `plan` | Read-only, no changes allowed | Code review, analysis |
| `auto` | Auto-approves everything | Automated pipelines |
| `bypassPermissions` | Skips all checks | CI/CD (dangerous) |

**Set via CLI:**
```bash
claude --permission-mode plan
```

**Cycle during session:** `Shift+Tab`

### Permission Rules

Fine-grained control over specific tools:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(git *)",
      "Bash(npm test)",
      "Bash(npm run lint)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force *)"
    ]
  }
}
```

**Manage interactively:**
```
/permissions
```

### Model Selection

```json
{
  "model": "claude-sonnet-4-6"
}
```

**Or via CLI:**
```bash
claude --model opus
claude --model sonnet
```

**Or during session:**
```
/model opus
```

### Theme

```
/theme
```

Options: light, dark, colorblind-accessible, ANSI variants.

### Editor Mode

Via `/config`:
- **Default** — standard text input
- **Vim** — full vim keybindings (see COMMANDS.md for details)

---

## Hooks Configuration

Hooks are the most powerful configuration. They run shell commands in response to events.

### Structure

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'About to edit a file'"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Prompt submitted'"
          }
        ]
      }
    ]
  }
}
```

### Hook Events

| Event | When It Fires | Common Use |
|-------|---------------|------------|
| `PreToolUse` | Before a tool runs | Validate, block, log |
| `PostToolUse` | After a tool runs | Format, test, notify |
| `UserPromptSubmit` | When you submit a prompt | Pre-process, log |

### Matcher Patterns

The `matcher` field determines which tool triggers the hook:

- `"Edit"` — matches Edit tool
- `"Bash"` — matches all Bash commands
- `"Bash(git *)"` — matches git commands only
- `"Bash(npm test)"` — matches only `npm test`
- `"*"` — matches everything

### Hook Behavior

- **Pre-hooks** that exit non-zero **block** the tool call
- Hook output is fed back to Claude as feedback
- Hooks run synchronously (Claude waits)
- Environment variables available: `$CLAUDE_FILE_PATH`, `$CLAUDE_TOOL_NAME`, etc.

### Easy Setup

Instead of editing JSON manually:
```
/update-config
After every file edit, run prettier on the changed file
```

Claude will set up the hook for you.

---

## Common Configurations

### Auto-Format on Edit

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### Lint Before Commit

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash(git commit *)",
        "hooks": [
          {
            "type": "command",
            "command": "npm run lint"
          }
        ]
      }
    ]
  }
}
```

### Allow Common Safe Commands

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)",
      "Bash(npm test)",
      "Bash(npm run *)",
      "Bash(ls *)",
      "Bash(cat *)"
    ]
  }
}
```

### Team Shared Settings

Commit `.claude/settings.json` to your repo so everyone on the team gets the same configuration:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Glob",
      "Grep",
      "Bash(npm test)",
      "Bash(npm run lint)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

Keep personal settings in `.claude/settings.local.json` (add to `.gitignore`).

---

## MCP Server Configuration

### Via Settings

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["./mcp-server.js"],
      "env": {
        "API_KEY": "..."
      }
    }
  }
}
```

### Via CLI

```bash
claude --mcp-config ./mcp.json
claude --strict-mcp-config --mcp-config ./mcp.json
```

### Interactive

```
/mcp
```

---

## Environment Variables

| Variable | What It Does |
|----------|-------------|
| `ANTHROPIC_API_KEY` | API key for authentication |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | Enable/disable prompt suggestions (`false` to disable) |
| `CLAUDE_MODEL` | Default model to use |

---

## Keybindings

Customize keyboard shortcuts in `~/.claude/keybindings.json`:

```
/keybindings
```

or

```
/keybindings-help
```

Then describe what you want to change.

---

## Tips

1. **Start with `/config`** to explore available settings visually
2. **Use `/update-config`** for complex changes — Claude sets it up for you
3. **Commit `.claude/settings.json`** for team-wide conventions
4. **Keep `.claude/settings.local.json`** for personal preferences (gitignore it)
5. **Use hooks for enforcement** — things that *must* happen, not just *should*
6. **Test hooks** with simple echo commands before adding real logic
7. **Use permission rules** to reduce approval fatigue for safe commands
