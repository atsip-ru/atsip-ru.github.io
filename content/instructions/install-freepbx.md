---
title: 'Установка Freepbx из скрипта'
date: 2025-04-14T09:06:57+05:00
draft: false
author: 'Dmitriy Q'
tags: [freepbx, asterisk, script, install]
---

В противовес официальному скрипту установки я решил сделать скрипт установки FreePBX 16 и Asterisk 18 из исходников на Ubuntu Linux 24.04.
После тестов с 20 версией Asterisk всё заработало и теперь там собирается 20 версия.

Сначала это была версия для собственных нужд, но после решил сделать и публичную, вырезав лишнее.

## Небольшое разъяснение

Скрипт разбит на функции:

```
preinstall
install_dahdi
install_libpri
install_asterisk
install_apache_php
install_mariadb_con
install_nodejs
install_freepbx
install_cdr
set_firewall
set_rinetd
set_tftp
inst_scripts
customize
```

Лишнее можно закомментировать перед запуском скрипта.

* `preinstall` - обновляет систему до свежего состояния и устанавливает некоторые полезные приложения

* `install_dahdi` и `install_libpri` - устанавливают DAHDI от Parabel и библиотеку libpri

* `install_asterisk` - собирает Asterisk из исходников, при этом в начале скрипта можно выбрать править menuselect (выбирает компоненты для сборки) или нет

* `install_apache_php` устанавливает апач и PHP 7.4

* `install_mariadb_con` - устанавливает MariaDB и коннектор к ней, при этом поправляет настройки для ODBC

* `install_nodejs` - установка NodeJS (необходима для FreePBX)

* `install_freepbx` - собственно установка FreePBX 16

* `install_cdr` - установка [Asterisk CDR Viewer Mod](https://github.com/atsip-ru/Asterisk-CDR-Viewer-Mod)

* `set_firewall` - преднастройка iptables - разрешается локальная сеть и DNS Яндекса 77.88.8.8 77.88.8.1

* `set_rinetd` - преднастройка rinetd для ограничения работы 30 минутами

* `set_tftp` - преднастраивается TFTPD

* `inst_scripts` - преднастройка MOTD, скриптов автоудаления голосовой почты и записей вызовов (180 дней) и ротации логов Asterisk

* `customize` - преднастройки вроде длины генерируемого пароля, часового пояса и более лучшей локализации веб-интерфейса

## Установка

* Загрузить каталог [pbx-3-public](https://github.com/atsip-ru/pbx-3-public) на сервер:

`git clone https://github.com/atsip-ru/pbx-3-public.git` или

`wget https://github.com/atsip-ru/pbx-3-public/archive/refs/heads/main.zip -O pbx-3-public.zip && unzip pbx-3-public.zip && mv pbx-3-public-main pbx-3-public`

* переместить в каталог root

* открыть в любом редакторе файл install.sh

```
export DEBIAN_FRONTEND=noninteractive
ASTVERSION=20
FPBXVERSION=16
PHPVERSION="7.4"
LOG_FOLDER="/var/log"
LOG_FILE="${LOG_FOLDER}/freepbx16-install-$(date '+%Y.%m.%d-%H.%M.%S').log"
touch ${LOG_FILE}
log=$LOG_FILE
exec 2>>${LOG_FILE}
CDRV_PASS="GhjcvjnhPdjyrjd"
# DISTRIBUTION="$(lsb_release -is)"
# Переменные
## Часовой пояс
timezone="Asia/Yekaterinburg"
start=$(date +%s)
## Hostname
NAMEPBX=atsip-pbx
codename=noble
## Веб-каталог
WEBROOT="/var/www/html/pbx"
WORK_DIR=$(pwd)
IP_ADDR=$(hostname -I)
```

Можно задать пароль для CDR Viewer, часовой пояс, хостнейм АТС и каталог web, хотя чуть дальше запрашивается правка menuselect, hostname и пароль вьювера.

Также можно закомментировать ненужные функции (например установки DAHDI и libpri) в конце скрипта.

* запустить файл `install.sh` от root

* ответить на вопросы в начале и, возможно, поправить menuselect

* дождаться конца установки и уведомления об этом

