# Classic ASP.NET WebApp

## wwwroot

Всю статику: js, css, images. Эта папка будет как корневая на сервере

## Program.cs

Входная точка нашего ASP.NET Core приложения

## Startup.cs

Выполняет две фукнции в классах ConfigureServices и Configure

ConfigureServices: регистрируем сервисы в этой функции

Configure: в этой фукнции есть два параметра ... для создания пайплайна (middleware)

## Appsetting.json

Используется .Net Core как web.config

## Connected services

Любые типы сервисов, которые мы могли добавить: Azure Cloud, WCF provider, 3-rd party service

## like MVC pattern folders

### View

&#x20;UI

### Model

все модели, такие как свойства столбцов таблиц (what?)

### Controller

все контроллеры

