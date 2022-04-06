# Объект для Set (множества)

\_\_eq\_\_ - нужно переопределять для сравнения (это происходит при добавлении в множество)

\_\_str\_\_ - нужно переопределять, что бы одинаковые объекты выглядели одинаково

\_\_hash\_\_ - для добавления в множество

```python
class Record(BaseModel):
    address: Address
    length: str
    record_type: str
    string: str

    def __eq__(self, other):
        if self.string == other.string:
            return True
        else:
            return False

    def __str__(self):
        return self.string

    def __hash__(self):
        return hash(str(self))
```
