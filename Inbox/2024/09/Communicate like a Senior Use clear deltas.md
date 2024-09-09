---
created: 2024-08-11T22:49:05 (UTC -03:00)
tags: []
source: https://read.highgrowthengineer.com/p/communicate-like-a-senior-use-clear?ref=dailydev
author: Jordan Cutler
---

# Communicate like a Senior: Use clear deltas

> ## Excerpt
> Level up your performance reviews and influence. Get clear expectations to the next level.

---
_Hey everyone_ 👋_, Jordan here. **This week, we hit 60,000 subscribers!**_

_**Thank you** so much for helping to make this happen. In true High Growth Engineer fashion, I have a little celebration video at the end (not axe throwing this time)_ 😄_._

I’m experimenting with a new series: **Communicate like a Senior.**

Each article will feature 1 essential tip for communicating more like a Senior+ engineer. Each tip will highlight 3+ use cases where you can apply it immediately.

Let’s get into it!

You’re likely familiar with the term “code smell”**—**like a 4-level deep if-statement.

But I’m inventing a new term called **“communication smell.”** A communication smell is the same idea, except it’s when you notice a bad pattern in your communication.

One communication smell I see looks like this:

> ❌ We can **improve** the load time by caching images on a CDN.

“Improve” is vague. Would it improve by .0001 seconds, 1 second, 3 seconds?

Sure, it will improve the load time. That’s great! But what about the 20 other things that improve the load time? Why should we do this over those?

Here’s an improved version:

> ✅ I think we can cut the load time by **25%** if we cache images on a CDN. That will take it from 3 seconds to 2.25 seconds.

The difference is communicating in **deltas**. There is a clear **before** and **after**:

-   **Before:** 3 seconds
    
-   **After:** 2.25 seconds
    
-   **Improvement:** 25%
    

[

![Bar with 3 seconds, bar with 2.25 seconds, and a line connecting them showing 25% improvement](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F50a351bc-5189-4b11-984f-1e394091fece_1056x938.png "Bar with 3 seconds, bar with 2.25 seconds, and a line connecting them showing 25% improvement")

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F50a351bc-5189-4b11-984f-1e394091fece_1056x938.png)

Before and after visualization

Communicating in deltas makes your point 5x more convincing (see, notice the 5x 😄).

It doesn’t only apply to prioritization though. In this article, I’ll show you 3 of the best places to use this strategy to grow your career.

## Use case 1: Performance reviews

The most common mistake I see in performance reviews is writing unclear impact.

Here’s an example:

> **❌ Impact:** Reduced home page load time by adding caching

It’s an okay start. It’s focused on the user outcome, not the developer output.

But there’s a key problem: **Your performance review isn’t only seen by your manager. It’s also seen by their peers who have little context on the work you did.**

You “reduced” page load time, but by how much?

Let’s quantify it:

> **🟡 Impact:** Reduced home page load time by 2 seconds through additional caching

It’s better, but it’s unclear **how valuable 2 seconds is.**

Let’s clarify by sharing the **before-and-after (delta) and tying it to team goals.**

> **✅ Impact:** Cut home page load time by 66%, from 3 seconds to 1 second through additional caching. This resulted in a 30% increase in engagement, accounting for more than half of our quarter OKR target.

Now, imagine you are a senior leader hearing about this work for the first time and reviewing this person’s performance. With the new version, you know the work done, what it looked like before, what it looks like now, and how good that is because it’s tied to the team goals.

Below, you can see the senior leaders who love this approach:

John calls out that the final version answers, **“So what?”** We know why it matters.

One more bonus example: Yangshun Tay, CEO of GreatFrontend and ex-staff Engineer at Meta, shares how this same principle can be applied to resumes.

Resumes are often an even more concise version of performance reviews.

Share the delta. Don’t be vague!

## Use case 2: Decision buy-in

Imagine you need to convince a team to adopt your new fancy framework.

**How will you frame the conversation?**

Would you say:

> **(A)** Hey team, we’re working on helping teams adopt a new framework to improve the build health in CI. Can we work with you to get your team migrated?

Or

> **(B)** Hey team, we’re working on helping teams adopt a new framework to improve the build health in CI. Right now, when developers on your team push up a PR to GitHub, it passes only 70% of the time because of build health problems. By adopting the new framework, we’d expect it to pass 99% of the time. This will give developers 1 hour per day in time back.

The first one, (A), is likely to get the answer, “I appreciate it, but we’re pretty slammed right now, so maybe next quarter?”

The second one, (B), will likely get the team jumping out of their seat asking how quickly they can migrate.

**So what’s the difference?**
