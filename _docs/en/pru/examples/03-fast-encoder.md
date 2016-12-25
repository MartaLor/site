---
title: "Fast encoder"
permalink: /docs/pru/examples/fast-encoder/
lang: en
excerpt: "Processing fast encoder in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---

Let's process values from ABZ encoder. In general, it does not differ that much from [fast counter](/docs/pru/examples/fast-counter/).

If you have Hardella IDE opened, you can either open
{% include ide_link addr="r:bac03eb6-fb9e-48db-8e6f-79d27afe9caf/5925665464103304700" text="code sample in the IDE" %}, or you can create a new project with this code (`File` > `New` > `Project` > `Fast encoder`).

<img width="685" alt="Encoder processing program" src="{{ "/assets/images/docs/pru/examples/fast-encoder/program.png" | absolute_url }}">

The most interesting parts are:
  1. PRU passes three values from the encoder block to the main PLC program: `position`, `counter`, `zeroDetected`
  
  1. The encoder block is not something magical. You can hold `ctrl` and click its name `PRU_ABZ_ENCODER` to navigate to the source:
  
     <img width="709" alt="Source code of encoder processing block" src="{{ "/assets/images/docs/pru/examples/fast-encoder/encoder-src.png" | absolute_url }}">

     You can use the same `ctrl+click` approach to see the sources of other blocks of the [standard library](/docs/pru/standard-library/).