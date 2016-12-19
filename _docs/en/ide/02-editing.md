---
title: "Code editing"
permalink: /docs/ide/editor/
lang: en
excerpt: "Code editing in Hardella IDE essentials"
modified: 2016-12-14T22:39:43+03:00
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

Для добавления элемента в список следует использовать `enter`. Т.е. нажимаем `enter` и Hardella создаёт "рыбу" для объявления переменной.

 <img width="453" alt="Рыба объявления переменной" src="{{ "/assets/images/docs/ide/var-template.png" | absolute_url }}">

Следует учитывать, что имя указывается отдельно, тип отдельно. Двоеточие посредине уже указано и с ним ничего делать не нужно.

Пишем `x` (имя переменной), нажимаем `tab` (или стрелку вправо) для перехода к выбору типа переменной.

 <img width="107" alt="Указали имя переменной" src="{{ "/assets/images/docs/ide/var-without-type.png" | absolute_url }}">

Нужно как-то указать `BOOL` тип. Один вариант это взять и написать `BOOL` (большими буквами).
Более интересный -- и спользованием автодополнения.
Можно написать часть идентификатора, а можно вообще ничего не писать и нажать `ctrl+пробел`.

 <img width="567" alt="Выбор типа переменной" src="{{ "/assets/images/docs/ide/type-autocomplete.png" | absolute_url }}">

`BOOL` находится на втором месте, его можно выбрать стрелочками и нажать `enter`.
Такой подход помогает когда точное называние блока забыто.

Хорошо, получили `x : BOOL`, но как обозначить, что это будет входная переменная?

#### Флаги input, output 

Для указания `input`, `output` флагов, нужно разместить курсор слева от названия переменной (т.е. слева от `x`), нажать `пробел` и воспользоваться одним из следующих вариантов:
  - напечатать `input` (или `output`)
  - нажать `ctrl+пробел` и выбрать нужный вариант в меню:
    <img width="352" alt="Флаги input, output" src="{{ "/assets/images/docs/ide/input-output.png" | absolute_url }}">
  
### Дублирование строк, блоков кода

Одной входной переменной явно мало, но можно её размножить клавишей `ctrl+d` (от слова duplicate).
Нажимаем несколько раз и получаем такую картину:
  <img width="165" alt="Размноженные переменные" src="{{ "/assets/images/docs/ide/vars-duplicated.png" | absolute_url }}">

Вторая и третья переменные подкрашены красным, т.к. нехорошо когда несколько переменных имеют одинаковое название. 

Редактируем имена, и меняем тип последней переменной с `input` на `output`. Для этого стираем `input` и добавляем `output`.

Среда автоматически добавляет пустые строки между разнотипными группами переменных, поэтому в итоге код будет выглядеть так:

  <img width="173" alt="Переменные объявлены" src="{{ "/assets/images/docs/ide/vars-declared.png" | absolute_url }}">

## Введение выражений

Переходим к телу ФБ, нажимаем `ctrl+пробел`, выбираем `StatementList`.
После этого можно писать код на языке ST.

Для того, чтобы получить `z := x OR y;`, нужно нажимать следующие клавиши:
  1. `z` -- появится `z;`
  1. `:=` -- появится `z := ___;`
  1. `x` -- появится `z := x;`
  1. `пробел`. Если после `x` не нажать пробел, то вместо `x OR` среда подумает, что мы имеем ввиду переменную с именем `xOR`
  1. `OR`. В итоге будет `z := x OR ___;`
  1. 'y'
  
Можно было зайти и с другого конца:
  1. `:` -- появится `___ := ___;` Дело в том, что присваивание это единственная операция с двоеточием, поэтому сразу после довоеточия среда и понимает, что нужна операция присваивания
  1. `z` -- появится `z := ___;`
  1. `tab` для перехода в правую часть
  1. `OR`, нажать `ctrl+пробел`, и выбрать в меню `or expression` -- появится `z := ___ OR ___;`
  1. Далее заполнить левый и правый аргументы операции `OR` нужными переменными

## Копирование и вставка

`ctrl+c` / `ctrl+v` работает.
Следует понимать, что среда следит за типами и смыслом выражения.
Например, не получится скопировать в буфер конструкцию `IF ... END_IF` и вставить её на место объявления переменной (смысла, конечно, в такой операции немного, но стоит понимать, что и в других местах будет подобное поведение).

С другой стороны, скопировать объявление переменной и вставить рядом с другим объявлением -- можно, в том числе и в другом блоке.

Большинство сложных конструкций копируются только целиком. Например, невозможно скопировать `IF` без закрывающего `END_IF` или без части `ELSIF` веток. Если реально нужно, то нужно скопировать целиком, а потом удалить лишнее.

## Переименование переменных, блоков

Для переименования переменных и блоков достаточно просто перейти к объявлению переменной и переименовать. При этом, все использования переменной будут автоматически исправлены.

## Перемещение строк, блоков вверх-вниз

Очень часто приходится тасовать строки, параметры и целые фрагменты кода (например, внести операцию внутрь `IF` или, наоборот, вынести из ветки).

Для этого есть горячие клавиши: `ctrl+shift+↑` и `ctrl+shift+↓`