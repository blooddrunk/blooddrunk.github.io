---
title: 关于Javascript的变量提升(Hoisting)
date: 2016-12-07 22:05:07
tags:
  - Javascript
  - ES6
categories:
  - 前端
  - Javascript
---

# var vs let

> http://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var-to-declare-a-variable

主要区别在于作用域。`var`声明的变量的作用域为最近函数作用域，`let`声明的变量仅当前的块级作用域有效。

<!-- more -->

1.  **全局作用域**  
    定义在全局时，`var`和`let`表现的非常相似

    ```javascript
    let a = 'foo';  // globally scoped
    var b = 'bar'; // globally scoped
    ```
    然而，与`var`不同，`let`定义在全局的变量不会成为`window`对象的一个属性

    ```javascript
    console.log(window.a); // undefined
    console.log(window.b); // 'bar'
    ```


2.  **重复声明**  
    在严格模式下，`var`可以在当前作用域下重复声明同一变量，`let`则不可以

    ```javascript
    'use strict';
    let a = 'foo';
    let a = 'bar'; // SyntaxError: Identifier 'me' has already been declared

    ...

    'use strict';
    var b = 'foo';
    var b = 'bar'; // No problem, `bme` is replaced.
    ```

# `let`/`const`声明的变量不会被提升？

> http://stackoverflow.com/questions/31219420/are-variables-declared-with-let-or-const-not-hoisted-in-es6?rq=1

实际上，javascript中所有的声明(`var`, `let`, `const`, `function`, `function*`, `class`)都会被提升。所以可以说，如果在某个作用域中定义一个变量，那么这个变量总是指向其在该作用域中的值。在函数作用域和块作用域中都是如此

```javascript
x = "global";
// function scope:
(function() {
    x; // not "global"

    var/let/… x;
}());
// block scope (not for `var`s):
{
    x; // not "global"

    let/const/… x;
}
```

`var`/`function`/`function*`和`let`/`const`/`class`声明的区别在于**初始化**。  
前者在'绑定(binding)'创建时，在作用域的最顶端被初始化为`undefined`；而后者并未被初始化，因此当你试图访问它时会抛出`ReferenceError`异常。
只有代码执行到`let`/`const`/`class`语句本身时初始化才发生，从作用域顶端(提升的位置)到`let`/`const`/`class`声明被执行到之前之间的这个语法上的区域，被称为*temporal dead zone*

> let vs const  
  在变量提升上，`let`和`const`表现的完全一致，两者的唯一区别在于`const`能切仅能在初始化时被赋值一次。
