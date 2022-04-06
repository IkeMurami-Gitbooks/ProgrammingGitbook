# Работа с файлами

## Построчное чтение файла

```bash
exec 9<config.txt

while read line <&9; do
    echo "$line"
done

exec 9<&-
```

## Чтение файла в переменную

```bash
#!/bin/bash

target=$(<input.txt)
echo "$target"
```
