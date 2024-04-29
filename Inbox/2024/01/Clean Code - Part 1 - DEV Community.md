---
created: 2024-01-20T15:53:18 (UTC -03:00)
tags: [webdev,javascript,beginners,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/sumisastri/clean-code-part-1-7h3
author: SumiSastri
---

# Clean Code - Part 1 - DEV Community

> ## Excerpt
> What is clean code?   There are some that are of the view that if code works then does it...

---
## [](https://dev.to/sumisastri/clean-code-part-1-7h3#what-is-clean-code)What is clean code?

There are some that are of the view that if code works then does it matter if it is clean. It works right?

But code written any how, in any way means that someone else reading our code - or even our future selves - may not understand the code when we see it again. It becomes a cognitive waste of energy trying to decipher it.

There is a value and effort put into writing code therefore it stands to reason that some value and effort must be derived from reading it, running it and maintaining it.

Even if clean code is written just as a matter of self-development, that is in my opinion, a goal worth achieving. It gives you mastery over the code you have written, therefore an ability to transfer that knowledge. It provides autonomy, self-sufficiency and the confidence, perhaps to write new and better code. There is a sense of purpose and a clarity of direction writing code that you and your team are proud of, and of course if it does not work, it isn't code. So it must work but also work hopefully elegantly and efficiently.

## [](https://dev.to/sumisastri/clean-code-part-1-7h3#clean-code-benefits-organisations)Clean code benefits organisations

Even if clean code does not benefit the individual code writer, surely, clean code is important as it provides organisations with code that they can sell to their clients as it is both reliable and predictable.

Refactoring code, writing it better, removing legacy and tech debt makes existing customers trust the software they are using and inspires hopefully both trust and confidence.

From a commercial point of view renewals and retaining customers is doubly important - long-term revenue - compared to new customers where the costs of acquisition are high.

Today, tech is everywhere. AI is no longer the realm of geekdom. Computers, personal laptops, wearables, phones, tablets - there is software everywhere. In weighing machines, slot machines, teller counters, CCTV cameras, doorbells, washing machines - tech is now both useful and intrusive.

## [](https://dev.to/sumisastri/clean-code-part-1-7h3#code-impacts-lives-so-the-cleaner-code-the-better)Code impacts lives so the cleaner code the better

I am not particularly keen on virtue signalling or talking about the moral responsibility of writing clean code. I am interested in the side-effects of writing bad code - law-suits, repair costs, product recalls.

Non-tech professionals will blame code writers for badly written code - look at the Volkswagen emissions case, or the privacy intrusions of search engines and social media software.

Scapgoating of dev teams and individual code writers have a terrible impact which could be loss of livelihood, reputation and worse - fines and imprisionment.

Keeping code clean therefore becomes an armour a sword of protection. And it does not have to be difficult.

For organisations it is a high cost and training and development of tech teams is as much an investment in your product as other research and development investments.

## [](https://dev.to/sumisastri/clean-code-part-1-7h3#design-and-clean-code-go-handinhand)Design and clean code go hand-in-hand

Working to a robust architectural design provides code with a strong foundation. To the architect, lies the task of figuring out if is the structure intuitive and the patterns clear. For the code writer, if the architecture is not well defined talk with other people in the team and work on a shared understanding of the design-patterns you will use as a team.

Often, as a code writer, you are writing on a body of work that already exists. It becomes easy to iterate already established patterns. Search for a good example you can follow: webhook, service, unit tests, integration tests, etc., there will be several your fellow code writers have invested good time writing. Follow their lead.

Often, unfamiliar code becomes an excuse to describe something as high effort. Let it serve as a signal that you need to invest in learning the approach. Work on a side project, check the code out, make your mistakes in a sandbox.

## [](https://dev.to/sumisastri/clean-code-part-1-7h3#all-code-needs-to-be-maintained-clean-code-needs-less-maintenance)All code needs to be maintained, clean code needs less maintenance

Code smells appear when code rots. Every line of code that you write should be read multiple times as once written, code already begins the decay process. My checklist in no particular order:-

-   Remove commented code (or anything similar, debuggers, etc.)
-   If something is not in spec leave it out
-   Check that you have added tests
-   Think about code dependencies
-   Work on tech debt and legacy to see if you can update and renew your code base
-   Heavily coupled or unclear code is very difficult to change
-   Value objects over arrays - they make the interface more obvious/ help to keep them cleaner
-   Avoid private methods as they are difficult to test, and could hide complex logic
-   Write better functions
-   Is there too much bloat in your code - is there something you can refactor that works more efficiently?
-   Contra-intuitively to the above, if 2 lines makes your code clearer, maybe 2 lines is better than 1 line - don't be a slave to writing DRY (don't repeat yourself) code
-   Write better variable names - 'n' - finally what is 'n' ? Any number of things if you come back and try to decipher it or someone else needs to decipher it - waste of cognitive energy isn't it easier to understand a variable that is `let number = () => {}` Instead of `n` which could mean infinity as well
-   Think about the packages/ libraries you add - are they performant, well-maintained, do they compile well, are issues reported and fixed

## [](https://dev.to/sumisastri/clean-code-part-1-7h3#uncle-bob-says-)Uncle Bob says ...

Much of this blog is inspired by watching Uncle Bob's, [also known as Robert C Martin](https://dev.to/sumisastri/(https://en.wikipedia.org/wiki/Robert_C._Martin)), YouTube videos when I need inspiration. Here are some of my key take outs from what he says in simple terms.

-   is your code easy to understand
-   does your brain relax when you read the code
-   does it read like good prose
-   does it step up from most important first - code execution - to least important last - gives parser place to exit quick
-   does the way the function you write start with simple first to complex later

[For those who don't know this industry maverick check this video out](https://www.youtube.com/watch?v=7EmboKQH8lM)

I am sure this conversation will grow. I am sure there will be many disagreements. There will definitely be divided opinions about what is clean code.

That's great! Let's start talking more about it. And for sure, doing more as a result of all of the talk. So let's keep it clean (both the talk and the code!).

Uncle Bob may disagree - as he says "let's not ship sh$@!X%".

[The complete series on my blog](https://sumisastri.github.io/dev-blogs/clean-code/)

Photo by [Crystal de Passillé-Chabot](https://unsplash.com/@cchabot?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/clear-spray-bottle-9gzU1mtTzWM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
