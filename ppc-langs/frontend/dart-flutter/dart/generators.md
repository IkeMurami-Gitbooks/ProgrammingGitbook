# Generators

два типа генераторов:

* **Synchronous** generator: Returns an [`Iterable`](https://api.dart.dev/stable/dart-core/Iterable-class.html) object.
* **Asynchronous** generator: Returns a [`Stream`](https://api.dart.dev/stable/dart-async/Stream-class.html) object.

```dart
// Sync
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}

// Async
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```

Если генератор рекурсивный, используем `yield*`.

```dart
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
```
