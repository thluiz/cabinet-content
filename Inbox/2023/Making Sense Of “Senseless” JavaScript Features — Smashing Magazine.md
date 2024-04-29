---
created: 2024-01-04T20:21:01 (UTC -03:00)
tags: []
source: https://www.smashingmagazine.com/2023/12/making-sense-of-senseless-javascript-features/
author: 
---

# Making Sense Of “Senseless” JavaScript Features — Smashing Magazine

> ## Excerpt
> JavaScript may be the most popular client-side language in the world, but it’s far from perfect and not without its quirks. Juan Diego Rodriguez examines several “absurd” JavaScript eccentricities and explains how they made it into the language as well as how to avoid them in your own code.

---
-   14 min read
-   [JavaScript](https://www.smashingmagazine.com/category/javascript), [React](https://www.smashingmagazine.com/category/react)

JavaScript may be the most popular client-side language in the world, but it’s far from perfect and not without its quirks. Juan Diego Rodriguez examines several “absurd” JavaScript eccentricities and explains how they made it into the language as well as how to avoid them in your own code.

Why does JavaScript have so many eccentricities!? Like, why does `0.2 + 0.1` equals `0.30000000000000004`? Or, why does `"" == false` evaluate to `true`?

There are a lot of mind-boggling decisions in JavaScript that seem pointless; some are misunderstood, while others are direct missteps in the design. Regardless, it’s worth knowing _what_ these strange things are and _why_ they are in the language. I’ll share what I believe are some of the quirkiest things about JavaScript and make sense of them.

## `0.1 + 0.2` And The Floating Point Format [#](https://www.smashingmagazine.com/2023/12/making-sense-of-senseless-javascript-features/#0-1-0-2-and-the-floating-point-format)

Many of us have mocked JavaScript by writing `0.1 + 0.2` in the console and watching it resoundingly fail to get `0.3`, but rather a funny-looking `0.30000000000000004` value.

What many developers might not know is that the weird result is not really JavaScript’s fault! JavaScript is merely adhering to the [**IEEE Standard for Floating-Point Arithmetic**](https://ieeexplore.ieee.org/document/8766229) that nearly every other computer and programming language uses to represent numbers.

But what exactly is the Floating-Point Arithmetic?

Computers have to represent numbers in all sizes, from the distance between planets and even between atoms. On paper, it’s easy to write a massive number or a minuscule quantity without worrying about the size it will take. Computers don’t have that luxury since they have to save all kinds of numbers in binary and a small space in memory.

Take an 8-bit integer, for example. In binary, it can hold integers ranging from `0` to `255`.

[![8-bit integers showing 0 and 255.](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_80/w_400/https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/1-8-bit-integers-showing-0-255.jpg)](https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/1-8-bit-integers-showing-0-255.jpg)

8-bit integers showing 0 and 255. ([Large preview](https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/1-8-bit-integers-showing-0-255.jpg))

The keyword here is _integers_. It can’t represent any decimals between them. To fix this, we could add an imaginary decimal point somewhere along our 8-bit so the bits before the point are used to represent the integer part and the rest are used for the decimal part. Since the point is always in the same imaginary spot, it’s called a **fixed point decimal**. But it comes with a great cost since the range is reduced from **`0` to `255`** to exactly **`0` to `15.9375`**.

[![Decimals with a fixed point.](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_80/w_400/https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/2-decimals-fixed-point.jpg)](https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/2-decimals-fixed-point.jpg)

Decimals with a fixed point. ([Large preview](https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/2-decimals-fixed-point.jpg))

Having greater precision means sacrificing range, and vice versa. We also have to take into consideration that computers need to please a large number of users with different requirements. An engineer building a bridge doesn’t worry too much if the measurements are off by just a little, say a hundredth of a centimeter. But, on the other hand, that same hundredth of a centimeter can end up costing much more for someone making a microchip. The precision that’s needed is different, and the consequences of a mistake can vary.

Another consideration is the size where numbers are stored in memory since storing long numbers in something like a megabyte isn’t feasible.

The _floating-point_ format was born from this need to represent both large and small quantities with precision and efficiency. It does so in three parts:

1.  A single bit that represents whether or not the number is positive or negative (`0` for positive, `1` for negative).
2.  A [significand](https://mathworld.wolfram.com/Significand.html) or [mantissa](https://mathworld.wolfram.com/Mantissa.html) that contains the number’s digits.
3.  An **exponent** specifies where the decimal (or binary) point is placed relative to the beginning of the mantissa, similar to how scientific notation works. Consequently, the point can move around to any position, hence the _floating_ point.

[![Decimals with a floating point.](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_80/w_400/https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/3-decimals-floating-point.jpg)](https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/3-decimals-floating-point.jpg)

Decimals with a floating point. ([Large preview](https://files.smashing.media/articles/making-sense-of-senseless-javascript-features/3-decimals-floating-point.jpg))

An 8-bit floating-point format can represent numbers between `0.0078` to `480` (and its negatives), but notice that the floating-point representation can’t represent all of the numbers in that range. It’s impossible since 8 bits can represent only 256 distinct values. Inevitably, many numbers cannot be accurately represented. There are _gaps_ along the range. Computers, of course, work with more bits to increase accuracy and range, commonly with 32-bits and 64-bits, but it’s impossible to represent all numbers accurately, a small price to pay if we consider the range we gain and the memory we save.

The exact dynamics are far more complex, but for now, we only have to understand that while this format allows us to express numbers in a large range, it loses precision (the gaps between representable values get bigger) when they become too big. For example, JavaScript numbers are presented in a double-precision floating-point format, i.e., each number is represented in 64 bits in memory, leaving 53 bits to represent the mantissa. That means JavaScript can only safely represent integers between –(2<sup>53</sup> — 1) and 2<sup>53</sup> — 1 without losing precision. Beyond that, the arithmetic stops making sense. That’s why we have the `Number.MAX_SAFE_INTEGER` static data property to represent the maximum safe integer in JavaScript, which is (2<sup>53</sup> — 1) or `9007199254740991`.

But `0.3` is obviously below the `MAX_SAFE_INTEGER` threshold, so why can’t we get it when adding `0.1` and `0.2`? The floating-point format struggles with some fractional numbers. It isn’t a problem with the floating-point format, but it certainly is across any number system.

To see this, let’s represent one-third (<sup>1</sup>⁄<sub>3</sub>) in base-10.

No matter how many digits we try to write, the result will never be _exactly_ one-third. In the same way, we cannot accurately represent some fractional numbers in base-2 or binary. Take, for example, `0.2`. We can write it with no problem in base-10, but if we try to write it in binary we get a recurring `1001` at the end that repeats infinitely.

We obviously can’t have an infinitely large number, so at some point, the mantissa has to be truncated, making it impossible not to lose precision in the process. If we try to convert `0.2` from double-precision floating-point back to base-10, we will see the actual value saved in memory:

It isn’t 0.2! We cannot represent an awful lot of fractional values — not only in JavaScript but in almost all computers. So why does running `0.2 + 0.2` correctly compute `0.4`? In this case, the imprecision is so small that it gets rounded by Javascript (at the 16<sup>th</sup> decimal), but sometimes the imprecision is enough to escape the rounding mechanism, as is the case with `0.2 + 0.1`. We can see what’s happening under the hood if we try to sum the actual values of `0.1` and `0.2`.

This is the actual value saved when writing `0.1`:

If we manually sum up the actual values of `0.1` and `0.2`, we will see the culprit:

That value is rounded to `0.30000000000000004`. You can check the real values saved at [float.exposed](https://float.exposed/0x3fb999999999999a).

Floating-point has its known flaws, but its positives outweigh them, and it’s standard around the world. In that sense, it’s actually a relief when all modern systems will give us the same `0.30000000000000004` result across architectures. It might not be the result you expect, but it’s a result you can predict.

## Type Coercion [#](https://www.smashingmagazine.com/2023/12/making-sense-of-senseless-javascript-features/#type-coercion)

JavaScript is a dynamically typed language, meaning we don’t have to declare a variable’s type, and it can be changed later in the code.

> [I find dynamically typed languages liberating since we can focus more on the substance of the code.](https://twitter.com/share?text=%0aI%20find%20dynamically%20typed%20languages%20liberating%20since%20we%20can%20focus%20more%20on%20the%20substance%20of%20the%20code.%0a&url=https://smashingmagazine.com%2f2023%2f12%2fmaking-sense-of-senseless-javascript-features%2f)

The issue comes from being weakly typed since there are many occasions where the language will try to do an implicit conversion between different types, e.g., from strings to numbers or _falsy_ and _truthy_ values. This is specifically true when using the equality ( `==`) and plus sign (`+`) operators. The rules for type coercion are intricate, hard to remember, and even incorrect in certain situations. It’s better to avoid using `==` and always prefer the strict equality operator (`===`).

For example, JavaScript will coerce a string to a number when compared with another number:

The inverse applies to the plus sign operator (`+`). It will try to coerce a number into a string when possible:

That’s why we should only use the plus sign operator (`+`) if we are sure that the values are numbers. When concatenating strings, it’s better to use the [`concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/concat) method or [template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

The reason such coercions are in the language is actually absurd. When JavaScript creator Brendan Eich was asked what [he would have done differently](https://thenewstack.io/brendan-eich-on-creating-javascript-in-10-days-and-what-hed-do-differently-today#:~:text=notorious) in JavaScript’s design, his answer was to be more meticulous in the implementations early users of the language wanted:

> “I would have avoided some of the compromises that I made when I first got early adopters, and they said, “Can you change this?”
> 
> — Brendan Eich

The most glaring example is the reason why we have two equality operators, `==` and `===`. When an early JavaScript user prompted his need to compare a number to a string without having to change his code to make a conversion, Brendan added the loose equality operator to satisfy those needs.

There are a lot of other rules governing the loose equality operator (and other statements checking for a condition) that make JavaScript developers scratch their heads. They are complex, tedious, and senseless, so we should avoid the loose equality operator (`==`) at all costs and replace it with its strict homonym (`===`).

Why do we have two equality operators in the first place? A lot of factors, but we can point a finger at Guy L. Steele, co-creator of the Scheme programming language. He assured Eich that we could always add another equality operator since there were dialects with five distinct equality operators in the Lisp language! This mentality is dangerous, and nowadays, all features have to be rigorously analyzed because we can always add new features, but once they are in the language, they cannot be removed.

## Automatic Semicolon Insertion [#](https://www.smashingmagazine.com/2023/12/making-sense-of-senseless-javascript-features/#automatic-semicolon-insertion)

When writing code in JavaScript, a semicolon (`;`) is required at the end of some statements, including:

-   `var`, `let`, `const`;
-   Expression statements;
-   `do...while`;
-   `continue`, `break`, `return`, `throw`;
-   `debugger`;
-   Class field declarations (public or private);
-   `import`, `export`.

That said, we don’t necessarily have to insert a semicolon every time since JavaScript can automatically insert semicolons in a process unsurprisingly known as **Automatic Semicolon Insertion** (ASI). It was intended to make coding easier for beginners who didn’t know where a semicolon was needed, but it isn’t a reliable feature, and we should stick to explicitly typing where a semicolon goes. Linters and formatters add a semicolon where ASI would, but they aren’t completely reliable either.

ASI can make some code work, but most of the time it doesn’t. Take the following code:

You can probably see where the semicolons go, and if we formatted it correctly, it would end up as:

But if we feed the prior code directly to JavaScript, all kinds of exceptions would be thrown since it would be the same as writing this:

In conclusion, know your semicolons.

## Why So Many Bottom Values? [#](https://www.smashingmagazine.com/2023/12/making-sense-of-senseless-javascript-features/#why-so-many-bottom-values)

The term “bottom” is often used to represent a value that does not exist or is undefined. But why do we have two kinds of bottom values in JavaScript?

Everything in JavaScript can be considered an object, except the two bottom values `null` and `undefined` (despite `typeof null` returning `object`). Attempting to get a property value from them raises an exception.

Note that, strictly speaking, **all primitive values aren’t objects**. But only `null` and `undefined` aren’t subjected to [_boxing_](https://stackoverflow.com/questions/34067261/is-boxing-coercion-in-javascript).

We can even think of `NaN` as a third bottom value that represents the absence of a number. The abundance of bottom values should be regarded as a design error. There isn’t a straightforward reason that explains the existence of two bottom values, but we can see a difference in how JavaScript employs them.

`undefined` is the bottom value that JavaScript uses by default, so it’s considered good practice to use it exclusively in your code. When we define a variable without an initial value, attempting to retrieve it assigns the `undefined` value. The same thing happens when we try to access a non-existing property from an object. To match JavaScript’s behavior as closely as possible, use `undefined` to denote an existing property or variable that doesn’t have a value.

On the other hand, `null` is used to represent the absence of an object (hence, its `typeof` returns an `object` even though it isn’t). However, this is considered a design blunder because `undefined` could fulfill its purposes as effectively. It’s used by JavaScript to denote the end of a recursive data structure. More specifically, it’s used in the prototype chain to denote its end. Most of the time, you can use `undefined` over `null`, but there are some occasions where only `null` can be used, as is the case with [`Object.create`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) in which we can only create an object without a prototype passing `null`; using `undefined` returns a `TypeError`.

`null` and `undefined` both suffer from the path problem. When trying to access a property from a bottom value — as if they were objects — exceptions are raised.

There is no way around this unless we check for each property value before trying to access the next one, either using the logical AND (`&&`) or optional chaining (`?`).

I said that `NaN` can be considered a bottom value, but it has its own confusing place in JavaScript since it represents numbers that aren’t actual numbers, usually due to a failed string-to-number conversion (which is another reason to avoid it). `NaN` has its own shenanigans because it isn’t equal to itself! To test if a value is `NaN` or not, use `Number.isNaN()`.

We can check for all three bottom values with the following test:

## Increment (`++`) And Decrement (`--`) [#](https://www.smashingmagazine.com/2023/12/making-sense-of-senseless-javascript-features/#increment-and-decrement)

As developers, we tend to spend more time reading code rather than writing it. Whether we are reading documentation, reviewing someone else’s work, or checking our own, **code readability will increase our productivity over brevity**. In other words, readability saves time in the long run.

That’s why I prefer using `+ 1` or `- 1` rather than the increment (`++`) and decrement (`--`) operators.

It’s illogical to have a different syntax exclusively for incrementing a value by one in addition to having a pre-increment form and a post-increment form, depending on where the operator is placed. It is very easy to get them reversed, and that can be difficult to debug. They shouldn’t have a place in your code or even in the language as a whole when we consider where the increment operators come from.

As we saw in a [previous article](https://www.smashingmagazine.com/2023/12/marketing-changed-oop-javascript/), JavaScript syntax is heavily inspired by the C language, which uses pointer variables. Pointer variables were designed to store the memory addresses of other variables, enabling dynamic memory allocation and manipulation. The `++` and `--` operators were originally crafted for the specific purpose of advancing or stepping back through memory locations.

Nowadays, pointer arithmetic has been proven harmful and can cause accidental access to memory locations beyond the intended boundaries of arrays or buffers, leading to memory errors, a notorious source of bugs and vulnerabilities. Regardless, the syntax made its way to JavaScript and remains there today.

While the use of `++` and `--` remains a standard among developers, an argument for readability can be made. Opting for `+ 1` or `- 1` over `++` and `--` not only aligns with the principles of clarity and explicitness but also avoids having to deal with its pre-increment form and post-increment form.

Overall, it isn’t a life-or-death situation but a nice way to make your code more readable.

## Conclusion [#](https://www.smashingmagazine.com/2023/12/making-sense-of-senseless-javascript-features/#conclusion)

JavaScript’s seemingly senseless features often arise from historical decisions, compromises, and attempts to cater to all needs. Unfortunately, it’s impossible to make everyone happy, and JavaScript is no exception.

> [JavaScript doesn’t have the responsibility to accommodate all developers, but each developer has the responsibility to understand the language and embrace its strengths while being mindful of its quirks.](https://twitter.com/share?text=%0aJavaScript%20doesn%e2%80%99t%20have%20the%20responsibility%20to%20accommodate%20all%20developers,%20but%20each%20developer%20has%20the%20responsibility%20to%20understand%20the%20language%20and%20embrace%20its%20strengths%20while%20being%20mindful%20of%20its%20quirks.%0a&url=https://smashingmagazine.com%2f2023%2f12%2fmaking-sense-of-senseless-javascript-features%2f)

I hope you find it worth your while to keep learning more and more about JavaScript and its history to get a grasp of its misunderstood features and questionable decisions. Take its amazing prototypal nature, for example. It was obscured during development or blunders like the `this` keyword and its multipurpose behavior.

Either way, I encourage every developer to research and learn more about the language. And if you’re interested, I go a bit deeper into questionable areas of JavaScript’s design in [another article published here on Smashing Magazine](https://www.smashingmagazine.com/2023/12/marketing-changed-oop-javascript/)!

![Smashing Editorial](https://www.smashingmagazine.com/images/logo/logo--red.png) (gg, yk)
