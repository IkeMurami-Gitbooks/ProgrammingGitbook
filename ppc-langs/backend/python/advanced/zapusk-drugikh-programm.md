# Запуск других программ

Для этого есть пакет `subprocess`:

```python
import subprocess

result = subprocess.run(['some_binary', '--help'])
```
