---
title: "Быстрый энкодер"
permalink: /docs/pru/examples/fast-encoder/
lang: ru
excerpt: "Инструкция по обработке быстрого энкодера в Hardella IDE"
modified: 2016-12-14T22:39:43+03:00
---

Сделаем обработку ABZ энкодера. В целом, этот пример мало чем отличается от создания [быстрого счётчика](/docs/pru/examples/fast-counter/).

Если у вас открыта среда Hardella IDE, то можете либо
{% include ide_link addr="r:bac03eb6-fb9e-48db-8e6f-79d27afe9caf/5925665464103304700" text="открыть код примера в среде" %}, либо создать свой проект на основе примера (`File` > `New` > `Project` > `Fast encoder`).

<img width="685" alt="Программа обработки энкодера" src="{{ "/assets/images/docs/pru/examples/fast-encoder/program.png" | absolute_url }}">

Из интересного:
  1. В основной цилк ПЛК передаются 3 значения с блока энкодера: `position`, `counter`, `zeroDetected`
  
  1. Сам по себе блок обработки энкодера не является чем-то магическим. Если зажать `ctrl` и нажать на название блока `PRU_ABZ_ENCODER`, то можно посмотреть как он устроен:
  
     <img width="709" alt="Исходный код блока обработки энкодера" src="{{ "/assets/images/docs/pru/examples/fast-encoder/encoder-src.png" | absolute_url }}">

Таким образом можно практически всегда подсмотреть внутрь «стандартной библиотеки».