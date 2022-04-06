# Variables

## Init

```dart
var name = 'Test';
Object name = 'Test';

var hex = 0xDEADBEEF;
var number = 1;
double d = 0.1;
num x = 1;  // x — is double and int
x += 0.1;

String name = 'Test';

int? lineCount;  // default: null
assert(lineCount == null);
```

## Late/Lazy

Dart подсветит, если посчитает, что вы объявили переменную (не nullable). Чтобы указать, что переменная будет проинициализирована позже, можно пометить ее ключевым словом `late`.

```dart
late String descr;

void main() {
    descr = 'Test';
    print(descr);
}
```

`late` используют в двух случаях:

* Если инициализация переменной слишком дорогостоящая операция
* Мы инициализируем инстанс переменной, и этот инициализатор нуждается в доступе к `this` (то есть, переменная должна быть проинициализирована после вызова конструктора и инициализации всего объекта).

По факту, инициализация произойдет только при обращении к переменной (`lazy`)

```dart
late String temperature = _readThermometer();  // Lazily initialized
```

## Const & Final

Если вы не собираетесь менять значение переменной, объявляем ее как final. const — это частный случай final-переменной, она получает свое значение на этапе компиляции.

```dart
const bar = 1000000;
const double atm = 1.01325 * bar;
final name = 'Bob';
final String nickname;
nickname = 'Bob';

var foo = const [];
final bar = const [];
const baz = []; // Equivalent to `const []`
```

## Build-in types

* int, double
* String
* bool
* List
* Set
* Map
* Runes (умные char'ы)
* Symbol
* null (класс Null)

Другие типы, имеющие специальные роли в Dart:

* `Object` — суперкласс для всех других классов в Dart, за исключением `Null`
* `Future` и `Stream` — используется в асинхронных функциях
* `Iterable` — используется в циклах `for` и в функциях-генераторах
* `Never` — индикатор того, что функция может никогда не вернуть своего результата (например, из-за исключения)
* `dynamic` —  индикатор того, что мы хотим отключить статические проверки. Обычно, лучше использовать `Object` или `Object?`.
* `void` — индикатор того, что значение никогда не используется. Часто используется для типа возвращаемого значения.

## Converting

```dart
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
```

## Strings

```dart
var s = 'some string';

var s1 = 'some s=$s';
var s2 = 'some s=${s}';

var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";

// Create row string — r; Runes
var s = r'In a raw string, not even \n gets special treatment.';
```

## Lists

```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);

var constantList = const [1, 2, 3];
// constantList[1] = 1; // This line will cause an error.
```

### Spread operator (...)

```dart
var list = [1, 2, 3];
var list2 = [0, ...list];
assert(list2.length == 4);


var list;
var list2 = [0, ...?list];
assert(list2.length == 1);
```

### if и for

```dart
var nav = [
  'Home',
  'Furniture',
  'Plants',
  if (promoActive) 'Outlet'
];


var listOfInts = [1, 2, 3];
var listOfStrings = [
  '#0',
  for (var i in listOfInts) '#$i'
];
assert(listOfStrings[1] == '#1');
```

## Sets

```dart
var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};

var names = <String>{};
// Set<String> names = {}; // This works, too.
// var names = {}; // Creates a map, not a set.
```

## Maps

```dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

var gifts = Map<String, String>();

```
