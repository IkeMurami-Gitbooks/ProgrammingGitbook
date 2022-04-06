# Создание объекта Credential

```
$Username = 'test_user'
$Password = 'test_password'
$pass = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $Username,$pass

Invoke-Command -ComputerName UFC-SQLDev -ScriptBlock {Get-UICulture} -Credential $Cred
```
