---
title: "Fast counter"
permalink: /docs/pru/examples/fast-counter/
lang: en
excerpt: "Creating a fast counter in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---

Let's create a fast counter that will count pulses coming to fast input 1.
As you might remember, if you create a counter in the regular PLC cycle, you would hit 1ms limit rather soon. That is you would fail to catch pulses that last less than 1ms (that is typical PLC cycle length). Of course you can configure the fast input in "counter" mode, however let's create a PRU program for fast counter so you can learn PRU programming.
 
PRU cycle can be as low as 1 µs, so it can catch very short pulses.

If you have Hardella IDE opened, you can either open
{% include ide_link addr="r:baebc184-c187-480d-b964-5b4447f7768e/5925665464103396016" text="code sample in the IDE" %}, or you can create a new project with this code (`File` > `New` > `Project` > `Fast counter`).


Let's set PRU cycle length to be 1 µs.

<img width="592" alt="PRU configuration" src="{{ "/assets/images/docs/pru/examples/fast-counter/pru-configuration.png" | absolute_url }}">

That means PRU program will check inputs once every 1 µs. That means you can have a reliable counter at speeds up to 250 kHz.

<img width="754" alt="Fast counter program" src="{{ "/assets/images/docs/pru/examples/fast-counter/program.png" | absolute_url }}">

The counter program is trivial, however let's note the following:
  1. `IF` condition has `R_TRIG` call, however there's no variable declared for the trigger. How's that possible? That's easy: Hardella can use implicit functional blocks. It is very convenient for one-time blocks like `R_TRIG` above.

  1. The counter `counter` variable is placed in PRU memory, however a [data exchange](/docs/pru/data-exchange/) is configured. That is how the results of the fast counter will be passed to the main CoDeSys program.