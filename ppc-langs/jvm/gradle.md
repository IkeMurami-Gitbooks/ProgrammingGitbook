# Gradle

## Intro

Система автоматизации сборки. Скрипты — это dsl на Groovy или Kotlin. Не только для Android и jvm, но и для native (C++, swift) и многого другого.

Youtube запись от Redmadrobot: [https://www.youtube.com/watch?v=WOBok2u-SL8](https://www.youtube.com/watch?v=WOBok2u-SL8)

## Install

releases: [https://gradle.org/releases/](https://gradle.org/releases/)

Скачиваем бинари, кладем в папку, и прописываем путь до папки bin в PATH.

```
gradle -v
```

### Update gradle

Для подключения gradle, надо задать версию для зависимости `com.android.tools.build:gradle`.

`build.gradle`:

```
buildscript {
    ext {
        gradle_version = "7.1.2"
    }

    repositories {
        google()
    }
    
    dependencies {
        classpath "com.android.tools.build:gradle:${gradle_version}"
    }
}
```

Вот здесь можно посмотреть какие версии доступны в google maven репозитории (`google()`): [https://mvnrepository.com/artifact/com.android.tools.build/gradle-api?repo=google](https://mvnrepository.com/artifact/com.android.tools.build/gradle-api?repo=google)

Есть два способа обновить версию gradle:

* Android Studio: File > Project Structure > установить версию Gradle из выпадающего списка. Нужная версия Gradle сразу подтянется
* Terminal:&#x20;

```
Информация по текущей версии
$ gradle help --scan
$ gradle --version

Обновить до 7.0:
$ gradle wrapper --gradle-version 7.0
```

## Basics

* Состоит из проектов (они же называются модулями, что может вызвать путаницу)
* Проекты могут быть сколь угодно вложенными

Пример структуры

```
root_project/
   sub-project-1/
      build.gradle
   sub-project-2/
      one-more-sub-project/
         build.gradle
      build.gradle
   sub-project-3/
      build.gradle
   build.gradle
   settings.gradle
```

settings.gradle — здесь описаны все проекты:

```
rootProject.name = "Root Project"
include("sub-project-1")
include("sub-project-2")
include("sub-project-3")
include("sub-project-2:one-more-sub-project")
```

### Tasks и Projects

* Это основные объекты в доменной модели Gradle
* Таск — основная единица выполнения в Gradle.
* Таски могут зависеть друг от друга

Пример таска (на Kotlin DSL):

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello world!")
    }
}
```

#### Task configuration

* doFirst() и doLast() — действия, совершаемые в начале и в конце выполенения таска
* description — описание таска
* group — имя группы тасков
* extra properties

#### Task actions

* Методы, помеченные аннотацией @TaskAction, будут запускаться при запуске таска
* У таска может быть несколько action'ов
* Все action'ы хранятся в списке и будут запускаться по порядку
* Методы doFirst и doLast на самом деле добавляют action'ы в начало или конец списка

#### Incremental Tasks

Некоторые таски достаточно тяжелые, и нет смысла их перезапускать, если произошла ошибка или произошли какие-то изменения в файлах в последующих тасках. Есть специальное API, которое позволяет не перезапускать таски, которые завершились успешно.

#### Результат выполнения тасков

* NO-SOURCE — таск не был запущен, так как не нашлось данных для него
* SKIPPED — таск не был запущен
  * был явно выключен
    * через командную строку (параметр -x)
    * через свойство enabled=false
    * через список excludedTaskNames
  * произошел StopExecutionException
  * у таска есть предикат onlyIf {}, который вернул false
* FROM-CACHE
* UP-TO-DATE
* (no label) or EXECUTED

### Фазы сборки Gradle

1. Initialization — Gradle проверяет, какие проекты будут участвовать в билде и создает инстансы Project
2. Configuration — Запускаются все скрипты build.gradle у каждого проекта и определяются таски проектов, строится граф тасков
3. Execution — Gradle запускает таски в нужном порядке на основе графа

#### settings.gradle

* Этот файл сапускается во время первой фазы инициализации
* Вызовы в этом файле делегируются объекту Settings
* Для многомодульных (multi-project) проектов settings.gradle нужен обязательно, чтобы в нем описать дерево проектов

#### Project evaluation

* Во время фазы конфигурации Gradle проходит по каждому проекту, запускает build скрипт и создает таски (Gradle умеет запускать только те build-скрипты, которые нужны для конкретного таска)
* В api есть возможность сделать что-то до или после конфигурации: методы beforeEvaluate() и afterEvaluate() у проектов
* Эти методы обычно используются из корневого проекта, чтобы сделать какую-то дополнительную конфигурацию у дочерних проектов

### Репозитории библиотек

Все пакеты публикуются в репозитории, чтобы каждый мог их себе подтянуть. Раньше были популярны JCenter и Google Maven, однако JCenter объявлен как deprecated и сейчас есть три варианта репозиториев:

* `google()` (Google Maven)
* `mavenCentral()`
* приватные Maven-репозитории

Посмотреть, какие версии доступны в публичных репозиториях можно по ссылке:

```
https://mvnrepository.com/artifact/<package-root>/<package-name>
```

Например, для пакета `org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version` узнать информацию о доступных версиях в открытых репозиториях можно по ссылке: [https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-gradle-plugin](https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-gradle-plugin)

### Flavors & BuildTypes

`flavor dimension` - под этим можно понимать категорию переменных, некоторый переменный набор. Пример:&#x20;

```
flavorDimensions "site", "endpoint", "market"
```

`productFlavors` - здесь определяются возможные значения для `dimension`. Пример:

```
productFlavors {
        prod {
            dimension 'endpoint'
            applicationId 'blabla1'
        }

        staging {
            dimension 'endpoint'
            applicationId 'blabla2'
            здесь могут быть объявлены любые переменные (те же, что в defaultConfig)
        }

        google {
            dimension 'market'
        }

        amazon {
            dimension 'market'
        }

    }
```

Здесь: в категории endpoint могут содержатся следующие наборы параметров: prod, staging

Далее, эти значения используется в определении buildTypes (каким образом?)

### Kotlin & Groovy DSL

Gradle Kotlin DSL Usage: [https://github.com/IkeMurami/AndroidAppExamples/tree/main/KotlinDSLUsage](https://github.com/IkeMurami/AndroidAppExamples/tree/main/KotlinDSLUsage)

## buildSrc

Особая директория Gradle, в которую выносятся:

* константы
* внешние зависимости и версии
* имена проектов и модулей
* таски для сборки
