# Claude Code — Git & GitHub Guide

> [Back to README](../README.md) | [Commands](../COMMANDS.md) | [Workflows](WORKFLOWS.md) | [Settings](SETTINGS-CONFIG.md)

Everything about using Claude Code with Git and GitHub — from basic commits to PRs, branch management, and access setup.

---

## Table of Contents

- [Setup & Access](#setup--access)
- [Git Basics with Claude](#git-basics-with-claude)
- [Commits](#commits)
- [Branches](#branches)
- [Pull Requests](#pull-requests)
- [Code Review](#code-review)
- [GitHub Issues](#github-issues)
- [GitHub Actions & CI/CD](#github-actions--cicd)
- [Common Git Workflows](#common-git-workflows)
- [Permissions & Safety](#permissions--safety)
- [Troubleshooting](#troubleshooting)

---

## Setup & Access

### What Claude Needs to Work with Git/GitHub

Claude uses two things to interact with Git and GitHub:

| Tool | What It Does | How to Set Up |
|------|-------------|---------------|
| **git** | Local version control (commit, branch, diff, merge) | Usually pre-installed. Check: `git --version` |
| **gh** (GitHub CLI) | Remote GitHub operations (PRs, issues, reviews, releases) | Install + authenticate (see below) |

### Installing the GitHub CLI (`gh`)

**Option 1 — With package manager:**
```bash
# macOS
brew install gh

# Ubuntu/Debian
sudo apt install gh

# Windows
winget install GitHub.cli
```

**Option 2 — Without sudo (manual install):**
```bash
mkdir -p ~/bin
curl -sL https://github.com/cli/cli/releases/latest/download/gh_*_linux_amd64.tar.gz | tar xz -C /tmp
cp /tmp/gh_*/bin/gh ~/bin/gh
export PATH="$HOME/bin:$PATH"
# Add the export line to your ~/.bashrc or ~/.zshrc to make it permanent
```

### Authenticating `gh`

This is **required once** — it gives the `gh` CLI permission to access your GitHub account.

```bash
gh auth login
```

You'll be asked:
1. **Where?** → GitHub.com
2. **Protocol?** → SSH (recommended) or HTTPS
3. **Auth method?** → Login with a web browser (easiest)
4. Follow the browser prompt to authorize

**Verify it works:**
```bash
gh auth status
```

**Why this matters:** Without `gh auth login`, Claude cannot create PRs, view issues, or interact with GitHub's API. Git operations (commit, push, branch) work without it as long as SSH keys are set up.

### Setting Up SSH Keys for Git

If `git push` fails with "Permission denied":

```bash
# Check if you have an SSH key
ls ~/.ssh/id_ed25519.pub

# If not, create one
ssh-keygen -t ed25519 -C "your-email@example.com"

# Copy the public key
cat ~/.ssh/id_ed25519.pub
```

Then add it to GitHub: **GitHub → Settings → SSH and GPG keys → New SSH key** → paste the key.

**Test it:**
```bash
ssh -T git@github.com
# Should say: "Hi username! You've successfully authenticated"
```

### Setting Up Git Identity

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Claude will use this identity for all commits.

---

## Git Basics with Claude

### Let Claude Handle Git

You can ask Claude to do any git operation in natural language:

```
Show me what files have changed
```
```
What's the git history for this file?
```
```
Create a new branch called feature-auth
```
```
Stash my current changes
```

### Do It Yourself with `!`

Use bash mode for quick git commands:

```
! git status
! git log --oneline -10
! git diff
! git stash
```

The output is added to context so Claude can see it too.

---

## Commits

### Ask Claude to Commit

The simplest way:
```
Commit these changes
```

Claude will:
1. Run `git status` and `git diff` to see changes
2. Read recent commit messages to match the repo's style
3. Draft a descriptive commit message
4. Stage relevant files and commit

### Use the /commit Skill

```
/commit
```

Same as above but uses the specialized commit skill.

### Commit with Specific Message

```
Commit with message "fix: resolve null check in auth middleware"
```

### What Claude Does When Committing

| Step | What Happens |
|------|-------------|
| 1 | Runs `git status` to see untracked and modified files |
| 2 | Runs `git diff` to see staged and unstaged changes |
| 3 | Runs `git log` to see recent commit message style |
| 4 | Drafts a commit message based on the actual changes |
| 5 | Stages files by name (not `git add .` — avoids accidents) |
| 6 | Creates the commit |
| 7 | Runs `git status` to verify success |

### Commit Message Format

Claude writes commit messages like:

```
Add user notification preferences API endpoint

Implements GET/PUT /api/users/:id/preferences with validation
and database persistence. Includes unit tests.

Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>
```

The `Co-Authored-By` line is added automatically so you can track which commits Claude helped with.

### What Claude Won't Do (Safety)

- Won't use `git add .` or `git add -A` (might include secrets or large files)
- Won't commit `.env`, `credentials.json`, or similar files
- Won't amend commits unless you explicitly ask
- Won't commit without you asking first

---

## Branches

### Create a Branch

```
Create a new branch called feature-notifications
```

or:

```
! git checkout -b feature-notifications
```

### Switch Branches

```
Switch to the main branch
```

```
! git checkout main
```

### List Branches

```
! git branch -a
```

### Delete a Branch

```
Delete the feature-notifications branch
```

**Note:** Claude will ask for confirmation before deleting branches — this is a destructive operation.

### Merge a Branch

```
Merge feature-notifications into main
```

Claude will:
1. Check for conflicts
2. Perform the merge
3. Handle conflicts if possible, or ask you for guidance

### Git Worktrees (Isolated Work)

Claude can work in isolated git worktrees — separate copies of your repo:

```bash
claude -w feature-auth          # Start session in worktree
claude -w feature-auth --tmux   # Start in tmux session
```

Useful when you want Claude to work on something without touching your current branch.

---

## Pull Requests

### Create a PR

```
Create a pull request for this branch
```

Claude will:
1. Analyze all commits on the branch (not just the latest)
2. Check if the branch is pushed to remote
3. Push with `-u` flag if needed
4. Draft a title (under 70 chars) and detailed description
5. Create the PR using `gh pr create`

### Create a PR with Specific Details

```
Create a PR titled "Add auth middleware" targeting the develop branch
```

### PR Description Format

Claude generates PRs like:

```markdown
## Summary
- Added JWT authentication middleware
- Implemented token refresh endpoint
- Added unit tests for all auth flows

## Test plan
- [ ] Run `npm test` to verify all tests pass
- [ ] Test login flow manually
- [ ] Verify token refresh works after expiration
```

### View a PR

```
Show me PR #5
```

```
What are the changes in PR #12?
```

```
! gh pr view 5
```

### View PR Comments

```
Show me the comments on PR #1
```

Claude uses:
```
gh api repos/owner/repo/pulls/1/comments
```

### Check PR Status

```
! gh pr status
```

Shows all PRs — created by you, assigned to you, and needing your review.

### Merge a PR

```
Merge PR #1
```

Claude will ask for confirmation since this affects the remote repository.

**Merge strategies:**
```
Merge PR #1 with a squash merge
Merge PR #1 with a rebase merge
```

### Close a PR Without Merging

```
Close PR #3 without merging
```

---

## Code Review

### Review a PR

```
Review PR #5 for bugs, security issues, and code quality
```

Claude will:
1. Fetch the PR diff with `gh pr diff 5`
2. Analyze all changes
3. Report issues found

### Review the Current Diff

```
/diff
```

Opens an interactive diff viewer showing:
- Uncommitted changes
- Per-turn diffs (what Claude changed in each step)

### Review Before Pushing

```
Review all my uncommitted changes before I push
```

```
! git diff main...HEAD
```

Then:
```
Review these changes for any issues
```

---

## GitHub Issues

### View Issues

```
! gh issue list
! gh issue view 42
```

### Create an Issue

```
Create a GitHub issue titled "Login fails on mobile" with details about the bug
```

### Work on an Issue

```
Look at issue #15 and implement what it describes
```

Claude will:
1. Fetch the issue details
2. Understand the requirements
3. Implement the solution
4. Optionally create a PR that references the issue

### Close an Issue

```
! gh issue close 42
```

---

## GitHub Actions & CI/CD

### Check CI Status

```
! gh run list
```

### View Failed CI

```
! gh run view --log-failed
```

Then:
```
The CI failed. Fix whatever is causing it.
```

### Create a Workflow

```
Create a GitHub Actions workflow that runs tests on every PR
and deploys to staging on merge to main
```

### Install Claude GitHub App

```
/install-github-app
```

This sets up Claude as a GitHub Actions bot that can:
- Respond to PR comments
- Auto-review code
- Run automated tasks on triggers

---

## Common Git Workflows

### Workflow 1: Feature Branch

```
1. Create a branch:        "Create branch feature-search"
2. Make changes:            "Add search functionality to the API"
3. Commit:                  "Commit these changes"
4. Push:                    "Push this branch"
5. Create PR:               "Create a PR for this branch"
```

### Workflow 2: Bug Fix

```
1. Create branch:           "Create branch fix-login-crash"
2. Fix the bug:             "Fix the null check error in login"
3. Run tests:               "! npm test"
4. Commit:                  "Commit with message 'fix: handle null email in login'"
5. Push and PR:             "Push and create a PR"
```

### Workflow 3: Quick Fix on Main

```
1. Make the fix:            "Fix the typo in the README"
2. Commit:                  "Commit this change"
3. Push:                    "Push to main"
```

### Workflow 4: Resolve Merge Conflicts

```
! git merge feature-branch
```

If conflicts arise:
```
Resolve the merge conflicts, keeping our version of the auth logic
but accepting their UI changes
```

### Workflow 5: Revert a Change

```
Revert the last commit
```

```
! git revert HEAD
```

### Workflow 6: Cherry-Pick

```
Cherry-pick commit abc123 into the release branch
```

---

## Permissions & Safety

### What Claude Will Always Ask Before Doing

| Action | Why It's Risky |
|--------|---------------|
| `git push` | Sends code to remote — visible to others |
| `git push --force` / `git push -f` | **Overwrites remote history** — can destroy work |
| `git reset --hard` | Discards all local changes permanently |
| `git branch -D` / `git branch -d -f` | Deletes a branch permanently (force delete) |
| `git checkout .` / `git restore .` | Discards all uncommitted changes |
| `git clean -f` | Deletes untracked files permanently |
| Creating/closing PRs | Visible to collaborators |
| Merging PRs | Changes the main codebase |

### What Claude Will Do Automatically (if permitted)

| Action | Risk Level |
|--------|-----------|
| `git status` | None — read-only |
| `git diff` | None — read-only |
| `git log` | None — read-only |
| `git branch` (list) | None — read-only |
| `git stash` | Low — reversible |
| `git add` (specific files) | Low — staging only |
| `git commit` | Low — local only |

### Setting Up Git Permissions

In `/permissions` or `.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(git log *)",
      "Bash(git branch)",
      "Bash(git stash *)",
      "Bash(git add *)",
      "Bash(git commit *)"
    ],
    "deny": [
      "Bash(git push --force *)",
      "Bash(git push -f *)",
      "Bash(git reset --hard *)",
      "Bash(git branch -D *)",
      "Bash(git branch -d -f *)",
      "Bash(git checkout .)",
      "Bash(git restore .)",
      "Bash(git clean *)"
    ]
  }
}
```

This lets Claude do safe git operations without asking, while blocking destructive ones.

### The Co-Authored-By Line

Every commit Claude creates includes:

```
Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>
```

This gives you a clear audit trail of which commits had AI assistance.

---

## Troubleshooting

### "Permission denied" on push

**Cause:** SSH key not set up or not added to GitHub.

```bash
# Check SSH key
ssh -T git@github.com

# If it fails, generate a key
ssh-keygen -t ed25519 -C "your-email@example.com"

# Add to GitHub: Settings → SSH keys → paste contents of:
cat ~/.ssh/id_ed25519.pub
```

### "gh: command not found"

**Cause:** GitHub CLI not installed.

```bash
# Install (see Setup section above)
brew install gh        # macOS
sudo apt install gh    # Ubuntu
```

### "gh: Not logged in"

**Cause:** GitHub CLI not authenticated.

```bash
gh auth login
# Follow the prompts
```

### "fatal: not a git repository"

**Cause:** You're not in a git repo.

```bash
git init                    # Initialize a new repo
# OR
git clone <url>             # Clone an existing repo
```

### "error: failed to push some refs"

**Cause:** Remote has changes you don't have locally.

```bash
git pull --rebase origin main
# Then push again
```

Or ask Claude:
```
My push failed because the remote has new changes. Help me resolve this.
```

### "merge conflict"

**Cause:** Two branches changed the same lines.

```
Help me resolve these merge conflicts
```

Claude will:
1. Read the conflicting files
2. Understand both sides
3. Resolve conflicts based on your guidance
4. Stage and commit the resolution

### PR creation fails

**Possible causes:**
- Not authenticated: `gh auth login`
- No upstream branch: Claude will push with `-u` automatically
- No commits: Need to commit first
- Same content as base: Nothing to PR

---

## Quick Reference

| Task | What to Say |
|------|-------------|
| Check status | `! git status` |
| See changes | `! git diff` or `/diff` |
| See history | `! git log --oneline -10` |
| Create branch | `Create branch feature-x` |
| Switch branch | `Switch to main` |
| Stage + commit | `Commit these changes` |
| Push | `Push this branch` |
| Create PR | `Create a PR for this branch` |
| View PR | `Show me PR #5` |
| Merge PR | `Merge PR #5` |
| Fix CI | `! gh run view --log-failed` then `Fix the CI` |
| View issues | `! gh issue list` |
| Resolve conflicts | `Help me resolve these merge conflicts` |
| Revert | `Revert the last commit` |
| Stash | `! git stash` |
| Unstash | `! git stash pop` |
