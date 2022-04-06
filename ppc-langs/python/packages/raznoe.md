# Разное

`tqdm` - console progress bar for python: [https://github.com/tqdm/tqdm](https://github.com/tqdm/tqdm)

пример использования tqdm - перебор паролей к zip-архиву:

```python
import zipfile
from tqdm import tqdm

wordlist = "rockyou.txt"
# wordlist = "filtered_top_100k.txt"
target = "TopSecret.zip"

n_words = len(list(open(wordlist, "rb")))  # count passwords for brute
print("Total passwords to test:", n_words)


zip_file = zipfile.ZipFile(target)  # open zip file

with open(wordlist, "rb") as wordlist:
	for word in tqdm(wordlist, total=n_words, unit="word"):  # read password
		try:
			zip_file.extractall(pwd=word)  # try open file
		except:
			continue
		else:
			print("[+] Password found:", word.decode().strip())
			exit(0)
			
	print("[!] Password not found, try other wordlist.")
```

`ptrlib` - пакет для работы с криптой, сетью, либами и тд - посмотреть что внутри, к себе перенять core [https://pypi.org/project/ptrlib/](https://pypi.org/project/ptrlib/)

Подключение библиотек на других языках:\
`comtypes, ctypes`\
`msl-loadlib` -> обертка над др пакетами, которая позволяет подключать `C/C++/.Net/COM/Fortran`\
Важная особенность COM-объектов:\
Перед использованием, надо зарегистрировать в системе библиотеку:\
`C:\Windows\System32> regsvr32 "/path/to/library.dll"`\
`C:\Windows\SysWOW64> regsvr32 "/path/to/library.dll"`\
Именно по двум путям от рута!!!! Иначе будет вылетать ошибка: `Класс не зарегистрирован в системе`.\
Для удаления библиотеки достаточно добавить `/u` аргументом

`gnureadline` - [https://github.com/ludwigschwardt/python-gnureadline](https://github.com/ludwigschwardt/python-gnureadline) Для чего пакет, точно не установил, но связан как-то с clipboard, чтением из консоли и тп

`pydantic` - валидация данных



