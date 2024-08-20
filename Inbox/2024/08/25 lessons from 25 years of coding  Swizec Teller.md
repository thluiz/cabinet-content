---
created: 2024-08-20T16:47:11 (UTC -03:00)
tags: []
source: https://swizec.com/blog/25-lessons-from-25-years-of-coding/
author: 
---

# 25 lessons from 25 years of coding | Swizec Teller

> ## Excerpt
> Today is my birthday which means I've been coding for 25 years. A quarter century 😱 Here's 25 lessons I've learned about code. In no particular order.

---
today is my birthday which means I've been coding for 25 years. A quarter century 😱

When I started, we didn't have a computer at home. An extra curricular class at school gave me 1 hour of programming per week. In [Logo](https://en.wikipedia.org/wiki/Logo_(programming_language)).

I don't remember which version we used but

```logo
repeat 4 [ fd 100 rt 90 ]
```

blew my 9 year old mind (it draws a square) and in that moment I knew that programming was my thing. One day I'll be a famous programmer like Bill Gates 💪

A lot has changed since then. Bill Gates isn't even a famous _programmer_ anymore.

Here's 25 lessons I've learned about code. In no particular order.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#1-coding-is-a-team-sport)[1\. coding is a team sport](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#1-coding-is-a-team-sport)

To go fast, go alone; to go far, go together.

Doesn't matter how good you are, there's a limit to how much you can do yourself. And a team can do all those things you're starting to think are boring.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#2-tests-are-great-but-tdd-is-a-cult)[2\. tests are great but tdd is a cult](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#2-tests-are-great-but-tdd-is-a-cult)

You need tests. Automated or otherwise. Always start with _"How will I know this works?"_ and verify in small increments.

But strict test driven development is a cult. You don't need to write an automated test and incur aeons of maintenance for every smol step.

> Write tests. Not too many. Mostly integration.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#3-mocks-make-brittle-tests)[3\. mocks make brittle tests](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#3-mocks-make-brittle-tests)

Mocks are said to make your code more unit testable. Help you isolate different units to ensure you have reliable tests testing what you meant to test. None of that other code by those other developers.

Can't rely on that!

Guess what, your code _does_ rely on that code and if it changes, you need to fix it. And with mocks you'll never know until you get a production bug.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#4-types-solve-80-of-the-need-for-unit-tests)[4\. types solve 80% of the need for unit tests](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#4-types-solve-80-of-the-need-for-unit-tests)

The main reason you write unit tests is to ensure you call functions correctly, don't make typos, and get test failures when a function's API changes.

A static type system tells you all that on every keypress. Runs right in your IDE. Gives you red squiggly lines when something doesn't match.

Types are unit tests with 100% coverage, if you squint a little.

You do need \[integration\] tests for behaviors.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#5-write-less-code)[5\. write less code](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#5-write-less-code)

Occam's razor style – the simplest solution that solves the problem is best. When you start adding exception upon exception, it means your core solution is wrong. Reconsider.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#6-lean-into-existing-abstractions)[6\. lean into existing abstractions](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#6-lean-into-existing-abstractions)

You see this in large projects. Programmer finds a function that does what they need. But they don't quite understand how it works or why it exists.

They wrap the function in an abstraction of their own. A cozy little corner of the codebase they can understand.

Few months later, another programmer finds this function. Same thing happens.

Before you know it your codebase is full of abstractions of abstractions of abstractions and you have to spelunk through 10 layers of indirection to find a function that _does_ something.

Stop that. Lean into code that exists.

Same with tools. You add workarounds when you don't understand the libraries and tools you're using.

Embrace what the authors envisioned. Do it their way. It's going to be fine, I promise.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#8-80-of-the-work-is-json-bureaucracy)[8\. 80% of the work is json bureaucracy](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#8-80-of-the-work-is-json-bureaucracy)

College, online challenges, and interviews make you think that the work of programming is a glorious fight to the death with gnarly issues and never-before-seen problems.

That may be true in some fields for some people. The job for most is ferrying data from one side of a system to the other and back.

> Databases are snake\_case!  
> API layer is snake\_case!  
> JavaScript is camelCase!
> 
> How many engineer decades have we lost as an industry on converting between these formats for zero benefit?
> 
> — Swizec Teller (@Swizec) [October 6, 2021](https://twitter.com/Swizec/status/1445813080487706627)

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#9-success-is-entrepreneurial)[9\. success is entrepreneurial](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#9-success-is-entrepreneurial)

Nobody is coming to save you. If you want something, go get it. Don't ask, don't know, don't get.

Want bigger challenges? Ask. Want more interesting problems? Ask. Want more responsibility? Ask. Want fame in opensource? Make things. Want millions? Sell things. Want a blurb in every textbook? Solve hard problems. Want fame in industry? Achieve things.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#10-its-a-programming-language-not-an-identity)[10\. it's a programming language not an identity](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#10-its-a-programming-language-not-an-identity)

Imagine how much you'd laugh at someone who builds their identity around the brand of hammer they use.

Programming languages are hammers.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#11-learn-different-paradigms)[11\. learn different paradigms](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#11-learn-different-paradigms)

The best way to improve as a programmer is to learn a new programming paradigm.

Clojure changed how I think about processing data, Haskell made me love good static typing, Prolog was just weird, microcontrollers helped me think like a computer – all of it helps me write better JavaScript.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#12-learning-new-languages-and-libraries-is-easy)[12\. learning new languages and libraries is easy](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#12-learning-new-languages-and-libraries-is-easy)

Once you understand the fundamentals and the problems you're solving, learning new languages and libraries and APIs is easy. They're all roughly the same.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#13-learning-new-ecosystems-is-hard)[13\. learning new ecosystems is hard](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#13-learning-new-ecosystems-is-hard)

Learning a new syntax is easy. Fluency comes harder.

You have to learn the common idioms and patterns of a community, their favorite tools, how they hold a language, what they use it for. You can't learn this from online exercises and tutorials.

You have to build something real.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#14-the-ecosystem-changes-every-5-years)[14\. the ecosystem changes every 5 years](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#14-the-ecosystem-changes-every-5-years)

> 5+ years have passed. You have a whole generation of programmers who have never felt the pain that React solves and don’t understand why it exists.
> 
> — Swizec Teller (@Swizec) [October 23, 2021](https://twitter.com/Swizec/status/1452013220110077954)

The number of programmers about doubles every 5 years. The doubling is globally uneven. Right now India is growing _much_ faster than USA or Europe.

With new programmers come new solutions to old problems. They don't know the old solutions.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#15-academia-figured-it-out-20-years-ago)[15\. academia figured it out 20 years ago](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#15-academia-figured-it-out-20-years-ago)

You think you've found a novel solution to an exciting problem, but there's probably an old paper about it. At least about solving the same problem in a different context.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#16-fundamentals-havent-changed-since-the-80s)[16\. fundamentals haven't changed since the 80's](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#16-fundamentals-havent-changed-since-the-80s)

Modern computing started in the 1940's. The [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture) was proposed in 1945. Our computers still use it today.

Computer networks are from the 60's, neural networks from the 70's, reliable databases from the 80's.

We've reinvented and rediscovered lots of tools since then, but fundamentally it's all the same. You might argue blockchain is at least trying to be new.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#17-you-cant-eliminate-complexity)[17\. you can't eliminate complexity](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#17-you-cant-eliminate-complexity)

We use computer systems to solve hard organizational problems. Your code and system can't be simpler than the problem it's solving.

But try not to make it more complex than that.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#18-if-you-cant-explain-the-problem-you-cant-solve-the-problem)[18\. if you can't explain the problem you can't solve the problem](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#18-if-you-cant-explain-the-problem-you-cant-solve-the-problem)

Computers are dumb. If you can't even explain your solution to another person, how can you explain it to a computer?

It's the illusion of explanatory depth. You think you know how a pen works or what bicycle looks like until you try to explain the pen and draw the bike.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#19-a-new-perspective-is-worth-80-iq)[19\. a new perspective is worth 80 IQ](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#19-a-new-perspective-is-worth-80-iq)

Many problems become simpler when you approach them a different way.

When your code is getting messy and you keep finding exceptions – try a new approach. You are likely using the wrong tool or the wrong [level of computing strength](https://en.wikipedia.org/wiki/Automata_theory).

You will never solve a contextual problem with a state machine well. And a graph problem is difficult to solve with hash tables.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#20-you-cant-learn-through-repetition)[20\. you can't learn through repetition](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#20-you-cant-learn-through-repetition)

Programming is not karate. You can't kata your way into being a good problem solver.

Bruce Lee fears the man who practiced the same kick 10,000 times, but problems fear the programmer who has 10,000 different tricks to try.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#21-code-without-users-is-useless)[21\. code without users is useless](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#21-code-without-users-is-useless)

Coding would be easier without those pesky users and business requirements. And what's the point of that?

The fun comes with changing and evolving your code to meet new requirements. That's the art. Programming over time.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#22-write-code-that-nudges-others-to-write-good-code)[22\. write code that nudges others to write good code](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#22-write-code-that-nudges-others-to-write-good-code)

Want a clean codebase that makes you proud? Write code that encourages good practices and leads by example. You won't always be there to keep it nice.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#23-industry-best-practices-are-for-large-teams)[23\. industry best practices are for large teams](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#23-industry-best-practices-are-for-large-teams)

Don't believe everything you read online. What works for a team of 10,000 may not work for your team of 10. And what works for a team of 10, makes no sense on a solo project of 1.

Think for yourself.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#24-a-bad-reason-is-better-than-no-reason)[24\. a bad reason is better than no reason](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#24-a-bad-reason-is-better-than-no-reason)

Always know _why_ you're doing something. Engineering is about tradeoffs and if you can't tell why you're doing something, don't do it.

_"Because <industry expert> said so"_ is not a reason. Neither is _"The boss told me to"_. Ask them to explain the tradeoffs, helps you learn.

The corollary to that is to _always explain your reasoning_ when helping others. Make a decision, then explain why. Next time they'll understand the reasoning and won't have to ask you.

## [](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#25-use-the-same-words-as-the-suits)[25\. use the same words as the suits](https://swizec.com/blog/25-lessons-from-25-years-of-coding/#25-use-the-same-words-as-the-suits)

> I think yall reinventing Domain Driven Design
> 
> step 1: Use the same words to talk about the same things  
> step 2: Yes use those words in code too  
> step 3: Yes in design too  
> step 4: Yes also in public  
> step 5: Yes in decks too  
> step 6: Yes also with investors  
> step 7: Really, everywhere
> 
> — Swizec Teller (@Swizec) [October 22, 2021](https://twitter.com/Swizec/status/1451353171108921350)

Use verbs and phrases from the business side and never wonder what to name a function or variable ever again.

Cheers,  
~Swizec

Published on October 24th, 2021 in [Year in review](https://swizec.com/categories/year%20in%20review/)

#### Did you enjoy this article?

#### Continue reading about 25 lessons from 25 years of coding

Semantically similar articles hand-picked by GPT-4

-   [My favorite lessons from Pragmatic Programmer](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/)
-   [What I learned from Software Engineering at Google](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/)
-   [Why I will always suck at programming \[Grega Stritar\]](https://swizec.com/blog/why-i-will-always-suck-at-programming-grega-stritar/)
-   [4 years of coding in San Francisco, lessons learned](https://swizec.com/blog/4-years-of-coding-in-san-francisco-lessons-learned/)
-   [Don't neglect your upgrades](https://swizec.com/blog/dont-neglect-your-upgrades/)

**Have a burning question that you think I can answer?** Hit me up on [twitter](https://twitter.com/swizec) and I'll do my best.

**Who am I and who do I help?** I'm Swizec Teller and I turn coders into engineers with _"Raw and honest from the heart!"_ writing. No bullshit. Real insights into the career and skills of a modern software engineer.

**Want to become a _true_ senior engineer?** Take ownership, have autonomy, and be a force multiplier on your team. The Senior Engineer Mindset ebook can help 👉 [swizec.com/senior-mindset](https://swizec.com/senior-mindset). These are the shifts in mindset that unlocked my career.

**Curious about Serverless and the modern backend?** Check out Serverless Handbook, for frontend engineers 👉 [ServerlessHandbook.dev](https://serverlesshandbook.dev/)

**Want to Stop copy pasting D3 examples and create data visualizations of your own?** Learn how to build scalable dataviz React components your whole team can understand with [React for Data Visualization](https://reactfordataviz.com/)

**Want to get my best emails on JavaScript, React, Serverless, Fullstack Web, or Indie Hacking?** Check out [swizec.com/collections](https://swizec.com/collections)

**Did someone amazing share this letter with you?** Wonderful! You can sign up for my weekly letters for software engineers on their path to greatness, here: [swizec.com/blog](https://swizec.com/blog)

**Want to brush up on your modern JavaScript syntax?** Check out my interactive cheatsheet: [es6cheatsheet.com](https://es6cheatsheet.com/)

**By the way, just in case no one has told you it yet today: I love and appreciate you for who you are ❤️**
