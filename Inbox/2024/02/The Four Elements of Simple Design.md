---
created: 2024-01-03T09:28:51 (UTC -03:00)
tags: []
source: https://blog.jbrains.ca/permalink/the-four-elements-of-simple-design
author: 
---

# The Four Elements of Simple Design

> ## Excerpt
> I use two simple rules to help me get started improving the design of systems even before I understand anything about them. (I know the title says

---
I have reduced everything I’ve ever learned about modular design to the _four elements of simple design_ that I first learned from Kent Beck’s work. Maybe you can, too.

**Updated November 19, 2016! Almost entirely rewritten and, I hope, more concise.**

I care about keeping my designs “simple”, because doing so tends to _reduce volatility in the marginal cost of adding features over time_. Keeping the design simple _protects against the long-term erosion of our capacity to deliver features_. We might find other benefits to keeping the design simple, but as a professional programmer — and I merely mean one who receives money to write software — I care most about _sustaining the pace of delivering features_, since that helps me build trust within the project community and leads to greater long-term success for the businesses that my software supports. So what makes a design simple?

I call a design **simple** to the extent that it:

1.  Passes its tests
2.  Minimizes duplication
3.  Maximizes clarity
4.  Has fewer elements

The order here matters. I value the items near the top more than the items near the bottom. These properties tend to support each other, but when they conflict, I tend to err on the side of favoring the “higher” ones over the “lower” ones.

Most notably, _nothing else matters if the system behaves incorrectly_, so I care most about whether the system passes its tests. If it doesn’t compute useful answers correctly, then _behavior mistakes_ will limit the system’s value to end users.

When adding behavior to the system, I use the guideline _get back to the green bar as soon as possible_. I do this because when all the tests pass, I feel confidence, safety, and therefore _freedom_ to change the code however I need to. When tests are failing, I have no guarantee that I can change the code and have those tests fail for the same reasons as they previously failed. Accordingly, **I spend as much time as I can with the green bar**. On the green bar, I can take my time, feel less stress, and decide the next move.

Sometimes, in order to make a test pass, I copy and paste a surprisingly large section of code, such as an entire function or an entire class. I do this so that I can quickly change one or two lines of code and get back to the green bar sooner. Once I get back to the green bar, I decide how to remove the remaining duplication. **Since I value passing tests over minimizing duplication, I routinely duplicate code in order to make a new test pass, but then immediately remove most of that duplication.**

Often, especially when working with legacy code, I notice 6 copies of the same impenetrable 12 lines of code. Even before I know what this code does, I routinely extract this code into a new function, calling it `foo()`, even though we agree that “foo” does nothing to express its intent or clarify its significance. Over time, as I understand better how this code fits into the rest of the system, I discover a better name, and then rename the function. This doesn’t happen only in legacy code: even in a pristine greenfield environment, I sometimes extract duplication before entirely understanding what it represents, and so I call the new thing `foo()` or `Foo` until I can figure out what it does and what it means. **Since I value minimizing duplication over maximizing clarity, I routinely introduce new containers for code before I know what they represent, confident that I will be able to give them more suitable name as I learn more about the design.** I even have a [system for gradually improving names](https://blog.jbrains.ca/permalink/the-four-elements-of-simple-design#references).

I have noticed that I like to think in terms of first creating little things, then looking for opportunities to put them together into cohesive bundles. I have noticed that some other people like to think in terms of putting everything in a “junk drawer”, then looking for opportunities to pull out cohesive bundles of things into more specialized containers. I find both approaches valid and I think one’s approach comes down to an accident of the way one’s mind works. Either way, I gladly create more functions, methods, classes, interfaces, protocols, modules, packages, and namespaces in order to clarify the significance and purpose of the elements of my design. I find it easier to create more of these, and then look for opportunities to remove them as they become obsolete. **Since I value maximizing clarity over arbitrary conciseness, I routinely add design elements, confident that I will notice and remove the ones that become obsolete as they become obselete.** In the process of making things clear, it becomes clear what I can cut.

## Rule 2 and Rule 3

~Some people put “minimize duplication” and “maximize clarity” in a tie for second place. I don’t. My experience has led me to conclude that removing duplication helps more than fixing bad names does. Moreover, removing duplication tends to allow a suitable structure to emerge, whereas bad names highlight an inappropriate distribution of responsibilities. I use this observation as a key element of my demonstration, “Architecture Without Trying”.~

When I add new behavior, I tend to notice duplication before I notice a lack of clarity; however, when I read existing code, I tend to notice a lack of clarity before I notice duplication. I used to think that the order of these two rules mattered deeply, but after several years of intense debate, I have decided that it doesn’t really matter. **Indeed, removing duplication and improving clarity in small cycles creates a dynamo that drives simple design.**

## Passing Tests Becomes Simply “Programming”

As I practised test-first programming, and then test-driven development, I eventually reached the point where passing tests became routine and even autonomic. Now, I don’t think about writing and passing tests, just like I don’t have to think about how to breathe or walk or eat food. This means that I just don’t worry about passing tests anymore, just like I don’t worry about typing or saving files — these things simply become part of “programming”.

_Version 2_

I call a design **simple** to the extent that it:

1.  ~Passes its tests~
2.  Minimizes duplication
3.  Maximizes clarity
4.  Has fewer elements

## Funny… It Has Never Come Up

I’ve never need a code base that has a good suite of tests, low duplication, high clarity, but still had more design elements than it needed. A well-factored, clearly-expressed design will very probably have a suitable number of functions, methods, classes, interfaces, protocols, modules, packages, and namespaces. I have never needed to invoke the last rule.

_Version 3_

I call a design **simple** to the extent that it:

1.  ~Passes its tests~
2.  Minimizes duplication
3.  Maximizes clarity
4.  ~Has fewer elements~

## We Agree On Duplication, But Clarity Is Unclear

Duplication as a concept has one major advantage: we agree on it fairly easily. If you and I see the same code in five places, we agree that there is duplication. We might notice different kinds of duplication, and I might need to change code for a few minutes to help you see the duplication that I see, but once I do that, we can usually agree that the duplication exists. (I consider it a separate argument whether we ought to remove this particular duplication now.) Unfortunately, “clarity” remains ultimately subjective, and so we probably find more opportunities to disagree about it. This might account for why I tend to focus on removing duplication over improving clarity: it generally leads to fewer distracting disagreements. (I think that, over time, as we remove duplication together, the unclear parts become more glaringly unclear and that might reduce the number of distracting disagreements about which things to try to clarify.) Fortunately, **I have noticed that I can use the technique of improving names as an effective proxy or improving clarity**. Most clarity problems manifest as naming problems: either the name of the thing doesn’t match the thing, or I find a thing whose name only vaguely describes it, or I find nearby names that seem unrelated, or I find the same words scattered throughout the system rather than nearby each other. As I improve names, clarity seems to improve, almost as though by magic.

I’ve noticed that names tend to go through four stages: _nonsense_, _accurate-but-vague_, _precise_, and then _meaningful_ or _intention-revealing_, as Kent Beck described it. **I focus my efforts on gradually moving names to the right of this spectrum.** Instead of agonizing over the choice of a name, I simply pick a name, confident that I always look for opportunities to move names towards becoming meaningful.

## An Example Would Be Handy…

When I find fifteen lines of duplicate code, I start by extracting them to a new method, and since I probably don’t yet know what those lines of code do yet, I name the new method `foo()`. After around 15 minutes of working in the same area, I begin to understand what this method does, so I give it an accurate-but-vague name, such as `computeCost()`. As I try to write tests for this new method, and find it more tedious than I expected, I realize that it does more than just compute the cost of an item, so I rename it more precisely: `findItemThenComputeCost()`. The mere appearance of a conjunction, like “and”, “or”, “but” or “then” tells me that the method has more than one responsibility, and as a result, I soon find myself writing tests that duplicate [irrelevant details](http://bit.ly/6wXgmS). When I remove this duplication, I have to split this method into two: one that finds the item and the other that computes the cost. When I look at `computeCost()` more closely, I see that it really adds taxes and shipping charges to the net price of an item, which leads me to split that behavior into multiple methods, each that applies a charge to an order item. This leads me to introduce the more meaningful `OrderItem` class and the `OrderItemCharge` interface with method `applyTo(item)` and implementations `PrinceEdwardIslandSalesTax`, `CanadianGoodsAndServicesTax` and`DomesticShipping`. These more meaningful names communicate the story of the design much better than the previous `computeCost()` name did. You can find similar examples in your code bases of the movement towards meaningful names. Most of the time, I struggle to reach the _accurate and precise_ stage, then I find the jump to _meaningful_ names quite easy. I have found that most clarity issues reduce to misleading names.

That leaves me with two key elements of simple design: **remove duplication and improve names**. When I remove duplication, I tend to see an appropriate structure emerge, and when I improve names, I tend to see responsibilities slide into appropriate parts of the design.

**I claim these to be axioms of modular design, with a “parallel postulate” of whether you use objects or not.** Whether you end up with procedural, object-oriented, or functional design depends mostly (only?) on how you choose to organize your functions.

**I claim that developing strong skills of detecting duplication, removing duplication, identifying naming problems, and fixing naming problems equates to learning everything ever written about object-oriented design.**

Put more simply, if you master removing duplication and improving names, then I claim you master modular design.

Now I wouldn’t bother burning your old OOD/OOP books, but I will tell you that if you have an interested buyer, then feel free to sell them.

_Now that you’ve read this article, you’ll almost certainly want to read [this article next](https://blog.thecodewhisperer.com/permalink/putting-an-age-old-battle-to-rest)._

## Reactions

> [@LutherBaker](https://twitter.com/LutherBaker) I find this article frustrating but generally agree with it :) I like this reference from it _a lot_ [http://t.co/DyatBix6dG](http://t.co/DyatBix6dG)
> 
> — Mark Katerberg (@diablomarcus) [February 4, 2015](https://twitter.com/diablomarcus/status/562999744239509506)

> [@mdchris](https://twitter.com/mdchris) remove duplication, \[improve\] names. (c/o [@jbrains](https://twitter.com/jbrains)). I was surprised how impactful that was while reading a stack trace.
> 
> — David Alpert (@davidalpert) [March 13, 2014](https://twitter.com/davidalpert/statuses/444132373546803200)

## References

J. B. Rainsberger, [“A Model for Improving Names”](http://blog.thecodewhisperer.com/permalink/a-model-for-improving-names). This diagram summarizes how I think about gradually improving names over time.

J. B. Rainsberger, [“Putting An Age-Old Battle To Rest”](https://blog.thecodewhisperer.com/permalink/putting-an-age-old-battle-to-rest). In trying to settle the question of the relative importance of removing duplication and improving clarity, I describe how I believe these two mechanical techniques contribute to simplifying our designs.
