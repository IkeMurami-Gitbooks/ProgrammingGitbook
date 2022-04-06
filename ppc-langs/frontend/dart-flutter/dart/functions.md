# Functions

Короч все, как и в других языках. Краткие примеры:

```dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}

bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

## Именованные параметры

```dart
// При вызове функций, можно использовать именованные аргументы
enableFlags(bold: true, hidden: false);
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool? bold, bool? hidden}) {...}
const Scrollbar({Key? key, required Widget child})
```

## Необязательные параметры

```dart
String say(String from, String msg, [String? device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

## Значения по умолчанию

```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold = false, bool hidden = false}) {...}

// bold will be true; hidden will be false.
enableFlags(bold: true);
```

## Анонимные функции

```dart
const list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item');
});

list.forEach(
    (item) => print('${list.indexOf(item)}: $item'));
```

## Замыкания

```dart
Function makeAdder(int addBy) {
  return (int i) => addBy + i;
}

var add2 = makeAdder(2);
var add4 = makeAdder(4);

assert(add2(3) == 5);
assert(add4(3) == 7);
```
