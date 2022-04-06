# Node.js

### Установка/Обновление

brew install node\
или\
npm install -g n\
sudo n stable

## NVM

Нужен для того, чтобы менять версию nodejs на лету

Link: [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

Про установку см в репозитории

Использование:

```
$ nvm install node
$ node -v
$ npm -v

$ nvm install 14.18.2
$ nvm ls

$ nvm use 17.4.0
$ nvm use default
$ nvm use stable

Установить конкретную версию npm
$ npm install -g npm@6.14.5

Установить последнюю версию npm для текущей ноды
$ nvm install-latest-npm

Установить версию по умолчанию
$ nvm alias default 10.16.0

Текущая версия:
$ nvm current

Все доступные версии:
$ nvm ls-remote
$ nvm ls-remote 15

Путь до установленной версии ноды:
$ nvm which 10.14.0

Запуск приложения с конкретной версии ноды без переключения:
$ nvm run 10.15.0 app.js
$ nvm exec 10.15.0 node app.js


```

Для обновления nvm достаточно заново запустить установщик с гита как в инструкции.

## Разное

переменная process — как window в браузере
