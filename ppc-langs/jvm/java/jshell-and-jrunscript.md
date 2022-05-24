# jshell & jrunscript

## jrunscript

На 11 java или старше, в состав JRE входит jrunscript с предустановленным Script Engine — Oracle Nashorn. Он позволяет запускать Java код:

```
jrunscript -e "java.util.Arrays.asList(javax.net.ssl.SSLContext.getDefault().getSocketFactory().getSupportedCipherSuites()).forEach(println)"
```

Однако на более новых JRE этот движок не установлен и как его подключить я не знаю.

## jshell

Это часть JDK. Java в консоли.

```
$ jshell
jshell> java.util.Arrays.asList(javax.net.ssl.SSLContext.getDefault().getSocketFactory().getSupportedCipherSuites()).forEach(System.out::println)
jshell> /exit
```
