---
created: 2024-03-06T09:58:01 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2019/03/25/pearconf-talk-the-technology-and-psychology-of-refactoring/
author: Chelsea
---

# PearConf Keynote – The Technology and Psychology of Refactoring – Chelsea Troy

> ## Excerpt
> PearConf is, in the words of the website: a gathering of people who pair and collaborate to build software. This includes pair programming, pair design, and all of manner of cross-functional work. …

---
Reading Time: 23 minutes

![pearcof 2019 logo](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-4.49.55-PM.png?resize=723%2C362&ssl=1)

PearConf is, in the words of the website:

> a gathering of people who pair and collaborate to build software. This includes pair programming, pair design, and all of manner of cross-functional work. It’s also a gathering of people who want their workplaces to level-up on inclusivity and who, themselves, want to work at being better teammates too.

My keynote talk focused on _The Technology and Psychology of Refactoring._ Here’s the _best_ video of this talk, which I recorded in my house, more than a year _after_ the PearConf premiere, for the supporting speaker track of JuneteenthConf 2020.

<iframe src="https://www.youtube.com/embed/UFDTneLtLSY?version=3&amp;rel=1&amp;showsearch=0&amp;showinfo=1&amp;iv_load_policy=1&amp;fs=1&amp;hl=en-US&amp;autohide=2&amp;start=1&amp;wmode=transparent&amp;listType=playlist&amp;list=PLwWm3SC4yPMzfAvy-JUkNkLqJfYVKt4Ed" allowfullscreen="true" sandbox="allow-scripts allow-same-origin allow-popups allow-presentation allow-popups-to-escape-sandbox" data-ratio="0.5629322268326418" data-width="723" data-height="407"></iframe>

This post also contains the full script interspersed with the relevant slides. The substantive differences between this transcript and the words I said onstage amount to crowd work.

If you’re interested in seeing this talk live, make sure the organizers of your favorite conference know my name. You can send them [this link](https://chelseatroy.com/speaking/) to my speaker page!

### Full Draft Transcript:

Hi folks! My name is Chelsea Troy. My pronouns are she and her. They/them is also fine.

Today I’m going to talk about refactoring.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Refactoring.png?resize=661%2C372&ssl=1)

I don’t think this talk requires any content warnings, but if I missed something and it does I’d like to hear from you afterwards. And in general I’d love to hear your feedback on this talk. You can reach me at chelsea@chelseatroy.com.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Refactoring-1.png?resize=649%2C365&ssl=1)

My hope is that I can provide something for everyone today. We’ll cover some technical-level details for the software engineers in the crowd and some operational details for the product managers.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Refactoring-2.png?resize=645%2C362&ssl=1)

### So what is refactoring?

Can I have a show of hands—who has done a refactor before?

OK, how many of you love refactoring?

OK, how many of you hate refactoring?

So if we’ve done it and we have opinions on it, we definitely know what it is.

It’s kinda one of those “I know it when I see it” things, right?

The canonical definition says it’s something like:

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Refactoring-3.png?resize=670%2C377&ssl=1)

changing the design of existing code without changing the user-facing features.

I don’t love this definition, because it comes with two assumptions baked into it—specifically into the term “user-facing features.”

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-4.58.39-PM.png?resize=668%2C418&ssl=1)

 **1. Who is a user?**

Refactoring changes the developer-facing characteristics of a code base, and developers are users.

I can almost hear the designers in the crowd wincing.

It’s true that in many cases, code design is, and should be, second to the software’s functionality. If the only way to write the feature that end users need is with messy code, then we need to write the feature that way, and that’s life. Because If the software doesn’t work for the end user, it doesn’t matter how beautiful the code is.

But it’s also true that if the code is illegible, the software will very quickly not work for the end user: developers will have a hard time understanding it and be afraid to touch it, so updates will take too long or even become impossible.

No designer wants to hear that a small tweak to this animation is going to take 2 weeks, right?

We keep code in a maintainable state to minimize that kind of wait time for designers, developers, end users—everyone.

OK? There’s another assumption baked into the term “user-facing features.”

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-4.58.47-PM.png?resize=654%2C408&ssl=1)

**2\. What is a feature?**

So we tend to equate “user facing features” to content and navigation features: buttons, tabs, text, pictures, requests…that kind of thing. Sometimes we call these features “functionality.”

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-4.58.55-PM.png?resize=649%2C406&ssl=1)

That’s one kind of feature, but it represents a very small sliver of the totality of features.

**Other kinds of features include:**

-   **scaling:** Does the software work when a lot of users are on it? Does the app work when the network connection is heavily taxed, poor, or nonexistent?
-   **robustness:** Does the software respond gracefully to unexpected input from the user or from network calls?
-   **security:** Does the software keep users and data safe when hackers try to compromise it?
-   **accessibility:** Does the software work for users who use screen readers, large font, high contrast UIs, loud and clear sounds? Does it work for users who have compromised motor control or dexterity?
-   **inclusion:** Does your computer vision app work for people with dark skin? Does your software only ask questions about users’ sex and gender if it absolutely _must_? And if it _must_, does it ask about those things in the right way? Does it work for all different body shapes and sizes?

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-4.59.30-PM.png?resize=616%2C385&ssl=1)

All of these are features, just like profile pages, buttons, and news feeds.

And we often refactor _to get_ some of these features, right? Better error handling, better network performance, or better predictive results—but whether we’d call that a new feature or a refactor isn’t clear.

By labeling our work as a feature or as refactoring based on whether users notice a difference makes some assumptions about who is a user,

and we don’t have to make those assumptions.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-4.59.39-PM.png?resize=555%2C347&ssl=1)

**Instead, let’s imagine that software exists on a continuum toward maintainability, and refactoring is the work that we do to make our code as maintainable as possible.**![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.05.57-PM.png?resize=609%2C381&ssl=1)

### What does it mean for software to be maintainable?

We’re looking for three things, specifically, and I’d like you to think of them as constant works in progress on a software product. Refactoring is the thing we do to give our software _more of_ these qualities.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.06.07-PM.png?resize=638%2C399&ssl=1)

 **1. Collaboration-friendly**

How can other technologists maintain this code together?

This is the thing we most commonly associate with the term refactoring, because we want to leave the code clean and organized so that other developers can figure out what it does, and why.

So, who does this well? I have a few examples for you.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.06.14-PM.png?resize=619%2C387&ssl=1)

The Spring framework makes exemplary use of documentation, rigorous tests, and thoughtful syntax and architecture choices to keep the code legible and thoroughly explained so that their distributed team—and outside contributors—can understand and improve the way it works.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.06.22-PM.png?resize=632%2C395&ssl=1)

Apple’s programming language, Swift, aims to enforce some collaborability on Swift projects.

Swift is a compiled, typed language. It leans on optionals and includes a lot of features to ensure the safety and legibility of the code written in it.

You can see this commitment to concise, legible code in the source, which is also on Github.

When code _isn’t_ collaborable, we end up with arcane legacy systems that developers are afraid to touch for fear of braking them. If the situation gets dire enough, the company may commission a complete rewrite rather than maintaining the existing system.

We want software to be collaboration friendly. We also want it to be:

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.06.38-PM.png?resize=614%2C384&ssl=1)

 **2. Dialogue-friendly:** How does a user or developer share their expectations about this program?

The feedback cycle is an important part of software development, so if we want to develop maintainable software, that software has to merit maintaining by being _used._ 

In order for software to be used, we need to make sure it keeps up with what users and development teams expect it to do. How do we do that?

When we deploy apps, we might do user testing. Sometimes we use app store ratings to get information about how our product is doing.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.06.49-PM.png?resize=611%2C382&ssl=1)

Google’s products often provide mechanisms for users to share how their expectations differed from what they got. These dialogues appeared while I was looking up how to do something in gmail. I can choose whether I found this article helpful, and if I did not, then Google shows me a text field to explain what I needed.

Software that fails to open this dialogue end up getting out of touch with user needs, and they become shelfware.

The third aspect of maintainability is…

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.07.11-PM.png?resize=646%2C404&ssl=1)

**1\. Resilience**

How does the code react to unexpected or unideal inputs?

A resilient app anticipates the things that a user could do, and it reacts gracefully even when something happens that it didn’t anticipate.

When software _isn’t_ resilient, we end up with failures where the software is clearly not working as intended, or where the development team didn’t anticipate something that could happen.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.07.28-PM.png?resize=614%2C384&ssl=1)

I once worked on an app for ramp agents working on the tarmac at airports to scan luggage, pets, mail, military equipment, and hazardous materials into the holds of commercial airplanes.

We build this beautiful app. API clients made calls to a server in real time to scan each bag, and they returned updated information about the whole flight calculated on the server with each call.

We sent it to the tarmac for user testing. The product owner came back to report that ramp agents spent 10% of their time with our app scanning bags.

So what were they doing the other 90% of the time?

They were walking around _on the tarmac,_ which, by the way, is _where the planes go,_ holding the phones _up in the air_.

Why do you think they were doing that?

This was the most likely way to get a network signal, since a network response blocked every single scan.

That wasn’t going to work. So we _completely_ refactored the app, this time with the question in mind “How much stuff can we do without _any_ network connection at all?” We couldn’t coordinate scanning between scanners this way, so we assigned each scanner to a pit in the hold and queued the scanning requests.

At the very end of scanning, the ramp agents needed a network connection to connect to check some security things. They could guarantee this network connection by plugging the phone into a networked holder called a cradle, just once at the end of scanning.

We took an existing app that did not do at all what the users needed, and we worked together to translate that feedback into changes that worked.

This is an extreme example that I chose for clarity, but this happens on _a lot_ of software: requirements change out from under you.

We write maintainable software _precisely_ so that, when the underlying requirements change, our code is ready to change, too.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.07.45-PM.png?resize=630%2C394&ssl=1)

### Writing Maintainable Software…

**…means assuming our assumptions will be wrong.**

Software has to change when it was originally written under assumptions that are not true, or assumptions that are _no longer_ true.

### **How do we hedge against the possibility that our assumptions are wrong?**

We can do that by noticing and examining our assumptions.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.07.55-PM.png?resize=597%2C373&ssl=1)

As I was developing this talk, Kenneth Mayer asked on Twitter: When is enough refactoring, enough?

**And I think it will be helpful to approach this question with an example.**

How many of you have heard of DRY software engineering?

What does DRY stand for?

It stands for “Don’t repeat yourself.” This is an adage developers learn to avoid duplicating stuff in code.

Coraline Ehmke articulates a misunderstanding with this term. She explains that the point of DRY is to not repeat _concepts._ It’s not referring to individual lines of code.

Sometimes it makes sense to leave similar lines of code in several places. Because when you extract a variable or a class or a method for them, you communicate the perspective that these two things are the same. And sometimes they’re not.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.03-PM.png?resize=606%2C379&ssl=1)

Suppose we have a class representing each view in our app. We have a ListView and a DetailView. In each one, we need to load the view, handle button taps, and load up a navigation bar on the screen.

They both do all of these things, so it makes sense to extract a superclass. Let’s call it View.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.11-PM.png?resize=632%2C394&ssl=1)

Now we’re adding login. So we make a LoginView, and we still need to load the view, handle button presses, et cetera. but no navigation bar here, since there’s nothing to navigate to until the person logs in. What do we do?

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.26-PM.png?resize=622%2C389&ssl=1)

Subclass it and no-op it to turn off that functionality?

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.33-PM.png?resize=635%2C397&ssl=1)

Pull loading the navigation bar out of the superclass, and add it back to the subclasses that used it?

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.33-PM.png?resize=640%2C400&ssl=1)

This is a case where creating a collaborator could be a helpful design pattern,

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.41-PM.png?resize=621%2C388&ssl=1)

or creating a taller hierarchy

(sidenote: you see very tall hierarchies in mobile frameworks because of this kind of thing).

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.48-PM.png?resize=587%2C367&ssl=1)

You might extract behavior into an interface, or compose classes together with separate modules. But if you need default functionality rather than interface adherence and you’re working with a language that prohibits multiple inheritance, that kind of pattern may not be an option for you.

So we get a new rule: the rule of 3.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.08.56-PM.png?resize=587%2C367&ssl=1)

What is it? It’s the rule that code should be duplicated in at least 3 places before factoring it out.

Why? What is it about 3? 3 is not a magic number. We could factor out the navigation bar, get 8 views in, and then run into the exceptional class, and have to decide between subclass and no-op or adding the nav bar method to the other 8 classes.

3 is, instead, a proxy heuristic: if we have to do something exactly the same way 3 times, the probability is _higher_ that these things really _are_ the same than if we only have similar functionality twice.

So how do we judge when to start refactoring or when to stop refactoring?

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.09.05-PM.png?resize=723%2C452&ssl=1)

We have refactored enough when we have balanced the probability that this will change times the amount of work to make that change against the amount of work we’re willing or happy to do.

In finance, when we multiply the probability of something happening and the cost if it does happen, we call that the **expected cost**. So in this case we’re balancing the expected cost of maintenance with the cost we’d be happy (or at least willing) to bear.

This means that we need to ask some questions as we plan how to write our maintainable code.

 **1. How likely is it that we’ll have to change this?**

It’s tough to get a definitive number here, but we can use a few cues to get an idea.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.09.31-PM.png?resize=723%2C452&ssl=1)

-   The **more examples** we have in the code that support our assumptions, the more likely it is that those assumptions are still useful, and the less likely it is that we’ll have to overhaul this code later. This is the generalized version of the rule of 3 that we talked about before.
-   The **more customers** we have using our app in the way we thought they would use it, the more likely it is that our understanding of their needs is accurate, and the less likely it is that we’ll have to overhaul the software later.
-   **Regular feedback cycles** allow us to catch and correct differences in user expectations and software requirements early on—so it’s less likely that we’ll have to make sweeping changes later.

By asking “how likely is it that we’ll have to change this,” we can estimate the probability part of our expected cost of maintenance. We still need to estimate the amount of work part.

 **2. How much work is this to change?**

Again, it’s tough to get a definitive number, but we can take some things into account as we make a guess.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.09.40-PM.png?resize=723%2C452&ssl=1)

-   **Consolidating is usually less work than separating**
    -   It’s less work to consolidate our views to inherit from a superclass with duplicate functionality than it is to break that superclass apart again when we realize it represents a concept that is too general for our use case.
-   **It’s usually less work to change documented patterns than undocumented patterns**
    -   This is important when deciding whether to go with system patterns or your own bespoke ideas for writing software. The more commonplace the pattern you choose, the more google-able they’ll be if team members run into issues.
-   **It’s usually less work to change an existing system than to start over**
    -   Refactoring the scanning app, as monumental a project as it was, still involved less work than rewriting the app would have been. Even sweeping refactors can save the team down the line from needing to do a complete rewrite. By the same token, replacing the plumbing in a building may be an expensive and onerous task—but it still beats ripping down the building and starting over.

That’s why it is worthwhile for teams to make these sweeping refactors, and it is worthwhile for technologists to build their skills in making sweeping refactors.

### **Planning and selling the refactor**

In many cases, sweeping refactors require some negotiation. Stakeholders don’t love to hear that we need to sink a bunch of developer time into a major change that won’t result in more navigation or content features. To them, that sounds like a lot of risk for no upside.

As I was developing this talk, Brent Halliburton asked on Twitter: as a product manager, what can I do to help my eng team be successful in appropriate refactoring over the life of a codebase?

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.10.02-PM.png?resize=723%2C452&ssl=1)

Here’s the information that a product manager can collect to make space for the development team to refactor:

**1\. What future developments does this change affect, and how does it affect them?**

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.10.10-PM.png?resize=723%2C452&ssl=1)

People respond to the prospect of losing something. The product manager can figure out, what _perceived pain or loss_ does this change address?

Maybe refactoring an inefficient server pattern will allow your company to save $8k of the $10k it’s currently spending per month on AWS costs.

Maybe this refactor allows multiple engineers to more easily work in the same part of the app, so work streams don’t get blocked and execs don’t pay developers to sit around waiting in line to deliver code.

Finding that pain or loss will help product managers make a case for a sweeping refactor being mission-critical. The tech team might know something about that pain or loss, but the sales team, the product owners, and potential customers can also help uproot these worries that the changes can address.

**2\. How can we maximize value delivery over the life of making this change?**

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.10.17-PM.png?resize=723%2C452&ssl=1)

If we have to make a sweeping change, how can we incrementally perform this change to produce benefits from the very beginning, so the business doesn’t have to wait on results?

Suppose, for example, you have a server that clients can query for the contents of federal laws. That’s a lot of text, so the responses take a long time to serialize and deliver over HTTP. Also, no human can effectively interpret that much text at once.

You’re working on a filtering feature so that this endpoint will now only serve the most relevant pieces of text for what the user is looking for. You _never_ want the call to take as long as it currently does.

How do you deliver user value before the new filtering feature is done? Maybe you can set up a temporary endpoint that delivers the most-read pieces of text and redirect to that while you finish the filtering. Maybe you can pre-populate some suggested search terms and make an interim version of the filter that only serves up results for those terms.

It’s valuable to think creatively about the question: _How do I deliver the most value to the user with the smallest amount of work?_ The best tech teams can leverage this question to sneak through monumental undertakings, piece by piece, delivering value the whole way.

### **Let’s talk about Allies.**

Even with the best sales strategy, sometimes your team might need a little help to get buy-in on a sweeping refactor. Who can you turn to for help? You have a few options that are often overlooked, so I’d like to highlight them.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.10.32-PM.png?resize=723%2C452&ssl=1)

-   **The sponsor of the project**
    -   This is not necessarily the end user. If you’re writing an app for a health insurance company’s enrollees, the enrollees are the end user, but that’s not who creates demand for the app. The demand comes from the insurance company’s clients: employers who offer this insurance to their staff. If you can convince these sponsors and the people who speak for them that this refactor is important, you can open doors that otherwise might be closed.
-   **The peanut gallery**
    -   The peanut gallery is a term with its origins in French Vaudeville performances, and it refers to the top-most gallery of the theater where the cheapest seats were located. People sat there and ate peanuts, and sometimes threw them at the stage. In modern parlance it’s slang for all the people who are removed from the action but nonetheless feel the need to lob their comments around, which usually aren’t that useful.
    -   It can be annoying when stakeholders have conflicting views of what should go into the software. Conveniently, if your refactor furthers several of these stakeholders’ goals, it might skyrocket to the front of the priority list by virtue of being the only thing multiple stakeholders agree on.
-   **The person who wrote it the first way**
    -   If you’re refactoring something, that means it was originally written some other way. It can be tempting to denigrate the original choices and even the people who made them.
    -   I recommend doing the opposite. The people who wrote it the first way might have intimate knowledge of the technological or institutional limitations that forced the original design.
    -   They are also the ones with the most practical perspective from which to advise on how they would do this feature, if they had it to do again.
    -   I recommend valuing this person’s work and seeking their counsel. I once worked on a rewrite that would have gotten completely blocked on some idiosyncratic API details known only to the person who wrote the thing we were rewriting. Without his help, the rewrite would have failed. We ended up naming a whole menu in the rewritten application in his honor.

### To Wrap Up

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.10.47-PM.png?resize=723%2C452&ssl=1)

We generally think of refactoring as improving the design of existing code without changing user-facing features. That’s a hard line to draw, and I don’t think we have to draw it.

Instead, we can focus on making our code more maintainable: more collaboration-friendly, more dialogue-friendly, and more resilient.

We can do that by making an effort to notice and examine our assumptions, and to consider the probability and the cost of our assumptions being wrong later on. This puts us in a good position to adapt our software to change.

And change, even big change, is often more practical than rewriting software from scratch. But how do we sell the business on big changes, especially if they don’t come with obvious changes to the software’s functionality?

We need to practice the psychology of refactoring. We can identify the pain that our refactors address and sell our ideas that way, then plan our work so that we deliver incremental value the whole way through. And, when it comes to it, we can reach out for help, remembering that sometimes, that help might come from some unlikely places.

If you’re interested in the topic of refactoring, I’d like to recommend some…

### Additional resources

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.10.56-PM.png?resize=723%2C452&ssl=1)

Christin Gorman talks about some excellent practical examples in [her presentation on how to make your code sustainable](https://vimeo.com/138774243), which she gave at JavaZone 3 years ago. I have watched this talk like 9 times, and I have sent it to other developers many more times than that.

Michael Feathers’ book [_Working Effectively with Legacy Code_](https://www.oreilly.com/library/view/working-effectively-with/0131177052/) is a go-to for me. I keep a copy on my desk at all times. Book examples are in Java, but I have used lessons from this book on projects in Swift, .Net, Kotlin, Python, Ruby, Java, and Golang.

I also recommend Cohere’s workshop on the subject of [Real-World Refactoring](https://www.wecohere.com/workshops/real-world-refactoring/?hsCtaTracking=6052b9ab-370d-4764-a202-f8a56d4eaf4c%7C5bfecfb7-d7aa-45eb-8982-a4af63e24568). Code examples in this workshop are in Ruby, but I think the material would be accessible to anyone with some object-oriented app development experience in any language.

### Questions

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/03/Screen-Shot-2019-03-23-at-5.11.04-PM.png?resize=723%2C452&ssl=1)

### End of Draft Transcript

Once again if you want to see me deliver this at a stage near you, [send this link to someone who can make it happen](https://chelseatroy.com/speaking/).

And if you like reading what I have to say about refactoring, I scraped up a few other posts on the topic that I thought you might enjoy.

### Other Times I Wrote About Refactoring:

I watched Michael Feathers’ talk on Edge Free Programming and wrote a [watcher’s guide](https://chelseatroy.com/2017/12/04/edge-free-programming-by-michael-feathers/)—this speaker is the person who wrote _Working Effectively with Legacy Code._ I find value in what he has to say.

I gave a full screencast on Avdi’s show, _RubyTapas,_ where I draw architecture diagrams and share code _before your very eyes!_ You can see the screencast [right here](https://chelseatroy.com/2018/12/18/risk-oriented-testing-from-rubytapas-screencast/). It’s about testing. You will love it.

Here’s [a case study](https://chelseatroy.com/2018/09/20/adventures-in-process-design-a-case-study-on-data-import/) of a refactor I did on a major data import feature with a high potential cost if things went wrong.
