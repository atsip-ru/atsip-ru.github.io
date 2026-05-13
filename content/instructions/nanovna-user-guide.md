---
title: 'Руководство пользователя NanoVNA-H'
date: '2026-05-13T11:46:23+05:00'
draft: true
author: 'Dmitriy Q'
tags: [nanovna, meshtastic]
---

![NanoVNA-H](/_resources/nanovna-1.png)

## Обзор

Мы разработали NanoVNA на основе разработки edy555 (https://twitter.com/edy555), но изменили некоторые схемы, добавили схемы управления аккумулятором и переработали
печатную плату. Программное обеспечение для управления с ПК может экспортировать файлы Touchstone (snp), которые можно использовать в различном программном
обеспечении для проектирования и моделирования радио. Улучшенный алгоритм частоты может использовать нечетное гармоническое расширение si5351 для поддержки
измерения частот до 900 МГц. Металлический экран разработан для уменьшения внешних помех и повышения точности измерений. Диапазон частот 50к-300МГц прямого
вывода si5351 обеспечивает динамику лучше 70 дБ. Расширенный диапазон 300М-600МГц обеспечивает динамику лучше 50 дБ, а диапазон 600М-900МГц — лучше 40 дБ динамики.
Это очень маленький портативный векторный анализатор цепей (VNA). Это автономное переносное устройство с аккумулятором и ЖК-дисплеем. Цель этого проекта —
предоставить радиолюбителям RF-устройство, а также полезный инструмент.

Векторный анализатор DIY, с ссылкой на соответствующий дизайн. Разработан с использованием простого и практичного программного обеспечения для управления через ПК,
вы можете экспортировать файлы Touchstone (snp) для различного радиопроектирования и программного обеспечения для моделирования через ПК. NanoVNA является проектом
аппаратного обеспечения с открытым исходным кодом, каждый может свободно клонировать и создавать DIY, но мы надеемся, что вы получите больше понимания в
разработке перед тем, как это делать. Некоторые клиенты сообщали, что купили низкокачественный nanoVNA из других источников. Мы нашли некоторые низкокачественные
клоны в Интернете, претендующие на завершенные проекты. Покупка такого оборудования может навредить производительности NanoVNA или привести к ошибке измерений.

Производитель обязуется продолжать предоставлять обновления и услуги технической поддержки для NanoVNA в течение 3 лет после покупки.

**Мы предоставляем 5 прошивок на сетевом хранилище, вы можете выбрать подходящую прошивку в соответствии с руководствами, различия между 5 прошивками следующие:**

nanoVNA_300_ch: 50K-300МГц, 5\*7 точечный шрифт, 4 дорожки
nanoVNA_900_ch: 50K-900МГц, 5\*7 точечный шрифт, 4 дорожки (По умолчанию)
nanoVNA_900_aa: 50K-900МГц, 7\*13 точечный шрифт, 2 дорожки (Анализатор антенн)
Прошивка 800МГц работает лучше при высоких температурах.
nanoVNA_800_ch: 50K-900МГц, 5\*7 точечный шрифт, 4 дорожки (Рекомендуется)
nanoVNA_800_aa: 50K-900МГц, 7\*13 точечный шрифт, 2 дорожки (Анализатор антенн)

## Передняя панель

![Передняя панель](/_resources/nanovna-2.png)

Вы можете использовать USB-интерфейс для подключения к компьютеру или подключения стандартного 5В устройства для зарядки.

При зарядке батареи LED-индикатор батареи будет мигать, постоянное свечение будет указывать на полную зарядку. При разряде батареи постоянное свечение
LED-индикатора указывает на нормальный разряд, а мигание будет указывать на низкий уровень заряда, пожалуйста, подключите зарядное устройство вовремя для зарядки.
Переключение выключателя питания приведет к отключению аппарата, батарея указывает на то, что светодиод необходимо выключить через 40 секунд после выключения.

При правильном сканировании системы индикатор системы загорается и затемняется. Вы можете переместить маркер или выполнить какие-либо действия в меню
с помощью многофункционального переключателя, а также напрямую управлять с помощью сенсорного экрана.

## Основной интерфейс

![Основной интерфейс](/_resources/nanovna-3.png)

### Базовые операции:

1. Установка диапазона частот (STIMULUS > START/STOP или CENTER/SPAN)
2. Калибровка (CAL)
3. Выбор формата отображения и канала (DISPLAY)
4. Сохранение (SAVE)

Вы можете изменить формат отображения и выбор канала в любое время. В обычном режиме нажмите на правую область экрана или на многофункциональный переключатель
для вызова меню. Нажмите на экран или поверните многофункциональный переключатель для выбора пункта меню.

Начальное состояние NanoVNA (данные в 0 не сохранены)
Диапазон сканирования: 50KHz ~ 900MHz
Track 1: LOGMAG CH0 (отражение)
Track 2: SMITH CH0 (отражение)
Track 3: LOGMAG CH1 (сквозной)
Track 4: CH1 stage (сквозной)
Mark 1: Активирован
Некалиброванный.

**Мы проведем тестирование перед отправкой и подключимся напрямую к порту SMA для калибровки, данные калибровки будут сохранены в состоянии 0, и при загрузке
будут загружены данные из состояния 0 напрямую.**

## Калибровка и нормализация

VNA Master - это портативное устройство, работающее в суровых полевых испытательных условиях. Для обеспечения точности измерений перед выполнением измерений в полевых условиях необходимо выполнить радиочастотную калибровку (OSLT). Для калибровки с помощью указанной механической калибровки требуются три нагрузки: разомкнутую (OPEN), короткозамкнутую (SHORT) и согласованную (LOAD). Данные калибровки сохраняются как данные пользовательской калибровки. Они могут быть сохранены в состоянии 0 и будут автоматически загружены при следующей загрузке, а также могут быть сохранены в состояниях 1-4 и могут быть загружены через меню RECALL.

![Калибровка и нормализация](/_resources/nanovna-4.png)

Нажмите меню CALIBRATE→CALIBRATE, чтобы открыть интерфейс калибровки, затем подключите разомкнутую, короткозамкнутую и согласованную нагрузку каждую последовательно, дождитесь стабилизации экрана и нажмите на пункт меню, соответствующий калибровке CH 0. К аппарату прилагается калибровочный элемент, для калибровки общедоступных деталей головки внутри медной иглы имеется короткое замыкание, внутренняя часть с иглой из белого пластика и корпусом из нержавеющей стали подключена к нагрузке (LOAD) 50 Ом, внутренняя часть пуста для открытого состояния. Для калибратора на задней панели используется короткое замыкание припоем, пайка двух резисторов по 100 Ом для нагрузки 50 Ом и разомкнутая цепь для разомкнутой нагрузки.

К аппарату прилагается калибровочный элемент, для калибровки общедоступных деталей головки внутри медной иглы произошло короткое замыкание, внутренняя часть с иглой из белого пластика и корпусом из нержавеющей стали подключена к нагрузке (НАГРУЗКА) 50 Ом, внутренняя часть пуста для открытого состояния. Для головного калибратора на задней панели используется короткое замыкание припоем для короткого замыкания, сварка двух резисторов по 100 Ом для нагрузки 50 Ом, разомкнутая цепь для разомкнутости.

Калибровка изоляции CH 1 требует подключения двух нагрузок к CH0 и CH1 соответственно, чтобы получить лучшую изоляцию, обычно используется только один комплект калибровочных нагрузок для порта 0, а нагрузку калибратора подключают к CH1, CH0 остается открытым, затем нажимают меню ISOLN для калибровки.

Подключите CH0 и CH1 анализатора с помощью опционального прямого адаптера (набор SMA-SMA или кабель), затем нажмите меню THRU для калибровки. Операция нормализации перемещает измерительную эталонную плоскость на оба конца прямого адаптера. Эта функция доступна только при измерении параметра S21.

После завершения калибровки нажмите кнопку Done, выйдите на экран Save, чтобы выбрать нужный статус (слот) для сохранения. После завершения калибровки три калибратора снова могут быть подключены к порту 0, и правильная калибровка должна основываться на карте SMITH, которая должна быть следующей: при подключении к OPEN кривые должны быть сосредоточены на самом правом краю карты SMITH, а при подключении SHORT кривые должны быть сосредоточены на самом левом краю карты SMITH, при подключении LOAD все кривые должны быть сосредоточены в центре карты SMITH. Используя RF-кабели для соединения портов CH0 и CH1, ошибки кривой S21 не должны превышать 0,1 дБ. Если будут обнаружены аномалии в данных калибровки, их следует перекалибровать.

**Примечание: Если уже есть сохраненные данные калибровки, сначала нажмите «Reset» для очистки данных калибровки, а затем выполните калибровку!**

Если калибровка была применена, отображается статус CAL.

Он скрыт в состоянии, когда не был применен. C* — это состояние, в котором применяется несохраненное калибровочное значение (оно исчезает при отключении питания). C0 ~ C4 0 до 4 указывает на то, что сохранённое калибровочное значение было применено к одному из мест сохранения.
SAVE Unsaved
Состояние изменяется на это при сохранении операцией. Каждый из символов, показанных ниже C, указывает на то, что применяются следующие ошибки:
D: Направленность, R: Отслеживание отражения, S: Совпадение источника, T: Отслеживание передачи, X: Изоляция

### Select the display trace and display format

The DISPLAY→TRACE item of the menu can choose to turn the corresponding display curve on or off, showing that the curve color is consistent with the color of the interface curve, and the display curve of the final operation is the active tracking curve, when the menu FORMAT, the SCALE,CHANNEL operation is valid for the display curve.

The display type can be modified by the DISPLAY→FORMAT of the menu, DISPLAY→SCALE can adjust the scale, DISPLAY→CHANNEL can select the measured port.
#### Setting the frequency range

The frequency range of a channel can be expressed by three groups of parameters: Start Frequency, Center Frequency and Stop Frequency. If any of the parameters change, the others will be adjusted automatically in order to ensure the coupling relationship among them
fcenter =(fstart +fstop )/2
fspan= fstop- fstart
Where fspan is the span.
Set the center frequency point of the current screen through the STIMULUS→CENTER of the menu, and display the values of the center frequency and sweep span, respectively, to the left and right of the bottom of the grid. **In the lower-right corner of the pop-up Settings value screen, click to eject the soft keyboard and enter the frequency value via the soft keyboard.**

Please pay attention to the following points:

The start and stop frequencies will vary with changes to the center frequency when the span is constant.

In Zero Span, the start frequency, stop frequency and center frequency are always set to the same value. Now, you can use port 0 as a signal source for a fixed output amplitude, but it is important to note that this machine uses the clock signal generator S5351 as the signal source, the output signal is square wave, contains a larger odd harmonic.

Set the frequency range through the STIMULUS→SPAN of the menu, display the center frequency and sweep span values on the left and right sides of the grid, and click on the lower right corner of the pop-up settings value screen to eject the soft keyboard and enter the frequency values through the soft keyboard.

Please pay attention to the following points:
The start and stop frequency vary with the span when the center frequency is constant.

When the span is set to the maximum, the analyzer enters full span mode.
In Zero Span, the start frequency, stop frequency and center frequency are always set to the same value.

Set the start frequency through the STIMULUS→START of the menu and display the start frequency and stop frequency values on the left and right sides of the grid, respectively. Click on the bottom right corner of the pop-up settings screen to eject the soft keyboard and enter the frequency value through the soft keyboard.

Please pay attention to the following points:
The span and center frequency vary with the start frequency when the Span does not reach the minimum (The parameters vary with the span, please refer to “Span”);
In Zero Span, the start frequency, stop frequency and center frequency are always the same value.

Set the stop frequency through the STIMULUS→STOP of the menu, and display the start frequency and stop frequency values on the left and right sides of the grid, respectively, in the lower right corner of the pop-up Settings screen, click to eject the soft keyboard and enter the frequency value through the soft keyboard.
Please pay attention to the following points:
The span and center frequency vary with the stop frequency. The change of the span will affect other system parameters. For more details, please refer to “Span”.

In Zero Span, the start frequency, stop frequency and center frequency are always
the same value.

### Menu items

DISPLAY
* TRACE
	* 0
	* 1
	* 2
	* 3
* FORMAT
	* LOGMAG
	* PHASE
	* DELAY (Not implemented, computer software provides Group delay functionality)
	* SMITH
	* SWR
	* MORE
		* POLAR
		* LINEAR
* SCALE
	* SCALE/DIV
	* REFERENCE POSITION
	* ELECTRICAL DELAY
* CHANNEL
	* CH0 REFLECT
	* CH1 THROUGH
MARKER
* SELECT
	* 1
	* 2
	* 3
	* 4
* MARKER→START
* MARKER→STOP
* MARKER→CENTER
* MARKER→SPAN (Not implemented,)
STIMULUS
* STAR
* STOP
* CENTER
* SPAN
* CW FREQ
CAL
* CALIBRATE
	* OPEN
	* SHORT
	* LOAD
	* ISOLN
	* THRU
	* DONE
		* SAVE
* RESET
* CORRECTION
RECALL/SAVE
* 0 (Default)
* 1
* 2
* 3
* 4
CLOSE

#### Basic performance

* PCB: 54mm x 85.5mm x 11mm (without connectors, switches)
* Measurement frequency: 50KHz -900MHz
* RF output: -13dbm (maximum -9dbm)
* Measurement range: 70dB (50kHz-300MHz), 50dB (300M-600MHz), 40dB (600M-900MHz));
* Port SWR: < 1.1
* Display: 2.8 inch TFT (320 x240)
* USB interface: USB type-C communication mode: CDC (serial)
* Power: USB 5V 120mA, built-in 400mAh battery, maximum charging current 0.8A
* Number of scanning points: 101 (fixed)
* Display Tracking: 4, Marking: 4, Setting Save: 5
* Frequency Tolerance:<2.5ppm
* Frequency Stability:<0.5ppm
### Packing list
NanoVNA host (with 400mAh battery) x 1
USB Type-C data cable x1
30cm SMA male to male RG174 RF cable x2
SMA male calibration kit (OPEN / SHORT /LOAD) x1
SMA female to female connector x1

![Basic performance](/_resources/nanovna-5.png)

### Block diagram

![Block diagram](/_resources/nanovna-6.png)

### NanoVNA Sharp

Это программное обеспечение для ПК предназначено для управления файлами NanoVNA и экспорта файлов Touchstone (snp).

![NanoVNA Sharp](/_resources/nanovna-7.png)