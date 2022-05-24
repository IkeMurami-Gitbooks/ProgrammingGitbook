# Packages & Plugins

## Про плагины и пакеты

[https://pub.dev/](https://pub.dev) — здесь есть немало пакетов, разработанных инженерами Google и комьюнити (Dart и Flutter).

Плагины – это определенный тип пакетов, которые предоставляют платформа-зависимую функциональность (например, доступ к камере мобильного устройства).&#x20;

Примеры пакетов: making network requests ([`http`](https://flutter.dev/docs/cookbook/networking/fetch-data)), custom navigation/route handling ([`fluro`](https://pub.dev/packages/fluro)), integration with device APIs ([`url_launcher`](https://pub.dev/packages/url\_launcher) and [`battery`](https://pub.dev/packages/battery)), and using third-party platform SDKs like Firebase ([FlutterFire](https://github.com/flutter/plugins/blob/master/FlutterFire.md)).

Про написание своих пакетов: [https://flutter.dev/docs/development/packages-and-plugins/developing-packages](https://flutter.dev/docs/development/packages-and-plugins/developing-packages)

## Использование

`pubspec.yml` — файл, в котором описаны зависимости проекта или пакета

Подтянуть зависимости:&#x20;

```
$ flutter pub get
```

## Channels

Есть разные каналы получения обновлений для пакетов Flutter — origin, stable, ...

Иногда может возникнуть ошибка, что какие-то зависимости не разрешены — пробуем переключиться на другую версию пакетов:

```
$ flutter channel stable
$ flutter upgrade
```
