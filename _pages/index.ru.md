---
layout: splash
author_profile: true
lang: ru
permalink: /
header:
  overlay_color: "#4269bc"
  overlay_filter: "linear-gradient(-45deg, #4253BD, #427FBD)"
  cta_label: "Загрузить Hardella 1.7.1 (2017-05-06)"
  cta_btn_class: 'btn--primary'
  cta_url: "/download"
excerpt: "Удобная среда разработки для ПЛК (ST 61131 и т.п.)"
description: >
  Hardella это мощная среда для программирования ПЛК на языках группы 61131.
  Приоритетыми направлением является надёжность кода, удобство разработки и подсветка ошибок до компиляции кода.
intro: 
  - excerpt: 'Приоритетным направлением Hardella является удобство разработки и безопасность кода. Ошибки отображаются на лету, автодополнение знает о параметрах и переменных'
feature_row:
  - image_path: /assets/images/docs/pru/examples/step-control.png
    image_width: 275px
    alt: "Управление шаговым двигателем"
    title: "Крути ШД без забот"
    excerpt: "В Hardella есть встроенный блок управления шаговым двигателем. PRU ядра могут выдавать контролируемые импульсы **вплоть до  500кГц**. Вы можете как создавать свои схемы управления на основе имеющихся компонент, так и создавать свои компоненты."
    url: "/docs/pru/examples/step-motor/"
    btn_label: "Узнать Больше"
    btn_class: "btn--inverse"
  - image_path: /assets/images/docs/pru/examples/st-logic.png
    image_width: 272px
    title: "Управление быстрыми входами-выходами"
    excerpt: "PRU ядра позволяют создавать логику, которая реагирует **быстрее 1 мкс**. И для этого не нужно программировать на ассемблере. Языка ST вполне достаточно."
    url: "/docs/pru/examples/four-blinkning-leds/"
    btn_label: "Узнать Больше"
    btn_class: "btn--inverse"
  - image_path: /assets/images/docs/pru/examples/encoder-feedback.png
    image_width: 271px
    alt: "Энкодер для обратной связи"
    title: "Энкодер для обратной связи"
    excerpt: "Hardella позволяет использовать любые сигналы. Например, вы можете использовать сигнал с энкодера для обратной связи и останавливать мотор вовремя. Это открывает новые горизонты по точности, ведь PRU цикл в 1000 раз быстрее «основного цикла ПЛК»."
    url: "/docs/pru/examples/material-cutter/"
    btn_label: "Узнать Больше"
    btn_class: "btn--inverse"
feature_row2:
  - image_path: /assets/images/docs/pru/examples/error-demo.png
    image_width: 289px
    alt: "Подсветка ошибок на лету"
    title: "Подсветка ошибок на лету"
    excerpt: 'Hardella подсвечивает ошибки прямо во время написания кода. Это сильно упрощает написание программ, т.к. типичные ошибки будут устраняться сразу.'
feature_row3:
  - image_path: /assets/images/docs/pru/examples/autocomplete.png
    image_width: 290px
    alt: "Автодополнение"
    title: "Автодополнение"
    excerpt: 'В любой непонятной ситуации нажимай `ctrl+пробел` и Hardella подскажет что может находиться на этом месте.'
feature_row4:
  - image_path: /assets/images/docs/pru/examples/cfc-example.png
    image_width: 278px
    alt: "Расширение CFC"
    title: "Легко расширять"
    excerpt: 'Среда Hardella основана [JetBrains MPS](https://www.jetbrains.com/mps/), поэтому её очень легко расширять. Для примера, диаграмму можно разместить прямо посреди кода, и такая доработка может быть выполнена за часы.'
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}

{% include feature_row id="feature_row2" type="left" %}

{% include feature_row id="feature_row3" type="right" %}

{% include feature_row id="feature_row4" type="center" %}
