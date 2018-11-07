---
layout: post
title: Will <code>(a===1 && a===2 && a===3)</code> (strict comparison) ever be true (in JavaScript)
subtitle: Extension to traditional JavaScript problem <strong>(a==1 && a==2 && a==3)</strong> (loose equality) and its solution
avatar: /img/avatars/avatar-js.png
gist: Let's understand how can we make  <strong>(a===1 && a===2 && a===3)</strong> to ever be true considering `===` operator, with help of getter-setter descriptors.
categories: [JavaScript, JS, Questions]
tag: [JavaScript, Interview-Questions]
draft: true
---

## Let's understand how can we make `(a==1 && a==2 && a==3)` to ever be true with help of getter-setter descriptors.

We shall take a brief dive into the traditional problem and move on with extension of it.

### Table of contents

---

## Revisiting `(a==1 && a==2 && a==3)` (loose equality) problem

If you are already familiar with this question and understand how can one solve this JavaScript tricky riddle (Yes, riddle, I don't see a use case in production code, ¯\\\_(ツ)\_/¯ ), you can move on to next section which is an attempt to solve the extension of it (with strict equality).
`todo : add link`
There is also a [reddit](https://www.reddit.com/r/javascript/comments/7r0i00/can_a_1_a_2_a3_ever_evaluate_to_true/) discussion around this problem. Most interesting comment which I noticed is,

<strong>"If this is the type of code I'm likely to encounter in your codebase, then I'm out".</strong>

---

### Problem `(a==1 && a==2 && a==3)`

    Can (a==1 && a==2 && a==3) ever evaluate to true?

Yes, and to make it true one can do this,

    const a = { value : 0 };
    a.valueOf = function() {
        return this.value += 1;
    };

    console.log(a==1 && a==2 && a==3); //true

### Explaination

The secret here is, loose equality operator (`==`)

## References

---
