# Работа с файлами

Чтение

```php
$file_name = 'test.phar';
$phar_content = file_get_contents($file_name);
```

Запись

```php
$outfile = 'out.phar'
file_put_contents($outfile, $phar_content);
```

Удаление файла

```php
$file_name = 'test.phar';
@unlink($file_name);
```
