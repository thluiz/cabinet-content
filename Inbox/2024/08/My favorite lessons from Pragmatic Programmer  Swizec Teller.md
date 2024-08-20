---
created: 2024-08-20T16:46:21 (UTC -03:00)
tags: []
source: https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/
author: 
---

# My favorite lessons from Pragmatic Programmer | Swizec Teller

> ## Excerpt
> Swizec shares software engineering lessons from production in his books, articles, talks, and workshops

---
Pragmatic Programmer is a book everyone should read at the start of their career. The earlier the better.

Don't be like me and wait until every lesson is a _"Oh yeah, learned that the hard way"_ ... _"Hah yes! That was a fun and painful lesson"_ ... _"ooo good one! I remember a project just like that. Fuck that hurt"_

  [![i2LW3tC](https://swizec.com/static/9157b317cd51f61093cf4cb00d78b0f3/b5245/i2LW3tC.png)](https://pragprog.com/book/tpp20/the-pragmatic-programmer-20th-anniversary-edition)

And if you _are_ like me, I suggest reading Pragmatic Programmer anyway.

David Thomas and Andrew Hunt put into words a lot of those little feelings inside your gut that you can't quite verbalize.

When you say _"No, we shouldn't do it that way."_ and someone asks why and you're like _"I don't know. It just doesn't feel right."_

"it doesn't feel right" is no argument, so your team does it _that_ way anyway. 6 months later the codebase goes to shit and you're like _"See! That's why"_.

But now it's too late.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#the-cat-ate-my-source-code)[The Cat Ate My Source Code](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#the-cat-ate-my-source-code)

> “Don’t blame someone or something else, or make up an excuse. Don’t blame all the problems on a vendor, a programming language, management, or your coworkers. Any and all of these may play a role, but it is up to _you_ to provide solutions, not excuses.”

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#good-enough-software)[Good-Enough Software](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#good-enough-software)

Building software is about trade-offs. Perfect software doesn't exist and the more you chase perfection, the more off target you'll be.

Good enough doesn't mean "sloppy". It means good enough to fulfill _all_ requirements, but no better.

Don't waste time and effort on things nobody needs.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#drythe-evils-of-duplication)[DRY—The Evils of Duplication](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#drythe-evils-of-duplication)

DRY – do not repeat yourself, is a maxim of the software industry. Touted as a basic wisdom, it's caused more harm than good over the years.

The beginner thinks DRY and bends over backwards to avoid duplicating her code.

The expert thinks DRY and copy pastes code to avoid duplicating the architecture.

Duplicate your code, not your intent. Just because it looks the same doesn't mean it is the same.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#tracer-bullets)[Tracer Bullets](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#tracer-bullets)

It's hard to hit your target. Especially when you can't see the target.

That's why machine gunners use tracer bullets – glow-in-the-dark bullets loaded as every 5th on the reel. They help you see where you're shooting.

The same works for software.

Build a working happy path version of your program first. That's your tracer bullet.

Users see if you're going the right way, you and your team get a skeleton to hang edge cases off of. This is not a prototype. This is part of your real code.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#prototypes-and-post-it-notes)[Prototypes and Post-it Notes](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#prototypes-and-post-it-notes)

Prototypes are throwaway. You build them to explore a new idea, technology, or architecture.

Do not refactor a prototype into production code. Stay away from shipping a prototype _as_ production code. Despite the business folks protestations of _"But it already works!"_

Best build your prototypes with post-it notes instead of code. Removes temptation.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#engineering-daybooks)[Engineering Daybooks](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#engineering-daybooks)

[When you code, write down everything](https://swizec.com/blog/when-you-code-write-down-everything/)

Keep a notebook. Write down your thoughts and ideas. When you get distracted, you can look it up. When you come back to some code 2 weeks later, you can look it up.

  ![CRQ522W](https://swizec.com/static/0d3a687b739aca051749302b14a37108/6b2ea/CRQ522W.png)

As the old proverb from the balkans says: _Budala pamti, pametan piše_. The fool remembers, the clever one writes.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#dont-outrun-your-headlights)[Don't Outrun Your Headlights](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#dont-outrun-your-headlights)

Build only as much as you can test. Run your code early, run it often.

Don't code for 2 hours then see if it works. You'll find it's hard to tell which change broke your program.

Test your code after every significant change.

What's significant depends on many factors. When you're new to a codebase you'll make smaller steps than when it all fits in your head.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#transforming-programming)[Transforming Programming](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#transforming-programming)

> “All programs transform data, converting an input into an output.”

When you think of code as a series of transformations, your life will be easier. Small discrete steps, rather than large objects and codebases.

Spend an afternoon learning about functional programming.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#inheritance-tax)[Inheritance Tax](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#inheritance-tax)

Inheritance is a trap, often misused, rarely understood.

Are you using inheritance to share code? Try mixins or traits.

Are you using inheritance to build types? The real world never fits a clean taxonomy. Try interfaces instead.

"Can do X" trumps "Is a X".

Modern computing is full of concurrency and parallelism. Anything you build has to deal with this reality.

Shared state is your enemy.

Immutable shared state and mutable local state are your friends. Avoid relying on state others may have changed.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#programming-by-coincidence)[Programming by Coincidence](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#programming-by-coincidence)

Do you know _why_ your code works?

If not, it might be a coincidence. Code that works for the wrong reason is worse than code that doesn't work.

Verify until you're certain.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#coconuts-dont-cut-it)[Coconuts Don't Cut It](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#coconuts-dont-cut-it)

Do what works, not what's fashionable.

You are not Google. You are not Facebook, Amazon, Netflix, Apple, or Spotify either. What works for them will not work for you.

Listen to the trends, try their ideas, judge for yourself.

What works for Google in 2020 wouldn't work for Google in 2010. Every team is different. Find what works for you now. Change when it stops working.

## [](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#delight-your-users)[Delight Your Users](https://swizec.com/blog/my-favorite-lessons-from-pragmatic-programmer/#delight-your-users)

> “Your users are not particularly motivated by code. They have a business problem that needs solving within the context of their objectives and budget.”

Find what your users need, not just what they ask.

The biggest danger to a software team is a client that limits their ask based on perceived software limitations. Encourage clients to ask for their wildest dream. Work together to find a solution.

Their goal is not the tool, their goal is what your tool enables them to do.

  ![Bfmuc3C](https://swizec.com/static/c18359cb5761fc7bbc227e479c57daf5/1e088/Bfmuc3C.png)

___

And remember, programming is not meant to be safe. It's a life-long lesson in building wisdom and tradeoffs.

  ![tTHHdPk](https://swizec.com/static/1a73ff689be72b54610f6025df22c2d1/4ef49/tTHHdPk.png)

Cheers,  
~Swizec

Published on March 27th, 2020 in [Learning](https://swizec.com/categories/learning/), [Personal](https://swizec.com/categories/personal/), [Books](https://swizec.com/categories/books/)

#### Did you enjoy this article?

#### Continue reading about My favorite lessons from Pragmatic Programmer

Semantically similar articles hand-picked by GPT-4

-   [25 lessons from 25 years of coding](https://swizec.com/blog/25-lessons-from-25-years-of-coding/)
-   [What I learned from Software Engineering at Google](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/)
-   [The best engineering books get good 5 years into your career](https://swizec.com/blog/the-best-engineering-books-get-good-5-years-into-your-career/)
-   [Don't neglect your upgrades](https://swizec.com/blog/dont-neglect-your-upgrades/)
-   [4 years of coding in San Francisco, lessons learned](https://swizec.com/blog/4-years-of-coding-in-san-francisco-lessons-learned/)

**Have a burning question that you think I can answer?** Hit me up on [twitter](https://twitter.com/swizec) and I'll do my best.

**Who am I and who do I help?** I'm Swizec Teller and I turn coders into engineers with _"Raw and honest from the heart!"_ writing. No bullshit. Real insights into the career and skills of a modern software engineer.

**Want to become a _true_ senior engineer?** Take ownership, have autonomy, and be a force multiplier on your team. The Senior Engineer Mindset ebook can help 👉 [swizec.com/senior-mindset](https://swizec.com/senior-mindset). These are the shifts in mindset that unlocked my career.

**Curious about Serverless and the modern backend?** Check out Serverless Handbook, for frontend engineers 👉 [ServerlessHandbook.dev](https://serverlesshandbook.dev/)

**Want to Stop copy pasting D3 examples and create data visualizations of your own?** Learn how to build scalable dataviz React components your whole team can understand with [React for Data Visualization](https://reactfordataviz.com/)

**Want to get my best emails on JavaScript, React, Serverless, Fullstack Web, or Indie Hacking?** Check out [swizec.com/collections](https://swizec.com/collections)

**Did someone amazing share this letter with you?** Wonderful! You can sign up for my weekly letters for software engineers on their path to greatness, here: [swizec.com/blog](https://swizec.com/blog)

**Want to brush up on your modern JavaScript syntax?** Check out my interactive cheatsheet: [es6cheatsheet.com](https://es6cheatsheet.com/)

**By the way, just in case no one has told you it yet today: I love and appreciate you for who you are ❤️**
