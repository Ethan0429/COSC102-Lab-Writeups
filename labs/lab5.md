---
layout: default
title: Lab 5 - Speeding Ticket Calculator
nav_order: 1
---
# Lab 5 - Speeding Ticket Calculator
{: .no_toc }

## Table of contents
{: .no_toc }
- TOC
{:toc}

## Step 1: Prompt for user input

- ### Prompt user for
    {: .no_toc }

  - ticket file (`std::string`)
  - report file (`std::string`)
  - report start date (3 `int`'s `mm dd yyy`)
  - report end date (3 `int`'s in the format `mm dd yyy`)

    ##### example
    {: .no_toc }

    ```c++
    Enter a ticket file: ticket // ticket file is "ticket"
    Enter a report file: output // report file is "output"
    Enter report start date (mm dd yyyy): 7 1 2017 // three ints separated by spaces
    Enter report end date   (mm dd yyyy): 8 11 2018
    ```

## Step 2: File I/O

the interesting part of this lab is that this will likely be your first time working with file input/output in C++ (or maybe any programming language for that matter)
It might seem daunting at first, but luckily C++ has made it almost as simple as `stdin` I/O, and you'll see that moving forward.

- ### The `Fstream` Library

  - Similar to `<iostream>`, `<fstream>` is basically a subset of the former. It provides very similar methods for you to interact with files the same way you would `stdin` (i.e. reading/writing) For more information about the library, you can check out [the documentation here](https://en.cppreference.com/w/cpp/header/fstream). But I'll give a brief description of the bulk of it below.

  - ### Opening a file
   
    - The `fstream` library has 3 types of ways to interact with a file: `ofstream`, `ifstream`, and `fstream`. Each of those words are types derived from the `fstream` library, and they allow you to define a variable that will be your "handle" for manipulating files. 
    `ofstream` allows you to *output* to a file, (hence the 'o'), `ifstream` allows you to *read* from a file, and `fstream` allows you to do both simultaneously. Their syntax varies compared to `iostream`'s `cout` & `cin`, but I hope you can already see the similarities.

    ```c++
    #include <iostream>
    #include <fstream> // including fstream
    using namespace std;

    int main() {
        /* define handlers for file i/o 
        note this is just demonstrating 
        how fstream file handlers are declared */

        ifstream fin; // can ONLY read files
        ofstream fout; // can ONLY output to files
        fstream finfout; // can BOTH read & output to files AND has a funny name

        // disclaimer you can call these whatever you want I was just making a joke
    }
    ```

    Generally it's good practice to use whichever type of stream you'll need as opposed to using an `fstream` for everything, but I don't think it really matters for the purposes of this lab.

    **The end goal of this section** is to use the `fstream` library to *read* lines from a file (so you won't be needing `ofstream` anyway, and can just stick to `ifstream`).
    Once you've opened a file, you can manipulate it the same way you can with `cin`.
    
    I'm sure you've already discussed this in lecture, but if you haven't you can read on [how to do so here](https://www.cplusplus.com/reference/fstream/fstream/open/).

    - ### Caveats

        There are two important things to note for this step:
        1. you must **check to make sure the file is open** before continuing your program. If it's not open (even after attempting to open it), then you should handle that. If you do encounter a file that cannot be opened, that means it doesn't exist, and your output should look like this if that is the case

                Enter a ticket file: somebadfilename
                Unable to open somebadfilename.

        2. you must **close the file** after you're finished with it. Technically this is not really necessary for C++, but it is for your grade. Everything that is opened must be closed, as this is the safest and best practice for you as a programmer.

## Step 3: Reading from the file

So you've opened the file, but now you need to read from it. Luckily, this is almost entirely the same as using `cin` with a few exceptions

- ### Using `ifstream`
  - while `cin` is an input stream manipulator just like `ifstream`, `cin` is pre-defined for you. You'll need to define your *own* `ifstream` manipulator before you can do any reading.
  ```c++
  ifstream fin;
  ```
  Hopefully you've already done this assuming you've finished step 2. Once you've opened a file, you can interact with it the same way you interact with `stdin` (standard input e.g. the console/terminal) when you're getting user input. The difference is that the input is the contents of the file you're reading from.
  ```c++
  ifstream fin; // pretend we opened a file
  int v;
  while (fin >> v)
  ```
  And now you should see the striking similarities! The above code reads everything from a file, and stuffs each "word" (whitespcae-separated string) into a variable as many times as we specify.

  - ### Caveats
 
    If you're paying attention, you'll notice that the above code isn't fullproof. It reads words into integer variables until you've reached the end of a file. But this has so much potential for errors. There's no guarantee that the file you'll open contains even *1* integer. The code is also overwriting `v1` over and over. It's not practical. Of course the example is pretty contrived, but my point is that it's much easier to read from a file if you know what to expect, and luckily for this lab the input file will adhere to a specific format:
    
        E059564 8 12 2018 89 55 i
        E515522 7 3 2017 105 50 r
        E712221 6 4 2015 200 25 h
        E219221 12 25 17 2000 10 p

    Each line contains
        
        <citation number> <month> <day> <year> <clocked speed> <speed limit> <type of road>

    in that exact order and specification. So line 1 for example would be parsed as

        citation number = E059564
        month           = 8
        day             = 12
        year            = 2018
        clocked speed   = 89
        speed limit     = 55
        type of road    = i

    More information about the file format (straight from the lab page on Canvas)
    > The citation number may contain numbers and letters.
    The month is an integer between 1 (January) and 12 (December).
    The day and year are integers.
    The clocked speed is an integer in miles per hour.
    The speed limit is an integer in miles per hour.
    The type of road is a single character: I or i (Interstate), R or r (Residential), H or h (Highway).

    ### **TLDR**
    {: .no_toc }
    Create variables to read in your citation number, month, day, etc, and read them from the file same as you would using `cin`.
    ```c++
    citation_number = std::string
    month           = int
    day             = int
    year            = int
    ...
    type of road    = char
    ```

## Step 4: Outputting to a file

This process is very similar to step 3, and again has extremely familiar syntax to its `iostream` analog `cout`. You'll be creating a file (this is done automatically when you output to a file) with the name of whatever you read in from step 1. The output should follow a strict format:
    
    <day>-<3-character Month>-<Year> <citation> $<fine>

And even stricter:
1. The day must be 2 digits exactly
2. Month has to be any one of Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, or Dec
3. The year must be either 4 digits. (ex. if you read 10 for the year, the output would be 2010; 17 the output would be 2017)
4. The citation will be output in a **left justified** field, **10 characters wide**. and should be immediately followed by a `$`. 
5. The fine will be output after the `$`, **right justified**, and **9 characters wide**.
6. Only print the citations that occurred between the start and end dates (inclusive) you read from step 1.

So input 
    
    E059564 8 12 2018 89 55 i
    E515522 7 3 2017 105 50 r
    E712221 6 4 2015 200 25 h
    E219221 12 25 17 2000 10 p

should have the corresponding output

    03-Jul-2017 E515522    $   943.39
    25-Dec-2017 E219221    $ 24182.48

All of this pedantic output formatting will be accomplished via the `<iomanip>` library. This allows you to manipulate streams using modifiers like `setw()`, `left`, `right`, and more.

- ### Caveats

    Same as step 3, you need to make sure you 
    1. are able to open the specified file
    2. are closing the file handle once you're done with it (i.e. done outputting)
    3. For printing the fine amount, you'll need to use the fine-multiplier specified in the lab writeup
            
            Interstate multiplier:  5.2252
            Highway multiplier:     9.4412
            Residential multiplier: 17.1525
            None of the above:      12.152
            
    From there, calculating the fine is easy. The multiplier is multiplied to the difference between the speed limit and the clocked speed to determine the fine's dollar amount. Note you'll need to use a switch statement to apply the fine multiplier as well. Look at step 5 to get more on that.
   

## Step 5: You made it!

Once you've done all of the above, you've basically finished! But there are a couple things to look out for

1. **MAKE SURE YOU'RE FORMATTING YOUR CODE NEATLY AND COMMENTING PROPERLY**. I made a small post about this on the Discord server, but it's very important you follow it. [Here's the link](https://discord.com/channels/935991929978621962/935991930582630404/945778803072974859). If you're not at least tangentially following that advice, you'll end up running into issues later down the line *I promise*.
2. For formatting your 3-char months, you're ***required*** to have `const` **array** containing the variables you'll need to print (i.e. the months). 
3. You're ***required*** to make your fine multipliers `const` variables as well. A `const` variable is just a variale that is declared at compile time, and using the keyword `const` is you making a promise (and the compiler as well) to not change the contents of that variable. It's good practice to make things that are static (unchanging) `const` so long as you know its value before even running the program.
4. You're ***required*** to use a switch statement to apply the fine multiplier. (Hint, the signifier for each fine multiplier will be a char [i for interstate, for example])

That's it. Bye!
