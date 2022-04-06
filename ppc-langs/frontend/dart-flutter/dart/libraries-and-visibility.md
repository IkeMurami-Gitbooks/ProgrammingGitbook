# Libraries and visibility

#### Usage

```dart
import 'dart:html';  // dart: — Импорт built-in библиотек
import 'package:test/test.dart';  // package: — импорт всех остальных библиотек по имени 
                                  // или по относительному пути
import 'dart:html' as myname;

// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```

Отложенная подгрузка библиотеки (deferred loading or lazy loading). Используется для скорейшей подгрузки библиотек, необходимы для старта приложения и чтобы не грузить редко используемые библиотеки.

```dart
import 'package:greetings/hello.dart' deferred as hello;

Future<void> greet() async {
  // load library when you need via .loadLibrary()
  await hello.loadLibrary();
  hello.printGreeting();
}
```

#### Implementing libraries <a href="#implementing-libraries" id="implementing-libraries"></a>

See [Create Library Packages](https://dart.dev/guides/libraries/create-library-packages) for advice on how to implement a library package, including:

* How to organize library source code.
* How to use the `export` directive.
* When to use the `part` directive.
* When to use the `library` directive.
* How to use conditional imports and exports to implement a library that supports multiple platforms.
