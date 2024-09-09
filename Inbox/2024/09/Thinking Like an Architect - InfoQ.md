---
created: 2024-08-11T22:51:18 (UTC -03:00)
tags: [thinking like architect,Architecture & Design,QCon London 2024,QCon Software Development Conference,Cloud Architecture,Architecture,]
source: https://www.infoq.com/articles/thinking-like-architect/?ref=dailydev
author: Gregor Hohpe
---

# Thinking Like an Architect - InfoQ

> ## Excerpt
> Are architects supposed to be the smartest people on the team? Certainly not. Rather, architects make everyone else smarter, for example by sharing decision models or revealing blind spots.

---
### Key Takeaways

-   Architects aren't the smartest people on the team, they are the ones making everyone else smarter. An architect is an IQ amplifier.
-   Riding the architect elevator means connecting the penthouse with the engine room. The value of a modern architect is measured by how many floors they can cover.
-   Using metaphors invites the audience into the thought process. Not inviting the audience is an underutilization of mental resources.
-   The most powerful models are the simplest. A good model simplifies and abstracts, providing clarity rather than confusion.
-   Architects see more dimensions: by expanding the problem and solution space, architects enable others to approach problems more intelligently.

Gregor Hohpe, author of _The Software Architect Elevator_, presented the "[Thinking Like an Architect](https://qconlondon.com/presentation/apr2024/thinking-architect)" session at QCon London 2024. This article represents the talk, which starts by explaining the roles of an architect and the concept of connecting levels.

Subsequently, we delve into the importance of metaphors to make complex technical concepts more relatable and we understand the advantage of making better decisions with models. Concurrently, we describe the importance of approaching problems from different angles, seeing more dimensions, and overcoming constraints.

## The Role of Architects

Being an architect is not defined by one's job title, it involves various perspectives and dimensions. Some excellent architects don't necessarily have "architect" on their business cards, and those with the title might not always be great. Architecture is more about a way of thinking and a lifestyle.

#### Related Sponsored Content

-   ##### [How to end the confusion in cloud transformations: Insights from McKinsey & Company](https://www.infoq.com/vendorcontent/show.action?vcr=fd6a6585-2ab0-4abe-827e-aef70463a48d&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fthinking-like-architect%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [8 Deployment Pattern Structures to Transform your CI/CD](https://www.infoq.com/vendorcontent/show.action?vcr=cd6f683e-d399-40fa-8794-e8fb2a79a5c4&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fthinking-like-architect%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Anatomy of an LLM](https://www.infoq.com/vendorcontent/show.action?vcr=2d401b86-a294-43b3-b038-0ae6116d9689&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fthinking-like-architect%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [API Management in the Age of AI](https://www.infoq.com/vendorcontent/show.action?vcr=f17bcc44-e811-471a-a126-492834419f29&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fthinking-like-architect%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Fast, Lean, and Unbreakable: Build Cloud Apps with Akka and Kubernetes](https://www.infoq.com/url/f/8b10d382-a68d-43e2-a6c0-8e79e690ee92/?ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    

Different organizations have varying perspectives on the role of architects. Some may operate without formally designated architects yet still maintain a coherent architecture.

Understanding what it means to be an architect is complex and multifaceted. It is helpful to consider various lenses through which one can be effective and valuable in the role of an architect.

## Architects Make Everyone Else Smarter

One of the biggest myths about architects is that they are the ultimate decision-makers, typically older, better paid, and seemingly wiser than everyone else. However, it's unrealistic for one person to make all the important decisions because nobody can be smarter than everyone else combined.

Architects mustn't be the smartest person in the room dictating to others. Their role is to help others make better decisions, for example by viewing problems from different perspectives, by better understanding trade-offs, or by considering additional options. That’s how architects are IQ amplifiers for the team: they make everyone else smarter.

## Architects Connect Levels

The book _The Software Architect Elevator_ discusses the role of an architect in connecting different levels of the organization and triggers the question of which architect is the most valuable for an organization. While some might argue it's the chief architect, others might believe it's those who are hands-on and implement solutions. However, the importance of the architects who bridge different levels within the organization is outlined.

A chief architect who creates diagrams can see their value diminish if they are isolated from reality. Similarly, a skilled developer's work must connect to the broader success of the organization. The key is not which level the developer occupies but how effectively they navigate and connect different levels. The concept of the architect elevator emphasizes the importance of moving between levels.

Organizations typically have many different levels due to division of labor, legal considerations, and structural hierarchy, often leading to issues. Leadership in the "penthouse" believes everything in IT is excellent, boasting about blockchain and GenAI. Meanwhile, developers in the engine room enjoy a sense of freedom since management is unaware of their actual work. This disconnect is facilitated by middle management, an isolation layer that creates a loosely coupled architecture, which in this case is not a good thing!

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/thinking-like-architect/en/resources/78figure-1-1721904839120.jpg)

<small><strong>Figure 1: The danger of too many floors</strong></small>

The danger of such a disconnect is outlined, as projects don't align with the strategic goals and the strategists are out of touch with reality. The most valuable architects are those who can bridge this gap.

Today's high-change environment requires recognizing the similarities between organization and technology. Architects should avoid oversimplifying when engaging with top executives, as they are highly knowledgeable leaders - while they may not know technical details, like Helm Charts, architects are encouraged to provide meaningful details that help leaders make better decisions. The architect elevator is proposed to unify understanding across organizational levels, not to present different stories.

While discussing topics like high automation levels, cloud infrastructure, and DevOps can be exhilarating for technical teams, CIOs or heads of IT are more concerned about avoiding security breaches, ensuring high availability, and maintaining cost efficiency. Hohpe outlines how to bridge these perspectives: the technical innovations directly address these CIO-level priorities, but architects riding the elevator have to connect the dots between the different levels. For example, automation assures consistent patch levels, which in turn improves security.

## Using metaphors

Architects have a powerful tool to connect these dots: metaphors. Choosing metaphors relevant to the industry; for instance, when working in financial services, using financial metaphors invites leaders into decision-making by making complex technical concepts more relatable. Instead of bombarding them with technical jargon, metaphors help bridge the gap and build trust. It's about engaging leaders in thoughtful decision-making rather than simply seeking approval. Using metaphors invites them into the decision process.

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/thinking-like-architect/en/resources/58figure-2-1721904839120.jpg)

<small><strong>Figure 2: Connecting the dots across layers</strong></small>

Similarly, when discussing delivery speed with managers focused on cost efficiency, mentioning frequent software releases might alarm them. To them, speed may translate into sloppiness. To bridge this gap, architects should articulate concepts like the cost of delay—how a six-month delay in deployment could cost millions. This approach translates time into monetary terms, overcoming mental barriers and helping to understand.

## Architects Make Better Decisions with Models

The world we're in is not simple. The applications we build today are complex because they are based on distributed systems, event-driven architectures, asynchronous processing, or scale-out and auto-scaling capabilities. While these are impressive capabilities, they add complexity.

Models are an architect’s best tool to tackle complexity. Models are powerful because they shape how people think. [Dave Farley](https://www.davefarley.net/) illustrated this with an example: long ago, people believed the Earth was at the center of the universe and this belief made the planets' movements seem erratic and complicated. The real problem wasn't the planets' movements but using an incorrect model. When you place the sun at the center of the solar system, everything makes sense.

Architects explaining things to others who operate differently may believe that others don't understand when they simply use a different mental model.

"All models are wrong, some are useful" is a famous quote by [George Box](https://en.wikipedia.org/wiki/All_models_are_wrong). However, we often overlook the second part of the statement:

> Just as the ability to devise simple but evocative models is the signature of the great scientist so overelaboration and overparameterization is often the mark of mediocrity.

In architecture, diagrams and complex tapestries like database schemas and system architectures are common. Yet, these models don't always offer much abstraction. The most powerful models are the simplest. A good model simplifies and abstracts, providing clarity rather than confusion.

When someone says, "But this model isn't reality," the answer should be: "It's not meant to be; it's a model. It clarifies our thinking, makes us smarter, and helps us make better decisions. It boosts our IQ." When architects try to capture reality—every detail in place–they fall into the trap of overelaboration highlighted by George Box.

How can we simplify a model? Let's use Planet Earth as an example, with four different models of it. Which one is the best? They are all good, depending on the question.

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/thinking-like-architect/en/resources/48figure-3-1721904839120.jpg)

<small><strong>Figure 3: The best models depend on the question</strong></small>

A model is only valuable if it helps answer something. Need to hike? Use a topographical map. Elections? A political map. A to B quickest? Route map. Density for logistics? Population map. Different questions require different maps. When asked, "Show me the architecture," it's fair to ask, "What's the question?" – different questions require different models.

## Architects See More Dimensions

Architects can make everyone else a bit smarter by seeing multiple dimensions. By expanding the problem and solution space, architects enable others to approach problems more intelligently. Often, disagreements arise when two parties view a problem from different angles, akin to debating between a square and a triangle without progress.

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/thinking-like-architect/en/resources/33figure-4-1721904839120.jpg)

<small><strong>Figure 4: Architects see more dimensions</strong></small>

Architects can introduce a third dimension, showing everyone that the object is a pyramid.  For example when teams debate between speeding up delivery and maintaining quality. Architects can suggest solutions like automating tests or integrating testing earlier, improving collective problem-solving capabilities beyond binary debates.

In the realm of IT strategy, many leaders grapple with the "between a rock and a hard place" dilemma. On one side, there's pressure to standardize and reduce costs for harmonization and economies of scale. This can limit flexibility, as developers may prefer other innovative solutions. On the other side, businesses demand agility and innovation to stay competitive. This creates a linear, unhappy scale where neither side finds satisfaction. Architects who recognize multiple dimensions can navigate these challenges more effectively.

[Platforms harmonize and standardize, but don't stifle innovation, they boost innovation](http://platformstrategybook.info/). This concept extends beyond IT into other industries, like automotive manufacturing in the 1970s when Volkswagen adopted platform models. By standardizing the underlying engineering in a common platform, they were able to diversify and innovate with multiple models. This approach didn't reduce variety, it increased it significantly. BMW has evolved from three car models to over 30 today.

Breaking away from one-dimensional thinking is vital: as automotive companies expand product diversity through platform standardization, so too should software architects. By adopting a multidimensional approach, they can balance quality and speed, and achieve both standardization and innovation with platforms. This shift from binary choices is crucial for architects seeking to maximize both efficiency and creativity.

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/thinking-like-architect/en/resources/24figure-5-1721904839120.jpg)

<small><strong>Figure 5: Overcoming constraints</strong></small>

Another example of adding dimensions relates to lock-in and switching costs, an unpopular topic among cloud vendors. Typically, the conversation revolves around the challenge of moving workloads between cloud providers, viewed traditionally as a single-dimensional problem: decisions often seem to involve choosing between evils without finding a middle ground. A better approach is to introduce multiple dimensions to the discussion.

For instance, lock-in and switching costs can be seen as potential liabilities, with the cost quantified, considering the probability and magnitude of such a move. On the other hand, there are benefits like serverless architectures and managed services that reduce operational complexity.

By considering benefits versus switching costs, it becomes possible to weigh the trade-offs more effectively, providing decision models rather than direct answers. This approach allows the customers to understand their unique constraints and goals better, empowering them to make informed decisions.

Customers say: I'm locked in. That sounds binary: the lock is either closed or open. It's not binary, it's a cost that can be higher or lower, with different dimensions.

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/thinking-like-architect/en/resources/17figure-6-1721904839120.jpg)

<small><strong>Figure 6: Multiple dimensions of lock-ins</strong></small>

Lock-in isn’t limited to switching vendors. AWS provides extended version support on EKS, its managed Kubernetes service. As an architect, you’d expect everyone to use the latest version, right? So why would you need such a service? The answer is simple: version upgrades also have a cost: you are locked into a specific version of an open-source project due to switching costs. Architects should understand these nuances and consider how to make upgrades more cost-effective.

## Architects Sell Options

Many view diversity and harmonization as opposites. Imagine developers have different preferences in programming languages. To harmonize these preferences, we, as IT and software architects, create standard APIs and interfaces, using protocols like OAuth and JSON. This approach is common in microservices architecture.

The key here is to standardize certain elements—such as interfaces, authorization protocols, transport methods, and data representation formats. By locking them down, architects sacrifice some options but gain others, enabling developers to code in various languages. Hohpe highlights the balance between harmonization and flexibility to allow for both innovation and standardization.

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/thinking-like-architect/en/resources/10figure-7-1721904839120.jpg)

<small><strong>Figure 7: Give up some options to gain others</strong></small>

To explain this trade-off, we can use metaphors, particularly from the financial sector. Standardizing certain elements to enable diversity is similar to trading options: software practitioners give up some options but gain others. These options, such as using different programming languages, moving workloads to another provider, scaling up systems, and adding hardware capacity, are valuable.

For those in finance, this concept is as clear as Kubernetes and Helm Charts are to IT professionals. Options pricing, for which the Black-Scholes model won a Nobel Prize, shows that having options is always valuable. Deferring decisions, such as adding capacity later with the cloud's elastic infrastructure and scale-out architecture, is a valuable option.

An important insight from this formula involves volatility: options become more valuable as unpredictability increases. For instance, with a consistent number of users, there is no need for elastic hardware. Launching instead a mobile app or e-commerce store with uncertain user numbers, the option to scale becomes crucial. The more uncertainty, the more valuable the option. This metaphor shows that in architecture, like in finance, uncertainty increases the value of options. Using metaphors like this makes complex ideas clearer and more convincing.

## Agile Architecture: Friend, Not Foe

Architecture and agile methodologies both thrive on dealing with uncertainty and volatility. If everything were predictable and unchanging, there would be little need for agility. Agile thrives on change and volatility, just like architecture: they both deal with uncertainty and are complementary. Using a car metaphor: agile is the steering wheel, guiding direction, while architecture is the engine, ensuring movement. Both are essential and work together. In a constantly changing world, both are needed to succeed. Architecture and agile are allies.

## Conclusions

In this article, we delved into the role of a software architect, reflecting on how architects think and make decisions. The topic of what architects are and do was discussed, focusing on the importance of finding suitable abstractions using models and metaphors.

The concept of an architect as an IQ amplifier was discussed, as well as the importance of understanding tradeoffs and navigating different options.

## About the Author

[![](https://cdn.infoq.com/statics_s1_20240807062301/images/profiles/cZM6r8HagaUEBk9g5RTpAPQz7286uPWP.jpg)](https://www.infoq.com/profile/Gregor-Hohpe/)

#### **Gregor Hohpe**

Gregor Hohpe helps technology leaders transform both their organization and their technology platform. You’ll find him riding the Architect Elevator from the engine room to the penthouse, perhaps automating serverless solutions in the morning and preparing board presentations in the afternoon. Gregor has served as Director at AWS and Google Cloud’s Office of the CTO, as Smart Nation Fellow to the Singapore government, and as Chief Architect at Allianz SE, where he oversaw the architecture of a global data center consolidation and deployed the first private cloud software delivery platform. Gregor is known as co-author of the seminal book _Enterprise Integration Patterns_, which provided the reference vocabulary for all modern ESBs. His book _The Software Architect Elevator_ tells stories from the trenches of IT transformation.

Show more
