---
title: 'Включаем уведомления в Telegram для Zabbix'
date: 2025-08-19T12:08:55+05:00
author: 'Dmitriy Q'
draft: false
tags: [telegram, zabbix]
---

Я не буду вдаваться в подробности как создать бота в Телеграм - про это достаточно статей.

## 1 Шаг - включение уведомлений

Первым делом заходим в **Alerts** => **Media Types**, ищем там Telegram и кликаем на **Disabled** чтобы изменить на **Enabled**.
После кликаем уже на **Telegram** для настройки бота.

![Notification in Telegram 1](/_resources/zabbix_telegram_1.png)

Тут указываем `api_parse_mode` - может быть `html` или `markdown` и `api_token` - токен бота.
По желанию можете перевести подсказки в **Message Templates** и обязательно применить нажав **Update**.

## 2 Шаг - настройка уведомлений для пользователя

**Users** => **Users** => имя нужного пользователя => **Media** => **Add**

![Notification in Telegram 2](/_resources/zabbix_telegram_2.png)

- **Type** конечно же выбран должен быть **Telegram**.
- В поле **Send to** указываем свой ID в Телеграм.
- **When active** - дни недели и период времени для отправки уведомлений.
- **Useif severinity** - тип событий для уведомлений.

## 3 Шаг

**Alerts** => **Actions** => **Trigger Actions**

У меня уже было событие **Report problems to Zabbix administrators**, его я и донастроил:

![Notification in Telegram 3](/_resources/zabbix_telegram_3.png)

![Notification in Telegram 4](/_resources/zabbix_telegram_4.png)

Тут можно настроить по разным условиям - Названию события (Event name), Триггеру (Trigger), Узлу (Host), Группе узлов (Host group), Шаблону (Template) и пр...

Я объединяю все узлы в одну группу и потом выбрал группу *VoIP*.

Проверяем галочку **Enabled** и как всегда не забываем нажать **Update**.

Без 3 шага уведомления приходить не будут, а ведь много инструкций в интернете пропускают его (за подсказку спасибо Максиму Данько).