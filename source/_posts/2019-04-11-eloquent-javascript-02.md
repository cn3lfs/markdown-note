---
title: eloquent_javascript_02
author: bro wen
date: 2019-04-11 21:08:22
tags: [eloquent javascript, JavaScript]
---
# Eloquent JavaScript
##  Expressions and statements
A fragment of code that produces a value is called an expression. Every value that is written literally (such as 22 or "psychoanalysis") is an expression. An expression between parentheses is also an expression, as is a binary operator applied to two expressions or a unary operator applied to one.Expressions can contain other expressions in a way similar to how subsentences in human languages are nested—a subsentence can contain its own subsentences, and so on. 

A statement stands on its own, so it amounts to something only if it affects the world. It could display something on the screen—that counts as changing the world—or it could change the internal state of the machine in a way that will affect the statements that come after it. These changes are called side effects. 

In some cases, JavaScript allows you to omit the semicolon at the end of a statement. In other cases, it has to be there, or the next line will be treated as part of the same statement. The rules for when it can be safely omitted are somewhat complex and error-prone.

## Bindings---Define a variable
 To catch and hold values, JavaScript provides a thing called a binding, or variable:
 let caught = 5 * 5;

That’s a second kind of statement. The special word (keyword) let indicates that this sentence is going to define a binding. It is followed by the name of the binding and, if we want to immediately give it a value, by an = operator and an expression.

The previous statement creates a binding called caught and uses it to grab hold of the number that is produced by multiplying 5 by 5.

After a binding has been defined, its name can be used as an expression. The value of such an expression is the value the binding currently holds.

### variable reference:
When a binding points at a value, that does not mean it is tied to that value forever. The = operator can be used at any time on existing bindings to disconnect them from their current value and have them point to a new one.You should imagine bindings as tentacles, rather than boxes. They do not contain values; they grasp them—two bindings can refer to the same value. A program can access only the values that it still has a reference to. When you need to remember something, you grow a tentacle to hold on to it or you reattach one of your existing tentacles to it.

### undefined
定义时未赋值:
When you define a binding without giving it a value, the tentacle has nothing to grasp, so it ends in thin air. If you ask for the value of an empty binding, you’ll get the value undefined

### let const var
A single let statement may define multiple bindings. The definitions must be separated by commas.
    let one = 1, two = 2;

The words var and const can also be used to create bindings, in a way similar to let.

    var name = "Ayda";
    const greeting = "Hello ";

The first, var (short for “variable”), is the way bindings were declared in pre-2015 JavaScript.

The word const stands for constant. It defines a constant binding, which points at the same value for as long as it lives. This is useful for bindings that give a name to a value so that you can easily refer to it later.

## Binding names
不能以数字开头, 可以有$ _ 字母:
Binding names can be any word. Digits can be part of binding names—catch22 is a valid name, for example—but the name must not start with a digit. A binding name may include dollar signs ($) or underscores (_) but no other punctuation or special characters.

Words with a special meaning, such as let, are keywords, and they may not be used as binding names. There are also a number of words that are “reserved for use” in future versions of JavaScript, which also can’t be used as binding names. The full list of keywords and reserved words is rather long.

## The environment

The collection of bindings and their values that exist at a given time is called the environment. When a program starts up, this environment is not empty. It always contains bindings that are part of the language standard, and most of the time, it also has bindings that provide ways to interact with the surrounding system.

比如一些DOM操作, 还是一些浏览器暴露的api

## Functions

A lot of the values provided in the default environment have the type function. A function is a piece of program wrapped in a value. Such values can be applied in order to run the wrapped program.

Executing a function is called invoking, calling, or applying it. You can call a function by putting parentheses after an expression that produces a function value. Usually you’ll directly use the name of the binding that holds the function. The values between the parentheses are given to the program inside the function. Values given to functions are called arguments. Different functions might need a different number or different types of arguments.

## The console.log function
In the examples, I used console.log to output values. Most JavaScript systems (including all modern web browsers and Node.js) provide a console.log function that writes out its arguments to some text output device. 

## Return values

Showing a dialog box or writing text to the screen is a side effect. A lot of functions are useful because of the side effects they produce. Functions may also produce values, in which case they don’t need to have a side effect to be useful.
When a function produces a value, it is said to return that value. Anything that produces a value is an expression in JavaScript, which means function calls can be used within larger expressions.

## Control flow
执行顺序, 自上而下, 一般是在stack
When your program contains more than one statement, the statements are executed as if they are a story, from top to bottom. 

## Conditional execution
if else:
Not all programs are straight roads. We may, for example, want to create a branching road, where the program takes the proper branch based on the situation at hand. This is called conditional execution.
Conditional execution is created with the if keyword in JavaScript. In the simple case, we want some code to be executed if, and only if, a certain condition holds. 

The statement after the if is wrapped in braces ({ and }) in this example. The braces can be used to group any number of statements into a single statement, called a block. You could also have omitted them in this case, since they hold only a single statement, but to avoid having to think about whether they are needed, most JavaScript programmers use them in every wrapped statement like this. 

You often won’t just have code that executes when a condition holds true, but also code that handles the other case. This alternate path is represented by the second arrow in the diagram. You can use the else keyword, together with if, to create two separate, alternative execution paths. If you have more than two paths to choose from, you can “chain” multiple if/else pairs together.

## while and do loops

That works, but the idea of writing a program is to make something less work, not more. If we needed all even numbers less than 1,000, this approach would be unworkable. What we need is a way to run a piece of code multiple times. This form of control flow is called a loop.Looping control flow allows us to go back to some point in the program where we were before and repeat it with our current program state. 

只要条件为真, 就一直执行, 先判断, 再执行
while loop:
A statement starting with the keyword while creates a loop. The word while is followed by an expression in parentheses and then a statement, much like if. The loop keeps entering that statement as long as the expression produces a value that gives true when converted to Boolean.

至少执行一次,先执行后判断
do while loop:
A do loop is a control structure similar to a while loop. It differs only on one point: a do loop always executes its body at least once, and it starts testing whether it should stop only after that first execution. To reflect this, the test appears after the body of the loop.

## Indenting Code

良好的缩进保持代码整洁:
In the examples, I’ve been adding spaces in front of statements that are part of some larger statement. These spaces are not required—the computer will accept the program just fine without them. In fact, even the line breaks in programs are optional. You could write a program as a single long line if you felt like it.

The role of this indentation inside blocks is to make the structure of the code stand out. In code where new blocks are opened inside other blocks, it can become hard to see where one block ends and another begins. With proper indentation, the visual shape of a program corresponds to the shape of the blocks inside it. 

## for loops

Many loops follow the pattern shown in the while examples. First a “counter” binding is created to track the progress of the loop. Then comes a while loop, usually with a test expression that checks whether the counter has reached its end value. At the end of the loop body, the counter is updated to track progress.

Because this pattern is so common, JavaScript and similar languages provide a slightly shorter and more comprehensive form, the for loop.
The parentheses after a for keyword must contain two semicolons. The part before the first semicolon initializes the loop, usually by defining a binding. The second part is the expression that checks whether the loop must continue. The final part updates the state of the loop after every iteration. In most cases, this is shorter and clearer than a while construct.

## Breaking Out of a Loop
Having the looping condition produce false is not the only way a loop can finish. There is a special statement called break that has the effect of immediately jumping out of the enclosing loop.

是否可以被某个数字整除
Using the remainder (%) operator is an easy way to test whether a number is divisible by another number. If it is, the remainder of their division is zero.

无限循环
The for construct in the example does not have a part that checks for the end of the loop. This means that the loop will never stop unless the break statement inside is executed.f you were to remove that break statement or you accidentally write an end condition that always produces true, your program would get stuck in an infinite loop. A program stuck in an infinite loop will never finish running, which is usually a bad thing.

直接结束这一轮, 跳到下一轮
The continue keyword is similar to break, in that it influences the progress of a loop. When continue is encountered in a loop body, control jumps out of the body and continues with the loop’s next iteration.

## Updating bindings succinctly

Especially when looping, a program often needs to “update” a binding to hold a value based on that binding’s previous value.

counter = counter + 1;

JavaScript provides a shortcut for this.

counter += 1;

Similar shortcuts work for many other operators, such as result *= 2 to double result or counter -= 1 to count downward.
For counter += 1 and counter -= 1, there are even shorter equivalents: counter++ and counter--

## Dispatching on a value with switch

There is a construct called switch that is intended to express such a “dispatch” in a more direct way. Unfortunately, the syntax JavaScript uses for this (which it inherited from the C/Java line of programming languages) is somewhat awkward—a chain of if statements may look better.

忘记break会使程序继续执行下去, 触发多个选项
You may put any number of case labels inside the block opened by switch. The program will start executing at the label that corresponds to the value that switch was given, or at default if no matching value is found. It will continue executing, even across other labels, until it reaches a break statement. But be careful—it is easy to forget such a break, which will cause the program to execute code you do not want executed.

## Capitalization

Binding names may not contain spaces, yet it is often helpful to use multiple words to clearly describe what the binding represents.
The first style can be hard to read. I rather like the look of the underscores, though that style is a little painful to type. The standard JavaScript functions, and most JavaScript programmers, follow the bottom style—they capitalize every word except the first. It is not hard to get used to little things like that, and code with mixed naming styles can be jarring to read, so we follow this convention

## Comments
多注释
Often, raw code does not convey all the information you want a program to convey to human readers, or it conveys it in such a cryptic way that people might not understand it. At other times, you might just want to include some related thoughts as part of your program. This is what comments are for.

A comment is a piece of text that is part of a program but is completely ignored by the computer. JavaScript has two ways of writing comments. To write a single-line comment, you can use two slash characters (//) and then the comment text after it.

## Summary
1. 介绍了变量 let const var
2. flow control: if, for loop, while loop, switch case
3. function

后面是三个小游戏:

附上第三个的代码:

```js
let row = "";
for (let i=0; i<8; i++) {
    for (let j=0; j<8; j++) {
        if ( (i + j) % 2 === 0) {
            row += " ";
        } else {
            row += "#";
        }
    }
    row += "\n";
}
console.log(row);
```
