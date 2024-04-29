---
created: 2023-10-09T08:30:44 (UTC -03:00)
tags: []
source: https://jeremydmiller.com/2023/09/14/notes-on-teaching-test-driven-development/?ref=dailydev
author: jeremydmiller
---

# Notes on Teaching Test Driven Development – The Shade Tree Developer

> ## Excerpt
> JasperFx Software has several decades worth of experience with Test Driven Development, developer focused testing, and test automation in general. We’re more than happy to engage with potenti…

---
[![](https://jeremydmiller.files.wordpress.com/2023/09/red-green-refactor.png?w=1024)](https://jeremydmiller.files.wordpress.com/2023/09/red-green-refactor.png)

_[JasperFx Software](https://jasperfx.net/) has several decades worth of experience with Test Driven Development, developer focused testing, and test automation in general. We’re more than happy to engage with potential clients who are interested in improving their outcomes with TDD or automated testing!_

Crap I feel old having typed out that previous sentence.

I’m going through an interesting exercise right now helping a [JasperFx](https://jasperfx.net/) client learn how to apply Test Driven Development and developer testing from scratch. The developer in question is very inquisitive and trying hard to understand how best to apply testing and even a little TDD, and that’s keeping me on my toes. Since I’m getting to see things fresh from his point of view, I’m trying to keep notes on what we’ve been discussing, my thoughts on those questions, and the suggestions I’ve been making as we go.

The first things I **should** have stressed was that the purpose of your automated test suite is to:

1.  Help you know when it’s safe to ship code — not “your code is perfect” but “your code is most likely ready to ship.” That last distinction matters. It’s not always economically viable to have perfect 100% coverage of your code, but you can hopefully do enough testing to minimize the risk of defects getting past your test coverage.
2.  Provide an effective feedback loop that helps you to modify code. And by “effective,” I mean that it’s fast enough that it doesn’t slow you down, tells you useful things about the state of your code, and it’s stable or reliable enough to be trusted.

Now, switching to [Test Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html) (TDD) itself, I try to stress that TDD is primarily a low level design technique and an important feedback loop for coding. While I’m not too concerned about whether or not the test is written first before the actual code in all cases, I do believe you should consider how you’ll test your code upfront as an input to how the code is going to be written in the first place.

### Think about Individual Responsibilities

What I absolutely did tell my client was to try to approach any bigger development task by first trying to pick out the individual tasks or responsibilities within the larger user story. In the first case we were retrofitting tests to, it was a pretty typical web api endpoint that:

-   Tried to locate some related entities in the database based on the request
-   Validated whether the requested action was valid based on the existence and state of the entities
-   On the happy path, make a change to the entity state
-   Persist the changes to the underlying database

In the case above, we started by focusing on that validation logic by isolating it into its own little function where we could easily “push” in inputs and do simple assertions against the expected state. Together, we built little unit tests that exercised all the unique pathways in the validation including the “happy path”.

Even this little getting started exercise potentially leads to several other topics:

-   The advantage of using [pure functions](https://en.wikipedia.org/wiki/Pure_function) for testable code whenever possible
-   Purposely designing for testability ([as I wrote about way back in 2008](https://learn.microsoft.com/en-us/archive/msdn-magazine/2008/december/patterns-in-practice-design-for-testability)!)
-   In our case, I had us break the code apart so we could start in a “bottom up” approach where we coded and tested individual tasks before assembling everything together, versus a top down approach where you try to code the governing workflow of a user story first in order to help define the new API calls for the lower level tasks to build after. I did stress that the bottom up or top down approach should be chosen on a case by case basis.

When we were happy with those first unit tests, we moved on to integration tests that tested from the HTTP layer all the way through the database. Since we had dealt with the different permutations of validation earlier in unit tests, I had us just write two tests, one for the happy path that should have made changes in the database and another “sad path” test where validation problems should have been detected, an HTTP status code of 400 was returned denoting a bad request, and no database changes were made. These two relatively small tests led to a wide range of further discussions:

-   Whither unit or integration testing? That’s a small book all by itself, or at least a long blog post like [Jeremy’s Only Rule of Testing](https://jeremydmiller.com/2012/10/11/test-with-the-finest-grai/).
-   I did stress that we weren’t even going to try to test every permutation of the validation logic within the integration test harnesses. I tried to say that we were trying to create just enough tests that worked through the execution pathways of that web api method that we could feel confident to ship that code if all the tests were passing
-   Watch how much time you’re needing to spend using debugging tools. If you or your team is finding yourself needing to frequently use debuggers to diagnose test failures or defects, that’s often a sign that you should be writing more granular unit tests for your code
-   Again with the theme that it’s actually inefficient to be using your debugger too much, I stressed the importance of trying to push through smaller unit tests on coding tasks before you even try to run end to end tests. That’s all about trying to reduce the number of variables or surface area in your code that could be causing integration test failures
-   And to not let the debugging topic go quite yet, we did have to jump into a debugger to fix a failing integration test. We just happened to be using the [Alba](https://jasperfx.github.io/alba) (one of the JasperFx OSS libraries!) library to help us test our web api. One of the huge advantages of this approach is that our web application is running in the same process as the test harness, so it’s very quick to jump right into the debugger by merely re-running the failing test. I can’t stress enough how valuable this is for faster feedback cycles when it inevitably comes time to debug through breaking code as opposed to trying to troubleshoot failing end to end tests running through user interfaces in separate processes (i.e. Selenium based testing).
-   Should unit tests and integration tests against the same code be in the same file or even in the same project? My take was just to pay attention to his feedback cycle. If he felt like his test suite ran “fast enough” — and this is purely subjective — keep it simple and put everything together. If the integration tests became uncomfortably slow, then it might be valuable to separate the two poles of tests into the “fast” and “slow” test suites
-   Even in this one test, we had to set up expected inputs through the actual database to run end to end. In our case, the data is all identified through globally unique identifiers, so we could add all new data inputs without worrying about needing to teardown or rebuild system data before the test executed. We just barely started a discussion about [my recommendations for test data setup](https://jeremydmiller.com/2013/01/26/my-opinions-on-data-setup-for-functional-tests/).

_As an aside, JasperFx Software strongly feels that overusing Selenium, Playwright, or_ _[Cypress.io](http://cypress.io/)_ _to primarily automate testing through browser manipulation is potentially very inefficient and ineffective compared to more balanced approaches that rely on smaller and faster, intermediate level integration tests like the Alba-based integration testing my client and I were doing above._

### “Quick Twitch” Working Style

In the end, you want to be quick enough with your testing and coding mechanics that your progress is only limited by how fast you can think. Both my client and I use JetBrains Rider as our primary IDE, so I recommended:

-   Get familiar with the keyboard shortcuts to run test, re-run the last test, or re-run the last test in the debugger so that he could mechanically execute the exact test he’s working on faster without fumbling around with a mouse. This is all about just being able to work as fast as you can think through problems. Other people will choose to use continuous test runners that automatically re-run your tests when file changes are detected. The point either way is just to reduce your mechanical steps and tighten up the feedback loop. Not everything is a hugely deep philosophical subject:-)
-   Invest a little time in micro-code generation tooling like [Rider’s Live Template](https://www.jetbrains.com/help/rider/Creating_and_Editing_Live_Templates.html) feature to help build repetitive code structures around unit tests. Again, the point of this is just to be able to work at the “speed of thought” and not burn up any gray cells dealing with mundane, repetitive code or mouse clicking

**Published** September 14, 2023

## Post navigation
