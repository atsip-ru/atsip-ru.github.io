+++
title = 'Оценка качества обслуживания'
date = 2024-11-22T12:51:44+05:00
draft = true
+++

Поступил заказ сделать модуль оценки работы оператора и качества оказания услуги.

За основу взял статью [merion networks](https://dzen.ru/a/W_ZfFFGEzACpkDVw).

Всё делалось на FreePBX 15 и Asterisk 18.

1. В файл /etc/asterisk/queues_post_custom.conf вносим редакцию для нужной очереди (например 500) чтобы получить номер ответившего агента (можно и проще, но не срабатывало с IAX):

```
[500](+)
setinterfacevar=yes
```
2. Клепаем само меню в /etc/asterisk/extensions_custom.conf:

```
[assessment]
exten => _X.,1,Answer()
     same => n,Set(CHANNEL(language)=ru)
     same => n,Set(COUNT=1)
     same => n,Wait(1)
     same => n,Playback(custom/qam-intro)
     same => n,Set(TIMEOUT(digit)=1)
     same => n,NoOP(Count: ${COUNT})
     same => n,GotoIf($["${COUNT}" = "1"]?start-1)
     same => n,GotoIf($["${COUNT}" = "2"]?start-2:hh,1)
     same => n(start-1),Background(custom/qam-question-service,m)       ;озвучиваем предложение об оценке оператора
     same => n,WaitExten(10)
     same => n,Goto(t,1)        ;переход к завершению по тайм-ауту
     same => n(start-2),Background(custom/qam-question-operator,m)      ;озвучиваем предложение об оценке сервиса
     same => n,WaitExten(10)
     same => n,Goto(t,1)        ;переход к завершению по тайм-ауту

exten => 1,1,NoOP(${CDR(src)}, ${DIALEDPEERNUMBER}, ${NODEST}, 1, ${COUNT})
   same => n,AGI(survey.php, ${CDR(src)}, ${DIALEDPEERNUMBER}, ${NODEST}, 1, ${COUNT})  ;если клиент вводит 1, то отправляем даные через AGI в скрипт, который отправляет их в SQL
   same => n,Set(COUNT=$[${COUNT} + 1])
   same => n,Goto(_X.,6)

exten => 2,1,AGI(survey.php, ${CDR(src)}, ${DIALEDPEERNUMBER}, ${NODEST}, 2, ${COUNT})
   same => n,Set(COUNT=$[${COUNT} + 1])
   same => n,Goto(_X.,6)

exten => 3,1,AGI(survey.php, ${CDR(src)}, ${DIALEDPEERNUMBER}, ${NODEST}, 3, ${COUNT})
   same => n,Set(COUNT=$[${COUNT} + 1])
   same => n,Goto(_X.,6)

exten => 4,1,AGI(survey.php, ${CDR(src)}, ${DIALEDPEERNUMBER}, ${NODEST}, 4, ${COUNT})
   same => n,Set(COUNT=$[${COUNT} + 1])
   same => n,Goto(_X.,6)

exten => 5,1,AGI(survey.php, ${CDR(src)}, ${DIALEDPEERNUMBER}, ${NODEST}, 5, ${COUNT})
   same => n,Set(COUNT=$[${COUNT} + 1])
   same => n,Goto(_X.,6)

exten => t,1,Playback(custom/zapis_7)
   same => n,Hangup()

exten => _[06-9],1,Playback(custom/zapis_18)
         same => n,Goto(_X.,7)
exten => i,1,Playback(custom/zapis_18)
   same => n,Goto(_X.,7)

exten => hh,1,Playback(custom/qam-end,skip)
    same => n,Hangup()
```

3. Добавить скрипт для обработки и отправки данных в БД /var/lib/asterisk/agi-bin/survey.php
Назначить ему права на исполнение и сменить пользователя/группу на asterisk.
Добавить и файлы из контекста ivr-qam для воспроизведения приветствия и ошибок при тайм-ауте и неверном вводе и также поправить им права.

4. Для срабатывания перехода на голосовое меню после разговора оператора есть 2 варианта: опция `c` для приложения `Queue()` и `g` для `Dial()`.
Выбираю второй вариант. Для этого добавляем в extensions_custom.conf:

```
;Модуль оценки качества обслуживания
[customer-survey]
exten => _X.,1,NoOp(Post call survey beginning)
     same => n,NoOP(Exttocall: ${EXTEN} Dialstring: ${DB(DEVICE/${EXTEN}/dial)})
     same => n,DIAL(${DB(DEVICE/${EXTEN}/dial)},,trg)           ;добавляем опцию g - когда вызываемый абонент вешает трубку, мы продолжим выполнение команд далее по текущему контексту
     same => n,NoOP(Dialstatus: ${DIALSTATUS})  ;проверяем статус
     same => n,GotoIf($["${DIALSTATUS}"="ANSWER"]?answered)     ;проверяем, был ли отвечен вызов
     same => n,GotoIf($["${DIALSTATUS}"="CHANUNAVAIL"]?busy)    ;проверяем, был ли отвечен вызов (доступен ли оператор) 25.06.2021
     same => n,GotoIf($["${DIALSTATUS}"="BUSY"]?busy)           ;проверяем, был ли отвечен вызов
     same => n,GotoIf($["${DIALSTATUS}"="CONGESTION"]?busy)             ;проверяем, был ли отвечен вызов, 14-02-2022 - проверка на DND
     same => n,GotoIf($["${DIALSTATUS}"="NOANSWER"]?busy:answered)      ;проверяем, был ли отвечен вызов
     same => n(busy),Hangup()
     same => n(answered),Goto(assessment,${EXTEN},1)
     same => n,Hangup()
```

5. Ну и теперь необходимо добавлять операторов с этим контекстом, либо вручную

```
[500]
member=Local/2214@customer-survey/n,0,"2214",hint:2214@ext-local
```

либо переопределить в контексте добавления в очереди `macro-toggle-add-agent` в файле /etc/asterisk/extensions_override_freepbx.conf:

> я делал отдельный файл для модуля оценки и инклюдил его в /etc/asterisk/extensions_override_freepbx.conf

```
exten => s,n,ExecIf($["${DB(AMPUSER/${CALLBACKNUM}/queues/qnostate)}" != "ignorestate"]?AddQueueMember(${QUEUENO},Local/${CALLBACKNUM}@customer-survey/n,${DB(QPENALTY/${QUEUENO}/agents/${CALLBACKNUM})},,${DB(AMPUSER/${CALLBACKNUM}/cidname)},hint:${CALLBACKNUM}@ext-local))
exten => s,n,ExecIf($["${DB(AMPUSER/${CALLBACKNUM}/queues/qnostate)}" = "ignorestate"]?AddQueueMember(${QUEUENO},Local/${CALLBACKNUM}@customer-survey/n,${DB(QPENALTY/${QUEUENO}/agents/${CALLBACKNUM})},,${DB(AMPUSER/${CALLBACKNUM}/cidname)}))
```
где вместо `from-queue` подставляем `customer-survey` 5 и 6 приоритетом.
Также заменить и в `macro-toggle-del-agent` чтобы удалялся агент с нужными параметрами (контекстом).

> Скрипт survey.php нужен только для записи переданных данных в базу данных, это делалось для версии Asterisk 11 и просто было перенесено, хотя можно переделать на func_odbc и писать в базу сразу из диалплана.

И после этого была нарисована простая веб-оболочка с формой запроса данных для чтения и отображения данных из базы, где сразу считается средняя оценка и впоследствии был добавлен вывод всех оценок под спойлером.

![web](/_resources/qam_web.png)