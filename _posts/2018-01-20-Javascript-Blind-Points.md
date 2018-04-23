---
layout: post
comments: true
title:  "Javascript Blind Points"
excerpt: "Some kernel features of JS."
date:   2018-01-20
mathjax: true
---


Array is a group of elements embraced by "[]", while obect is a group of properties embraced by "{}".



The array is accessed by number index, but the object is accessed by *property*.



All the *properties* in an object is **string**. Thus there're two ways to access object's property:

- One is using *dot notation*, which will be treated as string automatically.
- Another is using *square bracket*, which accepts the string **explicitly**.

```Js
const car = {
  color: 'red',
  year: 1992,
  isPreOwned: true
};

// dot access
car.color;

// square bracket
car['color']
```

Thus, when we use a variable to store the property name, it'll give different impact:

```Js
var myVariable = 'color'

// Error:
// car.myVariable

// Convert 'myVariable' to 'color' automatically
car[myVariable]
```





## var vs let

>  Reference: http://www.jstips.co/en/javascript/keyword-var-vs-let/ .

- The scope of a variable defined with `var` is function scope or declared outside any function, global.
- The scope of a variable defined with `let` is block scope.

```Js
function varvslet() {
  console.log(i); // i is undefined due to hoisting
  // console.log(j); // ReferenceError: j is not defined

  for( var i = 0; i < 3; i++ ) {
    console.log(i); // 0, 1, 2
  };

  console.log(i); // 3
  // console.log(j); // ReferenceError: j is not defined

  for( let j = 0; j < 3; j++ ) {
    console.log(j);
  };

  console.log(i); // 3
  // console.log(j); // ReferenceError: j is not defined
}
```





If pass *object* parameter to a function, it'll be passed by *reference*. Even the re-assigning an object to a new variable, it'll be passed by *reference*.

```Js
const iceCreamOriginal = {
  Andrew: 3,
  Richard: 15
};

const iceCreamCopy = iceCreamOriginal;

iceCreamCopy.Richard;
// 15

iceCreamCopy.Richard = 99;

iceCreamCopy.Richard;
// 99

iceCreamOriginal.Richard;
// 99
```





We can access it the same way that we do with other properties: by using *dot notation* or *square bracket* notation.

```Js
const developer = {
  name: 'Andrew',
  sayHello: function () {
    console.log('Hi there!');
  }
};

developer.sayHello();
// 'Hi there!'

developer['sayHello']();
// 'Hi there!'
```





## Functions v.s. Methods

In JS, these two things are different. A *method* is a property that points to a function. Thus, a *method* can't exist(or be called) without receiving a object.





Write an expression that invokes the `alerter()` function in the following array, `myArray`:

```Js
const myArray = [ function alerter() { alert('Hello!'); } ];
```

Thus, the `alerter()` function is just the name of the first array element *method* points to.









## `this` and Invocation

Let's compare the code from the `chameleon` object with the `whoThis()` code.

```Js
const chameleon = {
  eyes: 2,
  lookAround: function () {
     console.log(`I see you with my ${this.eyes} eyes!`);
  }
};

chameleon.lookAround();

function whoThis () {
  this.trickyish = true
}

whoThis();
```



---



**How the function is invoked determines the value of this inside the function.**

Because `.lookAround()` is invoked as a method, the value of `this` inside of `.lookAround()` is whatever is *left of the dot* at invocation. Since the invocation looks like:

```Js
chameleon.lookAround();
```

The `chameleon` object is left of the dot. Therefore, inside the `.lookAround()` method, `this` will refer to the `chameleon` object!



Now let's compare that with the `whoThis()` function. Since it is called as a regular function (i.e., *not* called as an method on an object), its invocation looks like:

```Js
whoThis();
```

Well, there is no dot. And there is no object *left of the dot*. So what is the value of `this` inside the `whoThis()` function? This is an interesting part of the JavaScript language.

**When a regular function is invoked, the value of this is the global window object.**

If you haven't worked with the `window` object yet, this object is provided by the browser environment and is globally accessible to your JavaScript code using the identifier, `window`. This object is not part of the JavaScript specification (i.e., ECMAScript); instead, it is developed by the [W3C](https://www.w3.org/Consortium/).

This `window` object has access to a ton of information about the page itself, including:

- The page's URL (`window.location;`)
- The vertical scroll position of the page (`window.scrollY'`)
- Scrolling to a new location (`window.scroll(0, window.scrollY + 200);` to scroll 200 pixels down from the current location)
- Opening a new web page (`window.open("https://www.udacity.com/");`)


---



You've seen what `this` refers to in `chameleon.lookAround();` and in `whoThis()`. Carefully review this code:

```Js
const car = {
  numberOfDoors: 4,
  drive: function () {
     console.log(`Get in one of the ${this.numberOfDoors} doors, and let's go!`);
  }
};

const letsRoll = car.drive;

letsRoll();

```

What does you think `this` refers to in the code above?

> Hint: that sentence again, **How the function is invoked determines the value of this inside the function.**

As it's called as a regular function not the form of `this.<some-identifier>`, `this` here refers to `window`.









## Global Variables are Properties on `window`

Since the `window` object is at the highest (i.e., global) level, an interesting thing happens with global variable declarations. Every variable declaration that is made at the global level (outside of a function) automatically becomes a property on the `window` object!

```Js
var currentlyEating = 'ice cream';

window.currentlyEating === currentlyEating
// true
```



#### Globals and `var`, `let`, and `const`

The keywords `var`, `let`, and `const` are used to declare variables in JavaScript. `var` has been around since the beginning of the language, while `let` and `const` are significantly newer additions (added in ES6).

Only declaring variables with the `var` keyword will add them to the `window` object. If you declare a variable *outside of a function* with either `let` or `const`, it will *not* be added as a property to the `window` object.

```Js
/*
** Also:
** const currentlyEating = 'ice cream';
*/
let currentlyEating = 'ice cream';

window.currentlyEating === currentlyEating
// false!
```

Similarly to how global variables are accessible as properties on the `window` object, any global function declarations are accessible on the `window` object as methods.



Variables declared by **let** have as their scope the block in which they are defined, as well as in any contained sub-blocks. In this way, **let** works very much like **var**. The main difference is that the scope of a **var** variable is the entire enclosing function:

```Js
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```



**let** sometimes makes the code cleaner when inner functions are used.

```js
var list = document.getElementById('list');

for (let i = 1; i <= 5; i++) {
  let item = document.createElement('li');
  item.appendChild(document.createTextNode('Item ' + i));

  item.onclick = function(ev) {
    console.log('Item ' + i + ' is clicked.');
  };
  list.appendChild(item);
}

// to achieve the same effect with 'var'
// you have to create a different context
// using a closure to preserve the value
for (var i = 1; i <= 5; i++) {
  var item = document.createElement('li');
  item.appendChild(document.createTextNode('Item ' + i));

  (function(i){
    item.onclick = function(ev) {
      console.log('Item ' + i + ' is clicked.');
    };
  })(i);
  list.appendChild(item);
}
```

The example above works as intended because the five instances of the (anonymous) inner function refer to five different instances of the variable `i`. Note that it does not work as intended if you replace **let** with **var**, since all of the inner functions would then return the same final value of `i`: 6. Also, we can keep the scope around the loop cleaner by moving the code that creates the new elements into the scope of each loop.

At the top level of programs and functions, **let**, unlike **var**, does not create a property on the global object. For example:

```js
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```





## Functions are First-Class Functions

In JavaScript, functions are *first-class* functions. This means that you can do with a *function* just about anything that you can do with other elements, such as numbers, strings, objects, arrays, etc. JavaScript functions can:

1. Be stored in variables
2. Be returned from a function.
3. Be passed as arguments into another function.

Note that while we can, say, treat a function as an object, a key difference between a function and an object is that functions can be called (i.e., invoked with `()`), while regular objects *cannot*.



A function that returns another function is known as **higher-order function**. Consider this example:

```Js
function alertThenReturn() {
  alert('Message 1!');

  return function () {
    alert('Message 2!');
  };
}
```

Since `alertThenReturn()` *returns* that inner function, we can assign a variable to that return value:

```Js
const innerFunction = alertThenReturn();
```

We can then use the `innerFunction` variable like any other function!

```Js
innerFunction();

// alerts 'Message 2!'
```

Likewise, this function can be invoked immediately without being stored in a variable. We'll still get the same outcome if we simply add another set of parentheses to the expression `alertThenReturn();`:

```Js
alertThenReturn()();

// alerts 'Message 1!' then alerts 'Message 2!'
```





## Callback Function

- A function that takes other functions as arguments (and/or *returns* a function, as we learned in the previous section) is known as a **higher-order function**.
- A function that is passed as an argument into another function is called a **callback** function.

Callback functions are great because they can delegate calling functions to other functions. They allow you to build your applications with *composition*, leading to cleaner and more efficient code.



Where have you probably seen callback functions used? In array methods! Functions are commonly passed into array methods and called on elements *within* an array (i.e., the array on which the method was called).

Let's check out a couple in detail:

- `forEach()`
- `map()`
- `filter()`

Remember that the key difference between `forEach()` and `map()` is that `forEach()` doesn't return anything, while `map()` returns a new array with the values that are returned from the function:

```
const nameLengths = names.map(function(name) {
  return name.length;
});

```

So `nameLengths` will be a *new* array: `[5, 7, 8]`. Again, it is important to understand that **the map() method returns a new array; it does not modify the original array**.



Array's `filter()` method is similar to the `map()` method:

- It is called on an array
- It takes a function as an argument
- It returns a new array

The difference is that the function passed to `filter()` is used as a test, and only items in the array that pass the test are included in the new array.



## Scope

If you took [Intro to Javascript](https://www.udacity.com/course/intro-to-javascript--ud803), you learned about *block* scope vs. *function* scope. These determine where a variable can be seen in some code. Computer scientists call this **lexical scope**.

However, there also exists *another* kind of scope called **runtime scope**. When a function is run, it creates a new runtime scope. This scope represents the *context* of the function, or more specifically, **the set of variables available** for the function to use.



A function's runtime scope describes the variables available for use inside a given function. The code inside a function has access to:

1. The function's arguments.
2. Local variables declared within the function.
3. Variables from its parent function's scope.
4. Global variables.

```Js
const a = 'a'    // global variables

function parent(){
  const b = 'b'    // variable from the parent function's scope

  function child(arg_in) {    // function parameter variable
    const c = 'c'    // local variable declared within the function
  }
}
```







## JavaScript is Function-Scoped

You may be wondering why scope is so heavily **associated** with *functions* in JavaScript. Especially if you've had past experience in another programming language, this might seem a bit unusual (e.g., *blocks* in Ruby have their own scope)!

This is all because variables in JavaScript are traditionally defined in the scope of a *function*, rather than in the scope of a *block*.

- Since entering a function will change scope, any variables defined inside that function are *not* available outside of that function.
- On the other hand, if there are any variables defined inside a *block* (e.g., within an `if` statement), those variables *are* **available outside of that block**.

```Js
var globalNumber = 5;

if(globalNumber < 4) {
  var cc = 3
}

// variable 'cc' is avialable,
// becuase js is not block scope
console.log(cc)
// 3
```



### JS Block-Scope

ES6 syntax allows for **additional scope** while declaring variables with the `let` and `const` keywords. These keywords are used to declare *block-scoped* variables in JavaScript, and largely replace the need for `var`.



![JS-scope-chain](/Users/xietao/Documents/TerryProjects/WordPress/素材/JS-scope-chain.png)







## Variable Shadowing

What happens when you create a variable with the *same name* as another variable somewhere in the scope chain?

JavaScript won't throw an error or otherwise prevent you from creating that extra variable. In fact, the variable with local scope will just temporarily "shadow" the variable in the outer scope. This is called **variable shadowing**. Consider the following example:

```js
const symbol = '¥';

function displayPrice(price) {
  const symbol = '$';
  console.log(symbol + price);
}

displayPrice('80');
// '$80'
```

In the above snippet, note that `symbol` is declared in two places:

1. Outside the `displayPrice()` function, as a *global* variable.
2. Inside the `displayPrice()` function, as a *local* variable.

After invoking `displayPrice()` and passing it an argument of `'80'`, the function outputs `'$80'` to the console.

How does the JavaScript interpreter know which value of `symbol` to use? Well, since the variable pointing to `'$'` is declared inside a function (i.e., the "inner" scope), it will override any variables of the same name that belong in an outer scope -- such as the global variable pointing to `'¥''`. As a result, `'$80'` is displayed rather than `'¥80'`.











## Functions RetainTheir Scope

Identifier lookup and the scope chain are really powerful tools for a function to access identifiers in the code. In fact, this lets you do something really interesting: *create a function now, package it up with some variables, and save it to run later*. If you have five buttons on the screen, you could write five different click handler functions, or you could use the same code five times with different saved values.

Let's check out an example of a function retaining access to its scope. Consider the `remember()` function below:

```Js
function remember(number) {
    return function() {
        return number;
    }
}

const returnedFunction = remember(5);

console.log( returnedFunction() );
// 5
```

When the Javascript engine enters `remember()`, it creates a new execution scope that points back to the prior execution scope. This new scope includes a reference to the `number` parameter (an immutable `Number` with the value `5`). When the engine reaches the inner function (a *function expression*), it attaches a link to the current execution scope.

This process of a function retaining access to its scope is called a **closure**. In this example, the inner function "closes over"(i.e. *capture*) `number`. A closure can capture any number of parameters and variables that it needs. MDN defines a closure as:

> "the combination of a function and the lexical environment within which that function was declared."

This definition might not make a lot of sense if you don't know what the words "lexical environment" mean. The [ES5 spec](http://es5.github.io/#x10.2) refers to a lexical environment as:

> "the association of Identifiers to specific variables and functions based upon the lexical nesting structure of ECMAScript code."

In this case, the "lexical environment" refers the code as it was written in the JavaScript file. As such, a closure is:

- The function itself, and
- The code (but more importantly, the *scope chain of*) where the function is declared

When a function is declared, it locks onto the scope chain. You might think this is pretty straightforward since we just looked at that in the previous section. What's really interesting about a function, though, is that it will *retain* this scope chain -- even if it is invoked in a location *other* than where it was declared. This is all due to the closure!



## Creating a Closure

Every time a function is defined, closure is created for that function. Strictly speaking, then, *every* function has closure! This is because functions close over at least one other context along the scope chain: the global scope. However, the capabilities of closures really shine when working with a nested function (i.e., a function defined within another function).

Recall that a nested function has access to variables outside of it. From what we have learned about the scope chain, this includes the variables from the outer, enclosing function itself (i.e., the parent function)! These nested functions close over (i.e., *capture*) variables that aren't passed in as arguments nor defined locally, otherwise known as **free variables**.

As we saw with the `remember()` function earlier, it is important to note that *a function maintains a reference to its parent's scope. If the reference to the function is still accessible, the scope persists* !





## Garbage Collection

JavaScript manages memory with automatic **garbage collection**. This means that when data is no longer referable (i.e., there are no remaining references to that data available for executable code), it is "garbage collected" and will be destroyed at some later point in time. This frees up the resources (i.e., computer memory) that the data had once consumed, making those resources available for re-use.

Let's look at garbage collection in the context of *closures*. We know that the variables of a parent function are accessible to the nested, inner function. If the nested function captures and uses its parent's variables (or variables along the scope chain, such as its parent's parent's variables), those variables will stay in memory as long as the functions that utilize them can still be referenced.

As such, referenceable variables in JavaScript are *not* garbage collected! Let's quickly look back at the `myCounter` function from the previous video:

```js
function myCounter() {
  let count = 0;

  return function () {
    count += 1;
    return count;
  };
}
```

The existence of the nested function keeps the `count` variable from being available for garbage collection, therefore `count` remains available for future access. After all, *a given function (and its scope) does not end when the function is returned*. Remember that functions in JavaScript retain access to the scope that they were created in!









## Function Declarations vs. Function Expressions

Before we jump into **immediately-invoked function expressions** (IIFE), let's make sure we're on the same page regarding the differences between function *declarations* and function *expressions*.

A function *declaration* defines a function and does not require a variable to be assigned to it. It simply declares a function, and doesn't itself return a value. Here's an example:

```js
function returnHello() {
  return 'Hello!';
}
```

On the other hand, a function *expression* does return a value. Function expressions can be anonymous or named, and are part of another expression's syntax. They're commonly assigned to variables, as well. Here's the same function as a function *expression*:

```Js
// anonymous
const myFunction = function () {
  return 'Hello!';
};

// named
const myFunction = function returnHello() {
  return 'Hello!';
};
```









## ⚠️Omitting the `new` Operator ⚠️

What happens if you inadvertently invoke a constructor function *without* using the `new` operator?

```js
function SoftwareDeveloper(name) {
   this.favoriteLanguage = 'JavaScript';
   this.name = name;
}

let coder = SoftwareDeveloper('David');

console.log(coder);
// undefined
```

What's going on? Without using the `new` operator, no object was created. The function was invoked just like any other regular function. Since the function doesn't *return* anything (except `undefined`, which all functions return by default), the `coder` variable ended up being assigned to `undefined`.

One more thing to note: since this function was invoked as a regular function, the value of `this` is also drastically different. Don't worry too much about this for now; we'll take a deep dive into the `this` keyword in the very next section!



```
const dog = {
  bark: function () {
    console.log('Woof!');
  },
  barkTwice: function () {
    bark();
    bark();
  }
};

dog.barkTwice();
```









## What Does `this` Get Set To?

At this point, we've seen `this` in many different contexts, such as within a method, or referenced by a constructor function. Let's now organize our thoughts and bring it all together!

There are **four ways** to call functions, and each way sets `this` differently.

- First, calling a constructor function with the `new` keyword sets `this` to a newly-created object. Recall that creating an instance of `Cat` earlier had set `this` to the new `bailey` object.
- On the other hand, calling a function that belongs to an object (i.e., a *method*) sets `this` to the object itself. Recall that earlier, the `dog` object's `barkTwice()` method was able to access properties of `dog` itself.
- Third, calling a function on its own (i.e., simply invoking a regular function) will set `this` to `window`, which is the global object if the host environment is the browser.

```Js
function funFunction() {
  return this;
}

funFunction();
// (returns the global object, `window`)
```

- The fourth way to call functions allows us to set `this` ourselves!













## Saving `this` with an Anonymous Closure

Let's recap the issue at hand. Here's the `invoiceTwice()` function from the previous video, as well as the `dog` object:

```Js
function invokeTwice(cb) {
   cb();
   cb();
}

const dog = {
  age: 5,
  growOneYear: function () {
    this.age += 1;
  }
};

```

First, invoking `growOneYear()` works as expected, updating the value of the `dog` object's `age` property from `5` to `6`:

```Js
dog.growOneYear();

dog.age;
// 6

```

However, passing `dog.growOneYear` (a function) as an argument into `invokeTwice()` produces an odd result:

```js
invokeTwice(dog.growOneYear);

dog.age;
// 6

```

You may have expected the value of the `age` property in `dog` to have increased to `8`. Why did it remain `6`?

As it turns out, `invokeTwice()` does indeed invoke `growOneYear` -- but it is invoked as a *function* rather than a *method*!





## Saving `this` with an Anonymous Closure

Recall that simply invoking a normal function will set the value of `this` to the global object (i.e., `window`). This is an issue, because we want `this` to be the `dog` object!

So how can we make sure that `this` is preserved?

One way to resolve this issue is to use an **anonymous closure** to close over the `dog` object:

```js
invokeTwice(function () {
  dog.growOneYear();
});

dog.age;
// 7

```

Using this approach, invoking `invokeTwice()` still sets the value of `this` to `window`. However, this has no effect on the closure; within the anonymous function, the `growOneYear()` method will still be directly called onto the `dog` object! As a result, the value of `dog`'s `age` property increases from `5` to `7`.

Since this is such a common pattern, JavaScript provides an alternate and less verbose approach: the `bind()` method.

## Saving `this` with bind()

Similar to `call()` and `apply()`, the `bind()` method allows us to directly define a value for `this`. `bind()` is a method that is also called *on* a function, but unlike `call()` or `apply()`, which both invoke the function right away -- `bind()`*returns* a new function that, when called, has `this` set to the value we give it.

```js
const myGrow = dog.growOneYear.bind(dog);

invokeTwice(myGrow);

dog.age;
// 7
```



`bind` is a function that we directly call on a function. And it **returns a copy** of that function, with a specified `this` value.













## Adding Methods to the Prototype

Recall that objects contain data (i.e., properties), as well as the means the manipulate that data (i.e., methods). Earlier in this Lesson, we simply added methods *directly* into the constructor function itself:

```
function Cat() {
 this.lives = 9;

 this.sayName = function () {
   console.log(`Meow! My name is ${this.name}`);
 };
}

```

This way, a `sayName()` method gets added to all `Cat` objects by saving a function to the `sayName` attribute of newly-created `Cat` objects.

This works just fine, but what if we want to instantiate more and more `Cat` objects with this constructor? You'll create a new function every single time for that `Cat` object's `sayName`! What's more: **if you ever want to make changes to the method, you'll have to update all objects *individually***. In this situation, it makes sense to have all objects created by the same `Cat` constructor function just *share* a single `sayName` method.

To save memory and keep things DRY (don't repeat yourself), we can add methods to the constructor function's `prototype` property. The prototype is just an object, and all objects created by a constructor function keep a reference to the prototype. Those objects can even use the `prototype`'s properties *as their own*!











### 💡 Replacing the `prototype` Object 💡

What happens if you completely replace a function's `prototype` object? How does this affect objects created by that function? Let's look at a simple `Hamster` constructor function and instantiate a few objects:

```js
function Hamster() {
  this.hasFur = true;
}

let waffle = new Hamster();
let pancake = new Hamster();
```

First, note that even after we make the new objects, `waffle` and `pancake`, we can still add properties to `Hamster`'s prototype and it will still be able to access those new properties.

```js
Hamster.prototype.eat = function () {
  console.log('Chomp chomp chomp!');
};

waffle.eat();
// 'Chomp chomp chomp!'

pancake.eat();
// 'Chomp chomp chomp!'
```

Now, let's replace `Hamster`'s `prototype` object with something else entirely:

```js
Hamster.prototype = {
  isHungry: false,
  color: 'brown'
};
```

The previous objects don't have access to the updated prototype's properties; they just retain their secret link to the old prototype:

```js
console.log(waffle.color);
// undefined

waffle.eat();
// 'Chomp chomp chomp!'

console.log(pancake.isHungry);
// undefined
```

As it turns out, any new `Hamster` objects created moving forward will use the updated prototype:

```Js
const muffin = new Hamster();

muffin.eat();
// TypeError: muffin.eat is not a function

console.log(muffin.isHungry);
// false

console.log(muffin.color);
// 'brown'
```











### The `constructor` Property

Each time an object is created, a special property property is assigned to it under the hood: `constructor`. Accessing an object's `constructor` property returns a reference to the constructor function that created that object in the first place! Here's a simple `Longboard` constructor function. We'll also go ahead and make a new object, then save it to a `board`variable:

```js
function Longboard() {
  this.material = 'bamboo';
}

const board = new Longboard();
```

If we access `board`'s `constructor` property, we should see the original constructor function itself:

```js
console.log(board.constructor);

// function Longboard() {
//   this.material = 'bamboo';
// }
```

Excellent! Keep in mind that if an object was created using literal notation, its constructor is the built-in `Object()`constructor function!

```js
const rodent = {
  teeth: 'incisors',
  hasTail: true
};

console.log(rodent.constructor);
// function Object() { [native code] }
```















## Object.create()

At this point, we've reached a few roadblocks when it comes to inheritance. First, even though `__proto__` can access the prototype of the object it is called on, using it in any code you write is not good practice.

What's more: **we also shouldn't inherit *only* the prototype; this doesn't set up the prototype chain, and any changes that we made to a child object will also be reflected in a parent object**.

So how should we move forward?

There's actually a way for us to set up the prototype of an object ourselves: using `Object.create()`. And best of all, this approach lets us manage inheritance *without* altering the prototype!

`Object.create()` takes in a single object as an argument, and returns a new object with its `__proto__` property set to what argument is passed into it. From that point, you simply set the returned object to be the prototype of the child object's constructor function. Let's check out an example!

> Inheritance in JavaScript is all about setting up the prototype chain. This allows us to **subclass**, that is, create a "child" object that inherits most or all of a "parent" object's properties and methods. We can then implement any of the child object's unique properties and methods separately, while still retaining data and functionality from its parent.
>
> An object (instance) is secretly linked to its constructor function's prototype object through that instance's `__proto__`property. **You should never use the __proto__ in any code you write.** Using `__proto__` in any code, or even inheriting just the prototype directly, leads to some unwanted side effects.
>
> To efficiently manage inheritance in JavaScript, an effective approach is to avoid mutating the prototype completely. `Object.create()` allows us to do just that, taking in a parent object and returning a *new* object with its `__proto__`property set to that parent object.
