# Venv

## Пакет venv

Этот пакет встроен в Python Libraries. Создание виртуального окружения (чтобы не мусорить в глобальные зависимости). Обязательный подход при программировании на Python.

<pre><code>// Создание виртуального окружения
$ python3 -m venv .venv

<strong>// Активация
</strong>$ source .venv/bin/activate

// Далее работаем все отсюда: устанавливаем зависимости, запускаем код

// Деактивация — выход из виртуального окружения
$ deactivate
</code></pre>

## Пакет venv-pack

Крутая штука для созданиия portable packages: [https://pypi.org/project/venv-pack/](https://pypi.org/project/venv-pack/)

Ошибка:

```
Currently fails due to missing site-packages/venv_pack/scripts/nt/ directory
```

Решение: заходим по указанному пути (в `scripts`) и копируем папку `common` в `nt`.

Подробнее: [https://github.com/jcrist/venv-pack/issues/6](https://github.com/jcrist/venv-pack/issues/6)
