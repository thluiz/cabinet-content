---
created: 2024-01-08T20:12:02 (UTC -03:00)
tags: [patterns,teamwork,software,coding,development,engineering,inclusive,community]
source: https://dev.to/ben/the-problem-with-temporary-solutions-dkh?ref=dailydev
author: Ben Halpern
---

# The problem with temporary solutions - DEV Community

> ## Excerpt
> Temporary solutions spread like viruses through pattern matching

---
In a way everything in software development are temporary solutions. It's silly to think that one implementation will live on forever. But some temporary solutions are more temporary than others. In an ideal world we have the time and energy to make everything perfect, but in reality it's always a tradeoff. Temporary solutions are a perfectly natural occurrence and if we didn't rely on them we'd never get anything done.

Temporary solutions often don't turn out so temporary. This is a problem, and it shouldn't be all that shocking. We move on and never find the time or energy to come back to it. Overcoming this usually involves reliable testing/monitoring to give people the confidence to make changes, along with the managerial will to accept the importance of "backtracking".

But that is not really the issue I want to talk about. The more difficult problem is the virus that temporary solutions create through pattern matching. In the absence of perfect communication and leadership, software developers rightly rely on pattern matching to contribute to a codebase. This is great a lot of the time. It is how people figure out how to be helpful on their own. Ideally developers will graduate to a greater fundamental understanding of the domain, but if that was a necessary precursor to contribution, we'd never get anything done.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--VzX23yQp--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_800/https://thepracticaldev.s3.amazonaws.com/i/usx0xodbfclf81ifnoq0.gif)

When a codebase is littered with temporary fixes and implementations, those become guidelines for future contributions and bad approaches spread like viruses. The original implementer typically knows what they see as ideal even if the current approach does not fit the standard. But it's easy to forgive contributors for pattern matching and allow these sorts of contributions with the feeling of "we can keep up this style for now, eventually we'll remedy it". This is a dangerous path to tread.

It is hard to escape our present mindset when contributing code, but we must think of our code direction as something not fully in our control. Assuming any reasonable amount of success, future developers will be all sorts of different people. Experience levels and personality types will vary, and the best we can do today is set ourselves up for success and hope for the best.
