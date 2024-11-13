+++
title = 'Задание определенного направления (выход через конкретный транк)'
date = 2024-11-02T14:38:41+05:00
author = 'Dmitriy Q'
draft = false
+++
Для начала создаем нужный нам маршрут: **Подключения → Исходящая маршрутизация**.

![6d792aa70fd0b7d7f644c884205cf8cc.png](/6d792aa70fd0b7d7f644c884205cf8cc.png)

Задаем произвольно описание , необходимый нам CallerID и транк, через который будет передаваться вызов с этим CallerID (если транк с регистрацией и допустим только один CallerID, то можно указать «**Форсировать использование транкового CID (Force Trunk CID) в настройках транка**»)

![bf2ae29dc9e282da384bee12fe2a4e07.png](/bf2ae29dc9e282da384bee12fe2a4e07.png)

Задаем необходимые (допустимые) нам шаблоны набора номеров:

![0494b10004354ef8df2036731a4db6b8.png](/0494b10004354ef8df2036731a4db6b8.png)

Можете включить запись вызова:

![68dfc868664b4fe78ecf2c70c1df736e.png](/68dfc868664b4fe78ecf2c70c1df736e.png)

После этого обязательно **Сохраняем** и **Применяем изменения.**

Как видим — появился новый маршрут в исходящих маршрутах.

![6f797669fdc2cfc0d80ba5a11a559676.png](/6f797669fdc2cfc0d80ba5a11a559676.png)

Ещё будет неплохо поместить его в самый низ по приоритету маршрутов — чтобы не мешал остальным номерам.

Теперь отправляемся в **Подключения** → **Custom Contexts** и создаем новый контекст:

![089b090d39440d041f2ed17d53a2b460.png](/089b090d39440d041f2ed17d53a2b460.png)

Жмем **Submit** и после этого он раскрывается:

![71e6fe78de27f0cb8931e6338d9d1cad.png](/71e6fe78de27f0cb8931e6338d9d1cad.png)

### В меню **See All To**: выбираем **Deny**, т.е. запретим всё, а после разрешим необходимое.

Теперь рассмотрим и повключаем необходимое:

### **Default Internal Context**

- *Call Parking* — отвечает за парковку вызовов

- *Custom Internal Dialplan* — отвечает за использование контекста from-internal-custom.


### **Internal Dialplan**

Тут как правило по использованию приложений:

- *app-callwaiting-cwoff* и *app-callwaiting-cwon* — позволяет использовать включение и отключение Call Waiting;

- *ext-meetme* — приложение конференции;

- *app-fmf-toggle, ext-findmefollow* и *fmgrps* — относятся к модулям Follow-Me и Find-Me;

- *app-calltrace* — трассировка вызова (из Сервисных кодов);

- *app-blacklist* — использование приложения Черный список;

- *app-echo-test* — Эхо-тест;

- *app-speakextennum* — Произношение вашего внутреннего номера;

- *app-speakingclock* — Говорящие часы;

- *app-recordings* — не уверен, но скорее всего Запись входящего звонка по необходимости;

- *ext-queues* — Приложение очередей (позволяет набирать очередь по внутреннему номеру);

- *ext-group* — Набор групп вызова напрямую;

- *app-dialvm* и *app-vmmain* — позволяют проверять и оставлять голосовую почту соответственно;

- *ext-local-confirm* — использование опции Подтверждать вызовы;

- *findmefollow-ringallv2* — до конца неясно, но похоже на использование стратегии обзвона ringallv2 для Следуйте-Сюда/Найди-меня;

- *app-pickup* — разрешает использование Назначенного перехвата звонка;

- *app-chanspy* — использование Шпионского канала;

- *ext-test* — судя по диалплану — это Симуляция входящего звонка;

- *ext-local* позволяет вызывать внутренние номера.


Так что если хотите запретить всё, кроме вызова внутренних номеров — разрешайте только *ext-local*.

### **Outbound Routes**

Вот здесь и выбираем созданный нами маршрут *from-buhgalter*. Можно выбрать и другие разрешенные маршруты и расположить их в порядке возрастания совпадения (по приоритетам: чем ниже значение — тем выше в списке и тем выше приоритет).

При этом напротив **ALL OUTBOUND ROUTES** должно стоять **Deny**.

![5c69ad27d3674c662cc086e02179d934.png](/5c69ad27d3674c662cc086e02179d934.png)

С охраняем созданный пользовательский контекст.

И теперь нам осталось применить его к внутреннему номеру.

Нужная нам настройка на вкладке **Расширенный**.

![8fcf8dc6f9bea003d5a96231cbab2cc2.png](/8fcf8dc6f9bea003d5a96231cbab2cc2.png)

Вот здесь напротив **Custom Context** и выбираем наш созданный пользовательский контекст **from-buhgalter**.

Сохраняем и применяем настройки.

Теперь пользователю будет разрешено звонить туда, куда мы определили для него.