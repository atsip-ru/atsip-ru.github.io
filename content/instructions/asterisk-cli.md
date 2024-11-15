+++
title = 'Работа с CLI Asterisk'
date = 2024-11-02T15:10:21+05:00
author = 'Dmitriy Q'
draft = false
+++
CLI (Command Line Interface) - очень полезный инструмент диагностики и отладки при работе с Asterisk.
Войти в него можно командой `asterisk -rvv` (или `sudo asterisk -rvv`), где количество `v` (verbose) означает детальность вывода, а `r` подключение к Asterisk.
![7d9e0cd304f7622a74cef7eb2f41e130.png](/_resources/7d9e0cd304f7622a74cef7eb2f41e130.png)
При этом отобразится пользователь, от которого запущен Asterisk, его версия, имя машины, на которой запущено приложение и pid процесса.
`*CLI>` означает интерактивный режим работы в CLI Asterisk.
Список всех **доступных** для Вашего Asterisk команд можно узнать командой `core show help`.
В Asterisk также отлично работает автодополнение команд, т.е. введя например `core show` и нажав клавишу Tab оно предложит все доступные варианты:
![d89cfd64504d754c64e789e35b8936a5.png](/_resources/d89cfd64504d754c64e789e35b8936a5.png)
Ещё одна удобная функция CLI - `like`, тоесть поиск возможных вариантов среди всех доступных, например:
![8717af5c9825cdbd75a593f41b137529.png](/_resources/8717af5c9825cdbd75a593f41b137529.png)
для `like` даже доступны базовые регулярные выражения, такие как якори и диапазоны:
![Снимок экрана от 2024-11-02 15-11-38](/_resources/screenshot-2024-11-02-15-11-38.png)

Также есть 2 типа команд: `show` и `set` - первые абсолютно безвредны и лишь отображают текущее состояние, вторые - изменяют текущее состояние, поэтому советую их использовать крайне осторожно, а лучше менять эти параметры через файлы конфигурации.

Ну и разберу детальность (`v` значит **verbose**) вывода лога при вызове:

1 и 2 уровень:

![Снимок экрана от 2024-11-05](/_resources/screenshot-2024-11-05-09-17-25.png)

3 уровень:

```
atsip-pbx*CLI> core set verbose 3
Console verbose was 2 and is now 3.
  == Using SIP RTP Audio TOS bits 184
  == Using SIP RTP Audio CoS mark 5
    -- Executing [71222@from-internal:1] Macro("PJSIP/002-00000031", "user-callerid,LIMIT,EXTERNAL,") in new stack
    -- Executing [s@macro-user-callerid:1] Set("PJSIP/002-00000031", "TOUCH_MONITOR=1730780250.107") in new stack
    -- Executing [s@macro-user-callerid:2] Set("PJSIP/002-00000031", "CHANCONTEXT=") in new stack
    -- Executing [s@macro-user-callerid:3] Set("PJSIP/002-00000031", "CHANCONTEXT=") in new stack
    -- Executing [s@macro-user-callerid:4] Set("PJSIP/002-00000031", "CHANEXTENCONTEXT=002-00000031") in new stack
    -- Executing [s@macro-user-callerid:5] Set("PJSIP/002-00000031", "CHANEXTEN=002-00000031") in new stack
    -- Executing [s@macro-user-callerid:6] Set("PJSIP/002-00000031", "CALLERID(number)=002") in new stack
    -- Executing [s@macro-user-callerid:7] Set("PJSIP/002-00000031", "AMPUSER=002") in new stack
    -- Executing [s@macro-user-callerid:8] Set("PJSIP/002-00000031", "HOTDESCKCHAN=002-00000031") in new stack
    -- Executing [s@macro-user-callerid:9] Set("PJSIP/002-00000031", "HOTDESKEXTEN=002") in new stack
    -- Executing [s@macro-user-callerid:10] Set("PJSIP/002-00000031", "HOTDESKCALL=0") in new stack
    -- Executing [s@macro-user-callerid:11] ExecIf("PJSIP/002-00000031", "0?Set(HOTDESKCALL=1)") in new stack
    -- Executing [s@macro-user-callerid:12] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(name)=)") in new stack
    -- Executing [s@macro-user-callerid:13] GotoIf("PJSIP/002-00000031", "0?report") in new stack
    -- Executing [s@macro-user-callerid:14] ExecIf("PJSIP/002-00000031", "1?Set(REALCALLERIDNUM=002)") in new stack
    -- Executing [s@macro-user-callerid:15] Set("PJSIP/002-00000031", "AMPUSER=002") in new stack
    -- Executing [s@macro-user-callerid:16] GotoIf("PJSIP/002-00000031", "0?limit") in new stack
    -- Executing [s@macro-user-callerid:17] Set("PJSIP/002-00000031", "AMPUSERCIDNAME=Support-ATC") in new stack
    -- Executing [s@macro-user-callerid:18] ExecIf("PJSIP/002-00000031", "0?Set(__CIDMASQUERADING=TRUE)") in new stack
    -- Executing [s@macro-user-callerid:19] GotoIf("PJSIP/002-00000031", "0?report") in new stack
    -- Executing [s@macro-user-callerid:20] Set("PJSIP/002-00000031", "AMPUSERCID=002") in new stack
    -- Executing [s@macro-user-callerid:21] Set("PJSIP/002-00000031", "__DIAL_OPTIONS=HhTtr") in new stack
    -- Executing [s@macro-user-callerid:22] Set("PJSIP/002-00000031", "CALLERID(all)="Support-ATC" <002>") in new stack
    -- Executing [s@macro-user-callerid:23] ExecIf("PJSIP/002-00000031", "0?Set(CUSDIAL=)") in new stack
    -- Executing [s@macro-user-callerid:24] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(all)="Support-ATC" <002>)") in new stack
    -- Executing [s@macro-user-callerid:25] GotoIf("PJSIP/002-00000031", "0?limit") in new stack
    -- Executing [s@macro-user-callerid:26] ExecIf("PJSIP/002-00000031", "1?Set(GROUP(concurrency_limit)=002)") in new stack
    -- Executing [s@macro-user-callerid:27] NoOp("PJSIP/002-00000031", "Macro Depth is 1") in new stack
    -- Executing [s@macro-user-callerid:28] GotoIf("PJSIP/002-00000031", "1?report2:macroerror") in new stack
    -- Goto (macro-user-callerid,s,29)
    -- Executing [s@macro-user-callerid:29] GotoIf("PJSIP/002-00000031", "1?continue") in new stack
    -- Goto (macro-user-callerid,s,47)
    -- Executing [s@macro-user-callerid:47] Set("PJSIP/002-00000031", "CALLERID(number)=002") in new stack
    -- Executing [s@macro-user-callerid:48] Set("PJSIP/002-00000031", "CALLERID(name)=Support-ATC") in new stack
    -- Executing [s@macro-user-callerid:49] GotoIf("PJSIP/002-00000031", "0?cnum") in new stack
    -- Executing [s@macro-user-callerid:50] Set("PJSIP/002-00000031", "CDR(cnam)=Support-ATC") in new stack
    -- Executing [s@macro-user-callerid:51] Set("PJSIP/002-00000031", "CDR(cnum)=002") in new stack
    -- Executing [s@macro-user-callerid:52] Set("PJSIP/002-00000031", "CHANNEL(language)=ru") in new stack
    -- Executing [71222@from-internal:2] Gosub("PJSIP/002-00000031", "sub-record-check,s,1(out,71222,dontcare)") in new stack
    -- Executing [s@sub-record-check:1] GotoIf("PJSIP/002-00000031", "0?initialized") in new stack
    -- Executing [s@sub-record-check:2] Set("PJSIP/002-00000031", "__REC_STATUS=INITIALIZED") in new stack
    -- Executing [s@sub-record-check:3] Set("PJSIP/002-00000031", "NOW=1730780250") in new stack
    -- Executing [s@sub-record-check:4] Set("PJSIP/002-00000031", "__DAY=05") in new stack
    -- Executing [s@sub-record-check:5] Set("PJSIP/002-00000031", "__MONTH=11") in new stack
    -- Executing [s@sub-record-check:6] Set("PJSIP/002-00000031", "__YEAR=2024") in new stack
    -- Executing [s@sub-record-check:7] Set("PJSIP/002-00000031", "__TIMESTR=20241105-091730") in new stack
    -- Executing [s@sub-record-check:8] Set("PJSIP/002-00000031", "__FROMEXTEN=002") in new stack
    -- Executing [s@sub-record-check:9] Set("PJSIP/002-00000031", "__MON_FMT=wav") in new stack
    -- Executing [s@sub-record-check:10] NoOp("PJSIP/002-00000031", "Recordings initialized") in new stack
    -- Executing [s@sub-record-check:11] ExecIf("PJSIP/002-00000031", "0?Set(ARG3=dontcare)") in new stack
    -- Executing [s@sub-record-check:12] Set("PJSIP/002-00000031", "REC_POLICY_MODE_SAVE=") in new stack
    -- Executing [s@sub-record-check:13] ExecIf("PJSIP/002-00000031", "0?Set(REC_STATUS=NO)") in new stack
    -- Executing [s@sub-record-check:14] GotoIf("PJSIP/002-00000031", "3?checkaction") in new stack
    -- Goto (sub-record-check,s,17)
    -- Executing [s@sub-record-check:17] GotoIf("PJSIP/002-00000031", "1?sub-record-check,out,1") in new stack
    -- Goto (sub-record-check,out,1)
    -- Executing [out@sub-record-check:1] NoOp("PJSIP/002-00000031", "Outbound Recording Check from 002 to 71222") in new stack
    -- Executing [out@sub-record-check:2] Set("PJSIP/002-00000031", "RECMODE=force") in new stack
    -- Executing [out@sub-record-check:3] ExecIf("PJSIP/002-00000031", "0?Goto(routewins)") in new stack
    -- Executing [out@sub-record-check:4] ExecIf("PJSIP/002-00000031", "0?Goto(routewins)") in new stack
    -- Executing [out@sub-record-check:5] Gosub("PJSIP/002-00000031", "recordcheck,1(force,out,71222)") in new stack
    -- Executing [recordcheck@sub-record-check:1] NoOp("PJSIP/002-00000031", "Starting recording check against force") in new stack
    -- Executing [recordcheck@sub-record-check:2] Goto("PJSIP/002-00000031", "force") in new stack
    -- Goto (sub-record-check,recordcheck,5)
    -- Executing [recordcheck@sub-record-check:5] Set("PJSIP/002-00000031", "__REC_POLICY_MODE=FORCE") in new stack
    -- Executing [recordcheck@sub-record-check:6] GotoIf("PJSIP/002-00000031", "1?startrec") in new stack
    -- Goto (sub-record-check,recordcheck,16)
    -- Executing [recordcheck@sub-record-check:16] NoOp("PJSIP/002-00000031", "Starting recording: out, 71222") in new stack
    -- Executing [recordcheck@sub-record-check:17] ExecIf("PJSIP/002-00000031", "0?Set(RECFROMEXTEN=)") in new stack
    -- Executing [recordcheck@sub-record-check:18] ExecIf("PJSIP/002-00000031", "1?Set(RECFROMEXTEN=002)") in new stack
    -- Executing [recordcheck@sub-record-check:19] Set("PJSIP/002-00000031", "__CALLFILENAME=out-71222-002-20241105-091730-1730780250.107") in new stack
    -- Executing [recordcheck@sub-record-check:20] MixMonitor("PJSIP/002-00000031", "2024/11/05/out-71222-002-20241105-091730-1730780250.107.wav,abi(),") in new stack
    -- Executing [recordcheck@sub-record-check:21] Set("PJSIP/002-00000031", "__MIXMON_ID=") in new stack
    -- Executing [recordcheck@sub-record-check:22] Set("PJSIP/002-00000031", "__RECORD_ID=PJSIP/002-00000031") in new stack
    -- Executing [recordcheck@sub-record-check:23] Set("PJSIP/002-00000031", "__REC_STATUS=RECORDING") in new stack
    -- Executing [recordcheck@sub-record-check:24] Set("PJSIP/002-00000031", "CDR(recordingfile)=out-71222-002-20241105-091730-1730780250.107.wav") in new stack
  == Begin MixMonitor Recording PJSIP/002-00000031
    -- Executing [recordcheck@sub-record-check:25] Return("PJSIP/002-00000031", "") in new stack
    -- Executing [out@sub-record-check:6] Return("PJSIP/002-00000031", "") in new stack
    -- Executing [71222@from-internal:3] Set("PJSIP/002-00000031", "ROUTE_CIDSAVE="Support-ATC" <002>") in new stack
    -- Executing [71222@from-internal:4] Set("PJSIP/002-00000031", "_ROUTEID=3") in new stack
    -- Executing [71222@from-internal:5] Set("PJSIP/002-00000031", "_ROUTENAME=shar_7XXX") in new stack
    -- Executing [71222@from-internal:6] Set("PJSIP/002-00000031", "MOHCLASS=default") in new stack
    -- Executing [71222@from-internal:7] Set("PJSIP/002-00000031", "_CALLERIDNAMEINTERNAL=Support-ATC") in new stack
    -- Executing [71222@from-internal:8] Set("PJSIP/002-00000031", "_CALLERIDNUMINTERNAL=002") in new stack
    -- Executing [71222@from-internal:9] Set("PJSIP/002-00000031", "_EMAILNOTIFICATION=FALSE") in new stack
    -- Executing [71222@from-internal:10] Set("PJSIP/002-00000031", "_NODEST=") in new stack
    -- Executing [71222@from-internal:11] Macro("PJSIP/002-00000031", "dialout-trunk,1,1222,,off") in new stack
    -- Executing [s@macro-dialout-trunk:1] Set("PJSIP/002-00000031", "DIAL_TRUNK=1") in new stack
    -- Executing [s@macro-dialout-trunk:2] ExecIf("PJSIP/002-00000031", "0?Set(DIAL_OPTIONS=Hhtr)") in new stack
    -- Executing [s@macro-dialout-trunk:3] ExecIf("PJSIP/002-00000031", "0?Set(DIAL_OPTIONS=HhTr)") in new stack
    -- Executing [s@macro-dialout-trunk:4] ExecIf("PJSIP/002-00000031", "0?Set(DIAL_OPTIONS=Hhtr)") in new stack
    -- Executing [s@macro-dialout-trunk:5] GosubIf("PJSIP/002-00000031", "0?sub-pincheck,s,1()") in new stack
    -- Executing [s@macro-dialout-trunk:6] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(num)=002)") in new stack
    -- Executing [s@macro-dialout-trunk:7] GotoIf("PJSIP/002-00000031", "0?disabletrunk,1") in new stack
    -- Executing [s@macro-dialout-trunk:8] Set("PJSIP/002-00000031", "DIAL_NUMBER=1222") in new stack
    -- Executing [s@macro-dialout-trunk:9] Set("PJSIP/002-00000031", "DIAL_TRUNK_OPTIONS=HhTtr") in new stack
    -- Executing [s@macro-dialout-trunk:10] Set("PJSIP/002-00000031", "OUTBOUND_GROUP=OUT_1") in new stack
    -- Executing [s@macro-dialout-trunk:11] Set("PJSIP/002-00000031", "DIAL_TRUNK_OPTIONS=T") in new stack
    -- Executing [s@macro-dialout-trunk:12] ExecIf("PJSIP/002-00000031", "0?Set(DIAL_TRUNK_OPTIONS=)") in new stack
    -- Executing [s@macro-dialout-trunk:13] GotoIf("PJSIP/002-00000031", "1?nomax") in new stack
    -- Goto (macro-dialout-trunk,s,15)
    -- Executing [s@macro-dialout-trunk:15] GotoIf("PJSIP/002-00000031", "0?skipoutcid") in new stack
    -- Executing [s@macro-dialout-trunk:16] Macro("PJSIP/002-00000031", "outbound-callerid,1") in new stack
    -- Executing [s@macro-outbound-callerid:1] NoOp("PJSIP/002-00000031", "002") in new stack
    -- Executing [s@macro-outbound-callerid:2] NoOp("PJSIP/002-00000031", "") in new stack
    -- Executing [s@macro-outbound-callerid:3] NoOp("PJSIP/002-00000031", "off") in new stack
    -- Executing [s@macro-outbound-callerid:4] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(name-pres)=)") in new stack
    -- Executing [s@macro-outbound-callerid:5] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(num-pres)=)") in new stack
    -- Executing [s@macro-outbound-callerid:6] Set("PJSIP/002-00000031", "HOTDESCKCHAN=002-00000031") in new stack
    -- Executing [s@macro-outbound-callerid:7] Set("PJSIP/002-00000031", "HOTDESKEXTEN=002") in new stack
    -- Executing [s@macro-outbound-callerid:8] Set("PJSIP/002-00000031", "HOTDESKCALL=0") in new stack
    -- Executing [s@macro-outbound-callerid:9] ExecIf("PJSIP/002-00000031", "0?Set(HOTDESKCALL=1)") in new stack
    -- Executing [s@macro-outbound-callerid:10] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(name)=)") in new stack
    -- Executing [s@macro-outbound-callerid:11] Set("PJSIP/002-00000031", "ALLOWTHISROUTE=NO") in new stack
    -- Executing [s@macro-outbound-callerid:12] ExecIf("PJSIP/002-00000031", "0?Set(ALLOWTHISROUTE=YES)") in new stack
    -- Executing [s@macro-outbound-callerid:13] ExecIf("PJSIP/002-00000031", "0?Hangup()") in new stack
    -- Executing [s@macro-outbound-callerid:14] ExecIf("PJSIP/002-00000031", "0?Set(REALCALLERIDNUM=002)") in new stack
    -- Executing [s@macro-outbound-callerid:15] ExecIf("PJSIP/002-00000031", "0?Set(AMPUSER=002)") in new stack
    -- Executing [s@macro-outbound-callerid:16] GotoIf("PJSIP/002-00000031", "1?normcid") in new stack
    -- Goto (macro-outbound-callerid,s,20)
    -- Executing [s@macro-outbound-callerid:20] Set("PJSIP/002-00000031", "USEROUTCID=") in new stack
    -- Executing [s@macro-outbound-callerid:21] Set("PJSIP/002-00000031", "EMERGENCYCID=") in new stack
    -- Executing [s@macro-outbound-callerid:22] ExecIf("PJSIP/002-00000031", "0?Set(EMERGENCYCID=)") in new stack
    -- Executing [s@macro-outbound-callerid:23] Set("PJSIP/002-00000031", "TRUNKOUTCID=") in new stack
    -- Executing [s@macro-outbound-callerid:24] GotoIf("PJSIP/002-00000031", "1?trunkcid") in new stack
    -- Goto (macro-outbound-callerid,s,30)
    -- Executing [s@macro-outbound-callerid:30] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(all)=)") in new stack
    -- Executing [s@macro-outbound-callerid:31] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(all)=)") in new stack
    -- Executing [s@macro-outbound-callerid:32] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(all)=)") in new stack
    -- Executing [s@macro-outbound-callerid:33] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(all)=002)") in new stack
    -- Executing [s@macro-outbound-callerid:34] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(all)=002)") in new stack
    -- Executing [s@macro-outbound-callerid:35] Set("PJSIP/002-00000031", "TIOHIDE=no") in new stack
    -- Executing [s@macro-outbound-callerid:36] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(name-pres)=prohib_passed_screen)") in new stack
    -- Executing [s@macro-outbound-callerid:37] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(num-pres)=prohib_passed_screen)") in new stack
    -- Executing [s@macro-outbound-callerid:38] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(name-pres)=prohib_passed_screen)") in new stack
    -- Executing [s@macro-outbound-callerid:39] ExecIf("PJSIP/002-00000031", "0?Set(CALLERID(num-pres)=prohib_passed_screen)") in new stack
    -- Executing [s@macro-outbound-callerid:40] Set("PJSIP/002-00000031", "CDR(outbound_cnum)=002") in new stack
    -- Executing [s@macro-outbound-callerid:41] Set("PJSIP/002-00000031", "CDR(outbound_cnam)=Support-ATC") in new stack
    -- Executing [s@macro-dialout-trunk:17] GosubIf("PJSIP/002-00000031", "0?sub-flp-1,s,1()") in new stack
    -- Executing [s@macro-dialout-trunk:18] Set("PJSIP/002-00000031", "OUTNUM=1222") in new stack
    -- Executing [s@macro-dialout-trunk:19] Set("PJSIP/002-00000031", "custom=IAX2/shar") in new stack
    -- Executing [s@macro-dialout-trunk:20] ExecIf("PJSIP/002-00000031", "0?Set(DIAL_TRUNK_OPTIONS=M(setmusic^default)T)") in new stack
    -- Executing [s@macro-dialout-trunk:21] ExecIf("PJSIP/002-00000031", "0?Set(DIAL_TRUNK_OPTIONS=TM(confirm))") in new stack
    -- Executing [s@macro-dialout-trunk:22] Macro("PJSIP/002-00000031", "dialout-trunk-predial-hook,") in new stack
    -- Executing [s@macro-dialout-trunk-predial-hook:1] MacroExit("PJSIP/002-00000031", "") in new stack
    -- Executing [s@macro-dialout-trunk:23] GotoIf("PJSIP/002-00000031", "0?bypass,1") in new stack
    -- Executing [s@macro-dialout-trunk:24] ExecIf("PJSIP/002-00000031", "1?Set(CONNECTEDLINE(num,i)=1222)") in new stack
    -- Executing [s@macro-dialout-trunk:25] ExecIf("PJSIP/002-00000031", "1?Set(CONNECTEDLINE(name,i)=CID:002)") in new stack
    -- Executing [s@macro-dialout-trunk:26] ExecIf("PJSIP/002-00000031", "0?Set(CONNECTEDLINE(name,i)=CID:(Hidden)002)") in new stack
    -- Executing [s@macro-dialout-trunk:27] GotoIf("PJSIP/002-00000031", "0?customtrunk") in new stack
    -- Executing [s@macro-dialout-trunk:28] ExecIf("PJSIP/002-00000031", "0?Set(DIAL_TRUNK_OPTIONS=)") in new stack
    -- Executing [s@macro-dialout-trunk:29] Set("PJSIP/002-00000031", "HASH(__SIPHEADERS,Alert-Info)=unset") in new stack
    -- Executing [s@macro-dialout-trunk:30] Dial("PJSIP/002-00000031", "IAX2/shar/1222,120,Tb(func-apply-sipheaders^s^1,(1))U(sub-send-obroute-email^1222^71222^1^1730780250^Support-ATC^002)") in new stack
    -- IAX2/shar-32314 Internal Gosub(func-apply-sipheaders,s,1(1)) start
    -- Executing [s@func-apply-sipheaders:1] NoOp("IAX2/shar-32314", "Applying SIP Headers to channel IAX2/shar-32314") in new stack
    -- Executing [s@func-apply-sipheaders:2] Set("IAX2/shar-32314", "TECH=IAX2") in new stack
    -- Executing [s@func-apply-sipheaders:3] Set("IAX2/shar-32314", "SIPHEADERKEYS=Alert-Info") in new stack
    -- Executing [s@func-apply-sipheaders:4] While("IAX2/shar-32314", "1") in new stack
    -- Executing [s@func-apply-sipheaders:5] Set("IAX2/shar-32314", "sipheader=unset") in new stack
    -- Executing [s@func-apply-sipheaders:6] ExecIf("IAX2/shar-32314", "0?Set(PJSIP_HEADER(remove,Alert-Info)=)") in new stack
    -- Executing [s@func-apply-sipheaders:7] ExecIf("IAX2/shar-32314", "0?Set(sipheader=<http://127.0.0.1>;info=unset)") in new stack
    -- Executing [s@func-apply-sipheaders:8] ExecIf("IAX2/shar-32314", "0?Set(sipheader=<http://127.0.0.1>unset)") in new stack
    -- Executing [s@func-apply-sipheaders:9] ExecIf("IAX2/shar-32314", "0?Set(PJSIP_HEADER(add,Alert-Info)=unset)") in new stack
    -- Executing [s@func-apply-sipheaders:10] EndWhile("IAX2/shar-32314", "") in new stack
    -- Executing [s@func-apply-sipheaders:4] While("IAX2/shar-32314", "0") in new stack
    -- Executing [s@func-apply-sipheaders:11] Return("IAX2/shar-32314", "") in new stack
  == Spawn extension (, 71222, 1) exited non-zero on 'IAX2/shar-32314'
    -- IAX2/shar-32314 Internal Gosub(func-apply-sipheaders,s,1(1)) complete GOSUB_RETVAL=
    -- Called IAX2/shar/1222
    -- Call accepted by 85.192.165.51:4569 (format alaw)
    -- Format for call is (alaw)
    -- IAX2/shar-32314 is ringing
    -- IAX2/shar-32314 is ringing
    -- IAX2/shar-32314 stopped sounds
    -- IAX2/shar-32314 is ringing
    -- IAX2/shar-32314 is ringing
    -- Hungup 'IAX2/shar-32314'
  == Spawn extension (macro-dialout-trunk, s, 30) exited non-zero on 'PJSIP/002-00000031' in macro 'dialout-trunk'
  == Spawn extension (from-internal, 71222, 11) exited non-zero on 'PJSIP/002-00000031'
    -- Executing [h@from-internal:1] Macro("PJSIP/002-00000031", "hangupcall") in new stack
    -- Executing [s@macro-hangupcall:1] GotoIf("PJSIP/002-00000031", "1?theend") in new stack
    -- Goto (macro-hangupcall,s,3)
    -- Executing [s@macro-hangupcall:3] Gosub("PJSIP/002-00000031", "sub-missedcallnotify,s,1()") in new stack
    -- Executing [s@sub-missedcallnotify:1] NoOp("PJSIP/002-00000031", "CALLERID(number): 002") in new stack
    -- Executing [s@sub-missedcallnotify:2] NoOp("PJSIP/002-00000031", "CALLERID(name): Support-ATC") in new stack
    -- Executing [s@sub-missedcallnotify:3] NoOp("PJSIP/002-00000031", "DialStatus: CANCEL") in new stack
    -- Executing [s@sub-missedcallnotify:4] NoOp("PJSIP/002-00000031", "VMSTATUS: ") in new stack
    -- Executing [s@sub-missedcallnotify:5] ExecIf("PJSIP/002-00000031", "0?System(curl -s -X POST https://api.telegram.org/bot/sendMessage -d parse_mode=html -d text="Пропущенный вызов: \nНомер 002\nИмя:Support-ATC" -d chat_id=)") in new stack
    -- Executing [s@sub-missedcallnotify:6] ExecIf("PJSIP/002-00000031", "0?System(curl -s -X POST https://api.telegram.org/bot/sendMessage -d parse_mode=html -d text="Пропущенный вызов: \nНомер 002\nИмя:Support-ATC\nОставлено голосовое сообщение." -d chat_id=)") in new stack
  == MixMonitor close filestream (mixed)
  == End MixMonitor Recording PJSIP/002-00000031
atsip-pbx*CLI>
```
От 4 до 9 детальность будет повышаться, а свыше 9 уже изменяться не будет.