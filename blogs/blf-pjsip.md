# Удаленные BLF с использованием PJSIP
Перевод статьи [Remote BLF's using PJSIP](https://wiki.freepbx.org/display/FDT/Remote+BLF%27s+using+PJSIP) с вики FreePBX.
На этой странице будет описано, как настроить BLF удаленного телефона с помощью PJSIP между двумя АТС, которые будут отслеживать состояния устройства удаленных телефонов.
### Так же к ознакомлению
Publishing Extension States
Exchanging Device State PJSIP
## Исходные данные
![](images/blf-pjsip-01.png)
Настройка BLF на телефонах
BLF Alerts
Создайте транк с `context=from-internal` на каждой АТС для маршрутизации вызовов и отправки публикации для состояний устройств
### FreePBX
![](images/blf-pjsip-02.png)
### PBXact
![](images/blf-pjsip-03.png)
**Настройка FreePBX Config**
    1. Отредактируйте pjsip.endpoint_custom.conf путем добавления следующих данных
```
[PBXact]
type=endpoint

[PBXact-devicestate]
type=outbound-publish
server_uri=sip:FreePBX@10.39.96.5
event=asterisk-devicestate
@body=application/dialog-info+xml
@context=^users

[PBXact]
type=asterisk-publication
devicestate_publish=PBXact-devicestate
mailboxstate_publish=PBXact-mwi
device_state=yes
mailbox_state=yes
@body=application/dialog-info+xml
@context=^users

[PBXact]
type=inbound-publication
event_asterisk-devicestate=PBXact
event_asterisk-mwi=PBXact
```
**    2. Отредактируйте extensions_custom.conf**
```
[from-internal-custom]
exten => _[123]xxx,hint,PJSIP/${EXTEN}&&PJSIP/90${EXTEN}&PJSIP/99${EXTEN}&Custom:DND${EXTEN},CustomPresence:${EXTEN}
; *****обратите внимание, что диалплан ищет расширения в диапазоне 1xxx, 2xxx и 3ххх
```
## Настройка PBXAct
**    1. Отредактируйте the pjsip.endpoint_custom.conf добавив следующие данные**
```
[FreePBX]
type=endpoint

[FreePBX-devicestate]
type=outbound-publish
server_uri=sip:PBXact@10.39.96.4:5061
event=asterisk-devicestate
@body=application/dialog-info+xml
@context=^users

[FreePBX]
type=asterisk-publication
devicestate_publish=FreePBX-devicestate
mailboxstate_publish=FreePBX-mwi
device_state=yes
mailbox_state=yes
@body=application/dialog-info+xml
@context=^users

[FreePBX]
type=inbound-publication
event_asterisk-devicestate=FreePBX
event_asterisk-mwi=FreePBX
```
**    2. Отредактируйте extensions_custom.conf**
```
[from-internal-custom]
exten => _[123]xxx,hint,PJSIP/${EXTEN}&&PJSIP/90${EXTEN}&PJSIP/99${EXTEN}&Custom:DND${EXTEN},CustomPresence:${EXTEN}
; *****обратите внимание, что диалплан ищет расширения в диапазоне 1xxx, 2xxx и 3ххх
```
Чтобы показать хинты от каждой АТС соответственно вы можете ввести следующее
    1. В CLI на FreePBX: `asterisk-rx 'core show hints' | grep-i (ext)`

![](images/blf-pjsip-04.png)

Как вы можете видеть, экземпляр FreePBX следит за расширениями 7200 и 4100 на PBXact
Когда пользователь находится в состоянии вызова - BLF изменится так же, как и хинт
![](images/blf-pjsip-05.png)
    2. DND так же можно контролировать
![](images/blf-pjsip-06.png)
