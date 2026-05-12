---
name: python-patterns
description: >
  Python-specific patterns and best practices. Covers asyncio,
  Pydantic v2, typing, error handling. Load references for deep
  details. Use when writing Python code that needs quality patterns.
---

# python-patterns — Python Best Practices

## Step 0: Context
Read `.ai/SKILL_CONFIG.md` → confirm Python stack.
Load relevant references as needed.

## Core Rules

### Type Hints (Python 3.11+)
```python
# ✅ Modern syntax
def process(items: list[str], config: dict[str, int] | None = None) -> bool: ...

# ❌ Legacy
from typing import List, Dict, Optional
def process(items: List[str], config: Optional[Dict[str, int]] = None) -> bool: ...
```

### Error Handling
```python
# ✅ Specific exceptions, actionable messages
class TradeValidationError(Exception):
    def __init__(self, field: str, reason: str) -> None:
        super().__init__(f"Trade validation failed: {field} — {reason}")

try:
    result = await client.execute(trade)
except httpx.TimeoutException:
    logger.warning("MOEX API timeout for %s", trade.symbol)
    raise
except httpx.HTTPStatusError as e:
    logger.error("MOEX API error %d: %s", e.response.status_code, e.response.text)
    raise TradeExecutionError(trade.symbol, str(e)) from e

# ❌ Never
try:
    ...
except:
    pass
```

### Logging
```python
import logging
logger = logging.getLogger(__name__)

# ✅ Lazy formatting (no f-strings)
logger.info("Processing trade %s at price %.2f", symbol, price)

# ❌ Eager evaluation (wastes CPU if level disabled)
logger.info(f"Processing trade {symbol} at price {price:.2f}")
```

### Dataclasses vs Pydantic
| Use case | Choice |
|----------|--------|
| Internal data structures | dataclass |
| External data (API, user input) | Pydantic |
| Config from .env | Pydantic Settings |
| LLM response parsing | Pydantic (strict validation) |

## References
- Complex Pydantic patterns → read `references/pydantic-v2.md`
- Async patterns → read `references/asyncio-patterns.md`
