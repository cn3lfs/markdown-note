---
title: es2015-data-types
author: bro wen
date: 2019-03-02 12:00:50
tags: [ES2015]
---
# JavaScript data types and data structures

## Dynamic typing

JavaScript is a loosely typed or a dynamic language. Variables in JavaScript are not directly associated with any particular value type, and any variable can be assigned (and re-assigned) values of all types:

```js
var foo = 42;    // foo is now a number
foo     = 'bar'; // foo is now a string
foo     = true;  // foo is now a boolean
```

## Data types

The latest ECMAScript standard defines seven data types:

    Six data types that are primitives:
        Boolean
        Null
        Undefined
        Number
        String
        Symbol (new in ECMAScript 6)
    and Object

Objects

In computer science, an object is a value in memory which is possibly referenced by an identifier.


## 为什么typescript会出现?

## why 0.3 !== 0.1 + 0.2?

According to the ECMAScript standard, there is only one number type: the double-precision 64-bit binary format IEEE 754 value (numbers between -(2^53 -1) and 2^53 -1). There is no specific type for integers. In addition to being able to represent floating-point numbers, the number type has three symbolic values: +Infinity, -Infinity, and NaN (not-a-number).

因为js里面没有真实的整数,根据IEEE 754标准,0.1 0.2 0.3 其实都是近似得到的小数,所以严格意义上,0.3 !== 0.1 + 0.2