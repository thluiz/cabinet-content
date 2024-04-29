---
created: 2023-10-30T09:02:21 (UTC -03:00)
tags: []
source: https://refactoring.fm/p/how-to-design-a-good-tech-strategy?utm_source=substack&utm_medium=email
author: Luca Rossi
---

# (3) How to Design a Good Technical Strategy 👁️ - by Luca Rossi

> ## Excerpt
> And how your job changes as a technical leader.

---
_Hey 👋 this is [Luca](https://twitter.com/lucaronin)! Welcome to a 🔒 **weekly edition** 🔒 of Refactoring._

_Every week I write advice on how to become a better engineering leader, backed by my own experience, research and case studies._

_You can learn more [about Refactoring here](http://refactoring.fm/about)._

_To receive all the full articles and support Refactoring, consider subscribing 👇_

[Become a better tech leader ✨](https://refactoring.substack.com/subscribe?)

A few months ago I wrote an article about the different [role of tech leads and engineering managers](https://refactoring.fm/p/the-role-of-tech-leads-and-engineering). After that, several people wrote me to tell their experience, and asked for advice about their responsibilities and career path.

I find it very hard to give good advice about leadership roles in tech. The same role can be very different in different companies — and also in different teams within the same company.

In software, I feel we are still figuring out what the various skills and responsibilities really are, and we keep adjusting roles and tracks accordingly.

For instance, there is now a healthy conversation about separating _management_ from _individual contribution_ career paths. However, even within the IC track, you eventually grow into _technical leadership_ roles, which mostly require a different set of skills.

So much that [Pat Kua](https://twitter.com/patkua) argues that the dual model for engineering careers should actually be a [trident](https://www.thekua.com/atwork/2019/02/the-trident-model-of-career-development/) model, where you have three routes:

-   🔨 **Individual Contributor** — 80% time focused on execution.
    
-   💼 **Management** — 80% time focused on managing people and processes.
    
-   👑 **Technical Leadership** — 80% time focused on leading people on technical topics.
    

As all careers start with pure IC, you can either transition into an Engineering Manager, or a Tech Lead (or Lead Developer, Principal Engineer, etc.).

While the move to management is now relatively well-documented, what is it that really separates pure ICs from Technical Leaders? 👇

As a Tech Leader of various kinds, your duties can be divided in two big categories:

1.  Working with your **engineering team**
    
2.  Working with **non-technical stakeholders**
    

The first is more of an evolution of your execution skills — you get to make design decisions, guide development and review other engineers' work.

The second is a different beast — you will discuss and negotiate technical requirements, risks, and the overall technical direction of your team/product, with stakeholders coming from other functions.

In this context, your focus shifts from **how** things should be built to **what** should be built and **why**_._ That is, you move from technical execution to technical strategy.

But what is a technical strategy and how can you design one?

The technical strategy of your company is the plan of how engineering enables the product and company respective strategies, and drives them forward.

In the best companies, this doesn't happen strictly top-down, but more as a **conversation**.

Product and engineering are **mutually influenced** — many of the most successful products of all times where in fact enabled by engineering breakthroughs.

-   📱 Famously, **Steve Jobs** was shown a prototype of a multi-touch screen and _then_ decided they could build a phone with it.
    
-   🎧 **Spotify** decided to go all-in on mobile because it was the right choice business-wise _and_ it was finally technically viable (encoding + buffering tech + mobile networks performance).
    

Think at engineering as a **mini-company** within you company. As your mini-company, it requires a vision, a mission, and a plan to make it happen.

A technical strategy that is successful at this can do wonders to all aspects of your work:

-   🔋 **Empower product and business** — it enables new opportunities and creates competitive advantage over time.
    
-   💬 **Establish clear communication** — planning and negotiation become easier when they start from an agreed, high-level direction.
    
-   📣 **Engage Engineers** — it provides a clear purpose that rallies people and motivates them.
    

The [Reforge](https://reforge.com/) guys wrote [a fantastic article](https://www.reforge.com/blog/overcome-the-tech-strategy-spiral) about the same topic, arguing there are two levels of strategy:

-   **Company Strategy** — the logical plan you design to bring your company’s mission into being.
    
-   **Functional Strategy** — the logical plan for how a specific function (product, engineering, marketing, etc.) will drive its part of the company strategy.
    

Functional strategies are influenced by one another about what can be done. They all contribute to the overall company strategy, and eventually to the roadmap.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fb17766cd-a5e6-4af7-a142-2dd4a05c2500_2070x1384.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fb17766cd-a5e6-4af7-a142-2dd4a05c2500_2070x1384.png)

When it comes to Engineering, you can design your strategy by reflecting on three elements:

1.  🏢 **Your Company**
    
2.  📱 **Your Product**
    
3.  👕 **Your Team**
    

What can engineering do to support your company mission? What should you invest in to create a core asset for your company, generating a hard-to-copy, **competitive advantage**?

Zoom invested early in custom, state-of-the art [streaming technology](http://highscalability.com/blog/2020/5/14/a-short-on-how-zoom-works.html), which turned into a moat for their business.

Airbnb invested early in [machine learning](https://news.airbnb.com/sharing-more-about-the-technology-that-powers-airbnb/) to support both guests' (_search & discovery_) and hosts' (_smart pricing_) experience.

How can engineering support your product strategy to deliver a superior experience that drives the company mission forward?

What kind of products can be **enabled** by a strong technical strategy?

Uber invested in a general purpose fulfillment platform, which down the line enabled the creation of [multiple product lines](https://www.uber.com/newsroom/go-get/), like Uber Eats and Uber Freight.

Finally, the best strategy doesn't exist in isolation, it depends on your team and evolves with it.

How can you leverage your engineering **team's strengths** to create something that benefits your company long term?

Airbnb famously took a big bet on [React Native](https://reactnative.dev/) in 2016, but ended up sunsetting it in 2018. In a long and articulate [retrospective](https://reactnative.dev/) they acknowledged their issues where mostly operational — i.e. already having two native apps and a large team of native mobile developers.

Native developers were reluctant to 1) learn a new framework that completely changed the way they work, and 2) having to maintain a complex, hybrid approach between the old and the new.

React Native might have been a good choice in isolation, or to start with, but it failed because of Airbnb's team configuration and pre-existing assets.

-   📑 **[How to Overcome the Technical Strategy Spiral](https://www.reforge.com/blog/overcome-the-tech-strategy-spiral)** — this great article by Reforge explains how to shift your skills from execution to strategy, and how to create a technical strategy from scratch.
    
-   📑 **[The Trident Model of Career Development](https://www.thekua.com/atwork/2019/02/the-trident-model-of-career-development/)** — [Pat Kua](https://twitter.com/patkua) argues the best model for Engineering career development is not the classic dual track, but a _trident_ that separates Technical Leadership from pure Individual Contribution. Thoughtful and recommended.
    
-   📑 **[React Native at Airbnb](https://medium.com/airbnb-engineering/react-native-at-airbnb-f95aa460be1c)** — long post-mortem of the failed attempt, lasted 2 years, to introduce React Native into the mobile stack of Airbnb. This is the best piece you can read about what does it mean to deploy a strategic tech initiative into a large, modern company, with its challenges and opportunities.
    

Here are the **remote engineering** jobs featured this week! They are all from great companies and I personally review them one by one.

1.  **Pager** — [Senior Software Engineer](https://pallet.xyz/list/refactoring-jobs/job/a37dac23-ac6a-4e53-9243-2567aa97f993) — _the best way to capture and share your digital life._
    
2.  **Journey** — [Founding Frontend Engineer](https://pallet.xyz/list/refactoring-jobs/job/31bc6537-7db9-4517-bd50-3b27ea8984cc) — _a storytelling tool designed for the internet age._
    
3.  **Zapier** — [VP Platforms & Labs](https://pallet.xyz/list/refactoring-jobs/job/518b0188-6975-4877-bf47-819146599b71) — _the easiest way to automate your work_
    
4.  **Hubspot** — [Director of Engineering, Fintech](https://pallet.xyz/list/refactoring-jobs/job/8a28c8f2-efd3-462e-ba58-d2748d5e2210) — _inbound marketing, sales and service software_
    
5.  **Stripe** — [Staff Engineer, Product Experience](https://pallet.xyz/list/refactoring-jobs/job/8885d449-0fbf-49e2-a262-aa96c0e4999e) — _online payment processing for internet businesses_
    

Browse many more open roles (or add your own) on the full board 👇

[Check out all Refactoring Jobs](https://pallet.xyz/list/refactoring-jobs/jobs)

_Hey, I am Luca 👋 thank you for reading this far!_

_Every week I read tens of articles and draw from my own experience to create a 10-minutes advice about some engineering leadership topic._

_Subscribe below to receive a new essay every Thursday and put your own growth on autopilot!_
