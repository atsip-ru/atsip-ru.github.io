---
title: 'Умная маршрутизация вызовов'
date: 2025-02-18T12:02:50+05:00
author: 'Dmitriy Q'
draft: false
tags: [freepbx, asterisk, custom, inbound, routes]
---
Чтобы разгрузить секретаря, на которого валится множество вызовов с голосового меню
решил сделать умную маршрутизацию чтобы вызовы клиентов направлять на звонивших им операторов,
в том числе полезно учитывая множество переадресаций на сотовые в компании.

Решил зайдествовать func_odbc - добавляем в файл */etc/asterisk/func_odbc.conf*:

```
; это уже должно быть там
[general]
dsn=asteriskcdrdb
prefix=ODBC

;а это наша функция
[MOBILE]
writehandle=asteriskcdrdb
synopsis=The function searches for the extension that last dialed this number.
syntax=${CALLERID(number):-10}
readsql=SELECT cnum FROM cdr WHERE dst like '%${ARG1}' ORDER BY calldate DESC LIMIT 1
```

Добавляем в */etc/asterisk/extensions_custom.conf*:

```
[smart-forwarding]
exten => s,1,NoOP(smart forwarding to the reception from ${CALLERID(number)})
    same => n,Set(NUM=${ODBC_MOBILE(${CALLERID(number):-10})})
    same => n,NoOP(Number is ${NUM})
    same => n,GotoIf($["${NUM}"=""]?secretar:oper)
    same => n(oper),Playback(silence/1)
    same => n,SayDigits(${NUM})
    same => n,Goto(ext-local,${NUM},1)
    same => n,Hangup()
    same => n(secretar),Goto(ext-group,060,1)
    same => n,Hangup()
```
Где через функцию мы находим номер оператора, при нахождении  - озвучиваем его номер и
соединяем, не находим - соединяем с секретарем (у меня группа 060, для внутреннего 100
 будет `ext-local,100,1`).

Теперь во FreePBX выбираем **Admin** -> **Custom Destinations** и добавляем:

![smart_forwarding](/_resources/smart-forwarding.png)

И теперь в нужном IVR по тайм-ауту выбираем созданный **smart forwarding**.
Ну и конечно же всё применить - это пересоздаст диалплан и перечитает его.