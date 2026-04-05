# Claude Code — Memory System

Claude Code has a persistent, file-based memory that carries knowledge between conversations.

---

## Why Memory Matters

Without memory, every conversation starts from zero. Memory lets Claude:
- Remember who you are and how you work
- Recall your preferences and past corrections
- Track ongoing project context
- Know where to find external resources

---

## Memory Types

### User Memory

**What it stores:** Your role, goals, expertise, preferences.

**Why it matters:** Claude collaborates differently with a senior engineer vs. a student. Knowing who you are lets Claude tailor its responses.

**Examples:**
- "User is a data scientist focused on observability/logging"
- "Deep Go expertise, new to React — frame frontend explanations in backend terms"
- "Prefers concise responses with no trailing summaries"

**How to save:**
```
Remember that I'm a backend engineer who mainly works in Python and Go
```

---

### Feedback Memory

**What it stores:** How you want Claude to approach work — what to avoid and what to keep doing.

**Why it matters:** This is how Claude learns from corrections AND successes. Without this, you'd repeat the same guidance every session.

**Examples:**
- "Don't mock the database in tests — use real DB. Why: mocked tests hid a broken migration"
- "Prefer one bundled PR for refactors. Why: splitting would be churn in this area"
- "Stop summarizing at the end of responses — user reads the diff"

**How to save:**
Claude automatically considers saving feedback when you correct it. You can also be explicit:
```
Remember: always run the linter before committing
```

---

### Project Memory

**What it stores:** Ongoing work context, goals, decisions, deadlines.

**Why it matters:** Projects have context that isn't in the code — why decisions were made, what's blocked, who's working on what.

**Examples:**
- "Merge freeze begins 2026-03-05 for mobile release cut"
- "Auth middleware rewrite is driven by legal compliance, not tech debt"
- "API v2 migration must be backward compatible until Q3"

**How to save:**
```
Remember that we're freezing merges after April 10th for the release
```

---

### Reference Memory

**What it stores:** Pointers to where information lives in external systems.

**Why it matters:** Claude can't always access external systems, but knowing where to look helps it guide you.

**Examples:**
- "Pipeline bugs tracked in Linear project 'INGEST'"
- "Oncall latency dashboard: grafana.internal/d/api-latency"
- "Design specs are in Figma project 'App Redesign 2026'"

**How to save:**
```
Remember that our bug tracker is in Linear project "BACKEND"
```

---

## How Memory Is Stored

### File Structure

```
~/.claude/projects/<project-path>/memory/
├── MEMORY.md              # Index file (always loaded)
├── user_role.md           # Individual memory file
├── feedback_testing.md    # Individual memory file
├── project_freeze.md      # Individual memory file
└── reference_linear.md    # Individual memory file
```

### Memory File Format

Each memory is a Markdown file with frontmatter:

```markdown
---
name: Testing Approach
description: User prefers integration tests over mocked tests
type: feedback
---

Don't mock the database in tests — use real DB connections.

**Why:** Prior incident where mocked tests passed but prod migration failed.

**How to apply:** When writing tests that interact with data stores, always use the real database. Set up test fixtures with actual data.
```

### MEMORY.md Index

The index is one line per memory, under 150 characters each:

```markdown
- [User Role](user_role.md) — Backend engineer, Python/Go, prefers concise responses
- [Testing Approach](feedback_testing.md) — Use real DB in tests, no mocks
- [Release Freeze](project_freeze.md) — Merge freeze April 10-17 for mobile release
- [Bug Tracker](reference_linear.md) — Linear project "BACKEND" for pipeline bugs
```

MEMORY.md is always loaded into context. Keep it under 200 lines.

---

## Managing Memory

### Save a Memory

Just tell Claude:
```
Remember that I prefer tabs over spaces
Remember that our CI runs on GitHub Actions
Remember that the deploy pipeline is documented in Notion
```

### Forget a Memory

```
Forget what you know about my testing preferences
```

### View Memories

```
/memory
```
Opens the memory editor where you can view and edit all memory files.

### What NOT to Save

Memory should store things that **can't be derived from the code**:

| Don't Save | Why | Use Instead |
|------------|-----|-------------|
| Code patterns and conventions | Visible in the code | CLAUDE.md |
| Git history and recent changes | Available via `git log` | Git commands |
| File paths and project structure | Discoverable via tools | Glob/Grep |
| Debugging solutions | The fix is in the code | Commit messages |
| Current task progress | Session-scoped | Tasks |
| Things already in CLAUDE.md | Redundant | CLAUDE.md |

---

## Memory Lifecycle

```
Something worth remembering happens
        │
        ▼
Is it already in memory? ──Yes──▶ Update existing memory
        │
        No
        │
        ▼
Can it be derived from code/git? ──Yes──▶ Don't save
        │
        No
        │
        ▼
Will it be useful in future sessions? ──No──▶ Don't save
        │
        Yes
        │
        ▼
Choose memory type (user/feedback/project/reference)
        │
        ▼
Write memory file with frontmatter
        │
        ▼
Add pointer to MEMORY.md index
```

---

## Important Notes

1. **Memories can go stale** — Claude verifies memories against current code/state before acting on them
2. **Memory ≠ CLAUDE.md** — CLAUDE.md is for project instructions; memory is for cross-session knowledge
3. **Memory ≠ Tasks** — Tasks track current-session progress; memory persists across sessions
4. **Keep MEMORY.md under 200 lines** — it's loaded every conversation
5. **Be specific in descriptions** — the description field determines when a memory gets loaded
6. **Relative dates are converted** — "next Thursday" becomes "2026-04-09" so it still makes sense later
