---
title: "CoDeSys setup for PRU programming"
permalink: /docs/pru/codesys-setup/
lang: en
excerpt: "CoDeSys setup for PRU programming in Hardella IDE"
modified: 2016-12-19T20:00:00+03:00
---

To make a long story short: you need to upload `.prg` and `.exp` files, and add `pruAccessLib.lib` library.
The library can be found [in pack1.zip archive at OWEN forum](http://www.owen.ru/forum/showthread.php?t=22169&highlight=owenlogicrt)

## PLC preparation

The PLC110 M02 that was bought in January 2016 worked out of the box. Firmware update was not required.

Before you start programming PRU cores, you need to switch them into free programming mode. This setup should be made just once and it will persist across reboots.

In order to prepare the PLC you need:
 1. Upload `PRU0.prg` file to the PLC (and `PRU1.prg`, if that is required) via CoDeSys menu `Online` > `Write file to PLC`
 
 1. Reboot the PLC

## Import .exp code to CoDeSys project

Hardella IDE creates standard `.exp` file, that can be imported via CoDeSys `Project` > `Import...` menu

<img width="316" alt="Import program to CoDeSys" src="{{ "/assets/images/docs/pru/codesys-import.png" | absolute_url }}">

Here's what will get imported:

<img width="295" alt="Imported CoDeSys programs" src="{{ "/assets/images/docs/pru/codesys-imported.png" | absolute_url }}">

  - Programs to setup PRU cores (`PROGRAM BlinkningLeds_Pru0Init` and `PROGRAM BlinkningLeds_Pru1Init`)
  - Programs for PRU data exchange (`PROGRAM  BlinkningLeds_Pru0MemoryTransfer` and `PROGRAM  BlinkningLeds_Pru1MemoryTransfer`)
  - Enumerations (`ENUM`), if used in the program

Remember to add `pruAccessLib.lib` library, otherwise the following CoDeSys would appear on the project build

    Error 4001: BlinkningLeds_Pru0MemoryTransfer (2): Identifier 'PRU_FB_GET_PARAMETER' not defined

## PRU core setup

By default PRU cores do nothing. They process no input and write no output. In order to upload some program, you need to call relevant `Init` programs.

In order to do that, add the following code to `PLC_PRG` program:

    BlinkningLeds_Pru0Init(); (* PRU0 code upload*)
    BlinkningLeds_Pru1Init(); (* PRU1 code upload *)
    (* PRU cores are started, so you can exchange data now *)
    BlinkningLeds_Pru0MemoryTransfer(BLINK_3_4_enable:=TRUE);
    BlinkningLeds_Pru1MemoryTransfer(BLINK_1_2_enable:=TRUE);

<img width="407" alt="PLC_PRG code" src="{{ "/assets/images/docs/pru/codesys-plcprg.png" | absolute_url }}">

You can upload new PRU programs on the fly, however you should do that at safe times only. Of course it is not a good idea to upload a new program while your plant is drilling.

You can call `BlinkningLeds_Pru0Init();` at any time even in the main PLC loop. The program has protection from multiple executions, so it will operate just once.
