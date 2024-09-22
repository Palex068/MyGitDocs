# git-clone - клонировать репозиторий в новый каталог

# Содержание

+ [SYNOPSIS (Краткий обзор)](#synopsis-краткий-обзор)
+ [DESCRIPTION (Описание)](#description-описание)
+ [GIT URLS](#git-urls)
+ [EXAMPLES (Примеры)](#examples-примеры)
+ [CONFIGURATION (Настройки)](#configuration-настройки)

[Оглавление](/GitHelp/README.md)

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

# OPTIONS (Опции)

Eng | Rus
--- | ---
__`-l`__ 
__`--local`__ |
When the repository to clone from is on a local machine, this flag bypasses the normal "Git aware" transport mechanism and clones the repository by making a copy of HEAD and everything under objects and refs directories. The files under `.git/objects/` directory are hardlinked to save space when possible. | Когда репозиторий, из которого нужно клонировать, находится на локальном компьютере, этот флаг обходит обычный транспортный механизм, поддерживающий Git, и клонирует репозиторий, создавая копию HEAD и всего остального в каталогах objects и refs. Файлы в `.git/objects/` каталоге жестко привязаны для экономии места, когда это возможно.
If the repository is specified as a local path (e.g., `/path/to/repo`), this is the default, and `--local` is essentially a no-op. If the repository is specified as a URL, then this flag is ignored (and we never use the local optimizations). Specifying `--no-local` will override the default when `/path/to/repo` is given, using the regular Git transport instead. | Если репозиторий указан как локальный путь (например, `/path/to/repo`), это значение по умолчанию, а параметр `--local`, по сути, не используется. Если репозиторий указан как URL, то этот флаг игнорируется (и мы никогда не используем локальные оптимизации). Указание `--no-local` переопределит значение по умолчанию, когда `/path/to/repo` будет задано, вместо этого используя обычный Git transport.
If the repository’s `$GIT_DIR/objects` has symbolic links or is a symbolic link, the clone will fail. This is a security measure to prevent the unintentional copying of files by dereferencing the symbolic links. | Если репозиторий `$GIT_DIR/objects` имеет символические ссылки или является символической ссылкой, клонирование завершится неудачно. Это мера безопасности для предотвращения непреднамеренного копирования файлов путем разыменования символических ссылок.
NOTE: this operation can race with concurrent modification to the source repository, similar to running `cp -r src dst` while modifying src. | ПРИМЕЧАНИЕ: эта операция может выполняться одновременно с внесением изменений в исходный репозиторий, аналогично выполнению `cp -r src dst` во время внесения изменений src.
__`--no-hardlinks`__ |
Force the cloning process from a repository on a local filesystem to copy the files under the `.git/objects` directory instead of using hardlinks. This may be desirable if you are trying to make a back-up of your repository. | Принудительно запустите процесс клонирования из репозитория в локальной файловой системе, чтобы скопировать файлы в `.git/objects` каталог вместо использования жестких ссылок. Это может быть желательно, если вы пытаетесь создать резервную копию своего репозитория.
__`-s`__ |
__`--shared`__ |
When the repository to clone is on the local machine, instead of using hard links, automatically setup `.git/objects/info/alternates` to share the objects with the source repository. The resulting repository starts out without any object of its own. | Когда клонируемый репозиторий находится на локальном компьютере, вместо использования жестких ссылок автоматически настраивается `.git/objects/info/alternates` совместное использование объектов с исходным репозиторием. Результирующий репозиторий запускается без какого-либо собственного объекта.
NOTE: this is a possibly dangerous operation; do __not__ use it unless you understand what it does. If you clone your repository using this option and then delete branches (or use any other Git command that makes any existing commit unreferenced) in the source repository, some objects may become unreferenced (or dangling). These objects may be removed by normal Git operations (such as `git commit`) which automatically call `git maintenance run --auto`. (See [git-maintenance](https://git-scm.com/docs/git-maintenance).) If these objects are removed and were referenced by the cloned repository, then the cloned repository will become corrupt. | ПРИМЕЧАНИЕ: это, возможно, опасная операция; __не__ используйте ее, если вы не понимаете, что она делает. Если вы клонируете свой репозиторий с помощью этой опции, а затем удаляете ветви (или используете любую другую команду Git, которая делает любую существующую фиксацию без ссылок) в исходном репозитории, некоторые объекты могут стать без ссылок (или зависшими). Эти объекты могут быть удалены обычными операциями Git (такими как `git commit`), которые вызываются автоматически `git maintenance run --auto`. (См. [git-maintenance](https://git-scm.com/docs/git-maintenance).) Если эти объекты удалены и на них ссылались в клонированном репозитории, то клонированный репозиторий станет поврежденным.
Note that running `git repack` without the `--local` option in a repository cloned with `--shared` will copy objects from the source repository into a pack in the cloned repository, removing the disk space savings of `clone --shared`. It is safe, however, to run `git gc`, which uses the `--local` option by default. | Обратите внимание, что запуск `git repack` без `--local` опции в репозитории, клонированном с помощью `--shared`, приведет к копированию объектов из исходного репозитория в пакет в клонированном репозитории, что приведет к экономии дискового пространства `clone --shared`. Однако безопасно запускать `git gc` с использованием параметра `--local`, который используется по умолчанию.
If you want to break the dependency of a repository cloned with `--shared` on its source repository, you can simply run `git repack -a` to copy all objects from the source repository into a pack in the cloned repository. | Если вы хотите разорвать зависимость репозитория, клонированного с помощью `--shared`, от его исходного репозитория, вы можете просто запустить `git repack -a`, чтобы скопировать все объекты из исходного репозитория в пакет в клонированном репозитории.
__`--reference[-if-able] <repository>`__|
If the reference repository is on the local machine, automatically setup `.git/objects/info/alternates` to obtain objects from the reference repository. Using an already existing repository as an alternate will require fewer objects to be copied from the repository being cloned, reducing network and local storage costs. When using the `--reference-if-able`, a non existing directory is skipped with a warning instead of aborting the clone. | Если ссылочный репозиторий находится на локальном компьютере, автоматически настройте `.git/objects/info/alternates` получение объектов из ссылочного репозитория. Использование уже существующего репозитория в качестве альтернативы потребует копирования меньшего количества объектов из клонируемого репозитория, что снизит затраты на сетевое и локальное хранение. При использовании `--reference-if-able` вместо прерывания клонирования пропускается несуществующий каталог с предупреждением.
NOTE: see the NOTE for the `--shared` option, and also the `--dissociate` option. | ПРИМЕЧАНИЕ: смотрите ПРИМЕЧАНИЕ к опции `--shared`, а также параметру `--dissociate`.
__`--dissociate`__ |
Borrow the objects from reference repositories specified with the `--reference` options only to reduce network transfer, and stop borrowing from them after a clone is made by making necessary local copies of borrowed objects. This option can also be used when cloning locally from a repository that already borrows objects from another repository—​the new repository will borrow objects from the same repository, and this option can be used to stop the borrowing. | Заимствуйте объекты из ссылочных репозиториев, указанных с `--reference` параметрами, только для уменьшения передачи данных по сети и прекратите заимствование у них после создания клона путем создания необходимых локальных копий заимствованных объектов. Этот параметр также можно использовать при локальном клонировании из репозитория, который уже заимствует объекты из другого репозитория — новый репозиторий будет заимствовать объекты из того же репозитория, и этот параметр можно использовать, чтобы остановить заимствование.
__`-q`__ |
__`--quiet`__ |
Operate quietly. Progress is not reported to the standard error stream. | Работает тихо. О ходе выполнения не сообщается в стандартном потоке ошибок.
__`-v`__ |
__`--verbose`__ |
Run verbosely. Does not affect the reporting of progress status to the standard error stream. | Выполняется подробно. Не влияет на отчет о состоянии выполнения в стандартном потоке ошибок.
__`--progress`__ |
Progress status is reported on the standard error stream by default when it is attached to a terminal, unless `--quiet` is specified. This flag forces progress status even if the standard error stream is not directed to a terminal. | Статус выполнения отображается в стандартном потоке ошибок по умолчанию, когда он подключен к терминалу, если не указано `--quiet`. Этот флаг определяет статус выполнения, даже если стандартный поток ошибок не направлен на терминал.
__`--server-option=<option>`__ |
Transmit the given string to the server when communicating using protocol version 2. The given string must not contain a NUL or LF character. The server’s handling of server options, including unknown ones, is server-specific. When multiple `--server-option=<option>` are given, they are all sent to the other side in the order listed on the command line. | Передайте указанную строку на сервер при обмене данными с использованием протокола версии 2. Данная строка не должна содержать символа NUL или LF. Обработка сервером параметров сервера, включая неизвестные, зависит от сервера. Когда задано несколько `--server-option=<option>` файлов, все они отправляются другой стороне в порядке, указанном в командной строке.
__`-n`__ |
__`--no-checkout`__ |
No checkout of HEAD is performed after the clone is complete. | После завершения клонирования проверка HEAD не выполняется.
__`--[no-]reject-shallow`__ |
Fail if the source repository is a shallow repository. The `clone.rejectShallow` configuration variable can be used to specify the default. | Сбой, если исходный репозиторий является неглубоким репозиторием. Для указания значения по умолчанию можно использовать переменную конфигурации `clone.rejectShallow`.
__`--bare`__ |
Make a __bare__ Git repository. That is, instead of creating `<directory>` and placing the administrative files in `<directory>/.git`, make the `<directory>` itself the `$GIT_DI`R. This obviously implies the `--no-checkout` because there is nowhere to check out the working tree. Also the branch heads at the remote are copied directly to corresponding local branch heads, without mapping them to `refs/remotes/origin/`. When this option is used, neither remote-tracking branches nor the related configuration variables are created. | Создайте __чистый__ репозиторий Git. То есть, вместо создания `<directory>` и размещения административных файлов в `<directory>/.git`, сделайте `<directory>` сам `$GIT_DI`R. Это, очевидно, подразумевает `--no-checkout`, потому что негде проверить рабочее дерево. Также заголовки ветвей на удаленном компьютере копируются непосредственно в соответствующие локальные заголовки ветвей, без их сопоставления с `refs/remotes/origin/`. При использовании этой опции ни ветви удаленного отслеживания, ни связанные переменные конфигурации не создаются.
__`--sparse`__ |
Employ a sparse-checkout, with only files in the toplevel directory initially being present. The [git-sparse-checkout](https://git-scm.com/docs/git-sparse-checkout) command can be used to grow the working directory as needed. | Используйте разреженную проверку, при которой изначально присутствуют только файлы в каталоге верхнего уровня. Команда [git-sparse-checkout](https://git-scm.com/docs/git-sparse-checkout) может использоваться для расширения рабочего каталога по мере необходимости.
__`--filter=<filter-spec>`__ |
Use the partial clone feature and request that the server sends a subset of reachable objects according to a given object filter. When using `--filter`, the supplied `<filter-spec>` is used for the partial clone filter. For example, `--filter=blob:none` will filter out all blobs (file contents) until needed by Git. Also, `--filter=blob:limit=<size>` will filter out all blobs of size at least `<size>`. For more details on filter specifications, see the `--filter` option in [git-rev-list](https://git-scm.com/docs/git-rev-list). | Используйте функцию частичного клонирования и запросите, чтобы сервер отправил подмножество доступных объектов в соответствии с заданным фильтром объектов. При использовании `--filter` предоставленный `<filter-spec>` используется для фильтра частичного клонирования. Например, `--filter=blob:none` будет отфильтровывать все большие двоичные объекты (содержимое файла) до тех пор, пока они не понадобятся Git. Также `--filter=blob:limit=<size>` будет отфильтровывать все большие двоичные объекты большего размера `<size>`. Для получения более подробной информации о спецификациях фильтров смотрите опцию `--filter` в [git-rev-list](https://git-scm.com/docs/git-rev-list).
__`--also-filter-submodules`__ |
Also apply the partial clone filter to any submodules in the repository. Requires `--filter` and `--recurse-submodules`. This can be turned on by default by setting the `clone.filterSubmodules` config option. | Также примените фильтр частичного клонирования к любым подмодулям в репозитории. Требуется `--filter` и `--recurse-submodules`. Это можно включить по умолчанию, установив `clone.filterSubmodules` параметр.
__`--mirror`__ |
Set up a mirror of the source repository. This implies `--bare`. Compared to `--bare`, `--mirror` not only maps local branches of the source to local branches of the target, it maps all refs (including remote-tracking branches, notes etc.) and sets up a `refspec` configuration such that all these refs are overwritten by a `git remote update` in the target repository. | Настройте зеркало исходного репозитория. Это подразумевает `--bare`. По сравнению с `--bare`, `--mirror` не только сопоставляет локальные ветви источника с локальными ветвями целевого объекта, но и сопоставляет все ссылки (включая ветви удаленного отслеживания, заметки и т.д.) И настраивает конфигурацию `refspec` таким образом, что все эти ссылки перезаписываются `git remote update` в целевом репозитории.
__`-o <name>`__ |
__`--origin <name>`__ |
Instead of using the remote name `origin` to keep track of the upstream repository, use `<name>`. Overrides `clone.defaultRemoteName` from the config. | Вместо использования удаленного имени `origin` для отслеживания вышестоящего репозитория используйте `<name>`. Переопределяет `clone.defaultRemoteName` из конфигурации.
__`-b <name>`__ |
__`--branch <name>`__ |
Instead of pointing the newly created HEAD to the branch pointed to by the cloned repository’s HEAD, point to `<name>` branch instead. In a non-bare repository, this is the branch that will be checked out. `--branch` can also take tags and detaches the HEAD at that commit in the resulting repository. | Вместо указания вновь созданного заголовка на ветвь, на которую указывает HEAD клонированного репозитория, вместо этого укажите на `<name>` ветвь. В неосновном репозитории это ветвь, которая будет извлечена. `--branch` также может принимать теги и отсоединять HEAD при этом коммите в результирующем репозитории.
__`-u <upload-pack>`__ |
__`--upload-pack <upload-pack>`__ |
When given, and the repository to clone from is accessed via ssh, this specifies a non-default path for the command run on the other end. | Когда задано, и доступ к репозиторию для клонирования осуществляется через ssh, это указывает путь, отличный от пути по умолчанию для команды, выполняемой на другом конце.
__--template=<template-directory>__ |
Specify the directory from which templates will be used; (See the "TEMPLATE DIRECTORY" section of [git-init](https://git-scm.com/docs/git-init).) | Укажите каталог, из которого будут использоваться шаблоны; (Смотрите раздел "КАТАЛОГ ШАБЛОНОВ" [git-init](https://git-scm.com/docs/git-init).)
__`-c <key>=<value>`__ |
__`--config <key>=<value>`__ |
Set a configuration variable in the newly-created repository; this takes effect immediately after the repository is initialized, but before the remote history is fetched or any files checked out. The key is in the same format as expected by [git-config](https://git-scm.com/docs/git-config) (e.g., `core.eol=true`). If multiple values are given for the same key, each value will be written to the config file. This makes it safe, for example, to add additional fetch refspecs to the origin remote. | Установите переменную конфигурации во вновь созданном репозитории; это вступает в силу сразу после инициализации репозитория, но до извлечения удаленной истории или каких-либо файлов. Ключ находится в том же формате, который ожидается в [git-config](https://git-scm.com/docs/git-config) (например, `core.eol=true`). Если для одного и того же ключа задано несколько значений, каждое значение будет записано в файл конфигурации. Это делает безопасным, например, добавление дополнительных ссылок на выборку к удаленному источнику.
Due to limitations of the current implementation, some configuration variables do not take effect until after the initial fetch and checkout. Configuration variables known to not take effect are: `remote.<name>.mirror` and `remote.<name>.tagOpt`. Use the corresponding `--mirror` and `--no-tags` options instead. | Из-за ограничений текущей реализации некоторые переменные конфигурации вступают в силу только после первоначальной выборки и извлечения. Известно, что переменные конфигурации не вступают в силу: `remote.<name>.mirror` и `remote.<name>.tagOpt`. Вместо этого используйте соответствующие опции `--mirror` и `--no-tags`.
__`--depth <depth>`__ |
Create a __shallow__ clone with a history truncated to the specified number of commits. Implies `--single-branch` unless `--no-single-branch` is given to fetch the histories near the tips of all branches. If you want to clone submodules shallowly, also pass `--shallow-submodules`. | Создайте __мелкий__ клон с историей, урезанной до указанного количества коммитов. Подразумевает, что для извлечения историй рядом с концами всех ветвей задано значение `--single-branch` если `--no-single-branch` не указано. Если вы хотите клонировать подмодули неглубоко, также передайте `--shallow-submodules`.
__`--shallow-since=<date>`__ |
Create a shallow clone with a history after the specified time. | Создайте мелкий клон с историей по истечении указанного времени.
__`--shallow-exclude=<revision>`__ |
Create a shallow clone with a history, excluding commits reachable from a specified remote branch or tag. This option can be specified multiple times. | Создайте мелкий клон с историей, исключая коммиты, доступные из указанной удаленной ветви или тега. Этот параметр можно указывать несколько раз.
__`--[no-]single-branch`__ |
Clone only the history leading to the tip of a single branch, either specified by the `--branch` option or the primary branch remote’s `HEAD` points at. Further fetches into the resulting repository will only update the remote-tracking branch for the branch this option was used for the initial cloning. If the HEAD at the remote did not point at any branch when `--single-branch` clone was made, no remote-tracking branch is created. | Клонируйте только историю, ведущую к началу одной ветви, либо указанную `--branch` опцией, либо основную ветвь, на которую `HEAD` указывает remote. Дальнейшие выборки в результирующий репозиторий обновят ветку удаленного отслеживания только для ветки, эта опция использовалась для первоначального клонирования. Если ЗАГОЛОВОК на удаленном компьютере не указывал ни на одну ветвь, когда `--single-branch` было сделано клонирование, ветвь удаленного отслеживания не создается.
__`--no-tags`__ |
Don’t clone any tags, and set `remote.<remote>.tagOpt=--no-tags` in the config, ensuring that future `git pull` and `git fetch` operations won’t follow any tags. Subsequent explicit tag fetches will still work, (see [git-fetch](https://git-scm.com/docs/git-fetch)). | Не клонируйте никаких тегов и установите `remote.<remote>.tagOpt=--no-tags` в конфигурации, гарантируя, что будущие `git pull` и `git fetch` операции не будут следовать никаким тегам. Последующие явные выборки тегов все равно будут работать (см. [git-fetch](https://git-scm.com/docs/git-fetch)).
Can be used in conjunction with `--single-branc`h to clone and maintain a branch with no references other than a single cloned branch. This is useful e.g. to maintain minimal clones of the default branch of some repository for search indexing. | Может использоваться совместно с `--single-branc`h для клонирования и обслуживания ветви без ссылок, отличных от одной клонированной ветви. Это полезно, например, для поддержания минимальных клонов ветви по умолчанию некоторого репозитория для поисковой индексации.
__`--recurse-submodules[=<pathspec>]`__ |
After the clone is created, initialize and clone submodules within based on the provided pathspec. If no pathspec is provided, all submodules are initialized and cloned. This option can be given multiple times for pathspecs consisting of multiple entries. The resulting clone has `submodule.active` set to the provided pathspec, or "." (meaning all submodules) if no pathspec is provided. | После создания клона инициализируйте и клонируйте подмодули внутри на основе предоставленной спецификации пути. Если спецификация пути не указана, все подмодули инициализируются и клонируются. Этот параметр может быть задан несколько раз для спецификаций путей, состоящих из нескольких записей. Результирующий клон имеет `submodule.active` значение предоставленной спецификации путей, или "." (имеются в виду все подмодули), если спецификация пути не указана.
Submodules are initialized and cloned using their default settings. This is equivalent to running `git submodule update --init --recursive <pathspec>` immediately after the clone is finished. This option is ignored if the cloned repository does not have a worktree/checkout (i.e. if any of `--no-checkout`/`-n`, `--bare`, or `--mirror` is given) | Подмодули инициализируются и клонируются с использованием настроек по умолчанию. Это эквивалентно запуску `git submodule update --init --recursive <pathspec>` сразу после завершения клонирования. Этот параметр игнорируется, если в клонированном репозитории нет рабочего дерева / оформления заказа (т.е. если задано какое-либо из `--no-checkout`/`-n`, `--bare` или `--mirror`)
__`--[no-]shallow-submodules`__ |
All submodules which are cloned will be shallow with a depth of 1. | Все клонируемые подмодули будут мелкими с глубиной 1.
__`--[no-]remote-submodules`__
All submodules which are cloned will use the status of the submodule’s remote-tracking branch to update the submodule, rather than the superproject’s recorded SHA-1. Equivalent to passing `--remote` to `git submodule update`. | Все клонированные подмодули будут использовать статус ветви удаленного отслеживания подмодуля для обновления подмодуля, а не записанный SHA-1 суперпроекта. Эквивалентно передаче `--remote` в `git submodule update`.
__`--separate-git-dir=<git-dir>`__ |
Instead of placing the cloned repository where it is supposed to be, place the cloned repository at the specified directory, then make a filesystem-agnostic Git symbolic link to there. The result is Git repository can be separated from working tree. | Вместо того, чтобы размещать клонированный репозиторий там, где он должен быть, поместите клонированный репозиторий в указанный каталог, затем создайте символическую ссылку Git, не зависящую от файловой системы, туда. В результате репозиторий Git может быть отделен от рабочего дерева.
__`-j <n>`__ |
__`--jobs <n>`__ |
The number of submodules fetched at the same time. Defaults to the `submodule.fetchJobs` option. | Количество подмодулей, извлекаемых одновременно. По умолчанию используется `submodule.fetchJobs` параметр.
__`<repository>`__
The (possibly remote) repository to clone from. See the [GIT URLS](#git-urls) section below for more information on specifying repositories. | (возможно, удаленный) репозиторий для клонирования. Смотрите раздел [GIT URLS](#git-urls) ниже для получения дополнительной информации об указании репозиториев.
__`<directory>`__
The name of a new directory to clone into. The "humanish" part of the source repository is used if no directory is explicitly given (`repo` for `/path/to/repo.git` and `foo` `for host.xz:foo/.git`). Cloning into an existing directory is only allowed if the directory is empty. | Имя нового каталога для клонирования. "Человеческая" часть исходного репозитория используется, если явно не указан каталог (`repo` для `/path/to/repo.git` и `foo` `for host.xz:foo/.git`). Клонирование в существующий каталог разрешено только в том случае, если каталог пуст.
__`--bundle-uri=<uri>`__ |
Before fetching from the remote, fetch a bundle from the given `<uri>` and unbundle the data into the local repository. The refs in the bundle will be stored under the hidden `refs/bundle/*` namespace. This option is incompatible with `--depth`, `--shallow-since`, and `--shallow-exclude`. | Перед извлечением с удаленного устройства извлеките пакет из заданного `<uri>` и разделите данные в локальном репозитории. Ссылки в пакете будут храниться в скрытом `refs/bundle/*` пространстве имен. Этот параметр несовместим с `--depth`, `--shallow-since` и `--shallow-exclude`.

# GIT URLS

Eng | Rus
--- | ---
In general, URLs contain information about the transport protocol, the address of the remote server, and the path to the repository. Depending on the transport protocol, some of this information may be absent. | Как правило, URL-адреса содержат информацию о транспортном протоколе, адрес удаленного сервера и путь к репозиторию. В зависимости от транспортного протокола часть этой информации может отсутствовать.
Git supports ssh, git, http, and https protocols (in addition, ftp, and ftps can be used for fetching, but this is inefficient and deprecated; do not use it). | Git поддерживает протоколы ssh, git, http и https (кроме того, для выборки можно использовать ftp и ftps, но это неэффективно и устарело; не используйте его).
The native transport (i.e. git:// URL) does no authentication and should be used with caution on unsecured networks. | Собственный транспорт (т. Е. git:// URL) не выполняет аутентификацию и должен использоваться с осторожностью в незащищенных сетях.
The following syntaxes may be used with them: | С ними могут использоваться следующие синтаксисы:
+ ssh://[user@]host.xz[:port]/path/to/repo.git/<br>
+ git://host.xz[:port]/path/to/repo.git/ <br>
+ http[s]://host.xz[:port]/path/to/repo.git/<br>
+ ftp[s]://host.xz[:port]/path/to/repo.git/

Eng | Rus
--- | ---
An alternative scp-like syntax may also be used with the ssh protocol: | Альтернативный синтаксис, подобный scp, также может использоваться с протоколом ssh:

+ [user@]host.xz:path/to/repo.git/

Eng | Rus
--- | ---
This syntax is only recognized if there are no slashes before the first colon. This helps differentiate a local path that contains a colon. For example the local path foo:bar could be specified as an absolute path or `./foo:bar` to avoid being misinterpreted as an ssh url. | Этот синтаксис распознается только в том случае, если перед первым двоеточием нет косых черт. Это помогает отличить локальный путь, содержащий двоеточие. Например, локальный путь foo:bar может быть указан как абсолютный путь или `./foo:bar` чтобы избежать неправильного толкования как ssh URL.
The ssh and git protocols additionally support ~username expansion: | Протоколы ssh и git дополнительно поддерживают расширение ~username:

+ ssh://[user@]host.xz[:port]/~[user]/path/to/repo.git/
+ git://host.xz[:port]/~[user]/path/to/repo.git/
+ [user@]host.xz:/~[user]/path/to/repo.git/

Eng | Rus
--- | ---
For local repositories, also supported by Git natively, the following syntaxes may be used: | Для локальных репозиториев, также поддерживаемых Git изначально, могут использоваться следующие синтаксисы:
+ /path/to/repo.git/
+ file:///path/to/repo.git/

Eng | Rus
--- | ---
These two syntaxes are mostly equivalent, except the former implies `--local` option. | Эти два синтаксиса в основном эквивалентны, за исключением того, что первый подразумевает параметр `--local` .
`git clone`, `git fetch` and `git pull`, but not `git push`, will also accept a suitable bundle file. See [git-bundle](https://git-scm.com/docs/git-bundle). | `git clone`, `git fetch` и `git pull`, но не `git push`, также будут принимать подходящий файл пакета. Смотрите [git-bundle](https://git-scm.com/docs/git-bundle).
When Git doesn’t know how to handle a certain transport protocol, it attempts to use the remote-<transport> remote helper, if one exists. To explicitly request a remote helper, the following syntax may be used: | Когда Git не знает, как обрабатывать определенный транспортный протокол, он пытается использовать remote-<транспорт> удаленный помощник, если таковой существует. Для явного запроса удаленного помощника может использоваться следующий синтаксис:

+ `<transport>::<address>`

Eng | Rus
--- | ---
where `<address>` may be a path, a server and path, or an arbitrary URL-like string recognized by the specific remote helper being invoked. See [gitremote-helpers](https://git-scm.com/docs/gitremote-helpers) for details. | где `<address>` может быть путем, сервером и путем или произвольной строкой, подобной URL, распознанной конкретным вызываемым удаленным помощником. Подробности см. в разделе [gitremote-helpers](https://git-scm.com/docs/gitremote-helpers).
If there are a large number of similarly-named remote repositories and you want to use a different format for them (such that the URLs you use will be rewritten into URLs that work), you can create a configuration section of the form: | Если существует большое количество удаленных репозиториев с одинаковыми именами и вы хотите использовать для них другой формат (чтобы используемые вами URL-адреса были переписаны в рабочие URL-адреса), вы можете создать раздел конфигурации формы:
`[url "<actual url base>"] insteadOf = <other url base>` |
For example, with this: | Например, с помощью этого:
`[url "git://git.host.xz/"] insteadOf = host.xz:/path/to/ insteadOf = work:` |
a URL like "work:repo.git" or like "host.xz:/path/to/repo.git" will be rewritten in any context that takes a URL to be "git://git.host.xz/repo.git". | URL-адрес типа "work:repo.git" или "host.xz:/path/to/repo.git" будет переписан в любом контексте, который принимает URL-адрес как "git://git.host.xz/repo.git".
If you want to rewrite URLs for push only, you can create a configuration section of the form: | Если вы хотите переписать URL-адреса только для push-рассылки, вы можете создать раздел конфигурации формы:
`[url "<actual url base>"] pushInsteadOf = <other url base>` |
a URL like "git://example.org/path/to/repo.git" will be rewritten to "ssh://example.org/path/to/repo.git" for pushes, but pulls will still use the original URL. | URL-адрес типа "git://example.org/path/to/repo.git" будет перезаписан в "ssh://example.org/path/to/repo.git" для отправки, но при извлечении все равно будет использоваться исходный URL-адрес.

# EXAMPLES (Примеры)

Eng | Rus
--- | ---
Clone from upstream: | Клонировать из восходящего потока: 
```bash
$ git clone git://git.kernel.org/pub/scm/.../linux.git my-linux
$ cd my-linux
$ make
```
Eng | Rus
--- | ---
Make a local clone that borrows from the current directory, without checking things out: | Создайте локальный клон, который заимствует из текущего каталога, без проверки чего-либо:

```bash
$ git clone -l -s -n . ../copy
$ cd ../copy
$ git show-branch
```

Eng | Rus
--- | ---
Clone from upstream while borrowing from an existing local directory: | Клонирование из восходящего потока при заимствовании из существующего локального каталога:

```bash
$ git clone --reference /git/linux.git \
	git://git.kernel.org/pub/scm/.../linux.git \
	my-linux
$ cd my-linux
```
Eng | Rus
--- | ---
Create a bare repository to publish your changes to the public: | Создайте простой репозиторий, чтобы опубликовать ваши изменения для всеобщего доступа:

```bash
$ git clone --bare -l /home/proj/.git /pub/scm/proj.git
```

# CONFIGURATION (Настройки)

Eng | Rus
--- | ---
Everything below this line in this section is selectively included from the [git-config](https://git-scm.com/docs/git-config) documentation. The content is the same as what’s found there: | Все, что находится под этой строкой в этом разделе, выборочно включено из документации [git-config](https://git-scm.com/docs/git-config). Содержимое совпадает с тем, что там найдено:
`init.templateDir` |
Specify the directory from which templates will be copied. (See the "TEMPLATE DIRECTORY" section of [git-init](https://git-scm.com/docs/git-init).) | Укажите каталог, из которого будут скопированы шаблоны. (Смотрите раздел "КАТАЛОГ ШАБЛОНОВ" [git-init](https://git-scm.com/docs/git-init).)
`init.defaultBranch` |
Allows overriding the default branch name e.g. when initializing a new repository. | Позволяет переопределять имя ветки по умолчанию, например, при инициализации нового репозитория.
`clone.defaultRemoteName` |
The name of the remote to create when cloning a repository. Defaults to `origin`, and can be overridden by passing the `--origin` command-line option to git-clone. | Имя удаленного устройства, создаваемого при клонировании репозитория. По умолчанию используется значение `origin`, и его можно переопределить, передав `--origin` параметр командной строки в git-clone.
`clone.rejectShallow` |
Reject to clone a repository if it is a shallow one, can be overridden by passing option `--reject-shallow` in command line. | Отклонить клонирование репозитория, если он мелкий, можно переопределить, передав опцию `--reject-shallow` в командной строке.
`clone.filterSubmodules` |
If a partial clone filter is provided (see `--filter` in [git-rev-list](https://git-scm.com/docs/git-rev-list)) and `--recurse-submodules` is used, also apply the filter to submodules. | Если предусмотрен фильтр частичного клонирования (см. `--filter` В [git-rev-list](https://git-scm.com/docs/git-rev-list)) и `--recurse-submodules` используется, также примените фильтр к подмодулям.