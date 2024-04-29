---
created: 2023-10-09T08:51:42 (UTC -03:00)
tags: []
source: https://jeremydmiller.com/2012/10/11/test-with-the-finest-grai/
author: jeremydmiller
---

# Jeremy’s Only Rule of Testing – The Shade Tree Developer

> ## Excerpt
> Test with the finest grained scenario that tells you something important Years ago I wrote a series of blog posts describing “Jeremy’s Laws of Test Driven Development” (1, 2, 3, a…

---
Years ago I wrote a series of blog posts describing “Jeremy’s Laws of Test Driven Development” ([1](http://codebetter.com/jeremymiller/2005/10/21/haacked-on-tdd-and-jeremys-first-rule-of-tdd/), [2](http://codebetter.com/jeremymiller/2006/03/09/jeremys-second-law-of-tdd-push-dont-pull/), [3](http://codebetter.com/jeremymiller/2006/05/30/achieve-better-results-by-following-jeremys-third-law-of-tdd-test-small-before-testing-big/), and [4](http://codebetter.com/jeremymiller/2007/04/27/jeremys-fourth-law-of-test-driven-development-keep-your-tail-short/)) describing what I thought were some important coding and design rules to be more successful while using TDD.  I still believe in the thinking behind all those silly “laws,” but I now I would say that all of that writing is a manifestation of lower level first causes in successful software development — namely the extreme importance of quality feedback in your software efforts.

Consider this thought:  every single line of code you write, every thought you have about the user experience, the business rules, the design you intend to use, and the assumptions about the system’s usage you’re making are potentially wrong — but often wrong in subtle, hard to notice ways.  My experience is that my projects have gone much better when my team and I are able to work in tight cycle times with solid feedback mechanisms that constantly nudge us towards better results.

With that in mind, I’ve boiled down my old personal rules for using TDD into a single,  lower level rule to maximize the effectiveness of the feedback my team gets from testing:

> _Test with the finest grained mechanism that tells you something important_

Since both the quantity and quality of your testing feedback matters, here’s a pair of examples from my new job that illustrate how this rule can guide your approach.

**Scenario #1:  Use a tighter feedback loop**

A couple weeks ago, I watched one of my new colleagues troubleshooting an issue with one of our phone helpdesk applications.  The call waiting elevator music wasn’t playing or switching off at the right time, and you know how annoying that can be.  My colleague had to work by kicking off the process by first making a call with the world’s lamest looking 90’s era cellphone and then stepping through the code manually until he was able to find the faulty logic in our system.  The problem turned out to be in the coordination logic written by my company and not in the 3rd party phone control software.

The fault definitely lies with the design of that code, but my colleague and I were violating my little testing rule because we were forced to use an unnecessarily slow and cumbersome feedback cycle.  What if instead, the code had been structured in a such a way that we could write narrowly focused unit test nothing but the logic that decided when to turn the call waiting music on and off.  That very narrowly focused, very fast running unit test could have told my colleague something valuable, namely that the if/then coordination logic was all wrong — all without having to look terminally uncool using the cheap 1990’s looking cell phone.  Add in the number of times we had to repeat the process to track down the problem and then to verify that the fix was correct and the finer grained tests look even better.

**Scenario #2:  Sometimes a unit test is useless**

I had a conversation the other day with a different colleague asking me if he’d be able to write a unit test in [Jasmine](https://github.com/pivotal/jasmine) for the code we’ll need to write that configures event handling and options in a [SlickGrid](https://github.com/mleibman/SlickGrid) table embedded in our application.  Applying my rule again, this proposed testing mechanism is a very tight feedback loop, but the test just doesn’t tell me anything useful.  I can assert all day long that I’m calling methods and setting properties on the SlickGrid JavaScript object, but that doesn’t tell me whether or not the grid behaves the way that we want it to when the application is running.  In this case, we have to go to a more coarse grained integration test that works against the user interface.

**Making testing more useful**

What’s the purpose of testing in your daily job?  Is it to certify that the software works exactly the way it’s supposed to?  What if instead we shifted our thinking about testing to focus on removing flaws and risk from our software project?  That might seem like a subtle restating of the same goal, but it can drastically change how your team or organization approaches software testing.

If your goal is to verify that the system works correctly, you’re probably more likely to focus on [black box testing](http://en.wikipedia.org/wiki/Black-box_testing) of the system in realistic scenarios and environments because that’s the only real way to know that the system really does work.  In that approach you probably have some formal separation between the developers and the testing team — again to guarantee that you have a completely independent appraisal of the code.

On the other hand, if you’re using testing as a way to remove defects and risk, I think you’re much more likely to follow a testing philosophy similar to my rule about tighter feedback loops, which I think inevitably leads to an emphasis on [white box testing](http://en.wikipedia.org/wiki/White-box_testing) solutions and fine-grained unit testing backed up with some minimal black box testing.  If you’re not familiar with the term “white box testing,” it means taking advantage of a detailed knowledge of the system internals in your testing.  I’m sure that it can be done otherwise, but I wouldn’t even begin to try to use white box testing without a very deep synergy and a high degree of collaboration between developers and testers.  In this approach, I think you’d be foolish to keep your developers and testers formally separated.

**… and lastly, a brief aside about mocking**

I once wrote that you [shouldn’t mock interfaces outside of your own codebase](http://codebetter.com/jeremymiller/2006/01/10/best-and-worst-practices-for-mock-objects/) or chatty interfaces.  Taking the two examples above, doing an assertion that a message was sent to “TurnOffCallWaiting()” or “TurnOnCallWaiting()” is useful in my opinion.  I certainly have to test the real code behind the “TurnOn/Off()” methods, but I will happily use interaction testing against this kind of goal-oriented interface.

Moving to my second scenario, doing mock object assertions that I fiddled a lot of fine-grained “beforeBeginCellEdit” and “invalidateRow()” methods when I really just care that the data in a row in an html table was updated?  Not so much.

If you do need to interact with any kind of chatty, low level API — especially if it’s in a 3rd party library or tool — I think you’re much better off to wrap a [gateway interface](http://martinfowler.com/eaaCatalog/gateway.html) around that API that’s expressed in the semantics of your goals for that API like “TurnOffCallWaithing().”
