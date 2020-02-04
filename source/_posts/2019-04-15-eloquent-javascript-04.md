---
title: eloquent-javascript-04
author: bro wen
date: 2019-04-15 07:24:15
tags: [eloquent javascript, JavaScript]
---

# Eloquent JavaScript

## Array

Fortunately, JavaScript provides a data type specifically for storing sequences of values. It is called an array and is written as a list of values between square brackets, separated by commas.

let listOfNumbers = [2, 3, 5, 7, 11];

console.log(listOfNumbers[2]);
// → 5
console.log(listOfNumbers[0]);
// → 2
console.log(listOfNumbers[2 - 1]);
// → 3

The notation for getting at the elements inside an array also uses square brackets. A pair of square brackets immediately after an expression, with another expression inside of them, will look up the element in the left-hand expression that corresponds to the index given by the expression in the brackets.

The first index of an array is zero, not one. So the first element is retrieved with listOfNumbers[0]. Zero-based counting has a long tradition in technology and in certain ways makes a lot of sense, but it takes some getting used to. Think of the index as the amount of items to skip, counting from the start of the array.

## PROPERTIES
访问null, undefined会报typeerror 错误
Almost all JavaScript values have properties. The exceptions are null and undefined. If you try to access a property on one of these nonvalues, you get an error.

null.length;
// → TypeError: null has no properties

用点访问, 后面必须直接是literal property name, 用"[]"访问, 先evaluate[]里面的值, 让后再用里面的值访问property, 通常如果property name超过一个 word或者是数字, 我们会用这种方法.
The two main ways to access properties in JavaScript are with a dot and with square brackets. Both value.x and value[x] access a property on value—but not necessarily the same property. The difference is in how x is interpreted. When using a dot, the word after the dot is the literal name of the property. When using square brackets, the expression between the brackets is evaluated to get the property name. Whereas value.x fetches the property of value named “x”, value[x] tries to evaluate the expression x and uses the result, converted to a string, as the property name.

So if you know that the property you are interested in is called color, you say value.color. If you want to extract the property named by the value held in the binding i, you say value[i]. Property names are strings. They can be any string, but the dot notation works only with names that look like valid binding names. So if you want to access a property named 2 or John Doe, you must use square brackets: value[2] or value["John Doe"].

The elements in an array are stored as the array’s properties, using numbers as property names. Because you can’t use the dot notation with numbers and usually want to use a binding that holds the index anyway, you have to use the bracket notation to get at them.

## Methods

Both string and array objects contain, in addition to the length property, a number of properties that hold function values.

Properties that contain functions are generally called methods of the value they belong to, as in “toUpperCase is a method of a string”.

比如String有 toUpperCase, toLowerCase
Array 的 push, pop

Array用的是Stack数据结构, 后进先出
The push method adds values to the end of an array, and the pop method does the opposite, removing the last value in the array and returning it.

These somewhat silly names are the traditional terms for operations on a stack. A stack, in programming, is a data structure that allows you to push values into it and pop them out again in the opposite order so that the thing that was added last is removed first. These are common in programming—you might remember the function call stack from the previous chapter, which is an instance of the same idea.

## Objects

Values of the type object are arbitrary collections of properties. One way to create an object is by using braces as an expression.

let day1 = {
  squirrel: false,
  events: ["work", "touched tree", "pizza", "running"]
};

用{}定义一个Object, 如果property name 不是变量命名或数字, 必须加"".
Inside the braces, there is a list of properties separated by commas. Each property has a name followed by a colon and a value. When an object is written over multiple lines, indenting it like in the example helps with readability. Properties whose names aren’t valid binding names or valid numbers have to be quoted.

This means that braces have two meanings in JavaScript. At the start of a statement, they start a block of statements. In any other position, they describe an object. Fortunately, it is rarely useful to start a statement with an object in braces, so the ambiguity between these two is not much of a problem.

访问不存在的property会得到undefined
Reading a property that doesn’t exist will give you the value undefined.

不同的binding 和 property 可以对应同一个东西
To briefly return to our tentacle model of bindings—property bindings are similar. They grasp values, but other bindings and properties might be holding onto those same values. You may think of objects as octopuses with any number of tentacles, each of which has a name tattooed on it.

The delete operator cuts off a tentacle from such an octopus. It is a unary operator that, when applied to an object property, will remove the named property from the object. This is not a common thing to do, but it is possible.

```js
let anObject = {left: 1, right: 2};
console.log(anObject.left);
// → 1
delete anObject.left;
console.log(anObject.left);
// → undefined
console.log("left" in anObject);
// → false
console.log("right" in anObject);
// → true
```

赋值undefined 和 delete他有什么区别?
The binary in operator, when applied to a string and an object, tells you whether that object has a property with that name. The difference between setting a property to undefined and actually deleting it is that, in the first case, the object still has the property (it just doesn’t have a very interesting value), whereas in the second case the property is no longer present and in will return false.

Object.keys:
To find out what properties an object has, you can use the Object.keys function. You give it an object, and it returns an array of strings—the object’s property names.

console.log(Object.keys({x: 0, y: 0, z: 2}));
// → ["x", "y", "z"]

Object.assign:
There’s an Object.assign function that copies all properties from one object into another.

let objectA = {a: 1, b: 2};
Object.assign(objectA, {b: 3, c: 4});
console.log(objectA);
// → {a: 1, b: 3, c: 4}

Arrays, then, are just a kind of object specialized for storing sequences of things. If you evaluate typeof [], it produces "object". You can see them as long, flat octopuses with all their tentacles in a neat row, labeled with numbers.

## Mutability

literal values are immutable
We saw that object values can be modified. The types of values discussed in earlier chapters, such as numbers, strings, and Booleans, are all immutable—it is impossible to change values of those types. You can combine them and derive new values from them, but when you take a specific string value, that value will always remain the same. The text inside it cannot be changed. If you have a string that contains "cat", it is not possible for other code to change a character in your string to make it spell "rat".

Objects work differently. You can change their properties, causing a single object value to have different content at different times.

对于两个一样的 literal value, 如果他们相同就是完全相同, 而对于两个 Object, 如果他们的binding不是reference to同一个Object, 则即便他们有相同的property 和 value, 那他们也是两个东西
When we have two numbers, 120 and 120, we can consider them precisely the same number, whether or not they refer to the same physical bits. With objects, there is a difference between having two references to the same object and having two different objects that contain the same properties. 

```js
let object1 = {value: 10};
let object2 = object1;
let object3 = {value: 10};

console.log(object1 == object2);

console.log(object1 == object3);


object1.value = 15;
console.log(object2.value);

console.log(object3.value);
```

bing可以通过改变指针指向不同的值, 但是就一个单纯的值来说, 他是不可改变的; const定义的binding指针不可改变, 但是指针指向的Object里面的内容可以改变.
Bindings can also be changeable or constant, but this is separate from the way their values behave. Even though number values don’t change, you can use a let binding to keep track of a changing number by changing the value the binding points at. Similarly, though a const binding to an object can itself not be changed and will continue to point at the same object, the contents of that object might change.

```js
const score = {visitors: 0, home: 0};

score.visitors = 1;

score = {visitors: 1, home: 1};
```

用==进行比较,  两个不同的Object, 即便他们里面的内容一样, 那他们也不会相等, js本身没有built-in的深度比较
When you compare objects with JavaScript’s == operator, it compares by identity: it will produce true only if both objects are precisely the same value. Comparing different objects will return false, even if they have identical properties. There is no “deep” comparison operation built into JavaScript, which compares objects by contents, but it is possible to write it yourself (which is one of the exercises at the end of this chapter).

## The lycanthrope’s log

```js
let journal = [];

function addEntry(events, squirrel) {
  journal.push({events, squirrel});
  // equal to journal.push({events: events, squirrel: squirrel});
}
```
Note that the object added to the journal looks a little odd. Instead of declaring properties like events: events, it just gives a property name. This is shorthand that means the same thing—if a property name in brace notation isn’t followed by a value, its value is taken from the binding with the same name.

相关性
Correlation is a measure of dependence between statistical variables. A statistical variable is not quite the same as a programming variable. In statistics you typically have a set of measurements, and each variable is measured for every measurement. Correlation between variables is usually expressed as a value that ranges from -1 to 1. Zero correlation means the variables are not related. A correlation of one indicates that the two are perfectly related—if you know one, you also know the other. Negative one also means that the variables are perfectly related but that they are opposites—when one is true, the other is false.

To compute the measure of correlation between two Boolean variables, we can use the phi coefficient (ϕ). This is a formula whose input is a frequency table containing the number of times the different combinations of the variables were observed. The output of the formula is a number between -1 and 1 that describes the correlation.

If we call that table n, we can compute ϕ using the following formula:
ϕ = (n11n00 − n10n01) / √ n1•n0•n•1n•0 

## Computing correlation

```js
function phi(table) {
  return (table[3] * table[0] - table[2] * table[1]) /
    Math.sqrt((table[2] + table[3]) *
              (table[0] + table[1]) *
              (table[1] + table[3]) *
              (table[0] + table[2]));
}

console.log(phi([76, 9, 4, 1]));

function tableFor(event, journal) {
    // 00 01 10 11
  let table = [0, 0, 0, 0];
  for (let i = 0; i < journal.length; i++) {
    let entry = journal[i], index = 0;
    if (entry.events.includes(event)) index += 1;
    if (entry.squirrel) index += 2;
    table[index] += 1;
  }
  return table;
}

console.log(tableFor("pizza", JOURNAL));
```

Arrays have an includes method that checks whether a given value exists in the array. The function uses that to determine whether the event name it is interested in is part of the event list for a given day.

## Array loops

普通for loop 与 for ... of
In the tableFor function, there’s a loop like this:

for (let i = 0; i < JOURNAL.length; i++) {
  let entry = JOURNAL[i];
  
}

This kind of loop is common in classical JavaScript—going over arrays one element at a time is something that comes up a lot, and to do that you’d run a counter over the length of the array and pick out each element in turn.

There is a simpler way to write such loops in modern JavaScript.

for (let entry of JOURNAL) {
  console.log(`${entry.events.length} events.`);
}

When a for loop looks like this, with the word of after a variable definition, it will loop over the elements of the value given after of. This works not only for arrays but also for strings and some other data structures. 

## The final analysis

```js
function journalEvents(journal) {
  let events = [];
  for (let entry of journal) {
    for (let event of entry.events) {
      if (!events.includes(event)) {
        events.push(event);
      }
    }
  }
  return events;
}
// 如果events中有重复的怎么办, 后面计数不是会重复吗
console.log(journalEvents(JOURNAL));

for (let event of journalEvents(JOURNAL)) {
  let correlation = phi(tableFor(event, JOURNAL));
  if (correlation > 0.1 || correlation < -0.1) {
    console.log(event + ":", correlation);
  }
}
```

## Further arraynology

Array data structure: queue. push pop shift unshift
```js
let todoList = [];
function remember(task) {
  todoList.push(task);
}
function getTask() {
  return todoList.shift();
}
function rememberUrgently(task) {
  todoList.unshift(task);
}
```

To search for a specific value, arrays provide an indexOf method. The method searches through the array from the start to the end and returns the index at which the requested value was found—or -1 if it wasn’t found. To search from the end instead of the start, there’s a similar method called lastIndexOf.

console.log([1, 2, 3, 2, 1].indexOf(2));

console.log([1, 2, 3, 2, 1].lastIndexOf(2))

indexOf从前往后搜搜, lastIndexOf从后往前搜索
Both indexOf and lastIndexOf take an optional second argument that indicates where to start searching.

Another fundamental array method is slice, which takes start and end indices and returns an array that has only the elements between them. The start index is inclusive, the end index exclusive.

console.log([0, 1, 2, 3, 4].slice(2, 4));

console.log([0, 1, 2, 3, 4].slice(2));

When the end index is not given, slice will take all of the elements after the start index. You can also omit the start index to copy the entire array.

The concat method can be used to glue arrays together to create a new array, similar to what the + operator does for strings.

```js
function remove(array, index) {
  return array.slice(0, index)
    .concat(array.slice(index + 1));
}
console.log(remove(["a", "b", "c", "d", "e"], 2));

```
## Strings and their properties

We can read properties like length and toUpperCase from string values. But if you try to add a new property, it doesn’t stick.
```js
let kim = "Kim";
kim.age = 88;
console.log(kim.age);
```
Values of type string, number, and Boolean are not objects, and though the language doesn’t complain if you try to set new properties on them, it doesn’t actually store those properties. As mentioned earlier, such values are immutable and cannot be changed.

slice, indexOf
But these types do have built-in properties. Every string value has a number of methods. Some very useful ones are slice and indexOf, which resemble the array methods of the same name.

console.log("coconuts".slice(4, 7));

console.log("coconut".indexOf("u"));

One difference is that a string’s indexOf can search for a string containing more than one character, whereas the corresponding array method looks only for a single element.

console.log("one two three".indexOf("ee"));


trim
The trim method removes whitespace (spaces, newlines, tabs, and similar characters) from the start and end of a string.

console.log("  okay \n ".trim());


zeroPad
The zeroPad function from the previous chapter also exists as a method. It is called padStart and takes the desired length and padding character as arguments.

console.log(String(6).padStart(3, "0"));


split, join
You can split a string on every occurrence of another string with split and join it again with join.

let sentence = "Secretarybirds specialize in stomping";
let words = sentence.split(" ");
console.log(words);

console.log(words.join(". "));


repeat
A string can be repeated with the repeat method, which creates a new string containing multiple copies of the original string, glued together.

console.log("LA".repeat(3));


accessing string
We have already seen the string type’s length property. Accessing the individual characters in a string looks like accessing array elements (with a caveat that we’ll discuss in Chapter 5).

let string = "abc";
console.log(string.length);

console.log(string[1]);

## Rest parameters

It can be useful for a function to accept any number of arguments. For example, Math.max computes the maximum of all the arguments it is given.

To write such a function, you put three dots before the function’s last parameter, like this:

function max(...numbers) {
  let result = -Infinity;
  for (let number of numbers) {
    if (number > result) result = number;
  }
  return result;
}
console.log(max(4, 1, 9, -2));

When such a function is called, the rest parameter is bound to an array containing all further arguments. If there are other parameters before it, their values aren’t part of that array. When, as in max, it is the only parameter, it will hold all arguments.


spread syntax
You can use a similar three-dot notation to call a function with an array of arguments.

let numbers = [5, 1, 7];
console.log(max(...numbers));

This “spreads” out the array into the function call, passing its elements as separate arguments. It is possible to include an array like that along with other arguments, as in max(9, ...numbers, 2).

Square bracket array notation similarly allows the triple-dot operator to spread another array into the new array.

let words = ["never", "fully"];
console.log(["will", ...words, "understand"]);

## The Math object

As we’ve seen, Math is a grab bag of number-related utility functions, such as Math.max (maximum), Math.min (minimum), and Math.sqrt (square root).

The Math object is used as a container to group a bunch of related functionality. There is only one Math object, and it is almost never useful as a value. Rather, it provides a namespace so that all these functions and values do not have to be global bindings.

Having too many global bindings “pollutes” the namespace. The more names have been taken, the more likely you are to accidentally overwrite the value of some existing binding. For example, it’s not unlikely to want to name something max in one of your programs. Since JavaScript’s built-in max function is tucked safely inside the Math object, we don’t have to worry about overwriting it.

同一个scope, let or const 定义的变量不能重复定义, var or function 定义的可以overlap
Many languages will stop you, or at least warn you, when you are defining a binding with a name that is already taken. JavaScript does this for bindings you declared with let or const but—perversely—not for standard bindings nor for bindings declared with var or function.

伪随机 Math.random()
Though computers are deterministic machines—they always react the same way if given the same input—it is possible to have them produce numbers that appear random. To do that, the machine keeps some hidden value, and whenever you ask for a new random number, it performs complicated computations on this hidden value to create a new value. It stores a new value and returns some number derived from it. That way, it can produce ever new, hard-to-predict numbers in a way that seems random.

Math.floor()不大于的整数, Math.ceil()大于的整数, Math.round()最近的整数四舍五入, Math.abs()绝对值
If we want a whole random number instead of a fractional one, we can use Math.floor (which rounds down to the nearest whole number) on the result of Math.random.

console.log(Math.floor(Math.random() * 10));

Multiplying the random number by 10 gives us a number greater than or equal to 0 and below 10. Since Math.floor rounds down, this expression will produce, with equal chance, any number from 0 through 9.

There are also the functions Math.ceil (for “ceiling”, which rounds up to a whole number), Math.round (to the nearest whole number), and Math.abs, which takes the absolute value of a number, meaning it negates negative values but leaves positive ones as they are.

## Destructuring

destructure for array
Let’s go back to the phi function for a moment.

function phi(table) {
  return (table[3] * table[0] - table[2] * table[1]) /
    Math.sqrt((table[2] + table[3]) *
              (table[0] + table[1]) *
              (table[1] + table[3]) *
              (table[0] + table[2]));
}

One of the reasons this function is awkward to read is that we have a binding pointing at our array, but we’d much prefer to have bindings for the elements of the array, that is, let n00 = table[0] and so on. Fortunately, there is a succinct way to do this in JavaScript.

function phi([n00, n01, n10, n11]) {
  return (n11 * n00 - n10 * n01) /
    Math.sqrt((n10 + n11) * (n00 + n01) *
              (n01 + n11) * (n00 + n10));
}

This also works for bindings created with let, var, or const. If you know the value you are binding is an array, you can use square brackets to “look inside” of the value, binding its contents.


destructure for Object
A similar trick works for objects, using braces instead of square brackets.

let {name} = {name: "Faraji", age: 23};
console.log(name);

destructure null or undefined会报错
Note that if you try to destructure null or undefined, you get an error, much as you would if you directly try to access a property of those values.

## JSON

properties只抓取他们的值, 而不真正拥有他们, 而是存储了指针, 用于找到内存上存储的值, 为了方便数据传输, 发明了json这种格式, 不能有 function binding等需要计算的值, 也不能有注释, serialize
Because properties only grasp their value, rather than contain it, objects and arrays are stored in the computer’s memory as sequences of bits holding the addresses—the place in memory—of their contents. So an array with another array inside of it consists of (at least) one memory region for the inner array, and another for the outer array, containing (among other things) a binary number that represents the position of the inner array.

If you want to save data in a file for later or send it to another computer over the network, you have to somehow convert these tangles of memory addresses to a description that can be stored or sent. You could send over your entire computer memory along with the address of the value you’re interested in, I suppose, but that doesn’t seem like the best approach.

What we can do is serialize the data. That means it is converted into a flat description. A popular serialization format is called JSON (pronounced “Jason”), which stands for JavaScript Object Notation. It is widely used as a data storage and communication format on the Web, even in languages other than JavaScript.

JSON looks similar to JavaScript’s way of writing arrays and objects, with a few restrictions. All property names have to be surrounded by double quotes, and only simple data expressions are allowed—no function calls, bindings, or anything that involves actual computation. Comments are not allowed in JSON.

A journal entry might look like this when represented as JSON data:

{
  "squirrel": false,
  "events": ["work", "touched tree", "pizza", "running"]
}

JavaScript gives us the functions JSON.stringify and JSON.parse to convert data to and from this format. The first takes a JavaScript value and returns a JSON-encoded string. The second takes such a string and converts it to the value it encodes.

let string = JSON.stringify({squirrel: false,
                             events: ["weekend"]});
console.log(string);

console.log(JSON.parse(string).events);


## Summary

Objects and arrays (which are a specific kind of object) provide ways to group several values into a single value. Conceptually, this allows us to put a bunch of related things in a bag and run around with the bag, instead of wrapping our arms around all of the individual things and trying to hold on to them separately.

Most values in JavaScript have properties, the exceptions being null and undefined. Properties are accessed using value.prop or value["prop"]. Objects tend to use names for their properties and store more or less a fixed set of them. Arrays, on the other hand, usually contain varying amounts of conceptually identical values and use numbers (starting from 0) as the names of their properties.

There are some named properties in arrays, such as length and a number of methods. Methods are functions that live in properties and (usually) act on the value they are a property of.

You can iterate over arrays using a special kind of for loop—for (let element of array)

## Exercises

### The sum of a range

The introduction of this book alluded to the following as a nice way to compute the sum of a range of numbers:

console.log(sum(range(1, 10)));

Write a range function that takes two arguments, start and end, and returns an array containing all the numbers from start up to (and including) end.

Next, write a sum function that takes an array of numbers and returns the sum of these numbers. Run the example program and see whether it does indeed return 55.

As a bonus assignment, modify your range function to take an optional third argument that indicates the “step” value used when building the array. If no step is given, the elements go up by increments of one, corresponding to the old behavior. The function call range(1, 10, 2) should return [1, 3, 5, 7, 9]. Make sure it also works with negative step values so that range(5, 2, -1) produces [5, 4, 3, 2]

```js
function range(start, end, step = 1) {
    const arr = [];
    while (step >= 0 ? start <= end : start >= end) {
        arr.push(start);
        start += step;
    }
    return ;
}

function sum(numbers) {
    return numbers.reduce((accumulator, currentVal) => {
        return accumulator + currentVal;
    });
}
```

### Reversing an array

Arrays have a reverse method that changes the array by inverting the order in which its elements appear. For this exercise, write two functions, reverseArray and reverseArrayInPlace. The first, reverseArray, takes an array as argument and produces a new array that has the same elements in the inverse order. The second, reverseArrayInPlace, does what the reverse method does: it modifies the array given as argument by reversing its elements. Neither may use the standard reverse method.

Thinking back to the notes about side effects and pure functions in the previous chapter, which variant do you expect to be useful in more situations? Which one runs faster?

```js
function reverseArray(arr) {
    const tempArr = arr.slice();
    const returnArr = [];
    for (var i = 0; i < arr.length ; i++) {
        returnArr.push(tempArr.pop());
    }
    return returnArr;
}
function reverseArrayInPlace(arr) {
    const tempArr = arr.slice();
    for (var i = tempArr.length - 2; i >= 0; i--) {
        arr.push(arr.splice(i, 1)[0]);
    }
    return arr;
}
// 交换第i位与length-1-i位
function reverseArrayInPlace2(arr) {
     for (let i = 0; i < Math.floor(arr.length / 2); i++) {
        [arr[i], arr[arr.length - 1 - i]] = [arr[arr.length - 1 -i], arr[i]];
    }
    return arr;
}
```

### A list

Objects, as generic blobs of values, can be used to build all sorts of data structures. A common data structure is the list (not to be confused with array). A list is a nested set of objects, with the first object holding a reference to the second, the second to the third, and so on.

```js
let list = {
  value: 1,
  rest: {
    value: 2,
    rest: {
      value: 3,
      rest: null
    }
  }
};
```

A nice thing about lists is that they can share parts of their structure. For example, if I create two new values {value: 0, rest: list} and {value: -1, rest: list} (with list referring to the binding defined earlier), they are both independent lists, but they share the structure that makes up their last three elements. The original list is also still a valid three-element list.

Write a function arrayToList that builds up a list structure like the one shown when given [1, 2, 3] as argument. Also write a listToArray function that produces an array from a list. Then add a helper function prepend, which takes an element and a list and creates a new list that adds the element to the front of the input list, and nth, which takes a list and a number and returns the element at the given position in the list (with zero referring to the first element) or undefined when there is no such element.

If you haven’t already, also write a recursive version of nth