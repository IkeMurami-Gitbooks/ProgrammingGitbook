# Usefull small examples

## Hello world

```php
<?php

echo "Hello1!\n";

echo var_dump($_REQUEST);

?>
```

```php
<?

echo "Hello1!\n";

?>
```

## Input args

```php
<?php

// TODO

?>
```

## Поднять php сервер локально

```
php -S localhost:8000
```

## Работа с переменными и объектами

```php
echo var_dump($_REQUEST);  // Вывести содержимое объекта

$param = 1;

echo "Param = ", $param, "\n";
echo "Param = ".$param."\n";
```

## Системные переменные

```php
$_REQUEST — все параметры запроса
$_REQUEST["someQueryParam"] -> 1 — https://example.com/?someQueryParam=1

$_SERVER — все переменные сервера
$_SERVER['DOCUMENT_ROOT'] — путь до ~

```

## Подключить другой скрипт

```php
$filename = "some.php";
$filename = "some.jpg";

@include($filename);  // еще есть директива required.
```

## try-catch

```php
try {
    // ...
}
} catch (Exception $e) {
    echo 'Выброшено исключение: ',  $e->getMessage(), "\n";
}
```
