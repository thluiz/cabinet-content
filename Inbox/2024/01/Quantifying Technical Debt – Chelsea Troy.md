---
created: 2024-03-05T12:45:49 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2021/01/14/quantifying-technical-debt/
author: Chelsea
---

# Quantifying Technical Debt – Chelsea Troy

> ## Excerpt
> “We’re swimming against a current. We keep swimming and swimming, but I look at the shore, and we haven’t moved.We’re exerting ourselves just to keep our software doing what…

---
Reading Time: 12 minutes

[![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-14-at-8.35.48-PM.png?resize=723%2C210&ssl=1)](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/01/Screen-Shot-2021-01-14-at-8.35.48-PM.png?ssl=1)

> “We’re swimming against a current. We keep swimming and swimming, but I look at the shore, and we haven’t moved.
> 
> We’re exerting ourselves just to keep our software doing what it already does for the people who already use it.”
> 
> –Todd, Engineering Director

Have you felt like this on a tech team?

Getting out of tech debt can feel like a Sisyphean task. It’s not uncommon for organizations to declare code bankruptcy and rewrite _working systems_ from the ground up. As a former enterprise software consultant, I have participated in some of these rewrites. They cost half a million dollars almost automatically. They can cost millions easily.

Something that pricey and frustrating deserves analysis: how do we end up in this situation? How do we measure it? And how do we alleviate, or even better, prevent such a situation?

### This is the first in a three part series:

1.  **Quantifying Technical Debt (this post)**
2.  [Avoiding Technical Debt](https://chelseatroy.com/2021/01/18/avoiding-technical-debt/)
3.  [Reducing Technical Debt](https://chelseatroy.com/2021/01/21/reducing-technical-debt/)

### First, we have to agree on what we mean by “tech debt.”

And do to that, we need to confront some misconceptions about what tech debt is and where it comes from.

[![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/01/code_quality.png?resize=550%2C433&ssl=1)](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/01/code_quality.png?ssl=1)

The comic is hilarious and accurate, but WTFs/minute doesn’t hold up in board meetings.

### Misconception #1: “Tech Debt” = “Code Shittiness”

This is funny, I’ll grant, but it’s not super helpful, for a few reasons.

First of all, we have to agree what we mean by “bad code,” and when we haven’t done that, developers adjudicate “bad code” based on their personal perspective. I have watched developers sink literal months into completely restructuring working, maintainable code because it didn’t match their personal architectural preferences. This is not tech debt: this is one developer making a cavalier choice about the business’s resources. This is code _renovation_, not code maintenance.

Second of all, we tend to equate “bad code” to “code written by bad developers.” This is _also_ not accurate—there’s plenty of illegible, hard-to-maintain code written by thoughtful programmers who were under constraints (more on that in [this talk](https://chelseatroy.com/2019/03/25/pearconf-talk-the-technology-and-psychology-of-refactoring/) about the technology and psychology of refactoring). Since no one thinks of themselves as a “bad developer,” they also don’t think of their code as an accrual of tech debt as often as it is—which is, always.

That’s right: all code incurs tech debt. Which brings us to:

### Misconception #2: Once you build a feature, it’s free.

I don’t love comparing software projects to physical construction projects because it results in _a lot_ of false equivalences. But it works in two cases: ticket estimation, and maintenance.

The work is not finished on a house after you build it. The building needs maintenance. The pipes wear out and you have to replace them. A hurricane rips the roof off and you have to replace it. A window gets broken and you have to replace it. The bigger the building, the more maintenance it needs. That’s an amount of work that you put into the building _outside_ of renovations (the corollary for our pet refactoring projects) or adding new wings to the building (our corollary for adding new features—which, by the way, will _also_ need to be maintained).

### Every software feature adds **maintenance load.**

**maintenance load** – how much effort your development team spends to keep the existing features running the same as before. **This** is a more useful metric for technical debt than code shittiness. Maintenance load is a function of the age of the project and the practices used to build it. The unit: ongoing developer effort. Let’s talk about some maintenance load growth rates I’ve seen in my career.<sup>1</sup>

### Yikes case: maintenance load growth of 1 dev per 18 months.

If a team doesn’t write tests, doesn’t document how to use the features they wrote, and primarily employs an **agglutinative** coding style that values adding features by stapling them onto the existing code base, they tend to need one developers’ worth of effort focused _only_ on maintenance for every 18 months this code base has been growing.<sup>2</sup>

I called this the “yikes case,” but it’s super common—not because all developers suck, but because…

1.  Up until ~2012, that was standard practice on software teams, especially young ones (young _teams_, not young _people_). It often still is. So any code written before 2012 probably did this.
2.  These practices don’t really bite you until either a) your code base gets big enough that no individual person can understand it all or b) you experience your first _churn event_ (the person who knows how this works leaving the team). By the time these practices cause a major problem, it’s too late: they’re already all over the code base.
3.  Code stewardship is a whole, _difficult_ skill that is _completely_ separate from writing feature code, which is what most developers are a) trained to do and b) rewarded for. Developers are neither prepared nor incentivized to test, document, or communicate, and they’re also not empowered to rethink or remove features. I just made a _lotta_ claims. We’ll come back to this later.

The point is, if you’re working on a yikes-style code base that’s 4.5 years old, you would need 3 members of your team just to maintain it. I don’t mean clean it up. I mean hang on at the current level of maintenance, with the thing in roughly its current state, keeping things working.

### Average case: maintenance load growth of 1 dev per 24-30 months.

Suppose the devs wrote some tests. They did some cursory documentation, but not everything is documented, and some of the docs are out of date. Occasionally something got rethought or removed. This team’s maintenance load is growing more slowly than the yikes team. They still add one full time developers’ worth of maintenance load every 2ish years. Once again, this isn’t to _grow_ the feature set. This is just to keep the current one working. So in a code base like this that’s 10 years old, 4-5 devs are _just_ on maintenance.

> “Woah, Chelsea. Your maintenance load estimate is bigger than the size of my team. Your number estimates are way too high.”
> 
> – You, probably

Listen, maybe your team is running circles around the average case and totally on top of their maintenance load, and you don’t even need to be reading this. That’s awesome, I’m so happy for you! I wouldn’t count on it, though.

### Signs you shouldn’t count on it:

-   Your team has explicitly agreed with product to “take on” technical debt to get something done
-   The product has features that just straight-up don’t work, and y’all’ve all accepted it
-   There are apps, repos, or features that the whole team won’t touch, or will only go in nervously and change, like, one feature flag before dashing out like a child who hears a creak in an abandoned house. If these things ever _really_ break, your team agrees, y’all’re screwed.

If your team sees this when they look at your repo, no, the maintenance load is not under control.

This isn’t a normative judgment on the conditions of your team. It’s not _wrong_ to add a kludge that fills an urgent need for your constituents.<sup>3</sup> It’s not _bad_ to put a `wontfix` label on a feature that no one wants. Abandoned houses in the code base are often the result of someone leaving the team without passing on their knowledge—not the mistakes of the current team.

But what these occurrences tell me is that your team is at its **maintenance limit**. Here’s why:

-   Fixing the kludge would bring _down_ the maintenance load.
-   Deleting a broken and unused feature would bring _down_ the maintenance load.
-   Figuring out what’s happening in the abandoned house would bring down maintenance _anxiety_, if not specifically _load._

We’ll come back to the abandoned house. These other two things? They _definitely_ have a positive return on investment—and, as a product grows and ages, a _high_ return on investment. Why would you not invest money in something with a known, high ROI? Because it’s money you don’t have. These symptoms tell me that the team **doesn’t have more resources** to spend on maintenance than they’re already spending. Your load is, at best, _right_ at the rim of what your team can address. Realistically, though, it’s probably above what your team can address.

[![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/01/meniscus_water.jpg?resize=723%2C414&ssl=1)](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2021/01/meniscus_water.jpg?ssl=1)

Asking a team at its maintenance limit to add more features = pouring more water into this glass.

And the thing is, unaddressed maintenance load, over time, begets _more_ maintenance load. For the same reason that [it’s expensive to be poor](https://www.theatlantic.com/business/archive/2014/01/it-is-expensive-to-be-poor/282979/), it costs more dev effort to not address the issues that siphon off your dev effort.

### Losing control of your maintenance load

If maintenance load outstrips maintenance limit by enough, or for long enough, you get fires, outages, and breakdowns. The team spends a lot of time on call and ends up stressed out. They’re trying to write new features while keeping the existing product duct taped together.

It’s worth noting here that if your maintenance load is equal to the number of developers you have, you’re probably still above your maintenance limit. The reason for this is that developers usually want to make stuff, and while they’re applying duct tape to things, they’re not making stuff. When your developers start saying “I can’t remember the last time I wrote meaningful code,” you’re at risk of losing them to another team where they’ll hope to contribute to constituents’ experience.

That’s called **churn**—losing developers who know your product and replacing them with developers who don’t know your product. This is a _massive_ source of maintenance load, because it causes **context loss**: when all the knowledge that the leaving developer had, but didn’t document, effectively evaporates. This is where abandoned houses come from, and this is one of the reasons that losing a dev can realistically cost the organization, all told, several times the outgoing dev’s annual salary. There’s a way to regenerate lost context, but it’s _expensive._ We’ll get there later.

### Maintenance load bankruptcy

Look, my father is an engineer and an old school Louisiana cajun farm boy. We don’t agree on much, but we do agree on one thing: it’s almost always more resource-efficient to fix it than to buy a new one.

Yeah, ripping the whole plumbing out of a house is arduous, time consuming, and expensive. But it’s not as arduous, time consuming, or expensive as razing and rebuilding the house.

Enter the rewrite. I have participated in nine ground-up software rewrites, and I think I’ve heard a healthy sampling of reasons for doing them: “It uses deprecated dependencies.” “We want to modernize it.” “No one knows how it works.” All these things are a big expense to change. None of them, though, are a bigger expense to change (in the long run) than rewriting the whole thing from scratch.

It’s common in tech for a team to go for a full rewrite, not because the original is hopeless of its own accord, but because it has reached the point where the maintenance load is too high for the existing team to feel like it can _ever_ catch up with. Starting over from zero, while _almost always massively_ more expensive than maintaining existing code in the long run, gives the existing team the illusion of control over their maintenance load for a couple of years before it gets out of control again.

So is maintenance load just destined to get out of control? Not necessarily, in my view. But it takes a specific skill set to avoid maintenance load accrual. I’ll talk about that in [the next post.](https://chelseatroy.com/2021/01/18/avoiding-technical-debt/)

[![banner-83274235](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/10/Screen-Shot-2019-10-02-at-10.27.29-AM.png?resize=723%2C116&ssl=1)](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/10/Screen-Shot-2019-10-02-at-10.27.29-AM.png?ssl=1)

### Footnotes

1.  I _did_ take detailed notes, but my number of projects is limited, so these numbers might be off for your org. Maybe you add one full time dev efforts’ worth of maintenance load every 3 years, or every 20 months, or every 9 months (oy vey, I hope not!)
2.  By the way, I’m calling this the “yikes” case, but it is by no means the worst case scenario: it’s a disappointing-but-common case that I see in teams of relatively capable professional software engineers. It’s not rock bottom. I also teach CS graduate students. I have advised on student projects where a student, as the sole developer on a code base, didn’t understand how their own code worked in a 6 week old project. This is a person who is _about to have a CS graduate degree_. So, in case I haven’t made it clear yet, this is partially about lack of skill, but it’s not about lack of _any_ skill. It’s about the lack of a specific skill (code stewardship) among a group of people who are probably much better at the skill they’re taught and hired for, which is feature development.
3.  I say “constituents” instead of “users” when I describe who benefits from the features of a piece of software. You’ll find all the reasons I shy far away from the word “user” in [this scorcher right here](https://chelseatroy.com/2021/06/28/the-siren-song-of-the-user-model/).

### If you liked this piece, you might also like:

[This series on socializing changes in the workplace](https://chelseatroy.com/2018/02/25/how-to-socialize-big-changes-at-work-part-1-start-at-the-grassroots-level/) (Helpful for getting started on changing your team’s relationship with maintenance load)

[The refactoring category](http://chelseatroy.com/category/refactoring) (For skills that help with addressing maintenance load)

[The risk analysis workshop](https://chelseatroy.com/2020/11/30/rubyconf-workshop-analyzing-risk-in-a-software-system/) (Probably a good “start here” square for the tech team to make a plan for addressing maintenance load)
