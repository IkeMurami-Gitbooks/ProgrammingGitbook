# pubspec.yaml

Конфигурационный файл проекта (как `package.json` в Node.js проектах). Содержит метаданные по проекту и/или пакету (всегда должен быть). Хранит информацию по зависимостям и об авторе программы

## Пример

```yaml
name: mytestprogram
description: Something aboutt the program
version: 1.0.0
author: IkeMurami
homepage: https://example.com/

environment:
    ...
    
dependencies:
    ...
    
dev_dependencies:
    ...
```

## Подключить Material Design

This will allow you to use more features of Material, such as their set of predefined [Icons](https://fonts.google.com/icons).

```yaml
flutter:
    uses-material-design: true
```

В коде:

```dart
import 'package:flutter/material.dart';
// ...
```
