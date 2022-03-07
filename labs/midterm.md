---
layout: default
title: Midterm Review
nav_order: 8
---
# Status: <font color="orange">In-Progress</font>
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
    <td class="tg-0lax">Information has been recently updated, so you should read the changelog below in case it concerns something you've already completed</td>
  </tr>
</tbody>
</table>

<details>
<summary>
<b><font color="maroon">click to view change-log</font></b>
</summary>

  <div markdown="1">

`Fri, 04 Mar 2022 00:49:19 EST`
  - added completed [Vim](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#vim) section
  - added completed [Unix/Linux](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#unixlinux) section
  - added *mostly* completed [C++ Input/Output](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#c++-inputoutput) section
  - added incomplete [Vectors](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#vectors) section

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
    
    <pre><code class="language-cpp">while (cin >> dest) {
      if (cin.eof()) {
        // we've reached the end of input, so break
        break;
      }
    }</code></pre>
  
  - `cin.clear()` - clears the error-state of `cin` which is "set" i.e. raised when `cin` reads in "bad-input" e.g. reading a `string` into an `int` variable.
    
    <pre><code class="language-cpp">int dest;
    while (cin >> dest) {
      /* if we read in something like "hello", then the error flag is raised for cin
      because "hello" is not an int but we're attempting to read it as such */
      if (!cin.good()) {
        // so clear the state and continue reading
        cin.clear();
      }
    }</code></pre>

  - `cin.ignore(size, delimiter)` - "ignores" (I like to say "erases") whatever is in the "buffer" that's between `cin` & the variable you're reading into. Its arguments `size` and `delimiter` are the amount of characters to be ignored if inside the buffer and the character that signals to stop ignoring once encountered, respectively. So `cin.ignore(size, delimiter)` will stop ignoring either when it reaches whatever number `size` is, or when it encounteres the `char` passed to `delimiter` (usually `'\n'`).<br><br>If you were to read bad input, then that input gets left in the buffer instead of going into the intended variable. If it's left inside the buffer, then it would cause issues for future reading because `cin` will start reading from where it left off from inside the buffer, and in this case would be corrupted because of the bad input left over. If either of these passages confuse you, consider reading this [little post I made](https://discord.com/channels/935991929978621962/935991930582630404/941465195572760576) to explain the concept a bit more in depth. Note that the following example uses `#include <limits>` because the `size` argument passed is `std::numeric_limits<streamsize>::max()`
    
    <pre><code class="language-cpp">int value;
    while (cin >> value) {
      /* if we read in something like "hello", then the error flag is raised for cin
      because "hello" is not an int but we're attempting to read it as such */
      if (!cin.good()) {
        // so clear the state and continue reading
        cin.clear();

        // erase the input that was placed in the buffer to restore future reading operations
        // which is "hello" in this case
        
        // notice we use numeric_limits which is from the &lt;limits&gt; library
        cin.ignore(std::numeric_limits&lt;streamsize&gt;::max(), '\n');
      }</code></pre>

  - `cin.get(dest)` - reads a `char` from `stdin` into whatever variable is specified by `dest`, which should always be a `char` since that is what `cin.get()` reads specifically. There are two big differences between `cin.get()` and `cin >>`. The obvious one is that `cin.get()` only reads a single `char`, whereas `cin >>` reads by starting at the first non-whitespace character and then ending when it encounters a whitespace character (basically it reads a word, by default).  The not so obvious difference is that `cin.get()` also reads the `\n` character, which is the newline character. Any time you enter something into the console and read it in, the computer interprets it as `[input]\n`. There is always an implicit `\n` at the end of any input.
    
    <pre><code class="language-cpp">char dest;
    while (cin.get(dest)) {
      // read & print every char while possible
      cout << dest;
    }</code></pre>

   - wrapping `cin` in if-statement - `cin` is able to return true or false based on whether or not it successfully read a value from input.
      
      <pre><code class="language-cpp">int dest;
      /* reads dest while it can. If "bad" input was read or EOF was 
        encountered, it should fail */
      while (cin >> dest) {
        cout << dest;
      }</code></pre>

  - `printf(string, formating_args...)` - I'm not sure if you need to know what `printf()` is exactly, but it is essentially the `C` analog to `cout <<`. `string` is the string to be printed, and `formatting_args` are the format specifiers to be passed to the string. It's a bit similar to how you would print with Java from 101.
      
      <pre><code class="language-c++">/* prints "I am 21 years old" using printf. */
      printf("I am %d years old", 21);</code></pre>

    At the very least, be familiar with the following format specifiers (very similar to the ones in 101)
        
      <pre><code class="language-plaintext">%d - integer specifier
    %f - floating point specifier
    %s - string specifier
    %c - char specifier</code></pre>

  - `iomanip` - You can refer to the [iomanip](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#iomanip) section of the lab 5 writeup for all of this.
  
## Vectors

- ### What are vectors? What is it similar to in Java?

  Vectors exist in Java as well as `Vector`, but they are a bit different behind the scenes compared to `std::vector`. The Java-equivalent you're familiar with is the `ArrayList`, which is almost identical in behavior and function. The *underyling* data structure for `std::vector` is an array, which is something you should all be familiar with. The difference between an array and a `vector` is that arrays are fixed in size, whereas `vectors` can change their size accordingly. That's more or less it. `vectors` provide methods like `push_back()`, `insert()`, `size()`, and more to make your life easier than if you were to use an array. **Just know that once you create an array, you cannot modify its size directly. You can modifiy its contents, but not the bounds of those contents. However with a `vector`, you can modify both the size and the contents whenever you want.**

- ## Syntax for declaring vectors
  To declare a `vector`, use the following syntax

  <pre><code class="language-c++">#include&lt;vector&gt;
  using namespace std;

  int main() {
    vector&lt;int&gt; v; // declare empty vectory
    vector&lt;int&gt; v2 = {1, 2, 3, 4}; // declare vector of 4 elements [1,2,3,4]
    vector&lt;int&gt; v3(10); // declare vector of 10 empty elements
    vector&lt;int&gt; v4(10, 3); // declare vector of 10 elements, all set to 3
  }</code></pre>

- ## `push_back()`, `size()`, `clear()`, `resize()`
  
  Like mentioned previously, `vector` provides many methods that make your life much easier, including but not limited to `push_back()`, `size()`, `clear()`, and `resize()`.

    - `push_back(element)` - appends an element to the end of a `vector`. The `vector` will automatically resize itself if appending the element would go past its boundaries. This is the most common way of inserting elements into your vector.
    - `size()` - returns the size of the vector it was called on. So a `vector` of 10 elements would be `v.size() == 10`.
    - `clear()` - erases all elements in the vector it was called on, reducing its size to 0. (note this does not reduce the *capacity* to 0)
    - `resize(n)` - resizes a `vector`'s capacity to fit `n` elements. So `v.resize(10)` will resize `v` to hold 10 total elements. If you resize an array that already contains elements, it will cut off any elements necessary to resize (if shrinking) or it will expand normally if no cutting is necessary. e.g. a vector of `{1, 2, 3}` with `resize(2)` will be shrunk to `{1, 2}`.

- ## What to #include
  As shown in the first example, you'll need to use `#include<vector>` if you want to have access to the `vector` library.

- ## Vectors of vectors
  Conceptually, vectors of vectors (I'm gonna call them 2D `vectors` from here on out) are basically just a "table" Like a multiplication table, or even a graph. You can think of it as a 2-dimensional grid that can be indexed the same way you would refer to a coordinate in math. So if you have a 2D `vector`, each with 10 elements, then if you wanted the last element in the table, you'd access it like `v[9][9]`. The mathemtical equivalent would be `(9, 9)`, where the layout is `(x, y)`. If you wanted the element on the 2nd row, first column, you'd access it like `v[1][0]`. It goes `[ROWS][COLS]`. To declare a 2D `vector`, you do

  <pre><code class="language-c++">#include&lt;vector&gt;
  using namespace std;

  int main() {

    // declare 2d vector
    
    vector&lt;vector&lt;int&gt;&gt; v;

    // initialize 2d vector

    v.resize(10); // create 10 rows
    for (int i = 0; i &lt; v.size(); i++) {
      v[i].resize(10, 0); // add 10 columns to each row
    }
  }</code></pre>