---
layout: post
title: Make you JS - Array `sort` game strong!
subtitle: Digging deeper into Array.prototype.sort function.
avatar: /img/avatars/avatar-js.png
gist: >-
    We shall look into default array sort function in details and that one common
    pitfall!
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
If you got it right, you have command over the most common pitfalls of `Array.prototype.sort` function!

</details>

</details>

### Table of contents

<!-- toc -->

-   [What is Array.prototype.sort function?](#what-is-arrayprototypesort-function)
-   [Syntax](#syntax)
-   [Breaking down the syntax](#breaking-down-the-syntax)
-   [Understanding the `compareFunction`](#understanding-the-comparefunction)
    -   [**return** value of `compareFunction`](#return-value-of-comparefunction)
-   [Implementations of `compareFunction`](#implementations-of-comparefunction)
    -   [Sorting on strings](#sorting-on-strings)
        -   [Case insensitive string sort](#case-insensitive-string-sort)
        -   [Locale sensitive sorting](#locale-sensitive-sorting)
    -   [Sorting on number](#sorting-on-number)
        -   [Random order for an array](#random-order-for-an-array)
    -   [Sorting on arbitrary Object](#sorting-on-arbitrary-object)
    -   [Descending - Ascending sort order](#descending---ascending-sort-order)

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

-   The array is sorted in place. This implies that the original array would change to sorted on.
    Thus, sort() is "destructive" in nature.

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

### Implementations of `compareFunction`

#### Sorting on strings

Even if the default function works pretty well on string, you may need your custom compareFunction. The default comparison on string does not care about lowercase/uppercase. Uppercase string are lexicographically greater the lowercase.

for eg,

```javascript
var stringArray = ['Blue', 'blue', 'Humpback', 'Beluga'];
stringArray.sort();
// ["Beluga", "Blue", "Humpback", "blue"]
```

---

##### Case insensitive string sort

```javascript
var stringArray = ['Blue', 'black', 'White', 'grey', 'Green', 'brown'];
stringArray.sort(); //default sort logic
// ["Blue", "Green", "White", "black", "brown", "grey"]

// sorting with case insensitive
stringArray.sort(function(a, b) {
	var nameA = a.toUpperCase();
	var nameB = b.toUpperCase();
	if (nameA < nameB) {
		return -1;
	}
	if (nameA > nameB) {
		return 1;
	}
	return 0;
});
// ["black", "Blue", "brown", "Green", "grey", "White"]
```

---

##### Locale sensitive sorting

For sorting strings with non-ASCII characters, i.e. strings with accented characters (e, é, è, a, ä, etc.), strings from languages other than English, use String.localeCompare. This function can compare those characters so they appear in the right order.

```javascript
var stringArray = ['réservé', 'premier', 'cliché', 'communiqué', 'café', 'adieu'];
stringArray.sort(function(a, b) {
	return a.localeCompare(b);
});

// items is ['adieu', 'café', 'cliché', 'communiqué', 'premier', 'réservé']
```

`String.prototype.localeCompare` compares string keeping lexicographical order in non-ASCII strings.

#### Sorting on number

As already discussed, default sort compare function converts elements under comparison to string, it is always a wrong choice for numbers. Even if your array is bound to contain only integers [0-9], it is always advisable to use number compare function. You can skip `toString` conversion time by writing number comparator

```javascript
var numberArray = [4, 2, 5, 1, 3];
numberArray.sort(function(a, b) {
	return a - b;
});
// [1, 2, 3, 4, 5]
```

This can be simple made smaller using ES6 arrow functions, `numbers.sort((a,b)=>a-b)`.

---

##### Random order for an array

One can randomize array ordering using `Array.prototype.sort` function.

Although, random ordering of an array can be done for other types, random ordered array for number seems to be more useful. Same logic can be applied for array that are not numbers.

```javascript
var numberArray = [4, 2, 5, 1, 3];
numberArray.sort(function(a, b) {
	return Math.round(Math.random()) ? 1 : -1;
});
```

Above function will randomize the array. This can be tweaked a little to return '0' which will keep the element at original place. The above algorithm has higher odds of randomization for small array length.

---

#### Sorting on arbitrary Object

On similar lines, `compareFunction` can be used to compare two objects.

```javascript
var objectArray = [{ name: 'Iron', rate: 21 }, { name: 'Copper', rate: 37 }, { name: 'Platinum', rate: 45 }, { name: 'Zinc', rate: -12 }, { name: 'Sulphur', rate: 13 }, { name: 'Silver', rate: 37 }];

// sort by rate
objectArray.sort(function(a, b) {
	return a.rate - b.rate;
});

// sort by name
objectArray.sort(function(a, b) {
	return a.name.localeCompare(b.name);
});
```

The compare function would depend on the schema of the object and sort criterion.

#### Descending - Ascending sort order

All the above example consider sorting in ascending order ,i.e, 1>2>3 or 'a'>'b'>'c'.

A common non-performance way of sorting is using the sort compare and `Array.prototype.reverse` function in conjunction. Although it gives the desired result, it iterates over array twice.

The correct way would be implementing the `compareFunction` in accordance to required sorting behavior. For example, descending sort for numeric array can be done as follows,

```javascript
var numberArray = [4, 2, 5, 1, 3];
numberArray.sort(function(a, b) {
	return b - a;
});
// [1, 2, 3, 4, 5]
```

Simply reversing the conditions will sort the array in descending order.

---

The above sorting behavior is for `Array.prototype.sort` function and does not guarantee performance/complexity gains. It would be better to opt for traditional performance sorting algorithms which can work on magnitudes of O(n log n) for particular dataset(array) under consideration.

---
