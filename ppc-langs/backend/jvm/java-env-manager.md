# Java Env Manager

Сложно вручную переключаться между окружениями и помнить, где что установлено.&#x20;

[jenv](https://github.com/jenv/jenv) — инструмент, который позволяет удобно управлять окружениями на компьютере.

Далее пример использования для MacOS:

<pre><code>$ brew install jenv

// Install some version Java
$ brew install openjdk@11  // 11
$ brew install openjdk@8  // 8
$ brew install openjdk@17  // 17
$ brew install openjdk@18  // 18
<strong>
</strong><strong>// Чтобы система могла найти java, надо добавить линки в папку /Library/Java/JavaVirtualMachines/
</strong>$ sudo ln -sfn /usr/local/opt/openjdk@8/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-8.jdk
$ sudo ln -sfn /usr/local/opt/openjdk@17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-17.jdk
$ sudo ln -sfn /usr/local/opt/openjdk@18/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-18.jdk
<strong>
</strong><strong>// Add path to openjdk home to jenv
</strong><strong>$ jenv add /Library/Java/JavaVirtualMachines/openjdk-8.jdk/Contents/Home/
</strong>$ jenv add /Library/Java/JavaVirtualMachines/openjdk-11.jdk/Contents/Home/
$ jenv add /Library/Java/JavaVirtualMachines/openjdk-17.jdk/Contents/Home/
$ jenv add /Library/Java/JavaVirtualMachines/openjdk-18.jdk/Contents/Home/
<strong>
</strong><strong>// Set java env
</strong><strong>$ </strong></code></pre>
