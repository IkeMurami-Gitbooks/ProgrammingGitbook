# Импорт модулей и скриптов

PowerShell поддерживает модули.

```
Import-Module <path to the module>  # импортировать модуль
Get-Command –Module <module name>  # all cmds in module

а еще можно вот так скрипт импортировать
. .\MyScript.ps1
Invoke-MyCustomCommandFromMyScript -Test ...
```
