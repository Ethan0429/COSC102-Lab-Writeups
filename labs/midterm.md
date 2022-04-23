---
layout: default
title: Midterm Review
nav_order: 9
---
# Status: <font color="green">Completed</font>
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

`Mon, 07 Mar 2022 18:21:49 EST`
  - added completed [String Streams](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#string-streams) section<br><br>

`Mon, 07 Mar 2022 11:54:02 EST`
  - added completed [Argv/Argc](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#argvargc) section<br><br>

`Mon, 07 Mar 2022 11:16:33 EST`
  - added completed [File Streams](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#file-streams) section<br><br>

`Mon, 07 Mar 2022 10:54:13 EST`
  - added completed [Vectors](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#vectors) section<br><br>

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

- ### Syntax for declaring vectors
  To declare a `vector`, use the following syntax

  <pre><code class="language-c++">#include&lt;vector&gt;
  using namespace std;

  int main() {
    vector&lt;int&gt; v; // declare empty vectory
    vector&lt;int&gt; v2 = {1, 2, 3, 4}; // declare vector of 4 elements [1,2,3,4]
    vector&lt;int&gt; v3(10); // declare vector of 10 empty elements
    vector&lt;int&gt; v4(10, 3); // declare vector of 10 elements, all set to 3
  }</code></pre>

- ### push_back(), size(), clear(), resize()
  
  Like mentioned previously, `vector` provides many methods that make your life much easier, including but not limited to `push_back()`, `size()`, `clear()`, and `resize()`.

    - `push_back(element)` - appends an element to the end of a `vector`. The `vector` will automatically resize itself if appending the element would go past its boundaries. This is the most common way of inserting elements into your vector.
    - `size()` - returns the size of the vector it was called on. So a `vector` of 10 elements would be `v.size() == 10`.
    - `clear()` - erases all elements in the vector it was called on, reducing its size to 0. (note this does not reduce the *capacity* to 0)
    - `resize(n)` - resizes a `vector`'s capacity to fit `n` elements. So `v.resize(10)` will resize `v` to hold 10 total elements. If you resize an array that already contains elements, it will cut off any elements necessary to resize (if shrinking) or it will expand normally if no cutting is necessary. e.g. a vector of `{1, 2, 3}` with `resize(2)` will be shrunk to `{1, 2}`.

- ### What to #include
  As shown in the first example, you'll need to use `#include<vector>` if you want to have access to the `vector` library.

- ### Vectors of vectors
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

## File Streams
I've covered file streams pretty extensively in the [Lab 5 writeup](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html), so I'll cover it briefly here since you can just refer to the notes there for more detail if needed

- ### What are file streams?
  Similar to `cout`/`cin`, a file stream is a way to interpret a file e.g. `ticket.txt` as a stream, which will allow you to process it for input or output.

- ### How to write to and from files using ifstream & ofstream
  Again, exactly the same as `cin`/`cout`, we read from a file using `ifstream fin` and we write to a file using `ofstream fout`. The **only** difference is we first have to open a file before we can read/write, AND we must close that file as well
  
  <pre><code class="language-c++">#include&lt;fstream&gt;
  using namespace std;

  int main() {
    // you can call these whatever you want, but we usually use "fin" & "fout"
    ifstream fin; 
    ofstream fout; 

    // have to open
    fin.open("ticket.txt");

    fout.open("output.txt");

    // read from ticket.txt word by word
    fin &gt;&gt; word1 &gt;&gt; word2 &gt;&gt; etc...

    // write to output.txt
    fout &lt;&lt; "we just read: " &lt;&lt; word1 &lt;&lt; word2 &lt;&lt; etc...

    // MUST close
    fin.close();
    fout.close();
  }</code></pre>

- ### What to #include
  To use file streams, include the file stream library `#include<fstream>`.

- ### Error checking

  <pre><code class="language-c++">#include&lt;fstream&gt;
  using namespace std;

  int main() {
    ifstream fin;
    
    // if we can't open the file, exit program
    if (!fin.open("foo.txt")) {
      cout &lt;&lt; "Could not open foo.txt!\n";
      return 1;
    }

    // otherwise do stuff...
    }
  }</code></pre>

- ### getline()

  I haven't gone over getline before, but it's pretty self explanatory. It takes two arguments: the stream you'll read from, and the destination you'll write to. (`getline(src, dest)`). It's different from `cin`, but it works the same in that A. it can read from `stdin` (standard input) and B. it can be used the same way in if statements, while loops, etc.

    <pre><code class="language-c++">#include&lt;string&gt;
  using namespace std;

  int main() {
    // line buffer
    string line;

    // read lines until we can't anymore
    while (getline(cin, line)) {
      // print each line
      cout &lt;&lt; line &lt;&lt; '\n';
    }
  }</code></pre>

  `getline()` will read a line until it reaches its "delimiter" (a `char` at which it'll stop) which is `\n` by default. It'll discard that delimiter from the stream once read (so it won't be left in the buffer), but it will not include it in the string it reads. So when you read a line, it will not be ended with a `\n`, hence why I output `cout << line << '\n';`.

## Argv/Argc

- ### What is argv/argc used for?
  argv/argc are implicitly defined variables that are arguments to `int main()`. They hold the "command-line arguments" and the number of command-line arguments, respectively.

  A command-line argument is anything that comes after you do `./program`. So in `./program one two`, there are 2 command-line args. `one` & `two`.<br>

  `argc` is an `int` that is equal to the number of command-line arguments used, and `argv` is an **array** of **c-style strings** that contains the actual command-line arguments. in C++, there is always at least one command-line argument which is the name of the program itself.<br>

  running `./program one two`, `argc == 3` and `argv == {"program", "one", "two"}`. Notice how although we technically only used two command-line args, `argc` is equal to three. That's what I mean when I say there is always at least one command-line argument, which is the name of the program being ran. In this case `program`. This is also evident in `argv`, as `argv[0]` is the name of the program. This is *always* true.<br>

  running `./program`, `argc == 1` and `argv == {"program"}`. Hopefully that makes sense.<br>

  Now you're probably asking why these are useful. It's pretty niche for you right now, but it's nice to be able to run your programs with "input" without really inputting it yourself over and over, if you're trying to debug your code for instance. Often times your program will accept a variety of command-line args which will change the behavior of your program based on what they are. E.g. if you have a program that reads a variable number of lines, it'd be nice to choose how many lines are read from command-line arguments instead of inputting it every time and reading it with `cin` or something.<br>

  `./read-lines 10` would read 10 lines, `./read-lines 1` would read just 1 line. You get the point.

- ### What does each stand for?
  
  `Argc` stands for argument count, and `argv` stands for argument values.

- ### What are the types of each one

  `argc` is type `int`, and `argv` is type `char**` or `char*[]` (both the same thing). `char*[]` just means an array (signified by the `[]`) of `char*`, which you can just think of as "C-style" strings for now. So it's an array of C-style strings. It's important to note that a C-style string is not the same thing as when you use `string` in C++. They're different. A `string` is similar to `char*`, but they are very different in practice.<br>**Also note** that even if you have a program `./read-lines 100`, the command-line argument `100` will ont be treated as a number, It is read as a C-style string, so it's stored as `"100"`. You need to do the work yourself to convert it to a number, if necessary.

- ### What is stored in each one
  
  I went over this already, but to reiterate, `argc` contains the number of commmand-line args, and `argv` contains the actual command-line args themselves. It's kinda like a scale. When you step on a scale, it shows your weight in lbs (`argc`), but YOU'RE what makes up that amount (`argv`).<br>**Also note that** `argv[0]` will always contain the program name. Thus, even if there are no command-line args passed (e.g. `./myprogram`), `argc` will be equal to 1, and `argv[0]` will be `"myprogram"`.

- ### Why use stringstreams with argc/argv?

  Like I mentioned in the [What are the types of each one](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/midterm.html#what-are-the-types-of-each-one), `argv` holds strings. (C-style, but I'm not gonona keep typing that). So even if you use a number as a command-line arg, it'll be stored as a string. So if you want to convert it to a number (which you will 99% of the time), then the easy way to do it is with `stringstream`'s.

- ### Argc/Argv examples
  
  - `./program one two` - `argc == 3` & `argv == {"program", "one", "two"}`
  - `./foo 1 two 3` - `argc == 4` & `argv == {"foo", "1", "two", "3"}`
  - `./bar` - `argc == 1` & `argv == {"bar"}`

- ### How it's defined in `main()`
  
  <pre><code class="language-c++">int main(int argc, char*[] argv) {
    // you define the arguments EXACTLY as they are up there.
  }</code></pre>

- ### Error checking with `argc`
  Say we have a program that reads how ever many lines we specify via the command-line. But maybe we don't want the program to run unless someone specifies the line amount. You would implement `argc` error checking to achieve this

  <pre><code class="language-c++">int main(int argc, char*[] argv) {
    // if there aren't two command-line args (the name and line number),
    // then exit the program

    if (argc != 2) {
      cout &lt;&lt; "usage: ./read-lines [n]\n";
      return 1;
    }
  }</code></pre>

## String Streams

Again, another stream-type. A lot of students get tripped up with string streams, but they are just a way to interpret `strings` as streams. Just like `fstream` lets you interpret files as streams. A stream is just a medium for us to fluidly read/write things in C++. The most common use cases for string streams are to either convert from strings to numbers easily, (or numbers to strings), or to "parse" a line from `getline` into individual words.

- ### What to include

  To use the string stream library, you must `#include<sstream>`

- ### How to write to strings and extract from strings

  - ##### Declaring an `istringstream`/`ostringstream` object

    Just like `fstream`, you have to create an input stream, and output stream. `istringstream` is the input string stream type, and `ostringstream` is the output string stream type.<br><br>

    You use declare and use an input string stream like so

    <pre><code class="language-c++">#include&lt;sstream&gt;
    using namespace std;

    int main() {
      // declare istringstream with a string already opened
      string my_string = "Parse me!";
      istringstream iss(my_string);

      string word;
      // "extract" & print every word from the string we're parsing
      while (iss &gt;&gt; word) {
        cout &lt;&lt; word &lt;&lt; '\n';
      }
    }</code></pre>

    If you recall back to COSC 101, you'll remember the concept of a *constructor*. When I declare the `istringstream` above, I pass the constructor a string `my_string` to initialize the stream it's going to extract from (`istringstream iss(my_string)`). You can also just declare an empty `istringstream` and initialize it later using the `str()` method.

    <pre><code class="language-c++">#include&lt;sstream&gt;
    using namespace std;

    int main() {
      // declare istringstream with a string already opened
      string my_string = "Parse me!";
      istringstream iss(); // use default constructor which leaves stream empty

      // initialize stream using .str()
      iss.str(my_string);

    }</code></pre>

    `str()` can also be used to change whatever the stream is for that string stream. So if you wanted to start extracting from a different string, you would also use `str()`

    <pre><code class="language-c++">#include&lt;sstream&gt;
    using namespace std;

    int main() {
      // declare istringstream with a string already opened
      string my_string = "Parse me! 25";
      istringstream iss(my_string);

      // change what the stream is for our istringstream
      string pick_me = "Pick me!";
      iss.str(pick_me);
    }</code></pre>

    `ostringstream` is defined similarly, except `.str()` has different behavior. `ostringstream` is like the `StringBuilder` class from Java. It allows you to fluidly "build" a string from words.

    <pre><code class="language-c++">#include&lt;sstream&gt;

    using namespace std;

    int main() {
      ostringstream string_builder;

      // build a string
      string_builder &lt;&lt; "Hello, my name is " &lt;&lt; name &lt;&lt; " and I am " &lt;&lt; age &lt;&lt; " years old!\n";

      /* create that string */
      string final_string = string_builder.str();
    }</code></pre>

    The above code looks very similar to printing something with `cout`, but it's building a string and then copying that string to an actual variable (`final_string`) So instead of outputting all of that to `stdout`, it writes it to the `ostringstream` buffer, and we extract all of it into a string using `string_builder.str()`. The use case for `ostringstream` is a bit more limited for you guys at the moment, but that's how you can use it.

    - ### Use cases

      - Converting string to number

      <pre><code class="language-c++">#include&lt;sstream&gt;

      using namespace std;

      int main() {
        // holds our number
        int number;
        // our string we're gonna convert
        string strNumber = "10";
        // initialize istringstream with the string "10"
        istringstream iss(strNumber);
        // extract "10" and place it in number variable
        iss >> number;
      }</code></pre>

      - Extracting words from a line
    
      <pre><code class="language-c++">#include&lt;sstream&gt;
      #include&lt;string&gt;

      using namespace std;

      int main() {
        // holds our line
        string line;

        // holds our words we'll extract from each line
        string word1, word2, word3;

        // read every line from stdin
        while (getline(cin, line)) {
          istringstream iss(line);

          // extract words from line to word1, 2, and 3
          iss &gt;&gt; word1 &gt;&gt; word2 &gt;&gt; word3;
        }
      }</code></pre>

<hr>

## That's it!
If you have any questions, feel free to ask on the Discord or DM me directly via whichever way you want. I'll be up all night to answer your midterm-related questions!