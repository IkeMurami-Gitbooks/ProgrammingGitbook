# Функции и методы

## init() и main()

Любой фрагмент программного кода на языке Go должен быть включен в пакет, а каждая программа должна иметь пакет main с функцией main().

Пакеты на языке Go могут иметь функции init() - они все выполнятся перед main(). Причем, функции init, как и main, можно не вызывать специально.&#x20;

## Функции

```go
func имяФункции (необязательныеПараметры) типНеобязательногоВозвращаемогоЗначения {
    тело
}
func имяФункции (необязательныеПараметры) (необязательныеоВозвращаемыеЗначения) {
    тело
}
```

### Неопределенное количество аргументов

может быть функция с неопределенным количеством параметров

```go
func test(a int, b ...int) {
    Здесь будет b []int
}
```

### Именованные возвращаемыее значения

Возвращаемые значения могут быть именованные и неименованные. Смешивать нельзя. Именованные возвращаемые значения рекомендуется использовать только в коротких функциях (чтобы не ухудшать читаемость кода).

Пример:

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```

На текущий момент компиляторы не способны распознать return в секции default или else безусловном. Принято в таких случаях использовать panic("unreachable")

### Передача параметров из одной функции в другую

```go
// Обрати вниманиее на компактное указание одинаковых типов
func test(a, b, c int) bool {
    return true
}
func test1(r int) (int, int, int) {
    return 1, 2, 3
}
dont := test(test1(1))
```

### Необязательные параметры

В Go нет поддержки необязательных параметров. Однако, этого можно добиться использованием структур, которые инициализируются по умолчанию.

```go
type Options struct {
    First int
    Audit bool
    ErrorHandler func(item Item)
}
func test(items Items, options Options) {
    ...
}

test(items, Options{})
test(items, Options{ErrorHandler: errorHandler})
```

### Выбор функции во время выполнения

#### Ветвление с помощью отображений и ссылок на функции

```go
var FunctionForSuffix = map[string]func(string) ([]string, error) {
    ".gz": GzipFileList,
    ".tar": TarFileList,
    ...
}
```

## Методы

### Примеры

Пример метода, переопределяющего функцию len для типа Stack. Имя stack (может быть любым) в терминах Go называется _приемником_ (в других языках это имя фиксировано - this, self,..).

```go
func (stack Stack) Len() int {
    return len(stack)
}
```

Если метод должен изменять значение приемника, его необходимо определить как указатель. В противном случае, в метод передается копия объекта.

```go
func (obj *Test) Method(x interface{}) {
    someLocalObj := *obj
    *obj = append(*obj, x)
}
```

Возвращение нескольких значений

```go
func (test Test) Top() (interface{}, error) {
    if len(test) == 0 {
        return nil, errors.New("blabla")
    }
    return test, nil
}
```

### Ограничения

Имена всех методов должны быть уникальны в пределах одного типа. Следовательно, нельзя создать два одинаковых метода - один для значения, другой для указателя.

Отсутствует поддержка перегруженных методов. Один способов реализовать подобие перегруженных методов - использовать методы с переменным числом аргументов, однако в Go принято использовать функции с уникальными именами. Например тип strings.Reader представляет три разных метода чтения: strings.Reader.Read(), strings.Reader.ReadByte(), strings.Reader.ReadRune().&#x20;

Множество методов значения - все методы этого типа, принимающие приемник по значению.\
Множество методов указателя - все методов этого типа, принимающий приемник по значению и по указателю!

### Переопределение методов

```go
type Item struct {
    id int
}
func (item *Item) GetID() int {
    return id
}
type SpecialItem struct {
    Item
    catalogId int
}
func (item *SpecialItem) GetID() int {
    return item.Item.GetID() + item.catalogId
}

item := Item{1}
special := SpecialItem{Item{2}, 3}
/*
item.id == 1
special.id == 2
special.catalogId == 3
special.GetID() == 5
special.Item.GetID() == 2
*/

```

### Конструкторы

Конструкторы - это функции, возвращающие проинициализированне значение. Их принято называть New...

```go
type Place struct {
    cost int        // private
    Name string     // public
}
func New(name string) *Place {
    return &Place{0, name}
}
func NewPlaceWithCost(name string, cost int) *Place {
    return &Place{cost, name}
}
func (place Place) Cost() int {
    // сделать необходимые проверки
    return place.cost
}
```

## Анонимная функция (замыкания)

Пример:

```go
some.Func(func(word string) string { return word })
```

Для чего используются

### Создание функций-оберток

```go
addPng := func(name string) string { return name + ".png" }
```

### Создание фабричных функций

фабричные функции - такие функции, которые возвращают другие функции

```go
func MakeAddSuffix(suffix string) func(string) string {
    return func(name string) string {
        if !strings.HasSuffix(name, suffix) {
            return name + suffix
        }
        return name
    }
}

addZip := MakeAddSuffix(".zip")
fmt.Println(addZip("filename"))
```
