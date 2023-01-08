# Errors

## VSCode не подсвечивает локальные пакеты и пути

В Pylance простовать путь до extrapath (тем самым добавляем/меняем root path — путь, по которому находится проект в VSCode Workspace (по умолчанию — src)): [https://stackoverflow.com/a/64103291](https://stackoverflow.com/a/64103291)

## Об ошибке: `CERTIFICATE_VERIFY_FAILED`

Возникает при обращении к https сайтам (с попыткой что-то скачать)\
Решение:\
`$ export PYTHONHTTPSVERIFY=0`\
`$ python your_script`\
или\
`$ PYTHONHTTPSVERIFY=0 python your_script`

## Прописать пути до OpenSSL (Если, python пакет их не видит при установке)

brew upgrade openssl (Если не установлен)\
brew link --force openssl\
Теперь вводим в консоль, все что вывалилось из ответа\
Далее, можно устанавливать то, что имело зависимость от OpenSSL\
Например, pip3 install adb

## Non-ASCII character in file

```python
# -*- coding: utf-8 -*-
print('Теперь можно писать с utf8')
```

## Проблема при чтении HTML по кодировке UTF8

Попробовать считать кодировкой "ISO-8859-1"
