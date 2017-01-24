---
title: "Управление шаговым двигателем"
permalink: /docs/pru/examples/step-motor/
lang: ru
excerpt: "Инструкция по управлению шаговым двигателем в Hardella IDE. Hardella это среда для программирования ПЛК на языках группы 61131 (ST и т.п.)"
modified: 2017-01-24T18:30:00+03:00
---


Напишем программу для управления шаговым двигателем.
`STEP` вход будем подключать к `fast output 3`, а `DIR` к `fast output 4`.

Из основной программы CoDeSys будем задавать количество импульсов, частоту их следования.

Если у вас открыта среда Hardella IDE, то можете либо
{% include ide_link addr="r:8ec7a8ed-4f5a-443d-98ba-db336a7e181c/6983793011668973636" text="открыть код примера в среде" %}, либо создать свой проект на основе примера (`File` > `New` > `Project` > `Step motors`).

<img width="934" alt="PRU программа управления ШД" src="{{ "/assets/images/docs/pru/examples/step-motor/program.png" | absolute_url }}">

Код довольно простой, так как тут лишь вызов встроенного блока управления ШД и запись выходов.

`@Export` перед объявлением переменной `dir` означает, что эта переменная будет напрямую передаваться из CoDeSys в PRU (с точки зрения PRU это `input` переменная)

А `@Export` перед объявлением ФБ `PRU_STEPPER` означает, что часть переменных этого ФБ будут обмениваться с основным циклом ПЛК.

Далее происходит запись значений в быстрые выходы.

Аналогично можно составить программу для PRU1 и управлять вторым шаговым двигателем на выходах fast output 1 и fast output 2.

Укажем длительность цикла PRU 1 мкс.

<img width="584" alt="Настройка PRU" src="{{ "/assets/images/docs/pru/examples/step-motor/pru-configuration.png" | absolute_url }}">

[Скомпилируем программу](/docs/pru/project-build/), [загрузим в CoDeSys](/docs/pru/codesys-setup/), и посмотрим что получилось.

Опишем запуск ШД на стороне КДС (перед использованием Pru0 программ нужно [залить PRU0.prg в ПЛК](/docs/pru/codesys-setup/), PRU1.prg -- не обязательно):

    SteppersConfig_Pru0Init(); (* загрузка кода в PRU0 *)
    (* PRU запущены, можно обмениваться данными *)

    SteppersConfig_Pru0MemoryRead(); (* читаем данные из PRU0 *)
    
    (* Выполняем логику программы *)
    SteppersConfig_Pru0MemoryWrite.STEPPER_PRU0_stepper_enable :=
      SteppersConfig_Pru0MemoryRead.STEPPER_PRU0_stepper_state <> STOP_STEPPER_RUN_STATE;
    
    (* Записываем данные в PRU0 *)
    SteppersConfig_Pru0MemoryWrite(
      STEPPER_PRU0_dir := TRUE,
      STEPPER_PRU0_stepper_accel_ramp := 100,
      STEPPER_PRU0_stepper_decel_ramp := 100,
      STEPPER_PRU0_stepper_max_speed := 30,
      STEPPER_PRU0_stepper_min_speed := 0,
      STEPPER_PRU0_stepper_quantity := 100
    );

- `quantity`, шт -- количество генерируемых импульсов
- `min_speed`, Гц -- начальная частота импульсов ШД
- `max_speed`, Гц -- максимальная частота импульсов ШД
- `accel_ramp`, `decel_ramp` Гц/сек -- ускорение при разгоне/торможении. 
- `enable` -- команда на запуск

Стоит учесть, что после отработки нужного количества импульсов ШД останавливается (импульсы прекращаются). А `enable` по-прежнему `TRUE`. Возникает вопрос: как узнать, что импульсы сгенерированы, и можно запускать заново?

Ответ на этот вопрос даёт выходной параметр `state`. В процессе работы, этот параметр принимает значения `INIT` (стоим, настройка параметров), `ACCEL` (разгон), `RUN` (движение на максимальной скорости), `DECEL` (замедление), `STOP` (движение завершено).

Следующее движение начнётся только после сброса `enable`. Т.е. чтобы запустить ШД второй раз, нужно передёрнуть `enable`.

Поэтому команда `enable := state <> STOP` по факту и организует «вечный запуск ШД», а небольшие частоты (30 Гц) позволяют глазами увидеть, что «импульсы идут».

При необходимости, аналогично запускается и второй ШД (при использовании PRU1 нужно заливать оба файла и PRU0.prg и PRU1.prg в ПЛК):

    SteppersConfig_Pru1Init(); (* загрузка кода в PRU1 *)
    (* PRU запущены, можно обмениваться данными *)

    SteppersConfig_Pru1MemoryRead(); (* читаем данные из PRU1 *)
    
    (* Выполняем логику программы *)
    SteppersConfig_Pru1MemoryWrite.STEPPER_PRU1_stepper_enable :=
      SteppersConfig_Pru1MemoryRead.STEPPER_PRU1_stepper_state <> STOP_STEPPER_RUN_STATE;
    
    (* Записываем данные в PRU1 *)
    SteppersConfig_Pru1MemoryWrite(
      STEPPER_PRU1_dir := TRUE,
      STEPPER_PRU1_stepper_accel_ramp := 100,
      STEPPER_PRU1_stepper_decel_ramp := 100,
      STEPPER_PRU1_stepper_max_speed := 30,
      STEPPER_PRU1_stepper_min_speed := 0,
      STEPPER_PRU1_stepper_quantity := 100
    );
