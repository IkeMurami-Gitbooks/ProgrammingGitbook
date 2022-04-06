# Materialize

## About

site: [https://materializecss.com/about.html](https://materializecss.com/about.html)

Material Design, созданный и разработанный Google, представляет собой язык дизайна, сочетающий в себе классические принципы успешного дизайна с инновациями и технологиями. Цель Google - разработать систему дизайна, обеспечивающую унифицированный пользовательский интерфейс для всех своих продуктов на любой платформе.

Я так понял, что Materialize — CSS фреймворк поверх Material Design.

## Usage

Подключение (ссылки с сайта Material Design можно взять), можно подключать только css или js:

```markup
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    
    <!-- Compiled and minified CSS -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
    <!-- Compiled and minified JavaScript -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
      
    <title>TypeScript</title>
</head>
<body>
    <div class="container">
        <!-- Классы берутся из Materialize  -->
        <button class="btn" id="btn">Click me pls</button>
    </div>
</body>
</html>

```

И эта кнопка сразу возьмет на себя стиль, определеный в materialize.

![](<../../../../.gitbook/assets/изображение (8).png>)
