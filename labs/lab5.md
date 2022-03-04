---
layout: default
title: Lab 5 - Speeding Ticket Calculator
nav_order: 1
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

`Mon, 29 Feb 2022 00:33:03 EST`
  - added `fixed` & `setprecision()`, and [IMPORTANT](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#important-iomanip-feature) to [iomanip](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#iomanip) section<br><br>

`Mon, 28 Feb 2022 13:30:03 EST`
  - updated the [ticket date range checking](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#checking-ticket-date-range)

  </div>
</details>
<hr>

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

    <pre><code class="language-plaintext">Enter a ticket file: ticket // ticket file is "ticket"
    Enter a report file: output // report file is "output"
    Enter report start date (mm dd yyyy): 7 1 2017 // three ints separated by spaces
    Enter report end date   (mm dd yyyy): 8 11 2018</code></pre>


    This part is the same way you've been getting input. Just use `cin >> ` and read into the appropriate variables. 
    
- ### General Setup
    Later in the lab you'll need to check if the line in the input file (more on that below) you're reading from contains a "date" that is between 
    `[start date, end date]` (inclusive). I recommend setting this up to prepare for the actual date recognition later on:

    <pre><code class="language-cpp">// from start date, read yyyy, dd, and mm into these variables respectively
  int end_year, end_day, end_month; 
  // read yyyy, dd, and mm into these variables respectively
  int start_year, start_day, start_month; </code></pre>


    You'll see why this is useful in the later steps...
    You should also initialize your `const` variables at the top of your program here as well. More info on `const` in the [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section, and I'll explain what exactly needs to be `const` in the later steps.

## Step 2: File I/O

the interesting part of this lab is that this will likely be your first time working with file input/output in C++ (or maybe any programming language for that matter)
It might seem daunting at first, but luckily C++ has made it almost as simple as `stdin` I/O, and you'll see that moving forward.

- ### The `<fstream>` Library

  Similar to `<iostream>`, `<fstream>` is basically a subset of the former. It provides very similar methods for you to interact with files the same way you would `stdin` (i.e. reading/writing) For more information about the library, you can check out [the documentation here](https://en.cppreference.com/w/cpp/header/fstream). But I'll give a brief description of the bulk of it below.

  - ### Opening a file
   
    The `fstream` library has 3 types of ways to interact with a file: `ofstream`, `ifstream`, and `fstream`. Each of those words are types derived from the `fstream` library, and they allow you to define a variable that will be your "handle" for manipulating files. 
    `ofstream` allows you to *output* to a file, (hence the 'o'), `ifstream` allows you to *read* from a file, and `fstream` allows you to do both simultaneously. Their syntax varies compared to `iostream`'s `cout` & `cin`, but I hope you can already see the similarities.

    <pre><code class="language-cpp">#include &lt;iostream&gt;
    #include &lt;fstream&gt; // including fstream
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
    }</code></pre>


    Generally it's good practice to use whichever type of stream you'll need as opposed to using an `fstream` for everything, but I don't think it really matters for the purposes of this lab.

**The end goal of this section** is to use the `fstream` library to *read* lines from a file (so you won't be needing `ofstream` anyway, and can just stick to `ifstream`).
Once you've opened a file, you can manipulate it the same way you can with `cin`.

I'm sure you've already discussed this in lecture, but if you haven't you can read on [how to do so here](https://www.cplusplus.com/reference/fstream/fstream/open/).

- ### Caveats

  There are two important things to note for this step:
  #### make sure file is open
  you must **check to make sure the file is open** before continuing your program. If it's not open (even after attempting to open it), then you should handle that. If you do encounter a file that cannot be opened, that means it doesn't exist, and your output should look like this if that is the case
  
    <pre><code class="language-plaintext">Enter a ticket file: somebadfilename
  Unable to open somebadfilename.</code></pre>

  #### make sure you close the file
  you must **close the file** after you're finished with it. Technically this is not really necessary for C++, but it is for your grade. Everything that is opened must be closed, as this is the safest and best practice for you as a programmer.

## Step 3: Reading from the file

So you've opened the file, but now you need to read from it. Luckily, this is almost entirely the same as using `cin` with a few exceptions

- ### Using `ifstream`
  while `cin` is an input stream manipulator just like `ifstream`, `cin` is pre-defined for you. You'll need to define your *own* `ifstream` manipulator before you can do any reading.

  <pre><code class="language-cpp">ifstream fin;</code></pre>

  Hopefully you've already done this assuming you've finished step 2. Once you've opened a file, you can interact with it the same way you interact with `stdin` (standard input e.g. the console/terminal) when you're getting user input. The difference is that the input is the contents of the file you're reading from.

  <pre><code class="language-cpp">ifstream fin; // pretend we opened a file
  int v, v2, v3
  while (fin >> v >> v2 >> v3) {
      // do whatever we want...
  }</code></pre>

  And now you should see the striking similarities! The above code reads everything from a file, and stuffs each "word" (whitespcae-separated string) into a variable as many times as we specify.

  - ### Input File Format
 
    Given an input file with the following format, you should easily be able to `fin >>` as many times as there are "words" in the file. Move each value into its respective variable with `while (fin >> v1 >> v2 >> v3 >> v4 >> v5 >> v6 >> v)` (dont use those var names) and do stuff with them. This will read one line at a time from an input file with the following format
        <pre><code class="language-plaintext">E059564 8 12 2018 89 55 i
        E515522 7 3 2017 105 50 r
        E712221 6 4 2015 200 25 h
        E219221 12 25 17 2000 10 p</code></pre>

    Each line contains
        
      <pre><code class="language-plaintext">&lt;citation number&gt; &lt;month&gt; &lt;day&gt; &lt;year&gt; &lt;clocked speed&gt; &lt;speed limit&gt; &lt;type of road&gt;</code></pre>

    in that exact order and specification. So line 1 for example would be parsed as

      <pre><code class="language-plaintext">citation number = E059564
    month           = 8
    day             = 12
    year            = 2018
    clocked speed   = 89
    speed limit     = 55
    type of road    = i</code></pre>

    More information about the file format (straight from the lab page on Canvas):
    <q>The citation number may contain numbers and letters.
    The month is an integer between 1 (January) and 12 (December).
    The day and year are integers.
    The clocked speed is an integer in miles per hour.
    The speed limit is an integer in miles per hour.
    The type of road is a single character: I or i (Interstate), R or r (Residential), H or h (Highway).</q>

    ### **TLDR**
    {: .no_toc }
    Create variables to read in your citation number, month, day, etc, and read them from the file same as you would using `cin`.

    <pre><code class="language-cpp">citation_number = string
    month           = int
    day             = int
    year            = int //or string, more on this in the Hints section
    ...
    type of road    = char</code></pre>


## Step 4: Outputting to a file

#### Output Format Requirements
This process is very similar to step 3, and again has extremely familiar syntax to its `iostream` analog `cout`. You'll be creating a file (this is done automatically when you output to a file) with the name of whatever you read in from step 1. The output should follow a strict format:
    
  <pre><code class="language-plaintext">&lt;day&gt;-&lt;3-character Month&gt;-&lt;Year&gt; &lt;citation&gt; $&lt;fine&gt;</code></pre>

And even stricter:
1. The day must be 2 digits exactly
2. Month has to be any one of Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, or Dec
3. The year must be 4 digits. (ex. if you read 10 for the year, the output would be 2010; 17 the output would be 2017)
4. The citation will be output in a **left justified** field, **10 characters wide**. and should be immediately followed by a `$`. 
5. The fine will be output after the `$`, **right justified**, and **9 characters wide**.
6. Only print the citations that occurred between the start and end dates (inclusive) you read from step 1.

So input 

  <pre><code class="language-plaintext">E059564 8 12 2018 89 55 i
E515522 7 3 2017 105 50 r
E712221 6 4 2015 200 25 h
E219221 12 25 17 2000 10 p</code></pre>

should have the corresponding output

  <pre><code class="language-plaintext">03-Jul-2017 E515522    $   943.39
25-Dec-2017 E219221    $ 24182.48</code></pre>

All of this pedantic output formatting will be accomplished via the `<iomanip>` library. This allows you to manipulate streams using modifiers like `setw()`, `left`, `right`, and more. You can learn more about `<iomanip>` in the [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section.

- ### Caveats

    Same as step 3, you need to make sure you open/close and some more stuff:

    1. you'll be calling `fout <<` in your `while` loop that's doing `fin >>`. So make sure you open the file with `fout` before starting the loop.
    2. are able to open the specified file
    3. are closing the file handle once you're done with it (i.e. done outputting)
    4. #### Checking Ticket Date Range
       before you print anything, you'll need to determine if the ticket line you've just read is within a valid date range. If you did as I recommended in Step 1, then you'll already have some variables set to make the process a bit easier. As far as I'm aware, you're not allowed to use the STL time library, so you'll have to resort to some good ol' logic unless your instructor has said otherwise. Here's how I've done it (the following code is "pseudo-code" which is just simplified code to make it easier on the eyes. The logic still applies and will be implemented the same regardless of your language of choice)

        <pre><code class="language-python"># check if the year we've just read is in (start_year, end_year)
        if current_year &gt; start_year and current_year &lt; end_year:
          # if it is, then we can print the ticket no matter what
        
        # if not, check if the current year is the same as start year
        else if current_year == start_year:
          # if it is, then we should now check the start month
          if current_month &gt; start_month:
            # if the current month is after the start month, then we can print
          
          # if not, chcek if the current month is the same as the start month
          if current_month == start_month:
            # if it is, then we need to compare the days
            if current_day &gt;= start_day:
              # if the current day is the same or past the start day, then we can print
        
        # if not, check if the current year is the same as end year
        else if current_year == end_year:
          # if it is, then we should now check the end month
          if current_month &lt; end_month:
            # if the current month is before the end month, then we can print
          
          # if not check if the current month is the same as the end month
          if current_month == end_month:
            # if it is, then we need to compare the days
            if current_day &lt;= end_day:
              # if the current day is the same or before the end day, then we can print
        
        # otherwise the current year is neither between nor equal to the years
        else
          # skip ticket</code></pre>

        The logic really isn't complicated, it's just tediously verbose. It's just stepping through the date from the broadest point (year) to the most specific point (day) and deciding to print based on the comparison from the current date (from the ticket line) vs. the start/end dates from Step 1.<br><br>
        If the current year is between the start/end years, then we can print it for sure (so print). If it's equal to one of the start/end years, then we need to check which one it's equal to. If it's the same as the *start year*, then we need to check if the current month is after or equal to the start month. (because then it falls in range of `(start month, end month)`, and we've already checked the year) And again, if the current month is equal to the start month, then we need to get more specific and check if the current day comes after the start day. Then you apply the same logic for the end year/month/day but invert the comparison. If the current year is equal to the end year, then we need to get more specific and see if the current month comes before the end month. If it does we can print, otherwise we need to check if the current month is equal to the end month, and then check if the current day comes before the end day. If it comes before or is equal to the end day, then we can print the ticket.<br><br>
        Technically we don't need to write any `else` statements here since the date will fall either between or outside of the range, and if it is outside then we don't do anything. We just continue to read the next ticket line and repeat the logic.<br><br>
    5. #### Formatting The Year
        for printing the correctly formatted year, you'll need to determine whether the `yyyy` you've read from the input file is 4 digits or less than 4 digits. There's more than one way to do this. 
    6. #### Formatting The Month
        for printing the month, you'll need to match the `mm` you've read in to the index of your `const string months[]` array. (I named my `months` you can name it whatever). 
   *If you don't know what* `const` *is, read the* [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section.
   
    7. #### Calculating Fine Amount

        for printing the fine amount, you'll need to use the fine-multiplier specified in the lab writeup

        <pre><code class="language-plaintext"> Interstate multiplier:  5.2252
        Highway multiplier:     9.4412
        Residential multiplier: 17.1525
        None of the above:      12.152</code></pre>
            
        From there, calculating the fine is easy. The multiplier is multiplied to the difference between the speed limit and the clocked speed to determine the fine's dollar amount. Note you'll need to use a switch statement to apply the fine multiplier as well. You should have a `const double` variable for each of the multipliers above. Define these at the beginning of your program. You'll use the switch statement to evaluate the `road type` `char`, and make a case for each potential `char` (`i`, `h`, `r`, and `p`). Each case should calculate the fine amount using whichever of the multipliers you've defined in your code. Again, more info on `const` in the [Hints](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab5.html#hints) section.

## Step 5: You made it!

Once you've done all of the above, you've basically finished! But there are a couple things to look out for

1. **MAKE SURE YOU'RE FORMATTING YOUR CODE NEATLY AND COMMENTING PROPERLY**. I made a small post about this on the Discord server, but it's very important you follow it. [Here's the link](https://discord.com/channels/935991929978621962/935991930582630404/945778803072974859). If you're not at least tangentially following that advice, you'll end up running into issues later down the line *I promise*.
2. Make sure you're adhering to the rubric on the lab Canvas page. If you've not done everything as specified in the rubrik, then don't be surprised if your grade is penalized for it. That includes having your output match the solution output *exactly* (including any newline characters i.e. `\n`!)

## Hints

- ### `const`
  a keyword that is added before a variable. If you use `const`, you are locking the value given to the variable in place for the entirety of your program. It's permanent, and you cannot change it. You won't ever `cin >> my_constant` either, because a `const` variable is defined at "compile time", meaning you define its contents in your code before it's even run. If you create a const variable, it should almost always look something like

    <pre><code class="language-cpp">// const array
  const int arr[] = {1, 2, 3};

  // const double
  const double dbl = 2.0;

  // const string
  const string str = "this is constant";</code></pre>

    Notice how they're each assigned a value the moment they're declared. Just remmeber that value will not change!

- ### `ifstream::open`
  in this case, it'll be `fin.open(filename)` because `fin` is the name I decided to give to my `ifstream` variable. This `fin.open(filename)` opens a file from the variable `filename`, which contains a `string` (e.g. something like "file.txt"). If that file doesn't exist, then `fin.open(filename)` will return `false`. If it does exist, then your `ifstream` handle (`fin` in this case) can read from the file the same way you read from input using `cin >>`:
   
    <pre><code class="language-cpp">ifstream fin // create ifstream
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

  fin.close() // make sure to close the file!!!</code></pre>


- ### `ofstream::open` 
    Almost the same as `ifstream::open`, except you can only **write** instead of **read**. If `filename` does not exist (e.g. file.txt is not a file in your current working directory) then `fout.open(filename)` will automatically create the file. If it *does* exist, then `fout.open(filename)` will delete the contents of `filename` and thus it will be empty. You can write to the file the same way you write to `stdout` (the console) using `cout <<`

    <pre><code class="language-cpp">ofstream fout // create ofstream
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
  fout.close() // make sure to close the file!!!</code></pre>


- ### `<iomanip>`
  This is your output formatting library. For this assignment, you'll be using the following methods it provides you:
  - `left` prints any output following it in a left-justified field (i.e. it sticks to the left)
   
    <pre><code class="language-cpp">#include &lt;iomanip&gt; // include iomanip!!!
    // other stuff...

    cout << left << "I'm a lefty\n";</code></pre>

  - `right` prints any output following it in a right-justified field (i.e. it sticks to the right)
   
    <pre><code class="language-cpp">#include &lt;iomanip&gt; // include iomanip!!!
    // other stuff...

    cout << right << "I'm a righty\n";</code></pre>

  - `setfill(fill)` - defines the "fill" character using the paramter provided `fill`. Anything printed after this is set will fill whatever whitespace is output with whatever `fill` is
 
    <pre><code class="language-cpp">#include &lt;iomanip&gt; // include iomanip!!!
    // other stuff...

    // set fill char to '-'
    cout << left << setfill('-') << setw(10) << 1000 << '\n';

    // prints:
    // 1000------</code></pre>

  - `setw(len)` - defines the width in integer length `len` of the next value printed. If the value printed is 4 chars wide, and `setw(10)` is applied, then the value will be "padded" 6 extra characters (determined by `setfill()`) to get to a total width of 10

    <pre><code class="language-cpp">#include &lt;iomanip&gt; // include iomanip!!!
    // other stuff...

    // set fill char to '-' & width to 10
    cout << left << setfill('-') << setw(10) << 1000 << '\n';

    // prints:
    // 1000------

    // notice how 1000 is 4 chars wide, and width is set to 10.
    // thus it prints 6 more '-' characters to make it to width of 10</code></pre>

  - `fixed` & `setprecision()` - sets the stream to print any decimal places to a fixed amount specified by `setprecision(x)` where `x` is some `int` specifying the amount of decimal places to be printed
  
    <pre><code class="language-cpp">#include &lt;iomanip&gt; // include iomanip!!!
    // other stuff
    double pi = 3.14;
    cout << fixed << setprecision(3) << pi;
    
    // prints:
    // 3.140

    cout << fixed << setprecision(1) << pi;

    // prints:
    // 3.1</code></pre>

  - #### IMPORTANT `IOMANIP` FEATURE
    All of the stream manipulators I've listed here, except for `setw()`, are "sticky". This means that whenever you use a manipulator like `setfill('x')`, that gets applied to anything you print after. So make sure you're aware of that when using `cout`. The only one that does not stick is `setw()`, which resets after it's been used.
- ### Formatting `yyyy`
  
  I recommend reading `yyyy` as an int, and then checking its magnitude using the modulo `%` operator. For instance, `1013 % 1000 = 13`, and `13 % 1000 = 13`. You'll need to print the year as a 4 digit number. So if you get a year `17` in the ticket file, you need to print `2017`, and if you get a year `2017`, you'll still print `2017`. You're also to assume that a 2 digit year like `17` is in the 21st century. Hopefully this hint helps you format the year.

- ### Formatting `mm`
  
  remember that there are 12 month strings, so the size of the array is 12. You can index from 0-11, and the `mm` you read in is any value from 1-12) 

That's it. Bye!