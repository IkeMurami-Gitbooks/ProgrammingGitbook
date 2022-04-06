# Default value for Enum

```python
from enum import Enum

class MyEnum(Enum):
    HEADER = 'HEADER'
    BODY = 'BODY'
    UNKNOWN = 'UNKNOWN'
    
    @classmethod
    def _missing_(cls, value):
        return MyEnum.UNKNOWN
```

Получение значения и имени:

```python
from enum import Enum

class MyEnum(Enum):
    HEADER = 'Test'

MyEnum.HEADER.name == 'HEADER'
MyEnum.HEADER.value == 'Test'

```
