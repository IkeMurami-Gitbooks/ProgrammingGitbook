# Основы/Описание команд некоторых

## Возвращаемые объекты

Все cmdlet'ы возвращают .Net объекты! Следовательно, очень удобно можно работать с результатами cmdlet'ов: `(Get-Item C:\Windows).LastAccessTime`

## Исполнение команд

iex - алиас для Invoke-Expression - позволяет исполнять команды, например, скачивать что-либо

## Сохранить результат в файл

```
"some string or data" | Out-File "test.txt"
```

## Примеры

{% file src="../../.gitbook/assets/PowerShell.txt" %}
офигенные заметки по PowerShell как языку
{% endfile %}

