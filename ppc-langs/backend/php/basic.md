# Basic

Краткий понятный Tutorial: [https://htmlacademy.ru/tutorial/php](https://htmlacademy.ru/tutorial/php)

```php
<?php

class User 
{
    public $username;
    public $admin = false;

    public function __construct(string $username, bool $admin)
    {
        $this->username = $username;
        $this->admin = $admin;
    }
}

$user = new User("wiener", false);
$user_ser = serialize($user);

print(urlencode(base64_encode($user_ser)) . "\r\n");

?>
```
