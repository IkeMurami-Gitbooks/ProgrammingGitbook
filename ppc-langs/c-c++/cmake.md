---
description: служит для сборки проектов c/c++
---

# cmake

Пример CMakeLists.txt:

```
project(test)
set(SOURCE_EXE FrameClassify.c LPCdecode.c LPCencode.c StateConstructW.c StateSearchW.c anaFilter.c constants.c createCB.c doCPLC.c enhancer.c filter.c gainquant.c getCBvec.c helpfun.c hpInput.c hpOutput.c iCBConstruct.c iCBSearch.c iLBC_decode.c iLBC_encode.c iLBC_test.c lsf.c packing.c syntFilter.c)
add_executable(test ${SOURCE_EXE})
```

header-файлы указывать не надо, они сами подцепятся.\
Если нужно часть кода скомпилить как библиотеку и/или есть папки/подпапки, то тут конфиг будет сложнее. Подробнее можно посмотреть здесь: [https://habr.com/ru/post/155467/](https://habr.com/ru/post/155467/)

```
Пусть путь до исходного кода находится в /path
$> cd /path
$> mkdir build # Сюда будем писать всякий мусор
$> cd build
$> cmake .. # Собираем все необходимое для компилирования проекта (cmake все сделает сам)
В /path должен быть файл CMakeLists.txt, в котором указано, как собирать проект.
$> cmake --build . --target test --config Release
--config Release|Debug
--build - путь до папки build
--target - проект (имя указываем в CMakeLists.txt)
```
