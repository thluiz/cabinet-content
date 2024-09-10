---
created: 2024-09-10T11:28:25 (UTC -03:00)
tags: []
source: https://www.softwaretestingnews.co.uk/software-is-eroding-before-our-eyes/?ref=dailydev
author: Vaishnavi nashte
---

# Software is eroding before our eyes - Software Testing News

> ## Excerpt
> In collaboration with the National DevOps Conference & Awards, this article discusses the erosion of software development.

---
**Software is eroding before our eyes**

**_Author: Juan Rodriguez, director of product management at Qt Group_**

Rocks erode. Mountains erode. That’s just how nature works. But there’s another thing eroding worldwide: software. Each day, developers find themselves staring more and more at an increasingly gargantuan mess of tangled software yarn in an architecture no one understands. And there’s barely the time to untangle it before someone says, “now add AI.”

Software seems to break a lot lately. The Crowdstrike outage might be the most high-profile recent one, but hardly the first. Other outages this year have stopped burgers from being served, stranded passengers at Heathrow Airport, and delayed fresh food at the Brexit Border.

And yet, developers sink disproportionate time into keeping this unsteady house of cards upright. The average developer work week is 41.1 hours, a third of which is dumped on addressing technical debt; over **40%** is spent on maintenance.

That’s a lot of time not innovating. We’re seeing the slow, creeping decay in the fabric of our software, as our tech industry asks developers to lay train tracks ahead while the tracks behind them disintegrate. The ‘fix it later’ mentality almost looks excusable in today’s extremely competitive markets.

**What is software erosion?**

Most of us don’t notice software erosion. It’s an invisible degradation of software’s inner structure. Eventually, it renders the readability, maintainability, extendibility, and reusability of software difficult, or impossible. It can even compromise the functional safety of a system.

As for what causes software erosion, well, software development is an additive process. New dependencies get introduced all the time between different parts of software. But sometimes, new code is unnecessary, spiralling the codebase into something increasingly unwieldy, more challenging to navigate. The larger the codebase, the trickier it is to understand, modify, and maintain. We don’t call it [‘dependency hell’](https://en.wikipedia.org/wiki/Dependency_hell) for nothing. It’s an exercise in frustration sussing out which changes are needed when implementing features or bug fixes.

Additions to the codebase are usually well-intentioned – perhaps a quality-of-life update to the product. Often, though, it’s developers creating shortcuts or workaround ‘hacks’ because they want to – or need to – speed up their workflow. This has a cost.

**The snowball effect of software erosion**

The consequence of added features and shortcuts is complexity, fragmenting and ‘eroding’ software architecture just a little more each time.

It’s a self-fulfilling prophecy, too. The developer adds the shortcut to their workflow. The codebase is now slightly more unwieldy than before. Want a new feature? You might break things. Rework one aspect of the product and it might impact teams in other silos through a destabilising chain reaction.

Understandably, the developer might grow frustrated from extra maintenance time spent, so…they add _another_ shortcut. And on and on it goes until their codebase resembles a high-stakes game of really unstable Jenga. Nobody wants to be _that_ person who finally collapses the tower.

That’s software erosion: added complexity everywhere that makes shipping even the simplest new feature a pain. In the long term, this hurts efficiency and scalability.

**Did we forget to shift left?**

Many companies have a disappointing ‘fix’ to this. They add time to fix the bugs or hire more QA professionals to alleviate the burden on developers. All this achieves is a game of whack-a-mole and new bugs that didn’t exist before the fixes. It’s a costly band-aid over a gunshot wound.

More prudent would be re-architecting the codebase. Some web-based firms have done this, rebuilding software from scratch. That’s easier done with two years’ worth of code behind you, but two decades’ worth of legacy code? Even if they accomplish this, the cycle of software erosion starts to repeat if none of the lessons were truly absorbed in the first place.

Judging from how long developers spend on maintenance, those lessons haven’t been digested. Software will continue to erode – you can bet your AI code assistant it will. That is unless every industry disciplines itself to tightly integrate QA into development from day one.

Start from design, not after all the code’s been written. Run static code analysis and functional testing as new code is written. Maybe you already do all those things and it’s not quite working. If that’s the case, go back to the source. Look at your architecture at a high level, not at the nitty-gritty-detailed level. Does your architecture do what you want it to? What would be the first component you define in your product? How do your components communicate with each other?

When you run static code analysis and understand where you’re cloning code; when you run your architecture and understand where your dependencies are; when run your functional tests to obtain results, that’s when you start to understand where the issues are. It’s not about picking one or the other. All software products should eventually be able to gain insights from a multitude of sources. For a startup, that might not be possible right away, but that’s the goal. Only _then_ can you go back to the drawing board and re-architect to avoid the same mistakes.

It sadly feels like few know what their implemented architecture truly is. But if you understand your architecture, then any feature you add, you can build according to the plan of what your software architecture was meant to be. And maybe then you don’t need the workarounds. Everyone gets served their burger.

___

**Upcoming events and contact information**

Register for [The National DevOps Conference and Awards](https://www.devopsonline.co.uk/national-devops-conference/) taking place on the **22nd and 23rd of October 2024** in London.

For sponsorship enquiries, please contact [calum.budge@31media.co.uk](mailto:calum.budge@31media.co.uk)

Foe media enquiries, please contact [vaishnavi.nashte@31media.co.uk](mailto:vaishnavi.nashte@31media.co.uk)

![](https://www.devopsonline.co.uk/wp-content/uploads/2024/07/XX-161_NDC-DA_e-sig.jpg)
