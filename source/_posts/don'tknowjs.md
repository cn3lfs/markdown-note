---
title: don'tknowjs
author: bro wen
date: 2018-9-20 11:01:12
tags: [JavaScript]
---
1. statement:In a computer language, a group of words, numbers, and operators that performs a specific task is a statement

2. expression:Statements are made up of one or more expressions. An expression is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

3. You should always declare the variable by name before you use it. But you only need to declare a variable once for each scope (see "Scope"); it can be used as many times after that as needed. 

4. JavaScript has built-in types for each of these so called primitive values:
When you need to do math, you want a number.
When you need to print a value on the screen, you need a string (one or more characters, words, sentences).
When you need to make a decision in your program, you need a boolean (true or false).

Values that are included directly in the source code are called literals. 

5. Similarly, in code we often need to group a series of statements together, which we often call a block. In JavaScript, a block is defined by wrapping one or more statements inside a curly-brace pair { .. }
6.  Only values have types in JavaScript; variables are just simple containers for those values.
7.  equality:
    If either value (aka side) in a comparison could be the true or false value, avoid == and use ===.
    If either value in a comparison could be one of these specific values (0, "", or [] -- empty array), avoid == and use ===.
    In all other cases, you're safe to use ==. Not only is it safe, but in many cases it simplifies your code in a way that improves readability.
You should take special note of the == and === comparison rules if you're comparing two non-primitive values, like objects (including function and array). Because those values are actually held by reference, both == and === comparisons will simply check whether the references match, not anything about the underlying values.
8. 
function scope:
You use the var keyword to declare a variable that will belong to the current function scope, or the global scope if at the top level outside of any function.
Wherever a var appears inside a scope, that declaration is taken to belong to the entire scope and accessible everywhere throughout.
If you try to set a variable that hasn't been declared, you'll either end up creating a variable in the top-level global scope (bad!) or getting an error, depending on "strict mode" (see "Strict Mode").
block scope:
In addition to creating declarations for variables at the function level, ES6 lets you declare variables to belong to individual blocks (pairs of { .. }), using the let keyword. Besides some nuanced details, the scoping rules will behave roughly the same as we just saw with functions:

9. Function scope encourages the idea that all variables belong to the function, and can be used and reused throughout the entirety of the function (and indeed, accessible even to nested scopes). This design approach can be quite useful, and certainly can make full use of the "dynamic" nature of JavaScript variables to take on values of different types as needed.

10. "Principle of Least Privilege" [^note-leastprivilege], also sometimes called "Least Authority" or "Least Exposure". This principle states that in the design of software, such as the API for a module/object, you should expose only what is minimally necessary, and "hide" everything else.

11. Closure is when a function can remember and access its lexical scope even when it's invoked outside its lexical scope.

12. this binding has nothing to do with where a function is declared, but has instead everything to do with the manner in which the function is called.

13. When a function is invoked, an activation record, otherwise known as an execution context, is created. This record contains information about where the function was called from (the call-stack), how the function was invoked, what parameters were passed, etc. One of the properties of this record is the this reference which will be used for the duration of that function's execution.

14. this is a binding made for each function invocation, based entirely on its call-site (how the function is called).

15.  In JS, constructors are just functions that happen to be called with the new operator in front of them. They are not attached to classes, nor are they instantiating a class. They are not even special types of functions. They're just regular functions that are, in essence, hijacked by the use of new in their invocation.

16.  Determining the this binding for an executing function requires finding the direct call-site of that function. Once examined, four rules can be applied to the call-site, in this order of precedence:

    Called with new? Use the newly constructed object.

    Called with call or apply (or bind)? Use the specified object.

    Called with a context object owning the call? Use that context object.

    Default: undefined in strict mode, global object otherwise.

17. Luckily, the language automatically coerces a "string" primitive to a String object when necessary, which means you almost never need to explicitly create the Object form. It is strongly preferred by the majority of the JS community to use the literal form for a value, where possible, rather than the constructed object form.

null and undefined have no object wrapper form, only their primitive values. By contrast, Date values can only be created with their constructed object form, as they have no literal form counter-part.

Objects, Arrays, Functions, and RegExps (regular expressions) are all objects regardless of whether the literal or constructed form is used. The constructed form does offer, in some cases, more options in creation than the literal form counterpart. Since objects are created either way, the simpler literal form is almost universally preferred. Only use the constructed form if you need the extra options.

18. At the same time, a shallow copy is fairly understandable and has far less issues, so ES6 has now defined Object.assign(..) for this task. Object.assign(..) takes a target object as its first parameter, and one or more source objects as its subsequent parameters. It iterates over all the enumerable (see below), owned keys (immediately present) on the source object(s) and copies them (via = assignment only) to target. It also, helpfully, returns target, as you can see below:

19. Note: There's a nuanced exception to be aware of: even if the property is already configurable:false, writable can always be changed from true to false without error, but not back to true if already false.
Another thing configurable:false prevents is the ability to use the delete operator to remove an existing property.

20. The in operator will check to see if the property is in the object, or if it exists at any higher level of the [[Prototype]] chain object traversal (see Chapter 5). By contrast, hasOwnProperty(..) checks to see if only myObject has the property or not, and will not consult the [[Prototype]] chain.

21. The in operator has the appearance that it will check for the existence of a value inside a container, but it actually checks for the existence of a property name. This difference is important to note with respect to arrays, as the temptation to try a check like 4 in [2, 4, 6] is strong, but this will not behave as expected.

22. for..in loops applied to arrays can give somewhat unexpected results, in that the enumeration of an array will include not only all the numeric indices, but also any enumerable properties. It's a good idea to use for..in loops only on objects, and traditional for loops with numeric index iteration for the values stored in arrays.

23. Object.keys(..) returns an array of all enumerable properties, whereas Object.getOwnPropertyNames(..) returns an array of all properties, enumerable or not.
Whereas in vs. hasOwnProperty(..) differ in whether they consult the [[Prototype]] chain or not, Object.keys(..) and Object.getOwnPropertyNames(..) both inspect only the direct object specified.

24. ES5 also added several iteration helpers for arrays, including forEach(..), every(..), and some(..). Each of these helpers accepts a function callback to apply to each element in the array, differing only in how they respectively respond to a return value from the callback.
forEach(..) will iterate over all values in the array, and ignores any callback return values. every(..) keeps going until the end or the callback returns a false (or "falsy") value, whereas some(..) keeps going until the end or the callback returns a true (or "truthy") value.

As contrasted with iterating over an array's indices in a numerically ordered way (for loop or other iterators), the order of iteration over an object's properties is not guaranteed and may vary between different JS engines. Do not rely on any observed ordering for anything that requires consistency among environments, as any observed agreement is unreliable.

Helpfully, ES6 adds a for..of loop syntax for iterating over arrays (and objects, if the object defines its own custom iterator):The for..of loop asks for an iterator object (from a default internal function known as @@iterator in spec-speak) of the thing to be iterated, and the loop then iterates over the successive return values from calling that iterator object's next() method, once for each loop iteration.

You'll always want to reference such special properties by Symbol name reference instead of by the special value it may hold. Also, despite the name's implications, @@iterator is not the iterator object itself, but a function that returns the iterator object -- a subtle but important detail!

##  Prototype
Nearly all objects in JavaScript are instances of Object; a typical object inherits properties (including methods) from Object.prototype, although these properties may be shadowed (a.k.a. overridden). However, an Object may be deliberately created for which this is not true (e.g. by Object.create(null)), or it may be altered so that this is no longer true (e.g. with Object.setPrototypeOf).
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)

1. Objects in JavaScript have an internal property, denoted in the specification as [[Prototype]], which is simply a reference to another object. Almost all objects are given a non-null value for this property, at the time of their creation.
2. "Inheritance" implies a copy operation, and JavaScript doesn't copy object properties (natively, by default). Instead, JS creates a link between two objects, where one object can essentially delegate property/function access to another object. "Delegation" (see Chapter 6) is a much more accurate term for JavaScript's object-linking mechanism.

### Constructor
The constructor property returns a reference to the Object constructor function that created the instance object. Note that the value of this property is a reference to the function itself, not a string containing the function's name. The value is only read-only for primitive values such as 1, true and "test".
All objects (with the exception of objects created with Object.create(null)) will have a constructor property. Objects created without the explicit use of a constructor function (i.e. the object and array literals) will have a constructor property that points to the Fundamental Object constructor type for that object.
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor)

1. In the above snippet, it's tempting to think that Foo is a "constructor", because we call it with new and we observe that it "constructs" an object.

In reality, Foo is no more a "constructor" than any other function in your program. Functions themselves are not constructors. However, when you put the new keyword in front of a normal function call, that makes that function call a "constructor call". In fact, new sort of hijacks any normal function and calls it in a fashion that constructs an object, in addition to whatever else it was going to do.

2. The fact is, .constructor on an object arbitrarily points, by default, at a function who, reciprocally, has a reference back to the object -- a reference which it calls .prototype. The words "constructor" and "prototype" only have a loose default meaning that might or might not hold true later. The best thing to do is remind yourself, "constructor does not mean constructed by".

### Inheritance
1. Recall this figure from earlier, which shows not only delegation from an object (aka, "instance") a1 to object Foo.prototype, but from Bar.prototype to Foo.prototype, which somewhat resembles the concept of Parent-Child class inheritance. Resembles, except of course for the direction of the arrows, which show these are delegation links rather than copy operations.
2. The important part is Bar.prototype = Object.create( Foo.prototype ). Object.create(..) creates a "new" object out of thin air, and links that new object's internal [[Prototype]] to the object you specify (Foo.prototype in this case).

```javascript 
function Foo(name) {
    this.name = name;
}

Foo.prototype.myName = function() {
    return this.name;
};

function Bar(name,label) {
    Foo.call( this, name );
    this.label = label;
}
// here, we make a new `Bar.prototype`
// linked to `Foo.prototype`

// doesn't work like you want!
//Bar.prototype = Foo.prototype;

// works kinda like you want, but with
// side-effects you probably don't want :(
//Bar.prototype = new Foo();

// pre-ES6
// throws away default existing `Bar.prototype`
Bar.prototype = Object.create( Foo.prototype );

// ES6+
// modifies existing `Bar.prototype`
Object.setPrototypeOf( Bar.prototype, Foo.prototype );

// Beware! Now `Bar.prototype.constructor` is gone,
// and might need to be manually "fixed" if you're
// in the habit of relying on such properties!

Bar.prototype.myLabel = function() {
    return this.label;
};

var a = new Bar( "a", "obj a" );

a.myName(); // "a"
a.myLabel(); // "obj a"
```
Bar.prototype = Foo.prototype doesn't create a new object for Bar.prototype to be linked to. It just makes Bar.prototype be another reference to Foo.prototype, which effectively links Bar directly to the same object as Foo links to: Foo.prototype. This means when you start assigning, like Bar.prototype.myLabel = ..., you're modifying not a separate object but the shared Foo.prototype object itself, which would affect any objects linked to Foo.prototype. This is almost certainly not what you want. If it is what you want, then you likely don't need Bar at all, and should just use only Foo and make your code simpler.

Bar.prototype = new Foo() does in fact create a new object which is duly linked to Foo.prototype as we'd want. But, it uses the Foo(..) "constructor call" to do it. If that function has any side-effects (such as logging, changing state, registering against other objects, adding data properties to this, etc.), those side-effects happen at the time of this linking (and likely against the wrong object!), rather than only when the eventual Bar() "descendants" are created, as would likely be expected.

So, we're left with using Object.create(..) to make a new object that's properly linked, but without having the side-effects of calling Foo(..). The slight downside is that we have to create a new object, throwing the old one away, instead of modifying the existing default object we're provided.

## Prototype Chain
When it comes to inheritance, JavaScript only has one construct: objects. Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype, and acts as the final link in this prototype chain.

Nearly all objects in JavaScript are instances of Object which sits on the top of a prototype chain.
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

As we've now seen, the [[Prototype]] mechanism is an internal link that exists on one object which references some other object.

This linkage is (primarily) exercised when a property/method reference is made against the first object, and no such property/method exists. In that case, the [[Prototype]] linkage tells the engine to look for the property/method on the linked-to object. In turn, if that object cannot fulfill the look-up, its [[Prototype]] is followed, and so on. This series of links between objects forms what is called the "prototype chain".

# Behavior Delegation
1. As a brief review of our conclusions from Chapter 5, the [[Prototype]] mechanism is an internal link that exists on one object which references another object.

This linkage is exercised when a property/method reference is made against the first object, and no such property/method exists. In that case, the [[Prototype]] linkage tells the engine to look for the property/method on the linked-to object. In turn, if that object cannot fulfill the look-up, its [[Prototype]] is followed, and so on. This series of links between objects forms what is called the "prototype chain".

2. In JavaScript, the [[Prototype]] mechanism links objects to other objects. There are no abstract mechanisms like "classes", no matter how much you try to convince yourself otherwise.
3. __Behavior Delegation__ means: let some object (XYZ) provide a delegation (to Task) for property or method references if not found on the object (XYZ).

This is an extremely powerful design pattern, very distinct from the idea of parent and child classes, inheritance, polymorphism, etc. Rather than organizing the objects in your mind vertically, with Parents flowing down to Children, think of objects side-by-side, as peers, with any direction of delegation links between the objects as necessary.

4. In addition to OLOO providing ostensibly simpler (and more flexible!) code, behavior delegation as a pattern can actually lead to simpler code architecture. 
5. We didn't need a base Controller class to "share" behavior between the two, because delegation is a powerful enough mechanism to give us the functionality we need. We also, as noted before, don't need to instantiate our classes to work with them, because there are no classes, just the objects themselves. Furthermore, there's no need for composition as delegation gives the two objects the ability to cooperate differentially as needed.

Lastly, we avoided the polymorphism pitfalls of class-oriented design by not having the names success(..) and failure(..) be the same on both objects, which would have required ugly explicit pseudopolymorphism. Instead, we called them accepted() and rejected(..) on AuthController -- slightly more descriptive names for their specific tasks.


6. Lack of a name identifier on an anonymous function:

    makes debugging stack traces harder
    makes self-referencing (recursion, event (un)binding, etc) harder
    makes code (a little bit) harder to understand

Items 1 and 3 don't apply to concise methods.

Even though the de-sugaring uses an anonymous function expression which normally would have no name in stack traces, concise methods are specified to set the internal name property of the function object accordingly, so stack traces should be able to use it (though that's implementation dependent so not guaranteed).

Item 2 is, unfortunately, still a drawback to concise methods. They will not have a lexical identifier to use as a self-reference. 

7. Classes and inheritance are a design pattern you can choose, or not choose, in your software architecture. Most developers take for granted that classes are the only (proper) way to organize code, but here we've seen there's another less-commonly talked about pattern that's actually quite powerful: behavior delegation.

Behavior delegation suggests objects as peers of each other, which delegate amongst themselves, rather than parent and child class relationships. JavaScript's [[Prototype]] mechanism is, by its very designed nature, a behavior delegation mechanism. That means we can either choose to struggle to implement class mechanics on top of JS (see Chapters 4 and 5), or we can just embrace the natural state of [[Prototype]] as a delegation mechanism.

__IMPORTANT__
alomost every function has an special property, denoted as prototype, is shared by its instances, in terms of instance, called [[prototype]]. in prototype property can have some other property(constructor, the methods you defined and [[prototype]] Object usually).

__Remember prototype property is not [[prototype]].__

almost every object has an special property, denoted as __proto__, is a reference to constructor's prototype property, and actually also reference to its [[prototype]]. Because object can not access its [[prototype]] directly. You must use __proto__ property.

use __proto__ to look up properties that do not belong to the object, and __proto__ connects objects together to form a prototype chain.

obj.__proto__ -> constructor.prototype -> constructor.prototype.__proto__(usually another object.prototype) -> .... -> Object.prototype -> Object.prototype.__proto__(null)
__IMPORTANT__

###  Difference of new, Object.create(), directly assign(=), class extends, Object.setPrototypeOf()

1. new:
f = new Foo();
properties of Foo.prototype "copy" to f.[[prototype]].
f.__proto__ actually is a reference to Foo.prototype(constructor's prototype).
so f.__proto__ will be {...}
2. Object.create():
ECMAScript 5 introduced a new method: Object.create(). Calling this method creates a new object. The prototype of this object is the first argument of the function:
Bar.prototype = Object.create(Foo.prototype);
first create a new object with Foo.prototype as its [[prototype]], then assign the new object to Bar.prototype property.the new object is {[[prototype]]:{...}}
so Bar.prototype will be {[...if anything...], [[prototype]]:{...}}
3. directly assign(=):
Bar.prototype = Foo.prototype;
Bar.prototype reference to Foo.prototype, change made to Bar.prototype can affect Foo.prototype, because they reference to the same thing.
4. class extends:
class Bar extends Foo

* Foo.prototype as a property add to Bar.prototype. Bar.prototype can have other properties.

Bar {
prototype {
    constructor: function Bar()
    doSomething: function doSomething()
    <prototype>: Object { … }
}
}
<prototype>: function Foo() 

* Bar.__proto__ get reference to Foo object.

5. Object.setPrototypeOf():
Object.setPrototypeOf(Bar, Foo);
var b1 = new Bar();
b1.speak(); // b1.speak is not a function

set Bar.[[prototype]] to Foo, and only properties(methods) in Bar.prototype can be inheritaed to its instance b1.

### Design Pattern || Mental Models
1. classical ("prototypal") OO design pattern
```javascript
// Parent class
function Widget(width,height) {
    this.width = width || 50;
    this.height = height || 50;
    this.$elem = null;
}

Widget.prototype.render = function($where){
    if (this.$elem) {
        this.$elem.css( {
            width: this.width + "px",
            height: this.height + "px"
        } ).appendTo( $where );
    }
};

// Child class
function Button(width,height,label) {
    // "super" constructor call
    Widget.call( this, width, height );
    this.label = label || "Default";

    this.$elem = $( "<button>" ).text( this.label );
}

// make `Button` "inherit" from `Widget`
Button.prototype = Object.create( Widget.prototype );

// override base "inherited" `render(..)`
Button.prototype.render = function($where) {
    // "super" call
    Widget.prototype.render.call( this, $where );
    this.$elem.click( this.onClick.bind( this ) );
};

Button.prototype.onClick = function(evt) {
    console.log( "Button '" + this.label + "' clicked!" );
};

$( document ).ready( function(){
    var $body = $( document.body );
    var btn1 = new Button( 125, 30, "Hello" );
    var btn2 = new Button( 150, 40, "World" );

    btn1.render( $body );
    btn2.render( $body );
} );
```
2. ES6 class sugar
```javascript
class Widget {
    constructor(width,height) {
        this.width = width || 50;
        this.height = height || 50;
        this.$elem = null;
    }
    render($where){
        if (this.$elem) {
            this.$elem.css( {
                width: this.width + "px",
                height: this.height + "px"
            } ).appendTo( $where );
        }
    }
}

class Button extends Widget {
    constructor(width,height,label) {
        super( width, height );
        this.label = label || "Default";
        this.$elem = $( "<button>" ).text( this.label );
    }
    render($where) {
        super.render( $where );
        this.$elem.click( this.onClick.bind( this ) );
    }
    onClick(evt) {
        console.log( "Button '" + this.label + "' clicked!" );
    }
}

$( document ).ready( function(){
    var $body = $( document.body );
    var btn1 = new Button( 125, 30, "Hello" );
    var btn2 = new Button( 150, 40, "World" );

    btn1.render( $body );
    btn2.render( $body );
} );
```
3. OLOO (objects-linked-to-other-objects) style delegation:
```javascript
var Widget = {
    init: function(width,height){
        this.width = width || 50;
        this.height = height || 50;
        this.$elem = null;
    },
    insert: function($where){
        if (this.$elem) {
            this.$elem.css( {
                width: this.width + "px",
                height: this.height + "px"
            } ).appendTo( $where );
        }
    }
};

var Button = Object.create( Widget );

Button.setup = function(width,height,label){
    // delegated call
    this.init( width, height );
    this.label = label || "Default";

    this.$elem = $( "<button>" ).text( this.label );
};
Button.build = function($where) {
    // delegated call
    this.insert( $where );
    this.$elem.click( this.onClick.bind( this ) );
};
Button.onClick = function(evt) {
    console.log( "Button '" + this.label + "' clicked!" );
};

$( document ).ready( function(){
    var $body = $( document.body );

    var btn1 = Object.create( Button );
    btn1.setup( 125, 30, "Hello" );

    var btn2 = Object.create( Button );
    btn2.setup( 150, 40, "World" );

    btn1.build( $body );
    btn2.build( $body );
} );
```

## ES6 Class
1. If there's any take-away message from the second half of this book (Chapters 4-6), it's that classes are an optional design pattern for code (not a necessary given), and that furthermore they are often quite awkward to implement in a [[Prototype]] language like JavaScript.

This awkwardness is not just about syntax, although that's a big part of it. Chapters 4 and 5 examined quite a bit of syntactic ugliness, from verbosity of .prototype references cluttering the code, to explicit pseudo-polymorphism (see Chapter 4) when you give methods the same name at different levels of the chain and try to implement a polymorphic reference from a lower-level method to a higher-level method. .constructor being wrongly interpreted as "was constructed by" and yet being unreliable for that definition is yet another syntactic ugly.

But the problems with class design are much deeper. Chapter 4 points out that classes in traditional class-oriented languages actually produce a copy action from parent to child to instance, whereas in [[Prototype]], the action is not a copy, but rather the opposite -- a delegation link.

### Problems ES6 Solute
1. Beyond this syntax looking nicer, what problems does ES6 solve?
2. 
There's no more (well, sorta, see below!) references to .prototype cluttering the code.
Button is declared directly to "inherit from" (aka extends) Widget, instead of needing to use Object.create(..) to replace a .prototype object that's linked, or having to set with .__proto__ or Object.setPrototypeOf(..).
super(..) now gives us a very helpful relative polymorphism capability, so that any method at one level of the chain can refer relatively one level up the chain to a method of the same name. This includes a solution to the note from Chapter 4 about the weirdness of constructors not belonging to their class, and so being unrelated -- super() works inside constructors exactly as you'd expect.
class literal syntax has no affordance for specifying properties (only methods). This might seem limiting to some, but it's expected that the vast majority of cases where a property (state) exists elsewhere but the end-chain "instances", this is usually a mistake and surprising (as it's state that's implicitly "shared" among all "instances"). So, one could say the class syntax is protecting you from mistakes.
extends lets you extend even built-in object (sub)types, like Array or RegExp, in a very natural way. Doing so without class .. extends has long been an exceedingly complex and frustrating task, one that only the most adept of framework authors have ever been able to accurately tackle. Now, it will be rather trivial!

### class Gotchas
1. Firstly, the class syntax may convince you a new "class" mechanism exists in JS as of ES6. Not so. class is, mostly, just syntactic sugar on top of the existing [[Prototype]] (delegation!) mechanism.

That means class is not actually copying definitions statically at declaration time the way it does in traditional class-oriented languages. If you change/replace a method (on purpose or by accident) on the parent "class", the child "class" and/or instances will still be "affected", in that they didn't get copies at declaration time, they are all still using the live-delegation model based on [[Prototype]]:

2. class syntax does not provide a way to declare class member properties (only methods). So if you need to do that to track shared state among instances, then you end up going back to the ugly .prototype syntax, like this:
3. For performance pragmatism reasons, super is not late bound (aka, dynamically bound) like this is. Instead it's derived at call-time from [[HomeObject]].[[Prototype]], where [[HomeObject]] is statically bound at creation time.

###  conclusion
class does a very good job of pretending to fix the problems with the class/inheritance design pattern in JS. But it actually does the opposite: it hides many of the problems, and introduces other subtle but dangerous ones.

class contributes to the ongoing confusion of "class" in JavaScript which has plagued the language for nearly two decades. In some respects, it asks more questions than it answers, and it feels in totality like a very unnatural fit on top of the elegant simplicity of the [[Prototype]] mechanism.

Bottom line: if ES6 class makes it harder to robustly leverage [[Prototype]], and hides the most important nature of the JS object mechanism -- the live delegation links between objects -- shouldn't we see class as creating more troubles than it solves, and just relegate it to an anti-pattern?

### Prototype Performance
1. prototype chain can't be long
The lookup time for properties that are high up on the prototype chain can have a negative impact on the performance, and this may be significant in the code where performance is critical. Additionally, trying to access nonexistent properties will always traverse the full prototype chain.

Also, when iterating over the properties of an object, every enumerable property that is on the prototype chain will be enumerated. To check whether an object has a property defined on itself and not somewhere on its prototype chain, it is necessary to use the hasOwnProperty method which all objects inherit from Object.prototype.

2. Bad practice: Extension of native prototypes

One misfeature that is often used is to extend Object.prototype or one of the other built-in prototypes.

This technique is called monkey patching and breaks encapsulation. While used by popular frameworks such as Prototype.js, there is still no good reason for cluttering built-in types with additional non-standard functionality.

The only good reason for extending a built-in prototype is to backport the features of newer JavaScript engines, like Array.forEach.

3. Not set prototye dynamicly
many browsers optimize the prototype and try to guess the location of the method in the memory when calling an instance in advance, but setting the prototype dynamically disrupts all these optimizations and can even force some browsers to recompile for deoptimization your code just to make it work according to the specs.

# You Don't Know JS: Async & Performance
##  Chapter 1: Asynchrony: Now & Later
### Event Loop
The JS engine doesn't run in isolation. It runs inside a hosting environment, which is for most developers the typical web browser. Over the last several years (but by no means exclusively), JS has expanded beyond the browser into other environments, such as servers, via things like Node.js. In fact, JavaScript gets embedded into all kinds of devices these days, from robots to lightbulbs.

But the one common "thread" (that's a not-so-subtle asynchronous joke, for what it's worth) of all these environments is that they have __a mechanism in them that handles executing multiple chunks of your program over time, at each moment invoking the JS engine, called the "event loop."__

So, in other words, your program is generally broken up into lots of small chunks, which happen one after the other in the event loop queue. And technically, other events not related directly to your program can be interleaved within the queue as well.

### Async Console
It's a moving target under what conditions exactly console I/O will be deferred, or even whether it will be observable. Just be aware of this possible asynchronicity in I/O in case you ever run into issues in debugging where objects have been modified after a console.log(..) statement and yet you see the unexpected modifications show up.

Note: If you run into this rare scenario, the best option is to use breakpoints in your JS debugger instead of relying on console output. The next best option would be to force a "snapshot" of the object in question by serializing it to a string, like with JSON.stringify(..).

### Parallel Threading
### Run-to-Completion
### Concurrency

let's imagine a site that displays a list of status updates (like a social network news feed) that progressively loads as the user scrolls down the list. To make such a feature work correctly, (at least) two separate "processes" will need to be executing simultaneously (i.e., during the same window of time, but not necessarily at the same instant).

Note: We're using "process" in quotes here because they aren't true operating system–level processes in the computer science sense. They're virtual processes, or tasks, that represent a logically connected, sequential series of operations. We'll simply prefer "process" over "task" because terminology-wise, it will match the definitions of the concepts we're exploring.

The single-threaded event loop is one expression of concurrency (there are certainly others, which we'll come back to later).

##### QUESTION
1. write a message app

### Noninteracting
### Interaction
### Cooperation

Another expression of concurrency coordination is called "cooperative concurrency." Here, the focus isn't so much on interacting via value sharing in scopes (though that's obviously still allowed!). The goal is to take a long-running "process" and break it up into steps or batches so that other concurrent "processes" have a chance to interleave their operations into the event loop queue.

So, to make a more cooperatively concurrent system, one that's friendlier and doesn't hog the event loop queue, you can process these results in asynchronous batches, after each one "yielding" back to the event loop to let other waiting events happen.

We process the data set in maximum-sized chunks of 1,000 items. By doing so, we ensure a short-running "process," even if that means many more subsequent "processes," as the interleaving onto the event loop queue will give us a much more responsive (performant) site/app.

### Jobs
As of ES6, there's a new concept layered on top of the event loop queue, called the "Job queue." The most likely exposure you'll have to it is with the asynchronous behavior of Promises (see Chapter 3).

A Job can also cause more Jobs to be added to the end of the same queue. So, it's theoretically possible that a Job "loop" (a Job that keeps adding another Job, etc.) could spin indefinitely, thus starving the program of the ability to move on to the next event loop tick. This would conceptually be almost the same as just expressing a long-running or infinite loop (like while (true) ..) in your code.

Jobs are kind of like the spirit of the setTimeout(..0) hack, but implemented in such a way as to have a much more well-defined and guaranteed ordering: later, but as soon as possible.

### Statement Ordering

### Review

A JavaScript program is (practically) always broken up into two or more chunks, where the first chunk runs now and the next chunk runs later, in response to an event. Even though the program is executed chunk-by-chunk, all of them share the same access to the program scope and state, so each modification to state is made on top of the previous state.

Whenever there are events to run, the event loop runs until the queue is empty. Each iteration of the event loop is a "tick." User interaction, IO, and timers enqueue events on the event queue.

At any given moment, only one event can be processed from the queue at a time. While an event is executing, it can directly or indirectly cause one or more subsequent events.

Concurrency is when two or more chains of events interleave over time, such that from a high-level perspective, they appear to be running simultaneously (even though at any given moment only one event is being processed).

It's often necessary to do some form of interaction coordination between these concurrent "processes" (as distinct from operating system processes), for instance to ensure ordering or to prevent "race conditions." These "processes" can also cooperate by breaking themselves into smaller chunks and to allow other "process" interleaving.

## Chapter 2: Callbacks

As you no doubt have observed, callbacks are by far the most common way that asynchrony in JS programs is expressed and managed. Indeed, the callback is the most fundamental async pattern in the language.

Countless JS programs, even very sophisticated and complex ones, have been written upon no other async foundation than the callback (with of course the concurrency interaction patterns we explored in Chapter 1). The callback function is the async work horse for JavaScript, and it does its job respectably.

### Continuations

### Sequential Brain

When we fake multitasking, such as trying to type something at the same time we're talking to a friend or family member on the phone, what we're actually most likely doing is acting as fast context switchers. In other words, we switch back and forth between two or more tasks in rapid succession, simultaneously progressing on each task in tiny, fast little chunks. We do it so fast that to the outside world it appears as if we're doing these things in parallel.

### Doing Versus Planning

You'll notice that this higher level thinking (planning) doesn't seem very async evented in its formulation. In fact, it's kind of rare for us to deliberately think solely in terms of events. Instead, we plan things out carefully, sequentially (A then B then C), and we assume to an extent a sort of temporal blocking that forces B to wait on A, and C to wait on B.

When a developer writes code, they are planning out a set of actions to occur. If they're any good at being a developer, they're carefully planning it out. "I need to set z to the value of x, and then x to the value of y," and so forth.

### Nested/Chained Callbacks

All of these issues are things you can manually hardcode into each step, but that code is often very repetitive and not reusable in other steps or in other async flows in your program.

Even though our brains might plan out a series of tasks in a sequential type of way (this, then this, then this), the evented nature of our brain operation makes recovery/retry/forking of flow control almost effortless. If you're out running errands, and you realize you left a shopping list at home, it doesn't end the day because you didn't plan that ahead of time. Your brain routes around this hiccup easily: you go home, get the list, then head right back out to the store.

But the brittle nature of manually hardcoded callbacks (even with hardcoded error handling) is often far less graceful. Once you end up specifying (aka pre-planning) all the various eventualities/paths, the code becomes so convoluted that it's hard to ever maintain or update it.

That is what "callback hell" is all about! The nesting/indentation are basically a side show, a red herring.

#### Pyramid of doom (programming)
In computer programming, the pyramid of doom is a common problem that arises when a program uses many levels of nested indentation to control access to a function. It is commonly seen when checking for null pointers or handling callbacks.[1]

Most modern object-oriented programming languages use a coding style known as dot notation[citation needed] that allows multiple method calls to be written in a single line of code, each call separated by a period. For instance:

theWidth = windows("Main").views(5).size().width();

This code contains four different instructions; it first looks in the collection of windows for a window with the name "Main", then looks in that window's views collection for the 5th subview within it, then calls the size method to return a structure with the view's dimensions, and finally calls the width method on that structure to produce a result that is assigned to a variable name theWidth.

The problem with this approach is that the code assumes that all of these values exist. While it is reasonable to expect that a window will have a size and that size will have a width, it is not at all reasonable to assume that a window named "Main" will exist, nor that it has five subviews. If either of those assumptions is wrong, one of the methods will be invoked on null, producing a null pointer error. 

### Trust Issues

The mismatch between sequential brain planning and callback-driven async JS code is only part of the problem with callbacks. There's something much deeper to be concerned about.

#### inversion of control

In software engineering, inversion of control (IoC) is a design principle in which custom-written portions of a computer program receive the flow of control from a generic framework. A software architecture with this design inverts control as compared to traditional procedural programming: in traditional programming, the custom code that expresses the purpose of the program calls into reusable libraries to take care of generic tasks, but with inversion of control, it is the framework that calls into the custom, or task-specific, code.

Inversion of control is used to increase modularity of the program and make it extensible,[1] and has applications in object-oriented programming and other programming paradigms. The term was used by Michael Mattsson in a thesis[2], taken from there[3] by Stefano Mazzocchi and popularized by him in 1999 in a defunct Apache Software Foundation project, Avalon, then further popularized in 2004 by Robert C. Martin and Martin Fowler.

The term is related to, but different from, the dependency inversion principle, which concerns itself with decoupling dependencies between high-level and low-level layers through shared abstractions. The general concept is also related to event-driven programming in that it is often implemented using IoC, so that the custom code is commonly only concerned with the handling of events, whereas the event loop and dispatch of events/messages is handled by the framework or the runtime environment. 
[inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control)

### Not Just Others' Code

Some of you may be skeptical at this point whether this is as big a deal as I'm making it out to be. Perhaps you don't interact with truly third-party utilities much if at all. Perhaps you use versioned APIs or self-host such libraries, so that its behavior can't be changed out from underneath you.

So, contemplate this: can you even really trust utilities that you do theoretically control (in your own code base)?

Think of it this way: most of us agree that at least to some extent we should build our own internal functions with some defensive checks on the input parameters, to reduce/prevent unexpected issues.

However you go about it, these sorts of checks/normalizations are fairly common on function inputs, even with code we theoretically entirely trust. In a crude sort of way, it's like the programming equivalent of the geopolitical principle of "Trust But Verify."

So, doesn't it stand to reason that we should do the same thing about composition of async function callbacks, not just with truly external code but even with code we know is generally "under our own control"? Of course we should.

###  Tale of Five Callbacks

It might not be terribly obvious why this is such a big deal. Let me construct an exaggerated scenario to illustrate the hazards of trust at play.

Imagine you're a developer tasked with building out an ecommerce checkout system for a site that sells expensive TVs. You already have all the various pages of the checkout system built out just fine. On the last page, when the user clicks "confirm" to buy the TV, you need to call a third-party function (provided say by some analytics tracking company) so that the sale can be tracked.

You notice that they've provided what looks like an async tracking utility, probably for the sake of performance best practices, which means you need to pass in a callback function. In this continuation that you pass in, you will have the final code that charges the customer's credit card and displays the thank you page.

This code might look like:

analytics.trackPurchase( purchaseData, function(){
    chargeCreditCard();
    displayThankyouPage();
} );

Easy enough, right? You write the code, test it, everything works, and you deploy to production. Everyone's happy!

Six months go by and no issues. You've almost forgotten you even wrote that code. One morning, you're at a coffee shop before work, casually enjoying your latte, when you get a panicked call from your boss insisting you drop the coffee and rush into work right away.

When you arrive, you find out that a high-profile customer has had his credit card charged five times for the same TV, and he's understandably upset. Customer service has already issued an apology and processed a refund. But your boss demands to know how this could possibly have happened. "Don't we have tests for stuff like this!?"

You don't even remember the code you wrote. But you dig back in and start trying to find out what could have gone awry.

After digging through some logs, you come to the conclusion that the only explanation is that the analytics utility somehow, for some reason, called your callback five times instead of once. Nothing in their documentation mentions anything about this.

Frustrated, you contact customer support, who of course is as astonished as you are. They agree to escalate it to their developers, and promise to get back to you. The next day, you receive a lengthy email explaining what they found, which you promptly forward to your boss.

Apparently, the developers at the analytics company had been working on some experimental code that, under certain conditions, would retry the provided callback once per second, for five seconds, before failing with a timeout. They had never intended to push that into production, but somehow they did, and they're totally embarrassed and apologetic. They go into plenty of detail about how they've identified the breakdown and what they'll do to ensure it never happens again. Yadda, yadda.

What's next?

You talk it over with your boss, but he's not feeling particularly comfortable with the state of things. He insists, and you reluctantly agree, that you can't trust them anymore (that's what bit you), and that you'll need to figure out how to protect the checkout code from such a vulnerability again.

After some tinkering, you implement some simple ad hoc code like the following, which the team seems happy with:

var tracked = false;

analytics.trackPurchase( purchaseData, function(){
    if (!tracked) {
        tracked = true;
        chargeCreditCard();
        displayThankyouPage();
    }
} );

Note: This should look familiar to you from Chapter 1, because we're essentially creating a latch to handle if there happen to be multiple concurrent invocations of our callback.

But then one of your QA engineers asks, "what happens if they never call the callback?" Oops. Neither of you had thought about that.

You begin to chase down the rabbit hole, and think of all the possible things that could go wrong with them calling your callback. Here's roughly the list you come up with of ways the analytics utility could misbehave:

    Call the callback too early (before it's been tracked)
    Call the callback too late (or never)
    Call the callback too few or too many times (like the problem you encountered!)
    Fail to pass along any necessary environment/parameters to your callback
    Swallow any errors/exceptions that may happen
    ...

That should feel like a troubling list, because it is. You're probably slowly starting to realize that you're going to have to invent an awful lot of ad hoc logic in each and every single callback that's passed to a utility you're not positive you can trust.

Now you realize a bit more completely just how hellish "callback hell" is.

### Trying to Save Callbacks

There are several variations of callback design that have attempted to address some (not all!) of the trust issues we've just looked at. It's a valiant, but doomed, effort to save the callback pattern from imploding on itself.

For example, regarding more graceful error handling, some API designs provide for split callbacks (one for the success notification, one for the error notification):

Another common callback pattern is called "error-first style" (sometimes called "Node style," as it's also the convention used across nearly all Node.js APIs), where the first argument of a single callback is reserved for an error object (if any). If success, this argument will be empty/falsy (and any subsequent arguments will be the success data), but if an error result is being signaled, the first argument is set/truthy (and usually nothing else is passed):

In both of these cases, several things should be observed.

First, it has not really resolved the majority of trust issues like it may appear. There's nothing about either callback that prevents or filters unwanted repeated invocations. Moreover, things are worse now, because you may get both success and error signals, or neither, and you still have to code around either of those conditions.

Also, don't miss the fact that while it's a standard pattern you can employ, it's definitely more verbose and boilerplate-ish without much reuse, so you're going to get weary of typing all that out for every single callback in your application.

Another trust issue is being called "too early." In application-specific terms, this may actually involve being called before some critical task is complete. But more generally, the problem is evident in utilities that can either invoke the callback you provide now (synchronously), or later (asynchronously).

This nondeterminism around the sync-or-async behavior is almost always going to lead to very difficult to track down bugs. In some circles, the fictional insanity-inducing monster named Zalgo is used to describe the sync/async nightmares. "Don't release Zalgo!" is a common cry, and it leads to very sound advice: always invoke callbacks asynchronously, even if that's "right away" on the next turn of the event loop, so that all callbacks are predictably async.

#### Ad hoc
Ad hoc is a phrase (literally, "for this") that describes ideas which are created solely for a specific task and not intended to be generalizable in any way. 

### Review

Callbacks are the fundamental unit of asynchrony in JS. But they're not enough for the evolving landscape of async programming as JS matures.

First, our brains plan things out in sequential, blocking, single-threaded semantic ways, but callbacks express asynchronous flow in a rather nonlinear, nonsequential way, which makes reasoning properly about such code much harder. Bad to reason about code is bad code that leads to bad bugs.

We need a way to express asynchrony in a more synchronous, sequential, blocking manner, just like our brains do.

Second, and more importantly, callbacks suffer from inversion of control in that they implicitly give control over to another party (often a third-party utility not in your control!) to invoke the continuation of your program. This control transfer leads us to a troubling list of trust issues, such as whether the callback is called more times than we expect.

Inventing ad hoc logic to solve these trust issues is possible, but it's more difficult than it should be, and it produces clunkier and harder to maintain code, as well as code that is likely insufficiently protected from these hazards until you get visibly bitten by the bugs.

We need a generalized solution to all of the trust issues, one that can be reused for as many callbacks as we create without all the extra boilerplate overhead.

We need something better than callbacks. They've served us well to this point, but the future of JavaScript demands more sophisticated and capable async patterns. The subsequent chapters in this book will dive into those emerging evolutions.

## Chapter 3: Promises

In Chapter 2, we identified two major categories of deficiencies with using callbacks to express program asynchrony and manage concurrency: lack of sequentiality and lack of trustability. Now that we understand the problems more intimately, it's time we turn our attention to patterns that can address them.

### What Is a Promise?

#### Future Value

#### Values Now and Later

To put it more plainly: to consistently handle both now and later, we make both of them later: all operations become async.

Of course, this rough callbacks-based approach leaves much to be desired. It's just a first tiny step toward realizing the benefits of reasoning about future values without worrying about the time aspect of when it's available or not.

#### Promise Value

Because a Promise is __externally immutable once resolved__, it's now safe to pass that value around to any party and know that it cannot be modified accidentally or maliciously.

#### Completion Event
foo(..) expressly creates an event subscription capability to return back, and the calling code receives and registers the two event handlers against it.

The inversion from normal callback-oriented code should be obvious, and it's intentional. Instead of passing the callbacks to foo(..), it returns an event capability we call evt, which receives the callbacks.

inversion of control 

uninversion of control

separation of concerns

### Promise "Events"

In the first snippet's approach, bar(..) is called regardless of whether foo(..) succeeds or fails, and it handles its own fallback logic if it's notified that foo(..) failed. The same is true for baz(..), obviously.

In the second snippet, bar(..) only gets called if foo(..) succeeds, and otherwise oopsBar(..) gets called. Ditto for baz(..).

### Thenable Duck Typing

"duck typing" -- "If it looks like a duck, and quacks like a duck, it must be a duck" 

As such, it was decided that the way to recognize a Promise (or something that behaves like a Promise) would be to define something called a "thenable" as any object or function which has a then(..) method on it. It is assumed that any such value is a Promise-conforming thenable.

If you try to fulfill a Promise with any object/function value that happens to have a then(..) function on it, but you weren't intending it to be treated as a Promise/thenable, you're out of luck, because it will automatically be recognized as thenable and treated with special rules (see later in the chapter).

### Promise Trust

Let's start by reviewing the trust issues with callbacks-only coding. When you pass a callback to a utility foo(..), it might:

    Call the callback too early
    Call the callback too late (or never)
    Call the callback too few or too many times
    Fail to pass along any necessary environment/parameters
    Swallow any errors/exceptions that may happen

The characteristics of Promises are intentionally designed to provide useful, repeatable answers to all these concerns.

### Calling Too Early

that is, when you call then(..) on a Promise, even if that Promise was already resolved, the callback you provide to then(..) will always be called asynchronously (for more on this, refer back to "Jobs" in Chapter 1).

### Calling Too Late

That is, when a Promise is resolved, all then(..) registered callbacks on it will be called, in order, immediately at the next asynchronous opportunity (again, see "Jobs" in Chapter 1), and nothing that happens inside of one of those callbacks can affect/delay the calling of the other callbacks.

### Promise Scheduling Quirks

To avoid such nuanced nightmares, you should never rely on anything about the ordering/scheduling of callbacks across Promises. In fact, a good practice is not to code in such a way where the ordering of multiple callbacks matters at all. Avoid that if you can.

### Never Calling the Callback

First, nothing (not even a JS error) can prevent a Promise from notifying you of its resolution (if it's resolved). If you register both fulfillment and rejection callbacks for a Promise, and the Promise gets resolved, one of the two callbacks will always be called.

Of course, if your callbacks themselves have JS errors, you may not see the outcome you expect, but the callback will in fact have been called. We'll cover later how to be notified of an error in your callback, because even those don't get swallowed.

But what if the Promise itself never gets resolved either way? Even that is a condition that Promises provide an answer for, using a higher level abstraction called a "race":

### Calling Too Few or Too Many Times

Promises are defined so that they can only be resolved once. any then(..) registered callbacks will only ever be called once (each).

### Failing to Pass Along Any Parameters/Environment

Something to be aware of: If you call resolve(..) or reject(..) with multiple parameters, all subsequent parameters beyond the first will be silently ignored.

If you want to pass along multiple values, you must wrap them in another single value that you pass, such as an array or an object.

As for environment, functions in JS always retain their closure of the scope in which they're defined (see the Scope & Closures title of this series), so they of course would continue to have access to whatever surrounding state you provide. Of course, the same is true of callbacks-only design

### Swallowing Any Errors/Exceptions

If at any point in the creation of a Promise, or in the observation of its resolution, a JS exception error occurs, such as a TypeError or ReferenceError, that exception will be caught, and it will force the Promise in question to become rejected.

### Trustable Promise?

One of the most important, but often overlooked, details of Promises is that they have a solution to this issue as well. Included with the native ES6 Promise implementation is Promise.resolve(..).

If you pass an immediate, non-Promise, non-thenable value to Promise.resolve(..), you get a promise that's fulfilled with that value.

Promise.resolve(..) will accept any thenable, and will unwrap it to its non-thenable value. But you get back from Promise.resolve(..) a real, genuine Promise in its place, one that you can trust. If what you passed in is already a genuine Promise, you just get it right back, so there's no downside at all to filtering through Promise.resolve(..) to gain trust.

Note: Another beneficial side effect of wrapping Promise.resolve(..) around any function's return value (thenable or not) is that it's an easy way to normalize that function call into a well-behaving async task. 

### Trust Built

Promises are a pattern that augments callbacks with trustable semantics, so that the behavior is more reason-able and more reliable. By uninverting the inversion of control of callbacks, we place the control with a trustable system (Promises) that was designed specifically to bring sanity to our async.

### Chain Flow

Every time you call then(..) on a Promise, it creates and returns a new Promise, which we can chain with.

Whatever value you return from the then(..) call's fulfillment callback (the first parameter) is automatically set as the fulfillment of the chained Promise (from the first point).

Note: As we discussed earlier, when returning a promise from a fulfillment handler, it's unwrapped and can delay the next step. That's also true for returning promises from rejection handlers, such that if the return 42 in step 3 instead returned a promise, that promise could delay step 4. A thrown exception inside either the fulfillment or rejection handler of a then(..) call causes the next (chained) promise to be immediately rejected with that exception.


A then(..) call against one Promise automatically produces a new Promise to return from the call.
Inside the fulfillment/rejection handlers, if you return a value or an exception is thrown, the new returned (chainable) Promise is resolved accordingly.
If the fulfillment or rejection handler returns a Promise, it is unwrapped, so that whatever its resolution is will become the resolution of the chained Promise returned from the current then(..).

### Terminology: Resolve, Fulfill, and Reject
``` js
var rejectedTh = {
    then: function(resolved,rejected) {
        rejected( "Oops" );
    }
};

var rejectedPr = Promise.resolve( rejectedTh );
```

As we discussed earlier in this chapter, Promise.resolve(..) will return a received genuine Promise directly, or unwrap a received thenable. If that thenable unwrapping reveals a rejected state, the Promise returned from Promise.resolve(..) is in fact in that same rejected state.

The Promise.resolve() method returns a Promise object that is resolved with a given value. If the value is a promise, that promise is returned; if the value is a thenable (i.e. has a "then" method), the returned promise will "follow" that thenable, adopting its eventual state; otherwise the returned promise will be fulfilled with the value. This function flattens nested layers of promise-like objects (e.g. a promise that resolves to a promise that resolves to something) into a single layer.
[Promise.resolve()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve#Return_value)

###  Error Handling

The most natural form of error handling for most developers is the synchronous try..catch construct. Unfortunately, it's synchronous-only, so it fails to help in async code patterns:

Promises don't use the popular "error-first callback" design style, but instead use "split callbacks" style; there's one callback for fulfillment and one for rejection:

### Pit of Despair

To avoid losing an error to the silence of a forgotten/discarded Promise, some developers have claimed that a "best practice" for Promise chains is to always end your chain with a final catch(..)

### Uncaught Handling

Browsers have a unique capability that our code does not have: they can track and know for sure when any object gets thrown away and garbage collected. So, browsers can track Promise objects, and whenever they get garbage collected, if they have a rejection in them, the browser knows for sure this was a legitimate "uncaught error," and can thus confidently know it should report it to the developer console.

### Pit of Success

This design is a pit of success. By default, all errors are either handled or reported -- what almost all developers in almost all cases would expect. You either have to register a handler or you have to intentionally opt out, and indicate you intend to defer error handling until later; you're opting for the extra responsibility in just that specific case.

### Promise Patterns

Promise.all()

In classic programming terminology, a "gate" is a mechanism that waits on two or more parallel/concurrent tasks to complete before continuing. It doesn't matter what order they finish in, just that all of them have to complete for the gate to open and let the flow control through.

In the Promise API, we call this pattern all([ .. ]).

Note: Technically, the array of values passed into Promise.all([ .. ]) can include Promises, thenables, or even immediate values. Each value in the list is essentially passed through Promise.resolve(..) to make sure it's a genuine Promise to be waited on, so an immediate value will just be normalized into a Promise for that value. If the array is empty, the main Promise is immediately fulfilled.

The main promise returned from Promise.all([ .. ]) will only be fulfilled if and when all its constituent promises are fulfilled. If any one of those promises instead is rejected, the main Promise.all([ .. ]) promise is immediately rejected, discarding all results from any other promises.



Promise.race()

While Promise.all([ .. ]) coordinates multiple Promises concurrently and assumes all are needed for fulfillment, sometimes you only want to respond to the "first Promise to cross the finish line," letting the other Promises fall away.

This pattern is classically called a "latch," but in Promises it's called a "race."

Warning: A "race" requires at least one "runner," so if you pass an empty array, instead of immediately resolving, the main race([..]) Promise will never resolve. 

### "Finally"

Promises cannot be canceled -- and shouldn't be as that would destroy the external immutability trust discussed in the "Promise Uncancelable" section later in this chapter -- so they can only be silently ignored.

### Concurrent Iterations

### Promise Limitations

#### Sequence Error Handling

But if any step of the chain in fact does its own error handling (perhaps hidden/abstracted away from what you can see), your handleErrors(..) won't be notified. This may be what you want -- it was, after all, a "handled rejection" -- but it also may not be what you want. The complete lack of ability to be notified (of "already handled" rejection errors) is a limitation that restricts capabilities in some use cases.

It's basically the same limitation that exists with a try..catch that can catch an exception and simply swallow it. So this isn't a limitation unique to Promises, but it is something we might wish to have a workaround for.

Unfortunately, many times there is no reference kept for the intermediate steps in a Promise-chain sequence, so without such references, you cannot attach error handlers to reliably observe the errors.

#### Single Value

Promises by definition only have a single fulfillment value or a single rejection reason. In simple examples, this isn't that big of a deal, but in more sophisticated scenarios, you may find this limiting.

The typical advice is to construct a values wrapper (such as an object or array) to contain these multiple messages. This solution works, but it can be quite awkward and tedious to wrap and unwrap your messages with every single step of your Promise chain.

#### Splitting Values

Sometimes you can take this as a signal that you could/should decompose the problem into two or more Promises.

#### Unwrap/Spread Arguments

#### Single Resolution

#### Inertia

Promise.wrap() -> helper function by some libraries

Promise.wrap(..) does not produce a Promise. It produces a function that will produce Promises. In a sense, a Promise-producing function could be seen as a "Promise factory." I propose "promisory" as the name for such a thing ("Promise" + "factory").

While ES6 Promises don't natively ship with helpers for such promisory wrapping, most libraries provide them, or you can make your own. Either way, this particular limitation of Promises is addressable without too much pain (certainly compared to the pain of callback hell!).

#### Promise Uncancelable

Note: Many Promise abstraction libraries provide facilities to cancel Promises, but this is a terrible idea! Many developers wish Promises had natively been designed with external cancelation capability, but the problem is that it would let one consumer/observer of a Promise affect some other consumer's ability to observe that same Promise. This violates the future-value's trustability (external immutability), but moreover is the embodiment of the "action at a distance" anti-pattern (http://en.wikipedia.org/wiki/Action_at_a_distance_%28computer_programming%29). Regardless of how useful it seems, it will actually lead you straight back into the same nightmares as callbacks.

By contrast, a chain of Promises taken collectively together -- what I like to call a "sequence" -- is a flow control expression, and thus it's appropriate for cancelation to be defined at that level of abstraction.

No individual Promise should be cancelable, but it's sensible for a sequence to be cancelable, because you don't pass around a sequence as a single immutable value like you do with a Promise.

### Promise Performance

More work to do, more guards to protect, means that Promises are slower as compared to naked, untrustable callbacks. That much is obvious, and probably simple to wrap your brain around.

Promises are a little slower, but in exchange you're getting a lot of trustability, non-Zalgo predictability, and composability built in. Maybe the limitation is not actually their performance, but your lack of perception of their benefits?

## Chapter 4: Generators

n Chapter 2, we identified two key drawbacks to expressing async flow control with callbacks:

    Callback-based async doesn't fit how our brain plans out steps of a task.
    Callbacks aren't trustable or composable because of inversion of control.

In Chapter 3, we detailed how Promises uninvert the inversion of control of callbacks, restoring trustability/composability.

### Breaking Run-to-Completion

```js
var x = 1;

function *foo() {
    x++;
    yield; // pause!
    console.log( "x:", x );
}

function bar() {
    x++;
}

// construct an iterator `it` to control the generator
var it = foo();

// start `foo()` here!
it.next();
x;                      // 2
bar();
x;                      // 3
it.next();              // x: 
```

So, a generator is a special kind of function that can start and stop one or more times, and doesn't necessarily ever have to finish. 

### Input and Output

A generator function is a special function with the new processing model we just alluded to. But it's still a function, which means it still has some basic tenets that haven't changed -- namely, that it still accepts arguments (aka "input"), and that it can still return a value (aka "output"):

### Iteration Messaging

```js
function *foo(x) {
    var y = x * (yield);
    return y;
}

var it = foo( 6 );

// start `foo(..)`
it.next();

var res = it.next( 7 );

res.value;      // 42
```

```js
function *foo(x) {
    var y = x * (yield "Hello");    // <-- yield a value!
    return y;
}

var it = foo( 6 );

var res = it.next();    // first `next()`, don't pass anything
res.value;              // "Hello"

res = it.next( 7 );     // pass `7` to waiting `yield`
res.value;              // 42
```


### Multiple Iterators

Warning: The most common usage of multiple instances of the same generator running concurrently is not such interactions, but when the generator is producing its own values without input, perhaps from some independently connected resource. We'll talk more about value production in the next section.

``` js
function *foo() {
    var x = yield 2;
    z++;
    var y = yield (x * z);
    console.log( x, y, z );
}

var z = 1;

var it1 = foo();
var it2 = foo();

var val1 = it1.next().value;            // 2 <-- yield 2
var val2 = it2.next().value;            // 2 <-- yield 2

val1 = it1.next( val2 * 10 ).value;     // 40  <-- x:20,  z:2
val2 = it2.next( val1 * 5 ).value;      // 600 <-- x:200, z:3

it1.next( val2 / 2 );                   // y:300
                                        // 20 300 3
it2.next( val1 / 4 );                   // y:10
                                        // 200 10 3
```

### Interleaving

```js
var a = 1;
var b = 2;

function *foo() {
    a++;
    yield;
    b = b * a;
    a = (yield b) + 3;
}

function *bar() {
    b--;
    yield;
    a = (yield 8) + b;
    b = a * (yield 2);
}

function step(gen) {
    var it = gen();
    var last;

    return function() {
        // whatever is `yield`ed out, just
        // send it right back in the next time!
        last = it.next( last ).value;
    };
}

var s1 = step( foo );
var s2 = step( bar );

s2();       // b--;
s2();       // yield 8
s1();       // a++;
s2();       // a = 8 + b;
            // yield 2
s1();       // b = b * a;
            // yield b
s1();       // a = b + 3;
s2();       // b = a * 2;

console.log( a, b );    // 12 18
```

### Generator'ing Values

```js
var something = (function(){
    var nextVal;

    return {
        // needed for `for..of` loops
        [Symbol.iterator]: function(){ return this; },

        // standard iterator interface method
        next: function(){
            if (nextVal === undefined) {
                nextVal = 1;
            }
            else {
                nextVal = (3 * nextVal) + 6;
            }

            return { done:false, value:nextVal };
        }
    };
})();

something.next().value;     // 1
something.next().value;     // 9
something.next().value;     // 33
something.next().value;     // 105


for (var v of something) {
    console.log( v );

    // don't let the loop run forever!
    if (v > 500) {
        break;
    }
}
// 1 9 33 105 321 969
```

### Iterables

The something object in our running example is called an iterator, as it has the next() method on its interface. But a closely related term is iterable, which is an object that contains an iterator that can iterate over its values.

As of ES6, the way to retrieve an iterator from an iterable is that the iterable must have a function on it, with the name being the special ES6 symbol value Symbol.iterator. When this function is called, it returns an iterator. Though not required, generally each call should return a fresh new iterator.

```js
var a = [1,3,5,7,9];

var it = a[Symbol.iterator]();

it.next().value;    // 1
it.next().value;    // 3
it.next().value;    // 5
..

[Symbol.iterator]: function(){ return this; }
```

### Generator Iterator


In JavaScript an iterator is an object which defines a sequence and potentially a return value upon its termination. More specifically an iterator is any object which implements the Iterator protocol by having a next() method which returns an object with two properties: value, the next value in the sequence; and done, which is true if the last value in the sequence has already been consumed. If value is present alongside done, it is the iterator's return value.

An object is iterable if it defines its iteration behavior, such as what values are looped over in a for...of construct.

The next() method also accepts a value which can be used to modify the internal state of the generator. A value passed to next() will be treated as the result of the last yield expression that paused the generator.

Calling a generator function does not execute its body immediately; an iterator object for the function is returned instead. When the iterator's next() method is called, the generator function's body is executed until the first yield expression, which specifies the value to be returned from the iterator or, with yield*, delegates to another generator function. The next() method returns an object with a value property containing the yielded value and a done property which indicates whether the generator has yielded its last value, as a boolean. Calling the next() method with an argument will resume the generator function execution, replacing the yield expression where execution was paused with the argument from next(). 

A return statement in a generator, when executed, will make the generator finished (i.e the done property of the object returned by it will be set to true). If a value is returned, it will be set as the value property of the object returned by the generator.
Much like a return statement, an error thrown inside the generator will make the generator finished -- unless caught within the generator's body.
When a generator is finished, subsequent next calls will not execute any of that generator's code, they will just return an object of this form: {value: undefined, done: true}.
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)


```js
for (var v of something()) {
    console.log( v );

    // don't let the loop run forever!
    if (v > 500) {
        break;
    }
}
// 1 9 33 105 321 969
```


Why couldn't we say for (var v of something) ..? Because something here is a generator, which is not an iterable. We have to call something() to construct a producer for the for..of loop to iterate over.
The something() call produces an iterator, but the for..of loop wants an iterable, right? Yep. The generator's iterator also has a Symbol.iterator function on it, which basically does a return this, just like the something iterable we defined earlier. In other words, a generator's iterator is also an iterable!


#### Stopping the Generator
```js
var it = something();
for (var v of it) {
    console.log( v );

    // don't let the loop run forever!
    if (v > 500) {
        console.log(
            // complete the generator's iterator
            it.return( "Hello World" ).value
        );
        // no `break` needed here
    }
}
// 1 9 33 105 321 969
// cleaning up!
// Hello World
```
When we call it.return(..), it immediately terminates the generator, which of course runs the finally clause. Also, it sets the returned value to whatever you passed in to return(..), which is how "Hello World" comes right back out. We also don't need to include a break now because the generator's iterator is set to done:true, so the for..of loop will terminate on its next iteration.

### Iterating Generators Asynchronously

```js
function foo(x,y) {
    ajax(
        "http://some.url.1/?x=" + x + "&y=" + y,
        function(err,data){
            if (err) {
                // throw an error into `*main()`
                it.throw( err );
            }
            else {
                // resume `*main()` with received `data`
                it.next( data );
            }
        }
    );
}

function *main() {
    try {
        var text = yield foo( 11, 31 );
        console.log( text );
    }
    catch (err) {
        console.error( err );
    }
}

var it = main();

// start it all up!
it.next();
```

### Synchronous Error Handling

```js
function *main() {
    var x = yield "Hello World";

    // never gets here
    console.log( x );
}

var it = main();

it.next();

try {
    // will `*main()` handle this error? we'll see!
    it.throw( "Oops" );
}
catch (err) {
    // nope, didn't handle it!
    console.error( err );           // Oops
}
```
Synchronous-looking error handling (via try..catch) with async code is a huge win for readability and reason-ability.

### Generators + Promises

It should listen for the promise to resolve (fulfillment or rejection), and then either resume the generator with the fulfillment message or throw an error into the generator with the rejection reason.

Let me repeat that, because it's so important. The natural way to get the most out of Promises and generators is to yield a Promise, and wire that Promise to control the generator's iterator.

Let's give it a try! First, we'll put the Promise-aware foo(..) together with the generator *main():

```js
function foo(x,y) {
    return request(
        "http://some.url.1/?x=" + x + "&y=" + y
    );
}

function *main() {
    try {
        var text = yield foo( 11, 31 );
        console.log( text );
    }
    catch (err) {
        console.error( err );
    }
}

var it = main();

var p = it.next().value;

// wait for the `p` promise to resolve
p.then(
    function(text){
        it.next( text );
    },
    function(err){
        it.throw( err );
    }
);
```
Most importantly, we took advantage of the fact that we knew that *main() only had one Promise-aware step in it. What if we wanted to be able to Promise-drive a generator no matter how many steps it has? We certainly don't want to manually write out the Promise chain differently for each generator! What would be much nicer is if there was a way to repeat (aka "loop" over) the iteration control, and each time a Promise comes out, wait on its resolution before continuing.

Also, what if the generator throws out an error (intentionally or accidentally) during the it.next(..) call? Should we quit, or should we catch it and send it right back in? Similarly, what if we it.throw(..) a Promise rejection into the generator, but it's not handled, and comes right back out?

### Promise-Aware Generator Runner

```js
// thanks to Benjamin Gruenbaum (@benjamingr on GitHub) for
// big improvements here!
function run(gen) {
    var args = [].slice.call( arguments, 1), it;

    // initialize the generator in the current context
    it = gen.apply( this, args );

    // return a promise for the generator completing
    return Promise.resolve()
        .then( function handleNext(value){
            // run to the next yielded value
            var next = it.next( value );

            return (function handleResult(next){
                // generator has completed running?
                if (next.done) {
                    return next.value;
                }
                // otherwise keep going
                else {
                    return Promise.resolve( next.value )
                        .then(
                            // resume the async loop on
                            // success, sending the resolved
                            // value back into the generator
                            handleNext,

                            // if `value` is a rejected
                            // promise, propagate error back
                            // into the generator for its own
                            // error handling
                            function handleErr(err) {
                                return Promise.resolve(
                                    it.throw( err )
                                )
                                .then( handleResult );
                            }
                        );
                }
            })(next);
        } );
}

function *main() {
    // ..
}

run( main );
```

That's it! The way we wired run(..), it will automatically advance the generator you pass to it, asynchronously until completion.

#### ES7: async and await?

```js
function foo(x,y) {
    return request(
        "http://some.url.1/?x=" + x + "&y=" + y
    );
}

async function main() {
    try {
        var text = await foo( 11, 31 );
        console.log( text );
    }
    catch (err) {
        console.error( err );
    }
}

main();
```

### Promise Concurrency in Generators

The most natural and effective answer is to base the async flow on Promises, specifically on their capability to manage state in a time-independent fashion (see "Future Value" in Chapter 3).
```js
function *foo() {
    // make both requests "in parallel"
    var p1 = request( "http://some.url.1" );
    var p2 = request( "http://some.url.2" );

    // wait until both promises resolve
    var r1 = yield p1;
    var r2 = yield p2;

    var r3 = yield request(
        "http://some.url.3/?v=" + r1 + "," + r2
    );

    console.log( r3 );
}

// use previously defined `run(..)` utility
run( foo );
```
Why is this different from the previous snippet? Look at where the yield is and is not. p1 and p2 are promises for Ajax requests made concurrently (aka "in parallel"). It doesn't matter which one finishes first, because promises will hold onto their resolved state for as long as necessary.

Then we use two subsequent yield statements to wait for and retrieve the resolutions from the promises (into r1 and r2, respectively). If p1 resolves first, the yield p1 resumes first then waits on the yield p2 to resume. If p2 resolves first, it will just patiently hold onto that resolution value until asked, but the yield p1 will hold on first, until p1 resolves.

Either way, both p1 and p2 will run concurrently, and both have to finish, in either order, before the r3 = yield request.. Ajax request will be made.




If that flow control processing model sounds familiar, it's basically the same as what we identified in Chapter 3 as the "gate" pattern, enabled by the Promise.all([ .. ]) utility. So, we could also express the flow control like this:

```js
function *foo() {
    // make both requests "in parallel," and
    // wait until both promises resolve
    var results = yield Promise.all( [
        request( "http://some.url.1" ),
        request( "http://some.url.2" )
    ] );

    var r1 = results[0];
    var r2 = results[1];

    var r3 = yield request(
        "http://some.url.3/?v=" + r1 + "," + r2
    );

    console.log( r3 );
}

// use previously defined `run(..)` utility
run( foo );
```
Note: As we discussed in Chapter 3, we can even use ES6 destructuring assignment to simplify the var r1 = .. var r2 = .. assignments, with var [r1,r2] = results.


#### Promises, Hidden

 a word of stylistic caution, be careful about how much Promise logic you include inside your generators. The whole point of using generators for asynchrony in the way we've described is to create simple, sequential, sync-looking code, and to hide as much of the details of asynchrony away from that code as possible.

For example, this might be a cleaner approach:
```js
// note: normal function, not generator
function bar(url1,url2) {
    return Promise.all( [
        request( url1 ),
        request( url2 )
    ] );
}

function *foo() {
    // hide the Promise-based concurrency details
    // inside `bar(..)`
    var results = yield bar(
        "http://some.url.1",
        "http://some.url.2"
    );

    var r1 = results[0];
    var r2 = results[1];

    var r3 = yield request(
        "http://some.url.3/?v=" + r1 + "," + r2
    );

    console.log( r3 );
}

// use previously defined `run(..)` utility
run( foo );
```

We treat asynchrony, and indeed Promises, as an implementation detail.

Hiding your Promise logic inside a function that you merely call from your generator is especially useful if you're going to do a sophisticated series flow-control. For example:

### Generator Delegation

It may then occur to you that you might try to call one generator from another generator, using our run(..) helper, such as:

```js
function *foo() {
    var r2 = yield request( "http://some.url.2" );
    var r3 = yield request( "http://some.url.3/?v=" + r2 );

    return r3;
}

function *bar() {
    var r1 = yield request( "http://some.url.1" );

    // "delegating" to `*foo()` via `run(..)`
    var r3 = yield run( foo );

    console.log( r3 );
}

run( bar );
```

But there's an even better way to integrate calling *foo() into *bar(), and it's called yield-delegation. The special syntax for yield-delegation is: yield * __ (notice the extra *). Before we see it work in our previous example, let's look at a simpler scenario:

```js
function *foo() {
    console.log( "`*foo()` starting" );
    yield 3;
    yield 4;
    console.log( "`*foo()` finished" );
}

function *bar() {
    yield 1;
    yield 2;
    yield *foo();   // `yield`-delegation!
    yield 5;
}

var it = bar();

it.next().value;    // 1
it.next().value;    // 2
it.next().value;    // `*foo()` starting
                    // 3
it.next().value;    // 4
it.next().value;    // `*foo()` finished
                    // 5
```

### Why Delegation?

The purpose of yield-delegation is mostly code organization, and in that way is symmetrical with normal function calling.

Imagine two modules that respectively provide methods foo() and bar(), where bar() calls foo(). The reason the two are separate is generally because the proper organization of code for the program calls for them to be in separate functions. For example, there may be cases where foo() is called standalone, and other places where bar() calls foo().

For all these exact same reasons, keeping generators separate aids in program readability, maintenance, and debuggability. In that respect, yield * is a syntactic shortcut for manually iterating over the steps of *foo() while inside of *bar().

Such manual approach would be especially complex if the steps in *foo() were asynchronous, which is why you'd probably need to use that run(..) utility to do it. And as we've shown, yield *foo() eliminates the need for a sub-instance of the run(..) utility (like run(foo)).

#### Delegating Messages

You may wonder how this yield-delegation works not just with iterator control but with the two-way message passing. 

```js
function *foo() {
    console.log( "inside `*foo()`:", yield "B" );

    console.log( "inside `*foo()`:", yield "C" );

    return "D";
}

function *bar() {
    console.log( "inside `*bar()`:", yield "A" );

    // `yield`-delegation!
    console.log( "inside `*bar()`:", yield *foo() );

    console.log( "inside `*bar()`:", yield "E" );

    return "F";
}

var it = bar();

console.log( "outside:", it.next().value );
// outside: A

console.log( "outside:", it.next( 1 ).value );
// inside `*bar()`: 1
// outside: B

console.log( "outside:", it.next( 2 ).value );
// inside `*foo()`: 2
// outside: C

console.log( "outside:", it.next( 3 ).value );
// inside `*foo()`: 3
// inside `*bar()`: D
// outside: E

console.log( "outside:", it.next( 4 ).value );
// inside `*bar()`: 4
// outside: F
```


#### Exceptions Delegated, Too!

```js
function *foo() {
    try {
        yield "B";
    }
    catch (err) {
        console.log( "error caught inside `*foo()`:", err );
    }

    yield "C";

    throw "D";
}

function *bar() {
    yield "A";

    try {
        yield *foo();
    }
    catch (err) {
        console.log( "error caught inside `*bar()`:", err );
    }

    yield "E";

    yield *baz();

    // note: can't get here!
    yield "G";
}

function *baz() {
    throw "F";
}

var it = bar();

console.log( "outside:", it.next().value );
// outside: A

console.log( "outside:", it.next( 1 ).value );
// outside: B

console.log( "outside:", it.throw( 2 ).value );
// error caught inside `*foo()`: 2
// outside: C

console.log( "outside:", it.next( 3 ).value );
// error caught inside `*bar()`: D
// outside: E

try {
    console.log( "outside:", it.next( 4 ).value );
}
catch (err) {
    console.log( "error caught outside:", err );
}
// error caught outside: F
```
#### Delegating Asynchrony


```js
function *foo() {
    var r2 = yield request( "http://some.url.2" );
    var r3 = yield request( "http://some.url.3/?v=" + r2 );

    return r3;
}

function *bar() {
    var r1 = yield request( "http://some.url.1" );

    var r3 = yield *foo();

    console.log( r3 );
}

run( bar );
```

#### Delegating "Recursion"

```js
function *foo(val) {
    if (val > 1) {
        // generator recursion
        val = yield *foo( val - 1 );
    }

    return yield request( "http://some.url/?v=" + val );
}

function *bar() {
    var r1 = yield *foo( 3 );
    console.log( r1 );
}

run( bar );
```
### Generator Concurrency

```js
function response(data) {
    if (data.url == "http://some.url.1") {
        res[0] = data;
    }
    else if (data.url == "http://some.url.2") {
        res[1] = data;
    }
}
```


```js
// `request(..)` is a Promise-aware Ajax utility

var res = [];

function *reqData(url) {
    res.push(
        yield request( url )
    );
}

var it1 = reqData( "http://some.url.1" );
var it2 = reqData( "http://some.url.2" );

var p1 = it1.next().value;
var p2 = it2.next().value;

p1
.then( function(data){
    it1.next( data );
    return p2;
} )
.then( function(data){
    it2.next( data );
} );
```

```js
// `request(..)` is a Promise-aware Ajax utility

var res = [];

function *reqData(url) {
    var data = yield request( url );

    // transfer control
    yield;

    res.push( data );
}

var it1 = reqData( "http://some.url.1" );
var it2 = reqData( "http://some.url.2" );

var p1 = it1.next().value;
var p2 = it2.next().value;

p1.then( function(data){
    it1.next( data );
} );

p2.then( function(data){
    it2.next( data );
} );

Promise.all( [p1,p2] )
.then( function(){
    it1.next();
    it2.next();
} );
```

```js
// `request(..)` is a Promise-aware Ajax utility

runAll(
    function*(data){
        data.res = [];

        // transfer control (and message pass)
        var url1 = yield "http://some.url.2";

        var p1 = request( url1 ); // "http://some.url.1"

        // transfer control
        yield;

        data.res.push( yield p1 );
    },
    function*(data){
        // transfer control (and message pass)
        var url2 = yield "http://some.url.1";

        var p2 = request( url2 ); // "http://some.url.2"

        // transfer control
        yield;

        data.res.push( yield p2 );
    }
);
```

### Thunks

In general computer science, there's an old pre-JS concept called a "thunk." Without getting bogged down in the historical nature, a narrow expression of a thunk in JS is a function that -- without any parameters -- is wired to call another function.

In other words, you wrap a function definition around function call -- with any parameters it needs -- to defer the execution of that call, and that wrapping function is a thunk. When you later execute the thunk, you end up calling the original function.

```js
// synchronous thunk
function foo(x,y) {
    return x + y;
}

function fooThunk() {
    return foo( 3, 4 );
}

// later

console.log( fooThunk() );  // 7
```


```js
// a synchronous thunk
function foo(x,y,cb) {
    setTimeout( function(){
        cb( x + y );
    }, 1000 );
}

function fooThunk(cb) {
    foo( 3, 4, cb );
}

// later

fooThunk( function(sum){
    console.log( sum );     // 7
} );
```


```js
// a utility that does this wrapping for us.
function thunkify(fn) {
    var args = [].slice.call( arguments, 1 );
    return function(cb) {
        args.push( cb );
        return fn.apply( null, args );
    };
}

var fooThunk = thunkify( foo, 3, 4 );

// later

fooThunk( function(sum) {
    console.log( sum );     // 7
} );
```




```js
// a better one "thunk" + "factory"
function thunkify(fn) {
    return function() {
        var args = [].slice.call( arguments );
        return function(cb) {
            args.push( cb );
            return fn.apply( null, args );
        };
    };
}

var whatIsThis = thunkify( foo );

var fooThunk = whatIsThis( 3, 4 );

// later

fooThunk( function(sum) {
    console.log( sum );     // 7
} );
```

#### s/promise/thunk/

Comparing thunks to promises generally: they're not directly interchangable as they're not equivalent in behavior. Promises are vastly more capable and trustable than bare thunks.

But in another sense, they both can be seen as a request for a value, which may be async in its answering.

Recall from Chapter 3 we defined a utility for promisifying a function, which we called Promise.wrap(..) -- we could have called it promisify(..), too! This Promise-wrapping utility doesn't produce Promises; it produces promisories that in turn produce Promises. This is completely symmetric to the thunkories and thunks presently being discussed.

```js
// symmetrical: constructing the question asker
var fooThunkory = thunkify( foo );
var fooPromisory = promisify( foo );

// symmetrical: asking the question
var fooThunk = fooThunkory( 3, 4 );
var fooPromise = fooPromisory( 3, 4 );

// get the thunk answer
fooThunk( function(err,sum){
    if (err) {
        console.error( err );
    }
    else {
        console.log( sum );     // 7
    }
} );

// get the promise answer
fooPromise
.then(
    function(sum){
        console.log( sum );     // 7
    },
    function(err){
        console.error( err );
    }
);
```

Symmetry wise, these two approaches look identical. However, we should point out that's true only from the perspective of Promises or thunks representing the future value continuation of a generator.

From the larger perspective, thunks do not in and of themselves have hardly any of the trustability or composability guarantees that Promises are designed with. Using a thunk as a stand-in for a Promise in this particular generator asynchrony pattern is workable but should be seen as less than ideal when compared to all the benefits that Promises offer (see Chapter 3).

If you have the option, prefer yield pr rather than yield th. But there's nothing wrong with having a run(..) utility which can handle both value types.
### Pre-ES6

#### Manual Transformation

#### Automatic Transpilation


### Review

Generators are a new ES6 function type that does not run-to-completion like normal functions. Instead, the generator can be paused in mid-completion (entirely preserving its state), and it can later be resumed from where it left off.

This pause/resume interchange is cooperative rather than preemptive, which means that the generator has the sole capability to pause itself, using the yield keyword, and yet the iterator that controls the generator has the sole capability (via next(..)) to resume the generator.

The yield / next(..) duality is not just a control mechanism, it's actually a two-way message passing mechanism. A yield .. expression essentially pauses waiting for a value, and the next next(..) call passes a value (or implicit undefined) back to that paused yield expression.

The key benefit of generators related to async flow control is that the code inside a generator expresses a sequence of steps for the task in a naturally sync/sequential fashion. The trick is that we essentially hide potential asynchrony behind the yield keyword -- moving the asynchrony to the code where the generator's iterator is controlled.

In other words, generators preserve a sequential, synchronous, blocking code pattern for async code, which lets our brains reason about the code much more naturally, addressing one of the two key drawbacks of callback-based async.

## Chapter 5: Program Performance

For example, if you have two Ajax requests to make, and they're independent, but you need to wait on them both to finish before doing the next task, you have two options for modeling that interaction: serial and concurrent.

You could make the first request and wait to start the second request until the first finishes. Or, as we've seen both with promises and generators, you could make both requests "in parallel," and express the "gate" to wait on both of them before moving on.

Clearly, the latter is usually going to be more performant than the former. And better performance generally leads to better user experience.

It's even possible that asynchrony (interleaved concurrency) can improve just the perception of performance, even if the overall program still takes the same amount of time to complete. User perception of performance is every bit -- if not more! -- as important as actual measurable performance.


### SIMD

Single instruction, multiple data (SIMD) is a form of "data parallelism," as contrasted to "task parallelism" with Web Workers, because the emphasis is not really on program logic chunks being parallelized, but rather multiple bits of data being processed in parallel.

With SIMD, threads don't provide the parallelism. Instead, modern CPUs provide SIMD capability with "vectors" of numbers -- think: type specialized arrays -- as well as instructions that can operate in parallel across all the numbers; these are low-level operations leveraging instruction-level parallelism.

The effort to expose SIMD capability to JavaScript is primarily spearheaded by Intel (https://01.org/node/1495), namely by Mohammad Haghighat (at the time of this writing), in cooperation with Firefox and Chrome teams. SIMD is on an early standards track with a good chance of making it into a future revision of JavaScript, likely in the ES7 timeframe.

SIMD JavaScript proposes to expose short vector types and APIs to JS code, which on those SIMD-enabled systems would map the operations directly through to the CPU equivalents, with fallback to non-parallelized operation "shims" on non-SIMD systems.

The performance benefits for data-intensive applications (signal analysis, matrix operations on graphics, etc.) with such parallel math processing are quite obvious!
### Review

The first four chapters of this book are based on the premise that async coding patterns give you the ability to write more performant code, which is generally a very important improvement. But async behavior only gets you so far, because it's still fundamentally bound to a single event loop thread.

So in this chapter we've covered several program-level mechanisms for improving performance even further.

Web Workers let you run a JS file (aka program) in a separate thread using async events to message between the threads. They're wonderful for offloading long-running or resource-intensive tasks to a different thread, leaving the main UI thread more responsive.

SIMD proposes to map CPU-level parallel math operations to JavaScript APIs for high-performance data-parallel operations, like number processing on large data sets.

Finally, asm.js describes a small subset of JavaScript that avoids the hard-to-optimize parts of JS (like garbage collection and coercion) and lets the JS engine recognize and run such code through aggressive optimizations. asm.js could be hand authored, but that's extremely tedious and error prone, akin to hand authoring assembly language (hence the name). Instead, the main intent is that asm.js would be a good target for cross-compilation from other highly optimized program languages -- for example, Emscripten (https://github.com/kripken/emscripten/wiki) transpiling C/C++ to JavaScript.

While not covered explicitly in this chapter, there are even more radical ideas under very early discussion for JavaScript, including approximations of direct threaded functionality (not just hidden behind data structure APIs). Whether that happens explicitly, or we just see more parallelism creep into JS behind the scenes, the future of more optimized program-level performance in JS looks really promising.


## Chapter 6: Benchmarking & Tuning

The more important goal of this chapter is to figure out what kinds of JS performance matter and which ones don't, and how to tell the difference.

But even before we get there, we need to explore how to most accurately and reliably test JS performance, because there's tons of misconceptions and myths that have flooded our collective cult knowledge base. We've got to sift through all that junk to find some clarity.

###  Review

Effectively benchmarking performance of a piece of code, especially to compare it to another option for that same code to see which approach is faster, requires careful attention to detail.

Rather than rolling your own statistically valid benchmarking logic, just use the Benchmark.js library, which does that for you. But be careful about how you author tests, because it's far too easy to construct a test that seems valid but that's actually flawed -- even tiny differences can skew the results to be completely unreliable.

It's important to get as many test results from as many different environments as possible to eliminate hardware/device bias. jsPerf.com is a fantastic website for crowdsourcing performance benchmark test runs.

Many common performance tests unfortunately obsess about irrelevant microperformance details like x++ versus ++x. Writing good tests means understanding how to focus on big picture concerns, like optimizing on the critical path, and avoiding falling into traps like different JS engines' implementation details.

Tail call optimization (TCO) is a required optimization as of ES6 that will make some recursive patterns practical in JS where they would have been impossible otherwise. TCO allows a function call in the tail position of another function to execute without needing any extra resources, which means the engine no longer needs to place arbitrary restrictions on call stack depth for recursive algorithms.