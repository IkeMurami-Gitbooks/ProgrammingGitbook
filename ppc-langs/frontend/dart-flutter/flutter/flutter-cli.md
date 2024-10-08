# Flutter CLI

## FlutterGen

Отвечает за кодогенерацию и работу с ассетами в Flutter

link: [https://pub.dev/packages/flutter\_gen](https://pub.dev/packages/flutter\_gen)

## Flutter Version Manager

Как для node — nvm, так для flutter — [fvm](https://fvm.app/):

```
fvm flutter ...
```

## Create project

```
$ flutter create myapp
$ cd myapp
```

## Devices

```
$ flutter devices
1 connected device:

Chrome (web) • chrome • web-javascript • Google Chrome 95.0.4638.69
$ ...
```

## Run

Можем запустить через VS Code:

* Command Palette: `> Flutter: Run Project`
* Через `Debug and Run`

Через cmd в корне, где лежит `pubspec.yaml`:

```
$ flutter run [-d chrome] [--release]
```

Здесь можно настроить в VS Code через какое устройство запускать

![](<../../../../.gitbook/assets/image (1) (1) (1).png>)

Если запускали через консоль, можно использовать горячую клавишу `r` для перезапуска приложения и отображения изменений (`Hot Reload`).

## Build

С помощью dart2js собираем один JS-файл main.dart.js.

```
$ flutter build web
```

## Format Code

* Android Studio and IntelliJ IDEA: Right-click the code and select **Reformat Code with dartfmt**.
* VS Code: Right-click and select **Format Document**.
* Terminal: Run `flutter format <filename>`.

## Add dependencies

Добавляем в `pubspec.yaml` зависимости и запускаем `flutter pub get` или жмем в IDE `Pub Get`.
