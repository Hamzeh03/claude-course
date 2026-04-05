# Examples & Exercises

Practice using Claude Code with these real-world scenarios. Each example includes the prompt to use and what to expect.

---

## Beginner Exercises

### Exercise 1: Your First Conversation

**Goal:** Get comfortable with basic interaction.

**Try these prompts:**
```
What files are in this project?
```
```
Read the README.md and summarize it
```
```
What programming languages are used in this project?
```

**What you'll learn:** Claude uses `Glob`, `Read`, and `Grep` tools to explore your codebase.

---

### Exercise 2: Edit a File

**Goal:** Watch Claude make targeted edits.

**Setup:** Create a file first:
```
Create a file called hello.py with a function that prints "Hello World"
```

**Then try:**
```
Change hello.py to accept a name parameter and print "Hello, {name}!"
```

**What you'll learn:** Claude uses `Write` to create and `Edit` to modify files. Notice how edits show exact before/after changes.

---

### Exercise 3: Run Commands

**Goal:** Learn the difference between Claude running commands vs. you running them.

**Claude runs it (uses Bash tool):**
```
Run python hello.py
```

**You run it (bash mode):**
```
! python hello.py
```

**What you'll learn:** `!` prefix runs commands directly without Claude interpreting them. The output is added to context either way.

---

## Intermediate Exercises

### Exercise 4: Debug a Bug

**Setup:** Create a buggy file:
```
Create a file called calculator.py with add, subtract, multiply, and divide
functions. Intentionally make the divide function not handle division by zero.
```

**Then:**
```
Find and fix any bugs in calculator.py
```

**What you'll learn:** Claude reads code, identifies issues, and applies fixes — all while explaining what was wrong.

---

### Exercise 5: Write Tests

**Continuing from Exercise 4:**
```
Write unit tests for all functions in calculator.py using pytest.
Cover edge cases including division by zero, negative numbers, and floats.
```

**Then verify:**
```
! pip install pytest && pytest test_calculator.py -v
```

**What you'll learn:** Claude generates comprehensive tests and follows testing conventions.

---

### Exercise 6: Use Context Management

**Goal:** Learn to manage conversation context.

```
/context
```
See how much context you've used.

```
/compact focus on the calculator exercise
```
Compress conversation while keeping calculator work.

```
/cost
```
See token usage and costs.

**What you'll learn:** Context is finite — managing it well makes long sessions productive.

---

### Exercise 7: Use Memory

**Goal:** Save preferences across sessions.

```
Remember that I prefer pytest over unittest for Python testing
```

```
Remember that I like detailed inline comments in my code
```

Then start a new session and ask Claude to write tests — it should use pytest automatically.

**What you'll learn:** Memory persists across conversations so you don't repeat preferences.

---

## Advanced Exercises

### Exercise 8: Multi-File Project

**Goal:** Build something real across multiple files.

```
Create a simple REST API in Python using Flask with:
- GET /tasks - list all tasks
- POST /tasks - create a task
- PUT /tasks/<id> - update a task
- DELETE /tasks/<id> - delete a task
Use an in-memory list for storage. Include proper error handling.
```

**Then:**
```
Add input validation and write integration tests for all endpoints
```

**What you'll learn:** Claude handles multi-file projects, creates proper project structure, and maintains consistency across files.

---

### Exercise 9: Plan Before Building

**Goal:** Use plan mode for complex work.

```
/plan
Add a SQLite database to the Flask task API, replacing the in-memory
storage. Include migrations, a repository pattern, and connection pooling.
```

Review the plan, then:
```
[Switch to acceptEdits mode with Shift+Tab]
Go ahead with the plan
```

**What you'll learn:** Plan mode prevents premature coding. Review the approach before executing.

---

### Exercise 10: Code Review

**Goal:** Practice code review workflows.

```
Review the entire Flask task API for:
- Security vulnerabilities
- Performance issues
- Code quality
- Missing error handling
```

**What you'll learn:** Claude can audit code and find issues you might miss.

---

### Exercise 11: Refactoring

**Goal:** Safely transform existing code.

```
Refactor the Flask task API to use Blueprints. Separate routes,
models, and configuration into their own files.
```

**What you'll learn:** Claude handles structural refactoring across multiple files while maintaining functionality.

---

### Exercise 12: Use Sub-Agents

**Goal:** Leverage parallel processing.

```
I need three things done:
1. Add logging to the Flask API
2. Add a health check endpoint
3. Create a Dockerfile for the app

Do these in parallel.
```

**What you'll learn:** Claude launches sub-agents to handle independent tasks simultaneously, saving time.

---

## Pro Tips for Exercises

1. **Try variations** — Modify the prompts to see how different wording affects results
2. **Break things** — Introduce bugs on purpose and ask Claude to find them
3. **Compare approaches** — Ask Claude to solve the same problem two different ways
4. **Use `/diff`** after each exercise to review exactly what changed
5. **Use `/cost`** to understand token economics of different prompt styles
6. **Try different models** — `/model sonnet` vs `/model opus` for the same task
