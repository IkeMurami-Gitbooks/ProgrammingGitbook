# Magic Methods

В PHP как и во многих языках есть magic methods: [https://www.php.net/manual/en/language.oop5.magic.php](https://www.php.net/manual/en/language.oop5.magic.php)

### \_\_sleap() & \_\_wakeup()

Метод `__sleep()` вызывается до `serialize()`. Он может очистить какие поля объекта и возвращает список полей, которые должны быть сериализованы.

И наоборот, `unserialize()` проверяет наличие функции с магическим именем `__wakeup()`. Если она присутствует, эта функция может восстановить любые ресурсы, которые может иметь объект.

Предполагаемое использование `__wakeup()` состоит в том, чтобы восстановить любые соединения с базой данных, которые могли быть потеряны во время сериализации, и выполнить другие задачи повторной инициализации.

Пример:

```php
<?php

class Connection
{
    protected $link;
    private $dsn, $username, $password;
    
    public function __construct($dsn, $username, $password)
    {
        $this->dsn = $dsn;
        $this->username = $username;
        $this->password = $password;
        $this->connect();
    }
    
    private function connect()
    {
        $this->link = new PDO($this->dsn, $this->username, $this->password);
    }
    
    public function __sleep()
    {
        return array('dsn', 'username', 'password');
    }
    
    public function __wakeup()
    {
        $this->connect();
    }
}

?>
```

## \_\_serialize() && \_\_unserialize()

Имеют больший приоритет над `__sleep()` и `__wakeup()`. Будут вызваны только `__serialize()` и `__unserialize()`.

Пример:

```php
<?php

class Connection
{
    protected $link;
    private $dsn, $username, $password;

    public function __construct($dsn, $username, $password)
    {
        $this->dsn = $dsn;
        $this->username = $username;
        $this->password = $password;
        $this->connect();
    }

    private function connect()
    {
        $this->link = new PDO($this->dsn, $this->username, $this->password);
    }

    public function __serialize(): array
    {
        return [
          'dsn' => $this->dsn,
          'user' => $this->username,
          'pass' => $this->password,
        ];
    }

    public function __unserialize(array $data): void
    {
        $this->dsn = $data['dsn'];
        $this->username = $data['user'];
        $this->password = $data['pass'];

        $this->connect();
    }
}

?>
```
