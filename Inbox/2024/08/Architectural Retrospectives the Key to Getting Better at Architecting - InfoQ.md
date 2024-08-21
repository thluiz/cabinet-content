---
created: 2024-08-21T15:13:56 (UTC -03:00)
tags: [architectural retrospectives,Architecture & Design,Retrospectives,Assessment,Continuous Improvement,Minimum Viable Architecture,Architecture,Agile,]
source: https://www.infoq.com/articles/architectural-retrospectives/?ref=dailydev
author: Pierre Pureur, Kurt Bittner
---

# Architectural Retrospectives: the Key to Getting Better at Architecting - InfoQ

> ## Excerpt
> The purpose of an architectural retrospective is to use experience to help the development team improve their architecting skills and their way of working as they make architectural decisions.

---
### Key Takeaways

-   Architectural Retrospectives differ from Architectural Reviews by focusing not on evaluating and improving the architecture itself, but on examining and improving how the team went about creating the architecture.
-   Software architecture is defined by the decisions the development team makes about architecturally significant issues. Architectural Retrospectives focus specifically on how the team made these decisions and looking for ways to improve the way it makes decisions. 
-   One challenging thing about retrospectives is to not spend too much time talking about the decisions themselves. The specific decisions are not the focus of a retrospective.
-   Teams must also be careful not to turn retrospectives into “blame sessions”. Instead, they need to look for biases that may be impairing their ability to make good decisions. 
-   Teams also cannot fault themselves for not knowing something that they learned by building and testing some part of the architecture. Retrospectives should instead focus on whether the team is asking the right questions as they form their architectural experiments.  
     

![](https://imgopt.infoq.com/fit-in/3000x4000/filters:quality(85)/filters:no_upscale()/articles/architectural-retrospectives/en/resources/17Picture1-1722355617181.jpg)

An oft-cited definition of insanity is doing the same thing and expecting different results, but that’s exactly what teams who never examine their way of working do. Designing a software architecture is no exception: if you never stop to examine how you make decisions, your results may reflect systemic biases in your ways of looking at the problem.

Software architecture reviews are important, but they are not a substitute for retrospectives, for reasons we will explore. Reviews focus on the product and its architecture, but not the way that the decisions that shape the architecture were made, or the way that the team works to realize those decisions. [Retrospectives](https://www.scrum.org/resources/what-is-a-sprint-retrospective) are a key element of many agile software development approaches for a good reason: they provide time and psychological space for the team to reflect on how it can improve its way of working. In the rest of this article, we will explore how and why.

## Architecture retrospectives are different from architecture reviews

#### Related Sponsored Content

-   ##### [\[eBook\] Cloud Portability: Building a Cloud-Native Architecture](https://www.infoq.com/url/f/4082004f-18c3-46ab-9088-5ab021bc0825/?ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Anatomy of an LLM](https://www.infoq.com/vendorcontent/show.action?vcr=2d401b86-a294-43b3-b038-0ae6116d9689&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Farchitectural-retrospectives%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [8 Deployment Pattern Structures to Transform your CI/CD](https://www.infoq.com/vendorcontent/show.action?vcr=cd6f683e-d399-40fa-8794-e8fb2a79a5c4&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Farchitectural-retrospectives%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Relational Restrictions: How to Select the Right Database Technology](https://www.infoq.com/vendorcontent/show.action?vcr=9e8f4929-3d3a-4005-b4c0-ec09321b8836&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Farchitectural-retrospectives%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Evolving the Agile Organization with Evidence-Based Management](https://www.infoq.com/vendorcontent/show.action?vcr=0ce62b6c-ed5d-471b-be3f-631b28087232&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Farchitectural-retrospectives%2F%3Fref%3Ddailydev&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    

The purpose of architectural reviews is to improve the architecture. In the context of the [MVA approach](https://www.infoq.com/articles/minimum-viable-architecture) we have described in other articles, an Architecture Review is focused on assessing the fitness for the purpose of the architectural increment (MVA) in supporting the incremental product MVP.

Because the entire purpose of working incrementally is to gather feedback that can be used to improve the product (MVP) and its associated architecture (MVA), architectural reviews are essential and should be conducted every increment (e.g. Sprint, in Scrum terms).

This is in contrast to architectural reviews in a traditional approach, which are rarely conducted until it’s largely too late. Then they become a kind of post-mortem after a major incident, whose purpose is to determine what, if anything, can be done to address problems experienced in the incident. The traditional architectural review, especially if conducted by outside parties, often turns into a blame-assignment exercise. The whole point of regular architectural reviews in the MVA approach is to learn from experience so that catastrophic failures never occur.

The architectural retrospective has a different purpose: it’s focused on using experience to help the development team improve their architecting skills and their way of working as they make architectural decisions. Architectural retrospectives are opportunities for the development team to reflect on what they can do better in the future, and are also useful during every iteration/Sprint, for there are almost always things the team can learn about how they work.

A comparison and contrast of reviews and retrospectives helps to clarify the differences (see Table 1).

<table><tbody><tr><td><small><strong>Dimension</strong></small></td><td><strong><small>Architectural Review</small></strong></td><td><strong><small>Architectural Retrospective</small></strong></td></tr><tr><td><small>Purpose&nbsp;&nbsp;&nbsp;</small></td><td><small>To improve the product’s architecture (MVA)</small></td><td><small>To improve the team’s way of working and effectiveness in developing the Minimum Viable Architecture (MVA).</small></td></tr><tr><td><small>Decisions/Trade-offs</small></td><td><small>Are the decisions and trade-offs we have made still acceptable?&nbsp;</small></td><td><small>Is the process by which the team makes architectural decisions effective? Is it based on empirical evidence gained from experiments?</small></td></tr><tr><td><small>QARs&nbsp;</small></td><td><small>Are the QARs complete/correct?&nbsp;&nbsp;</small></td><td><small>Is our process for finding QARs adequate? Are they based more on guesses and biases rather than objective facts?</small></td></tr><tr><td><small>Frequency&nbsp;&nbsp;&nbsp;&nbsp;</small></td><td><p><small>In classical projects, very infrequently, usually when something goes wrong (i.e. "blame session").</small></p><p><small>In an MVA approach, every iteration/Sprint.</small></p></td><td><p><small>In a classical approach, almost never.</small></p><p><small>In an MVA approach, every iteration/Sprint.</small></p></td></tr><tr><td><small>Skills&nbsp;&nbsp;&nbsp;</small></td><td><small>Not at all; the review is focused on the product/system.&nbsp;&nbsp;</small></td><td><small>Are the skills represented on the team adequate to develop our product/system? Does the team need to improve their skills in some area?</small></td></tr><tr><td><small>Feedback loops</small></td><td><small>Used to improve the MVA.&nbsp;</small></td><td><small>Used to improve the act of architecting.</small></td></tr><tr><td><small>Technical Debt (TD)&nbsp;&nbsp;&nbsp;</small></td><td><small>Measure TD and make decisions about how/if it should be "repaid"/reduced.</small>&nbsp;&nbsp;&nbsp;</td><td><small>Ask questions about what the team is doing to cause TD to increase/decrease, and whether that’s helping the team achieve an effective architecture.</small></td></tr></tbody></table>

**Table 1: Comparing architectural reviews with architectural retrospectives**

## Retrospectives should be separate from reviews

If you already conduct a retrospective event like the Sprint Retrospective in Scrum, you can simply make time in that event to ask critical questions about how you can improve your architectural way of working.

If you don’t already have a retrospective event, you can start by making time to ask critical questions about your way of working through architectural issues. This will probably expand to general issues about your team’s way of working, and that’s ok; we think both are important even though this article has focused on retrospecting on the architecture.

You might be tempted to add a retrospective section to your architecture review, but we recommend against this; there is a reason why Scrum, for example, has separate events for the [Sprint Review](https://www.scrum.org/learning-series/sprint-review/) and the [Sprint Retrospective](https://www.scrum.org/learning-series/sprint-retrospective/):

-   The audience for a review includes people from outside the team who won’t have perspective on how the team works, and may not care how the team works. If you start talking about things they don’t know or care about, you’re not respecting their time and they may not come to future reviews.
-   Developers may not feel comfortable talking about problems in their way of working with outsiders present. As a result, you won’t uncover important improvement opportunities without openly discussing the challenges the team is experiencing.
-   In most cases it’s worthwhile for developers to have some time to reflect after the architectural review. When an architectural review exposes problems in the architecture, it’s natural for everyone to focus on fixing that problem. Sometimes the root causes of problems are hidden in the way the team works or make decisions; addressing specific issues often doesn’t uncover these. Having a retrospective separate from the review gives them time to reflect and understand root causes.

This separation of concerns (adapted from, among other things, Scrum) is useful because it helps to focus attention. Reviewing the product or its architecture is complex enough without broadening the scope of the review, and having a special focus on the way of working helps the team to improve.

## Questions to ask in your retrospectives

The mechanics of running an architectural retrospective session are identical to those of running a [Sprint Retrospective in Scrum](https://www.scrum.org/learning-series/sprint-retrospective/). In fact, an architectural focus can be added to a more general-purpose retrospective to avoid creating yet another meeting, so long as all the participants are involved in making architectural decisions. This can also be an opportunity to demonstrate that anyone can make an architectural decision, not only the "architects." Adding architectural focus means asking different kinds of questions about how your team makes architectural decisions:

-   **Look at  how your QARs were established**. Were they guesses? Are they accurate (maybe they were copied from some other system and aren’t appropriate)? Are they useful? Are they overly restrictive? Which assumptions did we make? Does the team’s [Definition of Done](https://www.infoq.com/articles/definition-of-done-mva/) reflect QARs?
-   **Look at the way that you arrived at your decisions**. Was the whole team involved in making decisions or did the most senior individual influence the decisions? Are they supported by empirical evidence gained from running experiments (i.e. developing and testing an MVA)? Were decisions based on experience? Are there systematic biases in our cognitive or decision-making processes that are preventing us from achieving a good result?
-   **Look at the way you use feedback**. Are you measuring the effectiveness of your decisions? Have you ever reversed a decision because of new information? Have you eliminated work that you had on the backlog because feedback has made it irrelevant?
-   **Focus more deeply on trade-offs**. Were they effective, based on what you have learned from your MVA "experiments"? Did they meet the intended need or fall short?
-   **Review technical debt**. Is technical debt growing? (of course it is…) Is that ok, or is the team simply compounding (pun intended) future problems?
-   **Consider how you use feedback to improve your architecture**. Are you able to release the system in small MVP/MVA increments? Are you comfortable with doing experiments that may fail? Do you create MVAs that both support the MVP and test the validity of your decisions and trade-offs? Are you able to use the feedback from releasing an MVA to improve future MVAs?
-   **Look at the skills of your team**. Does your team have the technical skills, expertise, and experience to develop effective MVAs? Are there skills you need to improve or acquire on the team? Do you cling to what you know because you can’t make the time to learn new technologies?

The most important thing to remember in any retrospective is that the purpose of the retrospective is to find concrete ways to improve the way the team works, in the form of things the team can try. The purpose is not to criticize or to assign blame; when this happens the retrospectives become useless and even damaging. Teams only learn by building and testing architectural increments (MVAs) as not every idea will work out as hoped. Building and testing ideas is essential to trying new approaches and making better decisions.

## How frequently should a team retrospect?

Some people will say that it’s not necessary to retrospect every time an MVA is created (e.g. every Sprint in Scrum) because they feel it will take too much time. We think it is worth asking yourselves the questions noted above after every MVP/MVA release - if there are no interesting answers then the retrospective will be quite short and you can move on. If there are interesting answers, however, it’s worth spending some time to examine the way that you make architectural decisions before you launch into building your next incremental MVP/MVA. It will save you time and spare you grief in the long run.

## Conclusion

Retrospectives help teams to improve their way of working while reviews help teams to improve the product on which they are working. Reviews don’t take the place of retrospectives any more than retrospectives take the place of reviews.

Many teams skip retrospectives because they don’t like to confront their shortcomings, Architectural retrospectives are even more challenging because they examine not just the way the team works, but the way the team makes decisions. But architectural retros have great pay-offs: they can uncover unspoken assumptions and hidden biases that prevent the team from making better decisions. If you retrospect on the way that you create your architecture, you _will_ get better at architecting.

## About the Authors

[![](https://cdn.infoq.com/statics_s2_20240821064811/images/profiles/22c27753414b3827cb473ede92e13489.jpeg)](https://www.infoq.com/profile/Pierre-Pureur/)

#### **Pierre Pureur**

Pierre Pureur is an experienced software architect, with extensive innovation and application development background, vast exposure to the financial services industry, broad consulting experience and comprehensive technology infrastructure knowledge. His past roles include serving as Chief Enterprise Architect for a major financial services company, leading large architecture teams, managing large-scale concurrent application development projects and directing innovation initiatives, as well as developing strategies and business plans. He is coauthor of the books "Continuous Architecture in Practice: Scalable Software Architecture in the Age of Agility and DevOps"(2021) and "Continuous Architecture: Sustainable Architecture in an Agile and Cloud-Centric World" (2015) and has published many articles and presented at multiple software architecture conferences on this topic. 

Show more

[![](https://cdn.infoq.com/statics_s2_20240821064811/images/profiles/3789fc0e548a79744225f15083ab938c.jpg)](https://www.infoq.com/profile/Kurt-Bittner/)

#### **Kurt Bittner**

Kurt Bittner has more than 30 years of experience delivering working software in short, feedback-driven cycles. He has helped a wide variety of organizations adopt agile software delivery practices, including large banking, insurance, manufacturing, and retail organizations, as well as large government agencies. He has worked for or with large software delivery organizations including Oracle, HP, IBM, and Microsoft, and is a former technology industry analyst with Forrester Research. His focus is on helping organizations build strong, self-organizing, high-performance teams that deliver solutions that customers love. He is the author of four books on software development-related topics, including The Nexus Framework for Scaling Scrum. He is based in Boulder, Colorado and serves as VP of Enterprise Solutions for Scrum.org.

Show more
