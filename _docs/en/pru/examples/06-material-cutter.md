---
title: "Precise material cutter"
permalink: /docs/pru/examples/material-cutter/
lang: en
excerpt: "Implementing material cutter logic in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---

The problem was described in [owen.ru forum](http://www.owen.ru/forum/showthread.php?t=23600&page=9&p=222957&viewfull=1#post222957).

Here's the problem:
1. The motor shaft of the material transport is connected to the encoder that has resolution of 500 pulses / rev
1. The maximum engine speed is 3000 rev / min (typically about 2000 / min)
1. The time to supply the desired amount of material is near 0.4..0.6 seconds
1. The motor has low inertia, so the stop occurs almost instantly.

The task is to count the required number of pulses and stop the motor. 

As described above the counter should be reliable up to 30 kHz.

If you have Hardella IDE opened, you can either open
{% include ide_link addr="r:ce2450a8-9d37-4646-b206-51338109fd95/7014233255259703463" text="code sample in the IDE" %}, or you can create a new project with this code (`File` > `New` > `Project` > `Material cutter`).

## Solution

Let's create a enumeration for the system state (in order to do that you need to right-click, then select `New` > `c.g.v.iec66131.types` > `type alias`):

<img width="360" alt="Enum for the system state" src="{{ "/assets/images/docs/pru/examples/material-cutter/state.png" | absolute_url }}">

Then we need to create a ST logic:

<img width="946" alt="The logic of the cutter" src="{{ "/assets/images/docs/pru/examples/material-cutter/cutter-logic.png" | absolute_url }}">

It is more or less clear: it performs one or the another action depending on the current state.

If the movement is complete (that is current state is `STOP`), then it waits for the `enable` to become `FALSE`.

In the initial state `INIT` it waits for the setup of run length (`runLength`), then it waits for the start flag (`enable`)

During the run, it counts the run length into `offset` variable and stops as the required run length is achieved.

<img width="658" alt="PRU program" src="{{ "/assets/images/docs/pru/examples/material-cutter/program.png" | absolute_url }}">

PRU program is just 3 lines long:
  1. The first line calls ABZ encoder to get its position
  1. The second line calls the cutter logic. It uses the value of 4th input for the `cntr` argument
  1. The motor is controlled via fast output 3 (`out3`), and the value comes from the cutter logic (`cutter.out`)

