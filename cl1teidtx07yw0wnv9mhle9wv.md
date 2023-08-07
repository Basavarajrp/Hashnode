---
title: "Functional Programing Paradigm in JavaScript"
datePublished: Sun Apr 10 2022 14:47:51 GMT+0000 (Coordinated Universal Time)
cuid: cl1teidtx07yw0wnv9mhle9wv
slug: functional-programing-paradigm-in-javascript
canonical: https://basavarajrp.hashnode.dev/functional-programing-paradigm-in-javascript
tags: programming-blogs, javascript, developer, coding, functional-programming

---


# Introduction
### What is Functional Programming?
>  Functional Programming is the way of writing code that is bug resistant by avoiding the flow control statements(for, while, break, etc), and making use of pure functions.

There are a few sets of rules that we need to know to better understand what actually functional programming is.

Let's jump in and understand what actually functional programming is about.

### Imperative v/s Declarative Programming
In Imperative programming, we only focus on how to achieve the solution to a problem statement. Here the code gets executed line by line which includes looping statements such as for, while and do-while loops and the variables keep on changing their states because in Imperative programming we make use of mutable variables which have a lot of side effects.

In Declarative programming, unlike Imperative programming, we do not use mutable variables which avoids the side effects related to changes in the global state of variables, objects, etc.


```
let name = "David";

function modifiedName () {
     name = name.toUpperCase()
     return name
}

console.log(modifiedName())  //  returns DAVID
console.log(name)  //  returns DAVID

``` 

In the above example, the global variable(name) state is changed inside the function, now this is a few lines of code that we saw so it was easy for us to keep track of the variable state change, but this will lead to errors and bugs when we have thousands of lines of code, and it will be difficult for us to debug the code.


### State Change and Mutability
![Mutation (3).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649577887590/eYEAnJFKK.png)

As we saw in the above example after declaring the variable "name" we just keep on updating the variables in the rest of our code which will lead to more error-prone code and it's hard to understand where is the exact issue since the same variable is getting updated frequently.

So in functional programming we avoid updating the same variable, again and again, instead we define a new one.


```
let name = "David";

function modifiedName(name) {
   return name.toUpperCase()
}

let newName = modifiedName("David") // assigning new variable.

console.log(name); // returns David
console.log(newName); // returns DAVID

```

> 💡The problem with immutability is we keep on creating new copies of the variables like in the above example. If it was an array of size 1000 elements, then creating the new copy would affect our run-time speed and space efficiency even for replacing one element in the array. But to overcome this we can use some immutable data structure libraries in JavaScript.

Although JavaScript lacks immutable built-in data structures, an immutable state is still possible using libraries such as [Immutable.js](https://immutable-js.com/), [Mori](http://swannodette.github.io/mori/), etc.

### Use Pure Functions
Functions that take their own input parameters and perform some sort of computation on it and return the value are called Pure functions. Pure functions will not refer to the global variables and this will avoid the uncontrolled state change of the variables and objects through the program.

```
// Impure function
var num = 24;
/*  
  This function is not pure because it is referring to global variables
  and also has no return value in it.
*/
function addFoure () {
    num = num + 4;
}

// Pure function
var value = 40;
/*  
  This function is pure because it has its own input parameter and also 
  returns the computed value back 
*/
function addEight(num) {
   return num + 8;
}

let value = addEight(16)
``` 

Debugging pure functions is easy and we can keep track of all variables and their state, also it is less error-prone because they are not referring to any global variables that may be causing issues in the code.

### Functions are treated as first-class citizens
We can assign functions to variables, we can pass them as arguments to other functions, and we can return functions just like variables.


```
// Assign function to variables
const square = function (num) {
     return num * num
}

// Passing function as input parameter to other functions.
const numbers = [65, 44, 12, 4];
const newArr = numbers.map(myFunction) // myFunction is passed as input parameter to map function.

function myFunction(num) {
  return num * 10;
}

// Returning the function
function outer() {
  var counter = 0; // Backpack or Closure
  function incrementCounter() {
    return counter++;
  }
  return incrementCounter; // returning the inner function
}

const count = outer();
count(); // 0
count(); // 1
count(); // 2
``` 
### Avoid using looping statements
Instead of iterating the arrays using looping statements such as for and while loops use higher-order functions like map, reduce, and filter.

What is Map, Reduce and Filter functions?
> Higher-order functions are the functions that take another function as an input parameter and also return the inner function, map, reduce and filter are higher-order functions.

### Map
This function takes two input parameters one is an array of elements and another parameter is a function that needs to be executed on each element of the array.

Let's take an array of elements to prepare food and run a "cook" function to get delicious items from each element

```
map(['🌽', '🐔', '🥚'], cook)  // returns [ ‘🍿’, ‘🍗’, 🍳]

``` 
### Reduce
This function takes two input parameters one is an array of elements and another parameter is a function that needs to be executed on each element but returns a single element as the outcome of the array.

Let's prepare a vegetable salad from the array of vegetables.


```
reduce([‘🍍’, ’🍎’, ‘🍊’, ‘🥑’, ‘🥕’], salad)  // returns 🥗

``` 
The above reduce function just returned a single value by running the salad function on all the array elements.

### Filter 
This function takes two input parameters one is an array of elements and another parameter is a function that needs to be executed on each element of the array and removes the unmatched elements from the array that is defined inside the function.


```
filter([‘🍍’, ’🍎’, ‘🍊’, ‘ 🍗’, ‘🥑’, ‘🥕’], isVegetarian)  
// returns [‘🍍’, ’🍎’, ‘🍊’, ‘🥑’, ‘🥕’] 

``` 
The above filter function returned all the vegetarian elements and filtered out the Non-Vegetarian element.

### 🔑 Key Takeaways:

1. Avoid State Change and Mutability of variables and objects.

1. Use Pure Functions.

1. Functions should be treated as first-class citizens just like variables.

1. Avoid using looping statements instead use higher-order functions like Map, Filter and Reduce.




















