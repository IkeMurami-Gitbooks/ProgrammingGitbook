# poetry

Управление пакетами и зависимостями. [https://python-poetry.org/](https://python-poetry.org/)

```
pip install poetry
poetry init
poetry config virtualenvs.in-project true

poetry add some-dep-proj
poetry install

poetry show --tree

poetry run python main.py --help
poetry run uvicorn main:app ...
```
