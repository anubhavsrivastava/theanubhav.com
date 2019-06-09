---
layout: post
title: Make you JS - Array `sort` game strong!
subtitle: Digging deeper into Array.prototype.sort function.
avatar: /img/avatars/avatar-js.png
gist: >-
    We shall look into default array sort function in details and that one common pitfall!
categories:
    - JavaScript
    - JS
tag:
    - JavaScript
    - Code
draft: false
---

#### First Thing First

Predict the output of following JS snippet,

```javascript
let arrayToSort = [1, 6, 2, 10, 9, 17, 22, 45];
arrayToSort.sort();
console.log(arrayToSort);
```

<details>
<summary>Check output</summary>

Output -> [1, 10, 17, 2, 22, 45, 6, 9]

<details>
<summary>Got it right?</summary>

If you predicted the output as `[1, 2, 6, 9, 10, 17, 22, 45]` you need to go through the article to understand why the JS world didn't behave the way you expected it to be.
If you got it right, you have command over the most common pitfall of `Array.prototype.sort` function!

</details>

</details>

### Table of contents

<!-- toc -->

<!-- tocstop -->

### What is Array.prototype.sort function?

`Array.prototype.sort()` is the go to method for sorting arrays. The sorting order is by default lexicographic. So if array is ["z", "x", "y", "a", "b", "d" ], its sorting output will be ["a", "b", "d", "x", "y", "z"], because lexicographically, "a" occurs "b" and so on.

Simply, for example,

```javascript
> var arr = ['banana', 'apple', 'pear', 'orange'];
> arr.sort()
[ 'apple', 'banana', 'orange', 'pear' ]
> arr
[ 'apple', 'banana', 'orange', 'pear' ]
```

### Syntax

> arr.sort([compareFunction])

-   **Parameters**

    -   compareFunction [Optional]

    Specifies a function that defines the sort order.

    -   _firstEl_

        The first element for comparison.

    -   _secondEl_

        The second element for comparison.

-   **Return value**

    The sorted array. Note that the array is sorted in place, and no copy is made.

-   Complexity

    The time and space complexity of the sort cannot be guaranteed as it depends on the implementation.

### Breaking down the syntax

-   compareFunction is optional. If not passed, JS default implementation for comparison is taken into consideration.

-   The array is sorted in place. This implies that original array would change to sorted on.

### Understanding the `compareFunction`

If `compareFunction` is not supplied, all non-`undefined` array elements are sorted by converting them to strings. In a numeric sort, 9 comes before 80, but because numbers are converted to strings, "80" comes before "9" in the order. All undefined elements are sorted to the end of the array. This is the reason for the output of the very first code snippet in this article.

If `compareFunction` is supplied, all non-`undefined` array elements are sorted according to the return value of the compare function (all undefined elements are sorted to the end of the array, with no call to compareFunction)

#### **return** value of `compareFunction`

The `compareFunction` if of signature (as mentioned above) - `compareFunction(firstEl, SecondEl)` that is called with adjacent values from array to find the relative position of same values in a particular sort order. The behavior is as follows:

-   If compareFunction(a, b) is less than 0, sort a to an index lower than b (i.e. a comes first).

-   If compareFunction(a, b) is greater than 0, sort b to an index lower than a (i.e. b comes first).

-   If compareFunction(a, b) returns 0, leave a and b unchanged with respect to each other.

> Codewise as follows,

```javascript

(a, b) => {
  if(a is less than b) return -1;
  if(a is greater than b) return 1;
  if(a is equal to b) return 0;
}
```

---

Example sorting on object
Example sorting on string
Example sorting on numbers

Desc sorting vs reverse

Randomise array using sort function
roblem and system scales well.

---

When you decide to take on a technical debt, you had better make sure that your code stays squeaky clean. Keeping the system clean is the only way you will pay down that debt.

---

Read the first part - [Don't delay your technical debts](/2019/03/10/dont-delay-technical-debts/)
