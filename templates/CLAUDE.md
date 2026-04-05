# Project: [Your Project Name]

## Overview
[Brief 1-2 sentence description of what this project does]

## Tech Stack
- **Language:** [e.g., TypeScript, Python, Go]
- **Framework:** [e.g., React, FastAPI, Gin]
- **Database:** [e.g., PostgreSQL, MongoDB]
- **Other:** [e.g., Redis, Docker, Kubernetes]

## Build & Run

```bash
# Install dependencies
[e.g., npm install, pip install -r requirements.txt]

# Run development server
[e.g., npm run dev, python manage.py runserver]

# Build for production
[e.g., npm run build, docker build -t app .]
```

## Test

```bash
# Run all tests
[e.g., npm test, pytest]

# Run specific test file
[e.g., npm test -- path/to/test, pytest path/to/test.py]

# Run with coverage
[e.g., npm run test:coverage, pytest --cov]
```

## Lint & Format

```bash
# Lint
[e.g., npm run lint, ruff check .]

# Format
[e.g., npm run format, ruff format .]
```

## Project Structure

```
src/
├── components/    # [what's in here]
├── routes/        # [what's in here]
├── services/      # [what's in here]
├── utils/         # [what's in here]
└── types/         # [what's in here]
```

## Coding Conventions

- [e.g., Use functional components with hooks, not class components]
- [e.g., All API responses follow the { data, error, status } pattern]
- [e.g., Use snake_case for Python, camelCase for TypeScript]
- [e.g., Errors should be user-friendly messages, not stack traces]
- [e.g., All database queries go through the repository layer]

## Architecture Decisions

- [e.g., We use middleware for auth, not inline checks]
- [e.g., Caching is handled at the service layer with Redis]
- [e.g., Feature flags are managed through LaunchDarkly]

## Important Notes

- [e.g., Never commit .env files — use .env.example as template]
- [e.g., Database migrations must be backward compatible]
- [e.g., The /admin routes require the ADMIN role]
- [e.g., External API rate limits: 100 req/min for payments API]
