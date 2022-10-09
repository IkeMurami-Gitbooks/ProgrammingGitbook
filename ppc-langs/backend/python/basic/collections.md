# Collections

## Set

### Create Object for Set

`__eq__` — нужно переопределять для сравнения (это происходит при добавлении в множество)

`__str__` — нужно переопределять, что бы одинаковые объекты выглядели одинаково

`__hash__` — для добавления в множество

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
