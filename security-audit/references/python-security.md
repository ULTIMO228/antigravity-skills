# Python Security Reference

## FastAPI / ASGI Security
- CORS: never `allow_origins=["*"]` in production
- Use `Depends()` for auth middleware on every protected route
- Request body validation through Pydantic models (auto-sanitized)
- Response model filtering: `response_model=PublicUser` hides internal fields
- Rate limiting: `slowapi` or custom middleware

## httpx (HTTP Client)
- Always set timeouts: `httpx.AsyncClient(timeout=30.0)`
- Verify SSL: never `verify=False` in production
- Connection pooling: reuse client instances
- Retry logic: `tenacity` with exponential backoff

## SQLAlchemy / Database
- ALWAYS parameterized queries: `session.execute(text("SELECT * FROM users WHERE id = :id"), {"id": user_id})`
- NEVER: `f"SELECT * FROM users WHERE id = {user_id}"`
- Use ORM methods when possible (auto-parameterized)
- Connection pool limits: `pool_size=20, max_overflow=10`

## Pydantic Validation
- All user inputs through Pydantic models
- Field constraints: `Field(ge=0, le=100)`, `Field(min_length=1)`
- Custom validators for complex rules
- `model_config = ConfigDict(strict=True)` for strict type checking

## Secrets Management
- All secrets in `.env` via `pydantic-settings`
- Never log secrets: mask in error messages
- API keys: rotate regularly, use scoped permissions
- JWT: use RS256 (asymmetric), not HS256 for production

## Dependency Audit
```bash
pip audit                  # Check known vulnerabilities
safety check               # Alternative scanner
pip list --outdated        # Check for updates
```

## MOEX Cortex Specific
- MOEX ISS API: no auth needed, but rate limit (max 50 req/sec)
- OpenRouter API key: store in .env, never in code
- Risk Manager: no user input reaches risk calculations directly
- Knowledge Graph: sanitize data before NetworkX insertion
- LLM outputs: always validate through Pydantic schema before use
