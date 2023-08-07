---
title: "What is "this" keyword in JavaScript"
datePublished: Sun Apr 03 2022 11:25:59 GMT+0000 (Coordinated Universal Time)
cuid: cl1j76s9m064gjvnvgwbg66d2
slug: what-is-this-keyword-in-javascript
canonical: https://basavarajrp.hashnode.dev/what-is-this-in-javascript
tags: functions, programming-blogs, js, coding, objects

---

## 🤔What is "this" in JavaScript?
> "this" keyword in JavaScript stores the current execution context of the JavaScript function, and the state of the "this" keyword varies based on the declaration and invocation of the function.

### 💡Context in JavaScript?
> Context in JavaScript is related to objects, and "this" keyword provides an interface to the properties and methods that are members of that object.



The value of the "this" keyword depends on the context in which the code is getting executed. In the browser, it gives a window object that has properties attached to it, but the same "this" keyword gives an empty object "{}" in the Node engine, it depends on the type of JavaScript engine getting used.
#### "this" keyword In-Browser engine
![console.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648980747662/8pWvgdu6j.png)
#### "this" keyword in Node engine.
![console.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648981713292/AZy590A8b.png)

Functions in JavaScript can be invoked in multiple ways, based on the function invocation behavior of "this" keyword also differs.

#### Normal function call
```
function greet() {
    console.log(this)
}
greet()
``` 

![code.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648983170347/crUH5mU5h.png)

In the above example "this" keyword refers to the global context that is the window object in the browser.

#### Method invocation

```
let person = {
    name: "David",
    fullName: function () {
        console.log(this.name) // similar to person.name that is David
    }
}

person.fullName() // returns David
``` 
In the above lines of code, we are invoking the method fullName() using the person object, so "this" keyword will have all the properties and methods associated or defined inside the object person. Now "this" keyword is referring to the functional context, not the global context that we saw in the normal function call.

Even though if we console log "this" keyword it gives all the properties and methods associated with that particular object.

```
let person = {
    name: "David",
    fullName: function () {
        console.log(this)
    }
}
person.fullName() 
// returns { name: 'David', fullName: [Function: fullName] }
``` 
> 💡If we want to use the properties and methods declared inside the same object we will use the "this" keyword which is unique to a particular object.
 
#### Let's walk through the last example to understand the "this" keyword in a better way.

```
let name = "David";

let person = {
    name: "John",
    greet: function () {
        console.log(`Hello ${this.name}`); // prints Hello John
        console.log(`Hello ${name}`); // prints Hello David
    }
}

person.greet();
``` 
In the above lines of code, the "this" keyword is used to access the properties of the current object, if we don't use the "this" keyword it will always refer to the global context variables(David in our example).

### 🔑  Key Takeaways

1. "this" keyword in JavaScript stores the current execution context of the JavaScript function.
1. "this" keyword provides an interface to the properties and methods that are members of that object.
1. To access the property of the object within the method of the same object we need to use the "this" keyword.













 
