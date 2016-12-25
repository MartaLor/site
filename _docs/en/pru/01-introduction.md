---
title: "Features and limits"
permalink: /docs/pru/features-and-limits/
lang: en
excerpt: "Features and limits of PRU programming in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---
{% include toc icon="columns" title="Hardella for PRU programms" %}

To make long story short, you can create free-style 1µs programs.
You can process encoder inputs, you can control step motors. When using PRU program, you are not limited with 1ms cycle time of the main program. PRU cycle times are 1000 times faster, so you have a lot more fine grained control.

AM1808 CPU has two PRU cores, each of those operate at high frequency and the cores have direct access to fast inputs and outputs.

PRU cores operate at 200 MHz, and almost every command takes just 1 CPU tick to execute. The good part is PRU has no background program to execute, so you have dedicate processing power.

Of course it takes more than 1 tick to execute a complex program, however you can fit a lot in 200 ticks. So having cycle times of 1 µs should be doable.

The hard limit of a PRU core program is 1024 instructions. As you consider that there's just 2 outputs connected to each core, you might realize that 1024 is "enough for everybody". Instruction set includes simple 32 bit int math (**without** multiplication/division), and conditional jumps.

For instance, you can use PRU program to "disable the motor as soon as the required number of pulses arrives".

## Hardella vs CoDeSys

You can skip this section if you wish.

CoDeSys does not support PRU programming, so "PRU program" is composed in Hardella IDE. At the end of the day, everything is controlled from the main PLC loop, that is controlled by CoDeSys.

This works like the following:
  1. PRU is created in ST language in Hardella
  1. Hardella compiles the code into PRU machine code
  1. The compiled code is passed as a byte array in ST language to CoDeSys in a `.exp` format
  1. "Main PLC loop" uploads the program to PRU when the PLC is started, and PRU starts processing

## PLC110 М02 specifications

PRU programs were tested using [OWEN PLC110 М02](http://www.owen.ru/catalog/programmiruemij_logicheskij_kontroller_oven_plk110/opisanie).

  - PRU frequency: 200 МГц (lots of commands take 5 ns to execute)
  - Number of PRU cores: 2. PRU0 и PRU1
  - Fast inputs 1, 2, 3 and 4 are assigned to PRU0
  - Fast outputs 1 and 2 are assigned to PRU1
  - Fast outputs 3 and 4 are assigned to PRU0
  - Maximal PRU program size: 1024 assembly instructions
  - Amount of registry memory available: ~30 `DWORD` registers (~120 bytes) 

## Available options to drive PRU

By default fast inputs and outputs are configured in `PLC Configuration` in CoDeSys, however you can switch PRU to free programming mode.

Due to the PLC110 firmware limitations, only the following options are supported:

| PRU0              | PRU1              | Comment                       |
|-------------------|-------------------|-------------------------------|
| PLC Configuration | PLC Configuration | Default mode                  |
| Free program      | PLC Configuration | PRU0 runs user program        |
| Free program      | Free program      | Two programs at the same time |


## Programming language

PRU programming language is ST with certain limitations:
  - Complex expressions are not supported yet. For instance, in order to create `d := a+b-c` expression, you need to create a temporary variable as follows: `u := b-c; d:= a+b;`. Don't hesitate to create lots of temporary variables, as the system is typically smart enough to identify temporary variables. On contrary, if you use the same variable for different purposes, it might confuse the compiler.
  - Complex conditions with `AND`, `OR` in conditional statements (`IF`, `WHILE`, `REPEAT`) might fail to work. If the code got compiled, then it is fine, however there are unsupported cases that will result in compilation error.
  - `FUNCTION` is not supported
  - Structures are note supported
  - Arrays are not supported
  - **Only** `unsigned` data types are supported: `BOOL`, `BYTE`, `WORD`, `DWORD` and `ENUM`
  - All the variables are placed in the PRU registers. The code might fail to compile if you have lots of live variables. However intermediate calculations might reuse the same register, so the limit is not the number of variables, but the number of live variables. 

Features:
 - There's no "current time" function, however you can access PRU tick counter. Every tick is 5 ns. Remember, there's no div/multiply, so it is better to convert time to PRU ticks if you need notion of time.
 - You can use inline assembly in the ST code if you need


## PRU program execution model

Each PRU core follows the typical "cycle with fixed duration".

    WHILE TRUE DO
      execution_of_user_code(); (* <-- this executes user-provided code *)
      
      REPEAT 
         Data_exchange_with_main_program();
         Read_inputs();
      UNTIL need_to_wait_next_cyle 
      END_REPEAT;
      
      Write_outputs();
      Read_inputs(); (* just in case *)
    END_WHILE;

The cycle length is specified at build time and you can configure it via `PRU Configuration` object.
Like in a regular PLC, user-provided code should not use `WHILE TRUE`. That loop will be added by IDE automatically. User should just create a `PROGRAM` and specify which PRU core should execute that program.

In general PRU programming is close to PLC programing. The difference is PRU cycle length is way smaller.
