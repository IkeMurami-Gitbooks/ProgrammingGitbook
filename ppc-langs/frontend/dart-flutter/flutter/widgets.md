# Widgets

In Flutter, almost everything is a widget, including alignment, padding, and layout.

Есть два типа виджетов — `Stateless` и `Stateful`. `Stateless Widgets` неизменяемы — их свойства не изменяются, все значения окончательны. `Stateful Widgets` могут изменяться во время цикла жизни виджета. Реализуя `Stateful Widget` мы должны определить два класса:

1. `StatefulWidget` — класс, который создает `State`-класс. Сам по себе `StatefulWidget` неизменяем и может быть удален и пересоздан.
2. `State` — класс, который остается при виджете на все время его жизни

```dart
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: const MyHomePage(title: 'Test'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key, required this.title}) : super(key: key);
  
  final String title;
  
  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;
  
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }
  
  @override
  Widget build(BuildContext coontext) {
    return Column(
      children: <Widget>[
        const Text(
          'Some counter:',
        ),
        Text(
          '$_counter',
        ),
        FloatingActionButton(
          onPressed: _incrementCounter,
          tooltip: 'Increment',
          child: const Icon(Icons.add),
        ),
      ],
    );
  }
}


```

