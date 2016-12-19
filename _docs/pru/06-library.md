---
title: "Библиотека блоков"
permalink: /docs/pru/standard-library/
lang: ru
excerpt: "Описание PRU блоков в базовой поставке Hardella IDE"
modified: 2016-12-14T22:39:43+03:00
---

{% include toc icon="columns" title="Библиотека блоков" %}

## Блоки для аппаратной части PRU

### FAST_INPUTS

Блок для чтения значений с быстрых входов. Получаемые данные уже **учитывают настройки фильтрации**.

Следует помнить, что все входы заведены на PRU0, т.е. использовать `FAST_INPUTS` в программе для PRU1 не получится.
 
Использовать `FAST_INPUTS` можно в любом месте кода. Можно объявить и несколько переменных для `FAST_INPUTS`, но особого смысла нет.

    FUNCTION_BLOCK FAST_INPUTS (* returns values of fast inputs *) 
        output in1 : BOOL; (* fast input 1 *) 
        output in2 : BOOL; (* fast input 2 *) 
        output in3 : BOOL; (* fast input 3 *) 
        output in4 : BOOL; (* fast input 4 *) 

### FAST_OUTPUTS

Блок для управления быстрыми выходами.
Следует помнить, что выходы 3 и 4 распаяны к PRU0, а выходы 1 и 2 к PRU1.
При попытках "управлять 1-ым выходом в программе для PRU0" будет ошибка компиляции.

    FUNCTION_BLOCK FAST_OUTPUTS (* specifies values for the fast outputs *) 
        input out1 : BOOL; (* fast output 1 (PRU1) *) 
        input out2 : BOOL; (* fast output 2 (PRU1) *) 
        input out3 : BOOL; (* fast output 3 (PRU0) *) 
        input out4 : BOOL; (* fast output 4 (PRU0) *) 

### PRU_CURRENT_TIME

Позволяет "узнать" текущее время и длину PRU цикла (в **тактах**, каждый из которых составляет 5нс). Стоит понимать, что как таковых часов в PRU нет, поэтому под временем здесь понимается "счётчик выполненных тактов процессора".

В основе лежат следующие предположения:
  - PRU тактуется от кварца (так говорили ОВЕН на форуме owen.ru)
  - Частота PRU -- 200 МГц (подтверждена осциллографом)
  - PRU регистр, который считает выполненные такты работает верно. Например, считается, что на него не влияют разнообраные операции с памятью и т.п.

Специально точность ведения времени не проверяли, но импульсы 500кГц выглядели как 500кГц.

Каждый вызов блока `PRU_CURRENT_TIME` будет запрашивать новое значение "счётчика выполненных PRU тактов", поэтому не стоит ставить вызовы `time()` после каждой строки.
Например, блок фильтрации входов один раз обновляет текущее время, а затем на его основе выполняет фильтрацию всех входов.

    FUNCTION_BLOCK PRU_CURRENT_TIME 
        output totalCycles : DWORD; (* Total CPU ticks elapsed *) 
        output prevOutputWriteTime : DWORD; (* Absolute time when previous outputs were written (in CPU ticks) *) 
        output cycleLength : DWORD; (* PRU cycle length in CPU ticks *) 

Пример использования

    time : PRU_CURRENT_TIME;
    ...
    time(); (* узнать текущее время *)
    IF time.totalCycles < deadLine THEN ...
    ...
    deadLine := time.totalCycles + 100500;

## Блоки для сложных устройств

### PRU_ABZ_ENCODER

Блок обработки сигнала ABZ энкодера.
Учитываются оба фронта (передний и задний) A и B фаз.

`zeroDetected` переходит в `TRUE` когда впервые найдена Z метка.
`counter` возвращает общее количество учтённых фронтов A и B фаз.

    FUNCTION_BLOCK PRU_ABZ_ENCODER (* Decodes ABZ encoder. Each edge of both A and B are processed *) 
      variables: 
        input a : BOOL; (* A input *) 
        input b : BOOL; (* B input *) 
        input z : BOOL; (* Z input *) 
         
        output position : WORD; (* position of the encoder *) 
        output counter : WORD; (* number of encoder pulses processed *) 
        output zeroDetected : BOOL; (* TRUE when Z mark was detected *) 

### PRU_STEPPER

Блок для управления шаговым двигателем с помощью STEP сигнала.
Блок генерирует импульсы 50% скважности.

    FUNCTION_BLOCK PRU_STEPPER (* Step motor controller *) 
      variables: 
        input enable : BOOL; (* Starts and stops the motor *) 
        input quantity : DWORD; (* Number of pulses to generate *) 
        input accel_ramp : WORD; (* Acceleration in Hz/sec. 0 means no acceleration is made *) 
        input decel_ramp : WORD; (* Deceleration in Hz/sec. 0 means no deceleration is made *) 
        input min_speed : DWORD; (* Minimal pulse speed in Hz *) 
        input max_speed : DWORD; (* Maximal pulse speed in Hz *) 
         
        output state : STEPPER_RUN_STATE; (* State of the stepper block (init, moving, stopped, etc) *) 
        output step_count : DWORD; (* Number of steps made. DO NOT use to check the state of the motor. Use state instead *) 
        output Q : BOOL; (* STEP signal for the stepper motor *) 


## Математика

В PRU ядрах нет встроенного умножителя и делителя.
Тем не менее, алгоритмы "умножение по-школьному" позволяют умножать, делить и вычислять квадратные корни за разумное время.

Умножение и деление двух `DWORD` зависят от фактических значений и в худшем случае требуется около 1µs (200 тактов).
Если требуется умножать на степень двойки, то лучше использовать `SHL(x, y)` (PRU компилятор пока не поддерживает операцию `*`).

### PRU_DIV_DW_DW

Блок выполняет целочисленное деление `DWORD`, возвращает частное и остаток.
Время работы зависит от значений. В худшем случае требуется около 1µs (200 тактов)

    FUNCTION_BLOCK PRU_DIV_DW_DW 
        input x : DWORD; 
        input y : DWORD; 
         
        output div : DWORD; 
        output mod : DWORD; 

### PRU_MULDIV_DW_DW_DW

Блок для вычисления `DIV(MUL(x, y), z)` и `MOD(MUL(x, y), z)`.
Время работы зависит от значений.

    FUNCTION_BLOCK PRU_MULDIV_DW_DW_DW (* calculates DIV(x*y, z); MOD(x*y, z) *) 
        input x : DWORD; 
        input y : DWORD; 
        input z : DWORD; 
         
        output div : DWORD; (* div := DIV(MUL(x, y), z) *) 
        output mod : DWORD; (* mod := MOD(MUL(x, y), z) *) 

### PRU_MUL_DW_DW

Блок для умножения двух `DWORD`.
Время работы зависит от значений. В худшем случае требуется около 1µs (200 тактов)

    FUNCTION_BLOCK PRU_MUL_DW_DW (* x*y *) 
        input x : DWORD; 
        input y : DWORD; 
         
        output mul : DWORD; 

### PRU_MUL_DW_W

Блок для умножения `DWORD` * `WORD`.
Время работы зависит от значений. В худшем случае требуется около 0.5µs (100 тактов)

    FUNCTION_BLOCK PRU_MUL_DW_W (* x*y *) 
        input x : DWORD; 
        input y : WORD; 
         
        output mul : DWORD; 

### PRU_SQRT_DW

Блок для вычисления целой части квадратного корня из `DWORD` значения.
Время работы составляет около 0.25...1µs (50-150 тактов)

    FUNCTION_BLOCK PRU_SQRT_DW 
        input x : DWORD; 
         
        output sqrt : DWORD; 

## Прочее

### CTU_DWORD

    FUNCTION_BLOCK CTU_WORD 
        input CU : BOOL; (* counter input *) 
        input R : BOOL; (* reset *) 
        input PV : WORD; 
         
        output Q : BOOL; (* TRUE when CV >= PV *) 
         
        retain output CV : WORD; (* current counter value *) 

### F_TRIG

    FUNCTION_BLOCK F_TRIG (* ¯¯\__ detector *) 
        input CLK : BOOL; (* Input clock *) 
         
        output Q : BOOL; (* TRUE when falling edge is detected: ¯¯\__ *) 

### R_TRIG

    FUNCTION_BLOCK R_TRIG (* __/¯¯ detector *) 
        input CLK : BOOL; (* Input clock *) 
         
        output Q : BOOL; (* TRUE when rising edge is detected: __/¯¯ *) 

### RS-триггер

    FUNCTION_BLOCK RS (* reset-set trigger *) 
        input S : BOOL; 
        input R : BOOL; 
         
        output Q : BOOL; 

### SR-триггер

    FUNCTION_BLOCK SR (* set-reset trigger *) 
        input S : BOOL; 
        input R : BOOL; 
         
        output Q : BOOL; 

### PWM_DW

Блок генерирует ШИМ сигнал заданной скважности.
При каждом вызове внутренее состояние ШИМ блока увеличивается на 1.
С настройками `value=2, period=4` вывод Q будет принимать значения: 1, 1, 0, 0, 1, 1, ...
 
    FUNCTION_BLOCK PWM_DW (* Pulse Width Modulation generator *) 
        input value : DWORD;  (* Значение скважности *)
        input period : DWORD; (* Длина периода ШИМ *)
         
        output Q : BOOL; 

### PDM_DW

См [Pulse Density Modulation](https://en.wikipedia.org/wiki/Pulse-density_modulation). Не путать с ШИМ модуляцией!
Блок выдаёт 0 или 1 в такой пропорции, чтобы среднее значение было близко к `value/period`.

Наибольшее отличие от ШИМ будет на скважности 50%.
PDM будет выдавать 0, 1, 0, ..., а ШИМ будет выдавать длинные пачки нулей и единиц.

Второе отличие -- PDM гораздо быстрее переходит к новой сважности (не нужно дожидаться конца PWM импульса)

    FUNCTION_BLOCK PDM_DW (* Pulse Density Modulation generator *) 
        input value : DWORD; 
        input period : DWORD; 
         
        output Q : BOOL; 
