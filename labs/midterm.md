---
layout: default
title: Midterm Review
nav_order: 8
---
# Status: <font color="orange">In-Progress</font> | <font color="blue">Recently Updated</font>
{: .no_toc }

#### Status Rubric
{: .no_toc }

<style type="text/css">
.tg  {border-collapse:collapse;border-color:#ccc;border-spacing:0;}
.tg td{background-color:#fff;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{background-color:#f0f0f0;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-baqh{text-align:center;vertical-align:top}
.tg .tg-amwm{font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-0lax{text-align:left;vertical-align:top}
</style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-baqh"><span style="font-weight:bold;color:#036400">Completed</span></th>
    <th class="tg-amwm"><span style="color:#F8A102">In-Progress</span></th>
    <th class="tg-amwm"><span style="color:#3166FF">Recently Updated</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-0lax">All information is up-to-date and has compiled on hydra/tesla</td>
    <td class="tg-0lax">Information is subject to change and has not been tested on hydra/tesla</td>
    <td class="tg-0lax">Information has been recently updated, so you should read the changelog below in case it concerns something you've already completed.</td>
  </tr>
</tbody>
</table>

<details>
<summary>
<b><font color="maroon">click to view change-log</font></b>
</summary>

  <div markdown="1">

  - added completed [Vim](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#vim) section
  - added completed [Unix/Linux](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#unixlinux) section
  - added *mostly* completed [C++ Input/Output](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#c++-inputoutput) section<br>
  `Fri, 04 Mar 2022 00:49:19 EST`

  </div>
</details>
<hr>

# Midterm Review
All topics discussed in detail are referenced by the official study guide for the exam. [Refer to this study guide Camille created](../assets/../midterm.pdf) for a reference to the midterm material.

## Table of contents
{: .no_toc }
- TOC
{:toc}

## Vim

- ### What is Vim?

  Vim is a simple screen-based text editor on the surface, but in actuality it is a powerful tool for programmers looking to maximize their workflow. It's especially popular because it is ubiquitous on pretty much every **Unix** system out there, which means anyone experienced with Vim can be a productive developer no matter the environment. It stands for **Vi IMproved**.

- ### Different Vim modes

  There are many different modes in Vim, but the only ones you should know about at this point are **normal mode** & **insert mode**. You can enter these modes from within one another by using `ESC` and `i` respectively.

- ### How to open a file

  To open a file with Vim from the command line, you can use `vim [file here]` or (only on the lab machines) `vi [file here]`. "Vim" and "Vi" are two separate programs, but on Hydra & Tesla, the `vi` command is automatically converted to `vim`, so it'll open a file with Vim regardless of the command used. This is not the default behavior though.

- ### How to save & exit

  To save and exit while inside of Vim, use the command `:wq`. Alternatively, you can enter `ZZ` as well. To exit without saving, you can use `:q` or sometimes `:q!` if you've made changes to the file and still wish to exit without saving.

## Unix/Linux

- ### What is it?

  - `Unix`: the broader family of operating systems including any Linux architecture & MacOSX, as well as others.
  - `Linux`: a subset of the `Unix` family, and the quintissential operating system of choice for programmers and systems around the world.

## C++ Input/Output

- ### C++ Syntax for reading input from the terminal & output using cin/cout
  
   - `#include <iostream>` - Includes the IO library for `stdin/stdout` which allows us to utilize functions like `cout` & `cin`.

   - `using namespce std` - allows us to remove the `std::` prefix from `<iostream>` library methods and fields. 

   - `cin/cout` - the input/output stream operators, respectively. `cin` allows us to receive input from `stdin` i.e. input from console, and `cout` allows us to "print" output to `stdout` i.e. the console screen. `cin` reads **one word at a time**, where a word is any value until (and not including) a whitespace character is encountered.

  - `cin.eof()` - returns a `bool` indicating true if the end of `stdin` has been reached i.e. there is no more input to be read, stands for **end-of-file**.
    ```c++
    while (cin >> dest) {
      if (cin.eof()) {
        // we've reached the end of input, so break
        break;
      }
    }
    ```
  
  - `cin.clear()` - clears the error-state of `cin` which is "set" i.e. raised when `cin` reads in "bad-input" e.g. reading a `string` into an `int` variable.
    ```c++
    int dest;
    while (cin >> dest) {
      /* if we read in something like "hello", then the error flag is raised for cin
      because "hello" is not an int but we're attempting to read it as such */
      if (!cin.good()) {
        // so clear the state and continue reading
        cin.clear();
      }
    }
    ```
  - `cin.ignore(size, delimiter)` - "ignores" (I like to say "erases") whatever is in the "buffer" that's between `cin` & the variable you're reading into. Its arguments `size` and `delimiter` are the amount of characters to be ignored if inside the buffer and the character that signals to stop ignoring once encountered, respectively. So `cin.ignore(size, delimiter)` will stop ignoring either when it reaches whatever number `size` is, or when it encounteres the `char` passed to `delimiter` (usually `'\n'`).<br><br>If you were to read bad input, then that input gets left in the buffer instead of going into the intended variable. If it's left inside the buffer, then it would cause issues for future reading because `cin` will start reading from where it left off from inside the buffer, and in this case would be corrupted because of the bad input left over. If either of these passages confuse you, consider reading this [little post I made](https://discord.com/channels/935991929978621962/935991930582630404/941465195572760576) to explain the concept a bit more in depth. Note that the following example uses `#include <limits>` because the `size` argument passed is `std::numeric_limits<streamsize>::max()`
    ```c++
    int value;
    while (cin >> value) {
      /* if we read in something like "hello", then the error flag is raised for cin
      because "hello" is not an int but we're attempting to read it as such */
      if (!cin.good()) {
        // so clear the state and continue reading
        cin.clear();
        /* erase the input that was placed in the buffer to restore future reading operations
        which is "hello" in this case */
        
        // notice we use numeric_limits which is from the <limits> library
        cin.ignore(std::numeric_limits<streamsize>::max(), '\n');
      }
    }
    ```
    You'll notice we use `cin.clear()` & `cin.ignore()` in tandem, which is typically the case.<br><br>

  - `cin.get(dest)` - reads a `char` from `stdin` into whatever variable is specified by `dest`, which should always be a `char` since that is what `cin.get()` reads specifically. There are two big differences between `cin.get()` and `cin >>`. The obvious one is that `cin.get()` only reads a single `char`, whereas `cin >>` reads by starting at the first non-whitespace character and then ending when it encounters a whitespace character (basically it reads a word, by default).  The not so obvious difference is that `cin.get()` also reads the `\n` character, which is the newline character. Any time you enter something into the console and read it in, the computer interprets it as `[input]\n`. There is always an implicit `\n` at the end of any input.
    ```c++
    char dest;
    while (cin.get(dest)) {
      // read & print every char while possible
      cout << dest;
    }
    ```

   - wrapping `cin` in if-statement - `cin` is able to return true or false based on whether or not it successfully read a value from input.
      ```c++
      int dest;
      /* reads dest while it can. If "bad" input was read or EOF was 
        encountered, it should fail */
      while (cin >> dest) {
        cout << dest;
      }
      ```

  - `printf(string, formating_args...)` - I'm not sure if you need to know what `printf()` is exactly, but it is essentially the `C` analog to `cout <<`. `string` is the string to be printed, and `formatting_args` are the format specifiers to be passed to the string. It's a bit similar to how you would print with Java from 101.
      ```c++
      /* prints "I am 21 years old" using printf. */
      printf("I am %d years old", 21);
      ```
    At the very least, be familiar with the following format specifiers (very similar to the ones in 101)

        %d - integer specifier
        %f - floating point specifier
        %s - string specifier
        %c - char specifier

  - `iomanip` - You can refer to the [iomanip](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#iomanip) section of the lab 5 writeup for all of this.
  
### Vectors
Coming soon...