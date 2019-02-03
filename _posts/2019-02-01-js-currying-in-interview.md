---
layout: post
title: >-
    Currying in JS: Answering the traditional question, Add(2)(3), which gives sum of both numbers.
subtitle: >-
    Understanding concept of currying and in depth analysis of most frequent interview question around it
avatar: /img/avatars/avatar-js.png
gist: >-
    We will have a look at concepts that revolve around this question and progressively take it to next level.
categories:
    - JavaScript
    - JS
    - Questions
tag:
    - JavaScript
    - Interview-Questions

draft: false
---

### Table of contents

<!-- toc -->

<!-- tocstop -->

### First, Implement `add(2)(3)` in JavaScript

To begin with, if we do a simple analysis we may simply state that this is not a problem just for JavaScript but can be implemented in any language that has First Class functions.

A programming language is said to have First-class functions when functions in that language are treated like any other variable. For example, in such a language, a function can be passed as an argument to other functions, can be returned by another function and can be assigned as a value to a variable.

Now we just need to create a function which returns another function, which in turn would give the sum. That's it.

Before moving on, please try this problem yourself, if you have encountered it for first time.

Solution,

    function add(x){
        return function (y){
            return x+y;
        }
    }

Which can also be implemented in ES6 using arrow functions as,

    const add = x => y => x+y;

This problem is nothing but concept of `currying` in JS.

### What is currying?

Currying is a technique of evaluating a function with multiple arguments, into sequence of function with single/multiple argument.
In above problem, we simply turn up `add(2,3)` into `add(2)(3)`.

You can dig into currying from this [article](https://bjouhier.wordpress.com/2011/04/04/currying-the-callback-or-the-essence-of-futures/).

### Variants of add(2)(3)

#### `add(2)(3)(4)...`, for endless number of parameters

Hmmm, we know how to handle the summation and returning function (along with closure) but we arent sure when to stop, which implies, when would primary function return the result and when would it reture another curried function. There are possibily two options,

-   Making use of `valueOf` property
    We have already see
-   valueOf
-   () as last

#### add(2)(3)

-   simple solution
-   next level

-   will only consider 3

#### Explanation

### N number of parameters Add()()()()

## Github Gist

-   [`add(2)(3)` implementation in JS.](https://gist.github.com/anubhavsrivastava/9baa61b12abe8d8a952f762f886e477b)

## References

-   [Article - Currying the callback, or the essence of futures](https://bjouhier.wordpress.com/2011/04/04/currying-the-callback-or-the-essence-of-futures/)

---
