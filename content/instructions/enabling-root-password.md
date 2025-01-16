---
title: 'Включение удаленного входа по паролю для root'
date: 2025-01-16T16:02:38+05:00
author: 'Dmitriy Q'
draft: false
tags: [linux, ubuntu, ssh]
---

После установки Ubuntu понадобилось мне дать удаленный доступ по паролю.
Быстро нагуглил что нужно в конфиге sshd (etc/ssh/sshd_config) раскомментировать поле
с `PermitRootLogin yes`, хорошо, делаю, перезагружаю sshd - не пускает, PuTTY ругается что
вход возможен только по pubkey.

Ищу ещё - нашёл что нужен ещё `PasswordAuthentication yes`, делаю, перезагружаю sshd, но
всё равно не работает!

Путём долгого поиска выясняется что нужен параметр challenge-response, который сейчас
включается параметром `KbdInteractiveAuthentication yes`. Перезагружаю sshd и наконец PuTTY
запросил пароль после ввода логина.

Итак, 3 параметра:

```
PermitRootLogin yes
PasswordAuthentication yes
KbdInteractiveAuthentication yes
```

Но отмечу - ставьте при этом сложные трудноподбираемые пароли или лучше пользуйтесь входом
по ключам.