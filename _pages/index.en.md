---
layout: splash
author_profile: true
lang: en
permalink: /
header:
  overlay_color: "#4269bc"
  overlay_filter: "linear-gradient(-45deg, #4253BD, #427FBD)"
  cta_label: "Download Hardella 1.7.1 (2017-05-06)"
  cta_btn_class: 'btn--primary'
  cta_url: "/download"
excerpt: "Smart programming environment for PLC (61131 ST & Co)"
description: >
  Hardella is a capable IDE for PLC programming in 61131 languages.
  It focuses on developer productivity and on the fly error checking.  
intro: 
  - excerpt: 'Hardella aims to provide the features of modern IDEs like on the fly error highlight, find usages, context-aware autocomplete, and so on'
feature_row:
  - image_path: /assets/images/docs/pru/examples/step-control.png
    image_width: 275px
    alt: "Step motor control"
    title: "Step motor control"
    excerpt: "Hardella has integrated support for step motors. PRU cores can drive **up to 500kHz**. You can create your logic bases on existing components or even create your own components."
    url: "/docs/pru/examples/step-motor/"
    btn_label: "Read More"
    btn_class: "btn--inverse"
  - image_path: /assets/images/docs/pru/examples/st-logic.png
    image_width: 272px
    title: "Fast input-output control"
    excerpt: "PRU cores allow you to create logic that responds **under 1µs**. You don't need assembler for that. ST language is just fine."
    url: "/docs/pru/examples/four-blinkning-leds/"
    btn_label: "Read More"
    btn_class: "btn--inverse"
  - image_path: /assets/images/docs/pru/examples/encoder-feedback.png
    image_width: 271px
    alt: "Encoder as a feedback"
    title: "Encoder as a feedback"
    excerpt: "Hardella enables you to use encoder signal as a feedback, so you can turn the motor off right in time to achieve precise and even cuts."
    url: "/docs/pru/examples/material-cutter/"
    btn_label: "Read More"
    btn_class: "btn--inverse"
feature_row2:
  - image_path: /assets/images/docs/pru/examples/error-demo.png
    image_width: 289px
    alt: "On the fly error highlighting"
    title: "On the fly error highlighting"
    excerpt: 'Hardella highlights errors as you type making it a greater developer experience.'
    btn_label: "Read More"
    btn_class: "btn--inverse"
feature_row3:
  - image_path: /assets/images/docs/pru/examples/autocomplete.png
    image_width: 290px
    alt: "Autocomplete"
    title: "Autocomplete"
    excerpt: 'In case of trouble, just hit `ctrl+пробел`, and Hardella will give you a hint on the possible continuation.'
feature_row4:
  - image_path: /assets/images/docs/pru/examples/cfc-example.png
    image_width: 278px
    alt: "CFC extension"
    title: "Easy to extend"
    excerpt: 'Hardella is based on [JetBrains MPS](https://www.jetbrains.com/mps/), so it is easily extensible. For instance, diagraming language can be added right in the middle of the program code'
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}

{% include feature_row id="feature_row2" type="left" %}

{% include feature_row id="feature_row3" type="right" %}

{% include feature_row id="feature_row4" type="center" %}
