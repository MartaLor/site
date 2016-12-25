---
title: "Настройка CoDeSys для PRU программирования"
permalink: /docs/pru/codesys-setup/
lang: ru
excerpt: "Настройка CoDeSys для PRU программирования в Hardella IDE. Hardella это среда для программирования ПЛК на языках группы 61131 (ST и т.п.)"
modified: 2016-12-25T20:49:43+03:00
---

В двух словах: нужно загрузить `.prg` и `.exp` файлы, и добавить библиотеку `pruAccessLib.lib`.
Этот файл можно взять [в архиве pack1.zip на форуме ОВЕН](http://www.owen.ru/forum/showthread.php?t=22169&highlight=owenlogicrt)

## Подготовка контроллера

Контроллер ПЛК110 М02, купленый в январе 2016-го заработал сразу. Прошивку обновлять не требовалось. 

Перед использованием PRU ядер их нужно перевести в режим свободного программирования. Подготовку достаточно выполнить 1 раз, она не потеряется при перезагрузках.

Для этого нужно:
 1. Загрузить файл `PRU0.prg` в контроллер (и `PRU1.prg`, если требуется) через CoDeSys меню `Online` > `Write file to PLC`
 
 1. Перезагрузить ПЛК

## Импорт .exp кода в CoDeSys проект

Hardella IDE генерирует стандартный `.exp` файл, который загружается в CoDeSys через меню `Project` > `Import...`

<img width="316" alt="Импорт программы в CoDeSys" src="{{ "/assets/images/docs/pru/codesys-import.png" | absolute_url }}">

При этом в проект загружаются следующие сущности:

<img width="295" alt="Импортированные программы в CoDeSys" src="{{ "/assets/images/docs/pru/codesys-imported.png" | absolute_url }}">

  - Программы настройки PRU ядер (`PROGRAM BlinkningLeds_Pru0Init` и `PROGRAM BlinkningLeds_Pru1Init`)
  - Программы обмена данными с PRU ядрами (`PROGRAM  BlinkningLeds_Pru0MemoryTransfer` и `PROGRAM  BlinkningLeds_Pru1MemoryTransfer`)
  - Перечисления (`ENUM`), если они использовались

Нужно добавить библиотеку `pruAccessLib.lib`, иначе в CoDeSys возникнут ошибки похожие на

    Error 4001: BlinkningLeds_Pru0MemoryTransfer (2): Identifier 'PRU_FB_GET_PARAMETER' not defined

## Настройка PRU ядра

По умолчанию, PRU ядра "в режиме свободного программирования" остановлены. Т.е. они вообще ничего не делают. Для того, чтобы загрузить программу, нужно вызвать соответствующую программу.

Для этого в `PLC_PRG` добавим следующий код:

    BlinkningLeds_Pru0Init(); (* загрузка кода в PRU0 *)
    BlinkningLeds_Pru1Init(); (* загрузка кода в PRU1 *)
    (* PRU запущены, можно обмениваться данными *)
    BlinkningLeds_Pru0MemoryTransfer(BLINK_3_4_enable:=TRUE);
    BlinkningLeds_Pru1MemoryTransfer(BLINK_1_2_enable:=TRUE);

<img width="407" alt="Код для PLC_PRG" src="{{ "/assets/images/docs/pru/codesys-plcprg.png" | absolute_url }}">

В целом, можно менять PRU программы на ходу, но делать это нужно только тогда, когда оборудование находится в безопасном состоянии. Явно не стоит заменять программу сверления во время самого сверления.

В целом, можно вызывать `BlinkningLeds_Pru0Init();` хоть в каждом основном цикле ПЛК. В программе `Init` есть защита от повторных запусков.
