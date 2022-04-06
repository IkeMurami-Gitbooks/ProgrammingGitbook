# Копирование файлов и директорий

## Примеры

```powershell
# Копирование файла
Copy-Item -Path C:\from\path.txt -Destination C:\to\

# Копирование директории
Copy-Item -Path C:\from -Recurse -Destination C:\to\
```

## Копирование между машинами

```powershell
$remote_server = New-PSSession -ComputerName REMOTE

# На удаленный сервер
Copy-Item -Path C:\file.txt -Destination C:\remote\to -ToSession $remote_server

# С удаленного сервера
Copy-Item -FromSession $remote_server C:\remote\file.txt -Destination .
```
