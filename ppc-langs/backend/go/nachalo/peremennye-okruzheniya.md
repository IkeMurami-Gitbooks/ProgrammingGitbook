# GOPATH and GOROOT

Раньше модули организовывались через папку GOPATH. Сейчас **крайне рекомендуют** переходить на модульную систему (**go mod**).

## Deprecated

&#x20;**GOROOT** — это переменная, указывающая, где лежит, собственно, вся бинарная сборка Go и исходные коды. Что-то вроде JAVA\_HOME. Устанавливать эту переменную ручками нужно только в тех случаях, если вы ставите Go под Windows не с помощью MSI-инсталлера, а из zip-архива. Ну, или если вы хотите держать несколько версий Go, каждая в своей директории.

&#x20;А вот переменная **GOPATH** очень важна и нужна и её нужно установить обязательно, впрочем только один раз. Это ваш workspace, где будет лежать код и бинарные файлы всего с чем вы будете в Go работать. Поэтому выбирайте удобный вам путь и сохраняйте его в GOPATH.

Всё, сделайте это один раз и забудьте. Создайте только эту директорию, если её еще и нет, и на этом готово. Теперь любой вызов go get github.com/someuser/somelib автоматически будет скачивать исходники в $GOPATH/src, а бинарный результат компиляции складывать в $GOPATH/pkg или $GOPATH/bin (для библиотек и исполняемых файлов соответственно).

В конце-концов, если вам нужны раздельные workspace-ы, всегда можно заменить переменную GOPATH на нужную (в скрипте сборки, к примеру). Так, собственно, и делают.