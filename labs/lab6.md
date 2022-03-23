---
layout: default
title: Lab 6 - Selection Sort
nav_order: 3
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

<details>
<summary>
<b><font color="maroon">click to view change-log</font></b>
</summary>

  <div markdown="1">

`Wed, 23 Mar 2022 15:51:03 EST`
  - added [Overview](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#overview) section
  - added [Your First Algoritm!](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#your-first-algorithm---selection-sort) section
  - added [How It Works](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#how-it-works) section<br><br>

`Wed, 23 Mar 2022 16:14:03 EST`
  - added [Step 1](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#step-1---file-input) & [Step 2](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#step-2---implementing-selectionsortvectorint) sections

  </div>
</details>
<hr>

# Lab 6 - Selection
{: .no_toc }

## Table of contents
{: .no_toc }
- TOC
{:toc}

## Overview
This is probably going to be the easiest lab you'll have this semester. The skeleton code is written for you, and selection sort is a very simple algorithm once you understand the concept. The basic idea for this program is to simply read a list of space-separated integers from a text file into a `vector`. Then you'll just sort that `vector` using selection sort, and print the `vector` contents. That's literally it.

## Your First Algorithm - Selection Sort!

Algorithm is a pretty arbitrary term in computer science, but I believe selection sort is really the first *real* algorithm you've learned so far. There are a ton of sorting algorithms, and selection (gonna call it **SS** from here on out cuz I'm lazy) is not a very good one, but it's good for learning!

- ### How It Works
    Selection sort works in the following steps. If you know these, you should know how to implement the algorithm:

    1. Select the first **unsorted** element in your list (any arbitrary container e.g. `vector` in this case) and set it as the minimum element.
    2. Compare the next **unsorted** element to the current min.
    3. If the next **unsorted** element is **less than** the current min, then update the min to that element.
    4. Repeat this comparison/update process until you've reached the end of the list.
    5. Swap the current min with the first unsorted element.
    6. Repeat until you've done this for every element in the list.<br><br>

    So to reiterate, all you're doing is combing through every element in a list, and comparing each element in that list to every other element in that list that's not sorted. Then yous wap an unsorted element with whatever you've determined to be a minimum element -- if one is found. Hopefully that makes sense.<br><br>Here's a visual representation of the algorithm using the same inputs provided on the Canvas lab page:
    <video controls="controls" width="800" height="600" name="Video Name">
      <source src="../img/ss.mov">
    </video><br><br>
    In the example above
    - <font color="orange">Orange</font> indicates an **already sorted element**. 
    - <font color="red">Red</font> indicates the element that is currently the min in the whole list. 
    - <font color="green">Green</font> indicates whatever element is being compared to the min (red).

## Step 1 - File Input
This lab is pretty simple, but I'll try to break it into steps since I'm sure it'll confuse some people a bit. But this is pretty contrived, so don't be surprised if the step-process seems unnecessary. I'm doing this for **you** <3.

The first thing you'll do is read a list of integers from a text file provided to you. You won't need to ask the user for a text file, and you won't need to create any text files. You'll simply read one that already exists. You'll do this in `main`, as outlined by the skeleton code provided to you via Canvas. As usual, you'll need to 1. check if a file can be opened in the first place and return from your program (no print statement just close file & exit) if there is no such file (i.e. cannot be opened), and 2. close your file when you're finished with it.

This means you'll just be using `fstream` (namely `ifstream`) to read a file, and you can use `cin` to read each number and push them onto your `vector`.

The input file format is simple, it's just

<pre><code language="language-plaintext">[num1] [num2] [num3] [num4] ...and so on</code></pre>

So an example would be 

<pre><code language="language-plaintext">1 2 3 4</code></pre>

That's it. Read the numbers in, and put them in your `vector`.

## Step 2 - Implementing `selectionSort(vector<int>)`

The next and final step is to implement the TODO section of `selectionSort(vector<int>)`. Which I've already outlined in as much detail as I can without basically cheating for you. The general flow will be the process of you iterating through your vector and determining what elements are less than your current element, and then swapping the smallest element with the current element, if one is found. If a smaller element isn't found, then that element is already sorted and you can move onto the next and repeat. That's really it!

## Bye!
That's it! Good luck :)