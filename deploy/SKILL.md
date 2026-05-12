---
name: deploy
description: >
  Docker, compose, and CI/CD patterns. Dockerfile best practices,
  multi-stage builds, healthchecks. Use when containerizing
  or setting up deployment pipeline.
---

# deploy — Containerization & CI/CD

## Dockerfile (Python, multi-stage)

```dockerfile
# Stage 1: Dependencies
FROM python:3.11-slim AS deps
WORKDIR /app
COPY pyproject.toml uv.lock ./
RUN pip install uv && uv pip install --system --no-cache -r pyproject.toml

# Stage 2: Runtime
FROM python:3.11-slim AS runtime
WORKDIR /app

# Non-root user
RUN useradd --create-home appuser
USER appuser

COPY --from=deps /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=deps /usr/local/bin /usr/local/bin
COPY src/ ./src/

# Healthcheck
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD python -c "import httpx; httpx.get('http://localhost:8000/health')" || exit 1

EXPOSE 8000
CMD ["python", "-m", "src.main"]
```

## docker-compose.yml

```yaml
services:
  app:
    build: .
    ports:
      - "8000:8000"
    env_file: .env
    restart: unless-stopped
    depends_on:
      redis:
        condition: service_healthy

  redis:
    image: redis:7-alpine
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

## Rules
| Rule | Why |
|------|-----|
| Multi-stage builds | Smaller images, no build tools in runtime |
| Non-root user | Security: container breakout mitigation |
| `.dockerignore` | Exclude .env, .git, __pycache__, .venv |
| Pin base image versions | Reproducible builds |
| HEALTHCHECK | Orchestrator knows when app is ready |
| `env_file` not `environment` | Secrets not in compose file |

## .dockerignore
```
.env*
.git
.venv
__pycache__
*.pyc
.mypy_cache
.ruff_cache
tests/
.ai/
.foryou/
```

## GitHub Actions (basic CI)
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - run: pip install -e ".[dev]"
      - run: ruff check .
      - run: pytest --tb=short -q
```
