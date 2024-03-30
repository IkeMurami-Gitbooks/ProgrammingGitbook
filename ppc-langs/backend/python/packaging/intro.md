# Intro

Guide from PyPA: [https://packaging.python.org/en/latest](https://packaging.python.org/en/latest)

Пакеты — согласно гайду выше — все, что может быть собрано для дистрибуции, как правило — это то, что в корне содержит `pyproject.toml`, `setup.py` или `setup.cfg` (последние два использует `setuptools`).

Дистрибуция (библиотеки и инструменты):

* Исходным кодом (подходит, если среда, в которой будет запускать код соответствует вашей)
  * Называется **Source distribution** package или `sdist` (собирает в архив `.tar.gz`)
  * Можно собрать через обычные инструменты: `python -m build --sdist`
* Собранным пакетом
  * Называется **Built distribution** package (или `wheels`)
  * Wheel-формат (использует `pip`, пришел на замену Egg-формату)
  * Egg-формат (использует `setuptools`)

Сборщики состоят из двух частей:

* **Build Frontend** — это инструменты, которые собирают из исходного кода пакет для дистрибуции. Примеры build frontend — `pip` и `build`, `poetry`?. Используют Build Backend's.
  * Сборка:

```
~ python -m pip install --upgrade build
~ cat pyproject.toml
~ python -m build
~ ls -al
dist/
├── example_package_YOUR_USERNAME_HERE-0.0.1-py3-none-any.whl
└── example_package_YOUR_USERNAME_HERE-0.0.1.tar.gz
```

* **Build Backend** — это библиотеки, которые умеют собирать из исходного кода пакет для дистрибуции для разных платформ, окружений и тп. Примеры: `hatch`, `setuptools`, `meson`, `flit` и тд

Про [загрузку в PyPI](https://packaging.python.org/en/latest/tutorials/packaging-projects/#uploading-the-distribution-archives) через twine.
