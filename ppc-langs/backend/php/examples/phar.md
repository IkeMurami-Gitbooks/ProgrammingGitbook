# Phar

php.ini:

```ini
[phar]
phar.readonly = 0
```

Create phar:

```php
<?php

$template = new YourObject();
// Create Phar
$file_name = 'test.phar';

$phar = new Phar($file_name);
$phar->startBuffering();
$phar->addFromString("test.txt", "test");
$phar->setStub('$prefix<?php __HALT_COMPILER(); ?>');
$phar->setMetadata($template);
$phar->stopBuffering();

// Get Phar content
$phar_content = file_get_contents($file_name);

?>
```

Run script:

```
$ php -c php.ini create_phar.php
```
