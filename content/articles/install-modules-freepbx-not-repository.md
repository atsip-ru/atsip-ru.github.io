+++
title = 'Установка модулей во FreePBX не из репозитория'
date = 2024-11-13T16:08:41+05:00
draft = false
author = 'Dmitriy Q'
+++
Сегодня разберем как установить модуль без доступа к репозиториям Sangoma или вообще без доступа в интернет.

Заходим в модуль FreePBX **Управление модулями** -> **Admin** -> **Module Admin.**

Вкладка **Module Updates** и жмём кнопку **Upload Modules**.

![ed6920f9f60597e1aef493207d3b0643.png](/_resources/ed6920f9f60597e1aef493207d3b0643.png)

Можно выбрать как загрузку из веба, так и с жесткого диска.

![3db0d96a1f5f11e2c928aca71da99805.png](/_resources/3db0d96a1f5f11e2c928aca71da99805.png)

## Загрузка (с жесткого диска) - 2 пункт.

![8ef15e94ea1154f0a891f6f3707af805.png](/_resources/8ef15e94ea1154f0a891f6f3707af805.png)

Жмёте Обзор, выбираете файл со своего компьютера и по нажатию кнопки Upload (From Hard Disk) он загрузится, проверится и при успешной проверке на соответствие модулю установится.

## Скачивание (из веб)

У FreePBX есть 2 репозитория на GitHub - [FreePBX](https://github.com/FreePBX "FreePBX")[](https://github.com/FreePBX-ContributedModules "FreePBX-ContributedModules") и [FreePBX-ContributedModules](https://github.com/FreePBX-ContributedModules "FreePBX-ContributedModules") (видимо для модулей от сообщества).

На примере модуля [Queues](https://github.com/FreePBX/queues/ "Queues")

![abc040e38a6177fa26a24d4bd8e4bff6.png](/_resources/abc040e38a6177fa26a24d4bd8e4bff6.png)

![4892e649dd3706d17d5c40210a304747.png](/_resources/4892e649dd3706d17d5c40210a304747.png)

И выбираете нужную версию, где первое число соответствует версии FreePBX. Копируете ссылку на тарболл.

![e77bc2c09cb631cba01de6bba474d45c.png](/_resources/e77bc2c09cb631cba01de6bba474d45c.png)

И вставляете в строку адреса во FreePBX

![4f56aedea675e49b0c5cf14235a9409c.png](/_resources/4f56aedea675e49b0c5cf14235a9409c.png)

После этого жмёте кнопку **Download (From Web)**, получаете сообщение об успешной установке:

![eabddcd9520066a2f20f4f5bb1de834e.png](/_resources/eabddcd9520066a2f20f4f5bb1de834e.png)

И проверяете наличие модуля в **Управлении модулями**:

![2c10d693699e27c66b2f80ceb01a0217.png](/_resources/2c10d693699e27c66b2f80ceb01a0217.png)

После этого останется выбрать **Install**, **Process** и применить конфигурацию.