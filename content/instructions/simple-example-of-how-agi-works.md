+++
title = 'Простой пример работы AGI'
date = 2024-11-25T17:19:06+05:00
draft = false
+++

Рассмотрим работу AGI на примере скрипта:
```php
#!/usr/bin/php7.3 -q

<?php

require('phpagi.php');
$AGI = new AGI();
$filepath = "/var/www/html/statistic.local/";  //Путь к файлу
$filedb = $AGI->request['agi_arg_1'];      //Имя файла с базой
$ls = $AGI->request['agi_arg_2'];          //Лицевой счет абонента
$fullfile = $filepath . $filedb;

$arr = file($fullfile);
    foreach($arr as $v){
        if (strpos($v,$ls,0) !== false){
            $string = $v;
            $ls_array = explode('|', $string);
            if ($ls == $ls_array[2]){
                $dolg = $ls_array[6];
                break;
            }
        }
    }

$summa = explode('.',$dolg);
$summa_rub = (int) $summa[0];
$summa_kop = (int) $summa[1];

if ($summa_rub % 10 == 1){
    $sound_r = "rouble";
}
elseif (in_array(($summa_rub % 10), [2, 3, 4])){
    $sound_r = "roublya";
}
elseif (in_array(($summa_rub % 10), [5,6,7,8,9,0])){
    $sound_r = "roubley";
}

if ($summa_kop % 10 == 1){
    $sound_k = "copeck";
}
elseif (in_array(($summa_kop % 10), [2, 3, 4])){
    $sound_k = "copeck-i";
}
elseif (in_array(($summa_kop % 10), [5,6,7,8,9,0])){
    $sound_k = "copecks";
}

$AGI->set_variable('DOLG',$dolg);
$AGI->set_variable('DOLG_R',$summa[0]);
$AGI->set_variable('DOLG_K',$summa[1]);
$AGI->set_variable('sound_r',$sound_r);
$AGI->set_variable('sound_k',$sound_k);
?>
```
Вызывается из диалплана:
```
same => n,agi(getdolg.php,dbsum.txt,${bill})
```
Скрипт получает в виде аргументов имя файла с данными и лицевой счет абонента, парсит его и возвращает сумму долга из одного из столбцов.
`$filedb = $AGI->request['agi_arg_1'];` присваивает имя файла с данными переменной, а `$ls = $AGI->request['agi_arg_2'];` соответственно получаемый лицевой счет.

Далее читается файл, ищется в нем ЛС по 3 (2 по индексу) столбцу и возвращает значение из 7 (6 по индексу) столбца, присваивая его значение переменной `dolg`.
Далее сумма разбивается на рубли и копейки и подгоняются склонения, присваиваются нужные значения, которые после и передаются из AGI в диалплан в виде:

`$AGI->set_variable('переменная_в_диалплане',$значение_переменной_AGI);`

`DOLG` в самом дилалплане, конечно, не нужна, она скорее для проверки

`DOLG_R` - сумма в рублях

`DOLG_K` - сумма в копейках

`sound_r` - файл озвучки для "рублей"

`sound_k` - файл озвучки для "копеек"
