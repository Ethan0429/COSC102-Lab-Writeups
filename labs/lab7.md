---
layout: default
title: Lab 8 - BITSET
nav_order: 4
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

<details>
<summary>
<b><font color="maroon">click to view change-log</font></b>
</summary>

  <div markdown="1">

`Wed, 26 Mar 2022 01:29:19 EST`
  - added [Overview](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab8.html#overview) section
  - added [Background](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab8.html#background) section
  - added [Bits & Bytes](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab8.html#bits--bytes) section
  - added [Bitwise Operators](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab8.html#bitwise-operators) section
  - added [Examples](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab8.html#examples) section

  </div>
</details>
<hr>

# Lab 8 - BITSET
{: .no_toc }

## Table of contents
{: .no_toc }
- TOC
{:toc}

## Overview

In this lab, you'll develop a program that manipulates **bits** of integers belonging to a `vector`. You should already know what bits are, vaguely at least, but I will cover them in depth in this writeup to help you with the lab.

## Background

You can skip this section if you think you know what you're doing in terms of bits. But I HIGHLY recommend reading through it all thoroughly -- especially if you'll be taking 230 next year, because you'll thank me for having prepared you for it with the following information. Otherwise, skip to the [Bitwise Operators](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab8.html#bitwise-operators) section and/or [Examples](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab8.html#examples) for a briefer explanation if you're confident.

### Bits & Bytes
  
Every data type in your computer is stored in memory somehow. Where they're stored is not exactly important right now, but *how they're stored* is. Each data type has a specific amount of **bytes** that it takes up. A single **byte** is a unit comprised of 8 **bits**. Similar to how a meter is 100 centimeters, a byte is 8 bits. Bits are the lowest level unit in computing, and can be either `0` or `1`.<br><br>A `char` for example is 1 byte. That means it is 8 bits. So how do we represent a single char using bits? Well, binary (the "language" that uses bits) is in **base-2**. The decimal system for example is in **base-10**. I'm not going to get too much into it, but just pay attention to this next part.

<pre><code class="language-plaintext">0000 0000</code></pre>

The sequence of bits above is 8 bits in total (usually we'll separate bits by every fourth bit just to make it cleaner) which means in total it makes up some form of data comprising one byte. A `char` is one byte, so it could be represented by the above bits. If we chose to represent a `char` using the bits above, it would be equal to the following `\0`. This is known as the null terminating character. For `char`'s, there is a table that maps a specific decimal value to it's "letter" representation. This is known as an [ASCII table](https://www.asciitable.com/). If all bits in a byte are 0, then its decimal equivalent is just 0. So looking at the ASCII table I just linked, we can see what 0 maps to and then we will understand why the `char` is `\0`. This is why a `char` is one byte. The range of a typical ASCII table is `[0, 127]` inclusive. If all 8 bits are **set** (more on this later), then the bits would look like

<pre><code class="language-plaintext">1111 1111</code></pre>

  In this case, the decimal equivalent of those bits would be 255. A `char` can only hold one byte of data, so the highest value it can contain is 255, right? Well actually, most ASCII characters are **unsigned**, which means they do not include negative numbers. A `char` in C++ is implicitly signed. This means it can hold 127 **negative** values, and 127 **positive** values. So now you should see that's where we get the range `[0, 127]`<br><br>All of this to say that you should understand every data type is just a certain amount of bytes, which is just a certain amount of sets of 8 bits. An `int` is no different from a `char` in terms of *what* it stores at the binary level. The only difference between them to your computer is the amount of bytes each can hold. It's then up to the programming language to create rules that dictate what data types are comprised of and how they map into our reality. (e.g. `char` is comprised of 1 byte and it maps to letters, `int` is 4 bytes and it maps to integer values)<br><br>In this lab, you'll be working primarily with `int`'s so let's discuss that. Here are some sequences of bits representing `int`'s and their decimal equivalent

<pre><code class="language-plaintext">0000 0000 0000 0000 0000 0000 0000 0000 = 0 in dec</code></pre>
  
<pre><code class="language-plaintext">0000 0000 0000 0000 0000 0000 0000 0001 = 1 in dec</code></pre>

<pre><code class="language-plaintext">0000 0000 0000 0000 0000 0000 0000 0010 = 2 in dec</code></pre>

<pre><code class="language-plaintext">0000 0000 0000 0000 0000 0000 0001 0000 = 16 in dec</code></pre>

  The pattern here is 2 to the power of the **set** bit's index. A **set** bit is just a bit that is 1 instead of 0. That will give you the decimal equivalent of the binary representation. If there are more than 1 set bits, then you just need to calculate that bit's value individually and then add up all of the values. So if you have `int == 3`, it would look like

<pre><code class="language-plaintext">0000 0000 0000 0000 0000 0000 0000 0011</code></pre>
  The first bit's index is 0, and it is set. So `2^0 == 1`. Then the second bit is also set, and its index is 1. So `2^1` is `2`. `2 + 1 == 3`, which is how we calculate the value.

### Bitwise Operators
  
The task you've been given for this lab is to manipulate the bits of integers using **bitwise operators**. Bitwise operators are similar to regular operators like +,-,*,/, etc, except they work at the bit level and have slightly different rules.<br><br>Here are the following bitwise operators you'll be working with for C++

<pre><code class="language-plaintext">&  - bitwise AND operator
|  - bitwise OR operator
>> - bitshift right operator
<< - bitshift left operator
</code></pre>

Ignoring what these do for now, similar to an expression `a + 3` in C++, `a & 3` does not modify `a`. It hasn't been stored anywhere. You'd have to do `a += 3` to actually modify `a` itself. It's the same with bitwise operators `a &= 3`.

#### Operator Features

- `&` -- `left & right` where right's binary representation is **tested** against left's binary representation. Returns `1` if both bits are `1`, returns `0` otherwise.<br><br>
- `|` -- `left | right` where right's binary representation is **tested** against left's binary representation. Return `1` if either bit is `1`, returns `0` if both bits are `0`.<br><br>
- `>>` -- `a >> amnt` shift `a`'s bits to the right `amnt` times. However many times a number is shifted, that amount of bits gets ejected from the sequence to the right, and that same amount of bits gets inserted to the left of the sequence as 0's.<br><br>
- `<<` -- `a << amnt` shift `a`'s bits to the left `amnt` times. However many times a number is shifted, that amount of bits gets ejected from the sequence to the left, and that same amount of bits gets inserted to the right of the sequence as 0's.

### Bitmasking

  Before showing you some examples, you should understand the concept of **bitmasking**. All the bitwise operators I've shown you so far (except for the shift ones) are applied to every bit within a given data type. With the following code

  <pre><code class="language-cpp">int a = 8;
int b = 2;
int result = a & b;
// result == 0</code></pre>

All of the bits in `a` are tested against all the bits in `b`. Their bits look like

<pre><code class="language-plaintext">a == 0000 0000 0000 0000 0000 0000 0000 1000</code></pre>
<pre><code class="language-plaintext">b == 0000 0000 0000 0000 0000 0000 0000 0010</code></pre>

And the result of `a & b` looks like
<pre><code class="language-plaintext">result == 0000 0000 0000 0000 0000 0000 0000 0000</code></pre>

Each bit index of `a` is `&`'d against each corresponding bit index of `b`. You'll notice that none of the bits from `a` line up with `b` so that it ever does `1 & 1`, therefore the result for every bit in `a & b` is 0, and so the final value of `result` is 0.<br><br>As you can see from the example above, there's no real purpose in just ANDing two random values. This is where **bitmasks** come in. A bit mask is just a number that we'll use for specific purposes when using bitwise operatations against a specific value. So in the example above, instead of using `2` for `b`, we would replace `b` with a value that we intend to AND against be for a specific purpose. So if we wanted to test whether or not the 4th bit of `a` was set for example, then we should set `b` equal to a value that will return some non-zero number when using AND between `a` and `b`. So making `b` a "bitmask" to do this, we just do

<pre><code class="language-plaintext">a == 0000 0000 0000 0000 0000 0000 0000 1000</code></pre>
<pre><code class="language-plaintext">b == 0000 0000 0000 0000 0000 0000 0000 1000</code></pre>

Now when we do `a & b`, the result will be `8`. The example is a bit contrived, but hopefully that shows what a bitmask is and why you would want to use one. You curate a bitmask for whatever problem you're trying to solve, and then you test that bitmask against a specific value you're analyzing using any one of the bitwise operations that makes sense.
      
### Examples

I'm going to show some examples here so you understand what's happening at both the bit and the decimal level using each bitwise operator listed above in C++. (You can edit the following snippets and their inputs however you want and run the code yourself **in the browser** by clicking the edit button the in the bottom right of the compiler window if you're curious to see how it changes)

- #### AND Operator
  
  <iframe width="800px" height="200px" src="https://godbolt.org/e?readOnly=true&hideEditorToolbars=true#g:!((g:!((g:!((h:codeEditor,i:(filename:'1',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,selection:(endColumn:20,endLineNumber:14,positionColumn:20,positionLineNumber:14,selectionStartColumn:20,selectionStartLineNumber:14,startColumn:20,startLineNumber:14),source:'%23include+%3Ciostream%3E%0Ausing+namespace+std%3B%0A%0Aint+main()+%7B%0A++++/*+program+will+%22test%22+the+5th+bit+of+a+number+and+return%0A++++true+if+that+bit+is+set,+or+false+if+not+*/%0A%0A++++int+a%3B%0A++++cin+%3E%3E+a%3B%0A++++//+we+give+input+17+for+a...%0A%0A++++int+b+%3D+16%3B+//+1+0000+in+binary+(omitting+preceding+0!'s)%0A%0A++++printf(%22a:+%25d,+b:+%25d%5Cn%22,+a,+b)%3B%0A%0A++++if+(a+%26+b)+%7B%0A++++++++cout+%3C%3C+%225th+bit+is+set!!%5Cn%22%3B%0A++++%7D%0A++++else+%7B%0A++++++++cout+%3C%3C+%225th+bit+is+not+set!!%5Cn%22%3B%0A++++%7D%0A++++return+0%3B%0A%7D'),l:'5',n:'0',o:'C%2B%2B+source+%231',t:'0')),k:50,l:'4',n:'0',o:'',s:0,t:'0'),(g:!((h:executor,i:(argsPanelShown:'1',compilationPanelShown:'1',compiler:g112,compilerOutShown:'0',execArgs:'',execStdin:'17',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,libs:!(),options:'',source:1,stdinPanelShown:'0',tree:'1',wrap:'1'),l:'5',n:'0',o:'Executor+x86-64+gcc+11.2+(C%2B%2B,+Editor+%231)',t:'0')),header:(),k:50,l:'4',n:'0',o:'',s:0,t:'0')),l:'2',n:'0',o:'',t:'0')),version:4"></iframe>

- #### OR Operator

  Notice that the result does not change for any input to `a` less than 255. It compares every set bit from `b` (255 is `1111 1111`, so 8 bits of it are set) and then OR's that with whatever `a` is.
  <iframe width="800px" height="200px" src="https://godbolt.org/e?readOnly=true&hideEditorToolbars=true#g:!((g:!((g:!((h:codeEditor,i:(filename:'1',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,selection:(endColumn:2,endLineNumber:16,positionColumn:2,positionLineNumber:16,selectionStartColumn:2,selectionStartLineNumber:16,startColumn:2,startLineNumber:16),source:'%23include+%3Ciostream%3E%0Ausing+namespace+std%3B%0A%0Aint+main()+%7B%0A++++/*+program+will+%22set%22+the+first+8+bits+and+print+the+%0A++++resulting+integer+*/%0A%0A++++int+a%3B%0A++++cin+%3E%3E+a%3B+//+we+give+input+13+for+a...%0A++++int+b+%3D+255%3B%0A%0A++++int+result+%3D+a+%7C+b%3B%0A%0A++++printf(%22result:+%25d%22,+result)%3B%0A++++return+0%3B%0A%7D'),l:'5',n:'0',o:'C%2B%2B+source+%231',t:'0')),k:50,l:'4',n:'0',o:'',s:0,t:'0'),(g:!((h:executor,i:(argsPanelShown:'1',compilationPanelShown:'1',compiler:g112,compilerOutShown:'0',execArgs:'',execStdin:'13',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,libs:!(),options:'',source:1,stdinPanelShown:'0',tree:'1',wrap:'1'),l:'5',n:'0',o:'Executor+x86-64+gcc+11.2+(C%2B%2B,+Editor+%231)',t:'0')),header:(),k:50,l:'4',n:'0',o:'',s:0,t:'0')),l:'2',n:'0',o:'',t:'0')),version:4"></iframe>

- #### Bitshift Right Operator
  
  Notice that how ever many times an integer is shifted right, the resulting integer is `original_number / 2 * shiftamnt`. If the resulting integer is something like 1.6, the decimal is "truncated", (rounded down basically), so `17 >> 1` would just be 8.

  <iframe width="800px" height="200px" src="https://godbolt.org/e?readOnly=true&hideEditorToolbars=true#g:!((g:!((g:!((h:codeEditor,i:(filename:'1',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,selection:(endColumn:58,endLineNumber:10,positionColumn:58,positionLineNumber:10,selectionStartColumn:58,selectionStartLineNumber:10,startColumn:58,startLineNumber:10),source:'%23include+%3Ciostream%3E%0Ausing+namespace+std%3B%0A%0Aint+main()+%7B%0A++++/*+program+will+shift+an+integer+right+by+however%0A++++many+times+specified+by+input+*/%0A%0A++++int+a,+shiftamnt%3B%0A++++cin+%3E%3E+a%3B+//+we+give+input+18+for+a...%0A++++cin+%3E%3E+shiftamnt%3B+//+we+give+input+2+for+shiftamnt...%0A%0A++++int+result+%3D+a+%3E%3E+shiftamnt%3B%0A++++printf(%22result:+%25d%22,+result)%3B%0A++++return+0%3B%0A%7D'),l:'5',n:'0',o:'C%2B%2B+source+%231',t:'0')),k:50,l:'4',n:'0',o:'',s:0,t:'0'),(g:!((h:executor,i:(argsPanelShown:'1',compilationPanelShown:'1',compiler:g112,compilerOutShown:'0',execArgs:'',execStdin:'18%0A2',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,libs:!(),options:'',source:1,stdinPanelShown:'0',tree:'1',wrap:'1'),l:'5',n:'0',o:'Executor+x86-64+gcc+11.2+(C%2B%2B,+Editor+%231)',t:'0')),header:(),k:50,l:'4',n:'0',o:'',s:0,t:'0')),l:'2',n:'0',o:'',t:'0')),version:4"></iframe>

- #### Bitshift Left Operator
  
  The behavior for this is the same as bitshift right, but instead the resulting integer in this case is `original_number * 2 * shiftamnt`.

  <iframe width="800px" height="200px" src="https://godbolt.org/e?readOnly=true&hideEditorToolbars=true#g:!((g:!((g:!((h:codeEditor,i:(filename:'1',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,selection:(endColumn:41,endLineNumber:10,positionColumn:41,positionLineNumber:10,selectionStartColumn:41,selectionStartLineNumber:10,startColumn:41,startLineNumber:10),source:'%23include+%3Ciostream%3E%0Ausing+namespace+std%3B%0A%0Aint+main()+%7B%0A++++/*+program+will+shift+an+integer+left+by+however%0A++++many+times+specified+by+input+*/%0A%0A++++int+a,+shiftamnt%3B%0A++++cin+%3E%3E+a%3B+//+we+give+input+7+for+a...%0A++++cin+%3E%3E+shiftamnt%3B+//+we+give+input+1+for+shiftamnt...%0A%0A++++int+result+%3D+a+%3C%3C+shiftamnt%3B%0A++++printf(%22result:+%25d%22,+result)%3B%0A++++return+0%3B%0A%7D'),l:'5',n:'0',o:'C%2B%2B+source+%231',t:'0')),k:50,l:'4',n:'0',o:'',s:0,t:'0'),(g:!((h:executor,i:(argsPanelShown:'1',compilationPanelShown:'1',compiler:g112,compilerOutShown:'0',execArgs:'',execStdin:'7%0A1',fontScale:14,fontUsePx:'0',j:1,lang:c%2B%2B,libs:!(),options:'',source:1,stdinPanelShown:'0',tree:'1',wrap:'1'),l:'5',n:'0',o:'Executor+x86-64+gcc+11.2+(C%2B%2B,+Editor+%231)',t:'0')),header:(),k:50,l:'4',n:'0',o:'',s:0,t:'0')),l:'2',n:'0',o:'',t:'0')),version:4"></iframe>

Hopefully all of this gives you an idea of how to use each operator and what their functions are.<br><br>**NOTE** that the examples above are a bit barebones so you can think for yourself during the actual lab. (hint: you'll be using a combination of a bitmask, shifting, and `&`/`|` for most of the lab)
