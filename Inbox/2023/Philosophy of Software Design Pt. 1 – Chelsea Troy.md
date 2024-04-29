---
created: 2024-03-05T13:52:52 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2019/12/17/philosophy-of-software-design-part-1/
author: Chelsea
---

# Philosophy of Software Design: Pt. 1 – Chelsea Troy

> ## Excerpt
> I recently read John Ousterhout’s book, Philosophy of Software Design. This blog post includes my commentary on some parts that stuck with me. This is neither a review nor a summary of the bo…

---
Reading Time: 10 minutes

I recently read [John Ousterhout](http://web.stanford.edu/~ouster/cgi-bin/home.php)‘s book, _Philosophy of Software Design._ This blog post includes my commentary on some parts that stuck with me.

![philosophy of software design](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/philosophy-of-software-design-4090960235-1576552484600.jpeg?resize=189%2C268&ssl=1)

This is neither a review nor a summary of the book.<sup>1</sup> Instead, if you and I were in a book club and this book were the topic of our next meeting, I would likely bring up the following things in that meeting.

### The audience for _Philosophy of Software Design_

The author is a teacher at Stanford, so parts of this book are understandably written with a student audience in mind (CS undergrads or grad students).

So, as a professional software engineer, I chuckled at first when I read pieces of advice in it like “Don’t repeat your code exactly in the comments.” I would not do this today. But I remember being a student and thinking that surrounding all my code with comments would help cement my aspirational reputation as the author of the world’s most legible code.

I spoke to a few other professional software engineers who also have read this book. They mentioned having similar experiences with this book; however, they experienced it with _different_ parts of the book than I did. That’s fascinating: the four of us turned out to all have very different maps of which parts of the book felt “obvious” to us. So, if you pick up this book and find some advice in it that you think is beneath you, I encourage you to keep that in mind.

### Eschewing Complexity

The book hangs on the guiding principle of keeping our code as simple as possible. That’s not a unique stance in programming texts, but I appreciate the taxonomy that Ousterhout provides to understand why code gets complex in the first place. To wit:

> Three ways in which complexity manifests itself in software systems:
> 
> 1.  **Change amplification:** a seemingly simple change requires code modifications in many places.
> 2.  **Cognitive load:** in order to make a change, the developer must accumulate a large amount of information.
> 3.  **Unknown unknowns:** it is unclear what code needs to be modified, or what information must be considered in order to make those modifications.

Here we have a list of symptoms that allow us to diagnose complexity in our code bases. We change something in one place, and the thing still doesn’t work until we later realize that we must also change something somewhere else. We have to accumulate a lot of background knowledge before we understand what we have to do. We can’t even figure out where we need to change something to get the effect we want. To a programmer of tenure, these are smells. To a programmer without tenure, these are subtle indications that it is _them_, not the code, that is lacking in this interaction. That’s why it’s critical to attach memorable language to these principles, so programmers can describe precisely what’s giving them trouble.

The book discusses the symptoms of complexity, then identifies two causes: **dependencies** and **obscurity.** The remainder of the book, more or less, discusses techniques to guard against these two things.

Some of those techniques sound at odds with the prevalent interpretation of conventional programming advice.

For example, object-oriented programmers favor the idea of lots of little objects, rather than large ones. This principle, taken to its logical extreme, produces some ridiculous results. For example, in the Javascript package manager npm, we have two libraries: `left-pad` (which adds a padding to the left side of the string with the character of your choice or just blank space), and `right-pad` (a different library that _depends_ on left-pad and does the same thing, but on the right). The sheer number of things that depend on `left-pad`, in fact, made the whole JavaScript dependency tree sufficiently brittle to cause mass app crashes and [controversy](https://arstechnica.com/information-technology/2016/03/rage-quit-coder-unpublished-17-lines-of-javascript-and-broke-the-internet/) when the author of `left-pad` pulled it from npm).

Back to the book, which instead favors deeper objects. Ousterhout explains:

> “It’s more important for a module to have a simple interface than a simple implementation.”

![classes should be deep](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/classes-should-be-deep.png?resize=318%2C159&ssl=1)

Related responsibilities should live in the same class, which can have lots of functionality provided behind a simple interface.

### Unspecialing Special Cases

The book points out another thing that holds true in my experience: that a lot of complexity comes from handling exceptions and edge cases. I have done no quantification around this, but I mentally characterize this observation with the [Pareto Principle](https://en.wikipedia.org/wiki/Pareto_principle)<sup>2</sup>: maybe 80% of the spaghetti-ness of the code I read exists to deal with 20% of the cases that the code will encounter.

The book suggests finding ways, instead, to define exceptions and special cases out of existence. The book discusses an example: writing code to handle selection in a text editor. Rather than throwing an error when no text is selected, the book recommends setting the beginning and end indices of the selection to default to the same number, such that methods can still be _called_ on the selection, but if nothing is selected, they won’t do anything. Imagine if you text editor of choice crashed each time you hit the bold button, or the italic button, without any text being highlighted. It’s better for the thing to accept the instruction and do nothing than cause that frustration.

Here I want to stop for a second and talk about the nature of examples in software literature.

### Examples in Software Literature

I appreciate Ousterhout’s word processor example choice, and I want to explain why.

Software, by its nature, is _abstract_: we use it to represent concepts. That layer of abstraction sits between the reader and the concept that an author of a programming book might be trying to explain. How do we eliminate, or at least minimize, this confusion?

Many authors endeavor to do it by having their examples reference _physical_ objects. Uncle Bob’s books famously do a step-by-step software implementation of the Mark IV Coffee Maker. _[Practical Object-Oriented Design in Ruby](https://www.poodr.com/)_, by Sandi Metz, describes the object-oriented approach to building a bicycle.

The _goal_ here is noble: to choose a domain that readers already understand, so that they don’t have to learn new concepts unrelated to software in order to access the example. The problem: we do not build coffeemakers and bicycles out of software. So when we’re discussing separation of concerns, the ease with which you can do that in these examples is fake. Yes, _clearly_ there is a different set of responsibilities for a gear and a handlebar, or a water heater and a coffee pot. The boundaries between the concepts that we represent in most software—like users and accounts—are much, _much_ fuzzier. It’s this messiness that programming texts miss, which forces programmers to go forth and bridge the gap between these toy examples and the abstract world on their own.

By contrast, most of the examples in _Philosophy of Software Design_ come from real projects. Mind you, not all of these examples succeed at minimizing confusion. For example, the book regularly discusses the implementation of RAMCloud, which does super-high-speed storage for large-scale datacenter applications running on a fleet of servers. That’s the kind of thing you have to have a non-negligible amount of software background to care about. These examples therefore face a significant obstacle to getting their point across for folks unfamiliar with RAMCloud’s use case.

This word-processor example, though. _YES._ Most folks learning to write code have used at least one word processor. They know what this is. It’s a real thing we build with software, whose use case the reader understands. Other resources that nail this example choice, in my opinion, include _[The Ruby on Rails Tutorial](https://www.railstutorial.org/)_ by Michael Hartl. The whole book implements a blogging app; that’s a real software use case, and people know what a blog is.

### **Back to defining special cases out of existence.**

The idea here isn’t new. We did a few blog posts on a parallel idea that Avdi Grimm describes in his keynote, “[Build Graceful Processes](https://chelseatroy.com/2018/09/03/build-graceful-processes-an-approach-to-code-design/).” Michael Feathers gets at the same idea in his talk (and someday book) on “[Edge-Free Programming](https://chelseatroy.com/2017/12/04/edge-free-programming-by-michael-feathers/).”

I would love to spend some more time myself learning techniques (and maybe building a framework) to do this. I understand that in a survey text like _Philosophy of Software Design_, though, it makes sense to introduce the idea and move on.

### Next Time

I’ll do three posts about this book. In the [next one](https://chelseatroy.com/2019/12/30/posd-2-what-causes-insidious-bugs/), we’ll talk about comments, naming, and possibly either performance concerns or trends.

<sup>1</sup> I won’t review _Philosophy of Software Design_, but I will provide one usage recommendation: I recommend it as an alternative to _Clean Code,_ by Uncle Bob Martin. That book has three problems:

1.  The author famously has a [narrow view](https://medium.com/@laurenfraz/the-prob-with-bob-8600074c9236) about who belongs in software engineering, eschews rigor in his understanding of social issues in the programming community, does things like [publicly defend](https://medium.com/@BradleyHolt/what-uncle-bob-gets-wrong-c01d85c52163) Google’s misogynistic ex-manifesto author [James Damore](https://medium.com/@yonatanzunger/so-about-this-googlers-manifesto-1e3773ed1788), and regularly [doubles down](https://blog.cleancoder.com/uncle-bob/2017/08/14/WomenInTech.html) when held accountable by his more inclusion literate colleagues. His behavior injects hostility into the industry.
2.  In addition, his behavior indicates low skill acquisition relative to career length. Learning from others’ experiences is one of _the biggest_ leveling up hacks in the game, and Bob ain’t doin’ it, or else he would know by now that not all programmers are straight white men who met their CLI at age 13 (a heavy dose of the examples in _Clean Code_ and _The Clean Coder_ lean hard on this assumption). If you refuse to listen to other people—if you refuse to use that hack—you _cannot_ stay competitive skill-wise with colleagues who are using it.
3.  As evidence of the above, _Clean Code_ is not an extraordinarily good book. It misses critical concepts. It oversimplifies complex ideas. It overcomplicates simple concepts. It explains several things with unnecessary vitriol. It uses contrived examples, like building a coffeemaker; since we don’t build coffeemakers out of software, the example adds a layer of indirection to everything it describes.

Let me be clear; I have read a few programming books (I’ve written blog posts on some: [here](https://chelseatroy.com/tag/sicp/) [are](https://chelseatroy.com/tag/crafting-intepreters/) [three](https://chelseatroy.com/2018/01/01/one-page-notes-confident-ruby-by-avdi-grimm/) of my favorites). _Clean Code_ does not crack the top half of the books I’ve read in its utility. Frequently when I (or any of my colleagues) bring up these arguments, recommenders of _Clean Code_ ask “Then what’s the alternative?” Rest assured, there are dozens of alternatives to _Clean Code_ that deliver all the useful messages with none of the detrimental ones. If you’re looking for a title to replace _Clean Code_ in your automatic new-engineers gift drive, you could do a lot worse than _Philosophy of Software Design._

<sup>2</sup> That’s right, this principle has a history that dates back literally 100 years before Tim Ferriss or Ramit Sethi made YouTube videos about it.

### If you liked this piece, you might also like:

[This discussion and case study on building graceful processes](https://chelseatroy.com/tag/adventures-in-process-design/) (2 posts)

[This piece on the Edge-Free Programming talk](https://chelseatroy.com/2017/12/04/edge-free-programming-by-michael-feathers/) (1 post, and not even a long one)

[The API Design Series](https://chelseatroy.com/2018/08/01/api-design-part-1-before-there-was-rest/) (3 posts)
