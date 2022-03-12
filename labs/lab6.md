---
layout: default
title: Extra Credit Lab - Blackjack
nav_order: 2
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
    <td class="tg-0lax">Information has been recently updated</td>
  </tr>
</tbody>
</table>

<details>
<summary>
<b><font color="maroon">click to view change-log</font></b>
</summary>

  <div markdown="1">

`Fri, 11 Mar 2022 22:02:49 EST`
  - added completed [Conceptual Overview](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#conceptual-overview) section<br><br>
  - added completed [Game Rules & Implementation](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#game-rules--implementation) section<br><br>
  - added completed [Output Examples](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#game-rules--implementation) section<br><br>
  - added **semi**-completed [Restrictions/Requirements](https://ethan0429.github.io/COSC102-Lab-Writeups/labs/lab6.html#game-rules--implementation) section<br><br>

  </div>
</details>
<hr>

# Lab 6 - Blackjack
{: .no_toc }

## Table of contents
{: .no_toc }
- TOC
{:toc}

## Conceptual Overview

In case it wasn't obvious from the name of the lab, the objective of this lab is to emulate a simple, text-based, slightly less complex blackjack game.

- ### Setting up

  1. There will be **exactly 2 players** in the game - the actual player and the "dealer".
  2. The deck will consist of 52 cards belonging to both the player and the dealer. (i.e. no duplicates)
  3. Each card in the deck will have both a `value` and a `suit` combination of some kind

      <pre><code class="language-plaintext">Values: Ace, 2, 3, 4, 5, 6, 7, 8, 9, 10, Jack, Queen, King
     Suits: Hearts, Diamonds, Spades, Clubs</code></pre>
        
      so a card could be Ace of Spades, 3 of Clubs, Queen of Hearts -- you get the idea.<br>

  4. Each card will have an abbreviation of

  |     | Clubs | Hearts | Diamonds | Spades |
  |:---:|:-----:|:------:|:--------:|:------:|
  | Ace |   AC  |   AH   |    AD    |   AS   |
  |  2  |   2C  |   2H   |    2D    |   2S   |
  |  3  |   3C  |   3H   |    3D    |   3S   |
  |  4  |   4C  |   4H   |    4D    |   4S   |
  |  5  |   5C  |   5H   |    5D    |   5S   |
  |  6  |   6C  |   6H   |    6D    |   6S   |
  |  7  |   7C  |   7H   |    7D    |   7S   |
  |  8  |   8C  |   8H   |    8D    |   8S   |
  |  9  |   9C  |   9H   |    9D    |   9S   | 

  *(so basically just pair the number/letter of the face with the letter of the suit)*<br><br>If you've played blackjack before, you're probably familiar with the rules already. But you'll want to read this section so you understand the specific things you'll need to implement.

- ## Game Rules & Implementation

  - Objective: 
  Get 21 points without "busting" i.e. going over 21 OR get as many points as possible under 21. (21 is just the limit)<br><br>
  - Player Abilities: 
  As a player, you can request either a **hit** or a **stand**. Hit means the dealer will give you a random card, and stand means you'll end your turn with whatever cards you currently hold. You can hit as many times as you want until you bust or stand. Once you stand, the turn is over, and the dealer plays next. **You will need to check if a user provides "valid" input**.<br><br>
  - Dealer Abilities
  The dealer will play by continuously "hitting" until it reaches a value >= 18, at which point it will stand or forcibly bust.<br><br>
  - Outcome: 
  If the player scores 21, then they automatically win the game and the dealer does not get a turn. If you get below 21, then the dealer gets a chance to play to beat your score. If the player goes over 21, then they "bust" and automatically lose without the dealer getting a turn. If the dealer gets 21, they automatically win, and if they bust they automatically lose. If both the player and dealer scores are <= 21, then their scores will be comapred and the winner will be the score with the higher value. If the dealer and player tie, then the game is a draw.<br><br>
  - Miscellaneous:
  You'll need to format your output so that all of the cards are printed in a left-justified field of 20 characters wide:
    <pre><code class="language-plaintext">Dealer has cards: KD 3S               (13)
    You have cards  : QD AS               (11)
    Hit or stand    ? hit

    Dealer has cards: KD 3S               (13)
    You have cards  : QD AS 6D            (17)
    Hit or stand    ? stand

    Dealer hits     : KD 3S KS            (23)
    Dealer busts, player wins!</code></pre>

## Output Examples
Here are the example outputs to reference for your own program's correctness

  ### Example 1 - dealer bust

  <pre><code class="language-plaintext">Dealer has cards: KD 3S               (13)
You have cards  : QD AS               (11)
Hit or stand    ? hit

Dealer has cards: KD 3S               (13)
You have cards  : QD AS 6D            (17)
Hit or stand    ? stand

Dealer hits     : KD 3S KS            (23)
Dealer busts, player wins!</code></pre>

  ### Example 2 - player bust

  <pre><code class="language-plaintext">Dealer has cards: 4S 8D               (12)
You have cards  : 9D 10H              (19)
Hit or stand    ? hit

Dealer has cards: 4S 8D               (12)
You have cards  : 9D 10H 10C          (29)
Player busts, dealer wins!</code></pre>

  ### Example 3 - draw
  
  <pre><code class="language-plaintext">Dealer has cards: KS 9D               (19)
You have cards  : 9D 10H              (19)
Hit or stand    ? stand

Dealer has cards: KS 9D               (19)
You have cards  : 9D 10H              (19)
Player and dealer draw.</code></pre>

  ### Example 4 - dealer win

  <pre><code class="language-plaintext">Dealer has cards: 2S 9D               (11)
You have cards  : 10D 10H             (19)
Hit or stand    ? stand

Dealer hits     : 2S 9D 9S            (20)
Dealer stands   : 2S 9D 9S            (20)

Dealer has cards: 2S 9D 9S            (20)
You have cards  : 10D 10H             (19)
Dealer wins!</code></pre>
<br>

  Those should give you what you need to make your program output according to the rubric.

## Restrictions/Requirements

This is the first lab where you'll be **required** to use the following function conventions:

  1. ### Prototypes the function "signature" e.g.
     
     <pre><code class="language-c++">// #1 format: &lt;return type&gt; &lt;function name&gt; (&lt;types & names of any parameters you'll use&gt;)

     int my_function(int arg1, string arg2, double arg3);

     // #2 format: &lt;return type&gt; &lt;functoin name&gt; (&lt;types of any paramaters you'll use&gt;)

     int my_function(int  string, double);

     // #1 lists paramters with type AND name
     // #2 lists paramaters with JUST type
     // choose which one you want to use -- it's up to you</code></pre>

     The protoype includes no implementation, just the signature followed by a `;` like any other line of code, and it must come before your actual function definition.

  2. ### Definitions - the function "implementation" e.g.
     
     <pre><code class="language-c++">// adds two integers and returns their sum
     int add(int num1, int num2) {
             return num1 + num2;
     }</code></pre>

     The definition comes after the prototype and includes the actual code that it runs when called.

  3. ### Function Calls - runs a function from another function (usually `main`) scope e.g.
     
      <pre><code class="language-c++">int main() {
     /* we're in main, but you can call a function
     from anywhere, this is just an example */
 
     // we make two ints
     int a = 1, b = 2;
 
     // we "call" add on our two local int variables
     int sum = add(a, b);
 
     // sum becomes 3 after the function call
    }</code></pre>
     
  This one is pretty self explanatory.
  
  You'll need to write **protoypes**, **definitions**, and **calls**, for the following routines:

  1. dealing a single card to player or dealer. 
        - *function hint*
          <pre><code class="language-plaintext">return type: string
          parameter type(s): string array[52], vector<string>
          desciption: randomly choose an undealt card
          & assigns it to the player or deale. Returns the card abbreviation as string.</code></pre>

  2. return a random number between a min & max value
        - *function hint*
          <pre><code class="language-plaintext"> return type: int
          name: GetRandom
          paramter type(s): int, int
          desciption: returns a random int between
          [min, max] (inclusive)</code></pre>

  3. initializing cards
        - *function hint*
          <pre><code class="language-plaintext">return type: 
          name: InitializeCards
          paramter type(s): 
          desciption: fills array of 52 cards with every suit/face combination</code></pre>

  4. scoring the player and/or dealer hand.
        - *function hint*
          <pre><code class="language-plaintext">return type: new vector of cards i.e. vector<string>
          name: whatever you want
          paramter type(s): vector&lt;string&gt;
          desciption:</code></pre>