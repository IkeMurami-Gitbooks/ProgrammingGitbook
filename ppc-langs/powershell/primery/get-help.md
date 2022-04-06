# Get-Help

```
cmdlet - команда, например Get-Help
Get-Help <cmdlet> [-examples] [-detailed] [-full]  # Получить справку по cmdlet
Get-Help About_<Topic> # получить справку по разделу
Help <cmdlet>  # Help - это alias к Get-Help
<cmdlet> -?

Ex: Get-Help -?

Get-Help *  # Вывести все cmdlet (в тч по aliases)
Get-Help process  # Поиск по базовым cmdlets и aliases

Get-Command -CommnadType cmdlet  # Все cmdlets, зарегистрированные в системе
# Здесь можно найти очень много интересных, например, Get-Process - список процессов

Update-Help  # обновить справочник
```

Все команды - echo, chdir, ls.. - aliases к cmdlet'ам

cmdlet - не бинари, а скрипты. Следовательно, можно писать свои cmdlet'ы и вызывать их!
