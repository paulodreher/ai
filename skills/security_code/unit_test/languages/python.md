# Python — pytest

## Setup

```toml
# pyproject.toml
[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = "-v --tb=short"
```

```bash
pip install pytest pytest-mock pytest-cov
pytest                        # run all tests
pytest tests/test_orders.py   # single file
pytest -k "test_empty"        # filter by name
pytest --cov=src --cov-report=term-missing
```

## File & Naming Conventions

- Files: `tests/test_<module>.py`
- Functions: `def test_<behavior>():`
- Classes: `class TestClassName:` (no `__init__`)

## Basic Test

```python
# tests/test_cart.py
from src.cart import Cart

def test_add_item_increases_total():
    cart = Cart()
    cart.add("apple", price=1.50, qty=2)
    assert cart.total() == 3.00

def test_empty_cart_total_is_zero():
    assert Cart().total() == 0.0

def test_add_negative_qty_raises():
    with pytest.raises(ValueError, match="qty must be positive"):
        Cart().add("apple", price=1.0, qty=-1)
```

## Fixtures

```python
import pytest
from src.cart import Cart

@pytest.fixture
def cart():
    return Cart()

def test_total_after_add(cart):
    cart.add("x", price=10.0, qty=1)
    assert cart.total() == 10.0

# Parametrize to DRY up edge cases
@pytest.mark.parametrize("qty,expected", [
    (1, 10.0),
    (0, 0.0),
    (100, 1000.0),
])
def test_total_parametrized(cart, qty, expected):
    cart.add("x", price=10.0, qty=qty)
    assert cart.total() == expected
```

## Mocking

```python
from unittest.mock import MagicMock, patch
from src.notifier import send_confirmation

def test_send_confirmation_called_on_checkout(cart, mocker):
    mock_send = mocker.patch("src.cart.send_confirmation")
    cart.add("x", price=5.0, qty=1)
    cart.checkout(email="a@b.com")
    mock_send.assert_called_once_with("a@b.com", total=5.0)

# Freeze time
def test_order_timestamp(mocker):
    mocker.patch("src.orders.datetime").now.return_value = datetime(2024, 1, 1)
    order = Order()
    assert order.created_at.year == 2024
```

## Common Patterns

| Need                  | Tool                                  |
|-----------------------|---------------------------------------|
| Temporary files       | `tmp_path` fixture (built-in)         |
| Env vars              | `monkeypatch.setenv("KEY", "value")`  |
| DB (SQLAlchemy)       | In-memory SQLite + `@pytest.fixture`  |
| Async code            | `pytest-asyncio`, `@pytest.mark.asyncio` |
| HTTP (requests)       | `responses` or `httpretty` library    |
| HTTP (httpx)          | `respx` library                       |
