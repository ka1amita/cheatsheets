# Bash

<!-- TOC -->

* [Bash](#bash)
    * [Rules of Thumb](#rules-of-thumb)
    * [Glossary](#glossary)
    * [Overview](#overview)
    * [Theory](#theory)
        * [Outputs](#outputs)
        * [Inputs](#inputs)
        * [Functions](#functions)
            * [Definition](#definition)
            * [Call](#call)
            * [Parameters](#parameters)
        * [Cron](#cron)
            * [example](#example)
        * [Comparisons](#comparisons)
        * [Conditional statements](#conditional-statements)
        * [Loops](#loops)
            * [For](#for)
            * [While](#while)
            * [Until](#until)
        * [Arithmetics](#arithmetics)
        * [Variables](#variables)
        * [**`source`**](#source)
    * [Examples](#examples)
        * [n/a](#na)
        * [sourcing from files before executing commands](#sourcing-from-files-before-executing-commands)
        * [n/a](#na-1)
        * [passing of arguments](#passing-of-arguments)
        * [`$@` and `$*` are the same](#-and--are-the-same)
        * [`"$@"` vs `"$*"`](#-vs-)
        * [`$variable` vs `"$variable"`](#variable-vs-variable)

<!-- TOC -->

## Rules of Thumb

+ always **append** `/` after _directories_ to avoid ambiguity (e.g. with `find`, `mv`)

## Glossary

+ **_shell_** is a **_macro_** **_processor_** (_command_ language _**interpreter**_). It allows for
  an **interactive** or **non-interactive**
  _command_ **execution**.
+ **_Bourne-Again SHell_** (**Bash**) is a _shell_. It is widely available on various _operating
  systems_ and is a default command interpreter on most _GNU_/_Linux_ systems.
+ **_scripting_** allows for an **automatic** _commands_ **execution** that would otherwise
  be executed _interactively_ one-by-one.
+ **_shell script_** is a _text_ _file_, ...
+ variable **substitution** - assigning a value of a command output to a variable

+ `$?` returns last **exit code**
+ `set -x` sets the script to **stdout** (print) **each line** of script as it is **executed**; for
  **debugging**
+ `set -e` sets the script **exits** **immediately** after a single command **failure**

## Overview

+ `#!` **sha-bang**
+ `#`
+ `${variable:-word}`
+ `${variable:+word}`
+ `\$foo` or `\${foo}`
+ `whoami`
+ `date`
+ `$BASH_VERSION`
+ `which <command>`
+ `>` `2>` `&>`
+ `<`
+ `$0`, `$1`, `$#`, `$*`,`"$*"`, `$@`, `"$@"`[^commands]
+ `$$`
+ `$?`
+ `!`
+ `exit`
+ _command substitution_
+ `for` `in` `do` `done`
+ `while` `do` `done`
+ `until` `do` `done`
+ `$(( $a * $b ))`
+ `expr $a \* $b`
+ `let c++`
+ `echo 'scale=50;expression' | bc`
+ `*`[^asterisk]
+ `dirname $0`[^dirname]
+ `"$variable"` vs `$variable`
+ `${#my_array[@]}`

[^dirname]: `The `dirname` _argument_ `$0` is just the **full name** of the _sourcing_ script.
[^asterisk]: X _list_ of all _files_ and _directories_ in current location
[^commands]: https://stackoverflow.com/questions/9994295/what-does-mean-in-a-shell-script

## Theory

### Outputs

> The difference between _**stdout**_ and _**stderr**_ _output_ is an essential concept as it allows
> us to a threat, that is, to **redirect** each output **separately**. The **`>`** notation is used
> to redirect **_stdout_** to a **file** whereas **`2>`** notation is used to redirect **_stderr_**
> and **`&>`** is used to redirect **both**.

+ e.g. `2> /dev/null` to discard `stderr` output

### Inputs

> Any _keystroke_ you **type** is accepted as **_stdin_** input.
> The alternative method is to accept command input from a file using **`<`** notation

+ e.g. `cat > cat.txt` redirects `stdout` to a _file_ and reads `stdin`: I can type `stdin` until
  press `Ctrl+D`
+ e.g. `cat < cat.txt` (the same as `cat cat.txt`) reads input from a _file_

### Functions

> Allow us to **organize** and **reuse** _code_
> The moment your _script_ contains two _lines_ of the **same** _code_, you may **consider** to
> **enact** a _function_ instead.

> function _definition_ must **precede** function _call_

#### Definition

```shell
function my_function {
  # function body inside braces
}
```

#### Call

```
$ my_function # simple name is enough as a function call (just like any command)
```

#### Parameters

[//]: # (`$0` returns TODO)

+ `$1`...`$N` **returns** the **Nth** _argument_ (**indexed** from **1**!)
+ `$#` returns the total number of arguments
+ `$*`, `$@` returns the **_list_** (or a **_space_** **delimited** _string_) of all _arguments_
+ `"*"` see below
+ `"$@"` see below

+ `wrapped_script "$@"`

> this is correct and will hand over all arguments in the way we received them, i. e. as several
> arguments, each of them containing all the spaces and other uglinesses they have.

+ `wrapped_script "$*"`

> this will hand over exactly one argument, containing all original arguments, separated by single
> spaces.

+ `wrapped_script $*`

> this will join all arguments by single spaces as well and will then split the string as the shell
> does on the command line, thus it will split an argument containing spaces into several arguments.

### Cron

`crontab -l` and `crontab -e` to list and set *cronjob*s
`/var/log/syslog` _cron_ _logs_ on linux

##### example

`wrapper "one two    three" four five "six seven"`

+ `"$@"`: `wrapped "one two    three" four five "six seven"`
+ `"$*"`: `wrapped "one two    three four five six seven"`
    + These _spaces_ here are^^^^ **part** of the first argument and are **not changed**.
+ `$*`:   `wrapped one two three four five six seven`

### Comparisons

| Description              | Numeric Comparison        | String Comparison             |
|--------------------------|---------------------------|-------------------------------|
| Less than                | `-lt`                     | N/A                           |
| Greater than             | `-gt`                     | N/A                           |
| Equal                    | `-eq`                     | `=`                           |
| Not equal                | `-ne`                     | `!=`                          |
| Less or equal            | `-le`                     | N/A                           |
| Greater or equal         | `-ge`                     | N/A                           |
| Shell comparison example | `[ 100 -eq 50 ]; echo $?` | `[ "GNU" = "UNIX" ]; echo $?` |

+ note the _whitespaces_ around `[]` are **obligatory** `[ ` ` ]`
+ > **Comparing** _strings_ with _integers_ using **numeric** _comparison_ _operators_ will result
  in an **error**: **`integer expression expected`**.
+ > When comparing values, you may want to use **`echo`** _command_ to **confirm** that your
  variables hold **expected** values before using them as part of the comparison operation.

| Description      | Numeric Comparison | Example             | Result   |
|------------------|--------------------|---------------------|----------|
| Empty string     | `-z`               | `[ -z "" ]`         | `exit 0` |
| Empty string     | `-z`               | `[ -z "string" ]`   | `exit 1` |
| File string      | `-f`               | `[ -f file ]`       |          |
| Directory string | `-d`               | `[ -d directory/ ]` |          |

`!` **reverts** the _logic_ e.g. `[ ! -d "/home/$1" ]`
`exit` _command_ causing _script_ execution **termination**

### Conditional statements

`if` `then` `else`

```shell
if [ condition ]; then
    action
elif [ condition ]; then
    action
else
    action
fi # don't forget!
```

### Loops

#### For

```shell
for i in 1 2 3; do
    action 
done
```

#### While

```shell
counter=-5
while [ true ]; do
    action
    let counter+=1 # note the `let` keyword!
done
```

_action_ is **executed** only **when** the _condition_ is `true`

#### Until

```shell
counter=5
until [ false ]; do
    action
    let counter-=1
done
```

_action_ is **executed** **until** the _condition_ **becomes** `true` (i.e. only when `false` and *
*not** when finally `true`)

`for i in $(ls); do echo $i has $(echo -n $i | wc -c); done` has only one whitespace
`for i in $(ls); do echo "$i has $(echo -n $i | wc -c)"; done` has more whitespaces for some reason!

### Arithmetics

| Operator | Operation      |
|----------|----------------|
| `+`      | plus           |
| `-`      | minus          |
| `*`      | multiplication |
| `/`      | division       |
| `%`      | modulo         |
| `**`     | exponentiation |

+ ~~`$(( 1 * 0.5 ))`~~
+ `echo $((1* 0.5 ))`
+ ~~`expr 1 \* 0.5`~~
+ `expr 1 \* 1` **evaluates** a mathematical expression and **prints** the result to _stdout_, but
  note
  that `*` must be escaped
+ `let c=$a+($b*$c)` or `let c++` **evaluates** a mathematical expression and **assigns** the result
  to a variable
+ `echo 'scale=30;sqrt(2)' | bc` allows to use decimal numbers with big number of significant digits

### Basic String Operations

+ `expr index "$STRING" "$SUBSTRING"`  **finds** the numerical **position** in `$STRING` of **any** 
  **single** _character_ in `$SUBSTRING` that **matches**. Note that the `expr` _command_ is used 
  in this case.
+ `echo ${STRING:$POS:$LEN}` extract substring of length `$LEN` from `$STRING` starting after position 
  `$POS`. Note that first position is `0`.
  + If `:$LEN` is **omitted**, extract substring **from** `$POS` to **end of line**
+ `expr string : s` **finds** the numerical _position_ in `$STRING` of **any** **single** _character_ in 
  `$SUBSTRING` that **matches**.
+ `[[ $stringZ =~ $re ]] && echo ${#BASH_REMATCH}`
+ `echo STRING[@]/one/replace` **replaces** **first** occurrence of _substring_ with replacement
+ `echo ${STRING[@]//all/replace}` **replaces** **all** occurrences of _substring_
+ `echo ${STRING[@]// all_remove/}` **deletes** **all** occurrences of _substring_ (**replaces** with
+ `echo ${STRING[@]/#beginning/replace}` **replaces** occurrence of substring if at the **beginning** of `$STRING`
+ `echo ${STRING[@]/%end/replace}` **replaces** occurrence of substring if at the **end** of `$STRING`
empty string)
+ 

### Variables

+ _Variable_ names should **start** with a _letter_ or an _underscore_ (`_`).
+ _Variable_ names can **contain** _letters_, _numbers_, and _underscores_ (`_`).
+ _Variable_ names are **case-sensitive**.
+ _Variable_ names should **not contain** _spaces_ or _special characters_.
+ Use **descriptive** _names_ that reflect the _purpose_ of the variable.
+ **Avoid** using _reserved_ _keywords_, such as `if`, `then`, `else`, `fi`, `do`, `done`, and so on
  as variable
  names.
+ **Never** name your _private_ _variables_ using `UPPERCASE` characters. This is because uppercase
  variable names are **reserved** for _internal_ _shell_ _variables_, and you run a risk of
  overwriting them.
+ _Variables_ can be **assigned** with the value of a _command_ **output**. This is referred to as  
  **_substitution_**. Substitution can be done by **encapsulating** the command with ``` `` ```
  or `$()`
    + Note that when the _script_ runs, it will **run** the command **inside** the `$()`and
      **capture** its _output_.

### **`source`**

[Baeldung](https://www.baeldung.com/linux/source-include-files#:~:text=The%20source%20Command,or%20inside%20a%20bash%20script.)

+ When we execute the script using the **`source`** command, it is run in the **same** _shell_
  where we source it
    + **all** _definitions_ from the sourced file become **available** in the **parent** _shell_.
+ On the other hand, when we run a script with the **`bash`** command or just by **typing** its
  name, a **new** _shell_ instance is created.
    + Therefore, the script can **access** only variables and functions defined **with**
      **`export`** _keyword_ in the _parent_ _shell_. In addition, all definitions of the
      **descendant** _shell_ **disappear** when it **exits**[^1].
+ As a matter of fact, only **_Bourne again shell_** (_Bash_) provides this command in **this form
  **. Debian Almquist Shell (Dash) or Bourne Shell (sh), to name only a few, offer the **`.`**
  command (**_dot_**) instead. Of course, _dot_ is present in _Bash_, too.
    + scripts that use _dot_ are **portable** and run with any [_POSIX_][posix] **compliant** bash
      language implementation.
    + the presented description of the command concerns _Bash_ in a **non**-_POSIX_ mode

[^1]: therefore one doesn't need `export` nor `set -a` in `source .env && exec java app.jar`
or `VAR=val && exec java app.jar`

+ Significant properties
    + it **searches** for the script in folders listed in the `PATH` variable of the current shell
    + it **recognizes** an _absolute_ or _relative_ path given to the script
    + the script does **not need** to be _executable_
    + it **returns** the _status_ of the **last** executed _command_ in the script
    + if **sourced** _script_ **issues** the `exit` command, the **sourcing** one **exits** **too**!

+ > However, _command_ `source my_lib` inside `my_script` tries to **find** the `my_lib` script in
  the **directory** where `my_script` is **invoked** from. One can solve it by one of these ways:
    + use `source $(dirname "$0")/my_lib` instead
    + move it `my_lib` to e.g. `/Users/my_user/bin` _directory_ that is in the `PATH`. The
      simple `source my_lib` now works correctly again!

+ > In fact, **inside** another _script_, `source` **passes** the _arguments_ of the _wrapping_
  script **to** the _sourced_ one, **unless** the latter’s _arguments_ are explicitly **given**.

[posix]: https://en.wikipedia.org/wiki/POSIX

`${variable:-default}` indicates that if variable is **set** then the result will be that **value**,
**otherwise** the
result is the `default`.
`${variable:+override}` indicates that if variable is **set** then `override` will be the **result
**, **otherwise** the
result is the **empty** string.

### Arrays

+ An _array_ can **hold** **several** _values_ under **one** _variable_.
+ An _array_ is **initialized** by assign _space_-**delimited** values **enclosed** in `()`
+ Array members need **not** be _consecutive_ or _contiguous_. Some _elements_ of the _array_ can be
  **left** **uninitialized**.
+ `${#my_array[@]}` outputs **list** of _elements_ in the _array_
+ `${#my_array[@]}` outputs the **total** **number** of _elements_ in the _array_
    + note that _curly brackets_ `{}` are **needed**
+ `${my_array[3]}` outputs the **value** of the 4th _element_
    + note that _curly brackets_ `{``}` are **needed**
+ arrays are **indexed** from `0`
+

## Examples

##### n/a

```shell
#in test_var:
#!/bin/bash
echo "expVar = $expVar"
echo "myVar = $myVar"

export expVar="Exported variable"
myVar="My variable"

./test_var
#> expVar=Exported variable
#> myVar =

source test_var
#> expVar=Exported
#> variable myVar = My variable
```

##### sourcing from files before executing commands

```shell
#!/bin/bash
source /etc/profile
source ~/.bash_profile
source ~/.bashrc
# do something useful
```

##### n/a

```shell
source $(dirname "$0")/my_lib
```

The `dirname` _argument_ `$0` is just the *filepath* of the _sourcing_ script **without** it's
_filename_.

##### passing of arguments

+ When arguments given when sourcing the inner script:

```shell
# in my_args:
#!/bin/bash
echo "My argument(s): $@"

# in my_args_wrapper:
#!/bin/bash
source my_args foo

./my_args_wrapper bar
#> My argument(s): foo
```

+ But when no arguments given when sourcing the inner script:

```shell
# in my_args_wrapper:
#!/bin/bash
source arg_list

./my_args_wrapper bar
#> My argument(s): bar
```

```shell
for directory in $*; do
    backup $directory 
done;
```

##### `$@` and `$*` are the same

```shell
# in at_and_asterisk:
#!/bin/bash
echo $@` # or `echo $*
```

```shell
script "a b" c d
#> a b c d

# in at_and_asterisk:
for i in $* # or `echo $@;
do
  echo $i
done
#> a
#> b
#> c
#> d
```

##### `"$@"` vs `"$*"`

```shell
wrapper "one two    three" four five "six seven"

wrapped "$@"
#> wrapped "one two    three" four five "six seven"
wrapped "$*"
#> wrapped "one two    three four five six seven" 
# The _spaces_ are **part** of the first argument and are **not changed**.
wrapped $*:
#> wrapped one two three four five six seven
```

##### `$variable` vs `"$variable"`

```shell
greeting='Hello   world!'
echo $greeting" now with spaces: $greeting"
#> Hello world! now with spaces: Hello   world!
```

##### arrays

```shell
echo ${#my_array[@]}
```

```shell
i=5
echo ${my_array[$i]}
```

```shell
echo ${NUMBERS[@]}
```