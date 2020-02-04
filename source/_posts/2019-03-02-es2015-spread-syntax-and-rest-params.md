---
title: es2015-spread-syntax-and-rest-params
author: bro wen
date: 2019-03-02 16:54:26
tags: [ES2015]
---
## Spread syntax

Spread syntax allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected.

### Syntax

For function calls:

    myFunction(...iterableObj);

For array literals or strings:

    [...iterableObj, '4', 'five', 6];

For object literals (new in ECMAScript 2018):

    let objClone = { ...obj };
### Usage
#### Spread in function calls

__Replace apply__
```js
function myFunction(x, y, z) { }
var args = [0, 1, 2];
myFunction.apply(null, args);

```
```js
function myFunction(x, y, z) { }
var args = [0, 1, 2];
myFunction(...args);
```

__Apply for new__
When calling a constructor with new, it's not possible to directly use an array and apply (apply does a [[Call]] and not a [[Construct]]). However, an array can be easily used with new thanks to spread syntax:

``` js
var dateFields = [1970, 0, 1];  // 1 Jan 1970
var d = new Date(...dateFields);
```


``` js
function applyAndNew(constructor, args) {
   function partial () {
      return constructor.apply(this, args);
   };
   if (typeof constructor.prototype === "object") {
      partial.prototype = Object.create(constructor.prototype);
   }
   return partial;
}


function myConstructor () {
   console.log("arguments.length: " + arguments.length);
   console.log(arguments);
   this.prop1="val1";
   this.prop2="val2";
};

var myArguments = ["hi", "how", "are", "you", "mr", null];
var myConstructorWithArguments = applyAndNew(myConstructor, myArguments);

console.log(new myConstructorWithArguments);
// (internal log of myConstructor):           arguments.length: 6
// (internal log of myConstructor):           ["hi", "how", "are", "you", "mr", null]
// (log of "new myConstructorWithArguments"): {prop1: "val1", prop2: "val2"}
```

#### Spread in array literals

__A more powerful array literal__

Without spread syntax, to create a new array using an existing array as one part of it, the array literal syntax is no longer sufficient and imperative code must be used instead using a combination of push, splice, concat, etc. With spread syntax this becomes much more succinct:

__Copy an array__

```js
var arr = [1, 2, 3];
var arr2 = [...arr]; // like arr.slice()
arr2.push(4); 

// arr2 becomes [1, 2, 3, 4]
// arr remains unaffected
```

Note: Spread syntax effectively goes one level deep while copying an array. Therefore, it may be unsuitable for copying multidimensional arrays as the following example shows (it's the same with Object.assign() and spread syntax).

```js
var a = [[1], [2], [3]];
var b = [...a];
b.shift().shift(); // 1
// Now array a is affected as well: [[], [2], [3]]
```
__A better way to concatenate arrays__

```js
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
// Append all items from arr2 onto arr1
arr1 = arr1.concat(arr2);
```

With spread syntax this becomes:
```js
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1 = [...arr1, ...arr2]; // arr1 is now [0, 1, 2, 3, 4, 5]
```

#### Spread in object literals

The Rest/Spread Properties for ECMAScript proposal (stage 4) adds spread properties to object literals. It copies own enumerable properties from a provided object onto a new object.

Shallow-cloning (excluding prototype) or merging of objects is now possible using a shorter syntax than Object.assign().
```js
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

var mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```

Note that Object.assign() triggers setters whereas spread syntax doesn't.

Note that you cannot replace nor mimic the Object.assign() function:
```js
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };
const merge = ( ...objects ) => ( { ...objects } );

var mergedObj = merge ( obj1, obj2);
// Object { 0: { foo: 'bar', x: 42 }, 1: { foo: 'baz', y: 13 } }

var mergedObj = merge ( {}, obj1, obj2);
// Object { 0: {}, 1: { foo: 'bar', x: 42 }, 2: { foo: 'baz', y: 13 } }
```
In the above example, the spread syntax does not work as one might expect: it spreads an array of arguments into the object literal, due to the rest parameter.
## Rest Syntax

the rest parameter syntax allows us to represent an indefinite number of arguments as an array.

### Syntax
```js
function f(a, b, ...theArgs) {
  // ...
}
```
```js
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current;
  });
}

console.log(sum(1, 2, 3));
// expected output: 6

console.log(sum(1, 2, 3, 4));
// expected output: 10
```
## Destructuring assignment

The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

### Syntax

```js
var a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

({ a, b } = { a: 10, b: 20 });
console.log(a); // 10
console.log(b); // 20


// Stage 4(finished) proposal
({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```

## Difference between spread syntax and rest syntax

Rest syntax looks exactly like spread syntax, but is used for destructuring arrays and objects. In a way, rest syntax is the opposite of spread syntax: spread 'expands' an array into its elements, while rest collects multiple elements and 'condenses' them into a single element.
