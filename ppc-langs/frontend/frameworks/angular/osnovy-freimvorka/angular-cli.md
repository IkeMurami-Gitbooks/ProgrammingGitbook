# Angular CLI

## Create Project

```
ng new my-proj
```

Создание проекта с роутингом сразу

```
ng new my-proj --routing
```

## Create Module

Модуль создатся в `./src/app/module-name/*`

```
ng g m module-name
```

Создать модуль `SomemoduleModule` в поддиректории (путь относительный => относительно той директории, в которой мы сейчас находимся => можно управлять структурой создаваемого проекта) — `./src/app/somepath/somemodule/*`

```
ng g m somepath/somemodule
```

Создать модуль, который будет с заданным роутом, с компонентом и привязан к заданному модулю (это надо при создании Lazy Modules — это такой механизм, при котором модули, и соотв страницы, подгружаются по необходимости):

```
ng g m pages/testmodule --route my-route --module app.module
```

## Create Service

см в разделе работа с сервисами

## Create Component

см в разделе работа с компонентами
