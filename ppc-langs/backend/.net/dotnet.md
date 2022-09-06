---
description: Это консольная программа для управления проектом (сборка и тп)
---

# dotnet

Можно собирать проекты под линукс, винду, мак

Для сборки нужны&#x20;

* `.Net SDK`  [https://dotnet.microsoft.com/en-us/download/dotnet](https://dotnet.microsoft.com/en-us/download/dotnet)  Версии: 5.x, 6.x, 7.x
* `.Net Framework DevPack` (по-русски, пакет для разработчиков или распространяемого компонента), под который будет собираться конкретный проект [https://docs.microsoft.com/ru-ru/dotnet/framework/install/guide-for-developers](https://docs.microsoft.com/ru-ru/dotnet/framework/install/guide-for-developers) Версии: 3.5, 4.x.x,&#x20;

```
dotnet sln Mytest.sln list  // Список проектов, зарегистрированных в sln
dotnet build MyTest.sln  // build sln
```

Создать проект/решение из шаблона: [https://docs.microsoft.com/ru-ru/dotnet/core/tools/dotnet-new](https://docs.microsoft.com/ru-ru/dotnet/core/tools/dotnet-new)

```
Пример:

dotnet new sln -n TestMySolution
dotnet new console -n TestConsoleProject

dotnet sln TestMySolution.sln add TestConsoleProject/TestConsoleProject.csproj
```

Запуск проекта

```
dotnet run --project TestCSharpApp/TestCSharpApp.csproj
```
