# GIT --- Cheet Sheet

[Markdown Cheet Sheet](md.md) \
[VIM Cheet Sheet](vim.md)

## Основные команды

### Init (Инициализация)

```
git init
```
*Инициализация нового локального Git репозитория в текущей директории. Создаётся скрытая папка `.git` с локальным репозиторием, в которой хранятся данные для управления версией проекта.*

### Status (Статус)

```
git status
```
*Показывает статус рабочего дерева, включая файлы, которые добавлены, изменены или удалены. Полезно для проверки текущего состояния перед созданием коммитов.*

### Log (История коммитов)

```
git log
```
*Показать историю коммитов в текущей ветке, включая автора, дату и сообщения коммитов.*

```
git log --merge
```
*Показать историю конфликтов при слиянии веток.*

```
git ls-files
```
*Показать список файлов в рабочем дереве*

### Reflog (история всех действий)

```
git reflog
```
*Показать историю всех действий в репозитории, включая создание коммитов, переключение веток и другие операции:*

Эта команда показывает хеши коммитов и временную метку каждой операции. Если коммит не привязан ни к одной ветке, он всё ещё может быть найден в reflog до удаления сборщиком мусора (GC).

Чтобы восстановить Detached commit (коммит не привязанный к ветке), можно переключиться на него или создать ветку, как это показано в [Detached HEAD](#detached-head-(отсоединённый-указатель-head-на-конкретный-коммит)).

### Config (Конфигурация)

```
git config
```
*Управление конфигурацией Git. Настройки могут быть локальными (для одного репозитория), глобальными (для всех репозиториев в системе, который определяется в файле `~/.gitconfig` в домашней директории пользователя).*

```
git config user.name
```
*Получить текущее имя пользователя.*

```
git config user.email
```
*Получить текущий email пользователя.*

```
git config --global user.name "MyName"
```
*Установить глобальное имя пользователя, которое будет использоваться для всех репозиториев.*

```
git config --global user.email "myemail@example.com"
```
*Установить глобальный email пользователя.*

```
git config --list
```
*Список всех настроек конфигурации.*

```
cat ~/.gitconfig
```
*Просмотр содержимого глобального конфигурационного файла Git.*

### Add (Staging Area, область подготовленных изменений)

**Staging Area** (также называется 'индекс', 'область подготовленных изменений') — это промежуточная область, где фиксируются изменения перед их сохранением в истории репозитория. Она позволяет выбрать, какие изменения войдут в следующий коммит.

**Пример:**

* Изначально файл находится в состоянии **Untracked** (неотслеживаемый), при удалении файла его статус в репозитории становится аналогичным.
* После выполнения команды `git add` файл становится **Staged** (готовый к коммиту, отслеживаемый).
* После выполнения команды `git commit` файл переходит в состояние **Commited** (зафиксированный) или **Unmodified** (неизмененный).
* При внесении изменений в файл он становится **Modified** (изменённый).

```
git add
```
*Добавить файл в Staging Area для отслеживания изменений.*

```
git add .
# или
git add -a
```
*Добавить все изменённые файлы в текущем каталоге и подкаталогах.*

```
git add <file>
```
*Добавить конкретный файл.*

```
git add -p
```
*Интерактивно выбрать, какие изменения добавить в Staging Area.*

```
git add -a --amend
```
*Добавить все новые\измененные файлы в Staging Area и внести изменения в последний коммит.*

### Commit (Коммит, фиксация изменений)

```
git commit
```
*Создать коммит с изменениями из Staging Area. Коммит фиксирует состояние проекта в определённый момент времени.*

```
git commit -m "Сообщение коммита"
```
*Создать коммит с сообщением.*

```
git commit -m "commit message" -m "additional commit message"
```
*Создать коммит с сообщением и c дополнительным сообщением*

```
git commit -am "Сообщение коммита"
```
*Добавить все изменённые файлы в Staging Area и создать коммит (новые файлы добавлять вручную через `git add`).*

```
git commit --amend -m "commit message
# or
--no-edit
```
*Изменить последний коммит. Можно обновить сообщение или добавить изменения, которые забыли включить в предыдущий коммит.*

Если не указывать флаг `-m` , то сообщение коммита будет изменено во внешнем редакторе (nano\vim).С флагом `--no-edit` коммит будет изменен с прежним сообщением.

## Ветки и слияние

### Branch (Ветки)

```
git branch
```
*Показать список локальных веток. Текущая ветка будет помечена звёздочкой (\*).*

```
git branch -a
```
*Показать список всех веток (локальные и удалённые).*

```
git branch <новая ветка>
```
*Создать новую ветку. Эта ветка не будет автоматически переключена. Название ветки может содержать только буквы, цифры, дефисы и символы подчёркивания, но может содержать пробелы.*

Cоздания новой ветки не влияет на основную `master` ветку. После разработки новой функциональности на новой ветке, она может быть слита с основной `master` веткой.

```
git branch -d <ветка>
```
*Удалить локальную ветку. До удаления ветки нужно убедиться, что вы не находитесь в этой ветке. Используйте `git branch` для просмотря текущей ветки.*

Если попытаться удалить ветку, находясь в ней, Git выдаст ошибку об использовании ветки. Используйте `git log` и `git branch` для проверки, была ли ветка успешно удалена.

**Важное замечание:** При удалении ветки все невлитые коммиты тоже удаляются. Следует удалять ветку только после того, как её изменения были влиты в главную ветку и ветка больше не нужна.

```
git branch -vv
```
*Показать список веток с информацией об их отслеживании и последнем коммите.*

### Checkout (Переключиться, восстановить)

```
git checkout <ветка\коммит\тег>
```
* Переключиться на существующую ветку.*

Также эта команда позволяет переключиться на другой коммит или тег, откатывает файл к последнему комиту (если указать точку `git checkout .`)

Сохраняйте незакоммиченные изменения перед переключением в [Stash](#stash-(сохранение-изменений%2C-тайник)), чтобы не потерять их.

```
git checkout -b <новая ветка>
```
* Создать новую ветку и переключиться на неё.*

```
git checkout --track <origin\branch>
```
* Создать локальную ветку из ветки удаленного репозитория и переключиться на неё.*

### Merge (Слияние веток)

Слияние веток объединяет изменения из одной ветки в другую. Полезно для интеграции новых функций или исправлений.

Перед слиянием ветки - переключитесь на ту ветку в которую нужно слить изменения с помощью команды `git checkout <ветка>`. При слиянии ветки X в ветку Y, ветка X не удаляется сама по себе.

```
git merge <ветка>
```
*Слить указанную ветку в текущую. Git попытается автоматически объединить изменения.*

```
git merge --abort
```
*Отменить слияние, если возникли конфликты.*

### Switch (Переключиться на ветку)

```
git switch <ветка>
```
*Переключиться на другую ветку.*

Альтернатива `git checkout` для работы с ветками. В отличие от `git checkout`, команда `git switch` специализируется только на переключении веток и включает дополнительные проверки безопасности. Например, она автоматически остановит операцию, если обнаружит риск потери несохраненных локальных изменений. Однако с помощью ключа `--detach` можно переключиться на конкретный коммит или тег.

```
git switch -c <новая ветка>
```
*Создать новую ветку и переключиться на неё.*

```
git switch --detach <коммит>
```
*Переключиться на конкретный коммит или тег в режиме Detached HEAD.*

### Tag (Теги)

```
git tag
```
*Показать список тегов.*

```
git tag <имя>
```
*Создать новый тег для пометки релиза или значимого состояния.*

### HEAD (Указатель, ссылка на объект фиксации)

**HEAD** — это указатель Git, который показывает на текущий коммит в репозитории. Это тот коммит, от которого будут проходить дальнейшие изменения или коммиты.

При переключении между ветками командой `git checkout`, HEAD автоматически перемещается на последний коммит выбранной ветки. После внесения изменений в файлы, добавления их в Staging area и создания нового коммита, HEAD обновляется и указывает на этот новый коммит в текущей ветке.

[**Detached HEAD**](#detached-head-(отсоединённый-указатель-head-на-конкретный-коммит)) — состояние указателя HEAD, при котором он ссылается на определённый коммит, а не на ветку. Вы можете перейти к конкретному коммиту, чтобы изучить состояние репозитория в точке этого коммита. HEAD будет указывать на выбранный коммит без привязки к ветке.

**Файл `.git/HEAD`:** Внутри скрытой папки `.git` находится файл HEAD, содержащий сведения о текущем положении HEAD. Используя команды типа `cat .git/HEAD`, можно узнать, на какой коммит или ветку в данный момент указывает HEAD.

**Возвращение к ветке:** Чтобы выйти из состояния Detached HEAD и вернуться к работе с ветками, достаточно выполнить `git checkout` на имя ветки, например, в `master`. Таким образом, HEAD снова будет указывать на последний коммит активной ветки.

### Detached HEAD (Отсоединённый указатель HEAD на конкретный коммит)

**Detached HEAD** — состояние, в котором HEAD указывает на конкретный коммит, а не на ветку.

В таком состоянии можно внести изменения в уже совершенный коммит. Как воспроизвести это состояние и изменить коммит:

1. Используем `git checkout` с идентификатором коммита, чтобы перейти в режим отсоединённой HEAD.
2. Вносим необходимые изменения в файлы. Проверьте статус изменений: `git status`
3. Добавьте изменения в индекс: `git add <file>`. Создайте новый коммит с изменениями: `git commit -m "commit message"`.

    Сохраняем изменения:

    Создайте новую ветку: `git branch <new-branch-name>`

    Переключитесь на неё: `git switch <new-branch-name>`
4. После этого можно слить новую ветку. Переключитьесь в ту ветку в которую нужно слить изменения, например `git switch master`. Соедините изменения из созданной ветки `git merge <new-branch-name>`. В случае конфликтов:

    Разрешите конфликты вручную в файлах или через редактор (VS Code).

    Добавьте исправленные файлы в индекс `git add <file>` и завершите слияние `git commit`

Рекомендуется просматривать наличие таких коммитов через [`git reflog`](#reflog-(история-всех-действий)).

### Stash (сохранение изменений, тайник)

**Stash** используется для временного сохранения изменений в рабочем дереве, чтобы вы могли переключиться на другую ветку или выполнить другие операции, не затрагивая текущие изменения. Раньше без этой команды нужно было коммитить изменения, создавать новую ветку, а затем удалить коммит из исходной ветки.

```
git stash
```
*Сохранить изменения в стек.*

```
git stash list
```
*Показать список сохранённых изменений.*

```
git stash apply
```
*Применить последние сохранённые изменения (без удаления из стека).*

```
git stash apply stash@{[index]}
```

Применить изменения из stash по индексу

```
git stash pop
```
*Применить изменения и удалить их из стека.*

```
git stash clear
```
*Удалить все сохранённые изменения.*

## Удаление и откат изменений

Удалить файл можно простым способом, так и альтернативным.

**Простой способ:**

1. Удалить файл с помощью команды `rm -rf <filename>`, либо через графический интерфейс.
2. Проверяем что файл\директория удалены через `git status` и добавляем изменения в Staging area с помощью `git add .`.
3. Создаем коммит с помощью `git commit -m "message"`.

**Альтернативный способ с помощью команды `git rm <filename>`:**

1. Удалить файл с помощью команды
```
git rm <filename>
```
2. Проверяем что файл\директория удалены через `git status` или `git ls-files`.
3. Создаем коммит с помощью `git commit -m "message"`.

### Rm (удаление файлов)

```
git rm <файл>
```
*Удалить файл из рабочего дерева и из индекса.*

```
git rm --cached <файл>
```
*Удалить файл только из индекса (он останется в рабочем дереве).*

```
git rm -r <папка>
```
*Рекурсивно удалить папку и её содержимое.*

```
git rm -f <файл>
```
*Удалить файл принудительно.*

### Restore (откат изменений)

```
git restore <файл>
```
*Отменить изменения в файле, вернув его к состоянию последнего коммита. Эта команда является альтернативой команды `git checkout` для отката изменений.*

```
git restore --staged <файл>
```
*Убрать файл из Staging Area, оставив изменения в рабочей директории.*

### Clean (удаление неотслеживаемых файлов)

Команда `git clean` используется для удаления неотслеживаемых файлов и директорий из рабочего дерева. Неотслеживаемые файлы — это файлы, которые не были добавлены в индекс с помощью `git add` и не были зафиксированы в коммите.

Использование `git clean -n` (или `-dn`) для предварительного просмотра удаляемых файлов и `git clean -f` (или `-df`) для фактического удаления.

Еще раз: `git clean` удаляет только файлы, не добавленные в индекс (не staged)

```
git clean
```
*Удалить неотслеживаемые файлы из рабочего дерева.*

```
git clean -n
```
*Предварительный просмотр файлов, которые будут удалены.*

```
git clean -f
```
*Удалить неотслеживаемые файлы принудительно.*

```
git clean -df
```
*Удалить неотслеживаемые файлы и папки принудительно.*

### Reset (откат изменений)

```
git reset
```
*Отменить изменения в индексе и/или рабочем дереве.*

```
git reset --soft HEAD~1
```
*Отменить последний коммит, оставив изменения в индексе.*

```
git reset HEAD~1
```
*Отменить последний коммит, оставив изменения в рабочем дереве.*

```
git reset --hard HEAD~1
```
*Полностью удалить последний коммит и все изменения.*

## Типы слияния

### Fast-forward merge (прямое слияние)

**Fast Forward Merge** -- это тип слияния, который происходит тогда, когда в основной ветке (main/master) нет новых коммитов с момента создания новой ветки. Указатель основной ветки просто перемещается вперёд.

**Подробнее:**

1. Ветка, куда вы хотите выполнить слияние (например, `main`), не содержит новых коммитов после того момента, когда она разошлась с веткой, которую вы сливаете (например, `feature`).
2. Git просто перемещает указатель текущей ветки (HEAD) на последний коммит целевой ветки, без создания нового коммита слияния. Fast Forward Merge невозможен в том случае, если в основной ветке (`main`\\`master`) были новые коммиты после расхождения с новой веткой, то Fast Forward Merge невозможен. Git выполнит обычное слияние с созданием коммита слияния (merge commit).

### Recursive Merge (рекурсивное слияние)

**Recursive Merge** используется, когда обе ветки (например, `master` и `develop`) содержат изменения, что делает Fast Forward Merge невозможным.

Git объединяет изменения из обеих веток, создавая merge commit (merge branch '<branch>') - коммит слияния.

```
git merge --no-ff <branch>
```
*Принудительное создание merge commit в обход Fast Forwarding Merge происходит с помощью флага `--no-ff`.*

Recursive Merge является стандартным методом, если в обеих ветках есть изменения. При возникновении конфликтов требуется их ручное разрешение.

### Squash Merge (объединение изменений в один коммит из ветки)

**Squash Merge** используется для объединения нескольких коммитов из ветки в один, чтобы упростить и улучшить читаемость истории изменений, особенно в случае, когда ветка содержит множество изменений, фиксов и пр. Полезно для поддержания чистоты истории.

**Как работает:** Все коммиты из сливаемой ветки объединяются в один перед слиянием:
```
git merge --squash <branch>
```
После выполнения `--squash` нужно вручную выполнить коммит:
```
git commit -m "Объединённые изменения из ветки <branch>"
```
## Patching (исправления в репозитории)

### Rebase (переписывание истории коммитов из одной ветки поверх другой)

**Rebase Merge** переписывает историю коммитов, добавляя изменения из одной ветки поверх другой. Используется, чтобы избежать merge commit’ов и сохранить историю более чистой.

```
git rebase <ветка>
```
*Переместить изменения текущей ветки поверх указанной.*

**Как работает:** Команда `git rebase <branch>` переносит все коммиты из текущей ветки (например develop, перед этим переключитесь на неё) на вершину указанной ветки (например master), создавая линейную историю.

Далее нужно переключиться на ветку, в которую вы хотите слить изменения: `git switch <branch>`. Затем выполнить `git merge <branch>` для слияния изменений.

**Осторожно:** Rebase изменяет историю, поэтому не рекомендуется использовать его для веток, которые уже были опубликованы.

### Cherry-pick (применение конкретных изменений из другой ветки в текущую ветку)

```
git cherry-pick <хеш коммита>
```
*Применить изменения из конкретного коммита в текущую ветку.*

## Трекинг файлов

### .gitignore (игнорирование файлов)

Файл **.gitignore** используется для исключения файлов из отслеживания. В нём указываются файлы или директории, которые Git должен игнорировать.

### .gitkeep (отслеживание пустых директорий)

Для отслеживания пустых директорий можно создать файл **.gitkeep**. Он не содержит содержимого, но позволяет Git учитывать директорию.

## Удалённые репозитории

### Clone (клонирование удалённого репозитория)
```
git clone <url>
```
*Клонировать удалённый репозиторий.*

### Remote (добавление удалённого репозитория)
```
git remote add <имя> <url>
```
*Добавить удалённый репозиторий.*

```
git remote -v
```
*Просмотреть список удалённых репозиториев и их URL.*

### Remote tracking branches (ветки синхронизации между локальным и удалённым репозиториями)
(local tracking branches -> remote tracking branches -> origin)

Remote Tracking Branch — это промежуточная ветка, которые используются для синхронизации изменений между локальным и удалённым репозиториями. \
Облегчает синхронизацию локальных изменений с удаленным репозиторием и наоборот, помогает в разрешении конфликтов и слиянии.

**Процесс отправки изменений в удалённый репозиторий:**
Имеется локальная мастер ветка (например, `master`) и удалённая ветка (например, `origin/master`). \
Создание Tracking Branch: \
При выполнении команды `git push`, Git создает Tracking Branch (локальное представление удалённого репозитория). \
Имена веток слежения имеют вид <remote>/<branch>. Например, если вы хотите посмотреть, как выглядела ветка master на сервере origin во время последнего соединения с ним, используйте ветку origin/master. Если вы с коллегой работали над одной задачей и он отправил на сервер ветку iss53, при том что у вас может быть своя локальная ветка iss53, удалённая ветка будет представлена веткой слежения с именем origin/iss53
Отправка изменений: \
Через Tracking Branch изменения отправляются в удалённый репозиторий (Remote). \

**Работа с различными ветками:**
Отправка изменений в новые ветки: Требует явного указания, куда отправить изменения (упоминание о Upstream).
Симуляция работы в команде: Создание и редактирование новой фича-ветки непосредственно в удаленном репозитории через web interface.
Fetch изменений: Команда git fetch позволяет увидеть новые или измененные ветки в удаленном репозитории.
Важность Tracking Branch
Tracking Branch выполняет критически важную роль в процессе синхронизации и объединения изменений между локальными и удаленными репозиториями, обеспечивая эффективную командную работу и конфликт-резолюцию.

### Fetch (получение изменений)
```
git fetch
```
*Получить изменения из удалённого репозитория (без слияния).*

### Pull (получение изменений и слияние)
```
git pull
```
*Получить изменения и слить с текущей веткой (эквивалентно `git fetch` + `git merge`).*

### Push (отправка изменений)
```
git push
```
*Отправить изменения в удалённый репозиторий.*

```
git push --set-upstream <remote(origin)> <branch>
```
*Отправить изменения и установить связь для отслеживания локальной ветки в удалённом репозитории.*

### Local tracking branches
(local tracking branches <- remote tracking branches <- origin)
Local Tracking Branches - это локальные ветки, связанные с удалёнными ветками, позволяющие отслеживать их изменения для удобного выполнения команд `git pull` и `git push`.

Пример использования:
- При работе с веткой Master, которая связана с Remote Origin Master, команда `git pull` влечёт за собой загрузку изменений из удалённой ветки в локальную. Аналогично, `git push` отправляет изменения из локальной ветки в удалённую.
- Установление трекинга для ветки происходило автоматически при её создании, если выполняется `git branch` с подробным выводом, можно увидеть, что локальная ветка master уже отслеживает изменения в origin/master.

```
git branch --track <local_branch> <remote_branch>
```
*Начать трекинг определённой удалённой ветки - это создает локальную ветку и связывает ее с удалённой веткой.*

Практический пример:
- Если нужно отслеживать изменения в удалённой ветке dev/remote, сначала проверяем наличие интересующей удалённой ветки командой `git branch -a`
- Затем создаём локальную ветку и устанавливаем трекинг через `git branch --track dev/remote origin/dev/remote`.
- После выполнения этих шагов, любые изменения в origin/dev/remote будут синхронизироваться с локальной веткой dev/remote при выполнении `git pull`.

### Upstream, Origin (отслеживаемая ветка, название удаленного репозитория)
**Upstream** - это отслеживаемая ветка, связанная с удалённой веткой.
**Origin** - это удаленный репозиторий Git, он может быть назван как угодно, но обычно это просто `origin`.

```
git push -u origin master
```
*Создание внешней ветки (если не существует) и использование ее в качестве Upstream для отправки изменений*

### Удаление remote branches
При попытке удалить удаленную ветку (tracking branch) локально через `git branch -d <origin/branch>>` произойдет ошибка, так как ветка не найдена.

```
git branch -d --remote [-r] <origin/branch>
```
*Удаление удалённой ветки*

Ветка исчезнет из локального списка, но останется в удаленном репозитории. \
Используйте `git ls-remote` для просмотра списка удаленных веток.

Для удаления ветки во внешнем репозитории используйте: `git push origin --delete <branch>`. После этого команда `git ls-remote` покажет, что ветка удалена из удаленного репозитория.

Удаление ветки из удаленного репозитория предотвращает доступ других пользователей к этой ветке через репозиторий. \
Если у кого-то осталась копия ветки локально, он может снова запушить ее изменения, и ветка появится в репозитории заново.

### Force push
Если требуется удалить коммит из истории удаленного репозитория, можно использовать `git reset --hard` для отката на предыдущий коммит локально. \
При повторном выполнении `git pull`, удаленный коммит будет снова загружен, т.к. изменения были только локальными.

```
git push <remote> <branch> --force
```
*Принудительная отправка изменений в удалённый репозиторий*

При попытке выполнить `git push` после отката, Git может выдать ошибку о расхождении версий. \
`git push --force` позволяет переписать историю в удаленном репозитории, но это должно делаться с крайней осторожностью, чтобы не потерять изменения, внесенные другими пользователями. \
Существует риск перезаписи изменений других участников команды, что может привести к потере данных. \
Важно убедиться, что в удаленном репозитории нет новых коммитов от других участников перед выполнением force push.

При необходимости исправить недавний коммит (например, изменить сообщение или добавить забытые изменения), можно использовать команды типа `git commit --amend`, за которым следует `git push --force`. \
Тем не менее, следует проверить, не было ли новых изменений в удаленном репозитории, чтобы не перезаписать их.