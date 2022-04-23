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

This will be almost exactly like your `flip_x()` method, except 2 differences. Replace the **top/bottom** concept with **left/right**, AND every time you iterate, you'll be jumping to the next row in the same column (or the next column if you've reached the end of a column).<br><br>So imagining we have a picture of width 10 and height 4, then to loop through a full column (let's say it's the first column in the picture in this case so `i = 0` at first), you'd look at the pixel at index `i`, `i+10`, `i+10*2`, and `i+10*3`. That's 4 rows we looked at, but all in the same column.<br>Here's what the first 5 iterations should look like, imagine we're swapping the pixels at `L` & `R` every time.

**Step 1**:

| L | . | . | . | . | . | . | . | . | R |
|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|
| . | . | . | . | . | . | . | . | . | . |
| . | . | . | . | . | . | . | . | . | . |
| . | . | . | . | . | . | . | . | . | . |

**Step 2**:

| . | . | . | . | . | . | . | . | . | . |
|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|
| L | . | . | . | . | . | . | . | . | R |
| . | . | . | . | . | . | . | . | . | . |
| . | . | . | . | . | . | . | . | . | . |

**Step 3**:

| . | . | . | . | . | . | . | . | . | . |
|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|
| . | . | . | . | . | . | . | . | . | . |
| L | . | . | . | . | . | . | . | . | R |
| . | . | . | . | . | . | . | . | . | . |

**Step 4**:

| . | . | . | . | . | . | . | . | . | . |
|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|
| . | . | . | . | . | . | . | . | . | . |
| . | . | . | . | . | . | . | . | . | . |
| L | . | . | . | . | . | . | . | . | R |

**Step 5**:

| . | L | . | . | . | . | . | . | R | . |
|:--|:--|:--|:--|:--|:--|:--|:--|:--|:--|
| . | . | . | . | . | . | . | . | . | . |
| . | . | . | . | . | . | . | . | . | . |
| . | . | . | . | . | . | . | . | . | . |
etc...

### `bool read_input(istream& is)` 

This method will read from either a file using `fstream` or it will read from `stdin` (`cin`) based on the command line args provided to your program. Remember command line args are shows in `int main(int argc, char** argv)` where `argc` is the amount of arguments passed while `argv` is the array that holds the C-style strings for these arguments. so `./ppm file.ppm` will have `argc = 2` and `argv[0] = "./ppm"` and `argv[1] = "file.ppm"`.  For this program, if the first argument is `"-"`, then you'll want to read from `stdin`. Otherwise it should be something like `file.ppm`, in which case you'll open it using `fstream`. That's why for `read_input()`, we're using the paramater type `istream`, which is a general purpose type for input streams. `fin` and `cin` are both types of `istream`, so either of those can be passed to `read_input()` and will still work.<br><br>

This method has you assign the values you've read to each index in your vector of pixels. Basically, you'll want to do the following:

1. (BEFORE calling `read_input()`) determine whether or not you'll use `fstream` or `cin`
2. if you're using `fstream`, open the file first, then call `read_input(fin)`. If you're using `cin`, then just call `read_input(cin)`
3. **before reading anything, always make sure to skip lines that begin with `#`**
4. Read header (`P3`), return `false` if the header read isn't valid
5. Read `width` and `height`
6. Read `max intensity`
7. Now you'll read every rgb value. The way I recommend to do this is set up a for loop that loops as many times as there are pixels (i.e. `width * height`). In that for loop, read the entire line using `getline(is, line)`. This reads a line from the stream `is` to the `string line`. 
8. Check if `line[0]` is `'#'`, (that's a `char`, not a `string`) and if it is then you should skip that iteration, BUT make sure you decrement `i` when you skip, because otherwise we'd have empty spaces in our vector.
9. If `line[0]` is not `'#'`, then we can parse our `line` using the `stringstream` we made with it. Read the `r` `g` and `b` values from this line and create a `Pixel` object with these rgb values.
10. Pushback onto your vector the newly created `Pixel`.
11. Do this for every pixel you have to read.
