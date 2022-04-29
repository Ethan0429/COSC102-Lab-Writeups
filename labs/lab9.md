---
layout: default
title: Lab 9 - Multi-User Dungeons
nav_order: 6
---

# Status: <font color="green">Completed</font> | <font color="blue">Recently Updated</font>
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

# Lab 9 - Multi-User Dungeons
{: .no_toc }

## Table of contents
{: .no_toc }
- TOC
{:toc}

# Overview

This lab will be a text-based maze-ish game that works by generating "dungeons" based off of a file containing a variable amount of room information. The funky part of this lab is that it'll most likely be your first time using **pointers**! So strap in because this stuff sucks, and if you think you know pointers I would bet money you don't. (I don't know if I even know them)

# My Recommendations ❗ Read This ❗

I highly recommend you do this lab with a class. It will make it a lot easier than if you're just trying to do functional programming. I'll give some vague descriptions about what I think your class would look like, but I think this is a good time to learn how to start structuring them yourselves because you'll definitely have to later.

## General Structure

In general, my code was comprised of these things:

  - Class for the whole dungeon. This would encompass rooms and methods to interact with those rooms, as well as the room that the player is currently in.
  - Struct for rooms. This would hold information pertaining to a single room, including the exits.
  - Struct for exits. This would simply bundle what comprises an exit (direction e.g. `w` for west, and room# e.g. `2`) for ease of storage/access.
  - Methods that are part of your dungeon class. These should let allow you to execute some kind of command.
  - Loop in `main` that allows the user to call your class's methods to play the game.
  
The hard part about the class, the rooms, and exits in particular is that they will *all* need to be dynamically allocated.

## Tools You Should Use

As part of my recommendation, these are the tools I think you should use:

- `getline()` - You can use this in many ways, so get created. There are overloaded versions of it as well that give you more control over how it behaves.
- `stringstream` - Makes things a bit easier when parsing exits, in my code at least.
- `ignore()` - Useful for when you've read as much data as you want, and then need to ignore everything up to a certain character or just ignore the next character.
- `clear()` - Just throwing this out there, **you'll almost certainly use this at least once**.
- `string` - This might seem obvious, you'll have to use it in the lab anyway of course. But remember that a `string` is BASICALLY just a `vector` but all of its elements are `char`s. It provides the same methods your familiar with from `vector` like `push_back()` and `pop_back()` - two things I used in my code.
- `seekg()`/`tellg()` - these two methods belong to `fstream`. **You'll have to use both at least once**.
- Google - Not even joking. If you don't understand how something works like `getline`, `ignore`, `clear`, Google is your friend. Google until your fingers are bleeding, because I swear it will solve most of your problems. I would bet 99% of the stuff I know in terms of programming is thanks to Google. Don't know what `seekg()` does? GOOGLE IT. You'll see exactly what it does when you do that. Once you've learned what it does, you might be a bit confused on how to use it for your particular case. Let's say you learn that `seekg()` allows you to move the file pointer for a currently open file to a certain position. (So in english, it lets you move around in your file.) You know that now, but how do I move to the beginning of the file? GOOGLE IT. (unless you learned it already in which case ❤️)

# Restrictions

Not to beat a dead horse here, but the restrictions are very important for this lab so consider the horse abused -- **Any container or object (instantiation of a class/struct) will be dynamically allocated**. You're not allowed to use objects allocated on the stack. You must use pointers in this lab, which will be difficult because you've never even touched them before most likely. There's a lot of rules to pointers, so don't be afraid to ask questions because we'll answer all of them!

# Pointers, `new`, `delete`, oh my!

Okay now that you're working with pointers you are a bonified big boy/girl, so put on your big boy/girl pants because you're gonna need them. Some of this/all of this I'm sure you've already learned, but I'm gonna explain it here in my own words so it's 1. accessible and 2. potentially more understandable for some. You can never have too many resources.


## Pointers

Pointers are really pretty simple at the core. They are *pointers* (really?) to an address in memory. Every thing you declare in your program has a memory address associated with it, which is where it really lives in your computer. When you do something like `int a = 10`, your computer goes to what's called the stack, which is just a large space of memory that's reserved specifically for your program, and inserts your variable `a` at an address on the stack. Since it's an `int` that address will take up 4 bytes in C++. To create a pointer you use the following syntax:

<pre><code class="language-cpp">int* b; // you can also do int *a</code></pre>

This creates a pointer, denoted by the `*` to an `int`. So that means that our tag `b` will point to an address in memory that holds an `int`. Currently, it points to nothing, so its value is `NULL` or `nullptr` in C++. If you wanted to point it to something, you would do something like

<pre><code class="language-cpp">int a = 10;
int* b = a;</code></pre>

This creates a variable on the stack `a`. It holds the value `10`. Then we create a pointer to an int, `int*` with the name `b`. `b` now points to `a`, which means that the value of `b` is the address of `a`. Now we can **dereference** `b`, which will give us the value that lies at the address it is pointing to.

<pre><code class="language-cpp">int a = 10;
int* b = a;
cout << *b; // prints 10</code></pre>

As you can see, you dereference a pointer by adding an `*` in front of its name.<br><br>Seems kinda useless in this context right? Well yeah, but hopefully you get the point here. There's a lot more depth to it, but this will suffice for now. A more common use-case for pointers you'll see is in conjunction with the `new` operator.

## `new`/`new[]`

If you're coming from 101, then you kinda now about the `new` operator. It's quite a bit different in C++, but they kinda signal the same thing. When you use the `new` operator, you're **dynamically allocating** memory for something. This means that your program did not have the memory for this before, but now you've given it the memory to do so. It's used like so:

<pre><code class="language-cpp">int* a = new int; // creates a new block of memory that can hold an int
int* b = new int[10] // creates a new block of memory that can hold 10 ints
string* s = new string[2] // creates a new block of memory that can hold 2 strings</code></pre>

Note that `new` and `new[]` are a bit different. `new` creates a single object `new[n]` creates an array of `n` objects. You're probably wondering why we would want to do this. Well there's a couple reasons:
1. Objects created with `new` are not confined to a single scope. An object created without `new` will be limited to its scope. Once that scope is left, the object is destroyed. Objects created with `new` are destroyed either when we destroy them ourselves, or when our program terminates.
2. Now we can dynamically create space for something. If you wanted to create an array for some *variable* amount of `int`s for example, you couldn't do it without `new`. Creating an array is a compile-time task, meaning the size of the array has to be known before it even gets run. But with `new`, you can create an array on the spot that will have a size we determined during runtime. (Like how you'll use `new` to dynamically allocate an array with enough size to hold the number of rooms)<br><br>I won't try to bog you down with more details, so just know that stuff for now.

## `delete`/`delete[]`

Just like it sounds, where `new` allocates memory, `delete` **de**-allocates memory. It's pretty self explanatory

<pre><code class="language-cpp">int* a = new int;
int* b = new int[10];

delete a;
delete[] b;</code></pre>

First we allocated memory for `a` and `b`, then we deallocated memory for `a` and `b`. **As a rule of thumb, never `delete/delete[]` something that's not been created with `new/new[]`. Consequently, every `new/new[]` should eventually have a corresponding `delete/delete[]`**.

# Steps

Here I'm gonna write a general outline for how you should approach writing the program. **I recommend following them in the order they are written**. Good luck!

## Reading The Rooms

The first thing you should check off is reading in the rooms. There's no requirement for you to use a `read_input()` function or anything, so approach it the way you feel best. I did it by reading in the filename from the command line args, and then I passed that name to my read function which opened the file and did all the reading. You could open it in `main` like in lab 8 and pass an `ifstream` to your read function, or you could just do all your reading in `main`. It's up to you!<br><br>This will probably be the hardest part of the lab, because this is where *all* of your dynamic allocations should take place. Like the writeup says, you'll need to pass through the entire file once to get a count for how many rooms there will be, and then you'll allocate an array to hold those rooms. The exits for each room are variable too, so you'll also be dynamically allocating them in some way (I chose to make a list of `Exit` structs). You an do this when you first pass through the file, or you can do this when you're actually reading the exits. I chose to do the latter.<br><br>

The goal of this section is to be able to print a formatted output of the rooms. (This is not part of the lab, just a checklist) Once you can do that, you're in good shape to move onto the next step. Here's what my formatted output looks like after reading the room1 file:

<pre><code class="language-plaintext">Title: Room #0 - @ index 0
Desc: You are at the start. Your journey begins..
Exits: 
s 5

Title: Room #1 - @ index 1
Desc: You see a portrait of Dr. Marz hanging on the wall. What could it mean?
Exits: 
s 6

Title: Room #2 - @ index 2
Desc: There seem to be some animal droppings in the corner. Gross. 
Exits: 
e 3

Title: Room #3 - @ index 3
Desc: A dirty hall with cockroaches crawling around. Is this a student apartment complex?
Exits: 
w 2
e 4

Title: Room #4 - @ index 4
Desc: this is room 4s description
Exits: 
w 3
s 9

Title: Room #5 - @ index 5
Desc: You're near the start.
Exits: 
e 6
n 0

Title: Room #6 - @ index 6
Desc: There are many passageways here.
It would be easy to get lost...
Exits: 
s 11
w 5
e 7
n 1

Title: Room #7 - @ index 7
Desc: You hear a faint buzzing noise
Exits: 
e 8
w 6

Title: Room #8 - @ index 8
Desc: Have I been here before?
Exits: 
e 9
w 7

Title: Room #9 - @ index 9
Desc: Nothing remarkable.
Exits: 
w 8
n 4

Title: Room #10 - @ index 10
Desc: There's a mural of WWII propaganda on the wall. 
Exits: 
s 15

Title: Room #11 - @ index 11
Desc: Scrawled in blood on the wall is a message:
"post on piazza instead of emailing your ta"
what could it mean?
Exits: 
n 6
e 12

Title: Room #12 - @ index 12
Desc: A three way junction.
Exits: 
s 17
w 11
e 13

Title: Room #13 - @ index 13
Desc: There's bread crumbs on the floor. 
Exits: 
e 14
s 18
w 12

Title: Room #14 - @ index 14
Desc: There's a panel on the ceiling but you 
can't quite reach it. 
Exits: 
w 13
s 19

Title: Room #15 - @ index 15
Desc: Sitting on a desk is a bowl of soup and some tea
Exits: 
e 16
n 10

Title: Room #16 - @ index 16
Desc: There's a collection of rusty old road signs
Exits: 
w 15
e 17

Title: Room #17 - @ index 17
Desc: Someone has been here before you. There are footprints and signs of a struggle. 
Exits: 
n 12
w 16
s 22

Title: Room #18 - @ index 18
Desc: There's a trail of crumbs heading south..
Exits: 
s 23
n 13

Title: Room #19 - @ index 19
Desc: It's empty
Exits: 
n 14

Title: Room #20 - @ index 20
Desc: There's a pile of old newspapers in the corner. 
Exits: 
e 21

Title: Room #21 - @ index 21
Desc: You get the feeling you're closer to the exit now. 
Exits: 
e 22
w 20

Title: Room #22 - @ index 22
Desc: There's a wooden chair leaned against the wall. 
Exits: 
w 21
n 17

Title: Room #23 - @ index 23
Desc: There's a draft in here. 
Exits: 
n 18
e 24

Title: Room #24 - @ index 24
Desc: Freedom!!! A door opens to the outside. 
Exits: 
w 23
</code></pre>

It prints the title of every room, the description, and all of the exits on separate lines. If you can print all of this from what you've read from the file (after you've read everything in), then you've done it correctly (probably)

## Player Options

Now that you've completed the reading portion, you should have an object that contains all of the information we got from it. From there, you should provide methods that allow the player to interact with the rooms such as

- Look - prints the title, description and exit directions.
- Move - moves the player to a different room based on the direction key entered
- Quit - quits your program. This method should deallocate any memory that was allocated.

---

# ❤️

This was your last lab for this class. I'll miss TA'ing for you all and writing these. Good luck to those of you who are continuing the CS curriculum next year! (and anyone for that matter I guess) Feel free to DM me wherever you want, as I'll most likely answer no matter what :)

❤️❤️❤️ <br>
\- Ethan 