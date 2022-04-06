# LevelDB

### Документация

[https://plyvel.readthedocs.io/en/latest/user.html#getting-started](https://plyvel.readthedocs.io/en/latest/user.html#getting-started)



### Установка

```
~# brew install leveldb
~# pip3 install plyvel
```

### Использование

```python
import plyvel
import argparse
import os


def arg_parser():
    # brew install leveldb
    # pip3 install plyvel

    parser = argparse.ArgumentParser(description='Tool for read LevelDB databases')
    parser.add_argument('-i', '--input_dir', help='Path to LevelDB', required=True)
    
    args = parser.parse_args()

    return args
    

if __name__ == '__main__':
    parser = arg_parser()
    with plyvel.DB(parser.input_dir) as db:
        for key, value in db:
            print('Key:', key, 'Value:', value)


```

### Проблемы

Google Chrome использует IndexedDB со своим компаратором (со своим сравнением строк) - idb\_cmp1\
Инфа:\
[https://stackoverflow.com/questions/35074659/how-to-access-google-chromes-indexeddb-leveldb-files](https://stackoverflow.com/questions/35074659/how-to-access-google-chromes-indexeddb-leveldb-files)

Пример использования компаратора

```python
def comparator(a, b):
    print('Custom comparator')
    print(f'{a} :: {b}')
    return 0

if __name__ == '__main__':
    parser = arg_parser()
    with plyvel.DB(parser.input_dir, comparator=comparator, comparator_name=b'idb_cmp1') as db:
        for key, value in db:
            print('Key:', key, 'Value:', value)
```
