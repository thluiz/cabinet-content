---
created: 2023-12-14T20:57:27 (UTC -03:00)
tags: []
source: https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev
author: Nathan Sebhastian
---

# How to Use JavaScript Arrow Functions – Explained in Detail

> ## Excerpt
> Hello everyone! In this article, I’m going to explain one of the most useful
features in JavaScript: the arrow function.

I’ll compare the arrow function with the regular function syntax, I'll show you
how to convert a regular function into an arrow function easily, and I'll
discuss why the arrow function syntax is recommended over the regular function.

Here's what we'll cover:

 1. What Is the Arrow Function Syntax?
 2. How to Convert a Regular Function to an Arrow Function Easily
 3. Why Arro

---
  ![How to Use JavaScript Arrow Functions – Explained in Detail](https://www.freecodecamp.org/news/content/images/size/w2000/2023/11/How-to-get-file-extension-with-PHP.png)

Hello everyone! In this article, I’m going to explain one of the most useful features in JavaScript: the arrow function.

I’ll compare the arrow function with the regular function syntax, I'll show you how to convert a regular function into an arrow function easily, and I'll discuss why the arrow function syntax is recommended over the regular function.

Here's what we'll cover:

1.  [What Is the Arrow Function Syntax?](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#what-is-the-arrow-function-syntax)
2.  [How to Convert a Regular Function to an Arrow Function Easily](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#how-to-convert-a-regular-function-to-an-arrow-function-easily)
3.  [Why Arrow Functions Are Recommended Over Regular Functions](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#why-arrow-functions-are-recommended-over-regular-functions)
4.  [Arrow Functions Are Better for Short Functions](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#arrow-functions-are-better-for-short-functions)
5.  [Arrow Functions Have an Implicit Return Statement](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#arrow-functions-have-an-implicit-return-statement)
6.  [Arrow Functions Don’t Have `this` Binding](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#arrow-functions-don-t-have-this-binding)
7.  [When You Should Not Use Arrow Functions?](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#when-you-should-not-use-arrow-functions)
8.  [Conclusion](https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/?ref=dailydev#conclusion)

Let’s dive in!

## What Is the Arrow Function Syntax?

When you need to create a function in JavaScript, the main method is to use the `function` keyword followed by the function name as shown below:

```js
function greetings(name) { console.log(`Hello, ${name}!`); } greetings('John'); // Hello, John!
```

A regular JavaScript function named greetings()

The arrow function syntax allows you to create a function expression that produces the same result as the code above.

Here’s the `greetings()` function again, but using the arrow function syntax:

```js
const greetings = name => { console.log(`Hello, ${name}!`); }; greetings('John'); // Hello, John!
```

An arrow version of the greetings() function 

When you declare a function with the arrow function syntax, you need to assign the declaration to a variable so that the function has a name.

Basically, the arrow function syntax looks as follows:

```js
const myFunction = (param1, param2, ...) => { // function body }
```

The arrow function syntax

In the code above, the `myFunction` is the variable that holds the function. You can call the function as `myFunction()` later in your code.

`(param1, param2, ...)` are the function parameters. You can define as many parameters as required by the function.

Then you have the arrow `=>` to indicate the beginning of the function. After that, you can write curly brackets `{}` to indicate the function body, or remove them if you have a single-line function. More on this later.

At first, the arrow function may seem weird as you are used to seeing the `function` keyword. But as you start using the arrow syntax, you will see that it’s very convenient and easier to write.

Let me show you an easy way to convert a regular function to an arrow function next.

## How to Convert a Regular Function to an Arrow Function Easily

You can follow these **three easy steps** to convert a regular function to an arrow function:

1.  Replace the `function` keyword with the variable keyword `const`
2.  Add the `=` symbol after the function name and before the parentheses
3.  Add the `=>` symbol after the parentheses

Usually, a function is never changed after the declaration, so we use the `const` keyword instead of `let`.

The code below should help you visualize the steps:

```js
function greetings(name) { return `Hello, ${name}!`; } // step 1: replace function with const const greetings(name) { return `Hello, ${name}!`; } // step 2: add = after the function name const greetings = (name) { return `Hello, ${name}!`; } // step 3: add => after the parentheses const greetings = (name) => { return `Hello, ${name}!`; }
```

Steps to convert a regular function into an arrow function

The three steps above are enough to convert any old JavaScript function syntax to the new arrow function syntax.

When you have a single line function, there’s a fourth optional step to remove the curly brackets and the `return` keyword as follows:

```js
// from this const greetings = (name) => { return `Hello, ${name}!`; }; // to this const greetings = (name) => `Hello, ${name}!`;
```

Removing curly brackets surrounding the function body

When you have exactly one parameter, you can also remove the parentheses:

```js
// from this const greetings = (name) => `Hello, ${name}!`; // to this const greetings = name => `Hello, ${name}!`;
```

Removing parentheses surrounding the parameter

But the last two steps are optional. Only the first three steps are required to convert any JavaScript function created using the `function` keyword into the arrow function syntax.

## Why Arrow Functions Are Recommended Over Regular Functions

The arrow function syntax offers improvements to the way you write a function in JavaScript, such as:

-   You can write short functions in a more straightforward manner
-   For single-line functions, the `return` statement can be implicit
-   The `this` keyword is not bound to the function.

Let’s see how these improvements work with practical examples next.

### Arrow Functions Are Better for Short Functions

Suppose you have a single-line function that prints a string to the console. Using the `function` keyword, here’s how you would write the function:

```js
function greetings(name) { console.log(`Hello, ${name}!`); }
```

A regular function with a single-line statement

If you use the arrow function syntax, you can omit the curly brackets, creating a single-line function as shown below:

```js
const greetings = (name) => console.log(`Hello, ${name}!`);
```

A single-line arrow function

Even more, you can remove the parentheses that surround the function parameters when you have exactly one parameter:

```js
const greetings = name => console.log(`Hello, ${name}!`);
```

Remove the parentheses around parameter when you have exactly one parameter

If your function has no parameter, then you need to pass empty parentheses between the assignment and the arrow syntax as shown below:

```js
const greetings = () => console.log(`Hello, World!`);
```

A single-line arrow function without any parameter

When using the arrow function syntax, the curly brackets are required only when your function is more than a single line. For example:

```js
const greetings = () => { console.log('Hello World!'); console.log('How are you?'); };
```

Multi-line arrow function requires the curly brackets to indicate the function body

When you use the regular `function` keyword, you can’t omit the curly brackets no matter what.

Arrow functions are also great for situations where you don’t need to name the function, such as callbacks:

```js
const myArray = [1, 2, 3]; // From this: myArray.forEach(function (item) { console.log(item); }); // To this: myArray.forEach(item => console.log(item));
```

A callback function created with an arrow function

Or when you need to create an Immediately Invoked Function Expression (IIFE):

```js
// From this: (function () { console.log('Hello World'); })(); // To this: (() => console.log('Hello World'))();
```

Immediately Invoked Function Expression created with an arrow function

As you can see, using the arrow function syntax makes your code much more clean and concise.

### Arrow Functions Have an Implicit Return Statement

When you have a single-line arrow function, the return statement will be added implicitly by JavaScript. This means you shouldn't add the `return` keyword explicitly.

To show you what I mean, suppose you have a function that sums two numbers as follows:

```js
function sum(a, b) { return a + b; }
```

A regular function returning a value

When you write the function above using the arrow function syntax, you need to remove the curly brackets and the `return` keyword:

```js
const sum = (a, b) => a + b;
```

A single-line arrow function with an implicit return statement

If you didn’t remove the `return` keyword, then JavaScript will throw an error, saying an opening curly bracket `{` is expected.

When you use arrow functions, only write the `return` statement explicitly when you have multi-line statements:

```js
const sum = (a, b) => { const result = a + b; return result; };
```

When you have a multi-line arrow function, always declare the return statement if you have one

When you remove the curly brackets, don’t forget to remove the `return` keyword if you use it.

### Arrow Functions Don’t Have `this` Binding

One significant difference between the arrow function and the regular function syntax is in how they handle the `this` keyword.

In a regular function, the `this` keyword refers to the object from which you **call** the function. In an arrow function, the `this` keyword refers to the object from which you **define** the function.

To show you what I mean, suppose you have a `person` with the following properties and methods:

```js
const person = { name: 'Nathan', skills: ['HTML', 'CSS', 'JavaScript'], showSkills() { this.skills.forEach(function (skill) { console.log(`${this.name} is skilled in ${skill}`); }); }, }; person.showSkills();
```

Using the regular function syntax for the callback inside forEach()

If you run the code above, the result of calling the `showSkills()` method would be:

```txt
undefined is skilled in HTML undefined is skilled in CSS undefined is skilled in JavaScript
```

The name property is undefined

Here, the `this` keyword refers to the global `Window` object because we called the `showSkills()` method outside of the `person` object.

In the global object, the `name` property is `undefined`. Now, let’s rewrite the callback function using the arrow syntax:

```js
const person = { name: 'Nathan', skills: ['HTML', 'CSS', 'JavaScript'], showSkills() { this.skills.forEach(skill => { console.log(`${this.name} is skilled in ${skill}`); }); }, }; person.showSkills();
```

Using an arrow function for the callback inside forEach()

Run the code again, and the result would be:

```txt
Nathan is skilled in HTML Nathan is skilled in CSS Nathan is skilled in JavaScript
```

The new code output

Here, the `this` keyword refers to the object from which the arrow function is defined, which is the `person` object.

This one behavior is what makes people prefer arrow functions, because it makes more sense to have `this` refer to the object from which you define that function rather than from which you call it.

## When You Should Not Use Arrow Functions?

Arrow functions are typically preferred over standard functions, but there are a few situations when you shouldn't use the arrow function.

One of these situations is when you define an object method. Back to our `person` object example above, suppose you write the `showSkills()` method as an arrow function like this:

```js
const person = { name: 'Nathan', skills: ['HTML', 'CSS', 'JavaScript'], showSkills: () => { this.skills.forEach(skill => { console.log(`${this.name} is skilled in ${skill}`); }); }, }; person.showSkills();
```

Example of using arrow functions for an object method

Running the above code will cause an error:

```txt
TypeError: Cannot read properties of undefined (reading 'forEach')
```

The error message caused by the arrow function

When inside an object, the `this` keyword refers to the current object **only** when you declare the method using the standard syntax (`methodName()` or `methodName: function(){ }`)

When you declare an object method using the arrow function, the `this` keyword refers to the global object, and the `skills` property is `undefined` there.  Never use the arrow function when declaring a method.

## Conclusion

And that's all about arrow functions. Now you’ve learned the differences between arrow functions and regular functions, how to convert a regular function into an arrow function, as well as when the arrow functions are recommended (and not recommended!).

If you enjoyed this article and want to take your JavaScript skills to the next level, I recommend you check out my course the JavaScript Guide [here](https://codewithnathan.com/js-course):

[![nathan-js-guide-1](https://www.freecodecamp.org/news/content/images/2023/11/nathan-js-guide-1.jpg)](https://codewithnathan.com/js-course)

The course is designed to help you learn JavaScript quickly and in the right order. You can access the course as many times as you want, at your own pace. The course comes with plenty of exercises and solutions to help you practice the knowledge.

Here’s my promise: _You will actually feel like you understand what you’re doing with JavaScript._

Happy coding!

___

___

Learn to code for free. freeCodeCamp's open source curriculum has helped more than 40,000 people get jobs as developers. [Get started](https://www.freecodecamp.org/learn/)
