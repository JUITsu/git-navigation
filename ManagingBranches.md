[EXIT](./readme.md)
# 3.3 Ветвление в Git - Управление ветками

## Управление ветками
Теперь, когда вы уже попробовали создавать, объединять и удалять ветки, пора познакомиться с некоторыми инструментами для управления ветками, которые вам пригодятся, когда вы начнёте использовать ветки постоянно.

Команда `git branch` делает несколько больше, чем просто создаёт и удаляет ветки. При запуске без параметров, вы получите простой список имеющихся у вас веток:
```
$ git branch
  iss53
* master
  testing
```
Обратите внимание на символ `*`, стоящий перед веткой `master`: он указывает на ветку, на которой вы находитесь в настоящий момент (т. е. ветку, на которую указывает `HEAD`). Это означает, что если вы сейчас сделаете коммит, ветка `master` переместится вперёд в соответствии с вашими последними изменениями. Чтобы посмотреть последний коммит на каждой из веток, выполните команду `git branch -v`:
```
$ git branch -v
  iss53   93b412c Fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 Add scott to the author list in the readme
```
Опции `--merged` и `--no-merged` могут отфильтровать этот список для вывода только тех веток, которые слиты или ещё не слиты в текущую ветку. Чтобы посмотреть те ветки, которые вы уже слили с текущей, можете выполнить команду `git branch --merged`:
```
$ git branch --merged
  iss53
* master
```
Ветка `iss53` присутствует в этом списке потому что вы ранее слили её в `master`. Те ветки из этого списка, перед которыми нет символа `*`, можно смело удалять командой `git branch -d`; наработки из этих веток уже включены в другую ветку, так что ничего не потеряется.

Чтобы увидеть все ветки, содержащие наработки, которые вы пока ещё не слили в текущую ветку, выполните команду `git branch --no-merged`:
```
$ git branch --no-merged
  testing
```
Вы увидите оставшуюся ветку. Так как она содержит ещё не слитые наработки, попытка удалить её командой `git branch -d` приведёт к ошибке: 

```
$ git branch -d testing
error: The branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'.

```
Если вы действительно хотите удалить ветку вместе со всеми наработками, используйте опцию `-D`.

***Переименование ветки***
Предположим, у вас есть ветка с именем `bad-branch-name`, и вы хотите изменить её на `corrected-branch-name`, сохранив при этом всю историю. Вместе с этим, вы также хотите изменить имя ветки на удалённом сервере (GitHub, GitLab или другой сервер). Как это сделать?

Переименуйте ветку локально с помощью команды `git branch --move`:
```
$ git branch --move bad-branch-name corrected-branch-name
```
Ветка `bad-branch-name` будет переименована в `corrected-branch-name`, но это изменение пока только локальное. Чтобы все остальные увидели исправленную ветку в удалённом репозитории, отправьте её туда:
```
$ git push --set-upstream origin corrected-branch-name
```
Теперь проверим, где мы сейчас находимся:
```
$ git branch --all
* corrected-branch-name
  main
  remotes/origin/bad-branch-name
  remotes/origin/corrected-branch-name
  remotes/origin/main
```

Обратите внимание, что текущая ветка `corrected-branch-name`, которая также присутствует и на удалённом сервере. Однако, старая ветка всё ещё по-прежнему там, но её можно удалить с помощью команды:
```
$ git push origin --delete bad-branch-name
```
Теперь старое имя ветки полностью заменено исправленным.

***Изменение имени главной ветки***
Переименуйте локальную ветку `master` в `main` с помощью следующей команды:
```
$ git branch --move master main
```
После этого, локальной ветки `master` больше не существует, потому что она была переименована в ветку `main`.
Чтобы все остальные могли видеть новую ветку `main`, вам нужно отправить её в общий репозиторий. Это делает переименованную ветку доступной в удалённом репозитории.

```
$ git push --set-upstream origin main
```
В итоге, состояние репозитория становится следующим:
```
$ git branch --all
* main
  remotes/origin/HEAD -> origin/master
  remotes/origin/main
  remotes/origin/master
```
Ваша локальная ветка master исчезла, так как она заменена веткой `main`. Ветка `main` доступна в удалённом репозитории. Старая ветка `master` всё ещё присутствует в удалённом репозитории. Остальные участники будут продолжать использовать ветку master в качестве основы для своей работы, пока вы не совершите ряд дополнительных действий.

Теперь, для завершения перехода на новую ветку перед вами стоят следующие задачи:

+ Все проекты, которые зависят от текущего, должны будут обновить свой код и/или конфигурацию.

+ Обновите конфигурацию всех запускаемых тестов.

+ Исправьте скрипты сборки и публикации артефактов.

+ Поправьте настройки репозитория на сервере: задайте новую ветку по умолчанию, обновите правила слияния, а также прочие настройки, которые зависят от имени веток.

+ Обновите документацию, исправив ссылки, указывающие на старую ветку.

+ Слейте или отмените запросы на слияние изменений, нацеленные на старую ветку.

После того, как вы выполнили все эти задачи и уверены, что ветка main работает так же, как ветка `master`, вы можете удалить ветку `master`:
```
$ git push origin --delete master
```
