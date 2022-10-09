# pydantic

Описание модели данных и их валидация/конвертации под капотом

## Enum

```python
from enum import Enum

class AddressType(Enum):
    FIRST_ELEMENT = 'FIRST'
    SECOND_ELEMENT = 'SECOND'
    
    UNKNOWN = 'UNKNOWN'
    
    @classmethod
    def _missing_(cls, value):
        return AddressType.UNKNOWN
```

## Pydantic Object

```python
from pydantic import BaseModel

class Address(BaseModel):
    offset: str
    address_type: AddressType
    
    
address = Address(**{
    'offset': '123',
    'address_type': AddressType('SECOND')
})
```

## Pydantic Iterable Object

```python
from pydantic import BaseModel

class Record(BaseModel):
    address: Address
    length: str
    record_type: str
    string: str
    test_bool: bool = False

    def __eq__(self, other):
        if self.string == other.string:
            return True
        else:
            return False

    def __str__(self):
        return self.string

    def __hash__(self):
        return hash(str(self))
        

string = ['1', '2', '3']
record = Record(**{
    'address': address,
    'length': '123',
    'record_type': 'some_type',
    'string': ' '.join(string)
})
```
