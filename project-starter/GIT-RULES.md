# Git & GitHub Rules

<!-- Claude reads this to know how you want git operations handled in this project. -->
<!-- Fill in the sections that apply. Delete the rest. -->

## Repository Info

- **Remote:** [e.g., git@github.com:username/repo.git]
- **Default branch:** [CHOOSE: main | master | develop]
- **PR target branch:** [CHOOSE: main | develop | staging]
<!-- DELETE IF NOT NEEDED -->
- **Protected branches:** [e.g., main, production — never force push]
- **Branch naming:** [e.g., feature/*, fix/*, chore/*, release/*]

## Commit Rules

### Commit Message Style

<!-- CHOOSE ONE style, delete the rest -->

**Conventional Commits:**
```
type(scope): description

Types: feat, fix, docs, style, refactor, test, chore, perf, ci, build
Example: feat(auth): add JWT token refresh endpoint
```

**Simple descriptive:**
```
Add/Fix/Update/Remove [what] in [where]
Example: Fix null check in auth middleware
```

**Ticket-based:**
```
[TICKET-123] Description of change
Example: [AUTH-45] Add password reset flow
```

### Commit Preferences

<!-- CHOOSE the ones that fit, delete the rest -->

- Make small, focused commits — one logical change per commit
- Bundle related changes into one commit
- Always include `Co-Authored-By` line when Claude helps
- Don't include `Co-Authored-By` — I don't need attribution tracking
- Run [LINTER/FORMATTER COMMAND] before committing
- Run tests before committing
- Never commit without me explicitly asking

### Files to Never Commit

<!-- Add paths that should never be committed -->

- `.env`, `.env.local`, `.env.production`
- `credentials.json`, `secrets.yaml`, `*.key`, `*.pem`
- `node_modules/`, `__pycache__/`, `.venv/`
- [Add your own...]

## Branch Rules

### Creating Branches

<!-- CHOOSE ONE -->

- Always create a feature branch — never commit directly to [DEFAULT BRANCH]
- Small fixes can go directly to [DEFAULT BRANCH]
- Always branch from [CHOOSE: main | develop]

### Branch Naming Convention

<!-- CHOOSE ONE or customize -->

```
feature/short-description     # New features
fix/short-description          # Bug fixes
chore/short-description        # Maintenance
docs/short-description         # Documentation
refactor/short-description     # Code restructuring
```

## Pull Request Rules

### When to Create a PR

<!-- CHOOSE the ones that apply -->

- Always create a PR for any change (even small ones)
- Create PRs for features and fixes, skip for docs/typos
- Only create a PR when I ask for one

### PR Format

<!-- CHOOSE ONE or customize -->

**Standard:**
```markdown
## Summary
[2-3 bullet points of what changed and why]

## Test Plan
- [ ] Tests pass
- [ ] Manual testing done
- [ ] [Other checks...]
```

**Minimal:**
```markdown
[One paragraph explaining the change]
```

**Detailed:**
```markdown
## Summary
[What changed]

## Motivation
[Why this change was needed]

## Changes
[List of specific changes]

## Test Plan
[How to verify]

## Screenshots
[If UI change]
```

### PR Preferences

<!-- CHOOSE the ones that fit, delete the rest -->

- Keep PRs small and focused — split large changes into multiple PRs
- Bundle related changes into one PR to reduce review overhead
- Always request review from [REVIEWER USERNAME / TEAM]
- Auto-merge if CI passes — don't wait for review
- Never merge without my explicit approval
- Use [CHOOSE: merge commit | squash merge | rebase merge]
- Delete branch after merging

## Push Rules

### What to Push

<!-- CHOOSE the ones that fit, delete the rest -->

- Always push to a feature branch, never directly to [DEFAULT BRANCH]
- Ask me before pushing — I want to review first
- Push automatically after committing
- Never force push to shared branches

### Before Pushing

<!-- CHOOSE the ones that fit, delete the rest -->

- Run `[TEST COMMAND]` and verify all tests pass
- Run `[LINT COMMAND]` and fix any issues
- Check `git diff` one more time
- Nothing — just push it

## CI/CD

<!-- DELETE this section if you don't have CI/CD -->

- **CI runs on:** [e.g., GitHub Actions, GitLab CI, CircleCI]
- **Required checks:** [e.g., tests, lint, build, type-check]
- **If CI fails:** Fix the issue and push again (don't skip checks)
<!-- DELETE IF NOT NEEDED -->
- **Deploy trigger:** [e.g., merge to main auto-deploys to production]
- **Staging:** [e.g., merge to develop deploys to staging]

## Safety Rules

### Claude Must NEVER Do Without Asking

- `git push --force` (overwrites remote history)
- `git reset --hard` (destroys local changes)
- `git branch -D` (deletes branch permanently)
- `git checkout .` or `git restore .` (discards all changes)
- `git clean -f` (deletes untracked files)
- Merge or close PRs
- Push to protected branches

### Claude CAN Do Without Asking

<!-- Customize this list based on your comfort level -->

- `git status`, `git diff`, `git log` (read-only)
- `git add` specific files
- `git commit` (local only)
- `git stash` / `git stash pop`
- `git branch` (list branches)
- `git checkout -b` (create new branch)
- `git fetch`

## Merge Conflict Resolution

<!-- CHOOSE ONE -->

- Ask me how to resolve conflicts — don't decide on your own
- Resolve automatically, but show me the resolution before committing
- Resolve automatically using the simpler/more readable version
- Always prefer [CHOOSE: our changes | their changes | newest code]
