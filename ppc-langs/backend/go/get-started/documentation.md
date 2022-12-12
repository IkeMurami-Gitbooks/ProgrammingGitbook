# Документация

## Оф документация

Оф веб-сайт: [https://golang.org/](https://golang.org/)&#x20;

Структура сайта

* Packages: документация ко всем пакетам из стандартной библиотеки Go
* Tour of Go: Basic examples [https://tour.golang.org/](https://tour.golang.org/)
* How to write Go code: [https://golang.org/doc/code.html](https://golang.org/doc/code.html)
* References->Command Documentation: документация к программам, распространяемым вместе с Go (компиляторы, инструменты сборки и др)
* References->Language Specification: неоф и весьма полная спецификация языка
* Documents->Effective Go: документ, описывающий многие приемы программирования на Go

## Локальная документация

```
Будет локально поднята документация как оф сайт:
win:  C:\>godoc -http=:8000
unix: $ godoc -http=:8000

Документация в консоли:
Описание функции image.NewRGBA(): $ godoc image NewRGBA
Описание пакета image/png: $ godoc image/png
```

## Docs on github

На github (выглядит как наидболее полная дока): [https://github.com/golang/go/wiki/](https://github.com/golang/go/wiki/)
