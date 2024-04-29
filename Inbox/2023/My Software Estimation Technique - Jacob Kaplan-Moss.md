---
created: 2023-11-17T08:10:47 (UTC -03:00)
tags: []
source: https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev
author: 
---

# My Software Estimation Technique - Jacob Kaplan-Moss

> ## Excerpt
> Last time, I explained that, although estimating software project timelines is hard, you should do it anyway. With that background, I want to go into some detail and share the technique I use when I need to develop a project timeline. I don’t believe there’s a single “correct” technique; this is one system that works well for me. However, my system does have one critical characteristic that I believe any effective estimation technique should have: it captures both time and uncertainty.

---
[Estimating Software Projects](https://jacobian.org/series/estimation/):

Last time, [I explained that, although estimating software project timelines is hard, you should do it anyway](https://jacobian.org/2021/may/20/estimation/). With that background, I want to go into some detail and share the technique I use when I need to develop a project timeline.

## The critical characteristic: capture time _and_ uncertainty[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#the-critical-characteristic-capture-time-_and_-uncertainty)

This isn’t an area where I think there’s a “correct” technique; this is one system that works well for me, and I’ve been able to teach others to use it successfully. There are any number of other systems that might work as well; [I’ve shared a few below](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#other-estimation-techniques).

However, my system does have one critical characteristic that I believe any effective estimation technique should have: it **captures both time and uncertainty**. An estimate that only includes time implies a high degree of certainty: if you tell me “that will take 10 days”, and we have a deadline 14 days out, I’ll assume we’re in great shape. If, on the other hand, we have that same deadline but you tell me “that will take 10-15 days”, now I know our deadline is at risk.

## My technique[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#my-technique)

My technique is this:

1.  [Break down work](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#1-break-down-the-work) into less-complex tasks
2.  [Estimate uncertainty](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#2-estimate-uncertainty)
3.  [Do the math](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#3-do-the-math) to get expected- and worst-case estimates
4.  [Refine](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#4-refine-if-necessary), if needed
5.  [Track your accuracy](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#5-track-your-accuracy-so-you-can-improve-over-time), so you can improve over time

For details, read on:

### 1\. Break down the work into less complex tasks[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#1-break-down-the-work-into-less-complex-tasks)

The first part of almost any estimation technique is to break work down into smaller tasks based on complexity. I use the following sizes:

| Complexity | Time |
| --- | --- |
| small | 1 day |
| medium | 3 days |
| large | 1 week (5 days) |
| extra-large | 2 weeks (10 days) |

To some degree these names and numbers are arbitrary (though, you should pick a scale and stick with it long-term so that you can [get better over time](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#5-track-your-accuracy-so-you-can-improve-over-time)). They map well to the way I think about work, but different time scales would work just fine. Story points instead of labels work too; the names don’t matter. It _is_ critical to use real wall-clock time, which means:

-   **You must map complexity to actual time units**. The whole point here is to eventually get to a calendar-time estimate (e.g. “4 - 6 weeks”), and you should do that mapping in as granular a way as possible.
    
-   **Use real wall-clock hours and days**, not idealized “programmer days” that assume engineers will write code 8 hours a day. That is, in the above system a `small` task is something that really will take about 4 hours, accounting for all the other things an engineer has to deal with during a normal workday.
    
-   **Try to capture realistic expected times**. Don’t be overly optimistic and go with “best-case”: if something probably will take 3 days, but _might_ take 1 if you get lucky, call it `medium`. On the other hand, don’t be overly pessimistic: don’t “upgrade” that `medium` to a `large` just to cover your butt. Aim for the expected time; we’ll [capture uncertainty](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#2-estimate-uncertainty) next.
    

The more granular you can get, the more accurate your ultimate estimate will be. If you’re doing this well, most broken-down tasks should be `small` or `medium`. You should have few `large`s, and probably no `extra-large` tasks. However, you don’t have to do this all in one fell swoop. Often I’ll do an initial rough estimate, which _will_ include any number of too-large tasks, and then [refine the estimate](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#4-refine-if-necessary) later.

### 2\. Estimate uncertainty[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#2-estimate-uncertainty)

If you stopped now, and simply added up all the task times, you’d have a reasonable estimate – but it wouldn’t capture your degree of certainty. And, as I’ve covered above, that’s critical: “20-30 days” is a very different estimate from “5-45 days”, even though both have the same mid-point estimate (25 days).

Thus, the next step is to capture that uncertainty. Initially, you might think of capturing a best-case and worst-case scenario, but I don’t find that too useful. Instead, I prefer to capture expected-case and worst-case. Coming in _early_ is never a problem, and in my experience, the best-case rarely happens. But it is important to capture how long something _could_ take if things go poorly. Therefore, my uncertainty system starts with the expected time (captured above), and then applies an “if-things-go-wrong” multiplier:

| Uncertainty Level | Multiplier |
| --- | --- |
| low | 1.1 |
| moderate | 1.5 |
| high | 2.0 |
| extreme | 5.0 |

The “multiplier” here is a scale factor I apply to the length to get a pessimistic estimate. That is, if I have a `medium`\-length task with `high` uncertainty, that means “I think this will take 3 days, but it could take up to 6 days” (`3 ⨉ 2.0`). Or, a `large` task with `low` uncertainty means “I expect this will take 5 days, but it might bleed over into the sixth day.”

Just like with the time estimates, the specific labels and multipliers are arbitrary. I find these multipliers to work well, but different ones are fine (again, though: [choose a system and stick with it](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#5-track-your-accuracy-so-you-can-improve-over-time)).

Also like the time estimates: you should be aiming to have low uncertainty. Too many `high` and `extreme` uncertainty estimates are a sign you might want to [iterate and refine](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#4-refine-if-necessary) your estimate.

### 3\. Do the math[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#3-do-the-math)

Now it’s just a matter of doing the math, given our complexity/uncertainty definitions. That’ll give you a broken-down estimate that looks like this:

| Task | Complexity | Uncertainty | Expected | Worst-case |
| --- | --- | --- | --- | --- |
| Refactor the doodad | small | low | 1 day | 1.1 days |
| Swizzle columns | large | moderate | 5 days | 7.5 days |
| Reticulate splines | medium | extreme | 5 days | 25 days |
| Reverse manifold intake | medium | medium | 3 days | 4.5 days |
| Deploy | small | low | 1 days | 1.1 days |
|  |  | **Total**: | **15 days** | **39 days** |

Thus, I might say this about this imaginary project: “I expect it’ll take about 3 weeks. Worst-case, it could take up to 8 weeks.” (Or, if I’m communicating with someone familiar with hearing my estimates, I might shorten this to simply “3-8 weeks”).

### 4\. Refine, if necessary[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#4-refine-if-necessary)

Is such a wide range acceptable? It might be: if you just need the timeline for basic expectation-setting, it might be OK. But if there are externalities – e.g. if some other project is waiting on this one shipping first, or if there’s a conference in six weeks and the marketing team needs to know what they can safely promote, etc – this wide range might be too broad.

Looking at the task list, it should be immediately obvious where the uncertainty comes from: the “reticulate splines” task, with its `extreme` uncertainty, accounts for nearly all of the variance. This is common: it’s often the case that you’ll be able to break down most of the project into low-uncertainty tasks, but will have a few highly uncertain tasks left over.

Also common is having a very long timeline that’s dominated by a few high-complexity (`extra-large`) tasks. If those tasks are also risky, that can make for an incredibly broad range. Even if they’re not uncertain, a project plan with too many huge tasks can make more granular timelines hard to produce (e.g. if you want to understand where you’ll be at the end of each month on a six-month project).

In these cases, you’ll need to refine the estimate by breaking down those `extra-large` tasks into smaller ones, and figuring out how to reduce the uncertainty on those `extreme` uncertainty tasks. Of course, if this was easy to do, you would have already done it: those big tasks stay big because breaking them down isn’t obvious; those risky projects stay risky because of unanswerable questions.

The trick here is to invest some time – not in completing the project, per se, but in scoping or de-risking. The tool I use here is **time-boxing**: pick an arbitrary amount of time (typically, 1-2 weeks) and use that time to re-scope or de-risk. Some ways you might spend that time:

-   **Research**: if this is a kind of task that others have done in the past, you might be able to develop a playbook or figure out the uncertainty through research. If, for example, you needed to migrate a service from AWS to Azure, that might start as a single high-complexity, high-risk task. But, this is a thing that others will have done in the past; by talking to a few other folks who’ve run similar migrations, you should be able to break it down further. If you’re at a smaller organization this might mean reaching out to people outside your company, but it’s entirely doable.
    
-   **[Spikes](https://en.wikipedia.org/wiki/Spike_(software_development))** are a concept from agile software development specifically designed for these sorts of cases. The idea is that you spend a bit of time exploring a potential solution, usually by diving in and writing some software. Sometimes the spike ends up becoming part of your project, but more often it’s throw-away code designed to just prove a point. Following the same AWS to Azure example from above: another way to de-risk that project might be to give yourself a week and attempt to migrate a much smaller, simpler service. At the end of that week, you can extrapolate from this spike to get a better estimate for the more complex migration.
    
-   **JFDI**: sometimes, the best approach is just Just F—ing Do It: give yourself or your team a week or two, and dive in. If we spend five days reticulating those splines, we might find at the end that we’re 50% done and highly confident that we just need another week. Boom: uncertainty gone.
    

There’s no “correct” tactic here. The key observation is that, if you’re dealing with a too-uncertain estimate, or one that needs to be broken down further, you can trade some time up-front for a better estimate. Taking a week or two to produce a better estimate can be a much better idea than embarking down an uncertain path with crossed fingers.

### 5\. Track your accuracy, so you can improve over time[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#5-track-your-accuracy-so-you-can-improve-over-time)

Finally, be sure you’re tracking _actual_ time, as the project goes on. This forms a feedback loop: you make a projection, observe how much it deviates from reality, and you can use that knowledge to refine and train your estimation skills over time.

For example, at the end of the project, I might end up with a table like this:

| Task | Complexity | Uncertainty | Expected | Worst-case | **Actual** |
| --- | --- | --- | --- | --- | --- |
| Refactor the doodad | small | low | 1 day | 1.1 days | **2 days** |
| Swizzle columns | large | moderate | 5 days | 7.5 days | **3 days** |
| Reticulate splines | medium | extreme | 5 days | 25 days | **10 days** |
| Reverse manifold intake | medium | medium | 3 days | 4.5 days | **3 days** |
| Deploy | low | low | 1 days | 1.1 days | **3 days** |

From this, I might learn that future deployments are probably actually `medium`\-complexity tasks, that that swizzling is actually `medium` as well, and perhaps that I can remove some uncertainty next time we need to reticulate some splines.

This is why, although the specific complexity and uncertainty sizes are fairly arbitrary, **it’s important to choose a model and stick with it**. If you re-define what `low` means, you’ll need to start over and recalibrate all future estimates.

## Other Estimation Techniques[](https://jacobian.org/2021/may/25/my-estimation-technique/?ref=dailydev#other-estimation-techniques)

As I said at the beginning: there isn’t a “right” system for estimating. The system I’ve presented is one I’ve used, with reasonable success, but there are others. So, I’ll wrap up with some pointers to other systems that may be worth investigating. All share what I consider to be the most important characteristic of capturing both time and uncertainty, but they do it in different ways:

-   [Program Evaluation and Review Technique](https://www.techrepublic.com/blog/it-consultant/use-pert-technique-for-more-accurate-estimates/): PERT is a system that’s very close to the one I’ve presented above. PERT asks you to make three estimates: pessimistic (`P`), optimistic (`O`), and most likely (`M`). Then, you calculate a PERT estimate with the formula `(O + 4M + P) / 6`. I’ve never tried PERT – as I wrote above, I don’t much see the point in best-case estimates – but it seems good.
    
-   [Evidence-based scheduling](https://www.joelonsoftware.com/2007/10/26/evidence-based-scheduling/) takes a radically different approach to estimation. Instead of starting with estimates, you start with work: dive in without an estimate at first, and track tasks/stories, their sizes, and how long they \_actually\_\_ take as you go. Over time, you can then apply the observed times from past stories to your remaining backlog, and an estimate naturally reveals itself.
    
    When I’ve been able to make it work, evidence-based scheduling is fantastic. It’s doesn’t require any up-front estimating and is highly accurate. However, I’ve struggled to make it work: it requires a stable team, relatively consistent work, and projects that are long-lived enough to provide sufficient data to make projections. Many of the teams I’ve been on have been more ad hoc, or the projects too variable or short-lived, so I haven’t been able to use this technique much.
    
-   [Fruit-salad scrum](https://fberriman.com/2020/01/22/fruit-salad-a-scrum-estimation-scale/) is a somewhat silly alternative to story points or t-shirt sizes. But if you look past the jokes, it captures something that neither points nor shirt sizes do: it also captures uncertainty! It does this with a single axis: complexity and uncertainty both increase as the fruit gets bigger (i.e. the 🍉 watermelon is a task that’s large _and_ uncertain). This is a shortcut, but one that frequently works. All you have to do to get time estimates out of fruit salad is map each fruit to a complexity/uncertainty pair, and then do the math as above. I’ve done this; it’s pretty accurate. And don’t underestimate the value of injecting a little bit of levity into something tedious like estimating.
    

___

Thanks for sticking with me; I hope you find this system useful. If you use it for your next estimate, please let me know how it goes!
