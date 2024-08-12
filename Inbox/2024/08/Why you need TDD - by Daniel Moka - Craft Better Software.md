---
created: 2024-08-09T19:05:05 (UTC -03:00)
tags: []
source: https://craftbettersoftware.com/p/why-you-need-tdd?ref=dailydev
author: Daniel Moka
---

# Why you need TDD - by Daniel Moka - Craft Better Software

> ## Excerpt
> Why TDD is superior to any other software practices

---
## Motivation

I made an experiment. I didn’t use Test-Driven Development (TDD) for one week. Most developers don’t use TDD daily, so it shouldn’t be a big problem, right?!

**Well, it was the least productive week of my career.**

It has been a long time since I had such painful days of coding. **Writing code felt hacky, changing code was risky, and my goals were poorly defined.** My feedback loops increased, my mistakes became bigger, and lost trust in my code.

After this week, I'm convinced more than ever: TDD is the best way to write code. Here are all the reasons why:

## The silver bullet

There’s no silver bullet in the software industry. But if I needed to name one thing closest to being one, I would immediately say Test-Driven Development.

TDD won’t solve all your problems. **But it will prevent most of them.** The greatest engineers of all time such as Kent Beck, Martin Fowler, Dave Farley are all advocating TDD. The best developers I know use TDD whenever they can. What do you think, why is that?!

**Because TDD is the fastest and safest way to write code.**

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe630d53a-2858-4f99-b7ed-081d35116a59_569x363.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe630d53a-2858-4f99-b7ed-081d35116a59_569x363.png)

## The complete package to produce software

What is TDD? It’s a subject of ongoing debate. Is it a testing, development, or design tool? Well, it’s much more. It's the gold standard way of designing software involving the best practices such as taking baby steps, continuous refactoring, and writing testable code.

**It’s a blend of development, testing, and designing software.** TDD also comes with more fun and better business analyses. TDD is a complete package to produce quality software, resulting in a product that satisfies our customers.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37e9b6a-8f9d-454f-b56b-e69836ceb981_1065x572.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe37e9b6a-8f9d-454f-b56b-e69836ceb981_1065x572.png)

## Debugging is minimized

[According to research](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=0be446c155f8efcf17ef3a103188c6ae6d27d9b4), developers spend ~50% of their time debugging.

**!!! 50% !!!**

Let that sink in, and imagine how much money is wasted on debugging in companies. Debugging is an unproductive activity. It means we don't know how the code works. So developers don’t know how their code works 50% of the time. What a waste of resources.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F34bbc481-6548-486c-ac03-f97110231263_1158x450.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F34bbc481-6548-486c-ac03-f97110231263_1158x450.png)

Test-Driven Development fixes this. **TDD eliminates all the waste as you always know that your code works.** TDD prevents bugs as you test every step you make. If you use TDD, you will forget how to use your debugger.

## Managing complexity

The biggest problem with writing code without TDD is that nothing forces us to take baby steps. Without TDD, we tend to solve bigger tasks. Big tasks lead to a bigger mental load. Big tasks lead to bigger mistakes. Here is a question:

Can a juggler hold 7 balls in the air?

It’s possible, but the complexity makes it easy to fail. The same happens when you code without TDD: you try to keep many things in your head at once. But complexity often leads to mistakes.

Test-Driven Development fixes this. **TDD helps manage complexity by taking baby steps and solving one little problem at a time.** It reduces mental load and lets you focus on only one thing: the currently failing test.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0c8cd4a5-a6c8-423e-a630-420c149c3a3e_1189x572.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0c8cd4a5-a6c8-423e-a630-420c149c3a3e_1189x572.png)

## 100% code and behavior coverage by design

It’s 2024 and I still meet a lot of teams that heavily rely on code coverage. They use it in their CI/CD pipelines, aiming for 90-100% coverage. Once they have it, they're happy to deploy to PROD.

**The problem is that code coverage isn’t a quality metric.** You can have 100% coverage without any test assertions. Code coverage can help to detect untested areas of the code, **but it doesn't tell anything about the tested areas.** It gives a false sense of security, giving no information about the quality of your tests. Mutation testing could solve this, but that is also far from perfect.

What is the solution? Test-Driven development. **By using TDD you end up with a 100% code coverage and behavior coverage as a byproduct.** There’s no other software practice that has such a guarantee.

## Test-last is boring, test-first is fun

Writing test later sucks all the joy of writing tests. This is one of the reasons why most developers don’t like testing. Because they write tests after the fact.

Test-Driven Development fixes this. The joy of writing tests in TDD comes from the sense of progress you get from doing it. **TDD is a game-like feeling to write your code in a turn-based way, with exact goals to achieve.** The TDD cycle turns coding into a fun activity. The outcomes are visible and immediate, continuously boosting our morale and creativity.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3fe93a25-1cac-4031-9874-44b30597a06e_572x186.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3fe93a25-1cac-4031-9874-44b30597a06e_572x186.png)

## Conclusion

Think twice before you skip TDD. When you do TDD:

-   Bugs are prevented
    
-   Debugging times are minimized
    
-   Tests are used for designing code
    
-   Continuous refactoring becomes a habit
    
-   Code becomes testable as a byproduct
    
-   Code becomes easy and cheap to change
    
-   Quality is built in from day one
    
-   Code results in 100% code and behavior coverage
    

Don’t forget that TDD is just a means to an end: writing quality code that works. To achieve this, TDD works best for me. However, I am happy to ditch TDD if I learn something better. **But as long as TDD remains the best way to write code, I will continue to use it to Craft Better Software.**

## How to learn TDD

If you want to learn Test-Driven Development, then [check out my new TDD course course](https://transformyourcraft.com/). Join ~200 developers in this in-depth course that will transform the way you build software.

It gives you:

-   **A hands-on 4+ hours video course on TDD**
    
-   **A TDD e-book containing 10+ years of my experience**
    
-   **Guidelines to master testing and refactoring**
    
-   **Real-world projects in C#, TypeScript and Rust**
    
-   **Source code for the exercises**
    
-   **1:1 private mentorship for extra support**
    

[Get Instant Access](https://transformyourcraft.com/)
