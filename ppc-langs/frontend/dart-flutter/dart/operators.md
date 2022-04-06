# Operators

## Arithmetic

```dart
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5); // Result is a double
assert(5 ~/ 2 == 2); // Result is an int
assert(5 % 2 == 1); // Remainder

assert('5/2 = ${5 ~/ 2} r ${5 % 2}' == '5/2 = 2 r 1');


int a;
int b;

a = 0;
b = ++a; // Increment a before b gets its value.
assert(a == b); // 1 == 1

a = 0;
b = a++; // Increment a AFTER b gets its value.
assert(a != b); // 1 != 0

a = 0;
b = --a; // Decrement a before b gets its value.
assert(a == b); // -1 == -1

a = 0;
b = a--; // Decrement a AFTER b gets its value.
assert(a != b); // -1 != 0
```

## Equality and relational <a href="#equality-and-relational-operators" id="equality-and-relational-operators"></a>

```dart
assert(2 == 2);
assert(2 != 3);
assert(3 > 2);
assert(2 < 3);
assert(3 >= 3);
assert(2 <= 3);
```

## Type test

```dart
(employee as Person).firstName = 'Bob';

if (employee is Person) {
  // Type check
  employee.firstName = 'Bob';
}

if (employee is! Customer) { }
```

## Assignment <a href="#assignment-operators" id="assignment-operators"></a>

```dart
// Assign value to a
a = value;
// Assign value to b if b is null; otherwise, b stays the same
b ??= value;
```

## Logical

```dart
if (!done && (col == 0 || col == 3)) {
  // ...Do something...
}
```

## Bitwise and shift <a href="#bitwise-and-shift-operators" id="bitwise-and-shift-operators"></a>

```dart
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // Shift left
assert((value >> 4) == 0x02); // Shift right
assert((value >>> 4) == 0x02); // Unsigned shift right
assert((-value >> 4) == -0x03); // Shift right
assert((-value >>> 4) > 0); // Unsigned shift right
```

## Conditional <a href="#conditional-expressions" id="conditional-expressions"></a>

```dart
var visibility = isPublic ? 'public' : 'private';
String playerName(String? name) => name ?? 'Guest';  // Guest — default value
```

## Cascade notation

```dart
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0;
```

Это то же самое, что и

```dart
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = StrokeCap.round;
paint.strokeWidth = 5.0;
```

null-shorting cascade

```dart
querySelector('#confirm') // Get an object.
  ?..text = 'Confirm' // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
```
