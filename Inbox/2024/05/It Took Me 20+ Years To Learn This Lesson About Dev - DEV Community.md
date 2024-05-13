---
created: 2024-05-08T14:41:59 (UTC -03:00)
tags: [beginners,programming,productivity,career,software,coding,development,engineering,inclusive,community]
source: https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep
author: Matt Butcher
---

# It Took Me 20+ Years To Learn This Lesson About Dev - DEV Community

> ## Excerpt
> I started my software development career as a web developer. I worked at companies large and small,...

---
I started my software development career as a web developer. I worked at companies large and small, including stints at HP and Google. At Microsoft, I was a principal engineer. I left to become co-founder and CEO of Fermyon.

I’m not one to dispense advice often. Our individual experiences are varied, and the anecdotal evidence of one person’s experience rarely generalizes well. But having made the same mistake over and over again in my career (and having observed others do the same), I feel like I have one good piece of advice to share:

> Remember that every line of code you write is a line of code you will support.

This maxim carries more inside of it than appears at first glance. I’ll share what it says about:

-   Coding as artisanship
-   Not Invented Here Syndrome
-   The cost of complexity
-   Reducing future needs for support
-   And why your future you (and others) will thank you for the code you’re producing today

## [](https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep#coding-as-artisanship)Coding as artisanship

Let’s start with an optimistic application of the maxim. Artisanship is a word perhaps often associated with bygone eras. It implies dedication to a craft, where mastery emerges over time and by virtue of hard work and frequent activity. Unlike the term “engineering,” which suggests a practice heavy on planning, artisanship implies a hands-on effort. Producing code, with its debugging and testing and refactoring, is a hands-on effort.

Building good software requires skill, knowledge, and a continual desire to improve. As you build your code, take time to do it well. Yes, “perfection is the enemy of the good,” and you need to code to get the job done. But following good coding conventions, naming variables well, and thinking through the problem you are solving are all practices that will make it easier to maintain your code over the longer term. In short, when you approach coding as a hands-on process resulting in mastery acquired over time, you are an artisan.

It’s a pain to support poor code. It is intellectually stimulating to maintain good code. Artisanship focuses us on producing good code as part of our daily practice.

## [](https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep#not-invented-here-is-your-enemy)"Not Invented Here" is your enemy

We all know the feeling. Sure, there’s a library or tool that does this. But you can do it better!

Every software engineer has done this at least once. It 15 years of my career before I finally learned this lesson. I rewrote everything from template engines to low-level data structures. And I always had some kind of justification:

-   Sure, there were other libraries, but they were too big/bloated/complicated/special purpose…
-   I had a novel way of doing things that other people hadn’t thought of
-   I didn’t trust/know/feel comfortable with someone else’s library
-   I’d just read a book/article/blog post that inspired me
-   I could learn more by doing it myself than by using someone else’s (true, but at a cost far greater than I realized)
-   I didn’t have time to look

In all of these cases, I discounted two things:

-   How much time others had spent solving the problem already. (A mark of that code’s maturity)
-   How much time future-me would have to spend fixing, tweaking, generalizing, and maintaining that code

Seasoned engineers call this Not Invented Here (NIH) Syndrome. And it is a pitfall that leads to duplicating other peoples work while taking on new maintenance burden. So when you find yourself in that moment where you could pick a library or tool off-the-shelf, but you think you just might be able to do it better, ask yourself how passionate you are about maintaining that code for the next several years.

Are there exceptions to this application? Absolutely! But the exceptions should be rare, and after you’ve genuinely explored whether existing solutions are good enough to get the job done.

## [](https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep#the-more-complex-your-code-the-harder-it-is-to-debug)The more complex your code, the harder it is to debug

I worked on a package manager that had a complex string parser inside. In a bout of inspiration, I wrote the entire parser in one sitting. Moreover, I did it in just a couple of functions. And even while those functions weren’t terribly long, they were conceptually complex. Expressed using technical terms, my code had very high [cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity). There were lots of different paths through these two functions, making it hard to take in at a glance all the different potential results of using this pair of functions.

In my defense, I wrote tests. And I wrote at least a little bit of documentation. And my code was impeccably formatted. Best of all, the code functioned very well, and I didn’t have to touch it for years (nor did any of the other dozen developers who worked on the project).

But none of that made up for the fact that it was hard to understand.

Then one day, we got a CVE. In a particularly devious attack, a hacker could force the parser to allocate more memory than the system had. After three or four years of not touching (or thinking about) this code, I had to go fix it. As my eyes scanned the lines, it felt completely foreign. And over the next few days I had to pick apart my own code to try to figure out what it was doing and how I could fix what turned out to be a well-hidden bug.

[Demeter’s Law](https://en.wikipedia.org/wiki/Law_of_Demeter) is a good software practice to follow. Named after the Greek goddess Demeter, it suggests that limiting the cyclomatic complexity (the number of decision trees) in code is a virtue attained by splitting code into useful units. While this is less of a real law than just a rule of thumb, it is a good one. It may force you to write more lines of code (as you break complex tasks down into functions), but this is virtuous if it results in more debuggable and maintainable code.

## [](https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep#writing-test-for-your-code-now-means-supporting-your-code-less-later)Writing test for your code now means supporting your code less later

One of the most audacious things I have ever heard a software developer say—and this was from a programmer with more than 20 years of experience—was that “requiring unit tests means telling your team you don’t trust them.” The first time he said this to me, I was astonished. What a huge and improbable leap! In every important industry we have, from doling out bills at the bank to filling bottles at the pharmacy to fastening labels to a newly created pair of jeans, any good system has a series of quality checks.

Code should have that too.

And what’s cool is that with code, it’s actually remarkably easy to add quality checks. We just write code to test our code!

While the developer I quoted above cited trust as the main reason, I have found that in my own career, I’d cite a different reason for not writing tests: Being in a rush. I’ve found a few good antidotes for being in a rush, and those are almost always external:

-   Use a code coverage tool to make sure that I am covering at least 80% of my code. And that each time I write something new, that new code doesn’t drop test coverage beneath that 80% level.
-   Require tests before any code passes code review. I am a HUGE believer in the value of code review, and this is one area that a review can really help with You may find other ways to help you stick to a testing regimen. A friend of mine and excellent coder, Adam Reese, once told me that he found it intellectually challenging to write elegant and exacting tests. Viewing tests as a challenge—a puzzle to be solved—helped him stay motivated to keep to his coverage goals.

Regardless of your strategy, if you don’t test now, you’ll be debugging later. Possibly in much more urgent circumstances with more dire consequences.

## [](https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep#future-you-wont-remember-what-present-you-is-thinking)Future you won’t remember what present you is thinking

So document it all!

We humans have a funny bias. We think that we will remember tomorrow everything that happened today. And we think that when we look back (perhaps even years from now) we will recall our mental state when an event happened. The thing is, we know this isn’t true. Most of us have a hard time recollecting what we had for lunch a few days ago.

I’m an ex-philosopher. The early modern philosopher David Hume expressed eloquently that he doubted that we should even consider ourselves to be “the same person” today as we were yesterday. Our minds are remarkably fluid, and Hume worried we tend to overstate how similar present me is to past me or future me.

Future you will not remember the nitty details of why you wrote this code the way you did today, or why you named that variable `fhr` or what you intended that poorly named function to do or why you left the comment `// FIXME later` on line 235.

The best way you can combat the limitations of your own memory is to make your code clear. That means:

-   Comment liberally
-   Name things well
-   Write higher level documentation
-   Write useful commit messages in your version control system

Future you will thank you. Or at least not be angry with you.

## [](https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep#corollary-if-other-people-cant-understand-it-it-will-always-be-your-problem)Corollary: If other people can’t understand it, it will always be your problem

So document it in a way that other people understand.

Really, if you are doing a good job at the last one, you probably have this one covered anyway. But here’s where the problem can get more insidious (or at least more annoying).

Earlier I told the story of the string parser I wrote. And how there was a CVE some years later. And that I had to fix it. What I didn’t mention is that I wasn’t even really working on that project when I was called in to fix it. I had moved on to other things.

But because it was not clear how my code functioned, nobody else felt like they could fix that bug. I had to halt all my work on another project for a few days while I came back to this one and fixed my code.

What’s the lesson there? If my code isn’t easy to understand, it will come back to haunt me. Because other people will not be able to understand it. And they will `git blame` it. And they will hunt you down. And they will refuse to let you have coffee and pizza until after you fix the problem. It will be unpleasant.

Know what’s better? Making your code so easy to understand that _somebody else can fix your code_ without you even having to know about it.

## [](https://dev.to/fermyon/it-took-me-20-years-to-learn-this-lesson-about-dev-1mep#conclusion)Conclusion

I suggest the following maxim as a way to push yourself to improve your own code:

> Remember that every line of code you write is a line of code you will support.

It will save future you time and energy. It will prevent frustration in your peers. It will make it easier to transform today’s code into tomorrow’s code. And it will probably make everyone else think you are one of those uber-coders as well.

There is a craft to code, an artisanship. And the best way to attain a high degree of mastery is to think carefully about what you are building, and then pour that thought into code.

I’ll say it once more: It’s a pain to support poor code. It’s a delight to support good code.
