# Install

## Debian

```
apt update
apt install python3 python3-venv python3-pip
```

## MacOS

```
brew install python3
```

## pyenv

Удобно управлять версией python с помощью [pyenv](https://github.com/pyenv/pyenv#getting-pyenv)

```
brew install pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init - zsh)"' >> ~/.zshrc
```

Install python:

```
pyenv install 3.9.16
pyenv install 2.7.18
...
```

set global versions:

```
pyenv global 3.9.16 2.7.18
```

check python versions:

```
pyenv global
python --version
python2 --version
python3 --version
```
