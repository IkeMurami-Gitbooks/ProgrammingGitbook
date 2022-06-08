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
    doFirst {
        // Some actions — будет выполнено при запуске таска
        println("Hello world!")
    }
    println("Action in Configuration State") // Это действие будет выполнено только на стадии конфигурации
    doLast {
        // Some actions — будет выполнено при запуске таска
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
  * Означает, что результат выполнения task'а был взят из кэша
  * Чтобы gradle мог кэшировать task, надо его явно пометить аннотацией @CacheableTask
  * Gradle на основе имени класса, входных и выходных данных и других параметров сам вычисляет ключ
  * Кэш может быть не только локальным, но и удаленным, и использоваться несколькими машинами
* UP-TO-DATE
  * старый механизм в gradle, придуманный до кэша
  * Task имеет входные и выходные данные, которые не изменились
  * Task явно указал gradle, что его выходные данные не изменились (через лямбду, переданную в outputs.upToDateWhen{})
  * У task'а нет action'ов и он зависит от других task'ов, которые UP-TO-DATE, SKIPPED или FROM\_CACHE
  * У task'а нет ни action'ов, ни зависимостей
* (no label) or EXECUTED — Task и его зависимости были запущены

#### Как управлять зависимостями между тасками

1

`dependsOn()` — Используется, когда Task не может начать работу, пока не завершится один или несколько других task'ов (можно указать список)

```
// Groovy DSL
task TaskA {
    doFirst { println "running TaskA" }
}

task TaskB {
    dependsOn "TaskA"
    doFirst { println "running TaskB" }
}
```

2

`finalizedBy()` — Указывает, какой после текущего должен выполнится таск. Finalized task выполнится, даже если тот, от которого он зависит, завершится неудачей.

```
// Groovy DSL
task TaskC {
    doFirst { println "running TaskC" }
}

task TaskB {
    finlizedBy "TaskC"
    doLast { 
        println "running TaskB" 
        throw new RuntimeException("WTF")
    }
}
```

3

`shouldRunAfter()` и `mustRunAfter()` — Задают порядок выполнения тасков без явных зависимостей между ними. Главное отличие от `dependsOn` в том, что методы никак не влияют на запуск тасков, а ТОЛЬКО на порядок.

```
// Groovy DSL
task TaskA {
    doFirst { println "running TaskA" }
}

task TaskB {
    doLast { println "running TaskB" }
}

TaskA.mustRunAfter TaskB // можно запустить TaskA без TaskB и наоборот (то есть в отдельности)
```

`shouldRunAfter()` — порядок может быть не выполнен (если получился циклический порядок или при параллельном выполнении)

`mustRunAfter()` — порядок тасков должен выполнятся всегда

4

Inputs and Outputs

* Обычно, если task зависит от другого, то он ждет на вход данные, которые порождает другой task.
* В gradle можно связать входные данные одного таска с выходными данными другого, при этом можно явно не указывать зависимости через `dependsOn`.

В коде это выглядит примерно так

```
def producer = tasks.register("producer", Producer)
def consumer = tasks.register("consumer", Consumer)

// Связываем входной и выходной файл разных тасков
// Зависимости между тасками будут выставлены автоматически
consumer.confugire {
    inputFile = producer.flatMap { it.outputFile }
}
```

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

### gradle.properties

* Файл, лежащий в корневом проекте (а еще в `GRADLE_USER_HOME` и в директории gradle для глобальных свойств).
* можно указывать параметры JVM для запуска gradle (например, `org.gradle.jvmargs=-Xmx4096m`)
* можно указывать параметры самого Gradle
* можно писать свои свойства, которые будут доступны из gradle скриптов
* вместо использования gradle.properties, свойства можно передавать через командную строку через параметр `-p` (удобно для ключей, логинов и паролей)

### Extra properties

Многие объекты из доменной модели Gradle (в том числе Task и Project) могут содержать дополнительные свойства в виде ключ-значение. Ключ — это строка, а значение — это класс Object.

```
project.ext.test = "Test"   // Groovy DSL

// or

project.extra["test"] = "Test"   // Kotlin DSL

// or

project
    .getExtensions()
    .getExtraProperties()
    .set("test", "Test")
```

## Репозитории библиотек

Все пакеты публикуются в репозитории, чтобы каждый мог их себе подтянуть. Раньше были популярны JCenter и Google Maven, однако JCenter объявлен как deprecated и сейчас есть три варианта репозиториев:

* `google()` (Google Maven)
* `mavenCentral()`
* приватные Maven-репозитории
* Ivy
* локальная файловая система

Посмотреть, какие версии доступны в публичных репозиториях можно по ссылке:

```
https://mvnrepository.com/artifact/<package-root>/<package-name>
```

Например, для пакета `org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version` узнать информацию о доступных версиях в открытых репозиториях можно по ссылке: [https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-gradle-plugin](https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-gradle-plugin)

* Репозитории можно добавлять к build-скрипту или к проектам: первые будут использоваться для плагинов во время фазы конфигурации (они нужны gradle), вторые — во время получения зависимостей проекта (они нужны нашему проекту)
* В качестве транспорта можно указывать различные протоколы
* Лучше не прописывать все репозитории через allprojects, чтобы не увеличивать время конфигурации

## Dependencies

* Указывается в проектах и для build-скрипта
* Зависимости от внешних модулей:\
  `implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.4")`
* Проектные зависимости:\
  `implementation project(":core")`
* Файловые зависимости:\
  `implementation fileTree(dir: 'libs', include: ['*.jar'])`
* Дерево зависимостей можно получить вызовом специально таски dependencies — `./gradlew dependencies [--configuration implementation]`.

### Modules

Пример именования:

```
org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.4

<group>:<name>:<version>
```

### Configurations

* Каждая зависимость должна применяться к определенному скоупу: например, какие-то зависимости должны быть только во время компиляции, только во время тестов, только в runtime и т.д.
* Для управления скоупами в gradle используются конфигурации, которые хранятся в ConfigurationContainer
* По сути конфигурация — это контейнер для зависимостей
* Конфигурации можно создавать и наследоваться от других конфигураций через метод extendsFrom()
* Дочерние конфигурации наследуют все зависимости от своих родителей
* Обычно конфигурации создаются плагинами

Конфигурации, которые gradle создает по умолчанию (compile и runtime — deprecated) для java gradle plugin:&#x20;

### ![](../../.gitbook/assets/image.png)

В java library plugin еще появляется конфигурация `api`. Ее отличие от `implementation` в том, что она открывает доступ ко всем зависимым модулям (раньше `compile` так делал — он открывал доступ ко всем дочерним зависимостям, `implementation` так не делает).&#x20;

`compileOnly` — конфигурация подгружает модуль только на этап конфигурации (когда модуль не нужен в runtime). `implementation` подгружает модуль в runtime. Грамотное разделение модулей между `compileOnly` и `implementation` помогает ускорить время сборки проекта.

### Resolution strategy

* Между зависимостями могут встречаться конфликты
* Для каждой конфигурации можно настраивать поведение gradle при конфликтах — через resolution strategy
* Поведение по-умолчанию — gradle выбирает последнюю версию библиотеки

```
configurations.all {
    resolutionStrategy.failOnVersionConflict()
}
```

Что можно делать через resolution strategy

* заменять зависимости (dependencySubstitution)
* указать конкретную версию (force)
* выбирать проектные зависимости вместо бинарных (preferProjectModules)
* падать при конфликтах (failOnVersionConflict)
* менять параметры кэширования модулей, которые могут измениться (snapshot)

### Transitive dependencies

* Транзитивные зависимости — это дочерние зависимости прямой зависимости
* У конфигураций есть флаг isTransitive(), если он возвращает true, то будут разрешаться дочерние зависимости
* Такой же флаг есть у отдельных модулей
* По-умолчанию значение isTransitive()=true, но можно его поменять

```
dependencies {
    implementation('com.google.guava:guava:23.0') {
        transitive = false
    }
}
```

Это значит, что мы не подтягиваем зависимости guava к себе в проект, а укажем их вручную.

### Dependency constraints

Можно указать ограничения для версий модулей через constraints, включая транзитивные зависимости.

```
dependencies {
    implementation 'org.apache.httpcomponents:httpclient'
    constraints {
        implementation('org.apache.httpcomponents:httpclient:4.5.3')
        implementation('commons-codec:commons-codec:1.11')
    }
}
```

В примере выше `httpclient` зависит от `commons-codec`, и мы через constraints механизм фиксируем версию транзитивной зависимости.

### Excluding transitive dependencies

Можно убрать транзитивные зависимости через exclude (например, у нас две библиотеки тянут одинаковые зависимости; мы можем спокойно убрать из одной копии зависимостей и все будет спокойно собираться):

```
dependencies {
    implementation('log4j:log4j:1.2.15') {
        exclude group: 'javax.jms', module: 'jms'
        exclude group: 'com.sun.jdmx', module: 'jmxtools'
        exclude group: 'com.sun.jmx', module: 'jmxri'
    }
}
```

### Because

Вместо комментариев к зависимостям, можно использовать специальное свойство because. При выводе дерева зависимостей, этот комментарий отобразится в консоли :)&#x20;

```
implementation('log4j:log4j:1.2.15') {
    because "We love log4j"
}
```

## Plugins

* могут быть написаны скриптом, подключаться как jar (отдельный проект) или быть определены в buildSrc
* Добавляют таски, свойства, зависимости, конфигурации
* Расширяют DSL и доменную модель
* В общем, позволяют делать все, что угодно

Есть встроенные плагины, которые поставляются вместе с gradle. Например, java plugin, java library plugin.

Пример подключения скриптового плагина:

```
apply from: 'other.gradle'
```

Binary plugins имеют уникальный идентификатор, доступны из репозитория и подключаются через старый синтаксис `"apply plugin"` или блок `plugins{}` (всегда лучше использовать его).

## SourceSets

* Java Plugin (или Kotlin Plugin) вносит в доменную модель Gradle такое понятие как source set
* Source Sets позволяют группировать ресурсы и исходные файлы в логические группы
* Java Plugin создает для каждого Source Set соответствующий таск compile_SourceSet_Java и несколько конфигураций (для Source Set "main" имя опускается — compileJava)
* Аналогично для ресурсов создаются таски process_SourceSet_Resources.

Пример:

```
// Groovy DSL
sourceSets {
    main {
        java {
            srcDirs = [
                "src/main/java",
                "${protobuf.generatedFilesBaseDir}/main/javalite"
            ]
            exclude 'some/unwanted/package/**'
        }
    }
}
```

## Flavors & BuildTypes

* Build types — типы сборок, по умолчанию создаются только release и debug, обязательно наличие хотя бы одного типа
* Product flavors — разграничивают сборки по фичам (например, платная версия и с урезанной функциональностью)
* Build variants — все комбинации между Build types и Product flavors
* В доменной модели представлены соответствующими классами: BuildType, ProductFlavor и BaseVariant (содержит BuildType и ProductFlavor)
* BuildType и ProductFlavor наследуются от BaseConfigImpl

Для всех них создаются source set'ы и соответствующие таски.

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

## Kotlin & Groovy DSL

Gradle Kotlin DSL Usage: [https://github.com/IkeMurami/AndroidAppExamples/tree/main/KotlinDSLUsage](https://github.com/IkeMurami/AndroidAppExamples/tree/main/KotlinDSLUsage)

## buildSrc

Особая директория Gradle, в которую выносятся:

* константы
* внешние зависимости и версии
* имена проектов и модулей
* таски для сборки
* плагины для сборки
