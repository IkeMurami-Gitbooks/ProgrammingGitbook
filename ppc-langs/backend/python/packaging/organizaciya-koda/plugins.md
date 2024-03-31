# Plugins

link: [https://packaging.python.org/en/latest/guides/creating-and-discovering-plugins/](https://packaging.python.org/en/latest/guides/creating-and-discovering-plugins/)

В Python есть три способа организовать поддержку плагинов (подробнее про каждый в статье выше)

* Используя соглашение об  имени для плагинов (так например flask делает)
* Используя namespace'ы
* Используя блок метаданных в pyproject.toml

## Используя блок метаданных в pyproject.toml

Объявляем entry point в `pyproject.toml` (это просто указание, что нужно импортировать, чтобы подключить плагин), например, для `hatchling`:

```ini
[project.entry-points.'playground.plugins']
myplugin = "playground.core.plugins.myplugin"
```

где:\
`myplugin` — название плагина\
`playground.plugins` — название группы плагинов\
`playground.core.plugins.myplugin` — тот python-пакет, который реализует логику плагина (если бы мы подключали его сами, то писали бы `import playground.core.plugins.myplugin`)

В python (`>=3.10`) подключаем плагин следующим образом:

```python
from importlib.metadata import entry_points

"""
# если бы не было плагина, а мы подключали бы код напрямую:

from playground.core.plugins.myplugin import start

start()
"""

# Option 1: Load a plugin
(myplugin,) = entry_points(group='playground.plugins', name='myplugin')
module = myplugin.load()
module.start()

# Option 2: Load plugins
discovered_plugins = entry_points(group='playground.plugins')
for plugin in plugins:
    module = plugin.load()
    module.start()
```
