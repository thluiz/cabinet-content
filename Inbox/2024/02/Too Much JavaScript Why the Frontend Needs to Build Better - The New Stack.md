---
created: 2023-06-26T14:59:19 (UTC -03:00)
tags: []
source: https://thenewstack.io/too-much-javascript-why-the-frontend-needs-to-build-better/
author: Loraine Lawson
---

# Too Much JavaScript? Why the Frontend Needs to Build Better - The New Stack

> ## Excerpt
> One company found that too much JavaScript costs them $700,000 per year, per kilobyte. Here's what Alex Russell says needs to change.

---
Alex Russell loaded a [filmstrip comparison showing the speed](https://www.webpagetest.org/video/compare.php?tests=230416_BiDc9H_682-r%3A1-c%3A0%2C230409_BiDcNS_7BX-r%3A1-c%3A0&thumbSize=200&ival=100&end=visual) it takes to load two websites. One site — coming from the UK — loads the site essentials within three seconds and it’s ready to go. It’s a UK site for welfare recipients and, potentially, it loaded from servers on the other side of the ocean. The other site is for [SNAP](https://www.fns.usda.gov/snap/supplemental-nutrition-assistance-program) participants in Massachusetts, so a similar audience in terms of what devices you might expect to access it. It takes 23 seconds to load the basics of the Massachusetts site: Long enough to frustrate the most patient user.

”What’s the difference?” Russell asked, finishing my question for me. “The difference is megabytes of JavaScript.”

In a series of recent messages on [BlueSky](https://thenewstack.io/bluesky-vs-nostr-which-should-developers-care-about-more/), Russell contended we’ve over-prioritized developer experience and the end user experience suffers as a result. The New Stack interviewed Russell to find out what that means.

![Message about dire status of frontend development](https://cdn.thenewstack.io/media/2023/06/da9afcc7-alexrussellonblusky.png)

via BlueSky screenshot

Russell has been monitoring and writing about the trend for some time on his blog [Infrequently Noted](https://infrequently.org/). He spent 12 and a half years on the Google Chrome team and now works as a partner product manager on Microsoft Edge. He also has consulted with companies in an effort to speed up their sites.

## A Wrong Turn on the Way to a World-Wide Web

Something wrong happened on the way to building a world-wide web: We’ve wound up at a place where many websites are designed more for the minority of super users than for the majority, Russell contended. And frankly, that [majority is accessing the internet](https://infrequently.org/2022/12/performance-baseline-2023/) on much slower devices, at much slower speeds, than most developers (and tech journalists).

“It’s based on a series of assertions about how the world is going to evolve that … have not been true for a decade,” Russell said. “So the idea that organizations can manage complexity that comes with some of these new stacks is fantastically wrong. It has been wrong for a long time, but it’s still wrong. And the idea that CPUs and networks get faster every year has been wrong for at least a decade for most people, and it’s still wrong.”

## Smartphones Become the Primary Device

When near-ubiquity did come, it came on the backs of smartphones — and not the $1000 iPhone or Androids. It’s coming on smartphones that at most cost a few hundred dollars and are often on rural networks.

“Smartphones are now where the web is consumed most. They are where most computing is consumed. They are not the special case,” Russell said. “The biggest story in computing is not what’s happening at the high end, although that’s been pretty dramatic. The biggest story is what’s happening in the middle and down because computing is spreading out.”

Smartphones are just now penetrating the general market and getting into the hands and homes of people who previously only had feature phones, Russell said.

“The primary story is at the low end where volumes have exploded. \[…\] The average selling price of a \[mobile\] device has barely budged over the last 10 years. It’s about 300 to 320 US dollars, new unlocked, worldwide,” he said.

He showed a [chart depicting cell phone price growth](https://infrequently.org/2022/12/performance-baseline-2023/single_core_scores.png) based on how powerful the device is (see image below). High-end phones now cost significantly more, but the growth in volume has been at the mid-pricing and low end of device costs.

![](https://cdn.thenewstack.io/media/2023/06/a6a80b9a-single_core_scores.png)

“That means that you have to be selling a larger multiple number of lower-end devices every year to keep the average the same — the story here is we’re moving so many new devices into the world, and, as a fraction, a receding percentage of them are what we would consider to be high end,” he said. “The higher-end device that most decision makers, developers, CEOs, purchasing managers, engineering managers — the devices that they carry have stopped representing the real world in a very direct way.”

## $700,000 a Year per KiloByte of JavaScript

Frontend developers need to realize their own devices don’t represent the average user, he said. Often, companies don’t even realize there’s a problem until the CEO’s uncle shows up with a lower-range device, trying to perform a function on the company website and finding it painfully slow, he said.

“If your own devices are now kind of, in essence, lying to you about the way in which your systems behave in the world, then you need to go spend some time and effort to actually re-mend the fence around your expectations and try to cabin things that way,” he told The New Stack. “But the frontend community — to my very great frustration — has not internalized this.”

Part of the issue is that software engineers aren’t on the hook for the profit and loss of a website in any meaningful way, he added. Part of it is also that engineers are a hot commodity in the job market, and so work sites have prioritized improving their experience with the latest, greatest device.

“The happiness of engineers is now the central part of the narrative inside of the developer community that I intersect with most often,” he said. “I can’t cite exactly when it happened, but I can tell you that the effects have been pretty disastrous for a bunch of the businesses that I’ve consulted with over the years.”

Often, it boils down to one common problem: Too much client-side [JavaScript](https://thenewstack.io/no-more-javascript-how-microsoft-blazor-uses-webassembly/). This is not a cost-free error. One retailer realized they were losing $700,000 a year per kilobyte of JavaScript, Russell said.

“You may be losing all of the users who don’t have those devices because the experience is so bad,” he said.

That doesn’t mean developers are wrong to ship client-side JavaScript, which is why Russell hates to be prescriptive about how to handle the problem. Sometimes, it makes sense depending on the data model and whether it has to live on the client so that you can access the next email (think Gmail) or paint the next operation quickly (think [Figma](https://thenewstack.io/figma-targets-developers-while-it-waits-for-adobe-deal-news/)). The usage tends to correlate with very long sessions, he said. But developers should realize, too, that some frameworks prioritize this approach.

“The premise of something like [React](https://thenewstack.io/learn-react-delete-functionality-and-the-set-state-hook/), the premise of something like [Angular](https://thenewstack.io/dev-news-angular-v16-next-js-updates-and-prep-for-deno-2-0/), is that \[the\] data model is local,” he said. “So if that premise doesn’t meet the use case, then those tools just fundamentally don’t make sense,” he said. “You really do have to shoehorn them in for some kind of an exogenous reason, then you hope that it plays out. And that’s why we see this discussion around something like islands or server components or something like that, because these are just fundamentally inappropriate choices for most classes of applications because they don’t feature long sessions with client-side data models.“

## Core Vitals Could Re-Prioritize User Experience

One thing that may help change the tide is the Core Vitals effort coming out of the Chrome team and search team at Google, he said. Annie Sullivan and her team at Google want to move [Interaction to Next Paint](https://web.dev/optimize-inp/#:~:text=in%20March%202024.-,Interaction%20to%20Next%20Paint%20(INP)%20is%20a%20pending%20Core%20Web,user's%20visit%20to%20a%20page.) (INP) into core vitals, which would be a “foundational change to people’s expectations” because it would attach SEO to the question of whether or not something feels good on the client side, he said.

“Most engineering involves trade-offs; there’s no single effect that rules everything. There are complex systems that have to be balanced,” Alex Russell said. “If we just say, oh, making the developer experience \[better\] will make everything better, but we never bother to even attempt to characterize or quantify it, let alone prove it beyond a doubt — Hitchens’ razor applies.” ([which means](https://en.wikipedia.org/wiki/Hitchens%27s_razor#:~:text=It%20states%20%22what%20can%20be,Hitchens%20(1949%E2%80%932011).), “what can be asserted without evidence can also be dismissed without evidence.”)

Group Created with Sketch.
