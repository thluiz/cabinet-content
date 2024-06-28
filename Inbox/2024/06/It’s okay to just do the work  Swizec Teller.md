---
created: 2024-06-27T17:55:26 (UTC -03:00)
tags: []
source: https://swizec.com/blog/its-okay-to-just-do-the-work/?ref=dailydev
author: 
---

# It’s okay to just do the work | Swizec Teller

> ## Excerpt
> Not everything needs to work forever. Start by solving the problem

---
You don't always have to build a long-term sustainable solution to every problem. Sometimes you can just get it done.

> Remember, it's 99.9% likely that the thing standing between you and shipping is not the framework/library feature/tool/pattern discussion of the day. It's work. Buckle down and do it.
> 
> — Tanner Linsley (@tannerlinsley) [October 30, 2023](https://twitter.com/tannerlinsley/status/1719034968897470501)

## [](https://swizec.com/blog/its-okay-to-just-do-the-work/?ref=dailydev#get-it-done)[Get it done](https://swizec.com/blog/its-okay-to-just-do-the-work/?ref=dailydev#get-it-done)

We once found a bunch of user accounts that failed to cancel. The bug wasn't interesting, once we found the issue it was quick to fix. But what do you do with all this production data that's in the wrong state?

You can't rollback the database because this wasn't a new issue. Bad accounts going back several months were interspersed with accounts that are valid. You can't fix data directly in the database either because the change needs to run side-effects in the application layer. There's a bunch of stuff that happens when you cancel an account.

We all got in a meeting and started to debate: Do we build a CLI script? Make a button for someone to press? How do we identify which accounts need fixing? What's the best way to import a spreadsheet of names? How do we make sure it's done right?

After an hour of back-and-forth our PM goes _"You guys are driving me crazy. There's like 100 of these people and it takes me 3 seconds to fix each of them by hand in our admin tool. I'll just do it myself. This conversation isn't worth our time"_

Fueled by pure exasperation, she then spent 2 hours clicking around the admin interface and solved the problem. Zero code, just work. It was the perfect solution.

## [](https://swizec.com/blog/its-okay-to-just-do-the-work/?ref=dailydev#dont-overthink-the-work)[Don't overthink the work](https://swizec.com/blog/its-okay-to-just-do-the-work/?ref=dailydev#dont-overthink-the-work)

Not everything needs to work forever. It's okay to solve today's problem and worry about tomorrow's problems tomorrow.

Titus Winters says in [Software Engineering at Google](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/) that _"Software engineering is programming over time"_, and he's right, but often software engineering is about finding a solution that barely works. Not every problem is worth the full might of your engineering team.

Would manually cancelling user accounts work at Google scale? No, there's too many. Would it always work at startup scale? Nope. Even cancelling a few hundred by hand would be tough. Was it worth the PM's time? Probably not. She's pretty busy.

Was 2 hours of tedium worth avoiding the full overhead of writing a story, adding it to the sprint, deploying all the engineers, dealing with code review, and all the validation that comes after? Damn right it was.

Once a pointed story is on the board, it takes at least 4 person hours to resolve:

-   review story to make sure it's ready for the team
-   estimate and point the work
-   meet to plan your solution
-   do the work
-   review your code
-   validate in a safe environment
-   deploy to prod

For small stories these steps don't take a full hour. But your whole team is there and their time adds up fast. A 10 minute meeting between 6 people makes 1 person-hour of work.

Before you do all that, evaluate what size problem you're even solving. Anything less than 4 hours isn't worth putting on the board. Or at least it's not worth using the whole process.

Cheers,  
~Swizec

Published on June 21st, 2024 in [Scaling Fast Book](https://swizec.com/categories/scaling%20fast%20book/), [Engineering](https://swizec.com/categories/engineering/)

#### Did you enjoy this article?

**Have a burning question that you think I can answer?** Hit me up on [twitter](https://twitter.com/swizec) and I'll do my best.

**Who am I and who do I help?** I'm Swizec Teller and I turn coders into engineers with _"Raw and honest from the heart!"_ writing. No bullshit. Real insights into the career and skills of a modern software engineer.

**Want to become a _true_ senior engineer?** Take ownership, have autonomy, and be a force multiplier on your team. The Senior Engineer Mindset ebook can help 👉 [swizec.com/senior-mindset](https://swizec.com/senior-mindset). These are the shifts in mindset that unlocked my career.

**Curious about Serverless and the modern backend?** Check out Serverless Handbook, for frontend engineers 👉 [ServerlessHandbook.dev](https://serverlesshandbook.dev/)

**Want to Stop copy pasting D3 examples and create data visualizations of your own?** Learn how to build scalable dataviz React components your whole team can understand with [React for Data Visualization](https://reactfordataviz.com/)

**Want to get my best emails on JavaScript, React, Serverless, Fullstack Web, or Indie Hacking?** Check out [swizec.com/collections](https://swizec.com/collections)

**Did someone amazing share this letter with you?** Wonderful! You can sign up for my weekly letters for software engineers on their path to greatness, here: [swizec.com/blog](https://swizec.com/blog)

**Want to brush up on your modern JavaScript syntax?** Check out my interactive cheatsheet: [es6cheatsheet.com](https://es6cheatsheet.com/)

**By the way, just in case no one has told you it yet today: I love and appreciate you for who you are ❤️**
