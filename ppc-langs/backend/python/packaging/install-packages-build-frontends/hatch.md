# hatch

hatch: [https://hatch.pypa.io/latest/](https://hatch.pypa.io/latest/)

## Поддержка strict type проверок для Pylance для библиотек своих

Кладем в код библиотеки нашей пустой файл `py.typed`. Этот файл указывает системе сборки автоматически создавать stub-файлы, в которых будут перечислены все типы для `pylance`.

А далее добавляем в конфиг `pyproject.toml`:

```ini
[build-system]
requires = ["hatchling", "hatch-build-scripts"]
build-backend = "hatchling.build"

[project]
name = "bot-service"
version = "0.1.0"
description = "Telegram Bot Service"
requires-python = "==3.13.*"

dependencies = [
    "httpx==0.27.0",
    "openai==2.2.0",
    "pydantic-settings==2.2.1",
    "python-telegram-bot==22.5",
    "python-telegram-bot[job-queue]==22.5",
    "pytz==2024.1"
]

[tool.hatch.build.targets.wheel]
only-include = [
    "src/code",
]

# Включаем py.typed и все .pyi файлы
[tool.hatch.build.targets.wheel.force-include]
"src/code/py.typed" = "my/package/py.typed"

[tool.hatch.build.targets.wheel.sources]
"src/code" = "my/package"

```

Теперь, при использовании библиотеки Pylance не будет ругаться, что типов не видно для классов из библиотек:

```python
import my.package
```
