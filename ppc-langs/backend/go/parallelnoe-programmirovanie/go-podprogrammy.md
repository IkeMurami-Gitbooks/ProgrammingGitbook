# go-подпрограммы

## About

Go-рутины — это легковесный управляемый поток. Go-рутины запускаются в одинаковых адресных пространствах, так что доступ к общей памяти должен быть синхронизирован. Пакет `sync` предоставляет примитивы, однако в Go и есть другие подходы.

## Запуск go-подпрограммы

```go
go функция(аргументы)
go func(параметры) {блок}(аргументы)
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
Advanced Go Concurrency Patterns: [https://talks.golang.org/2013/advconc.slide](https://talks.golang.org/2013/advconc.slide)\
