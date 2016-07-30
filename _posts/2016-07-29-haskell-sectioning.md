---
layout: post
title: Sectioning and the Application Operator in Haskell
---


I've been learning Haskell recently, and I came across a problem that shows the intricacies of what can be a very simple and straightforward operator: the "$" ("application") operator.  It's a great example about how even an apparently simple and straightforward tool like `$` can still become quite counter-intuitive to use.  My biggest piece of advice so far for learning a functional language like Haskell is to pay very close attention to the basic rules, specifically the rules of how functions and operators are combined and applied to one another--because they are rarely as basic as the look. As quickly becomes obvious, functional languages are very terse compared to their object-oriented counterparts, and this terseness conceals a great deal of logic that you can fool yourself into thinking you understand until--suddenly--you don't anymore and start questioning your foundation.  It forces you to keep one eye on the fact that functional programming is an applied set of mathematical rules, and to be very deliberate about your understanding of these rules as you write your code.

In its most straightforward form, the `$` operator simply tells your program to evaluate something that it would have already evaluated without the `$.` The $ operator can be used in a form that is both redundant and unnecessary, for instance when I want to multiply two numbers I can write:

```haskell
(* 3) $ 2
```

It looks weird, but it's just basic multiplication.  I could just write `3 * 2` if I used the `*` function as an infix operator (i.e., between two numbers that are the arguments to `*`)  or `(*) 3 2` in its prefix form.  `*` is just a function that takes two integer arguments.  `$` is often used in place of parentheses to make a statement easier to read by eliminating parens, which means that the above expression is also equivalent to (* 3) (2).

So what is the `$` adding?  Nothing really so far.  It's just saying "evaluate this thing on the right, and supply it as an argument to this thing on the left."  But in special cases it becomes indispensable.   Take this simple example, which creates a list consisting of the first five integers starting with 1, and performs a simple "map" transformation that multiplies every integer by 2.

```haskell
>let x = map (*2) $ take 5 [1..]
> x
> [2,4,6,8,10]
```
This statement throws an error without the `$`, because we need all of the arguments after the $ to evaluate before we have a list to pass to the map. This is because map is right-associative--everything to its right evaluates (with a low precedence of 0) before it's passed to the function on the left.  First special property...  If we examine the source code for map's implementation, we'll see that it takes two arguments, and that it's defined recursively:

```haskell
map f []     = []
map f (x:xs) = f x : map f xs
```

The argument, `f`, is the function that is applied to each element of the list and `[]` or `(x:xs)` are the base and recursive cases, respectively, for our list to be mapped.  If I took the above multiply-by-two example and expanded how the list looks post-recursion but before evaluation, it would be this:

```haskell
((*2) 1 :((*2) 2: ((*2) 3: ((*2) 4: ((*2) 5: [])))))
```

The innermost parens on the right evaluate first, with our function `(*2)` applied to `5`, and it is then inserted into a new empty list.

As you can see from the example, the standard implementation of map takes a function, `f`, and applies it to every value in the list. But what if we wanted to do the reverse?  Let's say we had a list of functions--say, tests for whether a value passes a specific condition-- and we wanted to apply each function to a single integer value, like the following, then return whether the number passed that condition.  This list of functions tests for whether a number is greater than 0, less than 100, and even:

```haskell
funcList = [(> 0),(< 100),(\x -> (x `mod` 2 ) == 0)]
```
Ideally, we'd like to be able to pass each function to an integer, then return a list of booleans that tell us whether it passed the test, like `map 31 funcList`.  Ideally, if we passed in this `31`, we'd get `[True,True,False]` in return.  But we can't do this using the standard approach to map like we did above, because the the function and value would be in the wrong order once map expanded the expression.  Because of how map evaluates, instead of `(< 100) 31`, we'd have `31 (> 100)`, which throws an error because the `>` operator isn't passed the second argument,`31`, in an order that it recognizes.

Enter the `$` operator, and two concepts that come into play to help us solve our problem.  First, it's important to realize that when we define some but not all of the arguments to a function using parens, like the `(> 0)` test above, this is called a function *section*.  For function sections using infix operators, there are special rules for how it takes the argument for the missing section.  `>` needs two arguments to evaluate, but within a function section, the order matters.  For instance, `(> 0) 1` and `(0 >) 1` return opposite values.  The first is equivalent to `1 > 0` (`True`) and the second (`0 > 1`) (`False`).  If we write them like functions, `(> 0)` is like passing `\x -> x > 0` and `(0 >)` is like `\x -> 0 > x`.  Switching the order of the elements within the infix section is like reversing the variables within your function. (The order by which an infix operator takes its arguments depends on whether it is right or left-associative)

We can take these properties of infix operators within sections and use them to solve our `map` problem using `$`.  Recall that `$` is incredibly simple: it just says, "take this thing and apply it as an argument to this other thing!"  What if we tried to evaluate `($ 31)` and apply it to another function. `$` is an infix operator just like `>` or `*`. And `$` is right associative, so `($  31)` and `(31 $)` are equivalent to `(\x -> x $ 31)` and `(\x -> 31 $ x)` respectively. `$` doesn't care what x it is, and since this is a functional language, functions can be passed in as bound variables to other functions just like any other value.  Therefore, if we pass `($  31)` to `map`, it evaluates like:

```haskell
map ($ 31) funcList
($ 31) (> 2)
```

For our first predicate, which is equivalent to

```haskell
($ 31) (> 2)
(\x -> x $ 31)
(> 2) $ 31
(> 2) 31
True
```

And there's our answer! It returns `True`.  We've effectively reversed the way that map deals with its arguments through careful use of `$`'s properties as an infix section'. `f` becomes our value and the the list becomes our function, and we can turn them back around at the point of evaluation to make them evaluate in the correct order using `($ x)` to re-configure our arguments.

[Sections and Infix Operators](https://wiki.haskell.org/Section_of_an_infix_operator)
