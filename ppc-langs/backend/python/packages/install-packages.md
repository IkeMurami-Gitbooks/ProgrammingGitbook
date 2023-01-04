# Install packages

## pip

Установить пакет конкретной версии:\
1\. Удалить пакет, если уже установлен\
2\. `pip install -I packet==version`

## pipx

Это пакетный менеджер для приложений. Он создает изолированное окружение для python приложений, которые должны быть запущены из консоли. Например, mitmproxy.&#x20;

pipx - не замена pip, а дополнение. => через pip ставим библиотеки для разработки, через pipx ставим готовые приложения

Link: [https://github.com/pypa/pipx](https://github.com/pypa/pipx)

Install & Usage:

```
$ python3 -m pip install --user pipx
$ pipx ensurepath
$ pipx list
$ pipx install mitmproxy
```

Install package from git:

```
pipx install git+https://github.com/psf/black.git
pipx install git+https://github.com/psf/black.git@branch  # branch of your choice
pipx install git+https://github.com/psf/black.git@ce14fa8b497bae2b50ec48b3bd7022573a59cdb1  # git hash
pipx install https://github.com/psf/black/archive/18.9b0.zip  # install a release

The pip syntax with egg must be used when installing extras:

pipx install "git+https://github.com/psf/black.git#egg=black[jupyter]"
```

Больше примеров на github

## poetry

Управление пакетами и зависимостями. [https://python-poetry.org/](https://python-poetry.org/)

```
pip install poetry
poetry init
poetry config virtualenvs.in-project true

poetry add some-dep-proj
poetry install

poetry show --tree

poetry run python main.py --help
poetry run uvicorn main:app ...
```
