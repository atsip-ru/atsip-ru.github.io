+++
title = 'Multicast Paging в Asterisk'
date = 2024-11-26T11:01:43+05:00
draft = true
+++

![multicast-paging](/_resources/multicast-paging.png)

В настройках Yealink указываем как на скриншоте - адрес и порт, которые будет прослушивать телефон на наличие мультикаста.

В диалплан прописываем экстен для вызова пейджинга:

```
[ext-paging]
exten => *91,hint,Custom:PAGE91
exten => *91,1,NoOP(Multicast Paging)
    same => n,Set(CONNECTEDLINE(name-charset,i)=utf8)
    same => n,Set(CONNECTEDLINE(name,i)=Selector)
    same => n,Set(CONNECTEDLINE(num,i)=*91)
    same => n,Wait(1)
    same => n,Page(MulticastRTP/basic/239.0.0.1:6061)
    same => n,Hangup()
```

И теперь при вызове **\*91** на телефонах, у кого указаны подобные настройки, будет срабатывать пейджинг.