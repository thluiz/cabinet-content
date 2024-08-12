---
created: 2024-08-08T09:33:28 (UTC -03:00)
tags: []
source: https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev
author: 
---

# 90% of performance is data access patterns | Swizec Teller

> ## Excerpt
> Removing a single line of code slashed database CPU usage by 66% 🤘

---
If you want a fast app focus on your data patterns. Here's a war story from production.

_"Emergency meeting! WTF is your client doing to our servers!?"_

The platform team had been chasing a performance bug for weeks. It looked like API requests hogging resources and making the whole system slow even for unrelated requests.

This is an issue that happens on Nodejs servers due to [how JavaScript's event loop works](https://snyk.io/blog/nodejs-how-even-quick-async-functions-can-block-the-event-loop-starve-io/). The single-thread single-process model is great at parallelizing IO tasks with lots of waiting, but tends to get stuck on computation. Even a quick algorithm can block the event loop and force all requests to wait.

Platform suspected this was happening and built a visualization of slow API requests combined with their frequency. My team was at the top of the list 😅

  ![A major outlier at the top of the list](https://swizec.com/static/0999287c7379a65b56db45ba374c27f3/4ef49/A-major-outlier-at-the-top-of-the-list-91e172.png)

A major outlier at the top of the list

That's 1,000,000+ requests per week loading all of a patient's appointment history. You have to understand how wild that is. The company had around 30,000 patients at the time, most of whom access the patient portal once a week at best.

So why do we keep loading their history? Why do the slowest 5% of requests take over 1.1 seconds? (p95 latency)

## [](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#old-sins-come-back)[Old sins come back](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#old-sins-come-back)

At this point I remembered an old joke we used to say – _"Haha isn't it funny how our backend-for-frontend loads appointments on every request"_.

Waitaminute, surely we're not doing that?

And there it was: Deep in our session middleware, an old performance optimization linked to a 2 year old bug report where someone found this and could not yet remove.

Those jokes you share around when everyone's chill and it's time to gripe, they matter. They are your lore. The myths based on nuggets of truth. And sometimes they tell you where to find the dragon.

Before we had our current infrastructure, before there was even much of a team, a sole engineer faced a problem: Appointments were slow to load and the UI suffered. But the data was small and the time was short. _"Cache this in the session"_, they thought and moved on with their day.

The middleware would fetch all appointments, put them in the session, and all code could read that variable. This worked fine. For years!

## [](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#problems-change-with-scale)[Problems change with scale](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#problems-change-with-scale)

The home-made caching approach worked when we had few users, little data, and every engineer knew the whole system.

Back when we used a traditional webpage approach it even made sense! Putting data in the session means you're not hitting a slow API multiple times per page. Originally we may have cached across requests, but that has since been lost.

  ![Full page hits internal API once](https://swizec.com/static/fed83ebcfb243fa680a8ce2614705774/4ef49/Full-page-hits-internal-API-once-7jf7hi.png)

Full page hits internal API once

Despite the lack of cross-request caching on the frontend server, things worked fine thanks to our internal service caching data in the database. Running a SQL query for the rich data you need is much faster than hand-holding a series of slow vendor requests.

But then the client app started to grow more complex as our user expectations grew. We went from a series of basic webpages to a rich single-page application hitting our servers with API requests for all sorts of data.

All those requests had to hit the session middleware to verify authentication. The same session middleware that fetched, and then ignored, all appointments.

  ![Every API request hits the session wrapper and re-fetches appointments](https://swizec.com/static/2f8cee10bb84aaa652c50b41339a8398/4ef49/Every-API-request-hits-the-session-wrapper-and-re-fetches-appointments-i2a9f4.png)

Every API request hits the session wrapper and re-fetches appointments

The internal caching layer kept us afloat.

Except as usage grew, all those database reads started putting a strain on the system. The database struggled with frequent memory thrashing and the code struggled to keep processing all the payloads. Everything got slow.

\[side-note\] Databases internally try to keep frequently used data in memory. When you have lots of queries reading the same data, this works great. When you start having lots of queries for distinct chunks of data, the database can fall into a thrashing pattern where it's constantly moving data between disk and memory. This is slow.

## [](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#the-system-will-tell-you-where-it-hurts)[The system will tell you where it hurts](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#the-system-will-tell-you-where-it-hurts)

That visualization of slow requests was the system telling us where it hurts: Constantly fetching patient appointments.

After analyzing the code we found that nobody even uses this data anymore! The client uses its own API method to explicitly fetch appointments. _Once_.

We deleted 1 line of code and CPU usage on our central database dropped by 66%.

  ![CPU usage drop after we stopped over-fetching](https://swizec.com/static/878bdf104499915df08e4475bce9fb7e/4ef49/CPU-usage-drop-after-we-stopped-over-fetching-e6a03c.png)

CPU usage drop after we stopped over-fetching

We went from calling that endpoint 150,000x/day to 1,000x/day and the p95 latency dropped by half (slowest 5% of requests took more than 500ms instead of 1s). Latency improved on all other API requests because we stopped hogging the event loop.

This server-side change is [my biggest ever React app performance improvement](https://swizec.com/blog/my-biggest-react-app-performance-boost-was-a-backend-change/). It made everything feel snappier.

## [](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#you-cant-prevent-mistakes-but-you-can-invest-in-observability)[You can't prevent mistakes, but you can invest in observability](https://swizec.com/blog/90p-of-performance-is-data-access-patterns/?ref=dailydev#you-cant-prevent-mistakes-but-you-can-invest-in-observability)

Mistakes happen. You can't expect a towering pile of abstractions across dozens of repositories, hundreds of files, millions lines of code, a few years, and a few dozen engineers to stay internally consistent forever. You're going to miss things.

You build a thing that works today and forget about it while the code keeps doing its thing. A few years later there's a bug and someone makes a fix they didn't fully understand. Then you add a bunch of scale and things break in new interesting ways the original creator never could have predicted.

The lesson here isn't that years ago we wrote bad code or that those engineers didn't know what they were doing. The code was good when it happened and the engineers did their best.

The lesson is that the environment changed, it always does, and _observability_ helped us identify a simmering issue before it became an outage. That's a win.

Cheers,  
~Swizec

Published on July 25th, 2024 in [Software Engineering](https://swizec.com/categories/software%20engineering/), [Scalability](https://swizec.com/categories/scalability/), [Observability](https://swizec.com/categories/observability/), [Scaling Fast Book](https://swizec.com/categories/scaling%20fast%20book/)

#### Did you enjoy this article?

#### Continue reading about 90% of performance is data access patterns

Semantically similar articles hand-picked by GPT-4

-   [My biggest React App performance boost was a backend change](https://swizec.com/blog/my-biggest-react-app-performance-boost-was-a-backend-change/)
-   [Logging 1,721,410 events per day with Postgres, Rails, Heroku, and a bit of JavaScript](https://swizec.com/blog/logging-1721410-events-per-day-with-postgres-rails-heroku-and-a-bit-of-javascript/)
-   [How to DDoS yourself with analytics –– a war story](https://swizec.com/blog/how-to-ddos-yourself-with-analytics-a-war-story/)
-   [Immutability isn't free](https://swizec.com/blog/immutability-isnt-free/)
-   [The day I crashed production 4 times](https://swizec.com/blog/the-day-i-crashed-production-4-times/)

**Have a burning question that you think I can answer?** Hit me up on [twitter](https://twitter.com/swizec) and I'll do my best.

**Who am I and who do I help?** I'm Swizec Teller and I turn coders into engineers with _"Raw and honest from the heart!"_ writing. No bullshit. Real insights into the career and skills of a modern software engineer.

**Want to become a _true_ senior engineer?** Take ownership, have autonomy, and be a force multiplier on your team. The Senior Engineer Mindset ebook can help 👉 [swizec.com/senior-mindset](https://swizec.com/senior-mindset). These are the shifts in mindset that unlocked my career.

**Curious about Serverless and the modern backend?** Check out Serverless Handbook, for frontend engineers 👉 [ServerlessHandbook.dev](https://serverlesshandbook.dev/)

**Want to Stop copy pasting D3 examples and create data visualizations of your own?** Learn how to build scalable dataviz React components your whole team can understand with [React for Data Visualization](https://reactfordataviz.com/)

**Want to get my best emails on JavaScript, React, Serverless, Fullstack Web, or Indie Hacking?** Check out [swizec.com/collections](https://swizec.com/collections)

**Did someone amazing share this letter with you?** Wonderful! You can sign up for my weekly letters for software engineers on their path to greatness, here: [swizec.com/blog](https://swizec.com/blog)

**Want to brush up on your modern JavaScript syntax?** Check out my interactive cheatsheet: [es6cheatsheet.com](https://es6cheatsheet.com/)

**By the way, just in case no one has told you it yet today: I love and appreciate you for who you are ❤️**
