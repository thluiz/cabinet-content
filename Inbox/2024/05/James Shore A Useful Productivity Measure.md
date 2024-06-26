---
created: 2024-05-14T10:23:26 (UTC -03:00)
tags: []
source: https://www.jamesshore.com/v2/blog/2024/a-useful-productivity-measure?utm_source=tldrnewsletter
author: 
---

# James Shore: A Useful Productivity Measure?

> ## Excerpt
> In my new role as VP of Engineering, there was one question I was dreading more than any other: “How are you measuring productivity?”

---
In my [new role as VP of Engineering](https://www.jamesshore.com/v2/blog/2024/a-software-engineering-career-ladder), there was one question I was dreading more than any other: “How are you measuring productivity?”

I can’t fault the question. I mean, sure, I’d rather it be phrased about how I’m _improving_ productivity, rather than how I’m _measuring_ it, but fair enough. I need to be accountable for engineering productivity. There _are_ real problems in the org, I _do_ need to fix them, and I need to demonstrate that I’m doing so.

Just one little problem: software productivity is famously unmeasurable. Martin Fowler: “[Cannot Measure Productivity](https://martinfowler.com/bliki/CannotMeasureProductivity.html).” From 2003. _2003!_

More recently, Kent Beck and Gergely Orosz tackled the same question. Kent concluded: “Measure developer productivity? Not possible.”<sup>1</sup>

<sup>1</sup>Kent and Gergely’s two-part article is excellent and worth reading. [Part one.](https://tidyfirst.substack.com/p/measuring-developer-productivity) [Part two.](https://tidyfirst.substack.com/p/measuring-developer-productivity-440) And [a later followup](https://tidyfirst.substack.com/p/productivity-measurement-as-a-tradeoff).

So now what do I do? That’s it, I’m screwed, make up some bullshit metric and watch my soul die, McKinsey style? Fight with my CEO about his impossible request until he gives up and fires me?

Maybe not. I think I’ve found another way. It’s early, but it’s working for me so far. Will it work for you? Eeehhhhh... maybe. Probably not. But maybe.

### My Solution

It started half a year ago, in September 2023. My CEO asked me how I was measuring productivity. I told him it wasn’t possible. He told me I was wrong. I took offense. It got heated.

After things cooled off, he invited me to his house to talk things over in person. (We’re a fully remote company, in different parts of the country, so face time takes some arranging.) I knew I couldn’t just blow off the request, so I decided to approach the question from the standpoint of accountability. How could I demonstrate to my CEO that I was being accountable to the org?

We met at the CEO’s house, along with the CTO and CPO (Chief Product Officer). I led them through an exercise: “Imagine we’ve built the best product engineering organization in the world. What does that look like?” We came up with six categories of ideas. Then I asked, “Which indicators will help us understand how we’re getting closer to these ideals?” We came up with indicators in each category. Blissfully, none of those categories were “productivity.” I don’t think anyone noticed. Bullet dodged.

I came away feeling fairly positive about the conversation. I discussed the results with my Engineering and Product peers, we refined, and I finally presented the first “Product Engineering Accountability Review” a few weeks ago. It went well! I used the indicators to _support_ a qualitative discussion of what’s happening in Engineering, rather than just reporting numbers.

One problem: the CEO had a scheduling conflict and couldn’t come. So I don’t know what he would have thought. But at least the CTO and CPO liked it.

### The Productivity OKR

Meanwhile, back in January, the leadership team had established that one of our company-wide OKRs<sup>1</sup> would be to define and improve productivity metrics for each department. I was to present mine to the full Leadership team at the end of April. Crap. Bullet un-dodged.

<sup>1</sup>“OKRs” are “Objectives and Key Results.” They’re a way of setting and tracking goals. Similar to Management by Objectives, about which [Deming said](https://deming.org/explore/fourteen-points/): “Eliminate management by objective. Eliminate management by numbers, numerical goals. Substitute leadership.” But that’s a rant for another day.

In October, we had defined six aspects of being the greatest product engineering company in the world. One of them was “profitability.” Its indicators were the most related to outcomes. If I had to measure productivity—and I did—they were the ones to use.

We had three indicators for profitability: actual RoI, estimated RoI, and value-add capacity. The first was best, in theory. In practice, it might be impossible to measure. Before I explain why, I need to explain how we calculate RoI.

### Product Bets

Every engineering organization I’ve ever seen has had more demand than capacity. Prioritizing those demands is crucial. And fraught. Lots and lots of opportunity for conflict.

We’re no exception. To help bring order to the chaos, the VP of Product and I have introduced the idea of “Product Bets.” Each major initiative needs a Product Bet Proposal. It's a short, one-page document that explains:

-   What we’re going to accomplish
-   The value it’s estimated to generate
-   The amount we’re willing to bet
-   The justification for the value
-   How we’ll measure the value

In order for a proposal to be accepted, a member of the Leadership team needs to sponsor it, take accountability for its success, and convince the other Leadership members that their proposal is more important than all the others.

In theory, anyway. I’ve tried variants of this idea before, and it’s never lasted. Turns out leadership teams like accountability more when it’s _other_ people who have to be accountable.

But we’re trying. So far... it’s kind of working. Maybe. Too early to tell, honestly. I’ll write more after the verdict’s in.

### A True Measure of Productivity

But if the product bet process _does_ work... well. That would be cool. It would give us a true measure of productivity. We would know the value of a bet, and it’s easy to know how much we spend on a bet. Value produced over dollars spent. Boom. Productivity. Done.

Even better, the numbers are nice. Very nice. Each bet has a “maximum wager,” which determines how many engineer-days we’ll invest in the bet before giving up. Those wagers are based on one tenth of the expected value over five years. In other words, 10x return on investment.

10x return on investment is enough to make anybody take notice. But... measuring value is a problem. Sure, each bet has an section on how we’ll measure value, but can we really tease that out from sales team effort, customer success team effort, other feature changes, and changes in the market? Probably not.

It might not matter. We may not be able to measure the _actual_ value, but every bet also has an _estimated_ value attached. Combined with actual cost, that gives us a measure of estimated RoI. It may not be real RoI, but it’s good enough for understanding the productivity of the engineering team.

That’s our first two productivity measures: actual RoI and estimated RoI. Pretty good. Except that we don’t have any data.

### A Better Measure of Productivity... For Now

The RoI metrics rely on us having product bets. But we don’t. Not yet. We’re still rolling them out. So, no matter how good the metrics might be, we can’t use them. No data.

There’s a third indicator in the “profitability” category we _can_ use, though. It’s value-add capacity.

Like any engineering organization, we spend some percent of our time on fixing bugs, performing maintenance, and other things that are necessary but don’t add value from a customer or user perspective. The Japanese term for this is _muda._

If we didn’t have any muda, spent all our time on value-add work, and achieved a 10x return on each investment, our productivity would be ten: $10 for every $1 in salary (or close enough). If we spent 80% of our time on value-add work, our productivity would be eight. Twenty percent, two.

In other words, in the absence of RoI measures, the percent of engineering time spent on value-add activities is a pretty good proxy for productivity.

That’s the productivity number I reported to Leadership last week.

### How It Was Received

It worked really well. The nice thing about reporting this number was that people were _already_ frustrated with Engineering’s progress. They could see that we had capacity problems, but they didn’t know why. It was easy for them to assume that it was because people weren’t working hard, or didn’t know what they were doing.

I presented our metric as a single stacked bar chart. (Like a pie chart, but in a rectangle.) Muda on the bottom, value-add on the top. Then I expanded out the muda into another stacked bar chart, showing how much time was being spent across all of Engineering on deferred maintenance, bugs, on call, incident response, deployments, and so forth. Then expanded out again with more detail for the worst of those categories.

It completely changed the tenor of the conversation. Suddenly, the conversation shifted from, “how can we get the stuff we want sooner,” to “how can we decrease muda and spend more time on value-add work?” That’s exactly the conversation we need to be having.

Earlier in the week, the CEO told me that, next quarter, Leadership wants a briefing from me about how Engineering works. What my deliverables are, essentially. With the capacity measure, I have a good answer: my job is to double our value-add capacity over the next three years. Essentially, to double our output without increasing spending.

You know what? With my XP plans and the XP coaches I’ve hired, it’s totally doable. I think I’m being kind of conservative, actually.

### A Fatal Flaw

So that’s my productivity measure: value-add capacity. The percentage of engineering time we spend on adding value for users and customers. Can you use it? Eehhhhh... maybe.

I can use value-add capacity—so far—because I’m aggressively stubborn about honest data. I refuse to skew our numbers or do worse work to make my department look good, and I’m keeping a close eye on what my teams are doing, too. This is important, because value-add capacity has a fatal flaw:

> When a measure becomes a target, it ceases to be a good measure.
> 
> Goodhart’s Law

It’s ridiculously easy to cheat this metric. Even if you correctly categorize your muda—it’s very tempting to let edge cases slide—all you have to do is stop fixing bugs, defer some needed upgrades, ignore a security vulnerability... and poof! Happy numbers. At a horrible cost.

Actually, that’s the root of my org’s current capacity problems. They weren’t cheating a metric, but they _were_ under pressure to deliver as much as possible. So they deferred a bunch of maintenance and took some questionable engineering shortcuts. Now they’re paying the price.

Unfortunately, you can get away with cheating this metric for a long time. Years, really. It’s not like you cut quality one month and then the truth comes out the next month. This is a metric that only works when people are scrupulously honest, including with themselves.

So, yeah, I’m not sure if this will work for you. It depends on how much ability you have to police things. The RoI indicators might not work for you either. They require product bets, or something similar, and that requires a lot of org changes. Even if they do work for me—jury’s out on that—they’re not something you can introduce overnight.

But, so far, value-add capacity _is_ working for me, and I thought that might be interesting to you. Maybe spark some ideas. Just be cautious. Goodhart’s Law is a vengeful bastard. Remember, all this productivity metric stuff is a sideshow to what really matters:

Deliver valuable software. Do it often. And write it well.

Good luck.
