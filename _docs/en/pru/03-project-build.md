---
title: "PRU project build"
permalink: /docs/pru/project-build/
lang: en
excerpt: "This page describes how to build PRU project in Hardella IDE"
modified: 2016-12-19T19:35:00+03:00
---

In order to build the project you need the following:

  1. Call a context menu on a PRU Configuration node and pick `Run`

     <img width="570" alt="Create run configuration" src="{{ "/assets/images/docs/pru/create-run-configuration.png" | absolute_url }}">

  1. Specify the output path in the opened window. In case you run `Windows` the path could be like `C:\Temp`, however please refrain from using spaces.

     <img width="636" alt="Configuration of the output path" src="{{ "/assets/images/docs/pru/run-configuration-setup.png" | absolute_url }}">

  1. As you hit `Run` IDE will build the project. The result is printed in the `Run` tool window at the bottom (if it did not get activated, you could switch the required tool window)

     <img width="863" alt="The result of a successful build" src="{{ "/assets/images/docs/pru/build-result.png" | absolute_url }}">

As you created run configuration, you can just hit the green triangle at the screen top or at the left in the `Run` tool. You can use `Run` > `Run` menu as well.

As seen above:
  - The IDE created `/tmp/PRU0.prg`, `/tmp/PRU1.prg`, and `/tmp/BlinkningLeds.exp` files
  - The files `PRU0.prg` and `PRU1.prg` are always the same (they will be the same for all the projects build from Hardella IDE)
  - `/tmp/BlinkningLeds.exp` will be used as wrapper so CoDeSys program could communicate with PRU cores
