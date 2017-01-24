---
title: "Step motor control"
permalink: /docs/pru/examples/step-motor/
lang: en
excerpt: "Step motor programming in Hardella IDE via STEP/DIR interface. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2017-01-24T18:30:00+03:00
---


Let's create a program to control step motor.
`STEP` input will be connected to `fast output 3`, and `DIR` will be connected to `fast output 4`.

The main CoDeSys program will specify the number of pulses and pulse frequency.

If you have Hardella IDE opened, you can either open
{% include ide_link addr="r:8ec7a8ed-4f5a-443d-98ba-db336a7e181c/6983793011668973636" text="code sample in the IDE" %}, or you can create a new project with this code (`File` > `New` > `Project` > `Step motors`).

<img width="934" alt="PRU program that controls step motor" src="{{ "/assets/images/docs/pru/examples/step-motor/program.png" | absolute_url }}">

Let's set PRU cycle length to be 1 µs.

<img width="584" alt="PRU configuration" src="{{ "/assets/images/docs/pru/examples/step-motor/pru-configuration.png" | absolute_url }}">

We need to [build the project](/docs/pru/project-build/) and [load it to CoDeSys](/docs/pru/codesys-setup/). Let's check what we have got so far.

Here's how we would control the step motor from the CoDeSys program:

    SteppersConfig_Pru0Init(); (* PRU0 initialization *)
    (* PRU is up and running, now we can exchange the data *)

    SteppersConfig_Pru0MemoryRead(); (* read data from PRU0 *)
    
    (* Execute program logic *)
    SteppersConfig_Pru0MemoryWrite.STEPPER_PRU0_stepper_enable :=
      SteppersConfig_Pru0MemoryRead.STEPPER_PRU0_stepper_state <> STOP_STEPPER_RUN_STATE;
    
    (* Write data to PRU0 *)
    SteppersConfig_Pru0MemoryWrite(
      STEPPER_PRU0_dir := TRUE,
      STEPPER_PRU0_stepper_accel_ramp := 100,
      STEPPER_PRU0_stepper_decel_ramp := 100,
      STEPPER_PRU0_stepper_max_speed := 30,
      STEPPER_PRU0_stepper_min_speed := 0,
      STEPPER_PRU0_stepper_quantity := 100
    );

- `quantity`,  -- number of pulses to generate
- `min_speed`, Hz -- initial pulse frequency
- `max_speed`, Hz -- maximum pulse frequency
- `accel_ramp`, `decel_ramp` Hz/s -- acceleration and deceleration 
- `enable` -- start flag

As all the pulses are generated, the step motor stops (no more pulses is generated). Note that `enable` is still `TRUE`. That's the question: how do you know that all the pulses are generated and how do you know you can restart the stepper?

`state` parameter answers that question. That parameter takes the following values `INIT` (the step motor is stopped, parameters are being configured), `ACCEL` (acceleration), `RUN` (running at maximum speed), `DECEL` (deceleration), `STOP` (movement is complete).

The next move will happen only after `enable` is reset. That is you need to set `enable=FALSE`, then the subsequent `enable=TRUE` would start the next movement.

That is why `enable := state <> STOP` command would result in "evergreen step motor movement", and low frequencies (like 30 Hz) enable to see the pulses via naked eye.
