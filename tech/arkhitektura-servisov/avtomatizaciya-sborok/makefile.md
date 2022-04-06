# Makefile

make — утилита, автоматизирующая процесс преобразования файлов из одной формы в другую. Чаще всего это компиляция исходного кода в объектные файлы и последующая компоновка в исполняемые файлы или библиотеки.

Конфиг файл к этой утилите — Makefile.

Пример:

```
CONTAINER_NAME ?= mycontainer
RELEASE_VERSION ?= 0.0.0
DISTRIB_BUILD_NUMBER ?= 0
DISTRIB_PATH ?= mydistrib.zip

IMAGE_NAME = $(CONTAINER_NAME):$(RELEASE_VERSION)-$(DISTRIB_BUILD_NUMBER)

all: clean check build

build: build-image:
    docker build -t $(IMAGE_NAME) ./
    
build: build-image
    docker run -w /build --name $(CONTAINER_NAME) $(IMAGE_NAME) /bin/bash -c "./build.sh $(RELEASE_VERSION) $(DISTRIB_BUILD_NUMBER)"
    docker container cp $(CONTAINER_NAME):/some/path/x86_64/cont.rpm
    
check: build-image
    docker run --rm -w /build --name $(CONTAINER_NAME) $(IMAGE_NAME) /bin/bash -c "./check.sh"
    
clean:
    rm -f $(DISTRIB_PATH) /other/path
    docker container rm -f $(CONTAINER_NAME) 2>/dev/null || echo 'Container not found'
    docker image rm -f $(IMAGE_NAME) 2>/dev/null || echo 'Image not found'
    
distrib:
    cp -r build/liquibase ./
    zip -r -Z store $(DISTRIB_PATH) liquibase
    
.PHONY: all build-image build clean distrib
```

И соотв запуск:&#x20;

```
$ make build
```
