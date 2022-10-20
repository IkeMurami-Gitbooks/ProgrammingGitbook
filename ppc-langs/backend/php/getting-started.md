# Getting Started

## Install

```
brew install php
docker run --rm -it php
```

Пример настроенного в Docker окружения: [https://github.com/IkeMurami/zdocker-env/tree/master/PHP/DockerPHP](https://github.com/IkeMurami/zdocker-env/tree/master/PHP/DockerPHP)

## Package Manager

Один из самых распространенных — [Composer](https://getcomposer.org/).

### Install

Docker

```bash
docker pull composer/composer
docker run --rm -it -v "$(pwd):/app" composer/composer install
```

Или добавить в наш Docker-файл:

```docker
COPY --from=composer/composer /usr/bin/composer /usr/bin/composer
```
