---
created: 2024-09-09T17:41:20 (UTC -03:00)
tags: []
source: https://thenewstack.io/how-sprinting-slows-you-down-a-better-way-to-build-software/?ref=dailydev
author: AJ Shankar
---

# How Sprinting Slows You Down: A Better Way to Build Software - The New Stack

> ## Excerpt
> Sprints promise to accelerate development, but often do the opposite. Check out this alternative approach to building software.

---
Software sprints have become an article of faith in the technology world. In competitive industries that are driven by software, companies feel immense pressure to ship new products and features before their competitors. Aggressive two-week deadlines are common, with a small army of developers dividing up work production-line style and racing toward an immutable launch date.

But what if this isn’t the best way to build products?

I’ve run a software company for the past 12 years and before that I studied software engineering and received a Computer Science Ph.D. in Programming Systems. While there are nuggets of wisdom in sprints, scrums, and the whole panoply of modern software methodologies, they’re not how we at Everlaw build software today.

Sprints promise to accelerate development, but often do the opposite. Moreover, the relentless pace and unsatisfying nature of sprints has left developers burned out and quitting. Despite the uncertain economy, [less than half](https://www.enterprisedb.com/blog/how-build-employee-satisfaction-edb-open-source-talent-survey-2022) of developers say they’re very satisfied with their current jobs.

The approach we use empowers engineers to build the right functionality in the right way, without imposing deadlines. It favors leaner teams that are given autonomy to design entire features, not just a component that they build in isolation. And, critically, it involves exercising discipline with customers to not promise delivery dates for new software. The end result, perhaps surprisingly, is a development process that yields higher throughput: more features shipped per unit time.

I encourage you to consider this approach to development rather than defaulting to industry norms that lead to high levels of technical debt and developer turnover.

## Sprinting Slows You Down

Deadlines have a different impact on software development than they do on other disciplines. Sprints can have significant negative long-term consequences, encouraging developers to commit substandard, myopic code to stay on schedule. This leads to lower code quality, poor engineering decisions that are costly to fix later, and frustrated engineers.

TRENDING STORIES

1.  [Agile Reinvented: A Look Into the Future](https://thenewstack.io/agile-reinvented-a-look-into-the-future/)
2.  [The Anatomy of Slow Code Reviews](https://thenewstack.io/the-anatomy-of-slow-code-reviews/)
3.  [Introduction to Omakub, a Curated Ubuntu Environment by DHH](https://thenewstack.io/introduction-to-omakub-a-curated-ubuntu-environment-by-dhh/)
4.  [3 Lessons in Accessible Development From an Expert Tester](https://thenewstack.io/3-lessons-in-accessible-development-from-an-expert-tester/)
5.  [Devs Need System Design Tools, Not Diagramming Tools](https://thenewstack.io/devs-need-system-design-tools-not-diagramming-tools/)

So while at any given moment it may seem that throwing developers into sprints is the fastest way to get things done, in reality sprinting leads to less new software being developed over time as the negative effects of sprints compound.

It sounds intuitive that if you’re sprinting you must be moving as fast as possible, but in the world of software development, you rarely end up sprinting in a straight line. Often, you’re zig-zagging from one high-priority issue to the next, addressing issues and defects that were introduced in the past due to the pressure of deadlines.

If you’d jogged in a straight line instead, you would very likely have covered more ground — and be less exhausted to boot.

### Software Deadlines Favor Speed Over Quality

Many leaders think about software in the same way they do about other parts of the business, with an operational cadence driven by deadlines. Just as a marketing team is required to produce its holiday ad campaign by Oct. 31, or a finance team to close its books at the end of the month, so engineering teams are ordered to release a new product or feature by a set date.

This thinking overlooks the fact that developing software is fundamentally different from other business practices in a way that makes it ill-suited to deadlines. Here are three significant reasons:

-   Work is typically done in the context of a large, complex codebase
-   Correctness in software is a much higher standard than in other business areas
-   The downstream effects of good or bad decisions are massively compounding

Let’s break these down.

### Codebases Are Complex Beasts

First, it’s hard to anticipate the time it will take to implement new functionality, even simple functionality, because it’s rarely done in isolation. Instead, new features are typically added to an existing codebase, and its subtleties and nuances (such as issues around security, performance, and maintainability) are often discovered only while writing the code.

And because code is so interdependent, adding new functionality to good implementations often requires rewriting other code on which the new functionality will depend. This creates tension with a preset deadline: as an engineer dives into the code, when a deadline looms, they often must take shortcuts — “spaghetti code,” brittle implementations, and so on — even if they know the right way to implement something, simply because the deadline did not anticipate, and cannot accommodate, these challenges.

### A High Standard for Correctness

Second, what it means to be “correct” in software is unlike what it means for a marketing plan or a sales strategy. You can write a good enough press release by a fixed deadline and if it’s serviceable and factual — even if imperfect — it can be shipped. However, in code, any deviation from ideal is considered a defect and must be triaged. It can either be ignored (resulting in customer ire) or fixed at great cost in production — typically several multiples greater than fixing before release. Deadlines push more bugs into production, leading to more engineering time spent on fixing these costly production bugs, and less time on building new functionality for customers.

### Bad Decisions Compound

Third, the downstream impacts of poor implementations and architectural decisions continuously compound. A marketing pro can write a bad press release today, but then a good one tomorrow. But new code is dependent on old code: how it functions, how it’s structured and what paradigms it uses. Even code without obvious defects can be deficient in these ways. Hasty decisions about how to architect an application now can make it much harder to add new functionality later on, slowing down development over time. This “technical debt” is the scourge of engineering departments. Sprints may allow you to release a particular feature more quickly today, but at the expense of compromising future development.

### More People, More Problems

When you’re trying to ship a critical new feature by a certain deadline, it’s common to assign lots of engineers to it. However, large engineering teams suffer from two challenges: significant communication overhead and siloed implementations. These challenges reduce efficiency and quality of work.

When each developer has a narrow slice of a larger implementation, they’re forced to spend time coordinating with each other, and even then structural issues make it difficult to deliver high-quality implementations. Consider a frontend engineer whose job would be made much easier if the backend team would change the API to expose a critical piece of information to the front end. The amount of communication and reprioritization needed to effect changes like this is often so great that most engineers simply don’t bother.

Even more, the best implementations aren’t just locally optimal for every slice, they’re globally optimal for the whole feature, with a coherent implementation across the stack. These implementations are virtually impossible to achieve in big-team systems, since no one — save for an omniscient architect who can foresee every issue — has the authority to make all the necessary changes without superhuman levels of communication.

## Kill the Deadlines, Shrink the Teams

How might we build a system that delivers high-quality code with high throughput and happy engineers?

First, start by killing the deadlines. In our model, engineers determine when a feature is ready to ship. They are thus able to make principled engineering decisions about what to implement now versus later, delivering better code than they would when making decisions driven by a two-week deadline.

Second, assign smaller teams to features and give them greater scope. Because the teams are smaller (often just one engineer!), many new features are developed in parallel. These solo programmers or small teams own the entirety of implementation from back to front. There are no daily standups and needless communication is eliminated. And because the engineers control the implementation across the stack, they can make principled engineering decisions about how to build their functionality, rather than decisions constrained by the sliver of the codebase they happen to own, delivering a more cohesive implementation.

The common thread between these two ideas is that they institutionally support making principled decisions, because good decisions today lead to better outcomes tomorrow. This fundamental truth is easily overlooked when engineering is treated like any other department that does not have the context, correctness requirements, or compounding downstream impacts of writing code.

The counterintuitive result is that even though individual projects take longer to ship (no deadlines, smaller teams), total productivity is higher: we have many more projects in flight and being completed, and each engineer is able to get more done, because they’re spending less time dealing with critical production bugs and technical debt, and more time writing code in a high-quality codebase.

Engineers are happier, too. They don’t have to put out fires that they lit themselves due to an earlier deadline. They collaborate more freely, because they don’t have to weigh helping a colleague out against the deadline hanging over their head. And because they’re not siloed into a particular sliver of the codebase, they’re constantly learning new concepts and tackling novel challenges. The downstream benefits are a stronger culture — and better engineers.

### Keeping Your Customers Happy

You might ask: how do we get products out the door in a timely manner with no deadlines? First, it turns out that engineers like shipping code, and because they own entire features, they are highly motivated to see their projects come to fruition.

Second, our engineers do not make decisions in a vacuum. They understand that delivering software is our primary market advantage in a competitive industry, that our users rely on new functionality from Everlaw to remain competitive themselves, and that we reward strong performers like any other business does.

Third, there is a lot of support and transparency; leaders check with teams weekly on the progress of their software to address any roadblocks. We’re not running a free-for-all where everyone gets to follow their own path. We just don’t set arbitrary deadlines that compromise the quality (and the enjoyment) of the work.

Another common question is how we make commitments to deliver specific features to customers by a specific date, and the answer is that we do not. Our customers are happy because we produce high-quality software and deliver new features [more frequently than our competitors](https://support.everlaw.com/hc/en-us/sections/201109835-Release-Notes). Remember: the entire point of our approach is that we release more software over time.

We do, however, share our product roadmap with customers so that they may influence its direction and priorities. But if a prospect says, “we need this feature delivered by October or you won’t win our business,” we will decline, because doing so will compromise the quality of our products and do a disservice to other customers. A surprising amount of the time, these prospects understand the value we offer and sign on with Everlaw anyway. And of the ones that don’t, we know they’ll be back because we’ll be even further ahead in a year or two.

This model requires discipline at the leadership level, and an understanding from the go-to-market side of the house that as a result of this policy, while we might lose three deals this year to this policy, we’re going to win 30 more next year on the back of a cutting-edge product.

## The Long-Term Approach Wins the Race

Our model focuses on long-term outcomes rather than short-term ones, and thus is not the right fit for every business. For instance, very early-stage startups that are still identifying product-market fit need to experiment quickly to figure out what the market wants. They may want to test hypotheses rapidly, and so favor speed of development on a small number of features over throughput across many features.

As soon as they establish product-market fit, however, their engineering goals change from predominantly experimentation to predominantly execution. They’ll know what they need to build to win in their industry, and now they simply have to get there as fast as possible. Our model is best for them.

The golden trifecta of software development is to have a team of happy, motivated engineers who write high-quality code and ship lots of functionality. Engineers love working in our system because they have ownership over what they do. They do not have to compromise technically sound implementations to hit arbitrary deadlines; they are always learning new skills and domains rather than being siloed in a narrow part of the codebase; they are not constantly putting out fires in production code; and they ship meaningful, cohesive solutions with high frequency. The emphasis on high-quality code drives development throughput, which ultimately benefits the whole business and the users we serve.
