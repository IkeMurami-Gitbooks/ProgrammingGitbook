# go-подпрограммы

## About

Go-рутины — это легковесный управляемый поток. Go-рутины запускаются в одинаковых адресных пространствах, так что доступ к общей памяти должен быть синхронизирован. Пакет `sync` предоставляет примитивы, однако в Go и есть другие подходы.

## Запуск go-подпрограммы

```go
go функция(аргументы)
go func(параметры) {блок}(аргументы)
```

Например:

```go
package main

import "fmt"

func printHello() {
	fmt.Println("Hello from printHello")
}

func main() {
	// Встроенная Go-рутина
	go func() {
		fmt.Println("Hello from inline")
	}()

	// Функция как Go-рутина
	go printHello()

	// Функция в главном потоке
	println("Hello from main")
}

```

Если вызвать такую программу, выведется в консоль только Hello from main, так как main-функция не дождалась выполнения go-рутины. Тут нам понадобятся каналы:

```go
package main

import "fmt"

// Печатает на стандартный вывод и отправляет int в канал
func printHello(ch chan int) {
    fmt.Println("Hello from printHello")
    // Посылает значение в канал
    ch <- 2
}

func main() {
    // Создаем канал. Для этого нам нужно использовать функцию make
    // Каналы могут быть буферизированными с заданным размером:
    // ch := make(chan int, 2), но это выходит за рамки данной статьи.
    ch := make(chan int)

    // Встроенная горутина. Определим функцию, а затем вызовем ее.
    // Запишем в канал по её завершению
    go func(){
        fmt.Println("Hello inline")
        // Отправляем значение в канал
        ch <- 1
    }()

    // Вызываем функцию как горутину
    go printHello(ch)
    fmt.Println("Hello from main")

    // Получаем первое значение из канала
    // и сохраним его в переменной, чтобы позже распечатать
    i := <- ch
    fmt.Println("Received ",i)

    // Получаем второе значение из канала
    // и не сохраняем его, потому что не будем использовать
    <- ch
}
```

## sync.Mutex

Для блокирования каких-то объектов для горутин (для синхронизации).

## Пример запуска операций в несколько потоков (горутин)

```go
sources := []string{"a", "b", "c"}

var wg sync.WaitGroup
semaphore := make(chan struct{}, 10)  // Семафор, ограничивающий количество потоков

for _, source := range sources {
    wg.Add(1)
    go func(wg *sync.WaitGroup, semaphore chan struct{}, source string) {
        semaphore <- struct{}{}
     		defer func() {
     		    <- semaphore
     		    wg.Done()
     		}()
     
        // делаю что-нибудь c source
     
        return
        
    }(&wg, semaphore, source)
}

wg.Wait()
```

## Papers

Как правильно организовать (паттерны) многопоточное приложение:\
Go Concurrency Pattern: [https://talks.golang.org/2012/concurrency.slide](https://talks.golang.org/2012/concurrency.slide)\
Advanced Go Concurrency Patterns: [https://talks.golang.org/2013/advconc.slide](https://talks.golang.org/2013/advconc.slide)<br>
