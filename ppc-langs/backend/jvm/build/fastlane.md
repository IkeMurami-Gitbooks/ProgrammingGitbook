# Fastlane

Это штука похожая на gradle. Служит для автоматизации сборки и выпуска Beta и Release проекта (в том числе и iOS; Включает в себя отправку на Play Store, Crashlytics, App Store ...)

Поддерживает массу расширений от сообщества, в том числе resign — переподписыватель iOS-приложений

Сайт проекта: [https://docs.fastlane.tools](https://docs.fastlane.tools)

## Install

```
gem install fastlane -NV
brew cask install fastlane
```

## Usage

Создаем пустой проект (если у нас нет кода, а мы хотим делать какую-то автоматику без привязки к нему):

```
fastlane init
```

### Appfile

Здесь настраивается информация о разработчике, приложении и Apple-аккаунте: [https://docs.fastlane.tools/advanced/Appfile/](https://docs.fastlane.tools/advanced/Appfile/)

### Actions

Все actions: [https://docs.fastlane.tools/actions/](https://docs.fastlane.tools/actions/)

### resign

Action link: [https://docs.fastlane.tools/actions/resign/](https://docs.fastlane.tools/actions/resign/)

