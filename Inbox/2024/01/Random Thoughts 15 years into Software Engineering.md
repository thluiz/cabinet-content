---
created: 2023-10-24T15:39:56 (UTC -03:00)
tags: []
source: https://roughlywritten.substack.com/p/random-thoughts-15-years-into-software?utm_source=tldrnewsletter
author: Ryan O'Neill
---

# Random Thoughts 15 years into Software Engineering

> ## Excerpt
> I’m coming up on 15 years of professional software engineering. I’ve worked at companies from unknown startups to large FAANG-like silicon valley companies and everywhere in between. In no particular order, sharing some things I’ve learned that others may find helpful.

---
I’m coming up on 15 years of professional software engineering. I’ve worked at companies from unknown startups to large FAANG-like silicon valley companies and everywhere in between. In no particular order, just sharing some things I’ve learned that others may find helpful.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9fb0c2cf-2014-4956-b61e-4a4a7a85a676_1456x816.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9fb0c2cf-2014-4956-b61e-4a4a7a85a676_1456x816.png)

**Try not to think in absolutes**. Computers are great at binary, humans not so much. Every rule has an exception, every architectural approach has a tradeoff.

**Debuggability is highly underrated.** When writing code, you have to think about how it will execute. You also need to be thinking about how it will fail and how you will debug it \*in production\*. Leave yourself audit trails, store data in human readable formats, and invest in admin tooling.

**Projects are late, a lot.** This is not unique to software. The reality is that time is constantly moving against us, and when unexpected things happen they can take an order of magnitude longer than we planned. And in software, there’s always more we can add to a given feature or system. Give a best effort, and keep your stakeholders informed of progress and blockers.

**Aggressively manage scope.** Related to the above, protect your project’s scope. Defensively, as people will often try to add things throughout the project. You don’t have to push back if you don’t want, but be transparent about how it will affect the project delivery and communicate it widely. Offensively, look for things you can cut or, my favorite, look for things that you can ship AFTER launch and push to prioritize those at the end. I love a good “fast follow”.

**Staging is pretty much always broken.** I see a lot of younger devs hand wring about testing environments. Don’t get me wrong, testing environments are great and you should use them. But the larger your systems get the harder and harder is to maintain a parallel environment that actually mirrors production in a meaningful way. Make a best effort - but otherwise don’t sweat it and don’t be afraid to test things in production (safely, feature flags are your friend).

**Action is rewarded.** Pointing out problems or complaining is not.

**Take ownership of your systems**. Not just \*your\* code. Act like you are personally responsible for the success of the systems you work on, end-to-end (because you are!).

**You are part of a larger organization.** The software you produce might be the product it sells to make money, but that doesn’t mean your job is the center of the universe. Take time to meet people from other functions (sales, marketing, finance, etc) and learn how they think and work. You’ll have a much more holistic view of the entire business, and decisions that come down around you will make a lot more sense. This is true even in the smallest of startups.

**Ask dumb questions.** If you’re in a group of people and have a question about what is being discussed, there’s a great chance that someone else in the group has the same question. Speak up! Ask that question for the good of the team. Nobody will remember that you asked a question, but you and everyone else will forever have the information in the answer.

**You won’t have time to go back and fix technical debt.** Don’t kid yourself. Do your best to minimize it up front, and prioritize what to address based on what you would be okay living with for the foreseeable future. 99/100 you won’t get to come back and fix it until this particular part of the system needs a major overhaul, and you don’t know when that will be.

**Enjoy it!** If you’re anything like me, working with code is fun. Sometimes I still laugh that someone is willing to pay me a significant salary to do something I enjoy so much.

That’s all for now!
