+++
title = 'Оценка качества обслуживания в Asterisk (альтернатива)'
date = 2024-11-25T13:08:32+05:00
draft = false
+++

Альтернативное решение для оценки качества (на примере 200 очереди).

1. В файл */etc/asterisk/queues_post_custom.conf* вносим редакцию для 200 очереди чтобы получить номер ответившего агента (можно и проще, но не срабатывало с IAX):
```
[200](+)
setinterfacevar=yes
```
2. Создаем таблицу `service_survey` в БД `asteriskcdrdb` для значений:
```
CREATE TABLE `service_survey` (
  `num` varchar(20) DEFAULT NULL,
  `operator` varchar(20) DEFAULT NULL,
  `queue` varchar(20) DEFAULT NULL,
  `valuation` varchar(20) DEFAULT NULL,
  `date` datetime DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
```

3. В *func_odbc.conf* добавляем функцию записи в базу:

```
[qam]
writehandle=asteriskcdrdb
writesql=INSERT INTO service_survey (num,operator,queue,valuation,date) VALUES ('${ARG1}','${ARG2}','${ARG3}','${ARG4}',NOW())
```

4. Клепаем само меню в */etc/asterisk/extensions_custom.conf*:
```
[ivr-qam]
exten => s,1,Answer()
   same => n,Wait(1)
   same => n,Set(TIMEOUT(digit)=1)
   same => n,Set(QUEUEAGENT=${CUT(CUT(MEMBERINTERFACE,@,1),/,2)})
   same => n,NoOP(Answered Agent: ${QUEUEAGENT})
   ;запись с просьбой оценить качество предоставленной услуги
   same => n,Background(/var/lib/asterisk/sounds/ru/custom/qam-question-service,m)
   ;Ожидание оценки - 5 секунд, хотя вводить можно уже во время голосового приветствия
   same => n,WaitExten(5)
   same => n,Goto(t,1)

exten => 1,1,NoOP(${CDR(src)}, ${QUEUEAGENT}, ${QUEUENUM}, 1)
   same => n,Set(ODBC_qam(${CDR(src)}, ${QUEUEAGENT}, ${QUEUENUM}, 1, ${COUNT})=)
   same => n,Goto(hh,1)

exten => 2,1,Set(ODBC_qam(${CDR(src)}, ${QUEUEAGENT}, ${QUEUENUM}, 2, ${COUNT})=)
   same => n,Goto(hh,1)

exten => 3,1,Set(ODBC_qam(${CDR(src)}, ${QUEUEAGENT}, ${QUEUENUM}, 3, ${COUNT})=)
   same => n,Goto(hh,1)

exten => 4,1,Set(ODBC_qam(${CDR(src)}, ${QUEUEAGENT}, ${QUEUENUM}, 4, ${COUNT})=)
   same => n,Goto(hh,1)

exten => 5,1,Set(ODBC_qam(${CDR(src)}, ${QUEUEAGENT}, ${QUEUENUM}, 5, ${COUNT})=)
   same => n,Goto(hh,1)

;запись о тайм-ауте получения ответа
exten => t,1,Playback(/var/lib/asterisk/sounds/ru/custom/qam_7)
   same => n,Hangup()

;запись о неверном значении
exten => _[06-9],1,Playback(/var/lib/asterisk/sounds/ru/custom/qam_18)
   same => n,Goto(s,3)
;опять же запись о неверном значении
exten => i,1,Playback(/var/lib/asterisk/sounds/ru/custom/qam_18)
   same => n,Goto(s,3)
;благодарим за оценку и завершаем вызов
exten => hh,1,Playback(/var/lib/asterisk/sounds/ru/custom/qam-end,skip)
    same => n,Hangup()
```
Добавить и файлы из контекста **ivr-qam** для воспроизведения приветствия и ошибок при тайм-ауте и неверном вводе и также поправить им права.

5. Для срабатывания перехода на голосовое меню необходимо добавить приложению `Queue()` опцию `c` и сделать сам переход на ivr-qam в файл *extensions_override_freepbx.conf*:
```
[ext-queues]
;Включаем продолжение диалплана после вызова
exten => 200,17(qoptions),Set(QOPTIONS=tc)
;Включаем переход на голосовое меню с оценкой - сперва проверка статуса очереди на отвеченный вызов
exten => 200,56(gotodest),GotoIf($[${QUEUESTATUS}=TIMEOUT]?timeout:answered)
;Эта строка была из FreePBX
exten => 200,57(timeout),GotoIf($["${QDEST}"=""]?ext-queues,200,1:${CUT(QDEST,^,1)},${CUT(QDEST,^,2)},${CUT(QDEST,^,3)})
;Переход на IVR оценки качества
exten => 200,58(answered),Goto(ivr-qam,s,1)
```

6. Обязательно перезагружаем func_odbc `module reload func_odbc.so` и диалплан `dialplan reload` и проверяем запись в базу.
```
SELECT * FROM service_survey ORDER BY by date DESC LIMIT 10;
```