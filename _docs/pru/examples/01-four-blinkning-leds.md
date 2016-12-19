---
title: "Мигаем быстрыми выходами"
permalink: /docs/pru/examples/four-blinkning-leds/
lang: ru
excerpt: "Пример работы с быстрыми выходами PRU на Hardella IDE"
modified: 2016-12-14T22:39:43+03:00
---


Сделаем так, чтобы выходы 1 и 2 мигали с интервалом 250 мс, а выходы 3 и 4 с интервалом 500 мс.

Если у вас открыта среда Hardella IDE, то можете либо
{% include ide_link addr="r:31c55dd8-3058-4da1-84b2-4ce3e79191b5/2559549685963163651" text="открыть код примера в среде" %}, либо создать свой проект на основе примера (`File` > `New` > `Project` > `Blinking leds`).

В проекте можно увидеть следующее:
  1. PRU Configuration -- там описывается то, какие программы должно выполнять каждое PRU ядро, с каким интервалом цикла, и какая должна быть фильтрация входов. В данном случае конфигурация названа `BlinkningLeds`:

     <img width="841" alt="PRU configuration" src="{{ "/assets/images/docs/pru/default-project-view.png" | absolute_url }}">
     
     Здесь указано, что фильтрации входов нет (см `no debounce`), и указано какие программы выполняются на PRU ядрах. Например, на PRU0 выполняется `BLINK_3_4` со временем цикла 500 мс. Т.е. PRU ядро будет вызывать программу `BLINK_3_4` не чаще, чем раз в 500 мс.
 
  1. Программа для `PRU0`. В нашем случае она названа `BLINK_3_4`:

     <img width="587" alt="BLINK_3_4" src="{{ "/assets/images/docs/pru/examples/blinkingleds/blink_3_4.png" | absolute_url }}">
     
     В целом, должно быть понятно, но рассмотрим происходящее детальнее.
     Программа записывает значение `state` в `fast out 3` (`out3`) и значение `NOT state` в `fast out 4` (см `out4`).
     Само значение `state` изменяется только тогда, когда взведён флаг `enable`.
     
     Как же `enable` может изменить своё значение? При объявлении переменной `enable`, для неё настроен обмен с "основным циклом CoDeSys" (см раздел [обмен данных](/docs/pru/data-exchange/)). Т.е. флаг `enable` будет управляться из основного цикла, а вся программа будет выполняться на PRU.

  1. Программа для `PRU1`. В нашем случае она названа `BLINK_1_2`:

     <img width="587" alt="BLINK_1_2" src="{{ "/assets/images/docs/pru/examples/blinkingleds/blink_1_2.png" | absolute_url }}">

     Здесь всё аналогично, но используются `out1` и `out2`.

При компиляции в созданных `MemoryTransfer` программах будут переменные для `enable`.
Так, например, будет выглядеть программа для PRU0:

    PROGRAM BlinkningLeds_Pru0MemoryTransfer
    VAR_INPUT
      BLINK_3_4_enable : BOOL;
    END_VAR
    ...

Таким образом, чтобы передать `enable:=TRUE` в PRU0 программу, нужно в основном цикле ПЛК выполнить такой код:

    BlinkningLeds_Pru1MemoryTransfer(BLINK_3_4_enable := TRUE)
