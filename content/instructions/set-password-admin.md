---
title: 'Установка админского пароля на Yealink'
date: 2025-02-12T16:07:06+05:00
author: 'Dmitriy Q'
draft: false
tags: [yealink, web]
---
Итак, расскажу про способ смены админского пароля на телефонах Yealink при имеющемся доступе под
пользователем user через веб-интерфейс.

Заходим в веб-интерфейс телефона:

![Yealink](_resources/yealink-set-password.png)

*Settings* -> *Configuration* и там пункт *Import CFG Configuration File*.
Создаем файл с любым именем, но содержание должно быть таким:

```
#!version:1.0.0.1

### This file is the exported MAC-all.cfg.
lang.gui = Russian
lang.wui = Russian
### For security, the following parameters with password haven't been display in this file.
security.user_password = admin:qwerty
security.user_password = user:qwerty
```

Он задает русский язык для интерфейса телефона и его веб-интерефейса и перебивает пароли юзера и
админа на `qwerty`.

НО! На прошивках от IPMatika как правило такая возможность закрыта и пункт *Import CFG Configuration File* отсутствует.
Но если он в наличии - значит лазейка есть и стоит ей воспользоваться.