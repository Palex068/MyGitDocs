# GB Введение в контроль версий

![000](/GBGit/Pictures/000_001.PNG)

## [Знакомство с контролем версий](/GBGit/001/001.md)

+ [Приветствие](/GBGit/001/001.md#приветствие)
+ [На этом уроке](/GBGit/001/001.md#на-этом-уроке)
+ [Теория](/GBGit/001/001.md#теория)
  + [Что это и для чего может быть нужно](/GBGit/001/001.md#что-это-и-для-чего-может-быть-нужно)
  + [Причины осуществления контроля версий](/GBGit/001/001.md#причины-осуществления-контроля-версий)
  + [Пример 1](/GBGit/001/001.md#пример-1)
  + [Пример 2](/GBGit/001/001.md#пример-2)
  + [Пример 3](/GBGit/001/001.md#пример-3)
  + [Сохранение в играх](/GBGit/001/001.md#сохранение-в-играх)
  + [Пример 4](/GBGit/001/001.md#пример-4)
  + [Недостатки подхода](/GBGit/001/001.md#недостатки-подхода)
  + [Git](/GBGit/001/001.md#git)
  + [Linux](/GBGit/001/001.md#linux)
  + [Принцип работы Git](/GBGit/001/001.md#принцип-работы-git)
  + [Для работы потребуется](/GBGit/001/001.md#для-работы-потребуется)
  + [Наша задача](/GBGit/001/001.md#наша-задача)
+ [Практика](/GBGit/001/001.md#практика)
  + [Установка Git и Visual Studio Code](/GBGit/001/001.md#установка-git-и-visual-studio-code)
  + [Настраиваем Visual Studio Code](/GBGit/001/001.md#настраиваем-visual-studio-code)
  + [Терминал](/GBGit/001/001.md#терминал)
  + [Настраиваем Git](/GBGit/001/001.md#настраиваем-git)
  + [Создаём новый файл](/GBGit/001/001.md#создаём-новый-файл)
  + [Знакомимся с командами Git](/GBGit/001/001.md#знакомимся-с-командами-git)
  + [Изменяем файл](/GBGit/001/001.md#изменяем-файл)
  + [Создаём журнал изменений](/GBGit/001/001.md#создаём-журнал-изменений)
  + [Возвращаемся в актуальное состояние](/GBGit/001/001.md#возвращаемся-в-актуальное-состояние)
  + [Особенности Markdown](/GBGit/001/001.md#особенности-markdown)
  + [Возврат к предыдущим сохранениям](/GBGit/001/001.md#возврат-к-предыдущим-сохранениям)
  + [Добавляем выделение полужирным](/GBGit/001/001.md#добавляем-выделение-полужирным)
  + [Создаём списки](/GBGit/001/001.md#создаём-списки)
  + [Добавляем заголовки и разделы](/GBGit/001/001.md#добавляем-заголовки-и-разделы)
+ [Практическое задание](/GBGit/001/001.md#практическое-задание)
+ [Повторяем команды и элементы разметки в Markdown](/GBGit/001/001.md#повторяем-команды-и-элементы-разметки-в-markdown)
+ [Заключение](/GBGit/001/001.md#заключение)

## [Установка и настройка системы контроля версий](/GBGit/002/002.md)

+ [Вступление](/GBGit/002/002.md#вступление)
+ [На прошлой лекции](/GBGit/002/002.md#на-прошлой-лекции)
  + [Сохранения](/GBGit/002/002.md#сохранения)
  + [Команда git init](/GBGit/002/002.md#команда-git-init)
  + [Команда add](/GBGit/002/002.md#команда-add)
  + [Команда commit](/GBGit/002/002.md#команда-add)
  + [Команда git diff](/GBGit/002/002.md#команда-git-diff)
  + [Команда git log](/GBGit/002/002.md#команда-git-log)
  + [Команда git checkout](/GBGit/002/002.md#команда-git-checkout)
  + [Разберём пример с курсовой или книгой](/GBGit/002/002.md#разберём-пример-с-курсовой-или-книгой)
+ [Практическая часть](/GBGit/002/002.md#практическая-часть)
  + [Создаём новый файл](/GBGit/002/002.md#создаём-новый-файл)
  + [Нюанс начала работы с Git](/GBGit/002/002.md#нюанс-начала-работы-с-git)
  + [Создаём заголовки](/GBGit/002/002.md#создаём-заголовки)
  + [Ветки в Git и их использование на практике](/GBGit/002/002.md#ветки-в-git-и-их-использование-на-практике)
  + [Создаём новую ветку](/GBGit/002/002.md#создаём-новую-ветку)
  + [Редактируем ветку text_formatting](/GBGit/002/002.md#редактируем-ветку-text_formatting)
  + [Структура документа](/GBGit/002/002.md#структура-документа)
  + [Создаём ветку lists](/GBGit/002/002.md#создаём-ветку-lists)
  + [Проверяем структуру файла](/GBGit/002/002.md#проверяем-структуру-файла)
  + [Добавим информацию в text_formatting](/GBGit/002/002.md#добавим-информацию-в-text_formatting)
  + [Совмещение форматирования текста в Markdown](/GBGit/002/002.md#совмещение-форматирования-текста-в-markdown)
  + [Слияние ветвей](/GBGit/002/002.md#слияние-ветвей)
  + [Удаление ветви](/GBGit/002/002.md#удаление-ветви)
  + [Создаём ветку images](/GBGit/002/002.md#создаём-ветку-images)
    + [Добавляем картинку](/GBGit/002/002.md#добавляем-картинку)
  + [Добавление файлов в Git](/GBGit/002/002.md#добавление-файлов-в-git)
  + [Команда .gitignore](/GBGit/002/002.md#команда-gitignore)
  + [Объединение веток images и master](/GBGit/002/002.md#объединение-веток-images-и-master)
  + [Объединение ветвей с разной информацией](/GBGit/002/002.md#объединение-ветвей-с-разной-информацией)
  + [Команда git log -graph](/GBGit/002/002.md#команда-git-log--graph)
+ [Повторим изученное сегодня](/GBGit/002/002.md#повторим-изученное-сегодня)
+ [Окончание занятия](/GBGit/002/002.md#окончание-занятия)

## [Углубляемся в контроль версий](/GBGit/003/003.md)
