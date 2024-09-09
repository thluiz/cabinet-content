---
created: 2024-09-05T17:54:13 (UTC -03:00)
tags: [webdev,beginners,architecture,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/primalskill/on-writing-good-code-3p3f?ref=dailydev
author: 
---

# On Writing Good Code - DEV Community

> ## Excerpt
> Over the years I have realized that delivered code is light-years better than beautiful but useless...

---
Over the years I have realized that delivered code is light-years better than beautiful but useless code. This of course is not to belittle the "code artists" I look up to who have the mental capability to deliver "JIT code" that is also clean, beautiful, and reads like a good novel.

So what about the rest of us? What can we do to make our code just that tiny bit better for our future selves and colleagues who will maintain it?

I have gathered a few principles, North Stars if you will, to guide me along this journey, and the first is that:

> Programs must be written for people to read, and only incidentally for machines to execute. -- Hal Abelson

Even though code will ultimately be interpreted and executed by machines, the code base itself should always be written with humans first in mind.

I have never successfully got my way around in a code base that wasn't readable and the [code architecture could easily be reasoned about](https://primalskill.blog/wins-and-trade-offs-in-software). There's something beautiful when reading well-written code, the execution path flows in our mind like well-composed music.

> A long descriptive name is better than a short enigmatic name. A long descriptive name is better than a long descriptive comment. -- Robert C. Martin

There are always two camps of people when we're talking about software, and the idea above is no exception. In the past developers and mentors always told new programmers to have small variable names, functions, and constants.

This, like everything else, in programming is nuanced and can be interpreted in multiple ways. Yes, when variables don't have any meaningful underlying logic behind them and are ephemeral such as a counter variable in a loop, by all means, should have short names.

Anything else should instantly let the reader know what it is about, a function called `UpdateProductStatusCode` is leaps and bounds better than a function called `updProd` or `upd_prod_st`.

Also, the second part of the quote A long descriptive name is better than a long descriptive comment refers to the fact that if a developer has to write a long comment to explain what a function or code block is about is usually a "code smell" to refactor the code. Explaining complex code flows is still encouraged though, I like writing long comments to explain what a function does if the execution path is complicated.

> Code is read more than it is written. -- Daniel Roy Greenfeld

I've written more about this concept in this [blog post here](https://primalskill.blog/code-is-read-more-than-it-is-written).

This quote underlines the idea that the majority of developers' time is spent understanding code and only incidentally spending time writing code. I could argue that writing code is just a side effect of thinking deeply about a solution to a very specific software problem.

> Don't comment bad code, rewrite it. -- Brian Kernighan

Commenting a badly written piece of code is lazy, but not everybody has the luxury of time to refactor code often, but nonetheless, everybody should strive for it.

Production code is inherently messy and always in need of refactoring, commenting, and explaining why a poorly written function does what it does is always encouraged in my opinion when you don't have the necessary resources to improve it.

> In programming, boring and simple is always better than smart and complex. -- Rob Pike

A straightforward and simple solution is always preferable because it is easier to understand, maintain, and debug. Simple code is more accessible to other developers who may need to work on it in the future, reducing the likelihood of introducing bugs or errors.

Complex solutions, while they might seem clever or efficient, can introduce unnecessary complications, making the code harder to read or build on top of. Complex code can become a maintenance burden, as it needs more time to understand.

I will always choose the [simple, boring, and battle-tested tech.](https://primalskill.blog/its-probably-fine) over the latest and newest shiny thing as I mentioned at the beginning of this article that delivered code is better than beautiful code that misses the deadline.
