# Unit tests

## Simple unit test

```python
import pytest

def test_some():
   assert 1 == 1
```

## Async unit test

Надо поставить плагин для pytest — `pytest-asyncio`

```python
import pytest

@pytest.mark.asyncio
async def test_some():
    assert 1 == 1
```

## Test exceptions

```python
import pytest

def test_raise():
    with pytest.raises(TypeError):
        // do something that lead to the TypeError exception
        ... 
```

## Запустить тест с набором параметров

```python
@pytest.mark.parametrize('summary, owner, done',
                             [('sleep', None, False),
                              ('wake', 'brian', False),
                              ('breathe', 'BRIAN', True),
                              ('eat eggs', 'BrIaN', False),
                              ])
    def test_add_3(summary, owner, done):
        """Демонстрирует параметризацию с несколькими параметрами."""
        task = Task(summary, owner, done)
        task_id = tasks.add(task)
        t_from_db = tasks.get(task_id)
        assert equivalent(t_from_db, task)
```
