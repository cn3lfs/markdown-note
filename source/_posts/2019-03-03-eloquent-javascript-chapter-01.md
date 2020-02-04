---
title: eloquent_javascript_chapter_01
author: bro wen
date: 2019-03-03 11:52:22
tags: [eloquent javascript, JavaScript]
---
this chapter introduce some basic conception about programing, the most interesting part is the code review, make a complex task be expressed with code.

00110001 00000000 00000000
00110001 00000001 00000001
00110011 00000001 00000010
01010001 00001011 00000010
00100010 00000010 00001000
01000011 00000001 00000000
01000001 00000001 00000001
00010000 00000010 00000000
01100010 00000000 00000000

Each line of the previous program contains a single instruction. It could be written in English like this:

    Store the number 0 in memory location 0.

    Store the number 1 in memory location 1.

    Store the value of memory location 1 in memory location 2.

    Subtract the number 11 from the value in memory location 2.

    If the value in memory location 2 is the number 0, continue with instruction 9.

    Add the value of memory location 1 to memory location 0.

    Add the number 1 to the value of memory location 1.

    Continue with instruction 3.

    Output the value of memory location 0.


Although that is already more readable than the soup of bits, it is still rather obscure. Using names instead of numbers for the instructions and memory locations helps.

 Set “total” to 0.
 Set “count” to 1.
[loop]
 Set “compare” to “count”.
 Subtract 11 from “compare”.
 If “compare” is zero, continue at [end].
 Add “count” to “total”.
 Add 1 to “count”.
 Continue at [loop].
[end]
 Output “total”.


 Here is the same program in JavaScript:

let total = 0, count = 1;
while (count <= 10) {
  total += count;
  count += 1;
}
console.log(total);
// → 55


Note: that is the basic procedure to think and program, it is not business with programing language. it is the most hard and interesting part.

then introduce the most content of book. basic js grammar and browser, then Node.js