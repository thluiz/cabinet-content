---
created: 2023-10-30T08:29:25 (UTC -03:00)
tags: []
source: https://swizec.com/blog/solve-the-problem-not-a-different-more-difficult-problem/?ref=dailydev
author: 
---

# Solve the problem, not a different more difficult problem | Swizec Teller

> ## Excerpt
> Solve the problem at hand, not the one you imagine might come next. A simple fix now often beats a complex one later.

---
Here's a trap engineers fall into all the time.

You are asked to solve a problem and you think _"Ah yes! The general case for this is so and so and to make the solution work forever in all cases we'll need to ..."_.

Engineering is programming over time, right? Not always! Sometimes you need to solve today's problem and move on.

For example – our PM wanted to prove a hunch with an A/B test on the corporate site. We could descend upon this project, that we don't own, and design a generic A/B testing framework that works with Gatsby and our headless CMS and all our product testing infrastructure. _Or_ we can add a `useEffect` to one button. 🤔

The team's eyes lit up when discussing the first option. The PM's eyes lit up when discussing the second.

Guess which approach we took.

Yep, we built the `useEffect` in one afternoon and moved on. Turns out the team that owns our marketing site is building a more robust A/B testing solution anyway. Should be ready in a few weeks 🤞

## [Cost vs benefit](https://swizec.com/blog/solve-the-problem-not-a-different-more-difficult-problem/?ref=dailydev#cost-vs-benefit)

What we're getting at here is return on investment. What is the goal and how much does it cost to get there?

  [![How I think of ROI](https://swizec.com/static/c78aa11292cfec2cb1cb7e37a417386d/4ef49/How-I-think-of-ROIi36g80.png "How I think of ROI")](https://swizec.com/static/c78aa11292cfec2cb1cb7e37a417386d/3cb45/How-I-think-of-ROIi36g80.png)

How I think of ROI

This is roughly how I think about this.

You have different solutions that take a certain amount of time or money – the cost. Your solution line goes higher or lower on the graph. The solution will provide value after a starting gap.

The area between these two lines represents my fuzzy understanding of ROI – how much value does a solution need to provide to justify the initial investment.

Value and cost are fuzzy concepts here. Cost can be time, effort, maintenance burden, or even money. Value can be dollars in the bank, lower future cost, or data that your PM can use to win an argument.

Yes value can even be "I had fun building this and it looks great on my CV", but please save that for side projects :)

## [Why engineers get this wrong](https://swizec.com/blog/solve-the-problem-not-a-different-more-difficult-problem/?ref=dailydev#why-engineers-get-this-wrong)

As engineers we love solving big fun problems. And we hate solving the same problem twice.

So we look for the most comprehensive generalest version of an ask and solve for that. Why build a hammer if you can [build a hammer factory](https://gwern.net/doc/cs/2005-09-30-smith-whyihateframeworks.html) that creates any hammer you could ever want?

But that's a much harder problem! By the time you're ready to test the hammer factory, I've already fed 5 people.

Remember: Your customer doesn't care about the hammer, they want to eat dinner but the chair's broken. Make sure you ask _why_ they need a hammer factory factory.

Cheers,  
~Swizec

PS: like 90% of what I do as tech lead is to pull talented engineers who want to flex (including myself) away from shiny problems and onto the task at hand

## Did you enjoy this article?

Published on October 3rd, 2023 in [Mindset](https://swizec.com/categories/mindset/), [Engineering](https://swizec.com/categories/engineering/),

___

[

![Senior Engineer Mindset cover](https://swizec.com/static/49b03d61b7348ccaa62e58389b04f057/d0275/SeniorMindset-cover-3d.png)

](https://swizec.com/senior-mindset/)

### Senior Mindset Book

Get promoted, earn a bigger salary, work for top companies

[Learn more](https://swizec.com/senior-mindset/)

**Have a burning question that you think I can answer?** Hit me up on [twitter](https://twitter.com/swizec) and I'll do my best.

**Who am I and who do I help?** I'm Swizec Teller and I turn coders into engineers with _"Raw and honest from the heart!"_ writing. No bullshit. Real insights into the career and skills of a modern software engineer.

**Want to become a _true_ senior engineer?** Take ownership, have autonomy, and be a force multiplier on your team. The Senior Engineer Mindset ebook can help 👉 [swizec.com/senior-mindset](https://swizec.com/senior-mindset). These are the shifts in mindset that unlocked my career.

**Curious about Serverless and the modern backend?** Check out Serverless Handbook, for frontend engineers 👉 [ServerlessHandbook.dev](https://serverlesshandbook.dev/)

**Want to Stop copy pasting D3 examples and create data visualizations of your own?** Learn how to build scalable dataviz React components your whole team can understand with [React for Data Visualization](https://reactfordataviz.com/)

**Want to get my best emails on JavaScript, React, Serverless, Fullstack Web, or Indie Hacking?** Check out [swizec.com/collections](https://swizec.com/collections)

**Did someone amazing share this letter with you?** Wonderful! You can sign up for my weekly letters for software engineers on their path to greatness, here: [swizec.com/blog](https://swizec.com/blog)

**Want to brush up on your modern JavaScript syntax?** Check out my interactive cheatsheet: [es6cheatsheet.com](https://es6cheatsheet.com/)

**By the way, just in case no one has told you it yet today: I love and appreciate you for who you are ❤️**
