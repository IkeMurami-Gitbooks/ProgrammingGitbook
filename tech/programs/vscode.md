# vscode

Vscode (VNote для ценителей Вим) + mermaid/flowchart

У VSCode есть CLI, который позволяет удобно управлять Workspace'ми

`Ctrl+Shift-P` — открыть Command Palette — консоль управления VS Code.

## VSCode CLI

Открыть текущую папку (откроется новое окно)

```
code .
```

Открыть новое окно

```
code -n ...
```

Открыть в текущем окне

```
code -w ...
```

Добавить папку(и) в последний workspace

```
code --add my-folder-1 my-folder-2
```

Есть другие команды, например, сравнить два файла.

## Useful extensions

* [Compare Folders](https://marketplace.visualstudio.com/items?itemName=moshfeu.compare-folders)
* Для работы с Docker контейнерами
  * [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
  * [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
* Для разработки
  * [React](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
  * [React Native Tools](https://marketplace.visualstudio.com/items?itemName=msjsdiag.vscode-react-native)
  * [Graphviz](https://marketplace.visualstudio.com/items?itemName=joaompinto.vscode-graphviz) для поддержки синтаксиса dot
  * [Kotlin](https://marketplace.visualstudio.com/items?itemName=fwcd.kotlin)
  * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  * [Ruby](https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby)
  * [JSON crack](https://marketplace.visualstudio.com/items?itemName=AykutSarac.jsoncrack-vscode) для визуализации JSON структуры
* [New Relic Code Stream](https://marketplace.visualstudio.com/items?itemName=CodeStream.codestream)
* [VSCode PDF Support](https://marketplace.visualstudio.com/items?itemName=tomoki1207.pdf)

## .vscode/settings.json

Пример настроек

```json
{
  "editor.tabSize": 2,
  "editor.useTabStops": false,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "always"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.tabSize": 4,
    "editor.useTabStops": false
  },
  "typescript.tsdk": "./src/node_modules/typescript/lib",
  "i18n-ally.enabledFrameworks": ["vue"],
  "i18n-ally.localesPaths": ["src/i18n/locales"],
  "i18n-ally.sortKeys": false,
  "i18n-ally.keepFulfilled": false,
  "i18n-ally.keystyle": "nested",
  "editor.gotoLocation.multipleDefinitions": "goto"
}
```

## .vscode/extensions.json

Пример настроек

```json
{
  "recommendations": [
    "aaron-bond.better-comments",
    "dbaeumer.vscode-eslint",
    "antfu.goto-alias",
    "visualstudioexptteam.vscodeintellicode",
    "esbenp.prettier-vscode",
    "yoavbls.pretty-ts-errors",
    "bradlc.vscode-tailwindcss",
    "vue.volar",
    "lokalise.i18n-ally",
    "DavidAnson.vscode-markdownlint"
  ]
}
```
