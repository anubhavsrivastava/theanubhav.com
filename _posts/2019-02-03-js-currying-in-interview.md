---
layout: post
title: >-
    Currying in JS: Answering the traditional question, Add(2)(3), which gives sum
    of both numbers.
subtitle: >-
    Understanding concept of currying and in depth analysis of most frequent
    interview question around it
avatar: /img/avatars/avatar-js.png
gist: >-
    We will have a look at concepts that revolve around this question and
    progressively take it to next level.
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

-   [First, Implement `add(2)(3)` in JavaScript](#first-implement-add23-in-javascript)
-   [What is currying?](#what-is-currying)
-   [Variants of add(2)(3) problem](#variants-of-add23-problem)
    -   [`add(2)(3)(4)...`, for endless number of parameters](#add234-for-endless-number-of-parameters)
        -   [Making use of `valueOf` property](#making-use-of-valueof-property)
        -   [Explicit call to a property](#explicit-call-to-a-property)
    -   [add(2)(3)](#add23)

*   [Github Gist](#github-gist)
*   [References](#references)

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

##### 1. Making use of `valueOf` property

We have already seen how `ToPrimitive` operation is handled by JS engine in this [blog](/2018/11/07/understanding-primitive-and-getter-setters/). Taking into consideration of this fact, if we return an object(or function) whose `valueOf` property returns the resultant calculated so far, we would be able to differentiate between returning a function for further summation and result of summation so far.
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

##### 2. Explicit call to a property

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

Consumption would be,

    > add(3)(4)(5).result //output: 12
    > var t = add(3)(4);
    > t.result //output: 7
    > t(5).result //output: 12

If something of this sort has to be implemented, it should be via module/class and just not simple function to emulate the behavior.

##### 3. Explicit call to function with no arguments for final result

One could also design the function to return resultant summation when the function is called with no arguments. If argument is passed, it will keep adding those numbers to previous result.

    function add(x){
        let sum = x;
        return function resultFn(y){
            if(arguments.length){ //not relying on falsy value
                sum += y;
                return resultFn;
            }
            return sum;
        }
    }

This could be used in following manner,

    > add(2)(3)() //output: 5
    > var t = add(3)(4)(5)
    > t() //ouput: 12

#### `add(2)(3)(4) and add(2,3,4)` usage in same function.

This is another variance, where in same function should satisfy both use case `add(2)(3)(4)` and `add(2,3,4)` or any combination. So, a single function should satisfy following cases,

-   add(2)(3)(4)
-   add(2,3,4)
-   add(2)(3,4)
-   add(2,3)(4)

For this type, let us consider there would be fixed 'n' number of arguments (in our case, n=3). If we need to implement this with variable number of arguments we could club solution of these problem with solution of above discussed problem.
The trick here is to keep track of 'n' arguments and as soon as we have sufficient number of arguments, we return the sum.

##### 1. Solution using `arguments` count

Following code keeps a count of total arguments passed and if it reaches 3, it gives the resultant sum

    function add(){
        let args = [].slice.apply(arguments);
        function resultFn(){
            args = args.concat([].slice.apply(arguments));
            if(args.length>=3){
                return args.slice(0,3).reduce(function(acc,next){ return acc+next},0); //will only sum first 3 arguments
            }
            return resultFn;
        }
        return resultFn();
    }

Sample usage,

    > add(2)(3)(4) //output: 9
    > add(2,3,4) //output: 9
    > add(2)(3,4) //output: 9
    > add(2,3)(4) //output: 9

##### 2. Generic solution for fixed argument function

The approach here is to create a higher order function, which would take a function and number of arguments that are must for this function - say 3 in our case for add(2,3,4). This function would keep track of arguments unless the total collected arguments is same as expected no of arguments for the passed function.

    function fixCurry(fn, totalArgs){
        totalArgs = totalArgs ||fn.length
            return function recursor(){
                return arguments.length<fn.length?recursor.bind(this, ...arguments): fn.call(this, ...arguments);
            }
    }

The above function takes in a function - `fn`, and optionally `totalArgs` that are mandatory before calling `fn`. If `totalArgs` aren't passed it will rely on function signature and use the property `fn.length` which is number of parameter a function has been defined with.
`totalArgs` may be used for function - `fn` whose implementation itself relies on `arguments` and no parameters are defined in its signature.
`fixCurry` returns a function who keeps adding (via `bind`) arguments to a function, if the threshold reaches, it just calls the function with all parameter collected so far between all calls.

Lets see sample usage,

    > var add = fixCurry((a,b,c)=>a+b+c); //fn = summation function
    > console.log(add(1,2, 3))  // output: 6
    > console.log(add(1)(2,3)) // output: 6
    > console.log(add(1)(3)(2)) // output: 6
    > console.log(add(1,2)(3)) // output: 6

Same would work for multiply (or any other curried function),

    > var multiply = fixCurry((a,b,c)=>a*b*c); //fn = multiplication function
    > console.log(multiply(1,2, 3))  // output: 6
    > console.log(multiply(1)(2,3)) // output: 6
    > console.log(multiply(1)(3)(2)) // output: 6
    > console.log(multiply(1,2)(3)) // output: 6

This `fixCurry` can also be used for currying any function with fixed parameter.

Another thing to note with `add` and `multiple` example is, addition and multiplication of first 3 natural number is same. [#MindBlown](https://media.giphy.com/media/xT0xeJpnrWC4XWblEk/giphy.gif)

## Github Gist

-   [`add(2)(3)` implementation in JS.](https://gist.github.com/anubhavsrivastava/9baa61b12abe8d8a952f762f886e477b)
-   [`add(2)(3)(4)...` via valueOf.](https://gist.github.com/anubhavsrivastava/d178cb41a11795a078a327e3d9e3635c)
-   [`add(2)(3)(4)...` via explicit result property.](https://gist.github.com/anubhavsrivastava/6772d1a69d2581d9db2b8b742adb7beb)
-   [`add(2)(3)...` via explicit argumentless call](https://gist.github.com/anubhavsrivastava/b6301e95b7b405b6fb548a194a7c20f4)
-   [`add(2)(3)(4) with add(2,3,4)` via argument count](https://gist.github.com/anubhavsrivastava/50236dde3561454708d57830398b1226)
-   [`add(2)(3)(4) with add(2,3,4)` , generic solution](https://gist.github.com/anubhavsrivastava/d02e115f321de4852942c31627e33e0d)

## References

-   [Article - Currying the callback, or the essence of futures](https://bjouhier.wordpress.com/2011/04/04/currying-the-callback-or-the-essence-of-futures/)

---
