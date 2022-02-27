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

## Step 1: Prompt for user input & General Setup

- ### Prompt user for
    {: .no_toc }

  - ticket file (`std::string`)
  - report file (`std::string`)
  - report start date (3 `int`'s `mm dd yyy`)
  - report end date (3 `int`'s in the format `mm dd yyy`)

    ##### example
    {: .no_toc }

    {% highlight c++ %}
    Enter a ticket file: ticket // ticket file is "ticket"
    Enter a report file: output // report file is "output"
    Enter report start date (mm dd yyyy): 7 1 2017 // three ints separated by spaces
    Enter report end date   (mm dd yyyy): 8 11 2018
    {% endhighlight %}


    This part is the same way you've been getting input. Just use `cin >> ` and read into the appropriate variables. 
    
- ### General Setup
    Later in the lab you'll need to check if the line in the input file (more on that below) you're reading from contains a "date" that is between 
    `[start date, end date]` (inclusive). I recommend setting this up at least somewhat here using the following formula:

    {% highlight c++ %}
    // from start date, read yyyy, dd, and mm into these variables respectively
    int end_year, end_day, end_month; 
    // read yyyy, dd, and mm into these variables respectively
    int start_year, start_day, start_month; 

    // once those are read in...

    int year_difference = end_year - start_year;
    int day_difference = end_day - start_day;
    int month_difference = end_month - start_month;
    {% endhighlight %}


    You'll see why this is useful in the later steps...
    You should also initialize your `const` variables at the top of your program here. More info on this in the [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section.

## Step 2: File I/O

the interesting part of this lab is that this will likely be your first time working with file input/output in C++ (or maybe any programming language for that matter)
It might seem daunting at first, but luckily C++ has made it almost as simple as `stdin` I/O, and you'll see that moving forward.

- ### The `<fstream>` Library

  Similar to `<iostream>`, `<fstream>` is basically a subset of the former. It provides very similar methods for you to interact with files the same way you would `stdin` (i.e. reading/writing) For more information about the library, you can check out [the documentation here](https://en.cppreference.com/w/cpp/header/fstream). But I'll give a brief description of the bulk of it below.

  - ### Opening a file
   
    The `fstream` library has 3 types of ways to interact with a file: `ofstream`, `ifstream`, and `fstream`. Each of those words are types derived from the `fstream` library, and they allow you to define a variable that will be your "handle" for manipulating files. 
    `ofstream` allows you to *output* to a file, (hence the 'o'), `ifstream` allows you to *read* from a file, and `fstream` allows you to do both simultaneously. Their syntax varies compared to `iostream`'s `cout` & `cin`, but I hope you can already see the similarities.

    {% highlight c++ %}
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

        // string to hold the name of our file
        string file_name;

        cout << "Enter file name: ";
        // user enters file from the console and stores it in file_name
        cin >> file_name;

        // opens file_name for reading
        fin.open(file_name);

        // opens file_name for writing
        fout.open(file_name);

        // closes both file handles
        fin.close();
        fout.close();
    }
    {% endhighlight %}


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
  while `cin` is an input stream manipulator just like `ifstream`, `cin` is pre-defined for you. You'll need to define your *own* `ifstream` manipulator before you can do any reading.

  {% highlight c++ %}
  ifstream fin;
  {% endhighlight %}

  Hopefully you've already done this assuming you've finished step 2. Once you've opened a file, you can interact with it the same way you interact with `stdin` (standard input e.g. the console/terminal) when you're getting user input. The difference is that the input is the contents of the file you're reading from.

  {% highlight c++ %}
  ifstream fin; // pretend we opened a file
  int v, v2, v3
  while (fin >> v >> v2 >> v3) {
      // do whatever we want...
  }
  {% endhighlight %}

  And now you should see the striking similarities! The above code reads everything from a file, and stuffs each "word" (whitespcae-separated string) into a variable as many times as we specify.

  - ### Caveats
 
    Given an input file with the following format, you should easily be able to `fin >>` as many times as there are "words" in the file. Move each value into its respective variable with `while (fin >> v1 >> v2 >> v3 >> v4 >> v5 >> v6 >> v)` (dont use those var names) and do stuff with them. This will read one line at a time from an input file with the following format
    
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

    More information about the file format (straight from the lab page on Canvas):

    >The citation number may contain numbers and letters.
    >The month is an integer between 1 (January) and 12 (December).
    >The day and year are integers.
    >The clocked speed is an integer in miles per hour.
    >The speed limit is an integer in miles per hour.
    >The type of road is a single character: I or i (Interstate), R or r (Residential), H or h (Highway).

    ### **TLDR**
    {: .no_toc }
    Create variables to read in your citation number, month, day, etc, and read them from the file same as you would using `cin`.
    {% highlight c++ %}
    citation_number = string
    month           = int
    day             = int
    year            = int //or string, more on this in the Hints section
    ...
    type of road    = char
    {% endhighlight %}


## Step 4: Outputting to a file

This process is very similar to step 3, and again has extremely familiar syntax to its `iostream` analog `cout`. You'll be creating a file (this is done automatically when you output to a file) with the name of whatever you read in from step 1. The output should follow a strict format:
    
    <day>-<3-character Month>-<Year> <citation> $<fine>

And even stricter:
1. The day must be 2 digits exactly
2. Month has to be any one of Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, or Dec
3. The year must be 4 digits. (ex. if you read 10 for the year, the output would be 2010; 17 the output would be 2017)
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

All of this pedantic output formatting will be accomplished via the `<iomanip>` library. This allows you to manipulate streams using modifiers like `setw()`, `left`, `right`, and more. You can learn more about `<iomanip>` in the [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section.

- ### Caveats

    Same as step 3, you need to make sure you open/close and some more stuff:
    1. you'll be calling `fout <<` in your `while` loop that's doing `fin >>`. So make sure you open the file with `fout` before starting the loop.
    2. are able to open the specified file
    3. are closing the file handle once you're done with it (i.e. done outputting)
    4. before you print anything, you'll need to determine if the ticket line you've just read is within a valid date range. If you did as I recommended in Step 1, then you will have setup your `difference` variables. Using these variable, you can determine if the date read from the ticket input file (in Step 3) is between the original `start` & `end` date using the following formula
   
        {% highlight python %}
        python
        if end_year - ticket_yyyy <= diff_year and... [another condition] and ...:
            # then print ticket
        else:
            # skip this ticket
        {% endhighlight %}

    The formula is a bit vague, but you should be able to do the rest yourself. You'll need to apply that formula for `dd`, `mm`, and `yyyy`. If any one of those checks returns false, then you'll skip that ticket.

    1. for printing the correctly formatted year, you'll need to determine whether the `yyyy` you've read from the input file is 4 digits or less than 4 digits. There's more than one way to do this. 
    2. for printing the month, you'll need to match the `mm` you've read in to the index of your `const string months[]` array. (I named my `months` you can name it whatever). 
   *If you don't know what* `const` *is, read the* [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section.
    1. for printing the fine amount, you'll need to use the fine-multiplier specified in the lab writeup
            
            Interstate multiplier:  5.2252
            Highway multiplier:     9.4412
            Residential multiplier: 17.1525
            None of the above:      12.152
            
    From there, calculating the fine is easy. The multiplier is multiplied to the difference between the speed limit and the clocked speed to determine the fine's dollar amount. Note you'll need to use a switch statement to apply the fine multiplier as well. You should have a `const double` variable for each of the multipliers above. Define these at the beginning of your program. You'll use the switch statement to evaluate the `road type` `char`, and make a case for each potential `char` (`i`, `h`, `r`, and `p`). Each case should calculate the fine amount using whichever of the multipliers you've defined in your code. Again, more info on `const` in the [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section.

## Step 5: You made it!

Once you've done all of the above, you've basically finished! But there are a couple things to look out for

1. **MAKE SURE YOU'RE FORMATTING YOUR CODE NEATLY AND COMMENTING PROPERLY**. I made a small post about this on the Discord server, but it's very important you follow it. [Here's the link](https://discord.com/channels/935991929978621962/935991930582630404/945778803072974859). If you're not at least tangentially following that advice, you'll end up running into issues later down the line *I promise*.
2. Make sure you're adhering to the rubric on the lab Canvas page. If you've not done everything as specified in the rubrik, then don't be surprised if your grade is penalized for it. That includes having your output match the solution output *exactly* (including any newline characters i.e. `\n`!)

## Hints

- ### `const`
  a keyword that is added before a variable. If you use `const`, you are locking the value given to the variable in place for the entirety of your program. It's permanent, and you cannot change it. You won't ever `cin >> my_constant` either, because a `const` variable is defined at "compile time", meaning you define its contents in your code before it's even run. If you create a const variable, it should almost always look something like

    {% highlight c++ %}
    // const array
    const int arr[] = {1, 2, 3};

    // const double
    const double dbl = 2.0;

    // const string
    const string str = "this is constant";
    {% endhighlight %}


    Notice how they're each assigned a value the moment they're declared. Just remmeber that value will not change!

- ### `ifstream::open`
  in this case, it'll be `fin.open(filename)` because `fin` is the name I decided to give to my `ifstream` variable. This `fin.open(filename)` opens a file from the variable `filename`, which contains a `string` (e.g. something like "file.txt"). If that file doesn't exist, then `fin.open(filename)` will return `false`. If it does exist, then your `ifstream` handle (`fin` in this case) can read from the file the same way you read from input using `cin >>`:
   
    {% highlight c++ %}
    ifstream fin // create ifstream
    fin.open("example_file.txt");

    // pretend that example_file.txt has the following contents from /* to */:
    /*
    Hello my name is Ethan!
    */

    // define temporary string
    string temporary_str;

    fin >> temporary_str // temporary_str = Hello
    fin >> temporary_str // temporary_str = my
    fin >> temporary_str // temporary_str = name
    fin >> temporary_str // temporary_str = is
    fin >> temporary_str // temporary_str = Ethan!

    fin.close() // make sure to close the file!!!
    {% endhighlight %}


- ### `ofstream::open` 
    Almost the same as `ifstream::open`, except you can only **write** instead of **read**. If `filename` does not exist (e.g. file.txt is not a file in your current working directory) then `fout.open(filename)` will automatically create the file. If it *does* exist, then `fout.open(filename)` will delete the contents of `filename` and thus it will be empty. You can write to the file the same way you write to `stdout` (the console) using `cout <<`
    {% highlight c++ %}
    ofstream fout // create ofstream
    fout.open("example_file.txt");

    // example_file.txt before writing with fout (empty)

    fout << "I love ";
    fout << "computer";
    fout << " science\n";

    // example_file.txt after writing with fout 
    // has the following contents from /* to */:
    /*
    I love computer science

    */
    fout.close() // make sure to close the file!!!
    {% endhighlight %}


- ### `<iomanip>`
  This is your output formatting library. For this assignment, you'll be using the following methods it provides you:
  - `left` prints any output following it in a left-justified field (i.e. it sticks to the left)
   
    {% highlight c++ %}
    #include <iomanip> // include iomanip!!!
    // other stuff...

    cout << left << "I'm a lefty\n";
    {% endhighlight %}

  - `right` prints any output following it in a right-justified field (i.e. it sticks to the right)
   
    {% highlight c++ %}
    #include <iomanip> // include iomanip!!!
    // other stuff...

    cout << right << "I'm a righty\n";
    {% endhighlight %}

  - `setfill(fill)` - defines the "fill" character using the paramter provided `fill`. Anything printed after this is set will fill whatever whitespace is output with whatever `fill` is
    {% highlight c++ %}
    #include <iomanip> // include iomanip!!!
    // other stuff...

    // set fill char to '-'
    cout << left << setfill('-') << setw(10) << 1000 << '\n';

    // prints:
    // 1000------
    {% endhighlight %}

  - `setw(len)` - defines the width in integer length `len` of the next value printed. If the value printed is 4 chars wide, and `setw(10)` is applied, then the value will be "padded" 6 extra characters (determined by `setfill()`) to get to a total width of 10

    {% highlight c++ %}
    #include <iomanip> // include iomanip!!!
    // other stuff...

    // set fill char to '-' & width to 10
    cout << left << setfill('-') << setw(10) << 1000 << '\n';

    // prints:
    // 1000------

    // notice how 1000 is 4 chars wide, and width is set to 10.
    // thus it prints 6 more '-' characters to make it to width of 10
    {% endhighlight %}


- ### Formatting `yyyy`
  
  if you read `yyyy` into a `string` then you can check the size of a `string` similar to a `vector`; if you read `yyyy` into an `int` you can use `%` to determine the magnitude of a number, and thus its number digits.

- ### Formatting `mm`
  
  remember that there are 12 month strings, so the size of the array is 12. You can index from 0-11, and the `mm` you read in is any value from 1-12) 

That's it. Bye!
