# Bash

<!-- TOC -->
* [Bash](#bash)
    * [Rules of Thumb](#rules-of-thumb)
    * [Glossary](#glossary)
    * [Overview](#overview)
        * [commands](#commands)
    * [Theory](#theory)
        * [Outputs](#outputs)
        * [Inputs](#inputs)
        * [Functions](#functions)
            * [Definition](#definition)
            * [Call](#call)
            * [Parameters](#parameters)
                * [example](#example)
      * [Cron](#cron)
        * [Comparisons](#comparisons)
        * [Conditional statements](#conditional-statements)
            * [If](#if)
            * [Case](#case)
        * [Loops](#loops)
            * [For](#for)
            * [While](#while)
            * [Controlling loop execution](#controlling-loop-execution)
            * [Until](#until)
        * [Arithmetics](#arithmetics)
        * [Basic String Operations](#basic-string-operations)
        * [Variables](#variables)
        * [**`source`**](#source)
        * [Arrays](#arrays)
        * [**`trap`**](#trap)
        * [Pipelines (Pipes)](#pipelines-pipes)
        * [Process Substitution](#process-substitution)
      * [Parse CSV file](#parse-csv-file)
          * [Reading lines](#reading-lines)
          * [Ignoring the Header Line](#ignoring-the-header-line)
              * [using `tail` _command_ to **read** from the **second** _line_ of the
                _file_:](#using-tail-command-to-read-from-the-second-line-of-the-file)
              * [using `exec` _command_ to **change** the _standard input_ to **read** from the
                _file_](#using-exec-command-to-change-the-standard-input-to-read-from-the-file)
          * [Parsing Values From a CSV File](#parsing-values-from-a-csv-file)
              * [From the First few Columns](#from-the-first-few-columns)
              * [From Specific Columns](#from-specific-columns)
              * [From Specific Column Names](#from-specific-column-names)
          * [Mapping Columns of CSV File into Bash Arrays](#mapping-columns-of-csv-file-into-bash-arrays)
    * [Examples](#examples)
        * [n/a](#na)
        * [sourcing from files before executing commands](#sourcing-from-files-before-executing-commands)
        * [n/a](#na-1)
        * [passing of arguments](#passing-of-arguments)
        * [`$@` and `$*` are the same](#-and--are-the-same)
        * [`"$@"` vs `"$*"`](#-vs-)
        * [`$variable` vs `"$variable"`](#variable-vs-variable)
        * [arrays](#arrays-1)
        * [_if clause_](#if-clause)
        * [_loop_ with _if-clause_ and **`break`**](#loop-with-if-clause-and-break)
        * [**`case`**](#case-1)
        * [testing _filenames_](#testing-filenames)
        * [Pipes](#pipes)
        * [Process Substitution](#process-substitution-1)
      * [`awk`](#awk)
      * [`read`](#read)
      * [`tr` & `nl` translate & number lines](#tr--nl-translate--number-lines)
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

+ `^C` keybinding
+ `^D` keybinding
+ `^Z` keybinding
+ `#!` **sha-bang**
+ `#`
+ `${variable:-word}`
+ `${variable:+word}`
+ `\$foo` or `\${foo}`
+ `whoami`
+ `date`
+ `$BASH_VERSION`
+ `which <command>`
+ `<`
+ `>` `2>` `&>`
+ `|&`, `2>&1 |`
+ `>&2 echo "error message"` or `1>&2 echo ...`
+ `$0`, `$1`, `$#`, `$*`,`"$*"`, `$@`, `"$@"`[^commands]
+ `$$`
+ `$?`
+ `!`
+ `exit`
+ _command substitution_`$()`
+ `for` `in` `do` `done`
+ `while` `do` `done`
+ `until` `do` `done`
+ `break`
+ `continue`
+ `$(( $a * $b ))` or `$(($a*$b))`
+ `expr $a \* $b`
+ `let c++`or `c=$(($c + 1))`
+ `echo 'scale=50;expression' | bc`
+ `*`[^asterisk]
+ `dirname $0`[^dirname]
+ `"$variable"` vs `$variable`
+ `my_array=(a b c)`
+ ~~`my_array="a b c"`~~
+ `${my_array[@]}`
+ `${#my_array[*]}`
+ `arr=()`
+ `arr=(1 2 3)`
+ `${arr[2]}`
+ `${arr[@]}`
+ `${!arr[@]}`
+ `${#arr[@]}`
+ `arr[0]=3`
+ `arr+=(4,5)`
+ `str=$(ls)`
+ `arr=( $(ls) )`
+ `${arr[@]:s:n}`
+ `echo ${STRING:$POS:$LEN}`
+ `echo ${STRING::$LEN+1}`
+ `echo ${STRING:$POS}`
+ `echo ${STRING[@]/one/replace}`
+ `echo ${STRING[*]//all/replace}`
+ `echo ${STRING[@]//all_remove/}`
+ `echo ${STRING[*]/#beginning/replace}`
+ `echo ${STRING[@]/%end/replace}`
+ `[[ $string =~ ^s.* ]] && echo ${BASH_REMATCH}`
+ `[[ $string =~ ^s.* ]] && echo ${#BASH_REMATCH}`
+ `|`
+ _process substitution_ `<(command)`, `>(command)`
+ `IFS=","`
+ `read -r rec_column0 rec_remaining`
+ `open .` opens finder in the current location
+ `open - GUI Applaication`
+ `read`
+ `awk`
+ `tr`
+ `nl`

### commands

+
+ `trap`
+
+ `head`, `tail`
+ `wc`
+ `sort`
+ `diff`
+ `tee`
+ `tr` translate
+ `sed`
+ `awk`
+ `grep`
+ `sort`

[^dirname]: `The `dirname` _argument_ `$0` is just the **full name** of the _sourcing_ script.
[^asterisk]: X _list_ of all _files_ and _directories_ in current location
[^commands]: https://stackoverflow.com/questions/9994295/what-does-mean-in-a-shell-script

## Theory

### Outputs

> The difference between _**stdout**_ and _**stderr**_ _output_ is an essential concept as it allows
> us to a threat, that is, to **redirect** each output **separately**.

+ **`>`** redirects **_stdout_** to a **file**
+ **`2>`** redirects **_stderr_**
+ **`&>`** redirects **both**.

e.g.:

+ `2> /dev/null` to discard `stderr` output
+ `>&2 echo "error message"` or `1>&2 echo ...` to print to _stderr_

### Inputs

> Any _keystroke_ you **type** is accepted as **_stdin_** input.
> The alternative method is to accept command input from a file using **`<`** notation

+ e.g. `cat > cat.txt` redirects `stdout` to a _file_ and reads `stdin`: I can type `stdin` until
  press `Ctrl+D`
+ e.g. `cat < cat.txt` (the same as `cat cat.txt`) reads input from a _file_

### Functions

+ A function is a subroutine
+ Allow us to **organize** and **reuse** _code_

> The moment your _script_ contains two _lines_ of the **same** _code_, you may **consider** to
> **enact** a _function_ instead.

+ function _definition_ must **precede** function _call_

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

| Expression | Meaning                                                                                                                                                                                  |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `$0`       | The **_filename_** of the **current** _script_.                                                                                                                                          |
| `$n`       | The Nth _argument_ passed to script was invoked or function was called (**indexed** from **1**!).                                                                                        |
| `$#`       | The **total** number of _argument_ **passed** to _script_ or function.                                                                                                                   |
| `$@`, `$*` | **joins** all arguments by **single** _spaces_ as well and then **splits** the string as the _shell_ does, thus it **splits** an _argument_ containing _spaces_ into several _arguments_ |
| `"*"`      | **passes** over **exactly** **one** _argument_, containing all original _arguments_, separated by **single** _spaces_                                                                    |)
| `"$@"`     | **passes over all** _arguments_ in the **exact** way it had received them, i.e. as several arguments, each of them **containing** **all** the _spaces_ and other _ugliness_ they have.   |
| `$?`       | The exit status of the last command executed.                                                                                                                                            |
| `$$`       | The process ID of the current shell. For shell scripts, this is the process ID under which they are executing.                                                                           |
| `$!`       | The process number of the last background command.                                                                                                                                       |

+ `wrapped_script "$@"`
  **passes over** all _arguments_ in the **exact** way it had received them, i.e. as several
  arguments, each of them **containing** **all** the _spaces_ and other _ugliness_ they have

+ `wrapped_script "$*"`
  **passes** over **exactly** **one** _argument_, containing all original _arguments_, separated by
  **single** _spaces_

+ `wrapped_script $*` and `$@`
  **joins** all arguments by **single** _spaces_ as well and then **splits** the string as the
  _shell_ does, thus it **splits** an _argument_ containing _spaces_ into several _arguments_

##### example

`wrapper "o n e" two    three "fo   ur"`

+ `"$@"`: `wrapped "o n e" two three "fo   ur"`
+ `"$*"`: `wrapped "o n e two three fo   ur"`
    + These _spaces_ here ware **part**(^^^) of the original argument and are **not changed**.
+ `$*`, `$@`: `wrapped o n e two three fo ur`

### Cron

`crontab -l` and `crontab -e` to list and set *cronjob*s
`/var/log/syslog` _cron_ _logs_ on linux

### Comparisons

| Description           | Numeric Comparison | Example                   | Result   |
|-----------------------|--------------------|---------------------------|----------|
| Less than             | `-lt`              |                           |          |
| Greater than          | `-gt`              |                           |          |
| Equal                 | `-eq`              | `[ 100 -eq 50 ]; echo $?` | `exit 1` |
| Not equal (different) | `-ne`              |                           |          |
| Less or equal         | `-le`              |                           |          |
| Greater or equal      | `-ge`              |                           |          |

+ note the _whitespaces_ around `[]` are **obligatory** `[ ` ` ]`
+ > **Comparing** _strings_ with _integers_ using **numeric** _comparison_ _operators_ will result
  in an **error**: **`integer expression expected`**.
+ > When comparing values, you may want to use **`echo`** _command_ to **confirm** that your
  variables hold **expected** values before using them as part of the comparison operation.

| Description                   | String Comparison | Example               | Result  |
|-------------------------------|-------------------|-----------------------|---------|
| Any strings                   |                   | `[ "any" ]`,`[ any ]` | `true`  |
| Any strings                   |                   | `[ " " ]`             | `true`  |
| Empty strings                 |                   | `[ "" ]`, `[   ]`     | `false` |
| Equal strings                 | `=` or `==`       | `[ "GNU" = "UNIX" ]`  | `false` |
| Not equal (different) strings | `!=`              | `[ "GNU" != "UNIX" ]` | `true`  |
| Empty string                  | `-z`              | `[ -z "" ]`           | `true`  |
| Empty string                  | `-z`              | `[ -z "string" ]`     | `false` |
| File exists                   | `-f`              | `[ -f script.sh ]`    | `false` |
| File exists                   | `-e`              | `[ -e script.sh ]`    | `true`  |
| Directory exists              | `-d`              | `[ -d directory ]`    |         |
| File has read permission      | `-r`              | `[ -r filename ]`     |         |
| File has read permission      | `-r`              | `[ -r file ]`         |         |

+`-f` wont' find _directories_, _symlinks_, etc.
+ An **empty** _string_ or a string **consisting** of _spaces_ or an **undefined** _variable_ name,
  are evaluated as **_false_** with `-z`
+ `!` **reverts** the _logic_ e.g. `[ ! -d "/home/$1" ]`
+ `exit` _command_ causing _script_ execution **termination**
+ _whitespace_ around `=` is **required**
+ use `""` around _string_ _variables_ to **avoid** _shell_ **expansion** of _special characters_
  as `*`

### Conditional statements

The expression used by the conditional construct is evaluated to either true or false.

Logical operators

+ `&&` _AND_ (_conjunction_)
+ `||` _OR_ (_disjunction_)
+ `!` _NOT_
+ **Combined** conditional expressions should be surrounded by double brackets `[[` `]]`
  (outputs _stderr_ `exit 2` otherwise)
    + `[[ "a" && "b" ]]` returns

+ `if`
+ `then`
+ `else`

#### If

```shell
if [ condition ]; then
    action
elif [ condition ]; then
    action
else
    action
fi # don't forget!
```

#### Case

```shell
case "$variable" in
    "$condition1" )
        command...
    ;;
    "$condition2" )
        command...
    ;;
esac
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

#### Controlling loop execution

+ `continue` **skips** the rest of a particular _loop_ **iteration**
+ `break` **skips** the **entire** rest of _loop_

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

+ ~~`expr index "$STRING" "$SUBSTRING"`~~  **finds** the numerical **position** in `$STRING` of *
  *any**
  **single** _character_ in `$SUBSTRING` that **matches**. Note that the `expr` _command_ is used
  in this case.
+ `expr string : s` ~~**finds** the numerical _position_ of **any** **single** _character_
  that **matches**~~.
+ `echo ${STRING:$POS:$LEN}` extract substring of length `$LEN` from `$STRING` starting after
  position
  `$POS`. Note that first position is `0`.
    + `echo ${STRING::$index}` if `:$POS` is **omitted**  then the _substring_ starts from index `0`
    + `echo ${STRING::$index + 2}` **arithmetic** expressions can be evaluated inside _arguments_
    + ~~`echo ${STRING:$POS:}`~~ means empty (zero length) _substring_
    + `echo ${STRING:$POS}` if `:$LEN` is **omitted**, extract _substring_ **from** `$POS` to **end
      of line**
+ `[[ $string =~ ^s.* ]]; echo $?` returns `0` if **matches**, **otherwise** returns `1`
+ `[[ $string =~ ^s.* ]] && echo ${BASH_REMATCH}` outputs the **matched** _substring_
+ `[[ $string =~ ^s.* ]] && echo ${#BASH_REMATCH}` outputs the **length** of **matched** _substring_
+ `echo ${STRING[@]/one/replace}` or `echo ${STRING[*]/one/replace}` **replaces** **first**
  occurrence of _substring_ with replacement
+ `echo ${STRING[*]//all/replace}` or ... **replaces** **all** occurrences of _substring_
+ `echo ${STRING[@]//all_remove/}` or ... **deletes** **all** occurrences of _substring_ (**replaces
  ** with
+ `echo ${STRING[*]/#beginning/replace}` or ... **replaces** occurrence of substring if at the *
  *beginning** of `$STRING`
+ `echo ${STRING[@]/%end/replace}` or ... **replaces** occurrence of substring if at the **end**
  of `$STRING`
  empty string)
    + note that all the above **replace** **consecutive** _whitespaces_ with a **single** one

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

`${variable:-default}` indicates that if variable is **not defined**
then the result will be that **value**, **otherwise** the result is the `default`.
`${variable:+override}` indicates that if variable is **defined**
then `override` will be the **result**, **otherwise** the result is the **empty** string.

### Arrays

[learnshell.org](https://www.learnshell.org/)
[freecodecamp.org](https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/)
[learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/bash-introduction/0-introduction)

| Syntax                   | Result                                                                                                        |
|--------------------------|---------------------------------------------------------------------------------------------------------------|
| `arr=()`                 | Creates an **empty** _array_                                                                                  |
| `arr=(1 2 3)`            | **Initializes** _array_                                                                                       |
| `${arr[2]}`              | Retrieves **third** (_sic_) element                                                                           |
| `${arr[@]}`, `${arr[*]}` | Retrieves **all** _elements_ (**divides** them on _whitespaces_ into more arguments)                          |
| `"${arr[@]}"`            | Retrieves **all** _elements_, **exactly** as they were initialized (**doesn't** divide them on _whitespaces_) |
| `"${arr[*]}"`            | Retrieves **single** _element_ of all _elements_ separated by **single** _whitespaces_ (**single** _string_)  |
| `${!arr[@]}`             | **Retrieves** _array_ **_indices_**                                                                           |
| `${#arr[@]}`, `${#arr}`  | Calculates _array_ **_size_**                                                                                 |
| `arr[0]=3`               | **Overwrites** 1st element                                                                                    |
| `arr+=(4,5)`             | **Appends** value(s)                                                                                          |
| `str=$(ls)`              | Assigns `ls` _output_ as a **_string_**                                                                       |
| `arr=( $(ls) )`          | Assigns `ls` _output_ as an **_array_**                                                                       |
| `arr=( "$(ls)" )`        | Assigns `ls` _output_ as a single **single array element** (sic)                                              |
| `${arr[@]:s:n}`          | Retrieves **subarray** of `n` elements starting at _index_ `s`                                                |

[opensource.com (in the comments)](https://opensource.com/article/18/5/you-dont-know-bash-intro-bash-arrays)

+ an _array_ can **hold** **several** _values_ under **one** _variable_
+ an _array_ is **initialized** by assign _space_-**delimited** values **enclosed** in `()`
+ array members need **not** be _consecutive_ or _contiguous_. Some _elements_ of the _array_ can be
  **left** **uninitialized**
+ arrays are **indexed** from either `0`(_bash_) or `1`(in _zsh_) depending on the shell
+ `${my_array[@]}` or `${my_array[*]}` outputs **array** (list) of _elements_ in the _array_
  variable
+ `${#my_array[@]}` outputs the **total** **number** of _elements_ in the _array_
    + note that _curly brackets_ `{}` are **needed**
+ `${my_array[3]}` outputs the **value** of the 4th _element_
    + note that _curly brackets_ `{``}` are **needed**
+ `my_array=(a b c)`
    + ~~`my_array="a b c"`~~ is not a real _array_ despite `${my_array[@]}` works but
      `"${my_array[@]}"` doesn't)

### **`trap`**

synopsis: `trap <arg/function> <signal>`

| Signal  | Keybinding | Number | Meaning                            |
|---------|------------|--------|------------------------------------|
| SIGINT  | ^C         | 2      | **interrupt** signal               |
| SIGQUIT | ^D         | 3      | **quit** signal                    |
| SIGTERM |            | 15     |                                    |
| SIGFPE  |            | 8      | **illegal mathematical operation** |   
| ...     |            |        |                                    |

ref: `kill -l`

usage: `trap "rm -f folder; exit" 2` to **cleanup** _temporary files_:

### Pipelines (Pipes)

**_Pipeline_** `command1 | command2` is a way to **chain** _commands_ and **connect** _output_ from
one _command_ to the _input_ of the next.
By default _pipelines_ **redirects** only the standard _output_ (**_stdout_**), if you want to
include the
standard _error_ (**_stderr_**) you need to use the form `|&` which is a **shorthand** for `2>&1 |`

### Process Substitution

**_Process substitution_** allows a process’s _input_ or _output_ to be **referred** to using a
_filename_.
It has two forms: `<(cmd)` **output** form and **input** `>(cmd)`.

### Parse CSV file

[Baeldung](https://www.baeldung.com/linux/csv-parsing)

Let’s briefly review the standards defined for CSV files:

1. **Each** _record_ is on a **separate line**, **delimited** by a _line break_.
2. The **last record** in the file **may** or **may not** end with a line break.
3. There may be an **optional** **_header line_ appearing as the first line of the file
   with the **same format** as regular _record lines_.
4. Within the header and records, there may be one or more fields separated by a comma.
5. Fields containing _line breaks_, `"`, and `,`
   should be **enclosed** in _double-quotes_.
6. If _double-quotes_ are used to **enclose fields**,
   then a `"` appearing inside a field must be **escaped** by preceding it with **another double
   quote** like `""`.

#### Reading lines

```shell
#!/bin/bash
while read line
do
   echo "Record is : $line"
done < input.csv
```

`read` _command_ to **read** the _line-break_ `\n` separated records of our _CSV_ file.

#### Ignoring the Header Line

##### using `tail` _command_ to **read** from the **second** _line_ of the _file_:

`<(tail -n +2 input.csv)`

##### using `exec` _command_ to **change** the _standard input_ to **read** from the _file_

```shell
#!/bin/bash
exec < input.csv
read header
while read line; do
   echo "Record is : $line"
done 
```

#### Parsing Values From a CSV File

##### From the First few Columns

We’ll use _process substitution_ and a `tail` _command_

```shell
#! /bin/bash
while IFS="," read -r column0 remaining
do
  echo "Displaying 1st Column: $column0"
#  ...
done < <(tail -n +2 input.csv)
```

+ **_Internal (Input) Field Separator_** (**IFS**)

> Is a special _environment variable_ that **defines** the **_delimiter_**
> used for word splitting in _Unix shells_.

The **default** **value** of **_IFS_** is a _space_, _tab_, and _newline_.

+ note the `rec_remaining` that **stores** the **remaining** _fields_

##### From Specific Columns

We’ll use _process substitution_ to **pass** only **specific** _columns_ to the _while loop_ for
reading.

Print a specific character/field range of each line:
`command | cut -c|f 1|1,10|1-10|1-|-10` are the range options

Print a range of each line of a specific file:
`cut -c 1 path/to/file`

```shell
#! /bin/bash
while IFS="," read -r $column1 $column2
do
  echo "Displaying 1st Column: $column1"
done < <(cut -d "," -f1,3 input.csv | tail -n +2)
```

##### From Specific Column Names

[baeldung](https://www.baeldung.com/linux/csv-parsing#4-from-specific-column-names)

#### Mapping Columns of CSV File into Bash Arrays

`column1=( $(tail -n +2 input.csv | cut -d ',' -f1) )`
`echo "Data in first column: ${column1[@]}"`

#### Parsing CSV File Into a Bash Array

```shell
while IFS= read -r line 
do
    arr_csv+=("$line")
done < input.csv
```

#### Parsing CSV Files Having Line Breaks and Commas Within Records

> There can be several more permutations and combinations of _line-breaks_, _commas_, and _quotes_ within
> CSV files. 
> For this reason, it’s a complex task to process such CSV files with only Bash built-in
> utilities. Generally, third-party tools, like **_csvkit_**, are employed for advanced CSV parsing.

> However, another suitable alternative is _Python_’s **_CSV module_**, as Python is generally pre-installed on most Linux distributions.

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
`wrapper "o n e" two    three "fo   ur"`

+ `"$@"`: `wrapped "o n e" two three "fo   ur"`
+ `"$*"`: `wrapped "o n e two three fo   ur"`
+ `$*`, `$@`: `wrapped o n e two three fo ur`
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

##### _if clause_

```shell
if [[ $VAR_A[0] -eq 1 && ($VAR_B = "bee" || $VAR_T = "tee") ]] ; then
    command...
fi
```

##### _loop_ with _if-clause_ and **`break`**

```shell
for n in ${NUMBERS[@]}; do
    if [ $n -eq $LAST ]; then
        break
    fi
	if [ $(($n % 2)) = 0 ]; then
    	echo $n
    fi
done
```

##### **`case`**

```shell
my_case=1
case $my_case in
    1) echo "You selected bash";;
    2) echo "You selected perl";;
    3) echo "You selected phyton";;
    4) echo "You selected c++";;
    5) exit
esac
```

##### testing _filenames_

+ `-e` tests if _file_ **exist**

```shell
#!/bin/bash
filename="sample.md"
if [ -e "$filename" ]; then
echo "$filename exists as a file"
fi
```

+ `-d` tests if _directory_ **exists**

```shell
#!/bin/bash
directory_name="test_directory"
if [ -d "$directory_name" ]; then
echo "$directory_name exists as a directory"
fi
```

+ `-r` tests if file **has** **read** _permission_ for the _user_ **running** the _script/test_

```shell
#!/bin/bash
filename="sample.md"
if [ ! -f "$filename" ]; then
touch "$filename"
fi
if [ -r "$filename" ]; then
echo "you are allowed to read $filename"
else
echo "you are not allowed to read $filename"
fi
```

##### Pipes

`ls / | wc -l`
`ls / | head`
`ls / | grep <pattern>` #patterns should be quoted

##### Process Substitution

```shell
sort file1 > sorted_file1
sort file2 > sorted_file2
diff sorted_file1 sorted_file2
```

with process substitution turns into

```shell
diff <(sort file1) <(sort file2)
```

##### `awk`

**Print** the **last column** of each line in a file, **using** a _comma_ (instead of _space_) as a
field
separator:

```shell
    awk -F ',' '{print $NF}' path/to/file
```

##### `read`

```shell
while read line; do
    echo "$line"
done < "$filename"
```

```shell
read "Custom prompt: " my_veriable
```

**Prompts** the user to enter the name of the column they want to extract.
The entered value is stored in the variable

##### `tr` & `nl` translate & number lines

```shell
loc_col_b=$(head -1 input.csv | tr ',' '\n' | nl | grep -w "$col_b" | tr -d " " | awk -F " " '{print $1}
```

+ `tr ',' '\n'`: **translates** each _comma_ in the header to a _newline character_, effectively
  **converting** the CSV _header_ into a **_list_ of column names**.
+ `nl`: **Numbers** each _line_.
+ `grep -w "$match"`: **searches** for the **exact match**.
+ `tr -d " "`: **deletes** any spaces in the output.
+ `awk -F " " '{print $1}'`: **prints** the first field
  taking the _whitespace_ as the field separator. This effectively extracts the line number where
  the specified column name is found.

##### `readarray`

For _Bash_ versions **4+**, we can also populate the array using the `readarray` _command_:

`readarray -t my_array < input.csv`

The -t option will remove the trailing newlines from each line.