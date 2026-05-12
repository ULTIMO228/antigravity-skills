# Python Testing Reference — pytest

## Setup
```toml
# pyproject.toml
[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]
python_files = ["test_*.py"]
python_functions = ["test_*"]
addopts = "--tb=short -q"
```

## Fixtures + conftest.py
```python
# tests/conftest.py
import pytest
from unittest.mock import AsyncMock

@pytest.fixture
def mock_moex_client() -> AsyncMock:
    """Мок MOEX ISS API клиента."""
    client = AsyncMock()
    client.get_candles.return_value = [
        {"open": 100.0, "close": 101.0, "high": 102.0, "low": 99.0}
    ]
    return client

@pytest.fixture
def risk_config() -> dict:
    """Тестовая конфигурация риск-менеджера."""
    return {
        "max_position_pct": 0.02,
        "max_daily_loss": 1000.0,
        "max_open_positions": 5,
    }
```

## AsyncMock patterns
```python
# Mock async context manager
mock_session = AsyncMock()
mock_session.__aenter__.return_value = mock_session
mock_session.__aexit__.return_value = False

# Mock async iterator
async def mock_stream():
    yield {"data": "chunk1"}
    yield {"data": "chunk2"}
mock_client.stream.return_value = mock_stream()
```

## Parametrize
```python
@pytest.mark.parametrize("input_val,expected", [
    (0.0, False),      # граница: ноль
    (-1.0, False),     # ошибка: отрицательное
    (0.01, True),      # минимальное валидное
    (100.0, True),     # нормальное
    (1e10, False),     # граница: слишком большое
])
def test_validate_amount(input_val: float, expected: bool) -> None:
    assert validate_amount(input_val) == expected
```

## AAA Example (Python)
```python
class TestRiskManager:
    async def test_должен_одобрить_валидную_сделку(
        self, risk_config: dict, mock_moex_client: AsyncMock
    ) -> None:
        # Arrange
        manager = RiskManager(risk_config)
        trade = Trade(symbol="SBER", amount=100, price=250.0)

        # Act
        result = await manager.validate(trade, mock_moex_client)

        # Assert
        assert result.approved is True
        assert result.reason == ""

    async def test_должен_отклонить_превышение_лимита(
        self, risk_config: dict
    ) -> None:
        # Arrange
        manager = RiskManager(risk_config)
        trade = Trade(symbol="SBER", amount=1_000_000, price=250.0)

        # Act
        result = await manager.validate(trade)

        # Assert
        assert result.approved is False
        assert "лимит" in result.reason.lower()

    async def test_должен_обработать_пустой_символ(self) -> None:
        # Arrange & Act & Assert
        with pytest.raises(ValueError, match="Символ обязателен"):
            Trade(symbol="", amount=100, price=250.0)
```

## Coverage
```bash
pytest --cov=src --cov-report=term-missing --tb=short
# Target: ≥ 80% business logic
```

## Tips
- `pytest-asyncio` for async tests (asyncio_mode="auto" in config)
- `pytest-mock` for `mocker` fixture (cleaner than `unittest.mock`)
- `freezegun` or `time-machine` for datetime mocking
- Use `tmp_path` fixture for file operations
- `caplog` fixture for log assertions
