# namespaces

link: [https://packaging.python.org/en/latest/guides/packaging-namespace-packages/](https://packaging.python.org/en/latest/guides/packaging-namespace-packages/)

Пример `Native namespace packages`:

```
mynamespace-subpackage-a/
    pyproject.toml # AND/OR setup.py, setup.cfg
    src/
        mynamespace/ # namespace package
            # No __init__.py here.
            subpackage_a/
                # Regular import packages have an __init__.py.
                __init__.py
                module.py
```

И в `pyproject.toml`:

```ini
[build-system]
...

[tool.setuptools.packages.find]
where = ["src/"]
include = ["mynamespace.subpackage_a"]

[project]
name = "mynamespace-subpackage-a"
...
```

Поиск через код:

```python
import importlib
import inspect
import pkgutil
import types
import typing

import ikemurami.plugin
from ikemurami.core import Plugin


def iter_namespace(ns_pkg):
    return pkgutil.iter_modules(ns_pkg.__path__, ns_pkg.__name__ + '.')


def get_plugins(module: types.ModuleType) -> typing.List[Plugin]:
    plugins: typing.List[Plugin] = list()

    for attr_name in dir(module):
        attr = getattr(module, attr_name)

        if inspect.isclass(attr) and issubclass(attr, Plugin):
            plugins.append(attr)

    return plugins


def main():

    discovered_plugins: typing.List[Plugin] = sum(
        [
            get_plugins(importlib.import_module(name))
            for finder, name, ispkg
            in iter_namespace(ikemurami.plugin)
        ],
        []
    )
    print(discovered_plugins)

    for plugin in discovered_plugins:
        print('Plugin', plugin, plugin.run(3, 5))


if __name__ == '__main__':
    main()

```

Plugin base class:

```python
class Plugin:

    @staticmethod
    def run(a: int, b: int) -> int:
        ...

```

Plugin class:

```python
from ikemurami.core import Plugin


class MyPlugin(Plugin):

    def run(a: int, b: int) -> int:
        return a * b

```
