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

### Variants of add(2)(3) problem

Few variations in this currying problem may also be seen floating around

#### `add(2)(3)(4)...`, for endless number of parameters

Hmmm, we know how to handle the summation and returning function (along with closure) but we arent sure when to stop, which implies, when would primary function return the result and when would it reture another curried function. There are possibily two options,

##### Making use of `valueOf` property

We have already seen how `ToPrimitive` operation is handled by JS engine in this [blog](2018/11/07/understanding-primitive-and-getter-setters/). Taking into consideration of this fact, if we return an object(or function) whose `valueOf` property returns the resultant calculated so far, we would be able to differentiate between returning a function for further summation and result of summation so far.
Let's see,

    function add(x){
        let sum = x;
        function resultFn(y){
            sum += y;
            return resultFn;
        }
        resultFn.valueOf = function(){
                return sum;
            };
        return resultFn;
    }

The following execution would work,

    > 5 + add(2)(3) //output: 10
    > console.log(add(2)(3)(4)==9) //output: true
    > add(3)(4)(5).valueOf() //output: 12

On the other hand, this won't work or would unexpectedly at few places, for instance

    > add(3)(4)(5) //return function
    > console.log(add(3)(4)(5)) // output: function
    > console.log(add(3)(4)(5)===12)// output: false

This behavior is due to the fact that `valueOf` property would be called by JS engine when it needs to convert the result of add(2)(3)(4) to primitive type. All the above statements that gave correct result are due to the fact that JS engine tried to convert the result into primitive value.

##### Explicit call to a property

Another approach could be, we follow a convention, that consumer of the function should explicitly call a property in result to get the summation. This solution is very much similar to solution using `valueOf`, but no implicit conversion takes place.
Something like this,

    function add(x){
        let sum = x;
        return function resultFn(y){
            sum += y;
            resultFn.result = sum;
            return resultFn;
        }
    }

Consumption would look something like this,

    > add(3)(4)(5).result //output: 12
    > var t = add(3)(4);
    > t.result //output: 7
    > t(5).result //output: 12

-   Explicit call to function with no arguments for final result

One could also design the function to return resultant summation when the function is called with no arguments. If argument is passed, it will keep adding those numbers to previous result.

#### add(2)(3)

-   simple solution
-   next level

-   will only consider 3

## Github Gist

-   [`add(2)(3)` implementation in JS.](https://gist.github.com/anubhavsrivastava/9baa61b12abe8d8a952f762f886e477b)
-   [`add(2)(3)(4)...` via valueOf.](https://gist.github.com/anubhavsrivastava/d178cb41a11795a078a327e3d9e3635c)
-   [`add(2)(3)(4)...` via explicit result property.](https://gist.github.com/anubhavsrivastava/6772d1a69d2581d9db2b8b742adb7beb)
-   [`add(2)(3)...` via explicit argumentless call](https://gist.github.com/anubhavsrivastava/b6301e95b7b405b6fb548a194a7c20f4)

## References

-   [Article - Currying the callback, or the essence of futures](https://bjouhier.wordpress.com/2011/04/04/currying-the-callback-or-the-essence-of-futures/)

---
