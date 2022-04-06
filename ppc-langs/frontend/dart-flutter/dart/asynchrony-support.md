# Asynchrony support

Dart библиотеки все асинхронные, все обернуто в Future и Stream объекты.

```dart
Future<String> lookUpVersion() async => '1.0.0';

await lookUpVersion();

Future<void> checkVersion() async {
  var version = await lookUpVersion();
  // Do something with version
}
```

Handling Streams

Есть два способа получить значения из Stream:

* Use `async` and an _asynchronous for loop_ (`await for`).
* Use the Stream API, as described [in the library tour](https://dart.dev/guides/libraries/library-tour#stream).

await for использовать только в том случае, если мы хотим дождаться все результаты из Stream. Во многих случаях это лучше не использовать: например, в UI Stream бесконечен.

```dart
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```

For more information about asynchronous programming, in general, see the [dart:async](https://dart.dev/guides/libraries/library-tour#dartasync---asynchronous-programming) section of the library tour.
