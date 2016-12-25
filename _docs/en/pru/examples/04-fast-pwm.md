---
title: "Fast PWM"
permalink: /docs/pru/examples/fast-pwm/
lang: en
excerpt: "Example of generating PWM output in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---

Let's create a PWM output via fast outputs.

One of the "features" of PRU programs is you can't just call `TIME()` and get "current time". On the other hand, PRU programs are highly predictable, and PRU cores have no other work to do except user program. For instance PRU core does not have overhead like "TCP processing". That is why you can think of PRU cycle to have exact duration.

If you have Hardella IDE opened, you can either open
{% include ide_link addr="r:df9df427-3641-4bbb-bd89-88f80031fc2f/5925665464103441990" text="code sample in the IDE" %}, or you can create a new project with this code (`File` > `New` > `Project` > `Fast counter`).

PWM output is trivial if you use standard library:

<img width="434" alt="PWM program" src="{{ "/assets/images/docs/pru/examples/fast-pwm/program.png" | absolute_url }}">

PWM period (`period`) and PWM value (`value`) come from the main PLC program.

On the other hand, PWM is quite trivial: it just counts the number of calls and assigns the value to `Q` depending on the PWM value.

<img width="657" alt="PWM block source code" src="{{ "/assets/images/docs/pru/examples/fast-pwm/pwm-src.png" | absolute_url }}">
