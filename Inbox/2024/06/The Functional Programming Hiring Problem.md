---
created: 2024-06-11T08:09:05 (UTC -03:00)
tags: []
source: https://blog.janissary.xyz/posts/hiring-functional-programming?utm_source=tldrnewsletter
author: 
---

# janissary

> ## Excerpt
> June 9, 2024 | 20 min. read

---
June 9, 2024 | 20 min. read

___

If you've ever seen a discussion of functional programming languages on the Internet, you'll have probably noticed one talking point in particular that comes up frequently. For the sake of generalization, let's make up a hypothetical functional programming language called Gooby. In these discussions, often someone will say something like "Oh man, I love writing Gooby. I wish I could use it at my company or on my team, but it's just so hard to hire Gooby engineers."

Inevitably, someone will reply with something like, "You fool! Hiring Gooby engineers is actually a hidden advantage! Sure, there's less Gooby engineers than Java or Python or Node.js engineers, but everyone who bothers to learn a niche language like Gooby is a passionate rockstar 10x engineer! Despite a lower quantity of people to choose from, the average applicant is of much higher quality so the [expected value](https://en.wikipedia.org/wiki/Expected_value) from hiring is much higher. You should use Gooby at your company!"

I've worked at a company that used a functional programming language pretty ubiquitously throughout its codebaseI'm not going to name the language itself, because this post would just turn into a flame war over that language specifically, and I definitely don't want to cast shade on any language/community in particular. I'm also kind of hoping that the most annoying people read this and think, "Ah, of course he's talking about _that_ language over there! This criticism obviously doesn't apply to _my_ perfect and favorite language!" Regardless, I feel that the thesis and content of this post applies pretty evenly to most functional programming languages.. I think I agree with the above thesis - most of my coworkers were extraordinarily smart, hard-working, and curious people - but I think it leaves out some important information.

The fact of the matter is that, when hiring for functional programming languages (such as Gooby), you're primarily picking from one of three applicant pools:

1.  Résumé-spammers that are applying to any and every job posting they see, regardless of whether or not they are qualified for the job or have even heard of Gooby. There's tough times in the industry so I don't necessarily fault these people for trying to find work, but it's seriously polluting the hiring process for everyone else (and has turned the job hunt into an arms race for producing the highest number of LLM-slop LinkedIn messages and cover letters). In reality, you're probably not going to hire one of these people.
2.  Bright undergraduates that have just taken their first Programming Languages course, and have seen the light after their first brush with functional programming. They believe that Gooby is the One True Way to write software, and are a textbook example of "zeal of the convert". These people typically have little production engineering experience and are basing their opinion of Gooby on a few personal projects, some open-source work, and many blog posts.
3.  Senior engineers who are there for one purpose, and one purpose only - to write code in Gooby. It doesn't matter what the company does, or if it's successful, or if Gooby is the right tool for the job. As long as they're writing Gooby, they're happy.

As mentioned, you're probably not going to hire from Group 1 for reasons that should be obvious. Group 2, however, can be a source of great potential. If you can find a motivated and enthusiastic engineer early in their career and train them in your stack and processes, you've made a great hire. Once the rubber meets the road and they have to use Gooby in angerI'm not sure who coined this turn of phrase (maybe [Fred Herbert](https://erlang-in-anger.com/?ref=blog.janissary.xyz)?), but I love "using X in anger" as a way to describe real-world production use of something. It's easy for everything to be butterflies and rainbows when you're using a tool for fun. You can only truly know how you feel about a tool once you've had it page you at 3AM on a Saturday., they'll probably adopt a more nuanced opinion of the language and be willing to try something different. If using Gooby turns out to be a big win for the company, it'll be a pleasant surprise for everyone. It won't be a huge deal to pivot to something different if not, or if a new project uses a different stack, or any other scenario where Gooby might have an uncertain future at your company.

Group 3, however, is where hiring mistakes are made. On paper, these look like the best candidates anyone could ask for. They have real-world experience working with Gooby, they might be the author of popular open source libraries for Gooby, or even spoke at last year's GoobyCon! They love Gooby, they have skin in the game, and it shows.

That's the thing, though. Gooby is just a means to an end; the ultimate end goal of your company is to write software that is useful to people and makes money. For the zealots, however, Gooby is the end in and of itself. No man can serve two masters, and this is no different.

## The Problem

The [multi-armed bandit](https://en.wikipedia.org/wiki/Multi-armed_bandit) is a really interesting problem in mathematical optimization. A one-armed bandit is a sly name for a slot machine - if you pull the arm, you're likely to lose some money - but a multi-armed bandit is a hypothetical bank of multiple slot machines, each with a different payout amount and probability of payout. The problem is pretty simple: you're a gambler, and you're trying to maximize your total winnings. The challenge is in balancing the amount of money spent to determine average payouts of different machines, and the amount of money gained from exploiting the highest expected value.

This problem is so interesting, in fact, that during World War II the Allies proposed air dropping copies of the original paper over Germany. The end result, or so it was theorized, was that German scientists would be so fascinated and distracted by the problem that they would abandon the war effort and cripple any German military research projects in the process.

I feel like functional programming languages can have the same effect on certain software engineers. At first, it's just a convenient way to model data flows immutably or confidently get stuff done with robust type systems. Then, one thing leads to another, and you're knee deep in learning about homotopy type theory or continuations or whatever. Meanwhile, you're a week behind on that Jira ticket for issuing JSON Web Tokens because there's no actively maintained JWT libraries for Gooby.

I don't know if I would go so far as to describe it as an [infohazard](https://en.wikipedia.org/wiki/Information_hazard), but it's pretty darn close. I've seen engineers, to the detriment of their careers/company/reputation, _insist_ on using a functional language at every moment possible. I've seen an engineer threaten to quit if their next project wasn't written in their language of choice. I've seen buggy pre-1.0 libraries used for critical production softwareNot "critical" as in "if it goes down it's really annoying", but "critical" as in "checks and stores passwords" or "moves money from one bank account to another". simply because it was a wrapper library written in a functional language, as opposed to a more stable library written in an imperative language. I've seen, more times than I can count, hand-wavey "wisdom" from the language BDFL used as a kind of trump-card argument in technical discussions.

Granted, all of these things independently are personnel issues that should be solved with managerial means. But in aggregate, they paint a pretty striking picture of your "ideal" candidate when hiring for a functional programming language. Yes, you're hiring the most maximally curious engineers. The engineers are so curious, in fact, that they'll become enamored with the most esoteric and arcane tools available to the point of total tunnel vision. Yes indeed, you're hiring the most motivated engineers that really Know Their Shit. These engineers are so motivated, they don't mind wasting a few weeks reimplementing a protocol that's baked into the standard library of most other mainstream languages.

Some engineers - the very engineers that the hiring process is inadvertently filtering for! - just cannot be trusted to do what's actually productive if it's at the cost of playing with their favorite intellectual chew toy.

## The Origin of The Problem

A lot of ink has been spilled in the past year or so on the idea of the "[zero interest rate phenomenon](https://newsletter.pragmaticengineer.com/p/zirp)". If you're not in the industry or somehow haven't heard of it, the idea is pretty simple. After the financial crisis of 2008, interest rates in the US were pretty much rock bottom in order to stimulate economic activity. Due to this, money was cheap and venture capital could afford to throw money at bad bets fairly often (as long as their winners were big winners!). As a result, the tech industry was flush with cash and could tolerate a pretty extreme amount of waste. Watch any "day in the life of a software engineer" TikTok circa the Peak Good Times of 2021, and you'll get a good sample.

I think this is a pretty decent hypothesis for the origins of the functional programming hiring problem. With easy funding and relatively relaxed expectations on immediate profitability, companies could make poor technical decisions and slide by being less productive and efficient. As long as they could come up with semi-defensible tales of future growth, it wasn't a big deal if they chose the wrong tools to make their products. Meanwhile, the labor market for engineers became red-hot (for the engineers, that is!). With their pick of the litter, engineers could be pretty choosy about who they worked for and what they worked with (and make the big bucks all the same).

In the fallout of the industry bubble bursting, we're now left with a pretty bleak scene. Companies that made the fun choice instead of the pragmatic choice are facing the consequences and being shut down or acquihired for pennies. A lot of good developers are being laid off, with years of nothing but niche languages on their résuméA lot of people love to say that they're hiring "good engineers", rather than "good Java engineers" or "good Rails engineers" or whatever. That might have held true in better times, but as someone who was recently on the job hunt, employers are a whole lot pickier nowadays. . Worst of all, genuinely interesting and innovative programming languages are being falsely maligned for performing poorly on problems and domains they were never designed for.

## Solutions to The Problem

This isn't a screed against functional programming languages, or functional programming in general. I love functional programming! Learning Haskell in my college days genuinely revolutionized the way I think about programming, and I got my career started on Clojure. Even in imperative languages, I often reach for functional patterns to get the job done.

However, I think that we all need to be a little more honest with ourselves as software engineers. Is using Gooby _really_ an advantage for you? Does it _actually_ offer some [alpha](https://en.wikipedia.org/wiki/Alpha_(finance)), or would you have been just as successful creating the same product in some other language? Are you _sure_ you don't use Gooby just because it's fun to writeIf you answered "no" here, there's no shame! All other things being equal, I'd rather have fun while writing software.? If you answered "no" to any of the above questions, your path forward is difficult but [crystal clear](https://boringtechnology.club/?ref=blog.janissary.xyz). If you answered "yes" to every question, reflect a little harder and ask yourself the same questions again.

If you _still_ answered "yes" to every question, I have good news and bad news. The good news is that you have won the lottery. You're one of the few organizations with a _genuine_ need to use Gooby in production for the unique advantages that it offers. The bad news is that you are now cursed. You need to continue using and maintaining Gooby in production, while simultaneously maintaining a pragmatic engineering culture and preventing a cult of personality around Gooby from developing. Good luck!

To the hiring managers of the world, my advice is a bit more draconian. **Do not** post your job listings on language-specific job boards, unless you want the vast majority of your applicants to be language zealots. Filter **very strongly** for flexibility of thought amongst potential hires - "describe a time you had to learn something new" or "talk about a time where you realized you were wrong" are some great questions to ask. Above all, **do not** let the extremists or zealots into your company. They are under the memetic influence of dark programming language design warlocks, and their judgement is clouded. They will saddle you with technical debt, poison the engineering culture of your company, and, worst of all, leave you with a nostalgic longing for Java.

My advice to the language zealots of the world, if they have made it to the end of this post, is pretty simple. Take a step back, and analyze your [utility function](https://en.wikipedia.org/wiki/Utility#Utility_function). Are you in this to make useful things for people, or are you in this to explore what's possible at the bleeding edge of programming languages? If it's the latter, perhaps research would be more fulfilling than industry. Academia and private R&D each have their own (pretty significant) problems, but you'd be free to explore new ideas and their applications without worrying too much about the practicality of things. If it's the former, maybe soften your opinions a bit. Divest the tools you use from your personal and intellectual identity. Question honestly, and constantly, if the benefits of your tools outweigh their costs. Meditate on [Cromwell's rule](https://en.wikipedia.org/wiki/Cromwell's_rule):

> I beseech you, in the bowels of Christ, think it possible that you may be mistaken.

Or, more casually, this wisdom from Tumblr:

![](https://blog.janissary.xyz/hiring-functional-programming/wisdom.png)
