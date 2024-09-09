---
created: 2024-08-27T11:41:25 (UTC -03:00)
tags: []
source: https://bytes.dev/archives/315?ref=dailydev
author: 
---

# Bytes #315 - React Native goes stable

> ## Excerpt
> Node bends the knee to npm, Expo DOM Components go wild, and Cloudflare reaps the benefits of my slurpee habits.

---
![Bytes](https://bytes.dev/images/bytes-banner-rounded.png)

**Today’s issue:** Node bends the knee to npm, Expo DOM Components go wild, and Cloudflare reaps the benefits of my slurpee habits.

Welcome to [#315](https://bytes.dev/archives/315).

___

![Eyeballs logo](https://bytes.dev/images/content/eyes.png)

## The Main Thing

![A man looking shocked, holding small toy hands to his head](https://bytes.dev/images/content/tiny-hands.jpg)

When you realize React Native will turn 10 years old before releasing v1.0

### React Native goes stable

It usually doesn’t feel _great_ when someone tells you, “You almost look completely stable.” Especially when it’s your high school crush saying that to you as you perform a one-man show of _Shakespeare in Love_ as part of your Prom-posal 😞.

But when it comes to React Native’s new architecture, being “almost stable” is a major step forward – and that’s what we got with last week’s [v0.75](https://reactnative.dev/blog/2024/08/12/release-0.75) release.

It introduced a series of critical bug fixes and stability improvements to the New Arch that gets us that much closer to the long-awaited stable release that’s been years in the making.

**Why is this a big deal?** The New Architecture represents a complete refactoring of React Native’s internals, which will allow developers to utilize concurrent React features and enjoy a better and more modern DX. It will also help solve some of RN’s [long-standing issues](https://docs.expo.dev/guides/new-architecture/), like interoperability with synchronous native APIs.

Besides the New Arch, v0.75 introduced a few other notable improvements:

-   The auto-linking step is now 6.5x faster on Android and 1.5x faster on iOS, if you’re using Expo.
    
-   They’re sunsetting the `react-native init` command at the end of this year to ~extort~ _encourage_ you to use Expo going forward.
    
-   It comes with v3.1 of Yoga (RN’s layout engine), which provides percentage support for `gap` properties.
    

**Bottom Line:** v0.75 feels like one of those “releases between releases” that’s setting the stage for big things to come. With any luck, my children will live long enough to see React Native 1.0.

___

![QA Wolf logo](https://bytes.dev/images/content/qa-wolf-logo.png)

## Our Friends (With Benefits)

![NASA employees celebrating](https://bytes.dev/images/content/nasa-employees.jpg)

When my team doesn't have to wait 2 hours to deploy anymore

### [Save your team six figures by outsourcing testing to QA Wolf](https://www.qawolf.com/?utm_campaign=Automated4Months08192024&utm_source=bytes&utm_medium=newsletter)

They helped Drata save over $500k in engineering and infrastructure costs ([see breakdown](https://www.qawolf.com/case-studies/drata?utm_campaign=Automated4Months08192024&utm_source=bytes&utm_medium=newsletter)) – and they can probably do the same for you.

**Here’s how:**

-   They get you 80% E2E test coverage for the cost of a single QA engineer
    
-   They provide all the infra to run thousands of tests in parallel, so you can get pass/fail results in _3 minutes_ and start [deploying multiple times per day](https://qawolf.com/how-it-works?utm_campaign=SlowQACycles08192024&utm_source=bytes&utm_medium=newsletter&ck_subscriber_id=887711114)
    
-   They reproduce every bug and send a video walk-thru to your issue tracker with Playwright trace logs
    

This saves your team thousands of hours of engineering time _and_ dramatically speeds up your release velocity. Win-win.

[Try their 90-day pilot program](https://www.qawolf.com/?utm_campaign=Automated4Months08192024&utm_source=bytes&utm_medium=newslette) – and see why Drata’s Senior Engineering Manager says that, “QA Wolf has given us full confidence on each release, and we’re very happy.”

___

![Spot the Bug logo](https://bytes.dev/images/content/spot-the-bug.png)

## Spot the Bug

```js
const newsletter = "Bytes" const tagline = "Your weekly dose of JavaScript" [newsletter, tagline].forEach((el) => console.log(el))
```

___

![Cool Bits logo](https://bytes.dev/images/content/cool-bits.png)

## Cool Bits

1.  Speaking of React Native, Evan “I-invented-the-Wendy’s-Baconator” Bacon gave this [wild demo of Expo DOM Components](https://twitter.com/Baconbrix/status/1823366398405415176) – which let you incrementally adopt native views in React websites, instead of starting from scratch.
    
2.  sdegutis created this [Case for vanilla JSX](https://vanillajsx.com/).
    
3.  Sarah Gooding wrote about how [Node.js has formalized a plan for removing Corepack](https://socket.dev/blog/node-js-takes-steps-towards-removing-corepack) after a developer submitted an issue to enable Corepack by default. Let that be a lesson to anyone else who dares to publicly question the authority of npm.
    
4.  Neon offers a [fully managed Postgres platform](https://fyi.neon.tech/bytes) to help you build cool stuff faster. Your database scales automatically, and you can instantly spin up ready-to-use databases for development, testing, and previews. \[sponsored\]
    
5.  [Stepperize](https://stepperize.vercel.app/) is a simple, typesafe, and flexible library for building customizable steppers. “Michael, what did I tell you about ~yeppers~ steppers?”
    
6.  Steve Simkins wrote about [Why he left Neovim for Zed](https://stevedylan.dev/posts/leaving-neovim-for-zed/).
    
7.  Heikki Lotvonen wrote on the Glyph Drawing Club blog about [Syntax highlighting in handcoded websites](https://blog.glyphdrawing.club/font-with-built-in-syntax-highlighting/). I’ve been trying to break into the GDC for years now, but I just can’t make it through those hazing rituals.
    
8.  CarbonQA provides [high-quality QA services that scale](https://carbonqa.com/?utm_source=bytes&utm_medium=email&utm_campaign=cool_bits&utm_content=aug19). Their US-based testers work directly with your tools and break your app repeatedly, so you don’t have to do it yourself. \[sponsored\]
    
9.  Ryan Christian wrote about [Prerendering with Preact’s Preset Vite plugin](https://preactjs.com/blog/prerendering-preset-vite/). He previously preceded this post by previewing what the prerendering preset would proceed to produce. Precisely.
    
10.  Cloudflare just released [new developer docs](https://developers.cloudflare.com/) that are powered by Astro and now have much faster load times. But since correlation != causation, those speed gains could just as easily have been “caused” by the fact that I recently decided to boost my 7-Eleven slurpee intake to two per day (I needed the extra electrolytes).
    

___

![Spot the Bug logo](https://bytes.dev/images/content/spot-the-bug.png)

## Spot the Bug: Solution

```js
const newsletter = "Bytes" const tagline = "Your weekly dose of JavaScript" [newsletter, tagline].forEach((el) => console.log(el))
```

You might be surprised to learn that this code doesn’t execute. If you try, you’ll get an error – `Uncaught ReferenceError: tagline is not defined`. But clearly it is, right? Wrongo.

This is the _very_ rare scenario where not using semicolons can bite you. Because we have a string followed by _no_ semicolon followed by an opening array bracket, JavaScript interprets our code like this, as if we’re trying to access elements from the string.

```js
"Your weekly dose of JavaScript"[newsletter, tagline].forEach();
```

This, of course, throws an error because tagline isn’t defined. To fix this, add a `+` sign before the array.

```js
const newsletter = "Bytes" const tagline = "Your weekly dose of JavaScript" +[newsletter, tagline].forEach((el) => console.log(el))
```

That’s _mostly_ a joke, but it does work…

Instead, just use semicolons.

```js
const newsletter = "Bytes"; const tagline = "Your weekly dose of JavaScript"; [newsletter, tagline].forEach((el) => console.log(el));
```
