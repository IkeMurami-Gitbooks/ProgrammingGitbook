# Создание CLI

## argparser

Пример:

```python
import argparse

def arg_parser():
    parser = argparse.ArgumentParser(description='Source differ')

    parser.add_argument('-i', '--input', help='List source paths (file path)', default='source.list', required=True)
    parser.add_argument('-o', '--output', help='Directory for the local git-repo', required=True)
    parser.add_argument('-r', '--rules', help='Cleaner\'s rules (ex see ./rules/: all,media,test,mock,...)', default='all', required=False)
    parser.add_argument('-s', help='Get file statistics', default=False, required=False, action='store_true')

    parser.add_argument('--author', help='Author for git-repo', default='cleaner@example.com', required=False)
    parser.add_argument('--committer', help='Committer for git-repo', default='cleaner@example.com', required=False)

    args = parser.parse_args()

    return args
    
if __name__ == '__main__':
    args = arg_parser()
    # args.input
    # args.output
    # args.rules
    # args.s
    # args.author
    # args.committer
```

## Click

[Click](https://click.palletsprojects.com/en/8.1.x/) — альтернатива argparser для создания интерфейса CLI.

Пример

```python
import click

@click.command()
@click.option('--count', default=1, multiple=True, help='Number of greetings.')
@click.option('--name', prompt='Your name', help='The person to greet.')
@click.argument('test')
def hello(count, name):
    print(f'> {name} {count} — {test}')

if __name__ == '__main__':
    hello()
    # $ python example.py --count 1 --count 2 --count 3 --name SomeName Hello
    # > SomeName [1, 2, 3] — Hello
```
