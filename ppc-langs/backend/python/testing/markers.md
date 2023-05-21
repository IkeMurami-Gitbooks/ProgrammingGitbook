# Markers

Маркеры позволяют помечать тесты на группы, например:

```python
import pytest

@pytest.mark.smoke
def test_some():
    ...
    
@pytest.mark.smoke
@pytest.mark.asyncio
async def test_some_async():
    ...
```

Обратите внимание: smoke — придуманный маркер, его нет, но мы можем его использовать, а потом запустить только smoke-тесты:

```
pytest -m "smoke"
pytest -m "smoke and anothermarker"
```
