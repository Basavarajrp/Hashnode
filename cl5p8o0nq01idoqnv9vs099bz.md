---
title: "Scope in JavaScript"
datePublished: Sun Jul 17 2022 11:32:49 GMT+0000 (Coordinated Universal Time)
cuid: cl5p8o0nq01idoqnv9vs099bz
slug: scope-in-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1658050176556/tMYNQhVye.svg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1658061838426/C7NYlKNBV.svg
tags: variables, programming-blogs, js, es6, scope

---

As per my experience knowing the difference between `var`, `let`, and `const` in depth is very much important, when it comes to functional scope and block scope it is always important to know the difference between `var`, `let`, and `const` to avoid Temporal Dead Zone(TDZ) in JS.

Let's go and look into what actually the scope is all about and how it varies with `var`, `let`, and `const` keywords.

# What is Scope?
The scope is the current context in JavaScript, which determines the variable accessibility throughout the program.

## Different types of Scope.

1. **Block scope** 
2. **Function scope**
3. **Global scope**

Before ES6, JS was supporting only the Global scope and functional scope. Later ES6 introduced `let` and `const` which were used for the block level scope. Variables declared within the block({..}) with `let` and `const` keywords cannot be accessed outside the block.

 
Let's jump into the example.

```
var name = "David" // Global Scope

function greeting() {
    console.log(`Hello ${name}`) // Hello David
}
console.log(`Hello ${name}`) // Hello David
``` 

In the above block of code, the variable defined outside the function is accessible throughout the code in all places since it is not defined at any block-level or within the function. 

```
function greeting() {
  var name = "David";
  console.log(`Hello ${name}`) // Hello David
}

console.log(`Hello ${name}`) // throws error name not declared.
```
In the above block of code, the variable is defined inside the function so it is only accessible inside the function, not outside the function, this is called function scope, and the variable `name` acts as the local variable within the functional block itself.

**By making use of the `var` keyword we were able to create the Global and Function scope in JavaScript.**

# What was the Problem with Var?
 There's a weakness that comes with `var`, let’s look into the below example.
```
var name = "Harry";
var isAlive = true;

if (isAlive) {
   var name = "Alien"
}

console.log(name); // "Alien"
```
Now we understood that the `var` keyword can be used for the function scope, but in the above code, `var` was defined inside the block scope, not the function scope therefore the variable name was re-declared with another name from Harry to Alien. So to create the scope within the block level of code ES6 has introduced `let` and `cont` keywords.
```
var name = "Harry";
var isAlive = true;

if (isAlive) {
   let name = "Alien";
   console.log(name); // Alien
}

console.log(name); // "Harry"
```
By using the `let` keyword inside the if-block we have not re-defined the variable `name` instead we have created the block level variable which is local within the same block. 

# What is the difference between `Var`, `Let`, and `Const`?
### `Var` can be updated, re-declared, hoisted, and initialized to undefined.
```
console.log(name) // undefined

var name = "David";

console.log(name) // David

name = "Jonny" // update

var name = "Kyle"; // re-declared

```
In the JS all the variables are 1st scanned and made available for the program, If we try to access the variable before the line of assignment we get undefined, but at the time of execution actual value gets assigned so we get the actual value if we try to use variable after the assignment.

### `Let` can be updated but not re-declared.
Just like `var`,  a variable declared with `let` can be updated within its scope. Unlike `var`, a let variable cannot be re-declared within its scope.

```
// Can be re-declared within its scope.
let greeting = "say Hi";
greeting = "say Hello instead";

// Re-initializing the variable throws error within the same scope.
let greeting = "say Hi";
let greeting = "say Hello instead"; // error: Identifier 'greeting' has already been declared
```
However, if the same variable is defined in different scopes, there will be no error both instances are treated as different variables since they have different scopes.

```
let greeting = "say Hi"; // outer scope
if (true) {
    let greeting = "say Hello instead"; // inner scope
    console.log(greeting); // "say Hello instead"
}
console.log(greeting); // "say Hi"
```
### Hoisting with `let`:

Just like  `var`, `let` declarations are hoisted to the top. Unlike `var` which is initialized as `undefined`, the `let` keyword is not initialized. So if you try to use a `let` variable before the declaration, you'll get a `Reference Error`.
```
console.log(name); // Reference Error

let name = "David"
```
### `Const` cannot be updated or re-declared:

This means that the value of a variable declared with `const` remains the same within its scope. It cannot be updated or re-declared. So if we declare a variable with `const`, we can neither do this:
```
const greeting = "say Hi";
greeting = "say Hello instead";// error: Assignment to constant variable.

const greeting = "say Hi";
const greeting = "say Hello instead";// error: Identifier 'greeting' has already been declared
```

Like `let` declarations, `const` declarations can only be accessed within the block they were declared.

Declarations with const are block-scoped, they have to be initialized, and their value cannot be changed after initialization.
```
const PI = 3.1415;
PI = 3.14;
```
Not initializing a constant also throws an error.
```
const PI; //SyntaxError: Missing initializer in const declaration
```
Redeclaring another variable with the same name using `const` in the same scope will throw an error.

** `Const` Declarations will differ when it comes to objects:**
While a `const` object cannot be updated, the properties of these objects can be updated.

```
const person = {name: "Harry", age: 24}

// cannot do this
person = {name: "Krish", gender: "male"}

// Can do this
person.name = "Krish"
```

### Hoisting of `const`
Just like `let`, `const` declarations are hoisted to the top but are not initialized.

## What Exactly Is a Temporal Dead Zone in JavaScript?
A temporal dead zone (TDZ) is the area of a block where a variable is inaccessible until the moment the computer completely initializes it with a value.
```
{
console.log(name); // temperory dead zone(TDZ)
let name = "David";
console.log(name);
}
```

## 🔑Key Takeaways:
1. `var` declarations are globally scoped or function scoped while `let` and `const` are blocks scoped.

2. `var` variables can be updated and re-declared within its scope, `let` variables can be updated but not re-declared, and `const` variables can neither be updated nor re-declared. 

3. They are all hoisted to the top of their scope. But while `var` variables are initialized with `undefined`, `let` and `const` variables are not initialized.

4. While `var` and `let` can be declared without being initialized, `const` must be initialized during declaration.

5. Always declare and initialize all your variables at the beginning of your scope to avoid the temporal dead zone.

# Conclusion: 
If you reached this end then hopefully you got something about the scope, `var`, `let`, and `cont` keywords. Also, we have discussed Temporal Dead Zone in JavaScript and so on.
If you have any comments on this blog please share them with me or in the comments below.

** Thank you, keep writing some good code. **

# Resource:
1. Github: https://github.com/Basavarajrp/JS-ES6/tree/main/scope







