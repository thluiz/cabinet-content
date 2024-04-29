---
created: 2023-10-26T06:41:42 (UTC -03:00)
tags: [Erlang,Fault Tolerance,Supervisors,Durable Execution]
source: https://flawless.dev/essays/when-letting-it-crash-is-not-enough/?utm_source=tldrnewsletter
author: Bernard Kolobara
---

# When "letting it crash" is not enough

> ## Excerpt
> This essay explores the connection between Erlang's process supervision,
                 state management and durable execution.

---
 [![Flawless Logo, a beautiful woman with freckles head illustration.](https://flawless.dev/img/logo.png) flawless.dev](https://flawless.dev/)

When it comes to computer systems, The [Erlang/OTP](https://www.erlang.org/) programming platform has an interesting approach to failure handling. It's also widely known under the name "let it crash". The core idea behind it has to do with the fact that modern applications have a huge number of states that they can find themselves in. The more complex your application is, the more variables you need to keep track of everything. Eventually it becomes impossible for developers to predict all combinations of state that these variables will form. Once your app gets into an undesirable state, the best thing you can do is to reset it and start from a fresh, well known and correct state.

This works amazingly well together with Erlang's property that each process has a separate memory space. Allowing us to selectively reset only parts of our application, while keeping the system as a whole running. Then we can divide the application into a tree-like structure and keep restarting parts of it until we reach the root and are forced to restart the whole thing. This concept is also known as [supervision trees](https://www.erlang.org/doc/design_principles/des_princ).

If you take a closer look at Erlang's approach, this is exactly what a human would do. If your phone gets stuck playing a video, you will try restarting the app, and if this doesn't fix it, you might end up restarting your phone. Or even the router of your home internet connection. You intuitively know that propagating the failure through your home will eventually bring all devices into a state that resembles the one when you first set everything up and everything was working.

Even sometimes Erlang's claims around resilience and fault tolerance are a bit **[overblown](https://stackoverflow.com/a/26447543)**, I still think that it's a very powerful way to deal with failure. If my web app breaks, the first thing I do is to restart the server. In the majority of cases this immediately fixes the issue, while I dive deeper into the logs trying to figure out what went wrong. Having this automatic restarting capability at every level of abstraction in your app, will definitely help you sleep better.

However, resetting the state is almost never enough. In the phone example, once the phone starts up again, you still need to get back into the state where you stopped. This usually means finding the video again and jumping back to the right point on the timeline. We are partially reconstructing the state where we stopped, but ignoring all the other state where things might have gone wrong, like our home internet connection being broken.

For humans, it's easy to approximately remember where we stopped watching a video, but for an application it's impossible to reconstruct the state without the developer explicitly keeping track of it. In Erlang, this would mean that we have one process as part of our supervision tree that, for example, every second writes down how far the video got. Once it's restarted as part of a parent supervisor, it will know how to restore the state from disk.

### Durable Execution

This brings me to a recent discovery I made, another approach to dealing with failure that completely blew my mind 🤯. It's commonly known under the name durable execution, and is so new that most developers never have heard of it. It's an elegant solution to the "keeping track of the execution" problem, and perfectly complements Erlang's approach.

The simplest way of thinking of it is as a database for compute progression. It stores the minimal amount of data to be able to reconstruct your application's sate at any time. It makes your local variables indestructible, and for many scenarios you will not even require a traditional database anymore.

Imagine if you could just start an arbitrary computation and the system guarantees that it will run until completion and all the operations will be performed **exactly once**. Even if your app needs to be restarted in the middle. Or even if you need to stop the computation, move it to a different machine and continue. Your running code suddenly becomes this tangible thing, that you can look at, move and play with.

All this intrigued me so much that I started working on my own engine for durable execution called **[flawless](https://flawless.dev/)**.

~ Bernard
