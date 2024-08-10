# Linters

## black

Code formatter: [https://github.com/psf/black](https://github.com/psf/black)

```
black . [--check] --config pyproject.toml
```

## isort

Sort your imports: [https://pycqa.github.io/isort/](https://pycqa.github.io/isort/)

```
isort . [--check-only] [--diff]
```

## flake8

Linter: [https://flake8.pycqa.org/en/latest/](https://flake8.pycqa.org/en/latest/)

```
flake8 . --config setup.cfg
```
