# src-layout vs flat-layout

Link: [https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)

Есть два подхода — `src-layout` (код, который будет импортироваться куда-то помещается в директорию src) и `flat-layout` (код кладется в директорию наравне с вспомогательными скриптами и конфигами). src-layout гарантирует, что разрабатываемая часть кода будет выполнена только в том случае, если она будет установлена как пакет (в flat-layout такой гарантии нет, и код может быть исполнен даже без явного импорта), но это создает +1 шаг при разработки (пакет надо поставить, но можно использовать dev сборку / editable installation).

src-layout:

```
.
├── README.md
├── noxfile.py
├── pyproject.toml
├── setup.py
├── src/
│    └── awesome_package/
│       ├── __init__.py
│       └── module.py
└── tools/
    ├── generate_awesomeness.py
    └── decrease_world_suck.py
```

flat-layout:

```
.
├── README.md
├── noxfile.py
├── pyproject.toml
├── setup.py
├── awesome_package/
│   ├── __init__.py
│   └── module.py
└── tools/
    ├── generate_awesomeness.py
    └── decrease_world_suck.py
```

