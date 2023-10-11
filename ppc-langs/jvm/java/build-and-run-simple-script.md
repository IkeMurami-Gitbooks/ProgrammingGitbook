# Build and Run Simple Script

Пусть есть такой Java класс

```java
// MyJavaClass.java

class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!"); 
    }
}
```

Компилируем его:

```
$ javac MyJavaClass.java
$ ls
MyJavaClass.java HelloWorld.class
```

Упаковываем в JAR-архив. Этот архив должен содержать Manifest-файл, в котором указано для какой версии Java этот JAR-ник. Собираем с автоматической генерацией манифеста:

```
$ jar cfe MyJavaClass.jar HelloWorld HelloWorld.class
$ ls
MyJavaClass.java HelloWorld.class MyJavaClass.jar
```

Запускаем нашу программу

```
$ java -jar MyJavaClass.jar
Hello, World!
$ 
```

## Если нужны зависимости

Качаем jar-ники нужных версий в Maven репозитории — [https://mvnrepository.com/](https://mvnrepository.com/).

Компилируем с зависимостями:

<pre><code><strong>Windows:
</strong><strong>    javac -cp ".;/dir/commons.jar;/dir/more_jar_files.jar" MyClass.java
</strong>Unix:
    javac -cp ".:/dir/commons.jar:/dir/more_jar_files.jar" MyClass.java
</code></pre>

## OneCompiler

В этом сервисе — [https://onecompiler.com](https://onecompiler.com) — можно онлайн собирать Java-код с зависимостями и исполнять.
