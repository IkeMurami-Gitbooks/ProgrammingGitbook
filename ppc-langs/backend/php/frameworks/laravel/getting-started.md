# Getting Started

Install composer (скачает composer.phar в текущую папку):

```
curl -sS https://getcomposer.org/installer | php
```

Создать проект:

```
php composer.phar create-project laravel/laravel laravel-project
```

Доставляем зависимости:

```
cd laravel-project
php ../composer.phar require smi2/phpclickhouse
```

Запускаем приложение

```
php artisan serve
```
