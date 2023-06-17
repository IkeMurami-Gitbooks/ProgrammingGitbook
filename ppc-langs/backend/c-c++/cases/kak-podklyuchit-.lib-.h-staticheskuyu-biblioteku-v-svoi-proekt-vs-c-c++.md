# Как подключить .Lib/.h статическую библиотеку в свой проект VS/C/C++

Альтернативы: \
1\. #pragma comment(lib, "<путь к lib файлу>") \
2\. Указать lib-файлы в свойствах проекта \
Linker->General->Additional Library Directories - указать каталог с lib-файлами. Linker->Input->Additional Dependencies - указать само название lib-файлов. C/C++->General->Additional Include Directories - указать путь к \*.h файлам, чтоб в своих исходниках не прописывать полный путь на диске. \
3\. Если библиотека представляет из себя только h файл, тогда достаточно написать #include "путь к h файлу".
