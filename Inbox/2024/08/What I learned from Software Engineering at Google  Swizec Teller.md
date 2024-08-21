---
created: 2024-08-21T14:16:08 (UTC -03:00)
tags: []
source: https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/
author: 
---

# What I learned from Software Engineering at Google | Swizec Teller

> ## Excerpt
> When I first picked up Software Engineering at Google I thought it was another one of those FAANG books full of lessons that make no sense at human scale. I was surprised, lessons apply to teams as small as 5.

---
When I first picked up [Software Engineering at Google](https://www.oreilly.com/library/view/software-engineering-at/9781492082781/) I thought it was another one of those FAANG books full of lessons that make no sense at human scale. I was surprised, the lessons apply to teams as small as 5.

  [![Software Engineering at Google cover](https://swizec.com/static/785428dc6379ab5398daf90fa456ce5d/ac471/Software-Engineering-at-Google-cover65c97d.jpg)

Software Engineering at Google cover](https://www.oreilly.com/library/view/software-engineering-at/9781492082781/)

This is a "good shit stays" recap. The lessons that stick with you a few weeks after reading.

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#software-engineering-vs-programming)[Software Engineering vs. Programming](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#software-engineering-vs-programming)

The difference between Software Engineering and Programming is at the core of this book. Titus Winters, author of the first chapter, finally made it click for me.

> Software engineering is programming over time

Programming is about writing code. You take a task and write code to solve it.

Software engineering is when you take that piece of code and consider:

-   How will this task evolve?
-   How will this code adapt to those changes?
-   What does this code encourage others to do?
-   How does this code encourage other programmers to use it?
-   How will I understand this code in 5 months?
-   How will a busy team member jumping around grok this?
-   What happens when the business becomes bigger?
-   When will this code stop being good enough?
-   How does it scale?
-   How does it generalize?
-   What hidden dependencies are there?

That's engineering 👉 considering the long-term effects of your code. Both direct and indirect.

You always face the underlying concern of _"What's the expected life span of this code?"_. How you approach a black friday marketing site is different from how you approach your company's payment system.

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#beyonce-rule-and-hyrums-law)[Beyonce rule and Hyrum's law](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#beyonce-rule-and-hyrums-law)

> If you liked it you should've put a test on it

When you're small, coordination is easy. When you're big, anyone might touch your code.

Solo programmers on small projects don't need lots of tests. The whole thing fits in your head. You know how it works.

As the system grows, you start to forget. Add a feature, fix a bug, part of the system you didn't even touch breaks. 🤨

  ![99 bugs song](https://swizec.com/static/4be3c5f057137a44f2c170597e69be2c/df7e7/99-bugs-song1eiiib.jpg)

99 bugs song

Part of the problem is [Hyrum's Law](https://www.hyrumslaw.com/). As engineers join your project, they _will_ find a way to make it work. That's a threat 😉

Any observable behavior, documented or not, supported or not, will be relied upon by someone.

You fix a bug ... Joe from billing relied on that bug for his code to work. 💩

The Beyonce rule says that if Joe liked that bug, he shoulda put a test on it. When you fix the bug, his test breaks, and you say _"Oh shit, gotta fix Joe's code too"_.

And because Joe put tests on everything he likes about _his_ code, you can go fix it without understanding all the gnarly details.

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#shift-left)[Shift left](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#shift-left)

The earlier you find a mistake, the easier it is to fix.

**Static analysis** runs in your editor. Finds typos, incorrect function calls, autocompletes code.

**Unit tests** take a few seconds to verify your code does what you think it does. Like a checksum.

**Integration tests** take a few minutes to validate your system works. May catch fun edge cases.

**Code review** takes a few hours to validate you're following standard norms and practices of your team. Great way to share knowledge and learn from others.

**QA** takes a few hours or days to ensure everything works together as expected.

**Users in production** will find everything and expose you to edge cases you never thought possible.

The later you go in this sequence, the harder to recover from an error.

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#automate-common-tasks)[Automate common tasks](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#automate-common-tasks)

  ![Scalability](https://swizec.com/static/e520ae6b776a1c597bc70050f802b023/4ef49/Scalabilityf1h168.png)

Scalability

Scaling an organization is a lot like scaling a software system. How many resources does it take to support a new engineer?

Take production deploys, for example. How much work is it to combine everyone's contributions into a new release? At Google they measure velocity in _commits per second_.

What about code reviews?

Explaining codebase norms to 1 engineer is easier than 10. If 5 engineers join every quarter, can you keep up? What about 5 per week?

**You're in trouble when overhead grows faster than engineering.** Automation helps.

Code formatters, linters, codemods, continuous integration pipelines ... anything you can use to take work off people's plates. Many tasks are more mechanical than you think.

Sometimes if you make things less flexible, take control away from your engineers, you can automate the whole thing. Deploys and server config are a great example.

Less flexibility, more automation, easier codebase.

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#stubs-and-mocks-make-bad-tests)[Stubs and mocks make bad tests](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#stubs-and-mocks-make-bad-tests)

Large sections of Software Engineering at Google are devoted to automated testing. It's the only way you can scale to an organization of that size.

The part that stuck with me was a hunch I've had for a while 👉 stubs and mocks are bad.

Your tests are only as good as your mocks. They hide the true behavior of your system, drift away from reality, and take a lot of effort to maintain.

After all that, you can't even trust your tests. Like that time I had perfectly passing unit tests, deployed to staging, and realized my code relied on a database column that doesn't exist.

But it worked with my mocked database 🙃

Google recommends using fakes instead. Simplified implementations of the real thing maintained by the same team for API parity.

Use integration tests when possible.

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#small-frequent-releases)[Small frequent releases](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#small-frequent-releases)

A small release is easier to manage. A small release is easier to revert. A small release is easier to understand.

Keep shipping.

Weekly at slowest. Daily is great. Hourly is dreamy. Minutely is ... that's for large companies :D

The bigger the change, the harder to figure out which commit or feature broke production. Small changes make it obvious.

Use automation to make it painless.

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#upgrade-dependencies-early-fast-and-often)[Upgrade dependencies early, fast, and often](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#upgrade-dependencies-early-fast-and-often)

Same deal as releases. The smaller the change, the easier to manage.

Upgrading from 4.3.7 to 4.3.8 is no big deal. Going from 4.3.7 to 4.4.0 might require a few changes. You can run around and update.

But going from 4.3.7 to 4.9.0 is going to be a pain. Even if every intermediate version was backwards compatible, there's no telling how far it drifted.

When you jump multiple major versions, prepare for a world of pain. Last time I attempted a project like that, we abandoned after 2 weeks. Not worth it. 😅

But a small update? You can do that in an afternoon.

Keep up to date. It's counter-intuitive but it works. Straight from [The Theory of Constraints](https://swizec.com/blog/build-better-software-with-the-theory-of-constraints/)

## [](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#expert-makes-everyones-update)[Expert makes everyone's update](https://swizec.com/blog/what-i-learned-from-software-engineering-at-google/#expert-makes-everyones-update)

Hand-in-hand with keeping your code up to date is how.

You can upgrade a dependency or a piece of code or make a function deprecated. Then tell everyone _"Hey please upgrade"_.

They won't.

_You_ have to do it for them. It's the fastest way. Get into their code and make the update.

You're the expert on what needs to happen, you'll do it fast. Everyone else has to grok the change, find time in their schedule, ...

And if you have time, read the book. I liked it more than I thought.

Cheers,  
~Swizec

Published on July 15th, 2021 in [Learning](https://swizec.com/categories/learning/), [Books](https://swizec.com/categories/books/), [Software Engineering](https://swizec.com/categories/software%20engineering/), [SeniorMindset](https://swizec.com/categories/seniormindset/)

#### Did you enjoy this article?

#### Continue reading about What I learned from Software Engineering at Google

Semantically similar articles hand-picked by GPT-4

-   [The best engineering books get good 5 years into your career](https://swizec.com/blog/the-best-engineering-books-get-good-5-years-into-your-career/)
-   [Don't neglect your upgrades](https://swizec.com/blog/dont-neglect-your-upgrades/)
-   [25 lessons from 25 years of coding](https://swizec.com/blog/25-lessons-from-25-years-of-coding/)
-   [You can't stop the business, or why rewrites fail](https://swizec.com/blog/you-can-t-stop-the-business-or-why-rewrites-fail/)
-   [Can I get your opinion](https://swizec.com/blog/can-i-get-your-opinion/)

**Have a burning question that you think I can answer?** Hit me up on [twitter](https://twitter.com/swizec) and I'll do my best.

**Who am I and who do I help?** I'm Swizec Teller and I turn coders into engineers with _"Raw and honest from the heart!"_ writing. No bullshit. Real insights into the career and skills of a modern software engineer.

**Want to become a _true_ senior engineer?** Take ownership, have autonomy, and be a force multiplier on your team. The Senior Engineer Mindset ebook can help 👉 [swizec.com/senior-mindset](https://swizec.com/senior-mindset). These are the shifts in mindset that unlocked my career.

**Curious about Serverless and the modern backend?** Check out Serverless Handbook, for frontend engineers 👉 [ServerlessHandbook.dev](https://serverlesshandbook.dev/)

**Want to Stop copy pasting D3 examples and create data visualizations of your own?** Learn how to build scalable dataviz React components your whole team can understand with [React for Data Visualization](https://reactfordataviz.com/)

**Want to get my best emails on JavaScript, React, Serverless, Fullstack Web, or Indie Hacking?** Check out [swizec.com/collections](https://swizec.com/collections)

**Did someone amazing share this letter with you?** Wonderful! You can sign up for my weekly letters for software engineers on their path to greatness, here: [swizec.com/blog](https://swizec.com/blog)

**Want to brush up on your modern JavaScript syntax?** Check out my interactive cheatsheet: [es6cheatsheet.com](https://es6cheatsheet.com/)

**By the way, just in case no one has told you it yet today: I love and appreciate you for who you are ❤️**
