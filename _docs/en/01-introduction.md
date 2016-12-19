---
title: "Introduction"
permalink: /docs/introduction/
lang: en
excerpt: "Introduction to Hardella IDE"
modified: 2016-12-14T22:39:43+03:00
---

Hardella IDE makes PLC programming a breeze.

The key features of the IDE are developer productivity and code safety.

For instance, you can find usages of a variable, you can safely rename a variables, and so on. There's no problem if two variables placed in different blocks share a name, as Hardella would understand the variables are different.

Hardella would underline invalid code before it gets compiled. For instance, if you write `IF 1 THEN`, then a error message would appear saying that "if condition should have a boolean expression type".

 <img width="289" alt="Error example" src="{{ "/assets/images/docs/pru/examples/error-demo.png" | absolute_url }}">
  
For the newbies, the IDE simplifies coding: almost at any time you can hit `ctrl+space` and get autocomplete of the possible continuations. This frees you from invalid function and variable references making your code robust.

  <img width="704" alt="Autocomplete example" src="{{ "/assets/images/docs/ide/autocomplete-comments.png" | absolute_url }}">

You can type `IF`, `WHILE`, etc via autocomplete as well. Hardella would ensure proper `END_IF`, semicolons, indentation, and so on. As you delete `IF`, the IDE would automatically delete `END_IF` for you.  


Hardella IDE is based on top of [JetBrains MPS](https://www.jetbrains.com/mps/) and [Mbeddr Platform](http://mbeddr.com/).

Current features include:

  - Program creation for AM1808 PRU processor (for instance, you can control fast inputs and outputs of [OWEN PLC110 М02](http://www.owen.ru/catalog/programmiruemij_logicheskij_kontroller_oven_plk110/opisanie)).
  
    Hardella creates a binary program for PRU cores and it creates a CoDeSys wrapper library as well.
     
  - Code editing in ST language with export to CoDeSys 2.3. In this scenario Hardella is just a code editor (albeit with `PLC Configuration` support), and the code exported in `*.exp` CoDeSys format.

Upload to PLC and online debug is not implemented yet.
You can debug PRU programs via [pru-emulator](https://github.com/vlsi/pru-emulator).
