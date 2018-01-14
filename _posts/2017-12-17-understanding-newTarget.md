---
layout: post
title: Understanding meta-property - newTarget
subtitle: Digging deep into newTarget - `new.target` from ECMAScript 2015 (aka ES6)
avatar: /img/avatars/avatar-js.png
gist: ECMAScript 2015 has introduced a new meta-property or expression known as `new.target` (or `newTarget`);  'new' keyword followed by dot(.) and then target. Here is a primer on what it is and how to make use of it.
categories: [JavaScript]
tag: [ES2015,ES6,ECMASCRIPT2015,JavaScript]
---


`new.target` is one of newly introduced meta-properties that have made it to ECMAScript/JavaScript. It is also known as `newTarget` in official [ES2015 specifications](http://www.ecma-international.org/ecma-262/6.0/#sec-meta-properties). It lets you detect whether a function (constructor function) was called with `new` keyword or not.


-----------------
Following section shall shed some more light on the newly available meta-property.

### Table of contents

- [What it new.target](#what-it-newtarget)
- [`new.target` as per specification](#newtarget-as-per-specification)
- [Primary points](#primary-points)
  - [Contructor functions/methods](#contructor-functionsmethods)
  - [Problem with `this`](#problem-with-this)
  - [Typical Solution - Using strict mode](#typical-solution---using-strict-mode-use-strict)
  - [Use Cases](#suse-cases)
- [Introducing `newTarget`](#introducing-newtarget)
- [Is `newTarget` just a syntactical sugar?](#is-newtarget-just-a-syntactical-sugar)
- [Lexical new.target in arrow functions `()=>{}`](#lexical-newtarget-in-arrow-functions-)
- [Polyfill](#polyfill)
- [Browser Support](#browser-support)
- [References](#references)

-----------------
## What it new.target?
 The `new.target` property lets you find if, a function or constructor was called using the `new` operator and also helps to find the called constructor. In constructors and functions instantiated with the new operator, `new.target` returns a reference to the constructor or function. In normal function calls, i.e without `new` operator, new.target is `undefined`.


## `new.target` as per specification
`newTarget` is meta-property and is not a function. Its value is the function, if its called with `new` keyword else it is `undefined`. In official specification, it is referred as 'Runtime Sementics' and returns [GetNewTarget()](http://www.ecma-international.org/ecma-262/6.0/#sec-meta-properties).


-----------------
## Primary points
Before going into usage of `newTarget`, let us consider following few points,
### Contructor functions/methods
You can invoke a constructor function via the `new` operator, then only it becomes a constructor, a factory for objects. By convention, the names of constructors start with uppercase letters.
A example implementation of Constructor function. We shall use this in further discussion.

{% highlight javascript linenos %}
//constructor function
function Animal(type) {
	this.type = type;  
}
Animal.prototype.getType = function(){
 return this.type;
}

//Usage
var cat = new Animal('Cat');
{% endhighlight %}


Here we have a 'Animal' constructor, which takes 'type' of animal as its argument and creates an instance of Animal with that type. Here we have 'cat' instance whose type is 'Cat' (cat.type).


-----------------
### Problem with `this`
If  you forget to call constructor, `Animal`, without the keyword `new`, you will end up referring `this` as parent object (typically, `window` object) and eventually polluting the global scope/environment.

Consider this snippet,
{% highlight javascript linenos %}
//constructor function
function Animal(type) {
	this.type = type;  
}
Animal.prototype.getType = function(){
  return this.type;
}

//Usage
var cat = Animal('Cat'); //no error thrown, no warning!

// cat is not any instance.
console.log(cat); //undefined
// global variable is created, window.type -> 'Cat'
console.log(type); // Cat
{% endhighlight %}
This is also known as **sloppy mode**. Here, the constructor function always has `thisArg`. We won't be digging into how `new` operator works but typically, when `new` operator is used, `this` points to a new Object; else it points to global object (window).


-----------------
### Typical Solution - Using strict mode, 'use strict'

In strict mode, an exception is thrown when constructor function is not called with `new`.
Consider,
{% highlight javascript linenos %}
//constructor function
function Animal(type) {
	'use strict'
	this.type = type;  
}
Animal.prototype.getType = function(){
 return this.type;
}

//Usage
var cat = Animal('Cat'); //TypeError: Cannot set property 'type' of undefined!

{% endhighlight %}

In strict mode, if you do not call constructor function with `new` keyword, `thisArg` is undefined. Which means, one cannot set property 'type' for a undefined type. Hence, 'TypeError' is thrown in case of strict mode.


-----------------
### Use Cases
Apart from over coming problem of global scope change, ie, this referring to window object; constructor function needs to find whether it was called with `new` operator for following reasons,
1. Force usage of function as a constructor only, which implies raising exception when function is not used as constructor.
2. Provide dual functionality for constructor factory. For eg, wrapper objects for primitive, like Number() act as constructor when used with 'new' and acts as a function that converts values to primitive type for numbers.

```javaScript
> typeof new Number(123)
'object'
> new Number(123) === 123
false
> Number(123)
123
> Number('abc')
NaN
```

**Case 1 solution:**
{% highlight javascript linenos %}
//Strict constructor function
function Animal(type) {
	'use strict'
	if(this === undefined){
	 throw new Error('Animal() must be called with new.');
	}
	this.type = type;  
}
{% endhighlight %}

**Case 2 solution:**
{% highlight javascript linenos %}
//Wrapper Objects
function WrapperConstructor(type) {
	'use strict'
	if(this === undefined){
	 // WrapperConstructor is called without new
	 // Act as factory for sub-objects or conversion function
	 ...
	}
	// Else this is defined and used with new operator
	// Act as Constructor or factory of WrapperConstructor
	...  
}
{% endhighlight %}


-----------------
## Introducing `newTarget`
`new.target` is an implicit parameter that all functions have. It is to constructor calls what `this` is to method calls. As mentioned, if used with `new` operator, it returns the constructor function else it is undefined.

Here as typical solutions to use cases that we discussed earlier

**Case 1 solution: (using `newTarget`)**
{% highlight javascript linenos %}
function Animal(type) {
	if(!new.target){
	 throw new Error('Animal() must be called with new.');
	}
	this.type = type;  
}
{% endhighlight %}
**Case 2 solution:**
{% highlight javascript linenos %}
//Wrapper Objects
function WrapperConstructor(type) {

	if(!new.target){
	 // WrapperConstructor is called without new
	 // Act as factory for sub-objects or conversion function
	 ...
	}
	// Else this is defined and used with new operator
	// Act as Constructor or factory of WrapperConstructor
	...  
}
{% endhighlight %}


-----------------
## Is `newTarget` just a syntactical sugar?
Simply, no because
- Its sole purpose is to retrieve the current value of the [[NewTarget]] value of the current (non-arrow) function environment. It is a value that is set when a function is called (very much like the `this` binding).
- If this Environment Record was created by the [[Construct]] internal method, [[NewTarget]] is the value of the [[Construct]] newTarget parameter. Otherwise, its value is undefined.
- So, for one thing, finally enables us to detect whether a function was called as a constructor or not.

But that's not its actual purpose.It is part of the way how ES6 classes are not only syntactic sugar, and how they allow us inheriting from builtin objects.
When you call a class constructor via new X, the this value is not yet initialised - the object is not yet created when the constructor body is entered. It does get created by the super constructor during the super() call (which is necessary when internal slots are supposed to be created). Still, the instance should inherit from the .prototype of the originally called constructor, and that's where newTarget comes into the play. It does hold the "outermost" constructor that received the new call during super() invocations. You can follow it all the way down in the spec, but basically it always is the newTarget not the currently executed constructor that does get passed into the new.target.

{% highlight javascript linenos %}
function Parent(){
	console.log('Value of newTarget in Parent', new.target); //undefined
  console.log('Is Target Parent?', new.target===Parent); //false
}

function Child(){
	console.log('Value of newTarget in Child', new.target); //function Child
	Parent.apply(this,arguments);
  console.log('Is Target Child?', new.target===Child);//true
}

Child.prototype = Object.create(Parent);
Child.prototype.constructor = Child;

var child = new Child();
{% endhighlight %}

- Consider the case for ES6 Classes
{% highlight javascript linenos %}

class Parent {
    constructor() {
        // implicit (from the `super` call)
        //    new.target = Child;
        // implicit (because `Parent` doesn't extend anything):
        //    this = Object.create(new.target.prototype);
        console.log(new.target) // Child!
    }
}
class Child extends Parent {
    constructor() {
        // implicit (from the `new` call):
        //    new.target = Child
        super();
        console.log(this);
    }
}
let child = new Child();
{% endhighlight %}

Following extract for classes would explain `newTarget` in classes

{% highlight javascript linenos %}
class A {
  constructor() {
    console.log(new.target.name);
  }
}

class B extends A { constructor() { super(); } }

var a = new A(); // logs "A"
var b = new B(); // logs "B"

class C { constructor() { console.log(new.target); } }
class D extends C { constructor() { super(); } }

var c = new C(); // logs class C{constructor(){console.log(new.target);}}
var d = new D(); // logs class D extends C{constructor(){super();}}
{% endhighlight %}
Thus from the above example of class C and D, it seems that new.target points to the class Definition of class which is initialized. For example, when D was initialized using new, the class definition of D was printed and similarly in case of c, class C was printed.


-----------------
## Lexical new.target in arrow functions `()=>{}`
`new.target` are lexical in arrow functions, just like `arguments` and `this` are lexical, ie, value of `new.target` is taken from surrounding environment.


-----------------
## Polyfill
This is not polyfill-able property, as it is token/expression and uses property from callee. Also, Polyfill-ing doesn't makes any sense here because it not a function to be called. Even if treated as property, `new` is a keyword/operator and not a object that could hold `target` as its property.


-----------------
## Browser Support

| Chrome | Edge | Firefox  | Internet Explorer |	Opera | Safari |
| :------ |:--- | :--- |
| 46| Yes	| 41	| NO | Yes | yes|


-----------------
## References

- [ES2015 specifications](http://www.ecma-international.org/ecma-262/6.0)
- [StackOverflow.com article](https://stackoverflow.com/questions/32450516/what-is-new-target)
- [ExploringJS - 2ality article](http://2ality.com/2015/02/es6-classes-final.html)
- [MDN web-docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target)

For the interested species, refer [meeting notes from ES discussion](https://esdiscuss.org/notes/2015-01-27) on `newTarget`

***
