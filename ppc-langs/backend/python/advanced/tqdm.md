# tqdm

`tqdm` - console progress bar for python: [https://github.com/tqdm/tqdm](https://github.com/tqdm/tqdm)

Пример использования `tqdm` - перебор паролей к zip-архиву:

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
