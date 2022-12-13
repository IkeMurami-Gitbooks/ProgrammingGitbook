# Общепризнанные

## Cobra & Viper

Эти два пакета встречаются повсеместно. [Cobra](https://github.com/spf13/cobra) — разработка CLI для программы, [Viper](https://github.com/spf13/viper) — работа с конфигурационными файлами.

### Cobra

#### Install

Зависимость в проект:

```
go get -u github.com/spf13/cobra@latest
```

Затем в коде:

```go
import "github.com/spf13/cobra"
```

Очень помогает управление через Cobra CLI:

```
go install github.com/spf13/cobra-cli@latest
```

#### Cobra CLI: Init Project

После того как мы создали проект:

```
cd $HOME/code 
mkdir myapp
cd myapp
go mod init github.com/spf13/myapp
```

Заходим и инициализируем Cobra:

```
cobra-cli init [--author "Ike Murami my@email.com"] [--license GPLv3] [--viper]
```

Команда создаст `main.go` файл и файлы для запуска в директории `cmd`.

#### Cobra CLI: Add commands to a project

Добавляем команды:

```
cobra-cli add serve
cobra-cli add config
```

Добавить подкоманду команде config:

```
cobra-cli add create -p 'configCmd'
```

Теперь мы можем использовать эти команды:

```
go run main.go
go run main.go serve
go run main.go config
go run main.go config create
go run main.go helpe serve
```

Далее идем в файлы команд в cmd и кастомизируем их под себя, подробнее в [The Cobra User Guide](https://github.com/spf13/cobra/blob/master/user\_guide.md#using-the-cobra-library).

### Viper



## Others

Aurora — вывод в консоль красками [https://github.com/logrusorgru/aurora](https://github.com/logrusorgru/aurora)

Atomic — обертка над базовыми типами: [https://pkg.go.dev/go.uber.org/atomic](https://pkg.go.dev/go.uber.org/atomic)

```
go get -u go.uber.org/atomic@v1
```

