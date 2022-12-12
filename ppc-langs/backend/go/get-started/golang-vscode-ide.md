# Golang VSCode IDE

VSCode Extenstion for Go: [https://marketplace.visualstudio.com/items?itemName=golang.go](https://marketplace.visualstudio.com/items?itemName=golang.go)

1. Открываем go-модуль (директорию, созданную после go mod init)
2. (Если в первый раз) Открываем Command Palette и вбиваем Locate Configured Go Tools. Предложит установить **gopls** (Это Language Server, который из текстового редактора может сделать IDE; с ним не надо взаимодействовать напрямую, это для редактора кода)
3. Проверяем, что The Go Status Bar установился: [https://github.com/golang/vscode-go/wiki/ui](https://github.com/golang/vscode-go/wiki/ui)
4. Доставляем dlv — дебаггер (local и remote): [https://github.com/golang/vscode-go/wiki/tools](https://github.com/golang/vscode-go/wiki/tools)

VSCode поддерживает оба способа организации модулей — GOPATH и go mod, но крайне рекомендуют последний. Если работаете с несколькими модулями, то настройте [Workspace](https://code.visualstudio.com/docs/editor/multi-root-workspaces).
