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

