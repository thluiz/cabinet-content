---
created: 2023-07-19T18:06:48 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2023/07/17/works-on-most-machines/
author: Mark Seemann
---

# Works on most machines

> ## Excerpt
> TDD encourages deployment flexibility. Functional programming also helps.

---
_TDD encourages deployment flexibility. Functional programming also helps._

Recently several of the podcasts I subscribe to have had episodes about various container technologies, of which [Kubernetes](https://en.wikipedia.org/wiki/Kubernetes) dominates. I tune out of such content, since it has nothing to do with me.

I've never found containerisation relevant. I remember being fascinated when I first heard of [Docker](https://en.wikipedia.org/wiki/Docker_(software)), and for a while, I awaited a reason to use it. It never materialised.

I'd test-drive whatever system I was working on, and deploy it to production. Usually, it'd just work.

Since my process already produced good results, why make it more complicated?

Occasionally, I would become briefly aware of the lack of containers in my life, but then I'd forget about it again. Until now, I haven't thought much about it, and it's probably only the random coincidence of a few podcast episodes back-to-back that made me think more about it.

### Be liberal with what system you run on [#](https://blog.ploeh.dk/2023/07/17/works-on-most-machines/#98b5a360a5ba413ca4dbccce86cbe331)

When I was a beginner programmer a few years ago, things were different. I'd write code that [worked on my machine](https://blog.codinghorror.com/the-works-on-my-machine-certification-program/), but not always on the test server.

As I gained experience, this tended to happen less often. This doubtlessly have multiple causes, and increased experience is likely one of them, but I also think that my interest in loose coupling and test-driven development plays a role.

Increasingly I developed an ethos of writing software that would work on most machines, instead of only my own. It seems reminiscent of [Postel's law](https://en.wikipedia.org/wiki/Robustness_principle): Be liberal with what system you run on.

Test-driven development helps in that regard, because you write code that must be able to execute in at least two contexts: The test context, and the actual system context. These two contexts both exist on your machine.

A colleague once taught me: _The most difficult generalisation step is going from one to two_. Once you've generalised to two cases, it's much easier to generalise to three, four, or _n_ cases.

It seems to me that such from-one-to-two-cases generalisation is an inadvertent by-product of test-driven development. Once your code already matches two different contexts, making it even more flexible isn't that much extra work. It's not even [speculative generality](https://wiki.c2.com/?SpeculativeGenerality) because you also need to make it work on the production system and (one hopes) on a build server or continuous delivery pipeline. That's 3-4 contexts. Odds are that software that runs successfully in four separate contexts runs successfully on many more systems.

### General-purpose modules [#](https://blog.ploeh.dk/2023/07/17/works-on-most-machines/#a0315ee333ff454fb1bd6814f5806121)

In [A Philosophy of Software Design](https://blog.ploeh.dk/ref/a-philosophy-of-software-design) John Ousterhout argues that one should aim for designing general-purpose objects or modules, rather than specialised APIs. He calls them _deep modules_ and their counterparts _shallow modules_. On the surface, this seems to go against the grain of [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it), but the way I understand the book, the point is rather that general-purpose solutions also solve special cases, and, when done right, the code doesn't have to be more complicated than the one that handles the special case.

As I write in [my review of the book](https://www.goodreads.com/review/show/5498011140), I think that there's a connection with test-driven development. General-purpose code is code that works in more than one situation, including automated testing environments. This is almost tautological. If it doesn't work in an automated test, an argument could be made that it's insufficiently general.

Likewise, general-purpose software should be able to work when deployed to more than one machine. It should even work on machines where other versions of that software already exist.

When you have general-purpose software, though, do you really need containers?

### Isolation [#](https://blog.ploeh.dk/2023/07/17/works-on-most-machines/#960f22bd15fb48b1a3d7dfddc9f60408)

While I've routinely made use of test-driven development since 2003, I started my shift towards functional programming around ten years later. I think that this has amplified my code's flexibility.

As [Jessica Kerr](https://jessitron.com/) [pointed out years ago](http://www.functionalgeekery.com/episode-8-jessica-kerr), a corollary of [referential transparency](https://en.wikipedia.org/wiki/Referential_transparency) is that [pure functions](https://en.wikipedia.org/wiki/Pure_function) are _isolated_ from their environment. Only input arguments affect the output of a pure function.

Ultimately, you may need to query the environment about various things, but in functional programming, querying the environment is impure, so you [push it to the boundary of the system](https://blog.ploeh.dk/2016/03/18/functional-architecture-is-ports-and-adapters). Functional programming encourages you to [explicitly consider and separate impure actions from pure functions](https://blog.ploeh.dk/2018/11/19/functional-architecture-a-definition). This implies that the environment-specific code is small, cohesive, and easy to review.

### Conclusion [#](https://blog.ploeh.dk/2023/07/17/works-on-most-machines/#4775b8ab484c4833a6a5a86bac3b8b8e)

For a while, when Docker was new, I expected it to be a technology that I'd eventually pick up and make part of my tool belt. As the years went by, that never happened. As a programmer, I've never had the need.

I think that a major contributor to that is that since I mostly develop software with test-driven development, the resulting software is already robust or flexible enough to run in multiple environments. Adding functional programming to the mix helps to achieve isolation from the run-time environment.

All of this seems to collaborate to enable code to work not just on my machine, but on most machines. Including containers.

Perhaps there are other reasons to use containers and Kubernetes. In a devops context, I could imagine that it makes deployment and operations easier. I don't know much about that, but I also don't mind. If someone wants to take the code I've written and run it in a container, that's fine. It's going to run there too.
