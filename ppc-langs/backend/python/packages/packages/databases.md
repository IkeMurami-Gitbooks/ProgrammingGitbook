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

`Base` — это базовый класс, который будет хранить всю информацию о вашей будущей схеме базы. При этом, он должен быть определен только **один** раз. Все другие таблицы, должны наследоваться от `Base`.

### Declarative vs. Imperative Forms[¶](https://docs.sqlalchemy.org/en/20/orm/basic\_relationships.html#declarative-vs-imperative-forms)

#### Declarative Form **with** mapping (это новый подход, лучше использовать его!)

```python
from sqlalchemy.orm import Mapper

class Parent(Base):
    __tablename__ = "parent_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    children: Mapped[List["Child"]] = relationship(back_populates="parent")


class Child(Base):
    __tablename__ = "child_table"

    id: Mapped[int] = mapped_column(primary_key=True)
    parent_id: Mapped[int] = mapped_column(ForeignKey("parent_table.id"))
    parent: Mapped["Parent"] = relationship(back_populates="children")
```

#### Declarative Form **without** mapping (это по классике):

```python
class Parent(Base):
    __tablename__ = "parent_table"

    id = mapped_column(Integer, primary_key=True)
    children = relationship("Child", back_populates="parent")


class Child(Base):
    __tablename__ = "child_table"

    id = mapped_column(Integer, primary_key=True)
    parent_id = mapped_column(ForeignKey("parent_table.id"))
    parent = relationship("Parent", back_populates="children")
```

#### List and sets

По умолчанию, связи маппятся в **List**. Но можно сделать в **Set**:

```python
class Parent(Base):
    __tablename__ = "parent_table"

    id = mapped_column(Integer, primary_key=True)
    children = relationship("Child", collection_class=set, ...)
```

#### Imperative Form:

У нас есть две таблицы, описанные декларативно. Добавляем связь императивно:

```python
registry.map_imperatively(
    Parent,
    parent_table,
    properties={"children": relationship("Child", back_populates="parent")},
)

registry.map_imperatively(
    Child,
    child_table,
    properties={"parent": relationship("Parent", back_populates="children")},
)
```

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

### One-to-One

На базовом уровне это выглядит следующим образом. То есть, мы отключаем поддержку коллекций и тем самым добиваемся One-to-One отношения. Это нужно, когда у родителя может быть только один ребенок, а у ребенка — один родитель.

```python
class Parent(Base):
    __tablename__ = "parent_table"
    id = Column(Integer, primary_key=True)

    # previously one-to-many Parent.children is now
    # one-to-one Parent.child
    child = relationship("Child", back_populates="parent", uselist=False)


class Child(Base):
    __tablename__ = "child_table"
    id = Column(Integer, primary_key=True)
    parent_id = Column(Integer, ForeignKey("parent_table.id"))

    # many-to-one side remains, see tip below
    parent = relationship("Parent", back_populates="child")
```

### One-to-Many

На базовом уровне это выглядит следующим образом

```python
# base.py
from sqlalchemy.orm import declarative_base
Base = declarative_base()

# parent.py
from sqlalchemy import Column, Integer
from sqlalchemy.orm import relationship

from base import Base


class Parent(Base):    
    __tablename__  = "parent_table"
    id = Column(Integer, primary_key=True)
    children = relationship("Child")
    
# child.py
from sqlalchemy import Column, Integer, ForeignKey

from base import Base


class Child(Base):
    __tablename__  = "child_table"
    id = Column(Integer, primary_key=True)
    parent_id = Column(Integer, ForeignKey("parent_table.id"))
    
# main.py
from sqlalchemy import select, insert

from parent import Parent
from child import Child


print(select(Parent))
print(select(Child))
```

Для двунаправленной связи, чтобы Child видел Parent целиком, а не только id (далее не надо будет делать отдельный запрос на уровне кода), изменим Child и Parent следующим образом:

```python
# parent.py
class Parent(Base):    
    __tablename__  = "parent_table"
    id = Column(Integer, primary_key=True)
    children = relationship("Child", back_populates="parent")

# child.py
class Child(Base):
    __tablename__  = "child_table"
    id = Column(Integer, primary_key=True)
    parent_id = Column(Integer, ForeignKey("parent_table.id"))
    parent = relationship("Parent", back_populates="children")
```

### Many-to-One

На базовом уровне это выглядит следующим образом

```python
# base.py
from sqlalchemy.orm import declarative_base
Base = declarative_base()

# parent.py
from sqlalchemy import Column, Integer, ForeignKey
from sqlalchemy.orm import relationship

from base import Base

class Parent(Base):
    __tablename__ = "parent_table"
    id = Column(Integer, primary_key=True)
    child_id = Column(Integer, ForeignKey("child_table.id"))
    child = relationship("Child")

# child.py
from sqlalchemy import Column, Integer, ForeignKey

from base import Base

class Child(Base):
    __tablename__  = "child_table"
    id = Column(Integer, primary_key=True)


# main.py
from sqlalchemy import select, insert

from parent import Parent
from child import Child


print(select(Parent))
print(select(Child))
```

Для двунаправленной связи, чтобы из Child можно было получить всех родителей, поменяем Parent и Child следующим образом

```python
class Parent(Base):
    __tablename__ = "parent_table"
    id = Column(Integer, primary_key=True)
    child_id = Column(Integer, ForeignKey("child_table.id"))
    child = relationship("Child", back_populates="parents")
    
class Child(Base):
    __tablename__  = "child_table"
    id = Column(Integer, primary_key=True)
    parents = relationship("Parent", back_populates="child")
```

### Many-to-Many

На базовом уровне это выглядит следующим образом.&#x20;

```python
association_table = Table(
    "association_table",
    Base.metadata,
    Column("left_id", ForeignKey("left_table.id"), primary_key=True),
    Column("right_id", ForeignKey("right_table.id"), primary_key=True),
)


class Parent(Base):
    __tablename__ = "left_table"
    id = Column(Integer, primary_key=True)
    children = relationship(
        "Child", secondary=association_table, back_populates="parents"
    )


class Child(Base):
    __tablename__ = "right_table"
    id = Column(Integer, primary_key=True)
    parents = relationship(
        "Parent", secondary=association_table, back_populates="children"
    )
```

Мы создаем ассоциативную таблицу, которая хранит все уникальные пары ключей из обоих таблиц и подключаем ее через свойство `secondary`. Через свойство `back_populates` настраиваем связь.

Если нам надо добавить в ассоциацию доп поля, или мы хотим чтобы все было в одном стиле, можем создать обычную таблицу как Child и Parent: [https://docs.sqlalchemy.org/en/14/orm/basic\_relationships.html#association-object](https://docs.sqlalchemy.org/en/14/orm/basic\_relationships.html#association-object)

## Конструирование запросов

### Конструирование запросов к обычной таблице

```python
from sqlalchemy.orm import declarative_base
from sqlalchemy import Column, Integer
from sqlalchemy import select, insert, update

Base = declarative_base()

class MyTable(Base):
   __tablename__ = "my_table"
   id = Column(Integer, primary_key=True)  # default: auto increment
   value = Column(String)
   
# INSERT
query = (
   insert(MyTable).
   values(
      value="some"
   ).
   returning(MyTable.id, MyTable.value)
)
print(query)

# SELECT
print(select(MyTable))
print(select(MyTable).where(MyTable.id == 1))

# UPDATE
from pydantic import BaseModel
class Value(BaseModel):
   value: str
   
value = Value(**{"value": "some"})
query = (
   update(MyTable).
   where(MyTable.id == 1).
   values(value.dict())   
)
# Такое сработает только если все ключи в PyDantic объекте совпадают с именами колонок в таблице
# Иначе прописываем все поля вручную

# DELETE
delete(MyTable).where(MyTable.id == id)

```

### Работа с relationships

```python
class First(Base):
    __tablename__  = "first_table"
    id = Column(Integer, primary_key=True)
    seconds = relationship("Second", secondary=association_table, back_populates="firsts")
    
select(First.seconds)
# И мы получаем достаточно сложный запрос, который вернет нам эти данные

First.seconds.remove(somechild)
# И это автоматически удалит запись и из secondary. Самим ходить и удалять допом ничего не надо
```

## Паттерн Provider -> Repository -> Database

## Подсоединяемся к базе
