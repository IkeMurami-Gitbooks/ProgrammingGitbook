# Установка пакетов

Был такой эксперимент — [dev](https://golang.github.io/dep/), но он был закопан в пользу механизма модулей — [go mod](https://go.dev/ref/mod).

## Modules, packages and versions

A module is a collection of packages that are released, versioned, and distributed together. Modules may be downloaded directly from version control repositories or from module proxy servers.&#x20;

A module is identified by a [module path](https://go.dev/ref/mod#glos-module-path), which is declared in a [`go.mod` file](https://go.dev/ref/mod#go-mod-file), together with information about the module’s dependencies. The module root directory is the directory that contains the `go.mod` file. The main module is the module containing the directory where the `go` command is invoked.

Each package within a module is a collection of source files in the same directory that are compiled together. A package path is the module path joined with the subdirectory containing the package (relative to the module root). For example, the module `"golang.org/x/net"` contains a package in the directory `"html"`. That package’s path is `"golang.org/x/net/html"`.

## go.work

Определяет рабочую область — набор go модулей (по умолчанию, текущая директория — активная рабочая область)

Для работы с workspace  используем команды:

```
go work init
go work use
go work edit
```

Или `golang.org/x/mod/modfile` для выполнения подобных изменений программно.

## Build commands

All commands that load information about packages are module-aware. This includes:

* `go build`
* `go fix`
* `go generate`
* `go get`
* `go install`
* `go list`
* `go run`
* `go test`
* `go vet`

These commands accept the following flags, common to all module commands.

* The `-mod` flag controls whether `go.mod` may be automatically updated and whether the `vendor` directory is used.
  * `-mod=mod` tells the `go` command to ignore the vendor directory and to [automatically update](https://go.dev/ref/mod#go-mod-file-updates) `go.mod`, for example, when an imported package is not provided by any known module.
  * `-mod=readonly` tells the `go` command to ignore the `vendor` directory and to report an error if `go.mod` needs to be updated.
  * `-mod=vendor` tells the `go` command to use the `vendor` directory. In this mode, the `go` command will not use the network or the module cache.
  * By default, if the [`go` version](https://go.dev/ref/mod#go-mod-file-go) in `go.mod` is `1.14` or higher and a `vendor` directory is present, the `go` command acts as if `-mod=vendor` were used. Otherwise, the `go` command acts as if `-mod=readonly` were used.
* The `-modcacherw` flag instructs the `go` command to create new directories in the module cache with read-write permissions instead of making them read-only. When this flag is used consistently (typically by setting `GOFLAGS=-modcacherw` in the environment or by running `go env -w GOFLAGS=-modcacherw`), the module cache may be deleted with commands like `rm -r` without changing permissions first. The [`go clean -modcache`](https://go.dev/ref/mod#go-clean-modcache) command may be used to delete the module cache, whether or not `-modcacherw` was used.
* The `-modfile=file.mod` flag instructs the `go` command to read (and possibly write) an alternate file instead of `go.mod` in the module root directory. The file’s name must end with `.mod`. A file named `go.mod` must still be present in order to determine the module root directory, but it is not accessed. When `-modfile` is specified, an alternate `go.sum` file is also used: its path is derived from the `-modfile` flag by trimming the `.mod` extension and appending `.sum`.

## Vendoring

По умолчанию, модули грузятся в кэш, но если мы хотим поработать со старыми пакетами, мы можем зафризить в папку vendor определенные версии пакетов:

```
go mod vendor
```

Это создаст папку vendor и туда скопирует все пакеты для разработки и тестов. Так же будет создан файл `vendor/modules.txt`, который будет содержать версии пакетов. Можем их посмотреть:

```
go list -m
go version -m
```

Эти команды сравнят версии в vendor с версиями в go.mod. Если они совпадут, вылетит ошибка. Надо будет ввести `go mod vendor` снова.

Начиная с Go 1.14, поддержка vendor включена по умолчанию. Это значит, что `go build` и `go test` будут подгружать модули из vendor, а не идти в интернет или локальный кэш пакетов. go-команды игнорируют любые другие директории vendor, кроме той, что в корневом модуле.

## go get

Обновление модулей в go.mod.

## go install

Установка модулей

## go mod download

Загрузка модуля в кэш модулей

## go list -m

## go mod edit

Редактирование и форматирование go.mod.

## go mod graph

Граф зависимостей

## go mod init

Инициализация модуля в директории

## go mod tidy

## go mod vendor

Подтягивание в папку vendor зависимостей для сборки и теста проекта

## go mod verify

Проверка, что все зависимости разрешены

##
