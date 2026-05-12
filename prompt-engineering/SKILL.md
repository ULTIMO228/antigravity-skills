---
name: prompt-engineering
description: >
  Patterns for LLM prompt design: structured outputs via Pydantic,
  few-shot templates, chain-of-thought. Use when designing prompts
  for agents or LLM pipelines. Prevents hallucination and schema drift.
---

# prompt-engineering — LLM Prompt Design

## Core Principles

### 1. Always use Structured Outputs
Every LLM response → Pydantic schema. No free-form text parsing.

```python
from pydantic import BaseModel, Field
from typing import Literal

class MarketDecision(BaseModel):
    action: Literal["buy", "sell", "hold"]
    confidence: float = Field(ge=0, le=1)
    reasoning: str = Field(min_length=20)
    risk_level: Literal["low", "medium", "high"]
```

### 2. System prompt structure
```
ROLE: Who the LLM is (domain expert)
CONTEXT: What it knows (market data, rules)
TASK: What to do (analyze, decide, classify)
CONSTRAINTS: Boundaries (never exceed risk, always cite data)
FORMAT: Output schema (Pydantic model reference)
```

### 3. Few-shot examples
Include 2-3 examples in prompt for complex tasks:
```
Example 1:
Input: SBER daily candles showing uptrend, RSI=65, Hurst=0.72
Output: {"action": "buy", "confidence": 0.7, "reasoning": "..."}
```

### 4. Chain-of-thought
For complex reasoning, force step-by-step:
```
Think step by step:
1. Analyze current market regime (trending/mean-reverting/random)
2. Check technical indicators alignment
3. Assess risk-reward ratio
4. Make decision with confidence level
```

## Anti-patterns
| ❌ Don't | ✅ Do |
|---------|------|
| "Give me a trading signal" | Use Pydantic schema with constrained fields |
| Free-form JSON parsing | `model.model_validate_json(response)` |
| Trust LLM risk assessment | Risk Manager = deterministic Python only |
| Single long prompt | Decompose into agent pipeline |
| Hardcode prompt strings | Store in config / templates |

## Instructor integration
```python
import instructor
from openai import AsyncOpenAI

client = instructor.from_openai(AsyncOpenAI())
result = await client.chat.completions.create(
    model="deepseek/deepseek-chat-v4",
    response_model=MarketDecision,
    messages=[{"role": "user", "content": analysis_prompt}],
    max_retries=3,
)
# result is validated MarketDecision instance
```

## Temperature guidance
| Task | Temperature | Why |
|------|-------------|-----|
| Classification, extraction | 0.0–0.3 | Deterministic |
| Analysis, reasoning | 0.5–0.7 | Balanced |
| Creative, brainstorming | 0.8–1.0 | Diverse |
| Gemini 3 models | 1.0 ONLY | Other values break reasoning |
