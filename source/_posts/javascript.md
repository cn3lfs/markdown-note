---
title: javascript
author: bro wen
date: 2018-03-19 11:01:12
tags: [JavaScript]
---
1.javascript 1995 appeared, many browser use it. first it was used by form validation.
2.
Module import export
es5: 
export:
var Modul_var = {};modul.exports = Modul_var;
import:
var Modul_var = require("./XX.js");
function doSomething() {
    ...
    use Modul_var here
}


es6:
export:
let Modul_var ={}; export default Modul_var;
Or let Modul_var1 ={};let Modul_var2 ={}; export {Modul_var1 as [alias1], Modul_var2 as [alias2]};
import:
import {Modul_var as [alias]} from "./XX.js";
Or import {[alias1], [alias2]} from "./XX.js"; (the".js"extention can be ignored, just the name)
function doSomething() {
    ...
    use Modul_var here
}
## Built-In Types
In JavaScript, there are no true integers, all numbers are implemented in double-precision 64-bit binary format IEEE 754. When we use binary floating-point numbers, it will have some side effects.

Here is an example of these side effects.
0.1 + 0.2 == 0.3 // false

For the primitive data types, when we use literals to initialize a variable, the variable only has the literals as its value, it doesn’t have a type. It will be converted to the corresponding type only when necessary.

1 + '1' // '11'
2 * '2' // 4 // '2' will coerce to 2.
[1, 2] + [2, 1] // '1,22,1'

Note the expression 'a' + + 'b' for addition:
'a' + + 'b' // -> "aNaN" // + 'b' represent +b, if b not a number, will get NaN. Then 'a' + 'NaN' => 'aNaN'.
##  New

1. Create a new object
2. Chained to prototype
3. Bind this
4. Return a new object
It is recommended to create objects using the literal notation (whether it's for performance or readability), since a look-up is needed for Object through the scope chain when creating an object using new Object(), but you don't have this kind of problem when using literals.

For new, we also need pay attention to the operator precedence:
```javascript
function Foo() {
    return this;
}
Foo.getName = function () {
    console.log('1');
};
Foo.prototype.getName = function () {
    console.log('2');
};

new Foo.getName();   // -> 1
new Foo().getName(); // -> 2
```
As you can see from the above image, __new Foo() has a higher priority than new Foo,__ so we can divide the execution order of the above code like this:
```javascript
new (Foo.getName());
(new Foo()).getName();
```
For the first function, Foo.getName() is executed first, so the result is 1; As for the latter, it first executes new Foo() to create an instance, then finds the getName function on Foo via the prototype chain, so the result is 2.
## Scope
Executing JS code would generate execution context, as long as code is not written in a function, it belongs to the global execution context. Code in a function will generate function execution context. 

The [[Scope]] attribute is generated in the first stage of generating execution context, which is a pointer, corresponds to the linked list of the scope, and JS will look up variables through this linked list up to the global context.

### hoisting
There would be two stages when the execution context is generated. The first stage is the stage of creation(to be specific, the step of generating variable objects), in which the JS interpreter would find out variables and functions that need to be hoisted, and allocate memory for them in advance, then functions would be stored into memory entirely, but variables would only be declared and assigned to undefined, therefore, we can use them in advance in the second stage (the code execution stage).

In the process of hoisting, the same function would overwrite the last function, and __functions have higher priority than variables hoisting.__

Using var is more likely error-prone, thus ES6 introduces a new keyword let. let has an important feature that it can’t be used before declared, which conflicts the common saying that let doesn’t have the ability of hoisting. In fact, let hoists declaration, but is not assigned, because the temporal dead zone.

#class
* You can think of an object as a single data structure that contains data as well as functions; the functions of an object are called its methods. 

The __repr__() method defines what represents the object when a user tries to print that object using print.

-- CSS 
vertical-align only applies to inline and table-cell elements: you can't use it to vertically align block-level elements.
CSS align can only work in the element effect range!
inline element can't set height
margin: 0 auto Only works on block.
float: float elements don't contribute to parent's height, it's width is decided by content.

# BOM
The Browser Object Model (BOM) is a browser-specific convention referring to all the objects exposed by the web browser. Unlike the Document Object Model, there is no standard for implementation and no strict definition, so browser vendors are free to implement the BOM in any way they wish.

That which we see as a window displaying a document, the browser program sees as a hierarchical collection of objects. When the browser parses a document, it creates a collection of objects that define the document and detail how it should be displayed. The object the browser creates is known as the Document object. It is part of a larger collection of objects that the browser makes use of. This collection of browser objects is collectively known as the Browser Object Model, or BOM.

The top level of the hierarchy is the window object, which contains the information about the window displaying the document. Some of the window object are objects themselves that describe the document and related information. 

#IndexedDB

To sum up:

    IndexedDB is a transactional Object Store in which you will be able to store JavaScript objects.
    Indexes on some properties of these objects facilitate faster retrieval and search.
    Applications using IndexedDB can work both online and offline.
    IndexedDB is transactional: it manages concurrent access to data.

### What is the difference between .map, .every, and .forEach?

The difference is in the return values.

.map() returns a new Array of objects created by taking some action on the original item.

.every() returns a boolean - true if every element in this array satisfies the provided testing function. An important difference with .every() is that the test function may not always be called for every element in the array. Once the testing function returns false for any element, no more array elements are iterated. Therefore, the testing function should usually have no side effects.

.forEach() returns nothing - It iterates the Array performing a given action for each item in the Array.

.every() (stops looping the first time the iterator returns false or something falsey)
.some() (stops looping the first time the iterator returns true or something truthy)
.filter() (creates a new array including elements where the filter function returns true and omitting the ones where it returns false)
.map() (creates a new array from the values returned by the iterator function)
.reduce() (builds up a value by repeated calling the iterator, passing in previous values; see the spec for the details; useful for summing the contents of an array and many other things)
.reduceRight() (like reduce, but works in descending rather than ascending order)

# VueJS
## Component
1. component name:
When using a component directly in the DOM (as opposed to in a string template or single-file component), we strongly recommend following the W3C rules for custom tag names (all-lowercase, must contain a hyphen). This helps you avoid conflicts with current and future HTML elements.

2. Global registration
These components are globally registered. That means they can be used in the template of any root Vue instance (new Vue) created after registration. 
This even applies to all subcomponents, meaning all three of these components will also be available inside each other.

3. Local Registration
Global registration often isn’t ideal. For example, if you’re using a build system like Webpack, globally registering all components means that even if you stop using a component, it could still be included in your final build. This unnecessarily increases the amount of JavaScript your users have to download.
Note that locally registered components are not also available in subcomponents.For example, if you wanted ComponentA to be available in ComponentB,You have to:
*  using ES2015 modules
*  Or define a components property in object ComponentB.

4. with webpack:
Remember that global registration must take place before the root Vue instance is created (with new Vue).

## Vue Instance
1. When a Vue instance is created, __it adds all the properties found in its data object to Vue’s reactivity system__. When the values of those properties change, the view will “react”, updating to match the new values.
2. When this data changes, the view will re-render. It should be noted that properties in data are __only reactive if they existed when the instance was created__ .
3. The __only exception to this being the use of Object.freeze()__, which prevents existing properties from being changed, which also means the reactivity system can’t track changes.
4. In addition to data properties, Vue instances expose a number of useful instance properties and methods. These are __prefixed with $__ to differentiate them from user-defined properties.

## Conditionnal Rendering
1. so Vue offers a way for you to say, These two elements are completely separate don’t re-use them. __Add a key__ attribute with unique values.
2. The difference is that an element with v-show will always be rendered and remain in the DOM; v-show __only toggles the display CSS property__ of the element.

Note that v-show doesn’t support the <template> element, nor does it work with v-else.

3. v-if vs v-show

v-if is “real” conditional rendering because it ensures that event listeners and child components inside the conditional block are properly destroyed and re-created during toggles.

v-if is also lazy: if the condition is false on initial render, it will not do anything - the conditional block won’t be rendered until the condition becomes true for the first time.

In comparison, v-show is much simpler - the element is always rendered regardless of initial condition, with CSS-based toggling.

Generally speaking, v-if has higher toggle costs while v-show has higher initial render costs. So prefer v-show if you need to toggle something very often, and prefer v-if if the condition is unlikely to change at runtime.

4. Using v-if and v-for together is not recommended.When used together with v-if, v-for has a higher priority than v-if.

## List Rendering
1. Due to limitations in JavaScript, Vue cannot detect the following changes to an array:
When you directly set an item with the index, e.g. vm.items[indexOfItem] = newValue
When you modify the length of the array, e.g. vm.items.length = newLength

__To overcome caveat 1__, both of the following will accomplish the same as vm.items[indexOfItem] = newValue, but will also trigger state updates in the reactivity system:

// Vue.set
Vue.set(vm.items, indexOfItem, newValue)

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

You can also use the vm.$set instance method, which is an alias for the global Vue.set:

vm.$set(vm.items, indexOfItem, newValue)

__To deal with caveat 2__, you can use splice:

vm.items.splice(newLength)

2. Again due to limitations of modern JavaScript, Vue cannot detect property addition or deletion.
Vue does not allow dynamically adding new root-level reactive properties to an already created instance. However, it’s possible to add reactive properties to a nested object using the Vue.set(object, key, value) method. 

You could add a new age property to the nested userProfile object with:

Vue.set(vm.userProfile, 'age', 27)

You can also use the vm.$set instance method, which is an alias for the global Vue.set:

vm.$set(vm.userProfile, 'age', 27)

3. Sometimes you may want to assign a number of new properties to an existing object, for example using Object.assign() or _.extend(). In such cases, you should create a fresh object with properties from both objects. 

You would add new, reactive properties with:

vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})

### rest parameters

1. A function's last parameter can be prefixed with ... which will cause all remaining (user supplied) arguments to be placed within a "standard" javascript array. Only the last parameter can be a "rest parameter".

2. Difference between rest parameters and the arguments object

    rest parameters are only the ones that haven't been given a separate name (i.e. formally defined in function expression), while the arguments object contains all arguments passed to the function;
    the arguments object is not a real array, while rest parameters are Array instances, meaning methods like sort, map, forEach or pop can be applied on it directly;
    the arguments object has additional functionality specific to itself (like the callee property).



### shallow copy

Object.assign()

1. Properties in the target object will be overwritten by properties in the sources if they have the same key.  Later sources' properties will similarly overwrite earlier ones.  

The Object.assign() method only copies enumerable and own properties from a source object to a target object. It uses [[Get]] on the source and [[Set]] on the target, so it will invoke getters and setters. Therefore it assigns properties versus just copying or defining new properties. This may make it unsuitable for merging new properties into a prototype if the merge sources contain getters. For copying property definitions, including their enumerability, into prototypes Object.getOwnPropertyDescriptor() and Object.defineProperty() should be used instead.

Both String and Symbol properties are copied.

In case of an error, for example if a property is non-writable, a TypeError will be raised, and the target object can be changed if any properties are added before error is raised.

Note that Object.assign() does not throw on null or undefined source values.

2. Object.assign() copies property values. If the source value is a reference to an object, it only copies that reference value.

### deep copy

var obj2 = JSON.parse(JSON.stringify(obj1));

## Recursive

The (1) is the base of recursion, the trivial case.

The (2) is the recursive step.

### execution context
The information about a function run is stored in its execution context.

The execution context is an internal data structure that contains details about the execution of a function: where the control flow is now, the current variables, the value of this (we don’t use it here) and few other internal details.

One function call has exactly one execution context associated with it.

### for ... of
Note: for ... of only works for Array-like Object, typical object can not directly use it, must use __for (let item of Object.values(object))__

#### Seperation of Concerns

A key principle of software development and architecture is the notion of separation of Separation-of-Concerns-Feb-2013concerns.  At a low level, this principle is closely related to the Single Responsibility Principle of object oriented programming.  The general idea is that one should avoid co-locating different concerns within the design or code.  For instance, if your application includes business logic for identifying certain noteworthy items to display to the user, and your application formats such items in a certain way to make them more noticeable, it would violate separation of concerns if both the logic for determining which items were noteworthy and the formatting of these items were in the same place.  The design would be more maintainable, less tightly coupled, and less likely to violate the Don’t Repeat Yourself principle if the logic for determining which items needed formatted were located in a single location (with other business logic), and were exposed to the user interface code responsible for formatting simply as a property.

At an architectural level, separation of concerns is a key component of building layered applications.  In a traditional N-tier application structure, layers might include data access, business logic, and user interface.  More modern N-tier application designs might include a core domain model and separate infrastructure modules in addition to one or more front end services and/or user interfaces.  Web pages, to a greater or lesser degree, separate concerns relating to structure, logic, and formatting through the use of HTML, JavaScript, and CSS.  At a lower level,the networking model used by the Internet is broken into a series of layers each with specific concerns and responsibilities, and demonstrates how separation of concerns can be effectively applied.

In addition to separating logic across programming layers, one can also separate concerns along application feature sets.  Applications may be written to allow functionality to be added or removed in a modular fashion, and many commercial products support this functionality as a means of separating features across product SKUs or to allow third parties to create plug-ins.

Separation of Concerns tends to be a natural consequence of following the Don’t Repeat Yourself principle, since of necessity abstractions must be built to encapsulate concepts that would otherwise be repeated throughout the application.  As long as these abstractions are logically grouped and organized, then Separation of Concerns should be achieved.

### why 0.1 + 0.2 != 0.3

Floating Point Math

Your language isn't broken, it's doing floating point math. Computers can only natively store integers, so they need some way of representing decimal numbers. This representation comes with some degree of inaccuracy. That's why, more often than not, .1 + .2 != .3.
Why does this happen?

It's actually pretty simple. When you have a base 10 system (like ours), it can only express fractions that use a prime factor of the base. The prime factors of 10 are 2 and 5. So 1/2, 1/4, 1/5, 1/8, and 1/10 can all be expressed cleanly because the denominators all use prime factors of 10. In contrast, 1/3, 1/6, and 1/7 are all repeating decimals because their denominators use a prime factor of 3 or 7. In binary (or base 2), the only prime factor is 2. So you can only express fractions cleanly which only contain 2 as a prime factor. In binary, 1/2, 1/4, 1/8 would all be expressed cleanly as decimals. While, 1/5 or 1/10 would be repeating decimals. So 0.1 and 0.2 (1/10 and 1/5) while clean decimals in a base 10 system, are repeating decimals in the base 2 system the computer is operating in. When you do math on these repeating decimals, you end up with leftovers which carry over when you convert the computer's base 2 (binary) number into a more human readable base 10 number.

Below are some examples of sending .1 + .2 to standard output in a variety of languages.

read more: | wikipedia | IEEE 754 | Stack Overflow | What Every Computer Scientist Should Know About Floating-Point Arithmetic

### Time-based animation


1. Your application runs on different devices, and where 60 frames/s are definitely not possible. More generally, you want your animated objects to move at the same speed on screen, regardless of the device that runs the game.

2. You want to perform some animations only a few times per second. For example, in sprite-based animation (drawing different images as a character moves, for example), you will not change the images of the animation 60 times/s, but only ten times per second. Mario will walk on the screen in a 60 fps animation, but his posture will not change every 1/60th of second.

3. You may also want to accurately set the framerate, leaving some CPU time for other tasks. Many games consoles limit the frame-rate to 1/30th of a second, in order to allow time for other sorts of computations (physics engine, artificial intelligence, etc.)


__High Resolution Time API__:  "The only method exposed is now(), which returns a DOMHighResTimeStamp representing the current time in milliseconds. The timestamp is very accurate, with precision to a thousandth of a millisecond. 

Please note that while __Date.now()__ returns the number of milliseconds elapsed since 1 January 1970 00:00:00 UTC,__performance.now()__ returns the number of milliseconds, with microseconds in the fractional part, from performance.timing.navigationStart(), the start of navigation of the document, to the performance.now() call.

Another important difference between Date.now() and performance.now() is that the latter is monotonically increasing, so the difference between two calls will never be negative."

###  Promise:

Important: Always return results, otherwise callbacks won't catch the result of a previous promise (with arrow functions () => x is short for () => { return x; }).

Promises solve a fundamental flaw with the callback pyramid of doom, by catching all errors, even thrown exceptions and programming errors. This is essential for functional composition of asynchronous operations.

A Promise can be created from scratch using its constructor. This should be needed only to wrap old APIs.Mixing old-style callbacks and promises is problematic. If saySomething() fails or contains a programming error, nothing catches it. setTimeout is to blame for this.

Nesting is a control structure to limit the scope of catch statements. Specifically, a nested catch only catches failures in its scope and below, not errors higher up in the chain outside the nested scope. When used correctly, this gives greater precision in error recovery:


### script async defer
For classic scripts, if the async attribute is present, then the classic script will be fetched in parallel to parsing and evaluated as soon as it is available (potentially before parsing completes). If the async attribute is not present but the defer attribute is present, then the classic script will be fetched in parallel and evaluated when the page has finished parsing. If neither attribute is present, then the script is fetched and evaluated immediately, blocking parsing until these are both complete.

For module scripts, if the async attribute is present, then the module script and all its dependencies will be fetched in parallel to parsing, and the module script will be evaluated as soon as it is available (potentially before parsing completes). Otherwise, the module script and its dependencies will be fetched in parallel to parsing and evaluated when the page has finished parsing. (The defer attribute has no effect on module scripts.)