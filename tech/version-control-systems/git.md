# Git

## Краткая инструкция по созданию git-репозитория и заливки изменений

Создать локальный репозиторий и сделать первый коммит:

```
git init
git add -A
git commit -am "Create project"
```

Смотрим, какие репозитории удаленные нам доступны:

```
git remote -v
```

Запись будет вида: `[локальное название] [url]`\
например: `testrepo https://... (push)`

Удалить ветку репозитория:

```
git remote rm <branch>
```

Если нет записей, мы должны ее сами добавить:

```
git remote add testrepo https://...
```

Загружаем наши изменения на удаленный репозиторий:

```
git push -u testrepo main
```

## Настройка локального репозитория

Указать пользователя (иначе будут конфликты — с рабочей почты писать в личные репозитории):

```
git config --local user.name "Andrey Semakin"
git config --local user.email "andrey.semakin@incountry.com"
```

### Про ветки

Сейчас рекомендуют не master ветку иметь, а `main`, `trunk` и `development`. Зачем каждая из них?

```
Локальные ветки:
$ git branch [-r]
-r - ветки в удаленном репозитории

Переключиться на ветку:
git checkout <branch name>

Задать дефолтную ветк при создании репозитория
git config --global init.defaultBranch <name>

Переименовать текущую ветку
git branch -m <name>

Сохранить ветку в zip-архив:
git archive --format zip --output /full/path/to/zipfile.zip master
```

### .gitignore

Собранные сообществом конфигурации для разных языков и систем: [https://github.com/github/gitignore/](https://github.com/github/gitignore/)

Так же, с помощью `gitignore` можно добавить в репозиторий пустую папку (`bin`, `configs`, ...) Достаточно создать в этой папке файл `.gitignore` со следующим содержимым:

```
# Ignore everything in this directory
*
# Except this file
!.gitignore
```

Другой способ добиться этого же эффекта — добавить любой пустой файл. Негласно стали добавлять `.gitkeep`.

### Синтаксис у Gitignore

```
Любая подстрока (за искл /):
/some/path/te*.go

Любой путь:
/**/te*.go
```

### Удаление папок, загруженных в репозиторий

Есть еще нюанс с `.gitignore`. Как удалить папки, которые уже были загружены в репозиторий, или остались в кэше?

Используйте, чтобы выполнить сухой прогон и посмотреть, что будет удалено:

```
$ git clean -xdn
```

Затем для реального удаления:

```
$ git clean -xdf
```

Вы можете удалить их из репозитория вручную:

```
$ git rm --cached file1 file2 dir/file3
```

Или, если у вас много файлов:

```
$ git rm --cached `git ls-files -i --exclude-from=.gitignore`
```

## Погружаемся

### git status

Показывает:&#x20;

* пути до файлов, которые были изменены по сравнению с последним коммитом&#x20;
* пути, которые имеют разницу между текущей рабочей директорией и проиндексированным файлом&#x20;
* пути до файлов, которые не отслеживаются git (и не попадают в игнорируемые)

Примеры:

```
git status -s - короткий вывод - только файлы + характер изменений
                удобно для грепа
           -v[v] - подробный вывод по всем изменениям (с -vv) - считай diff --git
```

### git stash

About: [https://git-scm.com/book/ru/v1/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-Git-%D0%9F%D1%80%D1%8F%D1%82%D0%B0%D0%BD%D1%8C%D0%B5](https://git-scm.com/book/ru/v1/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-Git-%D0%9F%D1%80%D1%8F%D1%82%D0%B0%D0%BD%D1%8C%D0%B5)

### git diff

Просмотр изменений между ветками/файлами/...

Пример: отслеживание изменений, по сравнению с последнем коммитом (включая отслеживание непроиндексированных файлов)

```
git diff --cached <path>

# show differences between index and working tree
# that is, changes you haven't staged to commit
git diff [filename]

# show differences between current commit and index
# that is, what you're about to commit
# --staged does exactly the same thing, use what you like
git diff --cached [filename]

# show differences between current commit and working tree
git diff HEAD [filename]
```

### git blame

Получение аннотации файла - увидим: кем, какие, когда строчки были изменены. [Подробнее](https://git-scm.com/book/ru/v1/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-Git-%D0%9E%D1%82%D0%BB%D0%B0%D0%B4%D0%BA%D0%B0-%D1%81-%D0%BF%D0%BE%D0%BC%D0%BE%D1%89%D1%8C%D1%8E-Git).

**hunk** - означает одну часть, в которой два файла отличаются => одному диффу файлов соотв несколько hunk'ов

### Про версии и состояния в гите (очень понятно)

```
Этот вопрос можно понять по-разному:

    Что значит вернуться или откатиться: просто посмотреть, изменить содержимое рабочей области, изменить историю Git?
    Что именно откатить: рабочую область (worktree), индекс (область подготовки коммита, staging area), текущую ветку, удаленную ветку?
    К какой позиции откатить: к индексу, к последнему коммиту, к произвольному коммиту?

Обозначим начальную ситуацию на следующей схеме:

               (i) (wt)
A - B - C - D - ? - ?
            ↑
          master
          (HEAD)

A, B, C, D — коммиты в ветке master.
(HEAD) — местоположение указателя HEAD.
(i) — состояние индекса Git. Если совпадает c (HEAD) - пуст. Если нет - содержит изменения, подготовленные к следующему коммиту.
(wt) — состояние рабочей области проекта (working tree). Если совпадает с (i) — нет неиндексированных изменений, если не совпадает — есть изменения.
↑ обозначает коммит, на который указывает определенная ветка или указатель.

Вот решения, в зависимости от задачи:
1. Временно переключиться на другой коммит

Если вам нужно просто переключиться на другой коммит, чтобы, например, посмотреть на его содержимое, достаточно команды git checkout:

git checkout aaaaaa

 (wt)
 (i)
  A - B - C - D
  ↑           ↑
(HEAD)    master

Сейчас репозиторий находится в состоянии «detached HEAD». Чтобы переключиться обратно, используйте имя ветки (например, master):

git checkout master

2. Переключиться на коммит и продолжить работу с него

Если вы хотите продолжить работу с другого коммита, вам понадобится новая ветка. Можно переключиться и создать ее одной командой:

git checkout -b имя-новой-ветки aaaaaa

 (wt)
 (i)
  A - B - C - D
  ↑           ↑
 new       master
(HEAD)

3. Удалить изменения в рабочей области и вернуть ее к состоянию как при последнем коммите.

Начальное состояние:

               (i) (wt)
A - B - C - D - ? - ?
            ↑
          master
          (HEAD)

3.1 Безопасно — с помощью кармана (stash)
3.1.1 Только неиндексированные

Можно удалить прикарманить только те изменения, которые еще не были индексированы (командой add):

git stash save --keep-index

Конечное состояние:

               (wt)
               (i)       
A - B - C - D - ?         ?
            ↑             ↑
          master      stash{0}
          (HEAD)

3.1.2 Индексированные и нет

Эта команда отменяет все индексированные и неиндексированные изменения в рабочей области, сохраняя их в карман (stash).

git stash save

Конечное состояние:

           (wt)
           (i)           
A - B - C - D             ?
            ↑             ↑
          master      stash{0}
          (HEAD)

    Восстановление несохраненных изменений: легко и просто.

git stash apply

Если stash совсем не нужен, его можно удалить.

# удалить последнюю запись кармана
git stash drop

Подробнее про использование stash.

После этого восстановить изменения всё ещё можно, но сложнее: How to recover a dropped stash in Git?
3.2 Опасный способ

    Осторожно! Эта команда безвозвратно удаляет несохраненные текущие изменения из рабочей области и из индекса Если они вам все-таки нужны, воспользуйтесь git stash.

    Восстановление несохраненных изменений: неиндексированные потеряны полностью, но вы можете восстановить то, что было проиндексировано.

Здесь мы будем использовать git reset --hard

Выполняем:

git reset --hard HEAD

Конечное состояние:

           (wt)
           (i)
A - B - C - D - х - х
            ↑
          master
          (HEAD)

4. Перейти к более раннему коммиту в текущей ветке и удалить из нее все последующие (неопубликованные)

    Осторожно! Эта команда переписывает историю Git-репозитория. Если вы уже опубликовали (git push) свои изменения, то этот способ использовать нельзя (см. почему). Используйте вариант из пункта 5 (git revert).

4.1 При этом сохранить изменения в индекс репозитория:

git reset --soft bbbbbb

После этого индекс репозитория будет содержать все изменения от cccccc до dddddd. Теперь вы можете сделать новый коммит (или несколько) на основе этих изменений.

           (wt)
           (i)
A - B - C - D 
    ↑
  master
  (HEAD)

4.2 Сохранить изменения в рабочей области, но не в индексе.

git reset bbbbbb

Эта команда просто перемещает указатель ветки, но не отражает изменения в индексе (он будет пустым).

   (i)     (wt)
A - B - C - D 
    ↑
  master
  (HEAD)

4.3 Просто выбросить изменения.

    Осторожно! Эта команда безвозвратно удаляет несохраненные текущие изменения. Если удаляемые коммиты не принадлежат никакой другой ветке, то они тоже будут потеряны.

    Восстановление коммитов: Используйте git reflog и этот вопрос чтобы найти и восстановить коммиты; иначе сборщик мусора удалит их безвозвратно через некоторое время.

    Восстановление несохраненных изменений: неиндексированные потеряны полностью, но вы можете восстановить то, что было проиндексировано.

Начальное состояние:

               (i) (wt)
A - B - C - D - ? -  ?
            ↑
          master
          (HEAD)

Выполняем:

git reset --hard bbbbbb

Конечное состояние:

   (wt)
   (i)
A - B - C - D - х - х
    ↑
  master
  (HEAD)

5. Отменить уже опубликованные коммиты с помощью новых коммитов

Воспользуйтесь командой git revert. Она создает новые коммиты, по одному на каждый отменяемый коммит. Таким образом, если нужно отменить все коммиты после aaaaaa:

# можно перечислить отменяемые коммиты
git revert bbbbbb cccccc dddddd

# можно задать диапазон от более раннего к более позднему (новому)
git revert bbbbbb..dddddd

# либо в относительных ссылках
git revert HEAD~2..HEAD

# можно отменить коммит слияния, указывая явным образом номер предка (в нашем примере таких нет):
git revert -m 1 abcdef

# после этого подтвердите изменения:
git commit -m'детальное описание, что и почему сделано'
```

## Submodules

Можно использовать для подтягивания зависимостей сторонних в проект или при работе с двумя проектами одновременно.

Например, нам нужен для работы `git@github.com:nEST-Projects/burp-extension-jy-dependency-example.git`.

```
$ git submodule add --name pydep git@github.com:nEST-Projects/burp-extension-jy-dependency-example.git dependencies/pydep
```

После вызова команды: `git submodule update --init` (если у нас есть `.gitsubmodule` файл; `dependecies` как директория не должна быть проиндексирована) в директорию `dependecies/pydep` затянется проект

## Проблемы

`SSL certificate problem: self signed certificate in certificate chain`\
То есть, пушим в удаленный репозиторий, но тк серт самоподписанный, то git выдает ошибку Решение:

```
$ git config http.sslVerify "false"
или
$ git -c http.sslVerify=false clone https://...
```

## Git GUI

SourceTree — [https://www.sourcetreeapp.com/](https://www.sourcetreeapp.com/)

Github Desktop.
