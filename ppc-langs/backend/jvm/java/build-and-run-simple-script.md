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
