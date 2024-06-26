---
created: 2024-06-26T14:03:18 (UTC -03:00)
tags: []
source: https://levelup.gitconnected.com/why-story-points-dont-make-sense-117fabdac8ca
author: Garrett James Cassar
---

# Why Story Points don’t make sense | by Garrett James Cassar | Level Up Coding

> ## Excerpt
> Throughout my career, I’ve harboured some heretical “unagile” and unsanctimonious ideas. But none seemingly, quite as heretical as this one. A lot of people read that and think what I meant to say is…

---
A previous team of mine used to point things for a range of 0.5 (for really tiny) to 3 (for really big).

When quizzing the team on how much developer time it takes to do each the results are usually pretty conclusive.

```
<span id="65ee" data-selectable-paragraph=""> <span>0.5</span> - <span>No</span> time at all, two hour tops.<br> <span>1</span> - <span>Four</span> hours to a day.<br> <span>2</span> - <span>Three</span> days maybe? <span>Four</span>?<br> <span>3</span> - <span>The</span> best part <span>of</span> a two week sprint <span>if</span> not more.</span>
```

Quiz your own team about this.

Plotting out these estimates looks like this.

![](https://miro.medium.com/v2/resize:fit:793/0*lh-rv1nrClG_3LUd)

X Axis — Developer Time in days, Y Axis — Complexity rating

This is not a linear graph, It’s an exponential one, and you can’t add up exponential numbers.

So the question ‘How many points can we burn this sprint?’ Doesn’t make sense.

**Example  
**Say we can burn 15 points a sprint. And we have 3 developers on the team.

If the 15 is made up of 0.5 point stories only.

```
<span id="eb14" data-selectable-paragraph=""><span>15</span> total points / <span>0.5</span> points per jira = <span>30</span> <span>Jiras</span><br><span>0.2</span> effort <span>in</span> days * <span>30</span> jiras = <span>6</span> developer days <span>of</span> work.<br><span>6</span> / <span>3</span> devs = <span>2</span> days</span>
```

We work out that we finish the entire sprints worth of work in _2 days._

At the other extreme, let’s say we have 15 points worth of work in 3 point stories only.

```
<span id="0fcb" data-selectable-paragraph=""><span>15</span> total points / <span>3</span> points <span>per</span> <span>jira</span> <span>=</span> <span>5</span> Jiras<br><span>10</span> effort in days * <span>5</span> jiras = <span>50</span> developer days of work.<br><span>50</span> / <span>3</span> devs = <span>16.66</span> days</span>
```

Finishing the entire sprints worth of work in _16.66 days_.

So, with our small team of 3 that’s an error margin of over **800%.**

Assuming all developers have exactly the same velocity and estimation is done perfectly. The question of ‘How many points can we burn’ in any period of time still doesn’t make any sense.

Worse, If you’re using story point velocity for promotions and bonuses, the play is obvious.

How do we fix this?

## **Use the higher end of the fibonacci sequence**

Story points are usually estimated on the fibonacci sequence, this is probably done deliberately in recognition of this phenomenon.

```
<span id="21e5" data-selectable-paragraph=""><span>0.5</span> <span>-&gt;</span> <span>3</span><br><span>1</span> <span>-&gt;</span> <span>5</span><br><span>2</span> <span>-&gt;</span> <span>8</span><br><span>3</span> <span>-&gt;</span> <span>13</span></span>
```

![](https://miro.medium.com/v2/resize:fit:839/0*VAcVbjaZ5OkD7lRm)

X Axis — Developer time in days, Y Axis — Complexity rating

This doesn’t really fix the problem, it dilutes it.

If you really wanted to use a mathematical concept to describe this discrepancy, the correct one to use would be logarithms, as they are the inverse of exponentials. But who’s got time for that?

## **_Don’t use numbers at all_**

I think that the much more straightforward and truer recognition of this phenomenon is not to use numbers at all.

```
<span id="fbf5" data-selectable-paragraph=""><span>0.5</span> <span>-&gt;</span> XS<br><span>1</span> <span>-&gt;</span> S<br><span>2</span> <span>-&gt;</span> M<br><span>3</span> <span>-&gt;</span> L</span>
```

By using t-shirt sizes instead of story points we dispel the notion that they add up at all.

We can calculate velocity. It’s a bit more complicated, but actually accurate.

By relating the size with an amount of time you would expect it to be completed by a mid level developer, you could get all of the sizes of the stories completed, multiply them by their factor and add them up.

**Example**:

```
<span id="b363" data-selectable-paragraph=""><span>XS</span> = <span>0.2</span> days<br> <span>S</span> = <span>0.5</span> days<br> <span>M</span> = <span>3</span> days<br> <span>L</span> = <span>10</span> days</span>
```

Last sprint we burnt:

```
<span id="19d2" data-selectable-paragraph=""><span>6</span> <span>XSs</span> = (<span>6</span> * <span>0.2</span>) = <span>1.2</span> days<br><span>4</span> <span>Ss</span> = (<span>4</span> * <span>0.5</span>) = <span>2</span> days<br><span>3</span> <span>Ms</span> = (<span>3</span> * <span>3</span>) = <span>9</span> days<br><span>1</span> L = (<span>1</span> * <span>10</span>) = <span>10</span> days</span>
```

That’s roughly 23.2 developer days of work. Leaving 6.8 developer days for code reviews, meetings and bunking off early to go to the pub. Spiffing.

**Conclusion  
**You should never attempt to calculate velocity by adding up story points. The maths just doesn’t work.

It can make your estimates wildly off the mark, encourage clever developers to game the system, and leave delivery managers either extremely disappointed or extremely happy depending on the sprint.

I find it’s better to leave them with a more consistent but less intense feeling of disappointment.

Please:

-   Press and hold the clap button
-   Follow me: [https://medium.com/@garrett-james-cassar](https://medium.com/@garrett-james-cassar)
-   Add me on LinkedIn: [https://www.linkedin.com/in/gary-cassar-55648b67/](https://www.linkedin.com/in/gary-cassar-55648b67/)

**Further Reading**
