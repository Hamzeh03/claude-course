# Claude Code — Complete Commands Reference

A comprehensive guide to every command, shortcut, and flag available in Claude Code.

> [Back to README](README.md) | [Cheat Sheet](CHEATSHEET.md) | [Skills](guides/SKILLS.md) | [Workflows](guides/WORKFLOWS.md)

---

## Table of Contents

- [Slash Commands](#slash-commands)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Special Syntax](#special-syntax)
- [CLI Flags](#cli-flags)
- [Vim Mode](#vim-mode)

---

## Slash Commands

### Session & Navigation

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/help` | Shows help and available commands | When you're lost or need to discover features |
| `/clear` or `/reset` or `/new` | Clears conversation history | When you want a fresh start without leaving the session |
| `/exit` or `/quit` | Exits Claude Code | When you're done working |
| `/resume [session]` or `/continue` | Resume a previous conversation by ID or name | When you want to pick up where you left off |
| `/branch [name]` or `/fork` | Create a branch of the current conversation | When you want to explore an alternative approach without losing current progress |
| `/rewind` or `/checkpoint` | Rewind conversation/code to a previous point | When Claude made changes you want to undo |
| `/rename [name]` | Rename the current session | To organize and label sessions for easy recall |

### Context & Cost Management

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/compact [instructions]` | Compresses conversation to save context space | When conversation is getting long and context is running low |
| `/context` | Visualize context usage as a colored grid | To understand what's consuming your context window |
| `/cost` | Shows token usage and cost statistics | To track how much you're spending on API calls |
| `/diff` | Opens interactive diff viewer | To review all changes Claude has made (uncommitted and per-turn) |
| `/copy [N]` | Copy last (or Nth) assistant response to clipboard | To save a response; press `w` in picker to write to file |
| `/export [filename]` | Export conversation as plain text | To save the full conversation for reference |

### Model & Mode

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/model [model]` | Change the AI model mid-session | To switch between Opus, Sonnet, Haiku without restarting |
| `/fast [on\|off]` | Toggle fast mode | When you want faster (but same model) responses |
| `/effort [low\|medium\|high\|max\|auto]` | Set effort level | To balance quality vs. speed/cost |
| `/plan [description]` | Enter plan mode | When you want Claude to analyze and plan before making edits |

### Configuration

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/config` or `/settings` | Open settings interface | To change theme, model, output style, preferences |
| `/permissions` or `/allowed-tools` | Manage tool permission rules | To control what Claude can do automatically vs. ask first |
| `/theme` | Change color theme | To customize appearance (light, dark, colorblind, ANSI) |
| `/color [color]` | Set prompt bar color for session | To visually distinguish different sessions |
| `/keybindings` | Open keybindings config file | To customize keyboard shortcuts |
| `/terminal-setup` | Configure terminal keybindings | To set up Shift+Enter and other terminal-specific shortcuts |
| `/statusline` | Configure status line display | To show custom info in the status bar |

### Memory & Project

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/memory` | Edit CLAUDE.md memory files | To manage persistent project instructions and preferences |
| `/init` | Initialize project with CLAUDE.md | When starting a new project and want Claude to understand it |
| `/insights` | Generate report analyzing your sessions | To review work patterns and identify friction points |

### Authentication & Account

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/login` | Sign in to Anthropic account | To authenticate Claude Code |
| `/logout` | Sign out | To log out cleanly |
| `/usage` | Show plan usage limits and rate limit status | To check how much of your subscription you've used |
| `/extra-usage` | Configure extra usage when rate limited | To keep working when you hit limits |
| `/upgrade` | Open upgrade page | To switch plan tiers (Pro/Max) |

### Tools & Integrations

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/mcp` | Manage MCP server connections | To connect external tools and services |
| `/ide` | Manage IDE integrations | To connect VS Code, JetBrains, etc. |
| `/chrome` | Configure Chrome browser integration | To set up web automation |
| `/install-github-app` | Set up Claude GitHub Actions app | To enable CI/CD integration with GitHub |
| `/install-slack-app` | Install Claude Slack app | To enable Slack integration |
| `/plugin` | Manage plugins | To install, enable, disable, or update plugins |
| `/reload-plugins` | Reload all active plugins | To apply plugin changes without restarting |
| `/hooks` | View hook configurations | To see what automations are set up for tool events |

### Tasks & Automation

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/tasks` or `/bashes` | List and manage background tasks | To track long-running operations |
| `/loop <interval> <command>` | Run a command on a recurring interval | To automate repetitive checks (e.g., `/loop 5m /foo`) |
| `/schedule [description]` | Create/manage scheduled remote agents | To set up recurring cron-based automated tasks |
| `/skills` | List available skills | To see what custom slash commands are available |

### Advanced & Remote

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/desktop` or `/app` | Continue session in desktop app | To move from CLI to GUI (macOS/Windows) |
| `/remote <description>` | Create new web session on claude.ai | To start a cloud-based session |
| `/remote-control` or `/rc` | Make session available for remote control | To control from claude.ai web interface |
| `/ultraplan <prompt>` | Draft plan, review in browser, execute remotely | For complex multi-step planning across agents |
| `/agents` | Manage agent/subagent configurations | To set up and configure subagents |
| `/add-dir <path>` | Add working directory for file access | To grant access to files outside current directory |

### Informational

| Command | What It Does | When to Use |
|---------|-------------|-------------|
| `/doctor` | Diagnose and verify installation | When something seems broken or misconfigured |
| `/release-notes` | View changelog | To see what's new in recent versions |
| `/feedback` or `/bug` | Submit feedback | To report bugs or request features |
| `/powerup` | Interactive feature lessons | To learn Claude Code capabilities through demos |
| `/btw <question>` | Quick side question (no context cost) | To ask something without cluttering the conversation |
| `/voice` | Toggle push-to-talk voice input | To use voice dictation |

---

## Keyboard Shortcuts

### Essential Controls

| Shortcut | What It Does | When to Use |
|----------|-------------|-------------|
| `Ctrl+C` | Cancel current input or generation | To stop Claude mid-response |
| `Ctrl+D` | Exit Claude Code | To end the session |
| `Esc` + `Esc` | Rewind / summarize | To undo changes or restore a previous state |
| `Shift+Tab` or `Alt+M` | Cycle permission modes | To switch between default/acceptEdits/plan/auto modes |

### Model & Mode Switching

| Shortcut | What It Does |
|----------|-------------|
| `Alt+P` (or `Option+P` on Mac) | Switch model |
| `Alt+T` (or `Option+T` on Mac) | Toggle extended thinking |
| `Alt+O` (or `Option+O` on Mac) | Toggle fast mode |

### Navigation & Editing

| Shortcut | What It Does |
|----------|-------------|
| `Ctrl+G` or `Ctrl+X Ctrl+E` | Open prompt in external text editor |
| `Ctrl+R` | Reverse search command history |
| `Ctrl+L` | Redraw the screen |
| `Ctrl+O` | Toggle verbose output (transcript viewer) |
| `Up/Down` arrows | Navigate command history |
| `Alt+B` / `Alt+F` | Move cursor back/forward one word |

### Text Manipulation

| Shortcut | What It Does |
|----------|-------------|
| `Ctrl+K` | Delete to end of line |
| `Ctrl+U` | Delete from cursor to line start |
| `Ctrl+Y` | Paste deleted text |
| `Alt+Y` (after Ctrl+Y) | Cycle through paste history |

### Task Management

| Shortcut | What It Does |
|----------|-------------|
| `Ctrl+B` | Background the current running task |
| `Ctrl+T` | Toggle task list visibility |
| `Ctrl+X Ctrl+K` (twice) | Kill all background agents |

### Image & Clipboard

| Shortcut | What It Does |
|----------|-------------|
| `Ctrl+V` / `Cmd+V` (Mac) / `Alt+V` (Windows) | Paste image from clipboard |

### Multiline Input

| Method | Shortcut |
|--------|----------|
| Quick escape | `\` + `Enter` (works everywhere) |
| macOS default | `Option+Enter` |
| Standard | `Shift+Enter` (iTerm2, WezTerm, Ghostty, Kitty) |
| Line feed | `Ctrl+J` |
| Paste mode | Just paste multi-line text directly |

---

## Special Syntax

### Prefix Commands

| Prefix | What It Does | Example |
|--------|-------------|---------|
| `/` | Run a slash command or skill | `/help`, `/model sonnet` |
| `!` | Run a shell command directly (bash mode) | `! npm test`, `! git status` |
| `@` | File path autocomplete | Type `@` then start typing a filename |

### Bash Mode (`!`)

When you prefix with `!`, the command runs directly in your shell:
- Output is added to the conversation context
- Claude does not interpret or approve the command
- Supports Tab autocomplete from previous `!` commands
- Exit bash mode with `Escape`, `Backspace`, or `Ctrl+U` on empty prompt

### Side Questions (`/btw`)

- Ask a quick question without adding to conversation history
- No tool access — answers from context only
- Single response, no follow-ups
- Low cost (reuses prompt cache)

---

## CLI Flags

### Starting a Session

```bash
claude                              # Interactive session
claude "explain this code"          # Interactive with initial prompt
claude -p "explain this"            # Non-interactive (print mode), then exit
claude -c                           # Continue most recent conversation
claude -r "session-name"            # Resume a specific session
claude -n "my-feature"              # Name the session
claude -w feature-auth              # Start in isolated git worktree
```

### Model & Effort

```bash
claude --model opus                 # Use Opus model
claude --model sonnet               # Use Sonnet model
claude --effort high                # Set effort level
claude --effort max                 # Maximum effort (session-scoped)
claude -p --fallback-model sonnet   # Fallback model if primary overloaded
```

### Permissions & Tools

```bash
claude --permission-mode plan                # Start in plan mode
claude --permission-mode auto                # Auto-approve everything
claude --dangerously-skip-permissions        # Skip all permission prompts
claude --tools "Bash,Edit,Read"              # Restrict available tools
claude --allowedTools "Bash(git *)" "Read"   # Auto-allow specific tools
claude --disallowedTools "Bash(git log *)"   # Block specific tools
```

### Input & Output

```bash
claude -p --output-format json       # Get JSON output
claude -p --output-format stream-json # Get streaming JSON
claude -p --input-format stream-json  # Accept streaming JSON input
cat file.py | claude -p "review this" # Pipe input to Claude
```

### System Prompt

```bash
claude --system-prompt "You are a Python expert"
claude --system-prompt-file ./prompt.txt
claude --append-system-prompt "Always use TypeScript"
claude --append-system-prompt-file ./rules.txt
```

### Limits & Budget

```bash
claude -p --max-turns 3              # Limit agentic turns
claude -p --max-budget-usd 5.00     # Max spend before stopping
claude -p --json-schema '{...}'      # Get validated JSON matching schema
```

### Remote & Cloud

```bash
claude --remote "Fix login bug"      # Start cloud session
claude --teleport                    # Resume web session locally
claude --remote-control              # Enable remote control from claude.ai
```

### Advanced

```bash
claude --bare -p "query"             # Minimal mode (skip hooks, plugins, MCP, memory)
claude --verbose                     # Verbose logging
claude --debug "api,mcp"             # Debug mode with filtering
claude --init                        # Run init hooks then start
claude --init-only                   # Run init hooks then exit
```

---

## Vim Mode

Enable via `/config` → Editor mode.

### Mode Switching

| Key | Action |
|-----|--------|
| `Esc` | Enter NORMAL mode |
| `i` | Insert before cursor |
| `I` | Insert at beginning of line |
| `a` | Insert after cursor |
| `A` | Insert at end of line |
| `o` / `O` | Open line below / above |

### Navigation (NORMAL)

| Key | Action |
|-----|--------|
| `h/j/k/l` | Left / Down / Up / Right |
| `w` / `b` / `e` | Next word / Previous word / End of word |
| `0` / `$` / `^` | Beginning / End / First non-blank |
| `gg` / `G` | Beginning / End of input |
| `f{char}` / `F{char}` | Jump to / back to character |
| `t{char}` / `T{char}` | Jump to before / after character |

### Editing (NORMAL)

| Key | Action |
|-----|--------|
| `x` | Delete character |
| `dd` / `D` | Delete line / Delete to end |
| `dw` / `db` | Delete word forward / backward |
| `cc` / `C` | Change line / Change to end |
| `cw` / `cb` | Change word forward / backward |
| `yy` / `Y` | Yank (copy) line |
| `p` / `P` | Paste after / before cursor |
| `>>` / `<<` | Indent / Dedent line |
| `J` | Join lines |
| `.` | Repeat last change |

### Text Objects

| Key | Action |
|-----|--------|
| `iw` / `aw` | Inner / around word |
| `i"` / `a"` | Inner / around double quotes |
| `i(` / `a(` | Inner / around parentheses |
| `i{` / `a{` | Inner / around braces |
| `i[` / `a[` | Inner / around brackets |

---

## Platform Notes

### macOS
- Set Option as Meta key in your terminal for Alt shortcuts to work
- **iTerm2**: Settings → Profiles → Keys → Left Option = "Esc+"
- **Terminal.app**: Settings → Profiles → Keyboard → "Use Option as Meta Key"
- **VS Code**: `"terminal.integrated.macOptionIsMeta": true`

### Windows
- Use `Alt` instead of `Option`
- Run `/terminal-setup` for Shift+Enter support

### Linux
- Shift+Enter works out of box in Kitty, Ghostty, WezTerm
- Run `/terminal-setup` for Alacritty, Zed, Warp
