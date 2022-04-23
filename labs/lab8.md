---
layout: default
title: Lab 8 - PPM Manipulation
nav_order: 5
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
    <td class="tg-0lax">Information has been recently updated, so you should read the changelog below in case it concerns something you've already completed</td>
  </tr>
</tbody>
</table>

<hr>

# Lab 8 - PPM Manipulation
{: .no_toc }

## Table of contents
{: .no_toc }
- TOC
{:toc}

# Overview

The purpose of this lab is to manipulate a `.ppm` file via C++. In this lab, you'll familiarize yourself more with classes as well as file manipulation.

# What is PPM?
`PPM` is a file type that designates a specific format the file should adhere to. Just like how your `.cpp` files need to include things like `int main()` or `#include`, a .`ppm` file needs to include "meta" content that outlines details pertaining to the content, and of course it also needs to contain the content itself (e.g. the picture).<br><br>

### What it contains
If you were to open a `.ppm` file with a text editor, this is what you would see (this is on a per-line basis, in this exact order)
1. A heading which will always be the string `P3`. If a `.ppm` file does not contain this, then it does not conform to the standard that a `.ppm` file should, and therefore should be omitted.
2. The `width` and `height` of the image in pixels (e.g.`150 150` would be 150x150 image)
3. The `Max Intensity`. i.e. the absolute range of any color value in the image. Typically this will be `[0, 255]`, but the max intensity determines the bound, so assume the color range is `[0, maxIntensity]`.
4. Every line after this describes the color value of each subsequent pixel. Represented by integers ranging  from the color range mentioned earlier. **The important thing to note** is that these lines could be in various formats. They can follow/ precede commented lines (refer to #5), they can be in the format `r g b` or the can be in the format `r\nb\ng\n` (i.e. on separate lines), basically their format will be inconsistent. BUT we can work with that.
5. Any line beginning with `#` should be skipped, as it simply denotes a comment.

Here's an example of a `.ppm` file in plain text

<pre><code class="language-plaintext">P3
3 2
255
255 0 0
0 255 0
0 0 255
255 255 0
255 255 255
0 0 0</code></pre>
As you can see, it is a valid `.ppm` file as it conforms to the format required.<br>It has a width of `3px` and a height of `2px`.<br>The max intensity is `255`.<br>We know the dimensions of the file are 3x2, so logically there are 6 pixels in total in the image. Every line after the max intensity is the color value of a pixel at that index, so there should be 6 lines describing the color value since we have 6 pixels in the whole image.

# Your Job
Your job for this lab is to be able to parse these `.ppm` files and modify them in some way. Namely, your program should be able to do the following things:

- Read a `.ppm` file. e.g. check for `P3`, store your width/height/max intens
- Write a `.ppm` file
- Flip an image on the x-axis
- Flip an image on the y-axis

# Steps
In this section I will break down every part of the rubric into a *hopefully* more digestible step.

---

# The Pixel Structure & Picture Class

## Pixel

The first thing you'll want to get out of the way is your `Pixel` structure. This will make it easy to read/write data with pixels. Using the `Pixel` structure, we'll create a `Picture` class whose core data member will be a `vector` of `Pixel`s.<br>
`struct Pixel` contains:
- 3 `unsigned int`s, one for each `r`, `g`, and `b`. The reason we're using `unsigned` is because our color range is `[0,max_intensity]`, which is positive-only range.<br>
This should be pretty easy to create yourself.<br><br>
  
## Picture

### `Picture()`

A public constructor that sets up our class instance. It takes 0 arguments, and sets the width, height, and max intensity to 0.

### `private vector<Pixel>`

A private list of `Pixel`s. These will be our "coordinates" so to speak.

### Width, height, and max intensity
- `private int` - width of the picture
- `private int` - height of the picture
- `private int` - max intensity of the picture
  

### `const & Pixel get_pixel(int row, int col)` 

A public method that returns a **read-only** `Pixel` object based off of the row & col passed as arguments. You'll need to do some arithmetic to convert the `(row, col)` to a valid index to your vector of pixels, since the vector is 1D.


### `&Pixel get_pixel(int row, int col)`

A public method that returns a **mutable** `Pixel` object based off of the row & col passed as arguments. You'll notice this is an overloaded method, since there is another method with a similar signature. The difference between the two is the return value. This method returns `&Pixel`, whereas the other returns `const & Pixel`. i.e. this method is able to modify that pixel it returns, but the other one is not able to do so.

### `void set_pixel(int row, int col, const & Pixel px)` 

A public method that sets a pixel at index `(row, col)` (1D transposed) using the provided `px` argument. (e.g. `pixels[index] = px`) **Notice we're passing `px` as `const &` which means it is read-only**. `const` keyword means we cannot change a value, and `&` means we're passing-by-reference. Normally when you pass by reference, you're able to change the variable in a separate scope from where it was created and the changes will still persist outside of that scope. This is because passing by reference takes the memory address of the variable and passes that instead of the *value* itself. But we're not modifying `px` in this case, so why is it useful? Because this way we're not copying an entire data structure (`Pixel`) and passing it to a function. We're directly handing `px` to the function, which saves resources. Copying can be expensive with large data structures -- although it doesn't matter too much in this case.

### `void invert()` 

Inverts all pixels i.e. set every pixel color equal to `max_intensity - r/g/b`. **This should update the vector of pixels**. Simply loop through your vector of `Pixel`s and update the color values accordingly.

### `void flip_x()` 

Flips all pixels around the x-axis. The general idea here is to examine two rows simultaneously. So we look at every pixel in the top row, and swap each pixel with the corresponding pixel in the bottom row, and then we jump to the next set of rows after we've finished one.<br>Here is what the first 5 iterations should look like:

**Step 1**:

| T | . | . | . |
|:--|:--|:--|:--|
| . | . | . | . |
| . | . | . | . |
| B | . | . | . |

**Step 2**:

| . | T | . | . |
|:--|:--|:--|:--|
| . | . | . | . |
| . | . | . | . |
| . | B | . | . |

**Step 3**:

| . | . | T | . |
|:--|:--|:--|:--|
| . | . | . | . |
| . | . | . | . |
| . | . | B | . |

**Step 4**:

| . | . | . | T |
|:--|:--|:--|:--|
| . | . | . | . |
| . | . | . | . |
| . | . | . | B |

**Step 5**:

| . | . | . | . |
|:--|:--|:--|:--|
| T | . | . | . |
| B | . | . | . |
| . | . | . | . |

etc...

 Here is how I recommend you do this:
1. Loop through **half** of your vector.
2. Each iteration, create 2 sets of row/col variables -- one for the top row, another for the bottom row -- 4 variables in total.
3. Each iteration of the loop, calculate the **TOP** row/col based on whatever your `i` currently is.
4. Each iteration of the loop, calculate the **BOTTOM** row/col based on whatever your `i` currently is, BUT start from the **first** index of the **last** row.
5. Use `get_pixel(topRow, topCol)` to get the `topPixel`, and `get_pixel(botRow, botCol)` to get the `botPixel`. Then use `set_pixel(topRow, topCol, botPixel)` to set the top pixel equal to the bottom pixel, and `set_pixel(botRow, botCol, topPixel)` to set the bottom pixel equal to the top pixel. This effectively swaps the two pixels. 

### `void flip_y()`

This will be almost exactly like your `flip_x()` method, except 2 differences. Replace the **top/bottom** concept with **left/right**, AND every time you iterate, you'll be jumping to the next row in the same column (or the next column if you've reached the end of a column).<br><br>So imagining we have a picture of width 4 and height 4, then to loop through a full column (let's say it's the first column in the picture in this case so `i = 0` at first), you'd look at the pixel at index `i`, `i+4`, `i+4*2`, and `i+4*3`. That's 4 rows we looked at, but all in the same column.<br>Here's what the first 5 iterations should look like, imagine we're swapping the pixels at `L` & `R` every time.

**Step 1**:

| L | . | . | R |
|:--|:--|:--|:--|
| . | . | . | . |
| . | . | . | . |
| . | . | . | . | 

**Step 2**:

| . | . | . | . |
|:--|:--|:--|:--|
| L | . | . | R |
| . | . | . | . |
| . | . | . | . | 

**Step 3**:

| . | . | . | . |
|:--|:--|:--|:--|
| . | . | . | . |
| L | . | . | R |
| . | . | . | . | 

**Step 4**:

| . | . | . | . |
|:--|:--|:--|:--|
| . | . | . | . |
| . | . | . | . |
| L | . | . | R | 

**Step 5**:

| . | L | R | . |
|:--|:--|:--|:--|
| . | . | . | . |
| . | . | . | . |
| . | . | . | . | 

etc...

### `bool read_input(istream& in)` 

A lot of students seem to be confused on what is going on with `read_input()` here, so I will try to explain it to the best of my ability. All this method does it read the input of **either** a `.ppm` *file* **OR** `stdin`. Recall that if you are **reading input from a file**, you will use `ifstream`, which we commonly name `fin`. If you're reading input from `stdin`, then you will use `cin`. Now, pay attention: **Both `fin` and `cin` are stream objects**. Classes can have hierarchies and therefore can inheret attributes from other classes if we build them that way. `fin` and `cin` both inherit from the `istream` class, as they are both *input streams*. All this means is that when you call `read_input([INSERT ISTREAM OBJECT HERE])`, you can call `read_input(cin)` OR `read_input(fin)`, because both `cin` and `fin` are `istream` objects. Once you're actually *inside* the method, whether you used `cin` or `fin` as your argument, you'll refer to it as `in` (or whatever you've named the `istream` parameter) in the actual method itself. You can use `in` the same way you'd use `fin` or `cin`. The caveat here is you need to decide whether or not you'll be using `fin` or `cin` based on the commandline arguments for your program.<br><br>The program will be run like this `./ppm [in.ppm | -] [out.ppm | -]`. Where you see a `|`, you choose either of the operands preceding/following the `|` for that argument. So you can run your program like `./ppm file.ppm newfile.ppm` or `./ppm - -` or `./ppm bee.ppm -` or `./ppm - poo.ppm`.<br>The 1st argument is the file we're reading from. IF that argument is just `-`, then we're reading from `stdin`<br>The 2nd argument is the file we're outputting to. IF that argument is just `-` then we're writing to `stdout`.<br>Simply put, in `main()`, you'll want to check the arguments and decide based off of them whether or not you will be using `cin` to read or `fin` to read. If the 1st argument is a `-`. then you'll be using `cin`, which means you will simply call `read_line(cin)`. Otherwise, you'll **attempt** to open the file with `fin`. If the file is open, then you will call `read_line(fin)`, where you'll read input normally. (make sure to close your file after you're finished with your `read_line` call though).<br><br>Since the rgb values can be on separate lines or broken up in various ways, AND comments can be littered throughout the file, I've devised a kinda weird way of reading the input. The method will remove all lines with # (comments) and will build a single string using `stringstream` and `getline`. Then from that single string that's inside your `stringstream`, you'll be able to extract everything from it like you would with `cin` or `fin`. The following pseudocode is in Python, so the syntax will not be the same when you're trying to translate this to C++. Stringstreams don't even exist in Python. This is just to show you what the flow of this method should look like in your program. You should be able to convert it to C++ yourself.

<pre><code class="language-python"># create string to hold line
# create stringstream to build our completed string
    
# read every LINE from input using getline
# SKIP any line that starts with #
# build a string using your ss (ss << blah) and the line you just read 
# (remember to append a space at the end of each string)
while readline(inputstream, linebuffer):
  if linebuffer[0] == '#':
    continue
  else:
  # add our line to stringstream & append ' '

# extract the header, width, height, and max intensity separately -- from the stringstream

# make sure the header is P3, otherwise return false

# create placeholder Pixel object
Pixel px

# extract every r, g, b value from your ss and into the respective Pixel fields
# for your px object
# then add that object to your vector of Pixels
for r, g, b in stringstream:
  px.r = r
  px.g = g
  px.b = b
  pixels.add(px)

return True
</code></pre>
