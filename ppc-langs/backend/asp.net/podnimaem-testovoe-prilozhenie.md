# Поднимаем тестовое приложение

## Установка

Скачиваем microsoft .net sdk

## Создаем проект с помощью CLI (Win/MacOS)

Для начала создадим solution. Для этого как матерые ребята воспользуемся .net CLI. Открываем папку, где хотим создать решение и выполняем следующую команду в cmd:

```
PS D:\> mkdir IpLookup && cd IpLookup
PS D:\> dotnet new sln
```

После создадим пустой веб проект с помощью команды

```
PS D:\> dotnet new web -n WebApi
```

Теперь добавим новый проект, который мы создали в наш sln

```
dotnet sln IpLookup.sln add WebApi/WebApi.csproj
```

## Запуск нашего приложения

Может потребоваться создать HTTPS-сертификат разработчика и добавить его в систему:

```
dotnet dev-certs https --trust
```

Теперь запускаем приложение:

```
dotnet run --project WebApi/WebApi.csproj
```

Если ругнулся на порт, меняем здесь: IpLookup/WebApi/Properties/launchSettings.json

На https://localhost:\<port> должны увидеть Hello World

