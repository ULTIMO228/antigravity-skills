# Python Code Review Checklist

## Type Safety
- [ ] No `Any` types — use `Unknown` or proper generics
- [ ] Type hints on all public functions (Python 3.11+ syntax: `list[str]`)
- [ ] Pydantic models use `model_validator` not `@validator` (v2)
- [ ] No `Optional[X]` — use `X | None` (Python 3.10+)
- [ ] Return types specified

## Async Patterns
- [ ] `async with` for context managers (httpx, db sessions)
- [ ] No `asyncio.run()` inside async functions
- [ ] `asyncio.gather()` for parallel operations (not sequential await)
- [ ] Proper timeout handling (`asyncio.timeout()`)
- [ ] No blocking calls in async code (no `time.sleep`, use `asyncio.sleep`)
- [ ] TaskGroup for structured concurrency (Python 3.11+)

## Pydantic v2
- [ ] `ConfigDict` instead of inner `class Config`
- [ ] `model_validator(mode="before")` / `model_validator(mode="after")`
- [ ] `Field(...)` with proper constraints (ge, le, min_length)
- [ ] No mutable defaults in Field
- [ ] `model_dump()` not `dict()`, `model_validate()` not `parse_obj()`

## Error Handling
- [ ] No empty `except:` or `except Exception:`
- [ ] Specific exceptions caught
- [ ] Custom exceptions for domain errors
- [ ] Error messages are actionable
- [ ] No swallowed exceptions (log at minimum)

## Common Pitfalls
- [ ] Mutable default arguments: `def fn(items: list = [])` → `def fn(items: list | None = None)`
- [ ] `is` vs `==`: use `is` only for `None`, `True`, `False`
- [ ] f-strings in logging: use `logger.info("msg %s", var)` not `logger.info(f"msg {var}")`
- [ ] No `print()` in production code — use `logging`
- [ ] Comprehensions vs loops: prefer comprehensions for simple transforms

## Performance
- [ ] No N+1 queries (batch operations)
- [ ] Connection pooling for HTTP clients (reuse httpx.AsyncClient)
- [ ] Lazy loading where appropriate
- [ ] No unnecessary data copying (generators vs lists for large data)

## Project-Specific (MOEX Cortex)
- [ ] Risk Manager: pure Python, no LLM calls
- [ ] LLM responses: always through Pydantic schema (src/llm/schemas.py)
- [ ] Config values from `src/config.py`, not hardcoded
- [ ] MOEX API calls: proper rate limiting and retry logic
