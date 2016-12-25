---
title: "Creating a PRU project in Hardella"
permalink: /docs/pru/new-project/
lang: en
excerpt: "Creating a PRU project in Hardella IDE. Hardella is a capable IDE for PLC programming in 61131 languages (ST, etc)"
modified: 2016-12-25T20:49:43+03:00
---

In order to create a project you need the following:
 1. On a welcome screen click `Create New Project` button, or pick `File` > `New` > `Project...` on the main menu
 <img width="807" alt="Welcome screen" src="{{ "/assets/images/docs/pru/welcome-screen.png" | absolute_url }}">
 
 2. Use `PLC110[M02] PRU` section in the templates list (on the left). For instance, `Empty PRU Project` (that will create empty PRU project), or `Blinking leds` to create a project that will blink with fast outputs.
 Let's pick `Blinking leds`:
  <img width="735" alt="New Project Window" src="{{ "/assets/images/docs/pru/new-project.png" | absolute_url }}">
 
 3. Then you need to pick a name for your project and specify the location folder (it's better to use folder with no spaces)

As you hit `OK`, IDE will open the project:
 <img width="841" alt="Demo PRU project" src="{{ "/assets/images/docs/pru/default-project-view.png" | absolute_url }}">

In the created project, every PRU core executes its own program. PRU0 operates at 500 ms cycle time, and PRU1 uses 250 ms cycle time. It does not make much sense in such high cycle times, however that makes it simpler for the naked eye to observe the execution. PRU cycle does not affect the main PLC cycle, so there's no harm in such cycle times.

You can read more on that example in the [examples section](/docs/pru/examples/four-blinkning-leds)

The project is ready to be started.

Hardella IDE has lots of shortcuts, and one of the most used enables you to navigate to the node by its name. You can use `Navigate` > `Goto to Root Node` (`ctrl+N` in Windows, `cmd+N` in macOS). Of course you can just doubleclick the required node on the left and open it.

For more details on navigating around the code see [code navigation](/docs/ide/navigation/) section.
