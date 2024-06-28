---
created: 2024-06-27T17:31:08 (UTC -03:00)
tags: [codereview,productivity,software,coding,development,engineering,inclusive,community]
source: https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev
author: 
---

# How to review as a Pro - DEV Community

> ## Excerpt
> There is some dispute about whether it is worth having a code review as a step inside the development...

---
There is some dispute about whether it is worth having a code review as a step inside the development pipeline, is there more harm than good at adopting this practice?  
I personally believe that good code review can add a lot to our work, both from the team's perspective and for career and self-development. This explanation will show how to get the most benefit from this process.

## [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#what-not-to-review)What not to review

It’s important to know not only about good practices, but also about things to avoid. Sometimes, It could even be better not to review code at all and replace it with some other practices like architectural review, pair or XP programming, than to have a step in a process that will damage team relationships.

### [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#code-style)Code style

Many people struggle with finding as many “spelling” mistakes as they can during code review, arguing over the number of spaces in indent, line breaks and other language syntactic sugar.  
That's not the right approach. Specific code style is essential , but it doesn’t need to be done manually. What you need (among other things) is to automate this routine and boring process. As Rob Pike said at his closing [talk](https://commandcenter.blogspot.com/2024/01/what-we-got-right-what-we-got-wrong.html) at GopherConAU conference: _every language worth using has a standard formatter_.  
There are lots of linters for different program languages:

-   Java (built-in tools in IntelliJ or Eclipse)
-   Python (ruff, black, pyllint, flake8)
-   Go (Gofmt - default code formatter)

Not only can you check how well your code follows certain rules but also add auto-formatting options. This way, most inconsistencies are fixed automatically, saving time.  
Linters start up can be automated, for example, with [pre-commit tool](https://pre-commit.com/) for python and this logic can be integrated into GitHub PR or new commits workflow.  
So before a new Pull Request is reviewed, it's the author who resolves any code style issues.

### [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#attitude)Attitude

This paragraph is not about what, but about how, or rather how not to do a review. Some people treat code reviews as a challenge to uncover as many mistakes as and faults as possible. Even with the best intentions, this approach can make the process more personal than it should be. When work is taken too close to heart there’s a risk of negative and strong emotions that can harm our attitude towards colleagues and lead to rushed or wrong decisions.  
A PR review is not a place to compete, you do not need to win a battle of corrections. It’s a place to clear up questionable aspects of the implementation, better understand what’s happening, learn something new, or help the code's author discover new things.

## [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#what-to-review)What to review

What useful things can be done? After minimising any harm, it’s time to focus on productive approaches to code review.  
Everything I write below will work for us in the future. These practices will make it easier to understand and maintain the codebase by yourself later on if everyone who's touched it gets hit by a bus.

### [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#logic)Logic

One of the main goals of a review is to understand what is going on in this code and whether it meets the goal of an initial task. If you can't understand what the hell is going on, it could mean a couple of things:

-   The code is too complex logically or has too many layers of abstraction. In the future, this will make it harder to add more logic or understand the code's original purpose, increasing the time needed to fix related problems.
-   If this code horrifies you now, imagine how you'll feel when the responsibility to change this block suddenly falls on you because the original author is no longer available.
-   When we don't know the goal or reason for the code changes, how can we understand if the implementation is correct? So, before the review, we need to understand what was the exact intent of the author. Yes, this will increase the time spent on someone else’s task, taking time away from your own more interesting work. But software development is about collective efforts and communication. With the development of AI Coding Co-pilots we have a need to read and review code much frequently.

### [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#language-usage)Language usage

A linter can’t solve all our problems with incorrect or non-idiomatic usage of language constructs. There are some supporting tools for this purpose, like [refurb](https://github.com/dosisod/refurb) for Python. However, these tools usually only help with simple concepts.  
It's important to follow good practices, and mentor your colleagues to do so. Like not storing passwords and credentials in your code.  
In the age of fast developing AI-tools, there certainly will be better instruments to help us with these tasks. For now, we still need to check suggested options for correctness and can’t blindly trust any information provided by LLM tools. We certainly can ask ChatGPT about some things, but sometimes you just need to know what to ask about before compiling a question. So knowledge sharing is still in demand.

## [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#how-to-review)How to review

One common argument against code review is that it slows down the development process. But bug fixing and releasing hot fixes to production also slows down features development. It's up to us to create a process that minimises harm. Although it depends on a company and team structure, the process can be flexible.

-   Agree on a timeframe for code reviews and establish conventions for skipping review in certain cases, with agreement from all team members.
-   Set fixed time slots for reviewing others' code instead of writing your own. This can help manage long review queues and add structure to the work routine. Also, you will have some time milestones to pause the review process and take a break.
-   Break down your review into parts: start with higher-level logic of the code, and then move to individual functions. Reviewing by commits rather than by files can also be more effective.

But if you are strongly against code review practice, who am I to tell you what to do? Code review could be replaced by architectural review, pair or XP programming.

Personally, I had a few problems with my review comments. They could be perceived as quite harsh and emotional, though I didn't mean them that way. Often, my comments were written in an imperative tone, lacking polite and suggestive language. One technique that helped change this situation was a [conventional comments](https://conventionalcomments.org/) approach.  
A comment is marked by a keyword like `question`, `issue`, `remark`, `suggestion`, or any others that seem appropriate. This way, the code author can understand the severity of your notes and decide what definitely needs to be changed and what can stay as it is.  
For me, this tag also worked like magic, changing the narrative. When I write down a `question`, I encourage myself to expand on the initial idea further in text.  
The more explicitly and neutrally you write your comments, the better response you will get from the code creator.

Also, if you see convenient or effective code implementation from a less experienced colleague, don’t be afraid to point it out. Positive feedback is rare in such a small feedback loop. Yes, people are different, but most appreciate recognition for their work.

### [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#how-to-help-reviewers)How to help reviewers

It’s well known that many people don't like to review other codes. In my personal opinion, this anticipation comes from the thought: 'Oh no, I have to sort through someone else's messy code'.  
We can take a few simple steps to make code reviews less frustrating.

-   Write a description for the PR, so it's easy to understand the main goal or what’s happening inside.
-   Add links to the task issue in the tracker, if you have one, so the reviewer can understand the initial task deeper.
-   Try to divide the pull request into logical commits, so each commit can be checked separately.
-   Avoid huge PRs. Sometimes, the task can be split into smaller PRs that are easier to review. Other times, it's helpful to have regular architecture or code review sessions to understand the current task and discuss next steps of implementation.

The first two points also help leave a digital paper trail trail. The last ones add more structure to work. You can even separate commits after the code is written. Thus, we can look from the upper abstract layer and check that we do not forget any important part of our initial plan.

Sometimes, it helps to take a pause after creating a pull request and check it quickly from an outsider's perspective. This break can reveal obvious mistakes that can be fixed quickly.

## [](https://dev.to/nadia/how-to-review-as-a-pro-59a0?ref=dailydev#ending)Ending

I approach the review process with the aim of gaining knowledge — about the product, new features, or programming language. It’s also an opportunity to help colleagues avoid making some tricky mistakes. Questions that arise during reviews can lead to better understanding of our codebase or highlight areas that need rewriting and simplification.  
A thorough review would be very profitable when time passes and you need to refactor that part of code. Lately, I've been refactoring some quite ugly solutions that I could have insisted on reworking half a year ago during the code review stage (the last line of defence before merging to master). But hindsight is 20/20, and my foresight wasn't strong enough, so I'm paying the price now.

PS: I want to thank the reviewers of this article. Without theirs, the help text would be worse, and I wouldn't have gained new knowledge of the English language ❤️
