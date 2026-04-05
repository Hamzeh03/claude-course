# Claude Code — Prompting Tips

How to write effective prompts that get the best results from Claude Code.

---

## The Golden Rule

**Be specific.** The more context and precision you give, the better the result.

| Vague (Worse) | Specific (Better) |
|---------------|-------------------|
| "Fix the bug" | "Fix the null pointer error in `auth.ts` when user has no email" |
| "Add a feature" | "Add a dark mode toggle to the settings page using our existing theme context" |
| "Clean up the code" | "Refactor `UserService` to separate validation logic into its own method" |
| "Write tests" | "Write unit tests for the `calculateTax` function covering edge cases: zero, negative, and over-limit amounts" |

---

## Prompt Patterns

### 1. The Direct Ask

Best for simple, clear tasks.

```
Add a loading spinner to the login button while the API call is in progress
```

```
Fix the off-by-one error in the pagination logic in api/routes.ts
```

### 2. The Context + Task

Best when Claude needs background to make good decisions.

```
We're migrating from REST to GraphQL. The users endpoint is already done.
Now migrate the products endpoint following the same pattern used in users.
```

```
This is a high-traffic service (10k req/s). Optimize the database query
in getActiveUsers() — it's currently doing a full table scan.
```

### 3. The Constraint-Based Ask

Best when you have specific requirements.

```
Add input validation to the signup form. Requirements:
- Email must be valid format
- Password must be 8+ chars with one number
- Show inline errors, not alerts
- Use the existing FormError component
```

### 4. The Exploration Ask

Best when you're not sure what's wrong.

```
The checkout flow is failing for some users but I can't reproduce it.
Look at the checkout code and find potential race conditions or edge cases.
```

```
I'm new to this codebase. Explain how the authentication flow works
from login to token refresh.
```

### 5. The Multi-Step Ask

Best for complex tasks. Let Claude plan and execute.

```
Add a caching layer to the API:
1. Use Redis for caching
2. Cache GET responses for 5 minutes
3. Invalidate cache on POST/PUT/DELETE
4. Add cache-control headers
```

### 6. The "Do It Like This" Ask

Best when you have a pattern to follow.

```
Add error handling to the products API route, following the same pattern
used in the users route at src/routes/users.ts
```

```
Create a new React component for ProductCard. Follow the same structure
and styling approach as the existing UserCard component.
```

---

## What Makes a Good Prompt

### Include

- **What** you want done (the task)
- **Where** it should happen (file, function, component)
- **Why** if it matters (helps Claude make judgment calls)
- **Constraints** if any (performance, compatibility, style)
- **Examples** of similar work in the codebase

### Skip

- Don't explain how Claude works to Claude
- Don't over-explain obvious things
- Don't include irrelevant context
- Don't ask Claude to confirm before every step (unless it's risky)

---

## Advanced Techniques

### Use Plan Mode for Big Tasks

Before a large change, ask Claude to plan first:

```
/plan
Redesign the notification system to support email, SMS, and push notifications
```

Claude will analyze the codebase and create an implementation plan without making changes. Review the plan, then switch to a mode that allows edits.

### Use the `!` Prefix for Quick Context

Run a command and add its output to the conversation:

```
! cat error.log | tail -20
```

Then ask:
```
Based on that error log, what's going wrong?
```

### Reference Specific Code

Point Claude to exact locations:

```
Look at the handleSubmit function in src/components/LoginForm.tsx around line 45.
It's not handling the case where the API returns a 429 status.
```

### Give Examples of Expected Output

```
Write a migration that adds a "role" column to the users table.
The output should look like our existing migration at migrations/002_add_email.sql
```

### Use Iterative Refinement

Start broad, then narrow down:

```
1. "Show me how the payment flow works"
2. "Now find where the discount calculation happens"  
3. "Fix the rounding error in that calculation"
```

---

## Prompts to Avoid

### Too Vague
```
Make it better
```
Better: "Improve the error messages in the form to be more user-friendly"

### Too Broad
```
Rewrite the entire backend
```
Better: "Refactor the auth module to use middleware pattern instead of inline checks"

### Too Controlling
```
First read file A, then read file B, then grep for X, then edit line 42 to change Y to Z
```
Better: "Change Y to Z in the auth handler" — let Claude figure out the steps

### Contradictory
```
Make it simple but handle every edge case with detailed error messages
```
Better: Pick your priority and be clear about trade-offs

---

## Prompt Templates

### Bug Fix
```
Bug: [describe the bug]
Steps to reproduce: [how to trigger it]
Expected: [what should happen]
Actual: [what happens instead]
Relevant file: [file path if known]
```

### New Feature
```
Add [feature name] to [where].
- It should [behavior 1]
- It should [behavior 2]
- Follow the pattern in [existing similar code]
- Use [specific technology/library] if applicable
```

### Code Review
```
Review [file/PR] for:
- Security issues
- Performance problems
- Code quality
- Missing edge cases
```

### Refactor
```
Refactor [what] in [where].
Goal: [why — better performance, readability, maintainability]
Constraint: [don't break X, keep backward compatibility, etc.]
```

### Explanation
```
Explain how [system/feature] works.
I'm familiar with [your background] but new to [what you don't know].
Focus on [specific aspect you care about].
```

---

## Tips for Long Sessions

1. **Start with context** — Tell Claude what you're working on at the start
2. **Use `/compact`** when context gets large — add focus instructions
3. **Use memory** — "Remember that we decided to use Redis for caching"
4. **Name your session** — `/rename payment-refactor` for easy resuming
5. **Break large tasks** into smaller conversations
6. **Use sub-agents** — For research tasks that would bloat your context
