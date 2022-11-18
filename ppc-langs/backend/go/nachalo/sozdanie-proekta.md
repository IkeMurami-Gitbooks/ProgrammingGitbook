# Создание проекта

## Структура проекта

Package — набор скриптов и файлов, находящихся в одной директории или отнесенных к одному пакету через директиву package (в одной директории не может быть двх разных пакетов). Все классы, объекты, типы и тп видны в рамках одного пакета. Go программы организуются в такие пакеты

Module — коллекция пакетов. Репозитории состоят из модулей (обычно только один модуль находящийся в корне (/) репозитория). Файл go.mod определяет module path — import path prefix для всех пакетов внутри модуля.&#x20;

```
package/
    mod1/
        go.mod
    mod2/
        go.mod
    ...
    
или

package/
    go.mod
```

Что еще может быть в Go-проекте:

```
package/
    vendor/ — сюда будут складываться зависимости
    tests/ — для тестов
    tools/ - для кодогенерации
        tools.go - инструменты, которые запускаются на этапе сборки проекта
        generate.go - кодогенерация (например, protobuf'ы)
    configs/ — для конфигов
    cmd/ — для кода, который запускается через командную строкую. Грубо говоря для бинарников
    pkg/ — для всего кода (в том числе и того, который экспортируемый)
    internal/ — код, который мы не позволяем экспортировать: https://golang.org/cmd/go/#hdr-Internal_Directories
    build/ — для докерфайлов и всего, что нужно для билда
        migrations/ — для миграции с версии на версию баз
        package/ — для сборки пакета
    bin/ — куда бинари будут класться собранные
    service/ — клиенты к другим сервисам (kafka, rabbit, текущий сервис)
    .gitignore — что не должно попадать в репозиторий (конфиги например)
    .dockerignore — что не должно попасть в докер контейнер (конфиги например)
    README.md — описание
    Makefile
    ...
    другие ci штуки
```

## Создание проекта

```
$ mkdir project
$ cd project
$ go mod init project
 или
$ go mod init github.com/z3f1r/test-go-project
$ cat go.mod
```

### go.mod

Инициализируем go.mod файл. Показывает, что вы создаете модуль и ваш код может быть использован другим кодом. Так же этот файл хранит зависимости.

```
go mod init <project-name>

Например:
go mod init test_project
go mod init mysite.com/project-name
go mod init github.com/tests/ttests
```

Допустим, что вы (`main_project`) имеете зависимость от локального проекта (`test_project`), тогда в go.mod добавляем строчку:

```
module main_project

go 1.14

replace mysite.com/test_project => ../test_project
```

Теперь в коде можно использовать еще неопубликованный проект.

Или то же, с помощью go mod:

```
$ go mod edit -replace github.com/pselle/bar=/Users/pselle/Projects/bar
```

### Добавление зависимостей

После создания go.mod (-u означает добавление не в GOPATH, а локально):

```
$ go get -u golang.org/x/lint/golint
```

### Очистка от лишних пакетов

Перед созданием релиза рекомендуется очистить проект от лишних зависимостей, которые не используются

```
$ go mod tidy
$ cat go.mod
```

### tools.go

Не знаю для чего. Возможно, для импорта необходимых для сборки бинарей/инструментов.

Пример

```go
// +build tools

package main

import (
	_ "github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2"
)
```

`// +build tools` — **build constraint** a.k.a. **build tag**

### generate.go

Отвечает за кодогенерацию — то, что будет создано динамически перед go build

```go
package main

//go:generate scripts/protobuf.sh
//go:generate go install github.com/some/package
//go:generate protoc -I=. --go_out=. --go_opt=paths=source_relative ./protocol/some.proto
```
