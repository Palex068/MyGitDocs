# git-clone - клонировать репозиторий в новый каталог

# SYNOPSIS (Краткий обзор)

```bash
git clone [--template=<template-directory>]
	  [-l] [-s] [--no-hardlinks] [-q] [-n] [--bare] [--mirror]
	  [-o <name>] [-b <name>] [-u <upload-pack>] [--reference <repository>]
	  [--dissociate] [--separate-git-dir <git-dir>]
	  [--depth <depth>] [--[no-]single-branch] [--no-tags]
	  [--recurse-submodules[=<pathspec>]] [--[no-]shallow-submodules]
	  [--[no-]remote-submodules] [--jobs <n>] [--sparse] [--[no-]reject-shallow]
	  [--filter=<filter> [--also-filter-submodules]] [--] <repository>
	  [<directory>]
```
# DESCRIPTION (Описание)
Eng | Rus
--- | ---
Clones a repository into a newly created directory, creates remote-tracking branches for each branch in the cloned repository (visible using `git branch --remotes`), and creates and checks out an initial branch that is forked from the cloned repository’s currently active branch. | Клонирует репозиторий во вновь созданный каталог, создает ветви удаленного отслеживания для каждой ветви в клонированном репозитории (видимые с помощью `git branch --remotes`), а также создает и проверяет начальную ветвь, которая разветвляется на текущую активную ветвь клонированного репозитория.
After the clone, a plain `git fetch` without arguments will update all the remote-tracking branches, and a `git pull` without arguments will in addition merge the remote master branch into the current master branch, if any (this is untrue when "--single-branch" is given; see below). | После клонирования простая команда `git fetch` без аргументов обновит все ветви удаленного отслеживания, а `git pull` без аргументов дополнительно объединит удаленную главную ветвь с текущей главной ветвью, если таковая имеется (это неверно, когда указано `--single-branch`; см. Ниже).
This default configuration is achieved by creating references to the remote branch heads under `refs/remotes/origin` and by initializing `remote.origin.url` and `remote.origin.fetch` configuration variables. | Эта конфигурация по умолчанию достигается путем создания ссылок на удаленные заголовки ветвей в `refs/remotes/origin` и путем инициализации переменных конфигурации `remote.origin.url` и `remote.origin.fetch`.
OPTIONS | Опции
__-l__ 
__--local__ |
When the repository to clone from is on a local machine, this flag bypasses the normal "Git aware" transport mechanism and clones the repository by making a copy of HEAD and everything under objects and refs directories. The files under `.git/objects/` directory are hardlinked to save space when possible. | Когда репозиторий, из которого нужно клонировать, находится на локальном компьютере, этот флаг обходит обычный транспортный механизм, поддерживающий Git, и клонирует репозиторий, создавая копию HEAD и всего остального в каталогах objects и refs. Файлы в `.git/objects/` каталоге жестко привязаны для экономии места, когда это возможно.
If the repository is specified as a local path (e.g., `/path/to/repo`), this is the default, and `--local` is essentially a no-op. If the repository is specified as a URL, then this flag is ignored (and we never use the local optimizations). Specifying `--no-local` will override the default when `/path/to/repo` is given, using the regular Git transport instead. | Если репозиторий указан как локальный путь (например, `/path/to/repo`), это значение по умолчанию, а параметр `--local`, по сути, не используется. Если репозиторий указан как URL, то этот флаг игнорируется (и мы никогда не используем локальные оптимизации). Указание `--no-local` переопределит значение по умолчанию, когда `/path/to/repo` будет задано, вместо этого используя обычный Git transport.
If the repository’s `$GIT_DIR/objects` has symbolic links or is a symbolic link, the clone will fail. This is a security measure to prevent the unintentional copying of files by dereferencing the symbolic links. | Если репозиторий `$GIT_DIR/objects` имеет символические ссылки или является символической ссылкой, клонирование завершится неудачно. Это мера безопасности для предотвращения непреднамеренного копирования файлов путем разыменования символических ссылок.
NOTE: this operation can race with concurrent modification to the source repository, similar to running `cp -r src dst` while modifying src. | ПРИМЕЧАНИЕ: эта операция может выполняться одновременно с внесением изменений в исходный репозиторий, аналогично выполнению `cp -r src dst` во время внесения изменений src.
__--no-hardlinks__ |
Force the cloning process from a repository on a local filesystem to copy the files under the `.git/objects` directory instead of using hardlinks. This may be desirable if you are trying to make a back-up of your repository. | Принудительно запустите процесс клонирования из репозитория в локальной файловой системе, чтобы скопировать файлы в `.git/objects` каталог вместо использования жестких ссылок. Это может быть желательно, если вы пытаетесь создать резервную копию своего репозитория.
__-s__ |
__--shared__ |
When the repository to clone is on the local machine, instead of using hard links, automatically setup `.git/objects/info/alternates` to share the objects with the source repository. The resulting repository starts out without any object of its own. | Когда клонируемый репозиторий находится на локальном компьютере, вместо использования жестких ссылок автоматически настраивается `.git/objects/info/alternates` совместное использование объектов с исходным репозиторием. Результирующий репозиторий запускается без какого-либо собственного объекта.
NOTE: this is a possibly dangerous operation; do __not__ use it unless you understand what it does. If you clone your repository using this option and then delete branches (or use any other Git command that makes any existing commit unreferenced) in the source repository, some objects may become unreferenced (or dangling). These objects may be removed by normal Git operations (such as `git commit`) which automatically call `git maintenance run --auto`. (See [git-maintenance](https://git-scm.com/docs/git-maintenance).) If these objects are removed and were referenced by the cloned repository, then the cloned repository will become corrupt. | ПРИМЕЧАНИЕ: это, возможно, опасная операция; __не__ используйте ее, если вы не понимаете, что она делает. Если вы клонируете свой репозиторий с помощью этой опции, а затем удаляете ветви (или используете любую другую команду Git, которая делает любую существующую фиксацию без ссылок) в исходном репозитории, некоторые объекты могут стать без ссылок (или зависшими). Эти объекты могут быть удалены обычными операциями Git (такими как `git commit`), которые вызываются автоматически `git maintenance run --auto`. (См. [git-maintenance](https://git-scm.com/docs/git-maintenance).) Если эти объекты удалены и на них ссылались в клонированном репозитории, то клонированный репозиторий станет поврежденным.
Note that running `git repack` without the `--local` option in a repository cloned with `--shared` will copy objects from the source repository into a pack in the cloned repository, removing the disk space savings of `clone --shared`. It is safe, however, to run `git gc`, which uses the `--local` option by default. | Обратите внимание, что запуск `git repack` без `--local` опции в репозитории, клонированном с помощью `--shared`, приведет к копированию объектов из исходного репозитория в пакет в клонированном репозитории, что приведет к экономии дискового пространства `clone --shared`. Однако безопасно запускать `git gc` с использованием параметра `--local`, который используется по умолчанию.
If you want to break the dependency of a repository cloned with `--shared` on its source repository, you can simply run `git repack -a` to copy all objects from the source repository into a pack in the cloned repository. | Если вы хотите разорвать зависимость репозитория, клонированного с помощью `--shared`, от его исходного репозитория, вы можете просто запустить `git repack -a`, чтобы скопировать все объекты из исходного репозитория в пакет в клонированном репозитории.
__--reference[-if-able] <repository>__|
If the reference repository is on the local machine, automatically setup `.git/objects/info/alternates` to obtain objects from the reference repository. Using an already existing repository as an alternate will require fewer objects to be copied from the repository being cloned, reducing network and local storage costs. When using the `--reference-if-able`, a non existing directory is skipped with a warning instead of aborting the clone. | Если ссылочный репозиторий находится на локальном компьютере, автоматически настройте `.git/objects/info/alternates` получение объектов из ссылочного репозитория. Использование уже существующего репозитория в качестве альтернативы потребует копирования меньшего количества объектов из клонируемого репозитория, что снизит затраты на сетевое и локальное хранение. При использовании `--reference-if-able` вместо прерывания клонирования пропускается несуществующий каталог с предупреждением.
NOTE: see the NOTE for the `--shared` option, and also the `--dissociate` option. | ПРИМЕЧАНИЕ: смотрите ПРИМЕЧАНИЕ к опции `--shared`, а также параметру `--dissociate`.
__--dissociate__ |
Borrow the objects from reference repositories specified with the `--reference` options only to reduce network transfer, and stop borrowing from them after a clone is made by making necessary local copies of borrowed objects. This option can also be used when cloning locally from a repository that already borrows objects from another repository—​the new repository will borrow objects from the same repository, and this option can be used to stop the borrowing. | Заимствуйте объекты из ссылочных репозиториев, указанных с `--reference` параметрами, только для уменьшения передачи данных по сети и прекратите заимствование у них после создания клона путем создания необходимых локальных копий заимствованных объектов. Этот параметр также можно использовать при локальном клонировании из репозитория, который уже заимствует объекты из другого репозитория — новый репозиторий будет заимствовать объекты из того же репозитория, и этот параметр можно использовать, чтобы остановить заимствование.
__-q__ |
__--quiet__ |
Operate quietly. Progress is not reported to the standard error stream. | Работает тихо. О ходе выполнения не сообщается в стандартном потоке ошибок.
__-v__ |
__--verbose__ |
Run verbosely. Does not affect the reporting of progress status to the standard error stream. | Выполняется подробно. Не влияет на отчет о состоянии выполнения в стандартном потоке ошибок.
__--progress__ |
Progress status is reported on the standard error stream by default when it is attached to a terminal, unless `--quiet` is specified. This flag forces progress status even if the standard error stream is not directed to a terminal. | Статус выполнения отображается в стандартном потоке ошибок по умолчанию, когда он подключен к терминалу, если не указано `--quiet`. Этот флаг определяет статус выполнения, даже если стандартный поток ошибок не направлен на терминал.
__--server-option=<option>__ |
Transmit the given string to the server when communicating using protocol version 2. The given string must not contain a NUL or LF character. The server’s handling of server options, including unknown ones, is server-specific. When multiple `--server-option=<option>` are given, they are all sent to the other side in the order listed on the command line. | Передайте указанную строку на сервер при обмене данными с использованием протокола версии 2. Данная строка не должна содержать символа NUL или LF. Обработка сервером параметров сервера, включая неизвестные, зависит от сервера. Когда задано несколько `--server-option=<option>` файлов, все они отправляются другой стороне в порядке, указанном в командной строке.
__-n__ |
__--no-checkout__ |
No checkout of HEAD is performed after the clone is complete. | После завершения клонирования проверка HEAD не выполняется.
__--[no-]reject-shallow__ |
Fail if the source repository is a shallow repository. The `clone.rejectShallow` configuration variable can be used to specify the default. | Сбой, если исходный репозиторий является неглубоким репозиторием. Для указания значения по умолчанию можно использовать переменную конфигурации `clone.rejectShallow`.