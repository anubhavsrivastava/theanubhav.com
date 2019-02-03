---
layout: post
title: >-
    Currying in JS: Answering the traditional question, Add(2)(3), which gives sum of both numbers.
subtitle: >-
    Understanding concept of currying and in depth analysis of most frequest interview question around it
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

# First, Implement `Add(2)(3)` in JavaScript

To begin with, if we do a simple analysis we may simply state that this is not a problem just for JavaScript but can be implemented in any language that has First Class functions.

A programming language is said to have First-class functions when functions in that language are treated like any other variable. For example, in such a language, a function can be passed as an argument to other functions, can be returned by another function and can be assigned as a value to a variable.

Now we just need to create a function which returns another function, which in turn would give the sum. That's it.

## Let's understand how can we make `(a===1 && a===2 && a===3)` to ever be true with help of getter-setter descriptors.

### Table of contents

<!-- toc -->

<!-- tocstop -->

### Problem 1 Add(2)(3)

If you are already familiar with this question and understand how can one solve this JavaScript tricky riddle (Yes, riddle, I don't see a use case in production code, ¯\\\_(ツ)\_/¯ ), you can move on to [next section](#problem-a1--a2--a3-strict-comparison) which is an attempt to solve the extension of it (with strict equality).
There is also a [reddit](https://www.reddit.com/r/javascript/comments/7r0i00/can_a_1_a_2_a3_ever_evaluate_to_true/) discussion around this problem. Most interesting comment which I noticed is,

<strong>"If this is the type of code I'm likely to encounter in your codebase, then I'm out".</strong>

#### ES 5

#### ES 6

---

#### Problem Add(2)(3)(4)

-   will only consider 3

#### Explanation

### N number of parameters Add()()()()

#### Explanation

##### What are currying?

## Github Gist

-   [`add(2)(3)` implemenation](https://gist.github.com/anubhavsrivastava/9baa61b12abe8d8a952f762f886e477b)

## References

---