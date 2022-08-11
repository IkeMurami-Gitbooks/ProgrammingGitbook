# Если не компилируются стандартные библиотеки в Visual Studio 2015

Если только что созданный проект не собирается:

```
**Error C1083** Cannot open include file: 'stdio.h': No such file or directory  ConsoleApplication2 
c:\users\samsung\documents\visual studio 2015\consoleapplication2\consoleapplication2\stdafx.h
```

Причина всему - `виндовс 10`, которая по умолчанию не хочет работать правильно с `visual studio 15`. После загугливания англоязычных интернетов я решил эту проблему. Более детально:

Внутри `*Visual studio*` изменились пути к **C** заголовкам.

Информацию об этой ошибке можно найти здесь: [https://blogs.msdn.microsoft.com/vcblog/2015/03/03/introducing-the-universal-crt/](https://blogs.msdn.microsoft.com/vcblog/2015/03/03/introducing-the-universal-crt/)

Мне удалось решить эту непотребность: Идём в  `Project->Properties->`. В `Configuraton Properties->VC++ Diretories->Library Directories` добавляем пути к `C:\Program Files (x86)\Windows Kits\10\Lib\10.0.10150.0\ucrt\`_(Здесь выбираете вашу архитектуру)_

И в `C/C++->General->Additional include directories` в этой строке добавляем путь:&#x20;

```cpp
C:\Program Files (x86)\Windows Kits\10\Include\10.0.10150.0\ucrt
```

> Примечание: _10.0.10150.0_ может существенно зависеть от вашей версии операционной системы.

**2.** После вышепроделанных манипуляций, у вас может вылезти ошибка `LNK1104 cannot open file 'ucrtd.lib'`. (По крайней мере эта ошибка вылезла у меня). Как её решить, хорошо показано в [этом](https://www.youtube.com/watch?v=XlypjQUr0J8) видео.&#x20;
