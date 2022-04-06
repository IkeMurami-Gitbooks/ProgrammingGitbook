# Classes

## Get type

```dart
print('The type of a is ${a.runtimeType}');
```

## Constructors

```dart
class Point {
  double x = 0;
  double y = 0;

  Point(double x, double y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}

// or

class Point {
  double x = 0;
  double y = 0;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

### Named constructor

```dart
const double xOrigin = 0;
const double yOrigin = 0;

class Point {
  double x = 0;
  double y = 0;

  Point(this.x, this.y);

  // Named constructor
  Point.origin()
      : x = xOrigin,
        y = yOrigin;
}


// using named constructor
class Employee extends Person {
  Employee() : super.fromJson(fetchDefaultData());
  // ···
}
```

### Initializer list

```dart
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, double> json)
    : x = json['x']!,
      y = json['y']! {
  print('In Point.fromJson(): ($x, $y)');
}

Point.withAssert(this.x, this.y) : assert(x >= 0) {
  print('In Point.withAssert(): ($x, $y)');
}
```

### Redirecting constructor

```dart
class Point {
  double x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(double x) : this(x, 0);
}
```

### Constant constructors

```dart
class ImmutablePoint {
  static const ImmutablePoint origin = ImmutablePoint(0, 0);

  final double x, y;

  const ImmutablePoint(this.x, this.y);
}
```

### Factory constructors

```dart
// factory - keyword indicate that constructor can to not return object of class 
// or subclass
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    return _cache.putIfAbsent(
        name, () => Logger._internal(name));
  }

  factory Logger.fromJson(Map<String, Object> json) {
    return Logger(json['name'].toString());
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

## Methods

```dart
class Point {
    int x = 0;
    int y = 0;
    double test(Point other);
    Point operator +(Point p) => Point(x + p.x, y + p.y);
    int get summa => x + y;
    set summa(s) => x = s - y;
}
```

## Abstract classes

```dart
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```

## Implicit interfaces

```dart
class Person {
  // In the interface, but visible only in this library.
  final String _name;
}

class Impostor implements Person {
  String get _name => '';
}
```

## Extending a class

```dart
class Some {
    void test() {}
}

class SomeA extends Some {
    void test() {
        super.test();
        // ...
    }
}
```

## Overriding methods

```dart
class Television {
  // ···
  set contrast(int value) {...}
}

class SmartTelevision extends Television {
  @override
  set contrast(num value) {...}
  // ···
}
```

Чтобы перехватить обращение к несуществующему методу, можно переопределить `noSuchMethod()`.

```dart
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: '
        '${invocation.memberName}');
  }
}
```

## Extension methods

```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
  // ···
}

'42'.parseInt();
```

## Enums

```dart
enum Color { red, green, blue }

assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

## Adding features to a class: mixins <a href="#adding-features-to-a-class-mixins" id="adding-features-to-a-class-mixins"></a>

`mixins` — специальные классы, расширяющие возможности другого класса. Подключаются через `with`.

```dart
class Musician extends Performer with Musical {
  // ···
}

mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

Можно задать, для какого класса доступно эта фича

```dart
class Musician {
  // ...
}
mixin MusicalPerformer on Musician {
  // ...
}
class SingerDancer extends Musician with MusicalPerformer {
  // ...
}
```

## Callable classes

Через реализацию метода `call()`.

```dart
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

var wf = WannabeFunction();
var out = wf('Hi', 'there,', 'gang');
```
