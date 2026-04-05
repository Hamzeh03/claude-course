# [YOUR PROJECT NAME]

## Overview
[One or two sentences: what does this project do and why does it exist?]

## Tech Stack
- **Language:** [e.g., TypeScript, Python, Go, Rust]
- **Framework:** [e.g., React, Next.js, FastAPI, Express, Django]
- **Database:** [e.g., PostgreSQL, MongoDB, SQLite, Redis]
- **Infra:** [e.g., Docker, Kubernetes, AWS, Vercel]
<!-- DELETE IF NOT NEEDED -->
- **Testing:** [e.g., Jest, pytest, Go testing, Vitest]
- **CI/CD:** [e.g., GitHub Actions, GitLab CI, CircleCI]

## Build & Run

```bash
# Install dependencies
[e.g., npm install | pip install -r requirements.txt | go mod download]

# Run development server
[e.g., npm run dev | python manage.py runserver | go run ./cmd/server]

# Build for production
[e.g., npm run build | docker build -t app . | go build -o bin/app]
```

## Test

```bash
# Run all tests
[e.g., npm test | pytest | go test ./...]

# Run a single test file
[e.g., npm test -- path/to/test | pytest path/to/test.py -v]

# Run with coverage
[e.g., npm run test:coverage | pytest --cov | go test -cover ./...]
```

## Lint & Format

```bash
# Lint
[e.g., npm run lint | ruff check . | golangci-lint run]

# Format
[e.g., npm run format | ruff format . | gofmt -w .]
```

## Project Structure

```
[YOUR PROJECT STRUCTURE — example below, edit to match yours]
src/
├── components/    # UI components
├── routes/        # API routes / pages
├── services/      # Business logic
├── utils/         # Shared utilities
├── types/         # Type definitions
└── config/        # Configuration
```

## Coding Conventions

<!-- Keep the ones that apply, delete the rest, add your own -->

- [e.g., Use functional components with hooks, not class components]
- [e.g., All API responses follow { data, error, status } pattern]
- [e.g., Use snake_case for Python, camelCase for TypeScript]
- [e.g., Errors should be user-friendly messages, not raw stack traces]
- [e.g., All database access goes through the repository/service layer]
- [e.g., No inline styles — use CSS modules / Tailwind / styled-components]
- [e.g., Prefer composition over inheritance]

## Architecture Decisions

<!-- Things Claude needs to know about WHY you built it this way -->

- [e.g., Auth uses JWT tokens stored in httpOnly cookies, not localStorage]
- [e.g., We use a monorepo with shared packages in /packages]
- [e.g., Caching is at the service layer with Redis, TTL = 5 min]
- [e.g., Feature flags managed through LaunchDarkly / environment variables]

## Important Rules

<!-- Things Claude must NEVER do or must ALWAYS do in this project -->

- [e.g., Never commit .env files — use .env.example as template]
- [e.g., Database migrations must be backward compatible]
- [e.g., All API endpoints require authentication except /health and /login]
- [e.g., External API rate limits: 100 req/min for payments API]
- [e.g., Always run linter before committing]

## Identity & Preferences

<!-- If you have an IDENTITY.md file, reference it here. Otherwise delete this section. -->
See [IDENTITY.md](IDENTITY.md) for who I am and how I like to work.

## Git Workflow

<!-- If you have a GIT-RULES.md file, reference it here. Otherwise delete this section. -->
See [GIT-RULES.md](GIT-RULES.md) for how to handle git operations in this project.
