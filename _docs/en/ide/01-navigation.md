---
title: "Code browsing"
permalink: /docs/ide/navigation/
lang: en
excerpt: "Code browsing in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---
{% include toc icon="columns" title="Code browsing" %}

Here's how a typical project in Hardella IDE might look like:

 <img width="953" alt="Demo PRU project" src="{{ "/assets/images/docs/pru/default-project-view.png" | absolute_url }}">

There are a couple of tricks that might simplify code analysis
The same principles are applicable to the standard library as well. For instance, you can browse `R_TRIG` source code if you wish.

## The most important shortcut

`ctrl+A` -- this shortcut brings a window that enables you to search over all the existing shortcuts.

For instance, if you want to jump to the `FAST_INPUTS` declaration, you can hit `ctrl+A`, and type "declaration"

 <img width="579" alt="Find action" src="{{ "/assets/images/docs/ide/find-action.png" | absolute_url }}">

As you can see now, you can drill down via `ctrl+B`.

## Go to declaration

As shown above, you can use `ctrl+B` shortcut, however you can hold `ctrl` and mouseclick to do the same.

## Find usages

`Find Usages` in the context menu for variables and function blocks enables you to find all the places where a particular variable was used.

 <img width="331" alt="Find usages menu" src="{{ "/assets/images/docs/ide/find-usages-menu.png" | absolute_url }}">

The results will be displayed in a separate tool window. As you doubleclick the result, the IDE will open the relevant code.

 <img width="360" alt="Find usages results" src="{{ "/assets/images/docs/ide/find-usages-result.png" | absolute_url }}">

