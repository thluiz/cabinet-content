---
created: 2024-03-06T09:54:14 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2020/01/15/philosophy-of-software-design-part-4/
author: Chelsea
---

# PoSD 4: Don’t knock things you haven’t tried – Chelsea Troy

> ## Excerpt
> I recently read John Ousterhout’s book, Philosophy of Software Design. You can see all the posts that use it as a reference point right here. In the first post we talked about eschewing compl…

---
Reading Time: 9 minutes

I recently read [John Ousterhout](http://web.stanford.edu/~ouster/cgi-bin/home.php)‘s book, _Philosophy of Software Design._ You can see all the posts that use it as a reference point [right here](http://chelseatroy.com/tag/philosophy-of-software-design).

In the [first post](https://chelseatroy.com/2019/12/14/philosophy-of-software-design-part-1) we talked about eschewing complexity, handling special cases, and example choices in software literature. Then we went on a tangent about debugging. Now we’re back at the book to talk about code legibility and, uh… “software trends”.

### Legible Code

Once again, Ousterhout starts from a premise that feels (relatively) uncontested within the code writing industry:

> Software should be designed for ease of reading, not ease of writing.

After all, code will be read many more times than it will be written. We write it once (well, once per rewrite), and then we, and other programmers, read it for context multiple times.

On this much, the book and I agree. I have quibbles with some of the details, though.

For example, this particular example feels weird to me:

![Screen Shot 2019-12-09 at 9.21.17 PM.png](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/Screen-Shot-2019-12-09-at-9.21.17-PM.png?resize=723%2C622&ssl=1)

I wouldn’t use this `List`, `ArrayList` thing as an example of erroneously declaring and allocating variables as different types. First of all, `List` is not a superclass of `ArrayList`, as this text claims; `List` is an interface. `ArrayList` implements that interface. There are legitimate reasons to declare a variable by its adherence to an interface and then new it up, or receive it, as any one of several implementations of that interface.

Second, this exact example is, in my experience, relatively conventional in Java. And just pages earlier, the book says:

> **Don’t change existing conventions.** (emphasis by me, Chelsea Troy.) Resist the urge to “improve” on existing conventions. Having a “better idea” is not a sufficient excuse to introduce inconsistencies. Your new idea may indeed be better, but the value of consistency over inconsistency is almost always greater than the value of one approach over another.

But this situation—the example pages away from the quote that contradicts it—illustrates another broader point worth making: **software design decisions often involve a series of tradeoffs. Few decisions are always right for every case.**

The distinction between slinging code and engineering software, in my experience, largely revolves around the ability to move from making lists of our “favorite” techniques into the more nuanced realm of assessing the risks in a situation, considering the ways in which the code must be resilient, and choosing techniques that optimize for that particular type of resilience.

### Chapter 19: “Software Trends”

**TL;DR: Skip this chapter.**

Longer version:

I don’t think Ousterhout wanted to write this chapter. I think the publishers had a list of buzzwords that they made him include for search engine optimization.

Why do I think that? Because after a whole chapter devoted to _just_ the idea of naming things and _two_ whole chapters on the practice of writing comments, we arrive at a single chapter that discusses…\[deep breath\]…object oriented programming versus inheritance, agile development, unit tests, test driven development, design patterns, and getters/setters.

Not only does this chapter feel like a grab bag, the level of abstraction varies widely. Getters and setters are an implementation detail in particular programming languages. Agile development is a whole philosophy of project management in use among teams both inside and outside of tech.

The sections in this chapter are also not well-informed. I don’t blame Ousterhout for this because his book wasn’t supposed to be about these things. If I had written a book about , say, debugging fundamentals, and my publisher had given me a list of unrelated topics to include, I’d also have tossed them in a catchall chapter and shut the lid as fast as I could.

Since Ousterhout is beholden to publishers and the market, he can’t issue disclaimers on the contents of this chapter. But nobody’s paying _me_, so I can do it all I want.

The disclaimers for each section follow.

####  **1. Object oriented programming and inheritance:**

This section explains that inheritance guards against change amplification, but creates dependencies between a class and its subclasses. It suggests trying composition instead.

I’m not a fan of writing that gives the impression that one solution is universally better than another. The section on this even subtly acknowledges that that isn’t true:

> _If there is no viable alternative to implementation inheritance…_

OK. What would cause that? Under what circumstances might this approach be _useful_?

I do mobile development. Mobile frameworks use inheritance all the time. It’s not because mobile devs are a bunch of incompetent weenies who don’t know how to write code. Try writing a mobile framework without it, and you’ll discover _real fast_ what circumstances make inheritance suddenly seem like a great idea.

So, this discussion of inheritance and composition lacks nuance and fails to address the topic in a responsible way.

####  **2. Agile:**

This section says, verbatim:

> _One of the risks of agile development is that it can lead to tactical programming_.

No.

The thing that leads to tactical programming is rewarding developers for feature development in the short term more than we reward building things to last. The same thing happens in waterfall, or else enterprises wouldn’t be hiring “agile” consultancies to rewrite all the shit that they wrote with waterfall. So it’s pretty annoying to me that this section takes an effect of myopic end-stage capitalism and blames it on “agile.”

####  **3. Unit Tests:**

This section mostly explains unit versus system tests and then talks about the utility of unit tests for refactoring with confidence. I agree with everything it says.

There’s an additional thing that I’m surprised this section doesn’t mention, given the way the rest of the book focuses on the complexity created by dependencies: unit tests introduce a dependency risk. Poorly-scoped unit tests easily end up testing the _implementation_ of the code rather than its API, which makes refactoring _harder_ rather than easier. It takes some thought (and experience) to write tests that assert on the interface while allowing the implementation to change.

#### **4\. Test-driven Development:**

This is the section I most suspect was written with no research. It describes TDD as writing _all_ the tests for a class before starting the class. I taught TDD for years, and I never once did this. I’ve paired with 126 developers doing TDD, and not one of them did this, either.

The only context in which I have seen this done is academic: a professor attempts to use a test suite as a spec for a set of students to implement something. That only works for previously solved problems in environments where the implementation language and framework can be (wildly) out of date. I know that that describes academia. It does not describe industry. [We have covered the reasons for that](https://chelseatroy.com/2017/04/23/but-isnt-pair-programming-expensive/):

> **Writing custom code is not a process of cranking it out.** You’re asking for custom code because you want something specific to your company—that is, something that has _never been done quite this way before_.
> 
> In addition to the fact that you want something unique, the plethora of languages, libraries, frameworks, and tools that programmers use to make your features are _always_ changing. **If a programmer steps out of programming for _eight months_, her skills become obsolete.** She isn’t useless, but she’s out of date on every single language, library, framework, and tool she is asked to use upon her return. That’s how fast this industry changes.
> 
> Programmers aren’t cranking out the same thing day after day; instead, they are constantly learning new things on the job.
> 
> **The learn-and-apply process is not linear.** It involves experimenting, making mistakes, trying things several times with a different variable isolated each time.

So we don’t linearly go through and make a class’s worth of tests go from red to green. We follow a cycle called red-green-refactor, one test at a time.

The book does come back to the dependency risk in this section. I think TDD that produces tactical programming is more a product of lack of experience with TDD than TDD itself. This is like trying to surf a few times, not standing up a whole lot, and claiming that surfboards don’t work. I see people do the same thing with pair programming. “Oh, I tried it once; I don’t like it.” Maybe you just suck at it right now. Develop the skill and your opinion might change.

####  **5. Design Patterns:**

The book says:

> _The greatest risk with design patterns is over-application_.

I feel like that’s the greatest risk with a lot of this stuff; categorically claiming something is “good” without exploring the nuance of its benefits and drawbacks and how those match up to the current (and probable future) needs of a system and a team.

#### **6\. Getters and Setters:**

This feels oddly granular compared to all the other trends discussed. I use getters and setters in Java, largely because that’s what my collaborators typically expect. Other languages I follow other conventions: Ruby has `:attr_accessor`; Python leans toward direct access unless the property is computed; Swift goes with direct access, sometimes even if the property is computed.

### Conclusion

Look, I appreciated much of the content of _Philosophy of Software Design,_ and overall I’d call it a strong book. There were a couple of things that I took issue with, and I suspect the author of the book didn’t love including most of those things.

But given that many of those things took a critical approach without much nuance, I felt a responsibility to point them out so readers don’t take the author’s authority as license to write those things off and never try them.

For more on the parts of this book that I thought did a better job, [check out the posts at this tag](http://this%20part%20of%20the%20book%20felt%20weird%20and%20poorly%20considered,%20and/).

### If you liked this piece, you might also like:

[Watching some live coding](http://chelseatroy.com/category/live-coding) (complete with narration and show notes)

[This talk about the technology and psychology of refactoring](https://chelseatroy.com/2019/03/25/pearconf-talk-the-technology-and-psychology-of-refactoring/)—in which you’ll hear me explain some of what you see me do when I code.

[This 3 part series about time and space efficiency](https://chelseatroy.com/2019/05/23/time-and-space-efficiency-an-applied-example-part-1/)—In which I approach the topic of performance from the perspective of a code sample. Why make things fast? Why make them take up less space? And how do we evaluate the tradeoffs?
