# Отображения

## Объявления

```go
make(map[ТипКлюча]ТипЗначения, начальнаяЕмкость)
make(map[ТипКлюча]ТипЗначения)
map[ТипКлюча]ТипЗначения {}
map[ТипКлюча]ТипЗначения{ключ1: значение1, ключ2: значение2, ..., ключN: значениеN}
```

## Операции над отображениями

### Поиск

```go
if population, found := populationForCity[city]; found {
    fmt.Printf("%s’s population is %d\n", city, population) 
} 
else {
    fmt.Printf("%s’s population data is unavailable\n", city) 
}
```

