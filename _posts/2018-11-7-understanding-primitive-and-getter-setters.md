---
layout: post
title: >-
    Will <code>(a===1 && a===2 && a===3)</code> (strict comparison) ever be true
    (in JavaScript)
subtitle: >-
    Extension to traditional JavaScript problem <strong>(a==1 && a==2 &&
    a==3)</strong> (loose equality) and its solution
avatar: /img/avatars/avatar-js.png
gist: >-
    Let's understand how can we make  <strong>(a===1 && a===2 && a===3)</strong>
    to ever be true considering `===` operator, with help of getter-setter
    descriptors.
categories:
    - JavaScript
    - JS
    - Questions
tag:
    - JavaScript
    - Interview-Questions
draft: false
---

## Let's understand how can we make `(a==1 && a==2 && a==3)` to ever be true with help of getter-setter descriptors.

We shall take a brief dive into the traditional problem and move on with extension of it.

### Table of contents

<!-- toc -->

-   [Revisiting `(a==1 && a==2 && a==3)` (loose equality) problem](#revisiting-a1--a2--a3-loose-equality-problem)
    -   [Problem `(a==1 && a==2 && a==3)`](#problem-a1--a2--a3)
    -   [Explaination](#explaination)
-   [Problem `(a===1 && a===2 && a===3)` (strict comparison)](#problem-a1--a2--a3-strict-comparison)
    -   [Explaination](#explaination-1)
        -   [What are property descriptors?](#what-are-property-descriptors)

*   [References](#references)

<!-- tocstop -->

### Revisiting `(a==1 && a==2 && a==3)` (loose equality) problem

If you are already familiar with this question and understand how can one solve this JavaScript tricky riddle (Yes, riddle, I don't see a use case in production code, ¯\\\_(ツ)\_/¯ ), you can move on to [next section](#problem-a1--a2--a3-strict-comparison) which is an attempt to solve the extension of it (with strict equality).
There is also a [reddit](https://www.reddit.com/r/javascript/comments/7r0i00/can_a_1_a_2_a3_ever_evaluate_to_true/) discussion around this problem. Most interesting comment which I noticed is,

<strong>"If this is the type of code I'm likely to encounter in your codebase, then I'm out".</strong>

---

#### Problem `(a==1 && a==2 && a==3)`

    Can (a==1 && a==2 && a==3) ever evaluate to true?

Yes, and to make it true one can do this,

    const a = { value : 0 };
    a.valueOf = function() {
        return this.value += 1;
    };

    console.log(a==1 && a==2 && a==3); //true

The purpose of a question like this in interview, isn't to know the answer to the brain-teaser, so much as to get a feel for how the interviewee thinks through problems, and whether they have awareness of the kinds of features and oddities of JS that can make the '==' comparitor behave strangely.

#### Explaination

The secret here is, loose equality operator (`==`).

In JS, Loose equality compares two values for equality, after converting both values to a common type. After conversions (one or both sides may undergo conversions), the final equality comparison is performed exactly as === performs it. Loose equality is symmetric: A == B always has identical semantics to B == A for any values of A and B (except for the order of applied conversions).
Refer [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) for all in-depth explanation about loose and strict comparison.

Question here is how does JavaScript coerce this values?

Based on values of comparison, type coercion occurs, lets consider a internal fucntion to convert so,

    ToPrimitive(input, PreferredType?)

The optional parameter PreferredType indicates the final type of the conversion: it is either Number or String, depending on whether the result of ToPrimitive() will be converted to a number or a string.

Conversion happens in following order,

-   If input is primitive type, return it
-   If input is an object. Call input.valueOf(). If the result is primitive, return it.
-   Else, call input.toString(). If the result is primitive, return it.
-   throw a TypeError (indicating the failure to convert input to a primitive).

If PreferredType is Number, the above algorithm works in specified order.
If PreferredType is String, steps 2 and 3 are swapped.
The PreferredType can also be omitted; it is then considered to be String for dates and Number for all other values.
The default implementation of valueOf() returns `this`, while the default implementation of `toString()` returns type information.

This is how the operators + and == call ToPrimitive(). (Ahaa! )

So in above code, as soon as JS saw, a==1, '1' being primitive type it tried to convert 'a' to Number, and with above algorithm, `a.valueOf` was called returning '1', but incrementing its value for next time.
Similar coercion came into effect for a==2 and a==3 thus incrementing it for next time.

---

### Problem `(a===1 && a===2 && a===3)` (strict comparison)

    Can (a===1 && a===2 && a===3) ever evaluate to true?

Yes, below code would make this true,

    var value = 0; //window.value
    Object.defineProperty(window, 'a', {
        get: function() {
            return this.value += 1;
        }
    });

    console.log(a===1 && a===2 && a===3) /true

#### Explaination

Our understanding from previous problem is, primitive values would never satisfy above condition, we need by some means call a function and inside that function we can perform this magic.
But since there is no loose equality, .valueOf won't be called by JS Engine, bringing function `Property`, especially `getter`, to the rescue.

##### What are property descriptors?

A _property descriptor_ can be of two types: data descriptor, or accessor descriptor.

1.  Data descriptor

    Mandatory properties:

    -   `value`

    Optional properties:

    -   `configurable`
    -   `enumerable`
    -   `writable`

    Sample:

    {
    value: 5,
    writable: true
    }

2.  Accessor descriptor

    Mandatory properties:

    -   Either `get` or `set` or both

    Optional properties:

    -   `configurable`
    -   `enumerable`

    Sample:

    {
    get: function () {
    return 5;
    },
    enumerable: true
    }

Accessor Example from Mozilla pages,

        // Example of an object property added
        // with defineProperty with an accessor property descriptor

        var bValue = 38;

        Object.defineProperty(o, 'b', {
            // Using shorthand method names (ES2015 feature).
            // This is equivalent to:
            // get: function() { return bValue; },
            // set: function(newValue) { bValue = newValue; },
            get() { return bValue; },
            set(newValue) { bValue = newValue; },
            enumerable: true,
            configurable: true
        });
        o.b; // 38
        // 'b' property exists in the o object and its value is 38
        // The value of o.b is now always identical to bValue,
        // unless o.b is redefined

A property on a object be defined by using `Object.defineProperty` as mentioned in the solution. You can dig into syntax and definition [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) .
Interestingly, `get` and `set` are accessors which can be called via dot(.) operator, i.e if object `a` has getter property called `b` then it is called just like any other value iwth dot notation, viz, `a.b`.
Here is the solution to our problem, we are able to call a function without using `()` after function name. This is the solution to the problem.

In above mentioned solution, we are defining `a` getter property on window object, so `a` is directly accessible in code (global variables) and hence are able to achieve the result.
If we define a property called `a` on some other object than `window`, say `object1`, we need to change the condition to `object1.a===1 && object1.a===2 && object1.a===3`.

## References

-   [Reddit Post](https://www.reddit.com/r/javascript/comments/7r0i00/can_a_1_a_2_a3_ever_evaluate_to_true/)
-   [Speaking JS](http://speakingjs.com/es5/ch17.html)
-   [MDN web-docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

---
