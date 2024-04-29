---
created: 2024-03-05T14:03:14 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2021/10/29/a-rubric-for-evaluating-team-members-contributions-to-a-maintainable-code-base/
author: Chelsea
---

# A Rubric for Evaluating Team Members’ Contributions to a Maintainable Code Base – Chelsea Troy

> ## Excerpt
> PC: Growveg.com How much of your engineering team’s effort gets spent on keeping the existing system running, rather than changing the feature set? I call that maintenance load, and it’…

---
Reading Time: 11 minutes

[![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/10/productive-city-garden-2x.jpg?resize=723%2C337&ssl=1)](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/10/productive-city-garden-2x.jpg?ssl=1)

PC: Growveg.com

How much of your engineering team’s effort gets spent on keeping the existing system running, rather than changing the feature set? [I call that](https://chelseatroy.com/2021/01/14/quantifying-technical-debt/) **maintenance load**, and it’s expensive and frustrating when it gets too high.

One of the most important concepts for addressing a code base’s maintenance load is **code stewardship**—the trick (or really, the entire skill set) of keeping a code base maintainable.

I facilitate trainings<sup>1</sup> on addressing high maintenance loads, and in those trainings we talk about examples of code stewardship skills:

[![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/10/code_stewardship.png?resize=723%2C407&ssl=1)](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/10/code_stewardship.png?ssl=1)

This week, someone who took one of those trainings DM’d me about their experience with talking to their team about code stewardship:

> “My day to day is now mostly centered around training and mentoring our team…and I often struggle to communicate the importance of code stewardship. I’ve tried to present the idea in different way, from different perspectives but I feel it never really sticks with my team members.”
> 
> – a workshop taker (for obvious reasons, anonymized)

So, on the one hand, it’s _really_ hard to find good resources on code stewardship. Our engineering education, literature, and tutorials focus disproportionately on building things from scratch without considering how they’ll fare in a few years.

### But beyond that, there’s another barrier to adoption:

Engineers are, for the most part, not incentivized to care about code stewardship. In fact, they’re incentivized to _not_ care, because time and effort are zero sum games. Engineers receive more mentions, accolades, raises, and promotions from cranking out features than from investing time in code stewardship, so the former represents a massive opportunity cost of doing the latter.

### I’ve addressed an issue like this one before: inclusion.

Longtime readers might be familiar with [this piece](https://chelseatroy.com/2018/05/24/why-your-efforts-to-make-your-company-inclusive-arent-working/) entitled “_A Rubric for Evaluating Team Members’ Contribution to an Inclusive Company Culture._” It discusses a solution to the predicament where an organization has conducted umpteen D&I trainings and still finds itself dealing with inclusion-related bullshit (yes, it actually says “inclusion-related bullshit”).

Again, the reason is, even in the presence of information, no one is incentivized to care—which, when playing a zero-sum game, means that folks are disincentivized to care. I suggest fixing that by adding five specific skills to employee evaluations. It’s one of the most-read pieces on this blog<sup>2</sup>, and it has influenced the review process in at least twelve companies, including Mozilla<sup>3</sup>, Apple, and Amazon<sup>4</sup>.

### What if we deconstructed code stewardship into evaluatable skills, too?

Let’s try it.

To describe each of these skills, I have chosen a series of questions that you could use on a rubric to evaluate folks’ proficiency with them. These questions can help you give teammates benchmarks to reach for in improving their code stewardship. They also demonstrate practices that your reports can use on a daily basis to build those skills. In each, I’ll link to additional material about that specific skill. Throughout, I will refer to the person you are evaluating as ‘this engineer’ and with the pronoun ‘they.’

### 1\. Code Flexibility

-   Does this engineer make an effort to predict which parts of the code base are most likely to change in the future?
-   Do they ground those predictions in information from customers/clients, directives from the product team, or upcoming [accessibility opportunities](https://chelseatroy.com/2021/07/30/the-oxymoron-of-data-driven-innovation/)?
-   Do they use those predictions to inform which parts of the code base need to be easy for maintainers to change?
-   Can they allocate as much flexibility as possible to those areas—while acknowledging and executing rigidity in other parts of the code base—rather than sacrifice flexibility in those areas to attempt to retain flexibility everywhere?

Right off the bat here, we’re talking about a pretty advanced skill. The thing is, we harangue engineers to make their code flexible and maintainable, but in practice it’s really hard to make a system flexible _everywhere_. Attempts to do so tend to yield systems that truly flex almost _nowhere_, with begrudgingly rigid portions throughout. That result tends to derive from a culture of absolutism in tech—assuming one tool or practice is _always_ better or best, when in fact, every tool’s utility is context-dependent. This skill focuses an engineer on understanding the current and future needs of their clients, then investing in flexibility where it’s likely to be most helpful. [This piece](https://chelseatroy.com/2021/03/08/should-i-use-functional-or-object-oriented-programming/) exemplifies flexibility allocation with functionally-oriented and object-oriented solutions. [This talk](https://chelseatroy.com/2021/10/04/yow-keynote-what-counts-as-a-programming-language/) discusses how to uncover the hidden context for “universally” better technical solutions. [This piece](https://chelseatroy.com/2020/05/28/lessons-from-space-edge-free-programming/) exemplifies some of the ways that incremental design and engineering can accidentally introduce rigidity or complexity into a code base, and how an engineer might handle them.

### 2\. Documentation

-   Can a collaborating engineer get this engineer’s code working without spending excess time and trouble?
-   Can a product person evaluate this engineer’s code against acceptance criteria without spending excess time and trouble?
-   (If the engineer’s clients are other engineers) can clients, without spending excess time and trouble, get to using the tool correctly, and quickly bounce out of using it incorrectly?

Once again, kind of an advanced skill. This isn’t just about info-dumping into a README (which, in fact, is not that effective as a blanket documentation strategy). Instead, can this engineer anticipate when someone will need a piece of information and arrange for that piece of information to be available at the right time, in the right place, as isolated as possible from other, extraneous pieces of information? Good docs are _hard_. [Some strategies here.](https://chelseatroy.com/2021/09/14/the-art-of-documentation/) (You’ll notice that the documentation strategies often require sub-skills like testing, commit hygiene, et cetera. Engineering excellence requires lots of skills 😉. I try to link out on those from that piece, where I have something to offer).

### 3\. Forensic Software Analysis

-   When presented with a code base on which a lot of context has been lost, can this engineer recover some of the context?
-   To what extent can this engineer help us prevent or forestall rewrites of working code based on the fact that our current team doesn’t know how it works?

I know, I’m just slamming your engineers with herculean task after herculean task right now. You don’t come to this blog for easy, though. I _try_ to make the difficult easi_er_, but sometimes things are just difficult. This is one of those pieces.

To be honest, I’ve seen engineers who are relatively _good_ at this, but I’ve never seen someone especially _fast_ at it. This one is asking the engineer to do detective work, using clues in the remains of someone else’s work to piece together the full story of its construction. It deserves more attention than it gets from the programming community. I’ve [only made a start on that](https://chelseatroy.com/2021/01/21/reducing-technical-debt/), but it’s something. The sub-skills here include [gathering info from commit histories](https://chelseatroy.com/2020/01/08/storing-context-in-commit-messages/), using [debugging tools and strategies](https://chelseatroy.com/category/programming/programming-concepts/debugging/?order=asc) to gather context on how the code works, and [taking advantage of tests](https://chelseatroy.com/category/programming/programming-concepts/testing/) for experimenting with the existing functionality.

### 4\. Context Transfer

-   When this engineer is the only person holding context on something, can they transfer that context to another teammate?
-   When this engineer submits changes, are they able to support another engineer in being able to _fully_ take over these changes if that were necessary?
-   When this engineer leaves a project, do they leave artifacts behind that make forensic software analysis easier for another engineer, even if the two of them were to never communicate?

_Oh what the f\*\*k_, _Chelsea_. I know. I expect a lot. But you’d be _amazed_ how much time and effort a team can save by eliminating the risk of [abandoned houses](https://chelseatroy.com/2021/01/14/quantifying-technical-debt/)—that is, swathes of code where no one on the team knows what’s going on.

Please note, also, that I’m not requiring engineers to pair on everything, or even always review each other’s stuff. That third point up there is asking about an engineer’s ability, _when working alone_, to help someone pick up where they left off if they were to leave the project behind. I take [Ines Montani’s suggestion](https://chelseatroy.com/2021/01/18/avoiding-technical-debt/) to heart that sometimes a bus count of one is fine. I’d argue, it’s fine, particularly on _critical path_, _hard to reimplement_ stuff, only if “the one” ensures that some ambiguous subsequent person could _become_ “the one” in a pinch.

I [did a series on context transfer](https://chelseatroy.com/tag/temporally-distributed/) and I’m hoping that it helps.

### 5\. Feature Accountability

I _really considered_ leaving this one off because it’s at least as much about the product team as it is about the engineers. I left it on because a) sometimes the product team _is_ the engineers and b) even when they aren’t, I want to put this somewhere that someone with access to the people who evaluate the product team might see it.

Here’s the situation:

1.  Every feature in a code base, no matter how clean, adds maintenance load.
2.  Product people are generally focused _forward_ on _adding_ features—not removing features.
3.  As a result, feature sets tend to grow—not shrink or stay constant. That means, maintenance load always grows.
4.  Unless the team of engineers is _also_ always growing, that means engineers spend more and more time keeping the lights on, which means less, and eventually _zero_, time for adding features.
5.  At that point, everybody is sad. Product can’t get eng to enact their vision. Eng can’t get out from under their Ball of Mud to do any “fun” work. People start leaving. It sucks.

The solution: make sure the existing features are providing enough value to justify the “rent” cost of maintaining them. Does eng need to do that? Does product? I don’t care who is doing it as long as it’s done, and the way to get it done is to evaluate and reward it.

-   Does this engineer/product person (hereafter PP) regularly gather metrics on whether and how our features are used?
-   Does this engineer/PP compare a feature’s benefit to its maintenance cost?
-   Does this engineer/PP use that information to identify opportunities to remove, replace, and enhance features, rather than just adding features?
-   Does this engineer/PP prioritize those efforts to the degree that their level of authority allows?

I talk about feature bloat a little bit [here](https://chelseatroy.com/2021/01/21/reducing-technical-debt/), but the workshops<sup>1</sup> that I facilitate go into more tactical details on identifying and reducing feature bloat. If I ever do a blog post specifically on that I’ll link it here too, but maybe you just want to come and see me at a workshop until that happens 🙂.

### How do we set levels for these skills?

We have five skills that people can work on to raise their value as contributors to a maintainable code base—four for engineers, and one that gets product involved, too.

How do we level-set on these skills? What’s a hireable skill level? What’s a promotable skill level? At what point would a lack of these skills justify not hiring, not promoting, or even sending people back the other way? I trust you to make the decisions on where those lines fall for your company.

That having been said, it can be a helpful exercise for your team to sit down together and consider how you all might set these levels based on your values. What does inadequate code stewardship look like for you? What’s a hireable amount? At what point do you want that person in leadership? Whatever you choose, you’re way ahead of the majority of tech companies that aren’t systematically evaluating anyone on any of this.

### Footnotes

1.  I facilitate one recurring training on addressing technical debt [with O’Reilly](https://www.oreilly.com/live-events/technical-debt-first-steps/0636920058624/0636920058623/). I’ll be hosting another one at [RubyConf](https://rubyconf.org/program/workshops#session-1189) in a couple of weeks. I also do them occasionally for clients (ping me if you need one).
2.  Believe me, I’m as shocked as you are. Though, I confess, I was relieved when it first eclipsed [this piece](https://chelseatroy.com/2015/09/27/android-examples-a-test-driven-recyclerview/) on testing RecyclerViews in Android, because I would no longer test a RecyclerView the way I describe in that piece. I leave it up because I still think the method it describes is better than _not_ testing the RecyclerViews at all, which is what most people who read that piece would do if the piece didn’t exist. But I wouldn’t want to be judged by an Android expert on my Android knowledge based on that piece. (I could also write an updated piece, but frankly I can’t be arsed right now).
3.  This is not because of insider influence; it happened several years before I started working there.
4.  I did not sell it to anyone in there and I do not endorse the full slate of this company’s employment practices. I’m reporting what employees there have told me.

### If you liked this piece, you might also like:

[This three-part series on quantifying, avoiding, and reducing “technical debt”](https://chelseatroy.com/2021/01/14/quantifying-technical-debt/) (I replace that term pretty early on in the series)

[The Art of Documentation](https://chelseatroy.com/2021/09/14/the-art-of-documentation/) (I probably don’t say what you’re expecting me to say!)

[The Oxymoron of Data-Driven Innovation](https://chelseatroy.com/2021/07/30/the-oxymoron-of-data-driven-innovation/) (people either love this one or _hate_ it, which tells me…something, though I confess I’m not sure what)
