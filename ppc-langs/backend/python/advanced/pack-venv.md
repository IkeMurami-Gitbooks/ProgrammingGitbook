# Pack venv -> archive

## venv-pack

Крутая штука для созданиия portable packages: [https://pypi.org/project/venv-pack/](https://pypi.org/project/venv-pack/)

Ошибка:

```
Currently fails due to missing site-packages/venv_pack/scripts/nt/ directory
```

Решение: заходим по указанному пути (в `scripts`) и копируем папку `common` в `nt`.

Подробнее: [https://github.com/jcrist/venv-pack/issues/6](https://github.com/jcrist/venv-pack/issues/6)
