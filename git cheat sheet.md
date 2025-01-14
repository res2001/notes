Книга: https://git-scm.com/book/ru/v2/
https://eax.me/git-commands/
# Ссылки на комиты:
HEAD - текущий коммит
HEAD^ - первый родитель
HEAD^2 - второй родитель (2 родителя могут быть при merge комитах)
HEAD~N - предыдущий N-ый коммит (HEAD^ == HEAD~1; HEAD~2 -  позапрошлый комит). Через тильду переход идет по первым родителям
HEAD~~ - первый родитель первого родителя
HEAD~N^2 - допустимое использование
https://git-scm.com/book/ru/v2/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-Git-%D0%92%D1%8B%D0%B1%D0%BE%D1%80-%D1%80%D0%B5%D0%B2%D0%B8%D0%B7%D0%B8%D0%B8
# Reflog ссылки
HEAD@{1} - предыдущее положение HEAD (может отличаться от предыдущего комита в ветке)
# Инициализация репозитория.
## Инициализировать git репозиторий в текущем каталоге.
```bash
git init
```
После выполнения команды в текущем каталоге появится подкаталог .git, который является каталогом git репозитория.
После создания репозитория текущий каталог становится рабочим каталогом для данного репозитория.
## Инициализировать git репозиторий в текущем каталоге без создания рабочего каталога.
```bash
git init --bare
```
Каталог .git не создается, вместо этого текущий каталог будет являться каталогом репозитория.
В подобном репозитории нельзя выполнять команды работающие с рабочим каталогом репозитория (status, add, commit, ...).
## Инициализация подмодулей после clone
```bash
git submodule update --init --recursive
```
# Журнал и состояние репозитория
## Получить состояние рабочего каталога.
```bash
git status
```
## Получить лог (журнал) репозитория.
```bash
git log
```
## Показать лог репозитория с построением ASCII дерева веток.
```bash
git log --oneline --graph --decorate --all
```
## Короткий лог с деревом тегами, ветками, датой комита
```bash
git log --pretty="%C(yellow)%h %C(cyan)%ad %Cblue%aN%C(auto)%d %Creset%s" --date=short --date-order --graph
```

# Комиты...
## Начать отселживание нового файла name в рабочем каталоге. 
Если name - каталог, то рекурсивно добавляются все файлы/подкаталоги в указанном каталоге.
```bash
git add <name>
```
## Переключение текущей ветки HEAD на другой коммит, указанный в name. 
name может быть именем ветки или хэшем коммита.
```bash
git checkout <name>
```
## Сохранить изменения сделанные в рабочем каталоге в отслеживаемых файлах.
```bash
git commit -a
```
при этом откроется редактор по умолчанию, куда нужно будет добавить описание комита. После сохранения файла и выхода из редактора, команда git commit будет завершена. Новые файлы не будут добавлены в комит. Для того что бы новые файлы были добавлены в комит нужно для них выполнить команду git add.
## Сохранить изменения в рабочем каталоге для отслеживаемых файлов с добавление короткого описания комита.
```bash
git commit -a -m "описание"
```
## Добавить изменения к последнему коммиту
```bash
git add <добавляем в индекс все что нужно>
git commit --amend
```
## Просмотреть изменения в файле, по сравнению с предыдущим коммитом.
```bash
git diff HEAD^ <file name>
```
## Просмотреть изменения между текущим и прошлым комитами
```bash
git diff HEAD HEAD~
```
# Ветки
## Создание ветки name.
Создается новая ветка name в текущей ветке. Переключение на новую ветку не происходит.
```bash
git branch <name>
```
## Создание новой ветки name в текущем комите с одновременным переключением на вновь созданную ветку.
```bash
git checkout -b <name>
```
## Удаление ветки name.
```bash
git branch -d <name>
```
## Удалить ветку в remote репозитории
```bash
git push --delete <remote> <branch>
```
## Очистить все недействительные ссылки на ветки в remote репозитории
```bash
git remote prune <remote>
```
## Очистка недействительные ссылок совмещенное с получением обновлений
```bash
git fetch --prune <remote>
```
Что бы не указывать каждый раз опцию `--prune` можно задать соответствующий параметр в конфиге:
```bash
git config --global fetch.prune true
```
# Отмена
## Отменить изменения в незакомиченном файле:
```bash
git checkout -- <file name>
```
## Удалить проиндексированные изменения
```bash
git reset HEAD
# то же что
git reset --mixed HEAD
```

## Удалить все сделанные изменения в рабочем каталоге.
```bash
git checkout .
```
## Откатить последний коммит.
```bash
# Откат HEAD, индекса и рабочего каталога
git reset --hard HEAD~
# Откат HEAD и индекса
git reset --mixed HEAD~
# Откат только HEAD
git reset --soft HEAD~
```
При этом текущий комит удалится из репозитория. Вместо HEAD~ можно указать любой нижележащий комит, в этом случае откатятся все комиты.
--hard - полностью удаляет комиты.
--mixed - оставляет в рабочем каталоге текущее состояние.
--soft - оставляет в рабочем каталоге и индексе текущее состояние.
## Удалить все сделанные изменения в рабочем каталоге и индексе.
```bash
git reset --hard HEAD
```
## Удалить все неотслеживаемые файлы в рабочем каталоге
```bash
git clean -fdx
```
# Слияние
## Слияние текущей ветки с веткой name.
```bash
git merge <name>
```
Если коммит, на который указывает name, был прямым потомком текущей ветки, Git просто переместит указатель текущей ветки на ветку name. Это называется "перемотка" - fast-forward, о чем будет указано в выводе команды.
Если коммит, на который указывает name, не является прямым потомком текущей ветки, то происходит трехстороннее слияние. При этом автоматически создается новый комит слияния, в котором объединяются обе ветки. Указатель на текущую ветку переносится в новый комит слияния.
## Перенос изменений из ветки <name> в master, без истории коммитов в ветке <name>.
```bash
git checkout master
git merge --squash <name>
git commit -m "merged <name>"
```
Фактического слияния веток не производится. В master подтянутся все изменения из <name> одним коммитом.
## Слияние текущей ветки с веткой name. Если в процессе слияния произойдет конфликт, то автоматически будет взята наша версия (-Xours) или их (сливаемая) версия (-Xtheirs):
```bash
git merge -Xours <name>
git merge -Xtheirs <name>
```
Не конфликтующие измененя сливаются обычным образом.
## Слияние файлов
```bash
git merge-file ...
```

# [Инструменты Git - Продвинутое слияние](https://git-scm.com/book/ru/v2/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-Git-%D0%9F%D1%80%D0%BE%D0%B4%D0%B2%D0%B8%D0%BD%D1%83%D1%82%D0%BE%D0%B5-%D1%81%D0%BB%D0%B8%D1%8F%D0%BD%D0%B8%D0%B5)
## Сравнение результата слияния с тем, что было в нашей ветке:
```bash
git diff --ours
```
## Сравнение результата слияния с тем, что было в сливаемой ветке:
```bash
git diff --theirs
```
## Сравнение результата слияния с тем, что было в обоих ветках:
```bash
git diff --base
```
## После слияния заново выкачать файл с маркерами конфликта
```bash
git checkout --conflict <file name>
```
Если указать --conflict=diff3, то в маркерах будет присутствовать и базовая ветка.
## После слияния выбрать одну из версий файлов (нашу или их/сливаемую):
```bash
git checkout --ours
git checkout --theirs
```
# Перемещение коммитов на другую ветку.
Интерактивное ребазирование ветки *branch* в *upstream branch*. Если *branch* не указано, то ребазируется текущая ветка.

Ребазированию подлежат все коммиты с текущего и ниже вплоть до коммита в котором branch и upstream branch разошлись. Git сам подбирает все коммиты, которые будут участвовавть в ребазировании.
```bash
git rebase -i <upstream branch> <branch>
```
Послы выдачи команды откроется окно редактора по умолчанию, в котором будут перечислены все коммиты, участвующие в ребазировании. В ходе ребазирования с коммитами можно производить дополнительные манипуляции: сливание коммитов, пропуск и т.п. Краткая справка о манипуляциях над коммитами будет содержатся в окне редактора.

## Принудительно заливает текущую ветку в удаленный репозиторий в ветку *branch*.
Нужна после ребазирования в локальном репозитории.
```bash
git push -f <upstream repo> <branch>
```

## PULL в локальную ветку после git push -f в удаленном репозитории
https://stackoverflow.com/questions/9813816/git-pull-after-forced-update
В удаленном репозитории был принудительно изменен комит, на который ссылается локальная ветка. 
Это может быть после git commit --ammend или после git rebase от другого пользователя.
В локальном репозитории надо удалить комиты, которых уже нет в удаленном репозитории и переместить ветку ветку на новый комит.
### Принимаем изменения из удаленного репозитория:
```bash
git fetch
```
### Удаляем локальные комиты не существующие в удаленном репозитории
```bash
git reset origin/master --hard
# или
git rebase -i origin/master # все комиты в списке помечаем как удаляемые (drop)
```
В примере предполагается, что мы находимся в локальной ветке master, которая ссылается на удаленную ветку origin/master.

# Уборка мусора
## Запуск процедуры сжатия репозитория (garbage collection)
```bash
git gc --prune=now --aggresive
```
# Разное
## Алиас для вывода истории редактируемых веток:
```bash
lastbranches = "!git for-each-ref --sort='-authordate:iso8601' --format=' %(authordate:relative)%09%(refname:short)' refs/heads | less"
```
