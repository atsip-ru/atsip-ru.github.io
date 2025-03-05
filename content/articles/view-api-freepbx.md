---
title: 'Краткий обзор API FreePBX'
date: 2025-03-05T16:48:46+05:00
draft: false
author: 'Dmitriy Q'
tags: [freepbx, modules, api]
---

Несколько месяцев назад мне закидывали удочку как создавать экстен во FreePBX, я ковырял запрос создания, но он получался очень длинным.
И тут несколько дней назад в чате Asterisk Russian Community кто-то интересовался как можно создать диапазон номеров, даже кое-кто утверждал что такое было, но
порыскав я не нашёл, зато наткнулся на API FreePBX и даже в gist.gihtub.com одного из разработчиков FreePBX нашёл скрипт удаления экстена с АТС.
Попросил в чате поправить его на диапазон и мне прислали в личку простенький, но рабочий вариант.

Я его поправил для создания одного экстена с добавлением имени и выкладываю (актуальная версия всегда будет [тут](https://gist.github.com/krotesk/76b74c72e5ba934c005a444f5d03b269)).

```php
#!/usr/bin/env php

<?php

/*** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
* Скрипт для быстрого создания экстена
* Требуется как минимум 1 аргумент - номер, второй аргумент может служить именем (описанием)
* 2025-03-05
 *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***/

if (!isset($argv[1])){
    echo "u need 1 arg";
    exit;
}

$settings=array();

if (!isset($argv[2])){
    $settings["name"] = $argv[1];
} else {
    $settings["name"] = $argv[2];
}
    $settings["max_contacts"] = 2;


include '/etc/freepbx.conf';

$FreePBX = FreePBX::Create();

$device=$FreePBX->Core->getDevice($argv[1]);
$user=$FreePBX->Core->getUser($argv[1]);

if($device["user"]){
    echo 'device exist '.$device["user"];
    exit;
}

if($user["name"]){
    echo 'user exist '.$user["name"];
    exit;
}

$FreePBX->Core->processQuickCreate("pjsip", $argv[1], $settings);
```

В изначальном варианте всё делалось через `addUser` и `addDevice`, но порыскав [в исходном коде FreePBX](https://github.com/FreePBX/core/blob/release/17.0/Core.class.php),
нашёл более подходящую функцию `processQuickCreate`, которой воспользовался.

Многие параметры экстена задаются через именованный массив (`$settings`), а ключи аргументов можно узнать в теле функции. Например в теле функции `processQuickCreate` есть аргументы
`name`, `max_contacts`, `emergency_cid`, `callerid`, `secret`, `outboundcid`, `password` и `channel` для DAHDI. Так что использовать другие нет смысла (я пробовал
задавать код языка и транспорт, но они не применяются).

Ну и запускается скрипт командой `extension-add.php 100 Secretar`, как указано в комментарии скрипта второй аргумент можно не указывать и имя будет аналогично номеру.