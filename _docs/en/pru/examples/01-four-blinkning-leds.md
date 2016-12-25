---
title: "Fast input-output control"
permalink: /docs/pru/examples/four-blinkning-leds/
lang: en
excerpt: "Fast input-output control in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---


Let's make outputs 1 and 2 blink with 250 ms interval, and let's make outputs 3 and 4 blink with 500 ms interval.

If you have Hardella IDE opened, you can either open
{% include ide_link addr="r:31c55dd8-3058-4da1-84b2-4ce3e79191b5/2559549685963163651" text="code sample in the IDE" %}, or you can create a new project with this code (`File` > `New` > `Project` > `Blinking leds`).

As project opens you can see the following:
  1. PRU Configuration -- it describes which programs are assigned to each PRU core, it specifies PRU cycle length for each core, and it sets debounce intervals. In this particular case PRU Configuration is named `BlinkningLeds`:

     <img width="841" alt="PRU configuration" src="{{ "/assets/images/docs/pru/default-project-view.png" | absolute_url }}">
     
     Debounce is disabled (see `no debounce`). As you can see, PRU0 executes `BLINK_3_4` with 500 ms interval. That is PRU0 core will execute program `BLINK_3_4` at most once every 500 ms.
 
  1. Here's the program for `PRU0`. In this case it is named `BLINK_3_4`:

     <img width="587" alt="BLINK_3_4" src="{{ "/assets/images/docs/pru/examples/blinkingleds/blink_3_4.png" | absolute_url }}">
     
     It should not be that hard, however let's discuss that in more details.
     The program writes the value of `state` into `fast out 3` (`out3`), then it assigns the value of `NOT state` to `fast out 4` (see `out4`).
     The value of `state` variable is altered when `enable` is `TRUE` only.
     
     How can `enable` change its value? As you can see, `enable` variable has CoDeSys data exchange configured (see [data exchange](/docs/pru/data-exchange/)). That means `enable` would be passed from the main PLC cycle, however `BLINK_3_4` would be executed at PRU cores.

  1. Here's a program for `PRU1`. It is named `BLINK_1_2`:

     <img width="587" alt="BLINK_1_2" src="{{ "/assets/images/docs/pru/examples/blinkingleds/blink_1_2.png" | absolute_url }}">

     The contents is exactly the same as in `BLINK_3_4`, except `out1` and `out2` are used.

As you build the project, Hardella generates `MemoryTransfer` CoDeSys programs that will have variables to control `enable`.
Here's an example of the relevant program for PRU0:

    PROGRAM BlinkningLeds_Pru0MemoryTransfer
    VAR_INPUT
      BLINK_3_4_enable : BOOL;
    END_VAR
    ...

That means in order to pass `enable:=TRUE` to PRU0 program, it is required to execute the following code in the main PLC program:

    BlinkningLeds_Pru1MemoryTransfer(BLINK_3_4_enable := TRUE)
