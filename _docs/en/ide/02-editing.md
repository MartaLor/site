---
title: "Code editing"
permalink: /docs/ide/editor/
lang: en
excerpt: "Code editing in Hardella IDE essentials"
modified: 2016-12-19T18:50:00+03:00
---

{% include toc icon="columns" title="Code editing" %}

The important feature of Hardella IDE is that it is not a text editor.
ST language editor looks like text, however it has some features of "fill in the form" editor with empty cells waiting to be filled.

This might look odd, especially when you see it for the first time, however you will quickly learn how to use it. For instance, this feature is very visible when you declare a variables.

Due to this peculiarity text paste does not always work. If you copy the source from Hardella, it will paste just fine. However if you copy from other tool (e.g. browser), then paste will fail (this might be improved in future).

## Creation of functional blocks

To create a functional block, right click in a `Logical View` (on the left), and choose `New` > `c.g.v.iec61131.types` > `FB`.

 <img width="848" alt="New FB menu" src="{{ "/assets/images/docs/ide/new-fb-menu.png" | absolute_url }}">

Alternative approach is to use `ctrl+c / ctrl+v` for an existing block.

Here's how a block would like right after creation:

 <img width="721" alt="A brand new block" src="{{ "/assets/images/docs/ide/new-fb-empty.png" | absolute_url }}">

As you can see, the wire-frame is there (words like `FUNCTION_BLOCK`, `variables`, `body`), and there are gaps for the values.

Block name is mandatory, so the gap for the name is marked with red. The variable list and block body are optional, so they are not marked with red.

Let's create a block for `OR` operation. There's no much sense in that block (as one can just use `a OR b`), however it is fine for demo purposes.

So we type a block name:
 <img width="453" alt="New block with name" src="{{ "/assets/images/docs/ide/or-named.png" | absolute_url }}">

Then you need to declare variables. In order to do that you can hit `tab` and the cursor will move to the variable declaration section. You can use arrows and mouse as well depending on what works better for you.

### Variable declaration

In order to add an element to the list, you typically just need to hit `enter`. In other words, as soon as you hit `enter` Hardella generates template for the variable declaration.

 <img width="453" alt="Template for variable declaration" src="{{ "/assets/images/docs/ide/var-template.png" | absolute_url }}">

You need to specify variable name and type separate. The semicolon in between is already there and you don't need to type it.

Specify `x` for the variable name, and hit `tab` (or right arrow) to continue with data type declaration.

 <img width="107" alt="Variable name is specified" src="{{ "/assets/images/docs/ide/var-without-type.png" | absolute_url }}">

Now you need to specify `BOOL` type somehow. One of the ways is just type  `BOOL` in capital letters.
The more interesting way is to use autocomplete feature.
You can start typing or you can just hit `ctrl+space` to invoke autocomplete.

 <img width="567" alt="Datatype specification" src="{{ "/assets/images/docs/ide/type-autocomplete.png" | absolute_url }}">

`BOOL` is the second on the list, so you can use arrow keys to choose it and confirm with `enter`.
This approach enables you to proceed even if you don't remember the full variable name.

Well, we did achieve `x : BOOL`, however it does not mean the variable is `input` variable. How do we get there?

#### `input` and `output` flags 

In order to specify `input` and `output` flags, you need to place cursor to the left of the variable name (i.e. to the left of `x`), hit `space` and use one of the following possibilities:
  - type `input` (or `output`)
  - hit `ctrl+space` and pick the required option on the menu:
    <img width="352" alt="input and output flags" src="{{ "/assets/images/docs/ide/input-output.png" | absolute_url }}">
  
### Code line duplication

One input variable is not enough, so we can duplicate it with `ctrl+d` shortcut (that is named after duplicate).
Hit that shortcuts several times and you'll get the following:
  <img width="165" alt="Duplicated variables" src="{{ "/assets/images/docs/ide/vars-duplicated.png" | absolute_url }}">

The second and the third variables are underlined with red since they result in duplicate variable name declaration error. 

Now you need to update variable names, and change the type of the last variable from `input` to `output`. In order to do that, just delete`input` and add `output` like you did before.

The IDE adds empty divisor lines automatically between variables of different types, so the final result will look like the following:

  <img width="173" alt="Variables are declared" src="{{ "/assets/images/docs/ide/vars-declared.png" | absolute_url }}">

## Editing expressions

In order to create body of a function block, you need to navigate to the body part, press `ctrl+space`, and pick `StatementList`.
Now you can write ST code.

In order to produce `z := x OR y;`, you need to follow this sequence:
  1. `z`. The screen will read: `z;`
  1. `:=`. The screen will read: `z := ___;`
  1. `x`. The screen will read: `z := x;`
  1. `space`. If you omit the space after `x`, then IDE will think you mean variable named `xOR` instead of `x OR`
  1. `OR`. The screen will read: `z := x OR ___;`
  1. `y`
  
You can tackle the problem in another way:
  1. `:`. The screen will read: `___ := ___;` The thing is the assignment is the only valid operation in this context that has double colon, so as soon as you type `:` IDE creates assignment statement
  1. `z`. The screen will read: `z := ___;`
  1. `tab` in order to move to the right side of the assignment
  1. `OR`, press `ctrl+space`, and pick `or expression` on the menu. The screen will read `z := ___ OR ___;`
  1. Then you can fill in left and right sides of the or expression

## Copy & paste

`ctrl+c` / `ctrl+v` just works.
It is important that IDE honors data types and the sense of the code.
For instance, if you copy `IF ... END_IF` to the clipboard and try to paste it to the place of variable declaration, then nothing will happen. Well, it makes no sense to insert `IF` into variable declaration place, however IDE behaves similarly in other aspects.

On the other hand, if you copy a variable declaration, then you could insert it side by side with another variable just fine.

Typically statements are copied in whole. For instance, you can't copy `IF` without corresponsing `END_IF`. You cannot omit some of `ELSIF` branches. If you really need to copy just part of the `IF` statement, duplicate the statement and cut non required parts later.

## Variable and funciton block rename

In order to rename a variable or function block you just need to navigate to the declaration and rename it. All the usages of the variable in question will be update automatically.

## Code movement: up and down

It is often the case that you need to move lines around. For instance, you might need to move a couple of lines under `IF` statement, or move lines out of `IF` statement.

In order to do that, you can use `ctrl+shift+↑` and `ctrl+shift+↓` hotkeys.