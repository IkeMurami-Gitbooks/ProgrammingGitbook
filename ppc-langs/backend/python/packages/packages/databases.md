# Databases

Один из самых популярных пакетов для работы с базами данных — [sqlalchemy](https://www.sqlalchemy.org/). Он позволяет конструировать запросы для различных баз данных. Рассмотрим основные практические моменты в работе с sqlalchemy.

## Описание схемы базы данных

### Базовый пример

```python
from sqlalchemy import Column, ForeignKey, String, Integer, Boolean, Table
from sqlalchemy.orm import declarative_base

from table_base import Base
from myrelationship import MyRelationship


Base = declarative_base()


class TableA(Base):
    __tablename__ = "table_a"

    key = Column(String(40), primary_key=True)
    value = Column(Integer, ForeighKey('table_b.id'))
    b = Column(Boolean)
```

`Base` — это базовый класс, который будет хранить всю информацию о вашей будующей схеме базы. При этом, он должен быть определен только **один** раз. Все другие таблицы, должны наследоваться от `Base`.

### Организация кода для описания таблиц

Один из возможных способов организации кода для описания схемы базы:

<pre class="language-python"><code class="lang-python"><strong># table_base.py
</strong><strong>from sqlalchemy.orm import declarative_base
</strong>
Base = declarative_base()

# table_a.py
from sqlalchemy import Column, ForeignKey, String, Integer, Boolean, Table
from table_base import Base

class TableA(Base):
    __tablename__ = "table_a"

    value = Column(String(40), primary_key=True)

# table_b.py
from sqlalchemy import Column, ForeignKey, String, Integer, Boolean, Table
from table_base import Base

class TableB(Base):
    __tablename__ = "table_b"

    value = Column(String(40), primary_key=True)

# main.py
from table_a import TableA
from table_b import TableB
from table_base import Base

print(f'Base meta tables: {Base.metadata.tables}')
</code></pre>

## Relationships

## Конструирование запросов

## Паттерн Provider -> Repository -> Database

## Подсоединяемся к базе
