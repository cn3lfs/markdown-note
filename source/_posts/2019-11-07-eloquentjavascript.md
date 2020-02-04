---
title: eloquentjavascript-01
author: bro wen
date: 2019-11-07 12:45:07
tags: [eloquent javascript, JavaScript]
---

## Basic Types

### Numbers

**Caveats**: 因为 js 里面采取 64bit, 有些无限小数可能会失真, 因而 0.1 + 0.2 其实不等于 0.3
Calculations with whole numbers (also called integers) smaller than the aforementioned 9 quadrillion are guaranteed to always be precise. Unfortunately, calculations with fractional numbers are generally not. Just as π (pi) cannot be precisely expressed by a finite number of decimal digits, many numbers lose some precision when only 64 bits are available to store them. This is a shame, but it causes practical problems only in specific situations. The important thing is to be aware of it and treat fractional digital numbers as approximations, not as precise values.

#### Special numbers

Infinity and -Infinity, NaN

### Strings

You can use single quotes, double quotes, or backticks to mark strings, as long as the quotes at the start and the end of the string match.

**Caveats:** Newlines (the characters you get when you press enter) can be included without escaping only when the string is quoted with backticks (`).

**Escape Characters:**
To make it possible to include such characters in a string, the following notation is used: whenever a backslash (\) is found inside quoted text, it indicates that the character after it has a special meaning. This is called escaping the character. A quote that is preceded by a backslash will not end the string but be part of it. When an n character occurs after a backslash, it is interpreted as a newline. Similarly, a t after a backslash means a tab character. Take the following string:

'Hello! I'm \n Kang Kang'

**String are expressed by unicode in javascript:**
Strings, too, have to be modeled as a series of bits to be able to exist inside the computer. The way JavaScript does this is based on the Unicode standard. This standard assigns a number to virtually every character you would ever need, including characters from Greek, Arabic, Japanese, Armenian, and so on. If we have a number for every character, a string can be described by a sequence of numbers.

And that’s what JavaScript does. But there’s a complication: JavaScript’s representation uses 16 bits per string element, which can describe up to 216 different characters. _But Unicode defines more characters than that—about twice as many, at this point. So some characters, such as many emoji, take up two “character positions” in JavaScript strings_.

## Boolean

true
false

## Empty values

null
undefined

The difference in meaning between undefined and null is an accident of JavaScript’s design, and it doesn’t matter most of the time. In cases where you actually have to concern yourself with these values, I recommend treating them as mostly interchangeable.

## Automatic type conversion

When comparing values of the same type using ==, the outcome is easy to predict: you should get true when both values are the same, except in the case of NaN. But when the types differ, JavaScript uses a complicated and confusing set of rules to determine what to do. In most cases, it just tries to convert one of the values to the other value’s type.

I recommend **using the three-character comparison operators defensively to prevent unexpected type conversions from tripping you up**. But when you’re certain the types on both sides will be the same, there is no problem with using the shorter operators.

## Operators

addition subtraction multiplication division remainder

### Unary operators

typeof
plus
minus

### binary operator

#### Comparison operator

gt
lt
gt and equal
lt and equal
not equal
not strictly equal
loosely equal
strictly equal

**When comparing strings, JavaScript goes over the characters from left to right, comparing the Unicode codes one by one.**

#### Logical operators

&& logical and
|| logical or
! Not

##### Short-circuiting of logical operators

Logical AND (&&) expr1 && expr2

Returns expr1 if it can be converted to false; otherwise, returns expr2. Thus, when used with Boolean values, && returns true if both operands are true; otherwise, returns false.

We can use this functionality as a way to fall back on a default value.

Logical OR (||) expr1 || expr2

Returns expr1 if it can be converted to true; otherwise, returns expr2. Thus, when used with Boolean values, || returns true if either operand is true.
Logical NOT (!) !expr

Returns false if its single operand can be converted to true; otherwise, returns true.

### ternary operator

conditional operator
a ? b : c
