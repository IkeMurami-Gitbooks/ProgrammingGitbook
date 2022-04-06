# Control flow statements

* `if` and `else`
* `for` loops
* `while` and `do`-`while` loops
* `break` and `continue`
* `switch` and `case`
* `assert`

## if / else

```dart
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```

## for / for .. in ..

```dart
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}

var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}
callbacks.forEach((c) => c());

for (final candidate in candidates) {
  candidate.interview();
}
```

## while / do .. while

```dart
while (!isDone()) {
  doSomething();
}

do {
  printLine();
} while (!atEndOfPage());
```

## switch .. case

```dart
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}

var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Empty case falls through.
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}

var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
  // Continues executing at the nowClosed label.

  nowClosed:
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```

## assert

```dart
assert(urlString.startsWith('https'),
    'URL ($urlString) should start with "https".');
```
