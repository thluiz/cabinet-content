---
created: 2023-07-17T13:34:58 (UTC -03:00)
tags: []
source: https://jeremymikkola.com/posts/developer_productivity.html?utm_source=tldrnewsletter
author: 
---

# Jeremy Mikkola - What makes developers productive?

> ## Excerpt
> Posted on July 15, 2023

---
Posted on July 15, 2023

Many ingredients go into developer productivity. Some are obvious and easy to measure (build times are an easy example to point to). But I think we tend to miss other important factors, perhaps because they are hard to directly measure. For example, no system collects a number that shows how well each developer understands the system they work on. It’s impossible to directly measure the things that happen inside a developer’s head. If you only measure the output (their productivity), that data won’t tell you what is missing (e.g. documentation).

It might be hard to find a clear metric for some of the factors below, but it is still worthwhile to ensure that you don’t have problems in these areas that hurt productivity. Some of these factors overlap, but they are different enough mental models that it is useful to think about them separately.

**Knowing what to build**. Building the wrong thing very quickly isn’t especially productive. It’s important to know what the customer needs, what other teams will accept (How many indexes can a database table have? Would a feature share information we can’t legally share?), and what has been tried before and didn’t work.

Sometimes you build things acknowledging that they might be the wrong thing to build, but you build it anyway for the sake of what you’ll learn. You might learn from a prototype why an approach doesn’t work or from an MVP what a customer actually needs. Still, you need to have a sense for what ideas are worth trying.

**Doing fewer things**. Being able to get a task done quickly is good, not having to do it at all is better. A company’s processes can add “busywork” that you’d be more productive without. Sometimes there are easy ways to tweak a process so that it delivers the same value with much less work.

There can also be a certain amount of effort needed to “keep the lights on” (KTLO). This is work that constantly needs to be done (e.g. answering support tickets) but doesn’t move projects forward. This work can look productive by many metrics (tickets closed, commits merged) without leaving the company in a better place.

**Tooling that reacts quickly**. Developers are constantly using tools: their editor highlights code and autocompletes method names, git commits the code, a build system runs tests. Every additional second of time these tools take has a high cost when multiplied by how often they run. Besides the raw cost in minutes and hours, slow tools also break a developer’s focus. Having your work stopped in its tracks by a slow tool is also frustrating, especially to someone who is stressed about timelines.

See also [The problem with slow tools](https://jeremymikkola.com/posts/2021_04_01_the_problem_with_slow_tools.html).

**Knowledge in the developer’s head**. Here’s one that’s hard to measure!

All else being equal, a developer with more relevant knowledge will be more productive. They won’t have to dig through code because they’ll already know how it works. They’ll know how to use the tools and what pitfalls to avoid. They’ll ask the right questions. 10x developers exist, and they’re the people who really know the codebase.

This means that a team shouldn’t own more things than they can collectively keep in their heads (ideally with a bus number greater than 1). It also means it is advantageous to minimize how often ownership changes hands - no one will ever know more about a thing than the person who created it. Ideally people new to a system can work alongside those already familiar with it and learn from them.

You also need clean boundaries between systems. A clean interface with simple semantics means you can think about the properties of that interface instead of having to know about the entire system behind it.

Documentation is a great way to transmit knowledge. It is especially important to have when developers need to accomplish a specific task that they aren’t familiar with. A lack of documentation can slow a developer down because they’ll have to research for themselves how to accomplish the task, they’ll make mistakes and have to redo their work, and they’ll have to wait for other teams to answer questions. This can easily turn a one hour task into a two day task. If 100 developers need to do this task, the cost of missing a page of documentation could be roughly the yearly salary of a developer.

This is also an argument for increased specialization. It is unproductive to require every developer to work on a broad range of things. An hour spent learning details of some security system or a capacity planning process is an hour not spent understanding the system the engineer works on or the domain in which they need to solve problems.

**Helpful infrastructure**. The infrastructure should be an aid rather than a hindrance. This means it needs to align reasonably closely to what needs to be done. Every piece of infrastructure is designed with some use cases in mind, but the needs of a project might fall outside of those use cases. It’s frustrating to hear the statements “you must use our infrastructure” and “you can’t do that with our infrastructure.” You waste time either working around the infrastructure or sitting in meetings where you attempt to convince the owners of the infrastructure to meet your needs.

**Low tech debt**. The existing code will never be a perfect fit for what you are trying to do; the original author didn’t have a crystal ball to know what kind of change you’d need to make. But some pieces of code are much easier to change than others. The answer to “how do we do X” should never be “we have to rearchitect this entire thing”. When there’s more tech debt, smaller changes to the functionality require larger changes to the system. Lowering tech debt minimizes the surface area that (a) you need to understand and (b) you need to change.

Projects to pay off tech debt need to be finished. Abandoning or deprioritizing them in the middle can leave the system worse off than it started (see [Tech debt gets worse before it gets better](https://jeremymikkola.com/posts/2022_01_29_tech_debt_gets_worse_before_it_gets_beffer.html)).

**Low rates of failure**. If a tool fails to run, a flaky build fails, a deploy fails, or your change causes production errors and you have to revert, you’ve wasted time dealing with a failure. Lowering the probability of these failures increases productivity.

Besides the engineer who experienced it, failures tend to also waste the time of teams that own the failing system because they’ll be called upon to help diagnose and fix the failure.

**Productive practices are practical**. The best way to learn how to solve a particular problem might be to write a prototype. If the environment discourages prototyping, it could be discouraging the most productive approach. If your monitoring tool is painful to use, developers will make fewer dashboards, measure fewer things, and decisions will be less data-driven as a result. On the other hand, if you make it easier to split a huge change into smaller code reviews, that can make the code easier to review and safer to deploy.

**How much an engineer can focus**. Engineers operate on a maker schedule. They need to be able to focus. That focus can be stolen by meetings and interruptions. Interruptions include slow CLI commands, slow tests, and encountering a task you don’t know how to accomplish and now need to research.

Worrying about too many things in any given week also hurts the ability to focus. A looming deadline or an unanswered question from a manager takes up mental RAM even when you are trying to focus on something else.

**Finishing tasks**. Building 50% of a thing isn’t 50% as productive, it’s 0% as productive. There’s few things less productive than work that is thrown away.

There are cases where abandoning a project half way through is the right call. Fighting the Sunk Cost Fallacy is sometimes the right thing to do. But there shouldn’t be a consistent pattern of changing priorities before a project finishes. That can drop a team’s productivity to zero.

___

You can’t necessarily build a dashboard to measure all of these different factors, but I think any developer could tell you which of these is currently impacting their productivity. Fixing those problems could dramatically increase how much gets done. Some of the problems can be shockingly easy to fix: a few hours spent writing a page of documentation might save the company thousands of hours. When you need to cut trees quickly, start by sharpening the saw.
