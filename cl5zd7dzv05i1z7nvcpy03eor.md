---
title: "Default Parameters in JavaScript"
datePublished: Sun Jul 24 2022 13:37:33 GMT+0000 (Coordinated Universal Time)
cuid: cl5zd7dzv05i1z7nvcpy03eor
slug: default-parameters-in-javascript
canonical: https://hashnode.com/edit/cl5zd7dzv05i1z7nvcpy03eor
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1658670150464/6cgjbAnWK.png
tags: programming-blogs, javascript, es6, javascript-framework, functional-programming

---

# Introduction
Understanding the default parameters in JS functions is not much difficult, but when we are looking in more depth there is a lot of confusion which makes you think that JavaScript is so terrible. In this blog, I will try my best to make you comfortable with these concepts. 

Let's jump in, and see what actually the default parameters are in JavaScript.

# How the default parameters were handled before the introduction of ES6. 

Everyone knows about the functions and their use cases, during the ES5 we used functions with input parameters. 

😟 When calling the functions what if we have not passed the input parameters, and how this was handled.


```
function add(num1, num2) {
    let num_1 = num1 || 1;
    let num_2 = num2 || 1;
    return num_1 + num_2
}
console.log(add()) // 2
console.log(add(2, 4)) // 6
``` 

If we look into the above example we can see the default parameters were handled with some other programming technique, but the popular shorthand for optional parameters in ES5 was the logical or(||) operator.

If the first operand of a || expression is true, the second operand is not even evaluated. This phenomenon is called a logical shortcut.

So in our above example when we have not passed any default parameters to the function `num_1` and `num_2` was getting assigned with value `1`, but when we passed the parameters to the functions `num_1` and `num_2` was getting assigned with actual passed values.

### 🤔 This is working well, then what's the problem.
If we have a close look at the above example, we can see all the `falsy`( 0, ' ', false) values are treaded in the same way. Let me break this in a more easy way with an example.
```
function add(num1, num2) {
    let num_1 = num1 || 1;
    let num_2 = num2 || 1;
    return num_1 + num_2
}
console.log(add(0, 4)) // instead of 4 we are getting 5 as the output.
```
If we see the above example when we passed two parameters one with `0` and the other with `4`, we should be getting the result as `0+4=4` instead of that, we got the result as `5`. This is because the number `0` is considered as the falsy value in most of the programming languages so the `OR` logical shortcut was failing for the 1st operand which resulted in assigning the value `1` to `num_1` and the result was `1+4=5`.

To handle this in a more appropriate way we make use of the ES6 default parameters in JavaScript.

# ES6 default parameters.

In the ES5 it was necessary to check the default parameters in the function body and then assign the appropriate values to the parameters, as we have already seen in the above examples.

### ❓What's new in ES6 to solve this problem?
We can assign the default values to the parameters while defining the function.

```
function add(num1=1, num2=1) {
    return num1 + num2
}
console.log(add()) // 2
console.log(add(0, 4)) // 4
``` 
As we can see even after passing the `0`(falsy) value as the functional argument we are getting the proper result which is `0+4=4` in our case, which was not the case in the above examples. We don't need to do any checks in the function body as we did earlier.

# `Undefined` v/s `falsy` values as default parameters.

```
function print(num=2) {
    console.log(num)
}
print(undefined) // 2 (still takes the default value.)
print(null) // returns null
``` 
From the above example, we can say that even if we set the default parameter `num` to `undefined` still the default value `2` is considered. 

> Apart from `undefined` if we pass any other falsy values like `null` and empty string default value is not considered. 

# Evaluation of default parameters.
>  All the default parameters are evaluated at the call time, so every time we call the function new object is created, unlike in python.

```
function defaultParams (arr = []) {
    return arr
}
const res1 = defaultParams([1,2])
const res2 = defaultParams([1,2])
console.log(res1 === res2) // false
``` 
Every time we call the function we get the new object, also if check the equality of two objects it gives the false as output even though we have the same input parameters.

# Accessing the early parameters.
> Default parameters that are defined inside the function are evaluated from left to right. The default parameters that are defined on the left side can be accessed by the parameters defined on the right side.

```
function add(num1, num2, result = num1+num2){
    return result
}
console.log(add(4, 4)) // 8 
``` 
In the above example, we can see that the parameter `result` is making use of `num1` and `num2` for its calculation, so it is possible to make use of early defined parameters.

The above method is also known as `Expression as Default Values`.

# Effect of `scope` on default parameters.

The scope is a very important concept in JavaScript, understanding how scope works will make our development easy. If you want to understand more about scopes in JavaScript refer to my blog on scopes. 
 [Scope in JavaScript](https://basavarajrp.hashnode.dev/scope-in-javascript)

When we define the function and call it, the scope is created for that particular function so that variables defined inside that function cannot be accessed outside the function.

### 🧐 How the scope is created when we define the functions with default parameters.

When we have the function with default parameters then the second scope is created which will act as the parent scope to the function, so if we try to use the value from the function body inside the default parameter we will get `undefined`.

![scope.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658667251193/3snuIvhH_.png align="center")

```
function test(result = b){
    var b = 40;
    return result;
}
console.log(test()); // ReferenceError: b is not defined
``` 

In the above example, we are trying to use variable `b` as the value to the default parameter `result` that is defined inside the function body, as we discussed that default parameter creates the parent scope for the function body, so it is throwing the reference error.

A child can access the parent scope variables, but the parent cannot use the child scope variables, so we get the ReferenceError when we try to access the variables defined under the function body.

This is a bit tricky to understand if we don't know what actually the scope is all about.

# Destructuring array and object as default parameters.

### 💫 Destructuring of the array.

If the function is having one of its parameters as an array/object, then we can also have the default parameters defined for them.

By assigning the empty array/object we can make sure that even if the empty array/object is passed to the function the default values are considered.

Let's understand this by following code examples.

```
function add([num1=4, num2=5] = []) {
    return num1 + num2;
}
console.log(add()) // 9
console.log(add([2,2])) // 4
``` 

When we don't pass the object as the input parameter to the function default values are considered, so for the 1st time we got `9` as the output.

# Some cool tricks to evaluate the default parameter.

We can also do validation on the default parameter in the following way.

```
const validate = () => { throw new Error('Please enter the number.'); };

function print(num = validate()) {
    console.log(num);
}
print(5) // returns 5
print() // Error: Please enter the number.
``` 

We can create the validations for the default parameters like the above example and perform some other operations with the validated result.

I just wanted to show that we can use functions as the default values.


# 🔑Key Takeaways:

1. We can pass the default parameters to the function, so when the values are not passed during the function call the default parameters are considered.
2. When we pass `undefined` the default parameter is considered, but this is not the same for other falsy values like `" "`, `null`, etc. 
3. All the default parameters are evaluated at the call time, so every time we call the function new object is created.
4. Default parameters that are defined inside the function are evaluated from left to right.
5. We can't use the variables that are defined inside the function body as the default parameter for the same function.
6. We can also define the default parameters for the function that accepts an object as its input parameter.
7. We can also pass the external function as the value to the default parameters.

# Conclusion
This is all about the default parameters in JavaScript that I wanted to share with you all, if you feel any missing points in this article please share them with me in the comments section.
 
Thank you,
Happy Coding.
 






