# Pydantic v2 Reference

## Model Definition
```python
from pydantic import BaseModel, Field, ConfigDict, model_validator

class Trade(BaseModel):
    model_config = ConfigDict(strict=True, frozen=True)

    symbol: str = Field(min_length=1, max_length=12, examples=["SBER"])
    amount: int = Field(gt=0, le=1_000_000)
    price: float = Field(gt=0)
    direction: Literal["buy", "sell"]

    @model_validator(mode="after")
    def validate_total(self) -> "Trade":
        total = self.amount * self.price
        if total > 10_000_000:
            raise ValueError(f"Total {total} exceeds max 10M")
        return self
```

## Settings (from .env)
```python
from pydantic_settings import BaseSettings

class AppConfig(BaseSettings):
    model_config = ConfigDict(env_file=".env", env_file_encoding="utf-8")

    openrouter_api_key: str
    moex_base_url: str = "https://iss.moex.com/iss"
    max_position_pct: float = Field(default=0.02, ge=0, le=1)
    debug: bool = False
```

## Discriminated Unions
```python
class BuySignal(BaseModel):
    type: Literal["buy"] = "buy"
    symbol: str
    target_price: float

class SellSignal(BaseModel):
    type: Literal["sell"] = "sell"
    symbol: str
    stop_loss: float

Signal = Annotated[BuySignal | SellSignal, Field(discriminator="type")]
```

## LLM Response Schemas
```python
class MarketAnalysis(BaseModel):
    """Schema for LLM structured output."""
    regime: Literal["trending", "mean_reverting", "random"]
    confidence: float = Field(ge=0, le=1)
    reasoning: str = Field(min_length=10)
    recommended_action: Literal["buy", "sell", "hold"]
    risk_factors: list[str] = Field(default_factory=list)
```

## Migration from v1
| v1 (deprecated) | v2 (correct) |
|-----------------|-------------|
| `class Config:` | `model_config = ConfigDict(...)` |
| `@validator` | `@field_validator` |
| `@root_validator` | `@model_validator` |
| `.dict()` | `.model_dump()` |
| `.parse_obj()` | `.model_validate()` |
| `.schema()` | `.model_json_schema()` |
| `Optional[X]` | `X \| None` |
