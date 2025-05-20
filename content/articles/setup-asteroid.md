---
title: 'Настройка Asteroid для связи с Asterisk'
date: 2025-05-20T12:16:24+05:00
author: 'Dmitriy Q'
draft: false
tags: [asteroid, parabel, linux, putty]
---

Довелось мне на днях разбираться с Asteroid - на самом банке каналов светился Alarm, ну и в dahdi_tool был **RED** Alarm. Проверил линию LAN-тестером - всё в порядке и даже переобжал коннекторы - ошибка остаётся.

Ещё такой непонятный момент - там для сервера приспособлен неттоп с 4 сетевыми портами, если подключать Asteroid в один из этих портов - сервер просто теряет сеть и не пингуется. После отключения его и перезагрузки сервера - всё нормализуется, поэтому я взял внешнюю USB сетевую карту.

Тут попалась на глаза [статья](https://gammatelecom.ru/blog/nastrojka-shlyuza-asteroid-ot-zavoda-parabel) и решил проверить MAC-адрес. Через `minicom` с  Manjaro подключиться не получилось, в итоге пришлось искать ноут с виндой и подключаться через PuTTY со скоростью 38400 и эмуляцией COM-порта.

```
Asteroid monitor, v2.2 30/03/2010, Updates: http://parabel.ru/

Firmware: Asteroid{0xB}, Revision: 0x3, Temperature(C): 38
E1/A Cfg: Line code=HDB3, Clock=Internal, CRC4=On
Slots {0: ALaw,FXS  1: ALaw,FXO  2: ALaw,FXS  3: ALaw,FXS}
E1/A status: LOS=Off, LOF=Off, LOM=Off, LOC=Off, RAIS=Off, FrErr=0/0
TDMoE {status : SkipEr=0, SlipEr=65535, RxNuEr=0}
MAC src: 005555555500, dst: 000000000000
                 1 3 5 7 9 1 3 5 7 9 1 3 5 7 9 1
FXO/FXS state:   ...............................

1. Configuration >>
2. Status >>
3. Test >>
8. Start bootloader
9. Reset
```

нажимаем `1`

```
Configuration:
1. Common >>
2. E1/A >>
4. Slots >>
5. Coding law >>
7. Factory
8. Restore
9. Save
0. Quit
```

нажимаем `1`

```
Configuration/Common:
1. VCO: 0x0
2. MAC: 005555555501
3. DST MAC: 000000000000
4. EC: On
5. E1: Off
0. Quit
```

Вот в этом и косяк - адрес `000000000000`. Нажимаем 3 и вбиваем mac-адрес сетевухи с которой будет идти общение с астероидом, вбиваем без двоеточий маленькими буквами. Затем нажимаем `0`.

```
Configuration:
1. Common >>
2. E1/A >>
4. Slots >>
5. Coding law >>
7. Factory
8. Restore
9. Save
0. Quit
```

нажимаем `9` для сохранения конфига, возможно будет пункт `Reset` для перезагрузки Asteroid.

И после его перезагрузки Alarm погас и `dahdi_tool` выдал **OK**.

Контрольная перезагрузка и после запуска dahdi и всех нужных служб Asterisk по команде `dahdi show status` тоже определил связь с Asteroid

```
*CLI> dahdi show status 
Description                              Alarms  IRQ    bpviol CRC    Fra Codi Options  LBO
Dynamic 'eth' span at 'eth4/00:55:55:55  OK      0      0      0      CAS Unk           0 db (CSU)/0-133 feet (DSX-1)
```

и `dahdi show channels` вывел все 32 канала.

Правда пришлось делать скрипт для проверки и запуска нужных служб, но это уже совсем другая история.

