# pyproject.toml

Документация: [https://packaging.python.org/en/latest/guides/writing-pyproject-toml/](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/)

## Блоки

### \[build-system]

Конфиг `pyproject.toml` указывает `build frontend`'у через блок `[build-system]` какой `build backend` использовать.

```ini
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

`requires` — список пакетов, необходимых для сборки пакета (как правило, достаточно пакета build backend). Build frontend установит их перед сборкой.

Остальные настройки могут быть определены в блоке `tool` или в отдельных конфигах (например, для `setuptools` бекенда дополнительные параметры можно определить в файлах `setup.py` или `setup.cfg`).

### \[project]

Блок метаданных.
