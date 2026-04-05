# Claude Code — Common Workflows

Step-by-step guides for the most common tasks you'll do with Claude Code.

---

## 1. Debugging a Bug

### When You Know Where the Bug Is

```
The login form crashes when email is empty. The error is in
src/components/LoginForm.tsx in the handleSubmit function.
Fix it.
```

Claude will:
1. Read the file
2. Identify the issue
3. Apply the fix
4. Explain what was wrong

### When You Don't Know Where the Bug Is

```
Users are reporting that their cart totals are wrong after applying
a discount code. Find and fix the issue.
```

Claude will:
1. Search for discount/cart-related code
2. Read relevant files
3. Trace the logic
4. Identify and fix the bug

### Using Error Logs

```
! cat /var/log/app/error.log | tail -50
```

Then:
```
Based on that error log, find and fix the root cause.
```

---

## 2. Building a New Feature

### Small Feature

```
Add a "copy to clipboard" button next to each code block
in the documentation page.
```

### Large Feature (Use Plan Mode First)

**Step 1 — Plan:**
```
/plan
Add user notifications system with:
- In-app notifications (bell icon with badge)
- Email notifications for important events
- User preferences to control which notifications they get
```

**Step 2 — Review the plan** Claude creates.

**Step 3 — Execute:**
Switch to a mode that allows edits (`Shift+Tab`) and tell Claude to proceed:
```
Go ahead with the plan
```

### Feature Following Existing Patterns

```
Create a new API endpoint for products following the same pattern
as the existing users endpoint in src/routes/users.ts
```

---

## 3. Code Review

### Review a File

```
Review src/services/payment.ts for security issues, bugs,
and performance problems.
```

### Review a Pull Request

```
! gh pr diff 123
```

Then:
```
Review these changes. Look for bugs, security issues,
and anything that doesn't follow our coding conventions.
```

### Review Your Own Changes

```
/diff
```

Then:
```
Review the changes I've made. Anything I missed?
```

---

## 4. Refactoring

### Simple Refactor

```
Rename the "userData" variable to "userProfile" across the
entire src/services/ directory.
```

### Pattern Refactor

```
All our API routes have inline validation. Refactor them to use
middleware validation like we have in the users route.
Start with the products route.
```

### Large Refactor (Use Sub-Agents)

```
Refactor the entire auth system from session-based to JWT tokens.
This affects routes, middleware, and the frontend auth context.
```

Claude will launch sub-agents to handle different parts in parallel.

---

## 5. Writing Tests

### Unit Tests

```
Write unit tests for the calculateShipping function in
src/utils/shipping.ts. Cover:
- Standard shipping
- Express shipping
- Free shipping threshold
- International addresses
- Invalid inputs
```

### Integration Tests

```
Write integration tests for the checkout API endpoint.
Test the full flow: add to cart → apply discount → checkout → payment.
```

### Tests for Existing Code

```
The UserService class has no tests. Write comprehensive tests
for all public methods. Use the test patterns from the existing
ProductService.test.ts file.
```

---

## 6. Understanding a Codebase

### High-Level Overview

```
I'm new to this project. Give me a high-level overview of:
- The project structure
- Key technologies used
- How the main features work
```

### Specific System

```
Explain how the authentication flow works, from the login form
to the JWT token refresh mechanism.
```

### Tracing a Request

```
Trace what happens when a user clicks "Place Order" — from the
frontend button click through the API to the database.
```

---

## 7. Git Workflow

### Create a Commit

```
/commit
```

Claude will:
1. Check `git status` and `git diff`
2. Draft a descriptive commit message
3. Stage and commit the changes

### Create a Pull Request

```
Create a PR for this branch. The changes add user notification preferences.
```

Claude will:
1. Analyze all commits on the branch
2. Draft a title and description
3. Push and create the PR via `gh`

### Fix a Failed CI

```
! gh run view --log-failed
```

Then:
```
The CI failed. Fix whatever is causing it.
```

---

## 8. Pair Programming

### Interactive Building

Work iteratively with Claude:

```
You: Let's build a search feature for the blog.
Claude: [creates initial implementation]
You: Good, but add debouncing to the search input.
Claude: [adds debouncing]
You: Now add highlighting of matched terms in results.
Claude: [adds highlighting]
```

### Rubber Duck Debugging

```
I'm stuck on this caching issue. The cache invalidation isn't working
but only in production. Let me walk you through what I've tried...
```

Claude will ask clarifying questions and help you think through the problem.

---

## 9. Documentation

### Generate Docs from Code

```
Generate API documentation for all endpoints in src/routes/.
Include request/response examples.
```

### Explain Complex Code

```
Add comments explaining the complex regex in src/utils/parser.ts.
Only comment the non-obvious parts.
```

---

## 10. DevOps & Infrastructure

### Dockerfile

```
Create a production Dockerfile for this Node.js app.
Use multi-stage build, non-root user, and minimal image.
```

### CI/CD Pipeline

```
Create a GitHub Actions workflow that:
- Runs tests on PR
- Builds and pushes Docker image on merge to main
- Deploys to staging automatically
```

### Debug Infrastructure Issues

```
! kubectl get pods -n production
! kubectl logs -n production deploy/api --tail=100
```

Then:
```
The API pods are crash-looping. Diagnose the issue from those logs.
```

---

## 11. Database Work

### Write Migrations

```
Create a migration that adds a "preferences" JSONB column to the
users table with a default value of an empty object.
```

### Optimize Queries

```
This query in src/repositories/orders.ts is slow (takes 3s).
Analyze it and suggest optimizations. The orders table has 5M rows.
```

---

## 12. Security Audit

### Full Audit

```
Do a security audit of the authentication and authorization code.
Check for:
- SQL injection
- XSS
- CSRF
- Broken access control
- Sensitive data exposure
```

### Specific Check

```
Check if any API endpoints are missing authentication middleware.
```

---

## Workflow Tips

1. **Start in plan mode** for big tasks — review the plan before executing
2. **Use `/diff`** after changes to verify what was modified
3. **Run tests frequently** — `! npm test` after changes
4. **Use `! git diff`** to see what's uncommitted
5. **Name sessions** for ongoing work — `/rename feature-notifications`
6. **Resume sessions** to continue work — `claude -r "feature-notifications"`
7. **Use sub-agents** for tasks that need heavy research
8. **Compact regularly** during long workflows — `/compact "focus on the auth changes"`
