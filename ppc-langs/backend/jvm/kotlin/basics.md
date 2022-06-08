# Basics

## Documentation

Документация (Language Guide): [https://kotlinlang.org/docs/reference/exceptions.html](https://kotlinlang.org/docs/reference/exceptions.html)

## Packages

```kotlin
package ctf.zone.pwn  
// Имя пакета указывается в начале исходного файла, так же как и в Java
// Но в отличие от Java, нет необходимости, чтобы структура пакетов совпадала со структурой папок: 
// исходные файлы могут располагаться в произвольном месте на диске.


import java.lang.IllegalStateException
import java.util.*
import java.io.File
```

## Vars, constants and comments

```kotlin
val PI = 3.14 // test comment
var global_var = 0

/* Блочный комментарий из
нескольких строк */
```

## Classes and Functions

### Functions

```kotlin
class KotlinClass {
    fun sum(a: Int, b: Int): Int {  // Функция принимает два аргумента Int и возвращает Int
        return a + b
    }

    fun sum(a: Int, b: Double) = a + b // Функция с выражением в качестве тела и автоматически определенным типом возвращаемого значения

    fun printSum(a: Int, b: Int): Unit {  // Функция, не возвращающая никакого значения (void в Java):
        print(a + b)
    }

    fun printSum(a: Int, b: Double) {  // Тип возвращаемого значения Unit может быть опущен
        print(a + b)
        println("jsfvkjsdfg")
    }

    fun variables() {
        // Неизменяемая (только для чтения) внутренняя переменная
        val a: Int = 1
        val b = 1   // Тип `Int` выведен автоматически
        val c: Int  // Тип обязателен, когда значение не инициализируется
        c = 1       // последующее присвоение


        // Изменяемая переменная
        var x = 5 // Тип `Int` выведен автоматически
        x += 1

        // Изменение глобальных переменных
        global_var += 1
    }
}
```

### Static Functions

```kotlin
// Static functions
class MainActivity : AppCompatActivity() {
    companion object {
        // PARAM_RESULT — Static Field
        val PARAM_RESULT: String = "com.zfr.network.extra.RESULT" 
        fun test() {
            
        }
    }    
}
```

### Extensions

Это функции-расширения

```kotlin
// Функции-расширения
fun String.spaceToCamelCase() { return }
"Convert this to camelcase".spaceToCamelCase()
```

## Strings



### Шаблоны

```kotlin
fun string_patterns(args: Array<String>, test: Any) {
        /*
        Допустимо использование переменных внутри строк в формате $name или ${name}
         */
        if (args.size == 0) return
        else print("kjkfgd")

        print("Первый аргумент: ${args[0]}")

        var a = 1
        // просто имя переменной в шаблоне:
        val s1 = "a равно $a"

        a = 2
        // произвольное выражение в шаблоне:
        val s2 = "${s1.replace("равно", "было равно")}, но теперь равно $a"

        /*
          Результат работы программы:
          a было равно 1, но теперь равно 2
        */
}
```

### Дополнить строку символами

```kotlin
val name = "Barsik"
val pad = name.padStart(10, '#')
println(pad) // ####Barsik

val name = "Barsik"
val pad = name.padEnd(10, '*')
println(pad) // Barsik****
```

### Подстроки

```kotlin
// подстрока с указанного индекса
val result = "developer.alexanderklimov.ru".substring(10) // alexanderklimov.ru

// подстрока до первого указанного разделителя
val first = "developer.alexanderklimov.ru".substringBefore('.') // developer

// подстрока после первого указанного разделителя
val last = "developer.alexanderklimov.ru".substringAfter('.') // alexanderklimov.ru

// подстрока после последнего указанного разделителя
val last = "developer.alexanderklimov.ru".substringAfterLast('.') // ru

// подстрока до последнего указанного разделителя
val beforeLast = "developer.alexanderklimov.ru".substringBeforeLast('.') // developer.alexanderklimov
```

### Встроенные функции

```kotlin
val blank = "   ".isBlank() // true, если пустая строка или пустые символы пробела, табуляции и т.п.

// индекс последнего символа
val lastIndex = "Кот Мурзик".lastIndex // 9

// переводим в верхний регистр первый символ строки
// decapitalize() выполняем обратную задачу
val capitalize = "кот Мурзик".capitalize()

val withSpaces = "1".padStart(2) // добавляем пробел перед строкой
val endZeros = "1".padEnd(3, '0') // "100"  добавляем нули в конец

val dropStart = "Kotlin".drop(2) // "tlin" убираем первые символы в указанном количестве
val dropEnd = "Kotlin".dropLast(3) // "Kot" убираем последние символы в указанном количестве

// возвращаем строку без первого символа, который удовлетворяет условию
val string = "Мурзик"
val result = string.dropWhile{
    it == 'М'
}
textView.text = result // урзик

// возвращаем строку без последнего символа, который удовлетворяет условию
val string = "Мурзик"
val result = string.dropLastWhile{
    it == 'к'
}
textView.text = result // Мурзи

// разбиваем на массив строк
"A\nB\nC".lines() // [A, B, C]
"ABCD".zipWithNext() // [(A, B), (B, C), (C, D)]

// удаляем символы из заданного диапазона
val string = "Кот, который гулял сам по себе"
val result = string.removeRange(
    3..28 // range
)

// удаляем префикс из строки
val string = "Кот, который гулял сам по себе"
val result = string.removePrefix("Кот")

// удаляем суффикс из строки
val string = "Кот, который гулял сам по себе"
val result = string.removeSuffix("себе")

// удаляем заданный разделитель, который должен окружать строку с начала и с конца
val string = "та, тра-та-та, мы везём с собой кота"

val result = string.removeSurrounding(
    "та" // delimiter
)

textView.text = result // , тра-та-та, мы везём с собой ко

// Также можно указать разные начало и конец, которые окружают строку
val string = "Тра-та-та, тра-та-та, мы везём с собой кота"

val result = string.removeSurrounding(
    "Тра-", // prefix
    " кота" // suffix
)

textView.text = result // та-та, тра-та-та, мы везём с собой


// Добавляем отступы при явном переводе на новую строку
val string = "Какой-то длинный текст, \nсостоящий из имён котов: " +
        "\nВаська" +
        "\nБарсик" +
        "\nРыжик"

val result = string.prependIndent(
    "     " // indent
)

// Разбиваем символы на две группы. 
// В первую группу попадут символы в верхнем регистре, во вторую - символы в нижнем регистре
val string = "Кот Васька и кот Мурзик - Друзья!"

val result: Pair<String, String> = string.partition {
    it.isUpperCase()
}

textView.text = result.first + ":" + result.second //КВМД:от аська и кот урзик - рузья!

// Разбиваем строку на список строк. В качестве разделителя - перевод на новую строку
val string = "Кот Васька\nКотМурзик\nКот Мурзик"

// Split string into lines (CRLF, LF or CR)
val lines: List<String> = string.lines()

textView.text = "Кол-во строк: ${lines.size}"
lines.forEach {
    textView.append("\n" + it)
}

// Разбиваем строку на список строк с указанным числом символов. В последней строке может выводиться остаток
val string = "Тра-та-та, тра-та-та, мы везём с собой кота"
val list:List<String> = string.chunked(11)
list.forEach {
    textView.append("$it\n")
}

/*
Тра-та-та, 
тра-та-та, 
мы везём с 
собой кота
*/

// Содержит ли строка только цифры (используем предикат)
val string = "09032020"

// Returns true if all characters match the given predicate.
val result: Boolean = string.all{
    it.isDigit()
}
textView.append("Is the string $string contain only digit? $result")

// Содержит ли строка хотя бы одну цифру (используем предикат)
val string = "3 кота"
// Returns true if at least one character matches the given predicate.
val result: Boolean = string.any() {
    it.isDigit()
}
textView.append("Is the text \"$string\" contain any digit? $result")

// Сравниваем две строки с учётом регистра
val string1 = "This is a sample string."
val string2 = "This is a SAMPLE string."

if (string1.compareTo(string2, ignoreCase = true) == 0) {
    textView.append("\n\nBoth strings are equal, ignoring case.")
} else {
    textView.append("\n\nBoth strings are not equal, ignoring case.")
}
```

Можно фильтровать данные с помощью **filter()**. Допустим, мы хотим извлечь только цифры из строки.

```

val string = "9 жизней (2016) - Nine Lives - информация о фильме"
val filteredText = string.filter { it.isDigit() }
textView.text = filteredText // 92016
```

Если хочется решить обратную задачу и получить только символы, но не цифры - то достаточно вызвать **filterNot()**.

```

val filteredText = string.filterNot { it.isDigit() }
```

## Циклы, ветвления, групповые операции

```kotlin
// Использование циклов
for (arg in args)
    print(arg)

for (i in args.indices)
    print(args[i])

var i = 0
while (i < args.size) {
    print(args[i++])
}

// Использование switch (здесь он when называется)
when (test) {
    1 -> print("123")
    "Hello" -> print("jvcbsdjfv")
    is Long -> print("sjkdvnksdv")
    !is String -> print("sdfsdg")
    else -> print("Unknown")
}

// Использование интервалов
if (i in 1..5)
    print("OK")
if (i !in 1..5)
    print("FAIL")
for (i in 1..5 step 1)
    print(i)
    
// триарные операторы
fun max(a: Int, b: Int) = if (a > b) a else b

// return с оператором when
val color = "sfgsdg"
var test = when (color) {
    "Red" -> 0
    "Green" -> 1
    "Blue" -> 2
    else -> throw IllegalArgumentException("Invalid color param value")
}

// try/catch
val result = try {
    print("lkdfjvlkdfjvlkdfg")
} catch (e: ArithmeticException) {
    throw IllegalStateException(e)
}

// Вызов нескольких объектов через with
/*
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

val myTurtle = Turtle()
with(myTurtle) { //draw a 100 pix square
    penDown()
    for(i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
*/
```

## Nullable



```kotlin
1. Elvis operator "?:" - вернет значение или сделает действие (return)
test = value ?: return

2. Use safee call - выполнится если не null
left?.let { node -> queue.add(node) }
left?.let { queue.add(it) }
left?.let(queue::add)

3. 
```

```kotlin
// Nullable-значения и проверка на null
fun var_null_and_check(str: Any): Int? {
    // Возвращает null если str не содержит числа
    // Ссылка должна быть явно объявлена как nullable (символ ?) когда она может принимать значение null.
    val x = null
    if (str is String || str == null)
        return 1
        //return str.length
    else {
        if (str !is String)
            return 1
        return 0
    }
}
```



```kotlin
// Сокращение для "Если не null"
val files = File("Test").listFiles()
println(files?.size)

// Сокращение для "Если не null, иначе"
println(files?.size ?: "empty")

// Вызов оператора при равенстве null
/*
val email = name["email"] ?: throw IllegalStateException("Email is missing!")
*/

// Выполнение при неравенстве
/*
val data = ...

data?.let {
    ... // execute this block if not null
}
*/

```

## Коллекции

```kotlin
fun collections() {
        val items = setOf("apple", "banana", "kiwi")
        // Использование лямбда-выражения для фильтрации и модификации коллекции
        items
            .filter { it.startsWith("A") }
            .sortedBy { it }
            .map { it.toUpperCase() }
            .forEach { print(it) }

        // Read-only ассоциативный список (map)
        var map = mapOf("a" to 1, "b" to 2, "c" to 3)

        // Итерация по карте/списку пар
        for ((k, v) in map) {
            println("$k -> $v")
        }

        // Обращение к списку
        println(map["key"])

        // read-only list
        val list = listOf("a", "b", "c")
    }
```

## Ленивые свойства

Будут вычислены при обращении

```kotlin
// Ленивые свойства
val p: String by lazy {
    // compute the string
    return@lazy "sdfsdf"
}
```

## Синглтон

```kotlin
// Создание синглтона
/*
object Resource {
    val name = "Name"
}
*/
```
