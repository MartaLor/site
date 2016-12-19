---
title: "Компиляция PRU проекта"
permalink: /docs/pru/project-build/
lang: ru
excerpt: "Инструкция по компиляции PRU проекта Hardella IDE"
modified: 2016-12-14T22:39:43+03:00
---

Для сборки проекта нужно

  1. Вызвать контекстное меню для PRU Configuration и выбрать пункт `Run`

     <img width="570" alt="Создание run configuration" src="{{ "/assets/images/docs/pru/create-run-configuration.png" | absolute_url }}">

  1. В открывшемся окне указать путь для размещения результатов. С случае с `Windows` можно указать что-нибудь в духе `C:\Temp`, но лучше избегать папок с пробелами в именах.

     <img width="636" alt="Настройка пути размещения результатов" src="{{ "/assets/images/docs/pru/run-configuration-setup.png" | absolute_url }}">

  1. После нажатия `Run` среда произведёт сборку проекта. Результат видно в нижней вкладке `Run` (если активна другая нижняя вкладка, то нужно переключиться)

     <img width="863" alt="Результат успешной сборки" src="{{ "/assets/images/docs/pru/build-result.png" | absolute_url }}">

В дальнейшем, достаточно нажимать зелёный треугольник наверху или слева на нижней вкладке `Run`, или использовать меню `Run` > `Run`.

Как видно в примере выше:
  - Созданы файлы `/tmp/PRU0.prg`, `/tmp/PRU1.prg`,  и `/tmp/BlinkningLeds.exp`
  - Файлы `PRU0.prg` и `PRU1.prg` всегда одинаковые (они всегда одинаковы для всех проектов, созданных в Hardella IDE)
  - `/tmp/BlinkningLeds.exp` будет служить для того, чтобы программа в CoDeSys смогла работать с написанной нами PRU программой
