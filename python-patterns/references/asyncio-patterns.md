# Asyncio Patterns Reference

## TaskGroup (Python 3.11+, structured concurrency)
```python
async def fetch_all_data(symbols: list[str]) -> list[MarketData]:
    results: list[MarketData] = []
    async with asyncio.TaskGroup() as tg:
        for symbol in symbols:
            tg.create_task(fetch_symbol(symbol, results))
    return results
# If any task fails, ALL tasks are cancelled → clean shutdown
```

## Semaphore (rate limiting)
```python
# MOEX ISS API: max 50 req/sec
sem = asyncio.Semaphore(50)

async def fetch_with_limit(url: str) -> dict:
    async with sem:
        async with httpx.AsyncClient() as client:
            response = await client.get(url, timeout=30.0)
            return response.json()
```

## Async Context Managers
```python
from contextlib import asynccontextmanager

@asynccontextmanager
async def managed_client():
    """Reusable HTTP client with proper cleanup."""
    client = httpx.AsyncClient(
        base_url="https://iss.moex.com/iss",
        timeout=30.0,
        limits=httpx.Limits(max_connections=20)
    )
    try:
        yield client
    finally:
        await client.aclose()

# Usage
async with managed_client() as client:
    data = await client.get("/engines/stock/markets/shares.json")
```

## Gather with Error Handling
```python
async def fetch_multiple(urls: list[str]) -> list[dict | None]:
    tasks = [fetch_one(url) for url in urls]
    results = await asyncio.gather(*tasks, return_exceptions=True)
    return [
        r if not isinstance(r, Exception) else None
        for r in results
    ]
```

## Timeout Pattern
```python
async def with_timeout(coro, seconds: float = 30.0):
    try:
        async with asyncio.timeout(seconds):
            return await coro
    except TimeoutError:
        logger.warning("Operation timed out after %s sec", seconds)
        return None
```

## Event-Driven Pattern (for LangGraph triggers)
```python
class MarketEventBus:
    def __init__(self) -> None:
        self._subscribers: dict[str, list[Callable]] = {}

    async def publish(self, event: str, data: dict) -> None:
        for callback in self._subscribers.get(event, []):
            await callback(data)

    def subscribe(self, event: str, callback: Callable) -> None:
        self._subscribers.setdefault(event, []).append(callback)
```

## Anti-patterns
- ❌ `asyncio.run()` inside async function (RuntimeError)
- ❌ `time.sleep()` in async code (blocks event loop)
- ❌ Creating new event loop when one exists
- ❌ Fire-and-forget tasks without tracking
- ❌ Mixing sync and async without `run_in_executor`
