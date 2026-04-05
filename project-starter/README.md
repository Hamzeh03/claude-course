# Project Starter Kit

> [Back to Course](../README.md)

**Ready-to-use files you copy into any project to configure Claude Code.**

These are not learning materials — they are **working config files** with placeholders you fill in. Copy the whole folder or just the files you need.

---

## What's Inside

| File | What It Does | Copy To |
|------|-------------|---------|
| `CLAUDE.md` | Project instructions Claude reads every session | Your project root |
| `IDENTITY.md` | Tell Claude who you are and how you work | Your project root (referenced from CLAUDE.md) |
| `GIT-RULES.md` | How Claude should handle git operations | Your project root (referenced from CLAUDE.md) |
| `settings.json` | Permissions, hooks, and automation | `.claude/settings.json` in your project |

---

## Quick Setup

### Option 1: Copy Everything

```bash
# From your project root:
cp /path/to/claude-course/project-starter/CLAUDE.md ./CLAUDE.md
cp /path/to/claude-course/project-starter/IDENTITY.md ./IDENTITY.md
cp /path/to/claude-course/project-starter/GIT-RULES.md ./GIT-RULES.md
mkdir -p .claude
cp /path/to/claude-course/project-starter/settings.json ./.claude/settings.json
```

### Option 2: Pick What You Need

- Just want project instructions? → Copy `CLAUDE.md`
- Want Claude to know your style? → Copy `CLAUDE.md` + `IDENTITY.md`
- Want git workflow control? → Copy `CLAUDE.md` + `GIT-RULES.md`
- Want permissions/hooks? → Copy `settings.json` to `.claude/`

---

## After Copying

1. Open each file and **fill in the `[PLACEHOLDERS]`** with your project's details
2. Delete any sections you don't need
3. Commit them to your project repo (except `settings.local.json`)

---

## Placeholder Guide

Throughout the files you'll see placeholders like:

- `[YOUR PROJECT NAME]` — Replace with your project's name
- `[YOUR ROLE]` — Replace with your role (e.g., "Senior Backend Engineer")
- `[CHOOSE ONE: ...]` — Pick the option that fits and delete the rest
- `<!-- DELETE IF NOT NEEDED -->` — Remove the entire section if it doesn't apply
