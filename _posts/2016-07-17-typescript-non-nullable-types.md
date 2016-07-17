---
layout: post
title: Non-Nullable Types in TypeScript 2.0
---


As part of an [Angular 2 side project](https://github.com/pbsugg/NoteBlog) over the last several months, I've had a chance to use Microsoft's [TypeScript](https://www.typescriptlang.org), a Javascript superset language that is fully compatible with and compiles to Javascript. TypeScript supports many of the newest features of the ECMAScript 6 (2015) standard like a more recognizable class and module syntax, and it continues in the tradition of other transcompilers like CoffeeScript (i.e., a Ruby-to-Javascript system) that allow developers to use the more explicit, object oriented functionality of another language to make working with Javascript a bit easier. Most importantly, since Angular 2 is a front-end Javascript framework that is built in TypeScript, it has clearly convinced some important developers that it has a role to play in Javascript's future. That was enough to make me want to dip my toes in it.  TypeScript doesn't have a steep learning curve, and one quickly finds that it retains a lot of Javascript's syntax conventions, with one major difference that TypeScript's name makes clear: static typing.  

One of the new features in TypeScript's version 2.0 beta, [released last week](https://blogs.msdn.microsoft.com/typescript/2016/07/11/announcing-typescript-2-0-beta/) is its so-called "non-nullable" typing checks. Dealing with errors caused by variables that are unintentionally null or undefined is an extremely common problem. Not only does TypeScript's new feature represent an interesting solution to a problem that all programmers face (no matter whether they are using static or dynamically-typed languages), it also reveals some of the underlying functionality of TypeScript's static typing system, and it's a useful illustration of the power of static languages to solve a problem like null typing.

Consider the following variable assignments, which are compiled with TypeScript's new `--strictNullchecks` flag:

```javascript
let x: string;
x = "hello";
let y: string | null;
y = null;
let z: string | null | undefined;
z = undefined;
```

All of the above variable assignments are valid and the program compiles.  But if I flipped "x" and z," so that `x=undefined` and `z=hello`, the compiler would throw an error for x's value.  With the `--stringNullchecks` flag raised, typing assignment means what it says and **only** what it says. A variable can be assigned multiple types via the "union" (`|`) operator. Under TypeScript's particular implementation of null checking, "null" and "undefined" are distinct and separate, so if I had referenced `y` without first assigning it a value, I would have gotten a compiler error, whereas a reference  to `z` without an assigned value would not have thrown an error.

Not a terribly challenging concept, but it's clear that this can still be a useful feature for spotting assignment errors before they occur.  Let's say I have a value ("outsideValue") that depends on an external API or other piece of code that I can't directly control (we'll imagine here that it's a boolean). And let's say that there is a 10% possibility, which we represent with a random value, that something goes wrong with this API and my outside value comes back without being defined (represented by my calling `toString()` in the `else` fork below). Without `strictNullChecks` set to "false," the following code compiles with no errors:

```javascript
let outsideValue: boolean;
let chanceOfNull = Math.random();
if(chanceOfNull >= .1){
  outsideValue = true;
  outsideValue.toString();
}
else{
  outsideValue.toString();
};
```

Now, in that 10% window when `outsideValue` remains undefined, if we call a method on our value (`toString()` here), we're going to throw an error in runtime. But without `strictNullChecks` enabled, the program will still compile and run.  Once we enable `strictNullChecks`, we get a compiler error: that `outsideValue` is being used before it is assigned. We got no error without `strictNullChecks` because `null` and `undefined` are included in **all** types.  Our boolean came with an implicit null and undefined types, and that feature came back to bite us when we called `toString()`" on them.  But with `strictNullChecks`, our compiler followed the flow of logic in our program and saw that there was a chance `toString()` would be called on a null value.  Since we didn't explicitly tell `outsideValue` that it could be null, we didn't get that type for free with type `boolean`, and we got an error.  Just like we wouldn't get an implicit `string` type with `boolean`, the assignment of the `null` type has been made explicit--you only get it when you ask for it.

The logic under the hood that allows for `strictNullChecks` is "control flow analysis," a system of following the "flow" of logic through the branches in your program and isolating possible types to their most explicit form at all points.  Read more about its implementation for TypeScript in the pull requests that implement the `strictNullChecks` functionality [here](https://github.com/Microsoft/TypeScript/pull/8010) and [here](https://github.com/Microsoft/TypeScript/pull/7140). It's a feature that demonstrates why one might prefer the slightly greater effort required to use a statically typed language in the first place: fewer unintended errors down the line.
