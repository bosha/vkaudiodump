vkAudioDump
=====

## About ##

Скрипт предназначен для скачивания аудио записей из популярной социальной сети ВКонтакте.
Для своей работы скрипту не требуется ничего, кроме стандарных Linux утилит которые есть в большинстве современных дистрибутивов "из коробки".
Я его использую для скачивания (синхронизации) всех новых записей из своего плей-листа в фоне (добавлен в планировщик; прекрасно отрабатывает в случае если во время синхронизации "внезапно" ПК выключается)

Возможности:

* Скачивание аудио записей пользователей
* Скачивания аудио записей групп
* Скачивание аудио записей со страниц
* Скачивание определённых плейлистов групп/пользователей
* Синхронизация скачанных аудио записей (скачиваются только новые; не работает для страниц)
* Умеет работать через proxy

## Установка и конфигурация ##

Т.к. это обычный Bash скрипт, то для установки нужно его скачать, и поместить в директорию с остальными исполняемыми файлами. У меня, например, это ~/bin/, но так же можно и в /usr/bin/.

Все параметры можно указать используя аргументы командной строки, либо можно указать в самом скрипте ("захардкодить").

## Использование ##

Первым делом стоит посмотреть список доступных параметров командной строки:

```bash
user@hostname:~$ vkAudioDump --help

usage: "vkAudioDump" --email [EMAIL] --password [PASSWORD] ... [OPTIONS]
Script for downloading audio records from vk.com social network

OPTIONS:
-h,--help       Show this help
-e,--email      Specify email
-p,--password   Specify password
-d,--directory  Directory to where download records
-O,--override   Override already downloaded records if found
-q,--quite      Suppress any output
-P,--proxy      Specify proxy string (e.g., http://login:pass@host.com:23)
-N,--notreally  Don't download records, just print them out
-f,--from-url   Download records from given url (can be group/user/playlist)
-o,--osdnotify  Use notify-osd for script notifications
--sync          Download only new records
--group         Group which records will be downloaded
--user          User which records will be downloaded
--playlist      Playlist ID to download
--cookie        Use exists vk.com cookie
```

Для работы как минимум требуется указать логин и пароль:

```bash
user@hostname:~$ vkAudioDump --email test@mail.com --password 'testpassword'
Authorizing on vk.com ..
No previous authorization found. Authorizing again..
Gettings records list
Downloading current user records
Converting records list..
Downloading records
Downloading: Joe Cocker - Unchain My Heart.mp3
.....
```

Если не указать никаких дополнительных параметров, по-умолчанию, скрипт создаст директорию 'vkAudioDump' в домашней директории, и скачает аудио записи текущего пользователя (того, под которым вошли) в неё.
Можно указать другую директорию используя параметр -d/--directory. В случае её отсутствия - она будет создана.

Иногда бывает нужно посмотреть какие записи будут скачаны. Для этого следует использовать параметр --notreally:

```bash
user@hostname:~$ vkAudioDump --email test@mail.com --password 'testpassword' --notreally
Authorizing on vk.com ..
Checking is exists cookie valid..
Gettings records list
Downloading current user records
Converting records list..

List of all records to be downloaded:
===============================================================================================================================
Joe Cocker         You Can Leave Your Hat On                          https://cs1-48v4.vk-cdn.net/p3/6422cc395174b4.mp3
....
===============================================================================================================================
Total records count to download: 651
```

Прокси сервер указывается в стандартном для Linux формате:

* С авторизацией: http://username:password@hostname.com:3128
* Без авторизации: http://hostname.com:3128

```bash
user@hostname:~$ vkAudioDump --email test@mail.com --password 'testpassword' --proxy "http://user:superpassword@superhost.com:3128"
Using specified proxy
.....
```

В случае если требуется регулярно синхронизировать аудио записи, следует использовать параметр --sync. В этом случае, в указанной директории будет создан файл last.dump, в котором будет хранится список записей. Можно синхронизировать аудио записи любого количества пользователей указывая каждому свою директорию.

Параметр --from-url довольно удобно использовать обернув в "скрипт-обёртку", и в одно нажатие скачивать понравившиеся страницы с аудио записями.
Пример такого "скрипта-обёртки" - файл "vkAudioDumpLinkWrapper".

Остальные параметры должны быть ясны из --help.
