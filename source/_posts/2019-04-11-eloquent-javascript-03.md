---
title: eloquent_javascript_03
author: bro wen
date: 2019-04-11 23:10:53
tags: [eloquent javascript, JavaScript]
---
# Eloquent JavaScript

Function是我们用来减少重复代码的一种包装好的东西, 让我们可以反复使用, 减少程序的复杂性
The concept of wrapping a piece of program in a value has many uses. It gives us a way to structure larger programs, to reduce repetition, to associate names with subprograms, and to isolate these subprograms from each other.

这里采用ES2015标准定义函数
A function definition is a regular binding where the value of the binding is a function. For example, this code defines square to refer to a function that produces the square of a given number:

const square = function(x) {
  return x * x;
};

A function is created with an expression that starts with the keyword function. Functions have a set of parameters (in this case, only x) and a body, which contains the statements that are to be executed when the function is called. The function body of a function created this way must always be wrapped in braces, even when it consists of only a single statement.

A function can have multiple parameters or no parameters at all.

return会立即跳出当前的Function, 并将指定的值返回给调用它的地方, 如果没有指定返回值, 会返回undefined.
Some functions produce a value, such as power and square, and some don’t, such as makeNoise, whose only result is a side effect. A return statement determines the value the function returns. When control comes across such a statement, it immediately jumps out of the current function and gives the returned value to the code that called the function. A return keyword without an expression after it will cause the function to return undefined. Functions that don’t have a return statement at all, such as makeNoise, similarly return undefined. 
Parameters to a function behave like regular bindings, but their initial values are given by the caller of the function, not the code in the function itself.

## Bindings and scopes
这里讲的是this
Each binding has a scope, which is the part of the program in which the binding is visible. For bindings defined outside of any function or block, the scope is the whole program—you can refer to such bindings wherever you want. These are called global.

local scope:
But bindings created for function parameters or declared inside a function can be referenced only in that function, so they are known as local bindings. Every time the function is called, new instances of these bindings are created. This provides some isolation between functions—each function call acts in its own little world (its local environment) and can often be understood without knowing a lot about what’s going on in the global environment.

ES2015的block scope与之前的function scope的区别, block scope比如for loop, 在loop前面以及loop后面的东西都不能访问loop里面的变量, 而function scope是你只要在function内, 我就可以随意访问, 只有function 才可以创建新的scope
Bindings declared with let and const are in fact local to the block that they are declared in, so if you create one of those inside of a loop, the code before and after the loop cannot “see” it. In pre-2015 JavaScript, only functions created new scopes, so old-style bindings, created with the var keyword, are visible throughout the whole function that they appear in—or throughout the global scope, if they are not in a function.

变量mask
Each scope can “look out” into the scope around it, so x is visible inside the block in the example. The exception is when multiple bindings have the same name—in that case, code can see only the innermost one.

## Nested scope
function或block里面可以再放function以及block
JavaScript distinguishes not just global and local bindings. Blocks and functions can be created inside other blocks and functions, producing multiple degrees of locality.

里面的function可以看见外面function的变量, 反过来不行
The code inside the ingredient function can see the factor binding from the outer function. But its local bindings, such as unit or ingredientAmount, are not visible in the outer function.

lexical scope:
The set of bindings visible inside a block is determined by the place of that block in the program text. Each local scope can also see all the local scopes that contain it, and all scopes can see the global scope. This approach to binding visibility is called lexical scoping.

## Functions as values
function identifier 同样也是一个变量, 拥有变量所有的所有特性, 可以再次被赋值, 如果不是用const定义的话, 然后function本身是一个值, 拥有值得所有特性, 可以被用作expression, 所以可以用在另一个function里面, 当成参数, 这就是callback.
A function binding usually simply acts as a name for a specific piece of the program. Such a binding is defined once and never changed. This makes it easy to confuse the function and its name.

But the two are different. A function value can do all the things that other values can do—you can use it in arbitrary expressions, not just call it. It is possible to store a function value in a new binding, pass it as an argument to a function, and so on. Similarly, a binding that holds a function is still just a regular binding and can, if not constant, be assigned a new value, like so:

let launchMissiles = function() {
  missileSystem.launch("now");
};
if (safeMode) {
  launchMissiles = function() {};
}

## Declaration notation

There is a slightly shorter way to create a function binding. When the function keyword is used at the start of a statement, it works differently.

function square(x) {
  return x * x;
}

This is a function declaration. The statement defines the binding square and points it at the given function. It is slightly easier to write and doesn’t require a semicolon after the function.

function hoisting:
There is one subtlety with this form of function definition.

console.log("The future says:", future());

function future() {
  return "You'll never have flying cars";
}

The preceding code works, even though the function is defined below the code that uses it. Function declarations are not part of the regular top-to-bottom flow of control. They are conceptually moved to the top of their scope and can be used by all the code in that scope. This is sometimes useful because it offers the freedom to order code in a way that seems meaningful, without worrying about having to define all functions before they are used.

## Arrow functions

There’s a third notation for functions, which looks very different from the others. Instead of the function keyword, it uses an arrow (=>) made up of an equal sign and a greater-than character (not to be confused with the greater-than-or-equal operator, which is written >=).

const power = (base, exponent) => {
  let result = 1;
  for (let count = 0; count < exponent; count++) {
    result *= base;
  }
  return result;
};

The arrow comes after the list of parameters and is followed by the function’s body. It expresses something like “this input (the parameters) produces this result (the body)”.

When there is only one parameter name, you can omit the parentheses around the parameter list. If the body is a single expression, rather than a block in braces, that expression will be returned from the function. So, these two definitions of square do the same thing:

const square1 = (x) => { return x * x; };
const square2 = x => x * x;

When an arrow function has no parameters at all, its parameter list is just an empty set of parentheses.

const horn = () => {
  console.log("Toot");
};

## The call stack

stack是常见的数据结构, 后进后出
The way control flows through functions is somewhat involved. Because a function has to jump back to the place that called it when it returns, the computer must remember the context from which the call happened. In one case, console.log has to return to the greet function when it is done. In the other case, it returns to the end of the program.

每次function执行完, 需要返回到调用它的地方, 因此需要记住那个地方---context, 每次执行完就会移除最顶层的 call stack.
The place where the computer stores this context is the call stack. Every time a function is called, the current context is stored on top of this stack. When a function returns, it removes the top context from the stack and uses that context to continue execution.

过多次使用stack会导致内存不足报错, 比如多次recursion
Storing this stack requires space in the computer’s memory. When the stack grows too big, the computer will fail with a message like “out of stack space” or “too much recursion”. The following code illustrates this by asking the computer a really hard question that causes an infinite back-and-forth between two functions. Rather, it would be infinite, if the computer had an infinite stack. As it is, we will run out of space, or “blow the stack”.

## Optional Arguments
js里面可以传递多个参数, 当传递进去的参数少于必须参数时, 不够的参数会用undefined补充, 从而可以使你的Function更加灵活, 你可以对这些undefined的参数做判断, 从而实现某些额外的功能
The following code is allowed and executes without any problem:

    function square(x) { return x * x; }
    console.log(square(4, true, "hedgehog"));

We defined square with only one parameter. Yet when we call it with three, the language doesn’t complain. It ignores the extra arguments and computes the square of the first one.

JavaScript is extremely broad-minded about the number of arguments you pass to a function. If you pass too many, the extra ones are ignored. If you pass too few, the missing parameters get assigned the value undefined.

The downside of this is that it is possible—likely, even—that you’ll accidentally pass the wrong number of arguments to functions. And no one will tell you about it.

The upside is that this behavior can be used to allow a function to be called with different numbers of arguments. For example, this minus function tries to imitate the - operator by acting on either one or two arguments:

    function minus(a, b) {
      if (b === undefined) return -a;
      else return a - b;
    }

## Closure

The ability to treat functions as values, combined with the fact that local bindings are re-created every time a function is called, brings up an interesting question. What happens to local bindings when the function call that created them is no longer active?

The following code shows an example of this. It defines a function, wrapValue, that creates a local binding. It then returns a function that accesses and returns this local binding.

```js
function wrapValue(n) {
  let local = n;
  return () => local;
}

let wrap1 = wrapValue(1);
let wrap2 = wrapValue(2);
console.log(wrap1());

console.log(wrap2());
```
每次调用function, 都要重新生成lcoal binding, 每次的都不一样
This is allowed and works as you’d hope—both instances of the binding can still be accessed. This situation is a good demonstration of the fact that local bindings are created anew for every call, and different calls can’t trample on one another’s local bindings.

closure就是可以访问enclosing scope 里面的 lcoal binds 的instance, 也就是一个function可以访问它的enclosing scope里面的 local bindings.
This feature—being able to reference a specific instance of a local binding in an enclosing scope—is called closure. A function that references bindings from local scopes around it is called a closure. This behavior not only frees you from having to worry about lifetimes of bindings but also makes it possible to use function values in some creative ways.

```js
function multiplier(factor) {
  return number => number * factor;
}

let twice = multiplier(2);
console.log(twice(5));
```

参数本身就是 local binding
The explicit local binding from the wrapValue example isn’t really needed since a parameter is itself a local binding.

把function分成两部分, 一部分是code本身, 另一部分是创建他们的environment, 当调用他们的时候, 他们的context 是创建他们的 environment, 而不是执行的地方的 environment
Thinking about programs like this takes some practice. A good mental model is to think of function values as containing both the code in their body and the environment in which they are created. When called, the function body sees the environment in which it was created, not the environment in which it is called.

In the example, multiplier is called and creates an environment in which its factor parameter is bound to 2. The function value it returns, which is stored in twice, remembers this environment. So when that is called, it multiplies its argument by 2.

## Recursion

It is perfectly okay for a function to call itself, as long as it doesn’t do it so often that it overflows the stack. A function that calls itself is called recursive. Recursion allows some functions to be written in a different style. Take, for example, this alternative implementation of power:

```js
function power(base, exponent) {
  if (exponent == 0) {
    return 1;
  } else {
    return base * power(base, exponent - 1);
  }
}

console.log(power(2, 3));
```

使用起来会让整个程序看起来更加简洁, 但是缺点是会比for loop要慢上3倍
This is rather close to the way mathematicians define exponentiation and arguably describes the concept more clearly than the looping variant. The function calls itself multiple times with ever smaller exponents to achieve the repeated multiplication.

But this implementation has one problem: in typical JavaScript implementations, it’s about three times slower than the looping version. Running through a simple loop is generally cheaper than calling a function multiple times.

是否使用recursion 更像是在机器语言之间和人类语言之间取得一个平衡, 并不是任何时候都以效率为先的, 特别是当一些问题已经特别复杂的时候, 再去考虑这些只会让问题变得更难解决, recursion 在对待多次分支的问题上比较有优势. 当效率确实成为了我们程序的瓶颈时候, 我们再去优化程序也不迟.
The dilemma of speed versus elegance is an interesting one. You can see it as a kind of continuum between human-friendliness and machine-friendliness. Almost any program can be made faster by making it bigger and more convoluted. The programmer has to decide on an appropriate balance.

In the case of the power function, the inelegant (looping) version is still fairly simple and easy to read. It doesn’t make much sense to replace it with the recursive version. Often, though, a program deals with such complex concepts that giving up some efficiency in order to make the program more straightforward is helpful.

Worrying about efficiency can be a distraction. It’s yet another factor that complicates program design, and when you’re doing something that’s already difficult, that extra thing to worry about can be paralyzing.

Recursion is not always just an inefficient alternative to looping. Some problems really are easier to solve with recursion than with loops. Most often these are problems that require exploring or processing several “branches”, each of which might branch out again into even more branches.

Consider this puzzle: by starting from the number 1 and repeatedly either adding 5 or multiplying by 3, an infinite set of numbers can be produced. How would you write a function that, given a number, tries to find a sequence of such additions and multiplications that produces that number?

For example, the number 13 could be reached by first multiplying by 3 and then adding 5 twice, whereas the number 15 cannot be reached at all.

Here is a recursive solution:
```js
function findSolution(target) {
  function find(current, history) {
    if (current == target) {
      return history;
    } else if (current > target) {
      return null;
    } else {
      return find(current + 5, `(${history} + 5)`) ||
             find(current * 3, `(${history} * 3)`);
    }
  }
  return find(1, "1");
}

console.log(findSolution(24));
```

## Growing functions

There are two more or less natural ways for functions to be introduced into programs.

发现代码重复了, 不想每次都写那么多, 所以写个function, 每次要用的时候import, 调用一下
The first is that you find yourself writing similar code multiple times. You’d prefer not to do that. Having more code means more space for mistakes to hide and more material to read for people trying to understand the program. So you take the repeated functionality, find a good name for it, and put it into a function.

觉得这个功能值得为他写个function, 先写出来再说
The second way is that you find you need some functionality that you haven’t written yet and that sounds like it deserves its own function. You’ll start by naming the function, and then you’ll write its body. You might even start writing code that uses the function before you actually define the function itself.

function naming意味着你对这个问题理解有多深
How difficult it is to find a good name for a function is a good indication of how clear a concept it is that you’re trying to wrap. 


We want to write a program that prints two numbers: the numbers of cows and chickens on a farm, with the words Cows and Chickens after them and zeros padded before both numbers so that they are always three digits long.

007 Cows
011 Chickens

This asks for a function of two arguments—the number of cows and the number of chickens. Let’s get coding.
```js
function printFarmInventory(cows, chickens) {
  let cowString = String(cows);
  while (cowString.length < 3) {
    cowString = "0" + cowString;
  }
  console.log(`${cowString} Cows`);
  let chickenString = String(chickens);
  while (chickenString.length < 3) {
    chickenString = "0" + chickenString;
  }
  console.log(`${chickenString} Chickens`);
}
printFarmInventory(7, 11);
```

有新的需求, 需要对功能做一些拓展时, 你的程序最好是scalable的, 否则会比较麻烦
Mission accomplished! But just as we are about to send the farmer the code (along with a hefty invoice), she calls and tells us she’s also started keeping pigs, and couldn’t we please extend the software to also print pigs?

```js
function printZeroPaddedWithLabel(number, label) {
  let numberString = String(number);
  while (numberString.length < 3) {
    numberString = "0" + numberString;
  }
  console.log(`${numberString} ${label}`);
}

function printFarmInventory(cows, chickens, pigs) {
  printZeroPaddedWithLabel(cows, "Cows");
  printZeroPaddedWithLabel(chickens, "Chickens");
  printZeroPaddedWithLabel(pigs, "Pigs");
}

printFarmInventory(7, 11, 3);
```

让一个function仅仅执行它本身的功能, 不要给一个function赋予太多功能, 这样可以降低你的程序之间的dependency.
It works! But that name, printZeroPaddedWithLabel, is a little awkward. It conflates three things—printing, zero-padding, and adding a label—into a single function.

Instead of lifting out the repeated part of our program wholesale, let’s try to pick out a single concept.

```js
function zeroPad(number, width) {
  let string = String(number);
  while (string.length < width) {
    string = "0" + string;
  }
  return string;
}

function printFarmInventory(cows, chickens, pigs) {
  console.log(`${zeroPad(cows, 3)} Cows`);
  console.log(`${zeroPad(chickens, 3)} Chickens`);
  console.log(`${zeroPad(pigs, 3)} Pigs`);
}

printFarmInventory(7, 16, 3);
```

A function with a nice, obvious name like zeroPad makes it easier for someone who reads the code to figure out what it does. And such a function is useful in more situations than just this specific program. For example, you could use it to help print nicely aligned tables of numbers.

最后我自己拓展了一下, 只需提交一个inventory array, 即可完成接下来的补0和打印
```js
function zeroPad(number, width) {
  let string = String(number);
  while (string.length < width) {
    string = "0" + string;
  }
  return string;
}

function printFarmInventory(...inventory) {
  for (var i = 0; i < inventory.length; i++) {
      console.log(`${zeroPad(inventory[i].number)} ${inventory[i].label}`);
  }
}

printFarmInventory([{label: 'Cows', number: 7}, {label: 'Chickens', number: 16}, {label: 'Pigs', number: 3}]);
```

确实需要的功能才加, 不要可能需要就加, 这样能保持你的程序简洁
A useful principle is to not add cleverness unless you are absolutely sure you’re going to need it. It can be tempting to write general “frameworks” for every bit of functionality you come across. Resist that urge. You won’t get any real work done—you’ll just be writing code that you never use.

## Functions and side effects

function有的只返回一个值, 有的还会一些其他作用, function 被 call之后会改变程序的一部分, 对于那些只返回值的并且自己自足的, 我们称之为pure function. 他不依赖 global bindings, 每次给一样的参数返回的都是一样的结果, 并且调用它的地方可以用它的一个返回值代替.
Functions can be roughly divided into those that are called for their side effects and those that are called for their return value. (Though it is definitely also possible to both have side effects and return a value.)

The first helper function in the farm example, printZeroPaddedWithLabel, is called for its side effect: it prints a line. The second version, zeroPad, is called for its return value. It is no coincidence that the second is useful in more situations than the first. Functions that create values are easier to combine in new ways than functions that directly perform side effects.

A pure function is a specific kind of value-producing function that not only has no side effects but also doesn’t rely on side effects from other code—for example, it doesn’t read global bindings whose value might change. A pure function has the pleasant property that, when called with the same arguments, it always produces the same value (and doesn’t do anything else). A call to such a function can be substituted by its return value without changing the meaning of the code. When you are not sure that a pure function is working correctly, you can test it by simply calling it and know that if it works in that context, it will work in any context. Nonpure functions tend to require more scaffolding to test.

## Summary

函数定义的三种方式

const f = function(a) {
    console.log(a + 2);
}

function g(a) {
    console.log(a + 2);
}

let h = a => a + 2;

Scope:

let const对应的是block scope, var 对应的是function scope 或 global scope
A key aspect in understanding functions is understanding scopes. Each block creates a new scope. Parameters and bindings declared in a given scope are local and not visible from the outside. Bindings declared with var behave differently—they end up in the nearest function scope or the global scope.

Separating the tasks your program performs into different functions is helpful. You won’t have to repeat yourself as much, and functions can help organize a program by grouping code into pieces that do specific things.