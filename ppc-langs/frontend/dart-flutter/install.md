# Install

## Dart

```
 // Install
 $ brew tap dart-lang/dart
 $ brew install dart
 
 // Upgrade
 $ brew upgrade dart
 
 // Version info
 $ brew info dart
```

## Flutter

Если собираемся использовать Flutter SDK, то ставить отдельно Dart SDK не надо: он уже включен в flutter.&#x20;

Flutter устанавливается из архива (см на оф сайте)

Но я смог и через homebrew поставить:

```
$ brew install flutter
```

Для разработки под iOS должен быть настроен XCode, для разработки под Android — Android SDK.

Проверить, что все настроено можно через Flutter Doctor:

```
$ flutter doctor
```
