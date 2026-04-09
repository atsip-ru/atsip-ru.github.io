---
title: 'Обзор Meshview'
date: 2026-04-09T16:03:28+05:00
draft: false
author: 'Dmitriy Q'
tags: [meshtastic]
---

Я расскажу про интересный инструмент визуализации - [MeshView](https://github.com/pablorevilla-meshtastic/meshview).

Он берет данные с MQTT и оформляет их в удобочитаемом и наглядном виде.

Сперва попалась ссылка на Новосибирскую карту https://meshview.meshmon.ru/map где я и заметил визуализацию обмена данными между нодами.

![MeshView Novosibirsk](/_resources/meshview_1.png)

После этого решил развернуть самостоятельно: клонировал репозиторий проекта, поправил конфиг и запустил, затем несколько раз поправлял для отображения сразу своего города, но это немного другая история.

Есть интерактивная карта:

![MeshView Orsk](/_resources/meshview_2.png)

Список всех пакетов, отправляемых нодами:

![MeshView Orsk](/_resources/meshview_3.png)

и граф нод:

![MeshView Orsk](/_resources/meshview_4.png)

и другое...

Все данные берутся с настроенного сервера MQTT, в моём случае с mqtt.onemesh.ru.