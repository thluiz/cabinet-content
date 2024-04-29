---
created: 2023-05-10T14:44:21 (UTC -03:00)
tags: []
source: https://lakret.net/blog/2023-05-08-elixir-vs-microservices
author: 
---

# Monolith vs Microservices: With Elixir You Don't Need to Choose

> ## Excerpt
> Dmitry Slutsky's Software Consulting Services

---
There’s a very simple way out of the monolith vs. microservices discussion that was heating up again lately ([1](https://www.primevideotech.com/video-streaming/scaling-up-the-prime-video-audio-video-monitoring-service-and-reducing-costs-by-90), [2](https://world.hey.com/dhh/even-amazon-can-t-make-sense-of-serverless-or-microservices-59625580), [3](https://twitter.com/kelseyhightower/status/1654098279116992513)) - you don’t need to choose, as long as:

-   you have clear modular boundaries in your code and
-   you use a language / framework that allows you to easily extract those modules to a separate services if needed.

Elixir in particular shines as such a language, and Phoenix is probably the best framework for that too. Ditto for any language that has a good module system or any web framework that uses the actor model. Let’s look at the whys, but first - my thoughts on the debate.

## Don’t Use Microservices Lightly

I think it’s beyond clear at this point that microservices are overused. I wouldn’t recommend anybody to even consider using such architectures at least until they have hundreds/thousands of developers and many million lines of code. Otherwise, you’re essentially turning function calls into network hops, with all the [fallacies of the distributed computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing), associated complexity, costs, and latency invariably biting you and slowing any product progress to a standstill.

Microservices also have a negative effect on the engineering organization’s cohesion: instead of having a shared asset that everybody contributes to and that needs to have a certain level of technical conformity, the engineering org is split into many small fiefdoms that often would use extremely variable processes & tools - making any significant improvement on the organization’s scale nigh impossible.

It’s also quite clear that it pretty much never makes sense to _start_ with such an architecture - you will severely hamper your ability to iterate early on in the product’s lifecycle, dramatically increasing the risks.

Furthermore, microservices are the very extreme version of a significantly more reasonable [service oriented architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture). It does make sense even at smaller organization scales to use 3rd party or self-built (macro)services: if something is clearly out of the main flow of the product, it’s both low risk and can speed up development. Why on earth would you want to split that _separate_ and _contained_ functionality further at this stage is not clear & I’m yet to hear/read any reasonable justification of it.

## … But You Should Still Build Modular Software & Invest in DevOps

That being said, none of that is an excuse to drop the ball on modularity. Software that doesn’t have a clear modular structure is hard to maintain and will be riddled with foot guns and surprise compile and runtime circular dependencies.

Explicitly thinking about modular boundaries in your software also forces you to model your domain explicitly. There are various approaches you can take, from plain “functionality-based” modules all the way to a [domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design).

There are also easy to verify criteria for how reasonable a given modular architecture is, such as:

-   Looking at the physical layout of the modules in the program, can you easily map them to the functionality of the system? Can you guess where a specific product feature will land?
-   Are there circular dependencies between modules? If yes, most likely those modules are either artificially split and should be merged, or the base principle of choosing what constitutes a module in the system is faulty and needs to be reconsidered.
-   How [cohesive](https://en.wikipedia.org/wiki/Cohesion_(computer_science)) are those modules? Does each module operate on a set of data “owned” by that module primarily? Do the functions in the module solve a clearly defined problem / manage the same set of data? How many in/out-bound edges would go to/from each module to other modules?

Having modular software allows you to essentially replace any of those modules with a separate service or a 3rd party solution with a relative ease. It also simplifies testing and, especially, maintenance of software.

Same goes for your infrastructure: if you don’t have CI/CD, no-downtime deploys, etc. development experience will suck no matter if you’re doing monoliths or microservices. So while I’m not a fun of microservices, I’m a [fun of Kubernetes](http://localhost:4000/blog/2023-02-14-new-beginning) - and I see no contradiction there :)

## How Do You Scale Then?

There are also legitimate cases where you need to extract a specific part of your application into a separate service / you need to split a larger application into a set of services / create [outposts](https://m.signalvnoise.com/the-majestic-monolith-can-become-the-citadel/):

-   traffic / performance hot spots
-   part that requires maintenance from people with a specific expertise
-   part that requires specific hardware (easy example is ML models) or topological placement (e.g., edge)
-   cost reductions / acceleration by cutting out non-critical parts of the system to 3rd parties, etc.

Here’s where a language like Elixir can be your best friend. Elixir happens to combine:

-   A strong **[module system](https://elixir-lang.org/getting-started/modules-and-functions.html)** with [built-in tools](https://hexdocs.pm/mix/1.13.4/Mix.Tasks.Xref.html) to inspect & analyze cross-module dependencies, and quality of life niceties that allow you to [easily delegate function calls to other modules](https://hexdocs.pm/elixir/1.12/Kernel.html#defdelegate/2) and git rid of pretty much any boilerplate you want with a [hygienic macro system](https://elixir-lang.org/getting-started/meta/macros.html) - the language is mostly homoiconic (i.e., the [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree) of an Elixir program is a tree of Elixir tuples - code & the AST of that code [look very close](https://elixir-lang.org/getting-started/meta/quote-and-unquote.html)), so meta-programming is a breeze. And yes, I wrote an [extensive guide](https://lakret.net/blog/2019-03-01-elixir-modularity-toolbox) on different levels of modularity you can achieve with Elixir all the way back in 2019.
-   Built-in **[actor system](https://en.wikipedia.org/wiki/Actor_model)**: any Elixir code is already executed inside an actor (called [Process](https://elixir-lang.org/getting-started/processes.html) in Elixir), and those actors are memory-isolated from each other, have a clearly defined error-handling & supervision behavior and communicate with each other via message passing, allowing for (also supported out of the box!) [network transparency](https://hexdocs.pm/elixir/1.12/DynamicSupervisor.html). Essentially, you can take any function/module/part of a larger system and extract it into an actor/set of actors and easily scale or [move it to other machines](https://elixir-lang.org/getting-started/mix-otp/distributed-tasks.html). And yes, all of that with minimal overhead - you can easily run millions of those actors on a single machine, with such feats as handling 2 million websocket connection on one machine [achievable](https://phoenixframework.org/blog/the-road-to-2-million-websocket-connections).
-   Built-in **concurrency solution**: your program can automatically scale across cores and supports async from the get go without any [red functions](https://journal.stuffwithstuff.com/2015/02/01/what-color-is-your-function/) or special syntax. And when cores on one machines is not enough, you can move it to multiple machines without extensive modifications to your code! As long as you have a reasonable infrastructure approach, of course :) And, naturally, Elixir’s distribution & actor discovery mechanisms integrate very nicely with solutions [such as Kubernetes](https://github.com/bitwalker/libcluster) or [edge computing](https://fly.io/docs/elixir/the-basics/clustering/).
-   All of that comes with a **strong failure isolation model**: some actors are [Supervisors](https://elixir-lang.org/getting-started/mix-otp/supervisor-and-application.html) of other actors, which allows them to monitor their “children”, restart or kill them off on failure, [scale them](https://hexdocs.pm/elixir/1.12/DynamicSupervisor.html) in case of increased load, or escalate failures to their own supervisors. You can see how this allows you to deal with the aforementioned distributed computing challenges later on if you really need to tread those waters.

Essentially, you can start with a monolith (as you almost always should anyways!) and then, if the need arises, extract any parts warranting that into separate services with minimal changes to your code. And if you need to transition that to microservices eventually? You already will have both the modularity needed for that in any case, the devops setup that will allow you to continue splitting your application if you really need that, and it will require significantly less adjustment for your developers.

## Phoenix Sweetens the Deal Even More

That would’ve already been a strong proposition to consider Elixir as a good starting point for building systems that might need to grow / scale dynamically, but there’s yet another huge plus with going Elixir: the [Phoenix (Web) Framework](https://www.phoenixframework.org/).

Phoenix comes with many benefits, the headline one being [LiveView](https://github.com/phoenixframework/phoenix_live_view) - a way to build rich interactive web user interfaces without (or with minimal) JavaScript, using server-rendered HTML & WebSockets. It’s probably the fastest way to iterate on a web UI I’ve seen so far, and it can be used to deliver production apps - I used it since 2020 in prod without any major problems in several apps.

But Phoenix is relevant to this microservices discussion too:

-   It encourages modular architecture by specifically adopting DDD idea of [aggregates & aggregate roots](https://martinfowler.com/bliki/DDD_Aggregate.html) in [Contexts](https://hexdocs.pm/phoenix/contexts.htm). Naturally, the Phoenix itself is built on this principle too, so it allows you to, for example, easily use different databases for different parts of the system via different [Ecto.Repos](https://hexdocs.pm/ecto/Ecto.Repo.html).
-   Every request in Phoenix is handled by an actor - naturally taking advantage of the failure isolation, supervision, network transparency, and other useful actor model properties.
-   You can further split your app into multiple Phoenix and non-Phoenix sub-applications using [umbrella projects](https://elixir-lang.org/getting-started/mix-otp/dependencies-and-umbrella-projects.html), and deploy those applications to the same or different node(s). This allows you to both reuse common code with ease, without managing external packages/artifacts, and have a full freedom to define your topology however you see fit. It’s a natural fit for a monorepo pattern too.
-   Naturally, Phoenix nodes can be clustered together via the built-in clustering mechanism or Kubernetes / other node discovery mechanism.

## What If I Can’t Use Elixir?

In this case, you can take those ideas and try to apply it to a language or framework you are using. Some examples:

-   Rust also has a good [module system](https://doc.rust-lang.org/book/ch07-02-defining-modules-to-control-scope-and-privacy.html) built-in, as well as a feature called [workspaces](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html) - essentially the same as the Elixir’s umbrella projects. [Actix Web](https://actix.rs/) framework allows you to use [actors](https://actix.rs/docs/actix/getting-started) as well, and [Axum](https://github.com/tokio-rs/axum) though not being built on actors has good modularity-enhancing features such as the [Tower Service trait](https://docs.rs/tower/latest/tower/index.html) abstraction for reusable middlewares.
-   Python also has a [module system](https://docs.python.org/3/tutorial/modules.html) and a set of conventions for designing modular software: don’t do `import *`, use `_private` naming convention for functions and modules, avoid circular dependencies, etc. Even though neither Django nor Flask have anything similar to Phoenix’s contexts, nothing prevents you from adopting similar module boundaries pattern in those frameworks either.

The list goes on. Apart from languages that don’t have module system at all, you can always design your software in such a way that even if you eventually need to adopt SOA/microservices, it won’t be nearly as much pain as the “monolith refactoring” projects of the epic architecture tales.
