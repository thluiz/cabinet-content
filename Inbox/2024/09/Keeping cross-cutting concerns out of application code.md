---
created: 2024-09-05T14:52:22 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/
author: Mark Seemann
---

# Keeping cross-cutting concerns out of application code

> ## Excerpt
> Don't inject third-party dependencies. Use Decorators.

---
_Don't inject third-party dependencies. Use Decorators._

I recently came across [a Stack Overflow question](https://stackoverflow.com/q/78887199/126014) that reminded me of a topic I've been meaning to write about for a long time: [Cross-cutting concerns](https://en.wikipedia.org/wiki/Cross-cutting_concern).

When it comes to [the usual suspects](https://en.wikipedia.org/wiki/Casablanca_(film)), logging, fault tolerance, caching, the best solution is usually to apply the [Decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern).

I often see code that uses Dependency Injection (DI) to inject, say, a logging interface into application code. You can see an example of that in [Repeatable execution](https://blog.ploeh.dk/2020/03/23/repeatable-execution), as well as a suggestion for a better design. Not surprisingly, the better design involves logging Decorators.

The Stack Overflow question isn't about logging, but rather about fault tolerance; [Circuit Breaker](https://martinfowler.com/bliki/CircuitBreaker.html), retry policies, timeouts, etc.

### Injected concern [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#02d07297ea6341c6aef55c0fcb76678c)

The question does a good job of presenting a minimal, reproducible example. At the outset, the code looks like this:

```
<span>public</span>&nbsp;<span>class</span>&nbsp;<span>MyApi</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>ResiliencePipeline</span>&nbsp;pipeline;
&nbsp;&nbsp;&nbsp;&nbsp;<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>IOrganizationService</span>&nbsp;service;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>MyApi</span>(<span>ResiliencePipelineProvider</span>&lt;<span>string</span>&gt;&nbsp;<span>provider</span>,&nbsp;<span>IOrganizationService</span>&nbsp;<span>service</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.pipeline&nbsp;=&nbsp;<span>provider</span>.<span>GetPipeline</span>(<span>"retry-pipeline"</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.service&nbsp;=&nbsp;<span>service</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>List</span>&lt;<span>string</span>&gt;&nbsp;<span>GetSomething</span>(<span>QueryByAttribute</span>&nbsp;<span>query</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>result</span>&nbsp;=&nbsp;<span>this</span>.pipeline.<span>Execute</span>(()&nbsp;=&gt;&nbsp;service.<span>RetrieveMultiple</span>(<span>query</span>));
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>result</span>.Entities.<span>Cast</span>&lt;<span>string</span>&gt;().<span>ToList</span>();
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

The Stack Overflow question asks how to test this implementation, but I'd rather take the example as an opportunity to discuss design alternatives. Not surprisingly, it turns out that with a more decoupled design, testing becomes easier, too.

Before we proceed, a few words about this example code. I assume that this isn't [Andy Cooke](https://stackoverflow.com/users/3805597)'s actual production code. Rather, I interpret it as a reduced example that highlights the actual question. This is important because you might ask: _Why bother testing two lines of code?_

Indeed, as presented, the `GetSomething` method is [so simple that you may consider not testing it](https://blog.ploeh.dk/2018/11/12/what-to-test-and-not-to-test). Thus, I interpret the second line of code as a stand-in for more complicated production code. Hold on to that thought, because once I'm done, that's all that's going to be left, and you may then think that it's so simple that it really doesn't warrant all this hoo-ha.

### Coupling [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#32211a755e0a4b9bbd04a049ddbba0c8)

As shown, the `MyApi` class is coupled to [Polly](https://www.thepollyproject.org/), because `ResiliencePipeline` is defined by that library. To be clear, all I've heard is that Polly is a fine library. I've used it for a few projects myself, but I also admit that I haven't that much experience with it. I'd probably use it again the next time I need a Circuit Breaker or similar, so the following discussion isn't a denouncement of Polly. Rather, it applies to all third-party dependencies, or perhaps even dependencies that are part of your language's base library.

Coupling is a major cause of [spaghetti code](https://en.wikipedia.org/wiki/Spaghetti_code) and code rot in general. To write sustainable code, you should be cognizant of coupling. The most decoupled code is [code that you can easily delete](https://blog.ploeh.dk/2022/11/21/decouple-to-delete).

This doesn't mean that you shouldn't use high-quality third-party libraries like Polly. Among myriads of software engineering heuristics, we know that we should be aware of the [not-invented-here syndrome](https://en.wikipedia.org/wiki/Not_invented_here).

When it comes to classic cross-cutting concerns, the Decorator pattern is usually a better design than injecting the concern into application code. The above example clearly looks innocuous, but imagine injecting both a `ResiliencePipeline`, a logger, and perhaps a caching service, and your real application code eventually disappears in 'infrastructure code'.

It's not that we don't want to have these third-party dependencies, but rather that we want to move them somewhere else.

### Resilient Decorator [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#67a215289ba944b984b4d113b10e419c)

The concern in the above example is the desire to make the `IOrganizationService` dependency more resilient. The `MyApi` class only becomes more resilient as a transitive effect. The first refactoring step, then, is to introduce a resilient Decorator.

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>ResilientOrganizationService</span>(
&nbsp;&nbsp;&nbsp;&nbsp;<span>ResiliencePipeline</span>&nbsp;<span>pipeline</span>,
&nbsp;&nbsp;&nbsp;&nbsp;<span>IOrganizationService</span>&nbsp;<span>inner</span>)&nbsp;:&nbsp;<span>IOrganizationService</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>QueryResult</span>&nbsp;<span>RetrieveMultiple</span>(<span>QueryByAttribute</span>&nbsp;<span>query</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>pipeline</span>.<span>Execute</span>(()&nbsp;=&gt;&nbsp;<span>inner</span>.<span>RetrieveMultiple</span>(<span>query</span>));
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

As Decorators must, this class composes another `IOrganizationService` while also implementing that interface itself. It does so by being an [Adapter](https://en.wikipedia.org/wiki/Adapter_pattern) over the Polly API.

I've applied [Nikola Malovic's 4th law of DI](https://vuscode.wordpress.com/2009/10/16/inversion-of-control-single-responsibility-principle-and-nikola-s-laws-of-dependency-injection/):

> "Every constructor of a class being resolved should not have any implementation other then accepting a set of its own dependencies."

Instead of injecting a `ResiliencePipelineProvider<string>` only to call `GetPipeline` on it, it just receives a `ResiliencePipeline` and saves the object for use in the `RetrieveMultiple` method. It does that via a [primary constructor](https://learn.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/instance-constructors#primary-constructors), which is a recent C# language addition. It's just syntactic sugar for Constructor Injection, and as usual [F#](https://fsharp.org/) developers should feel right at home.

### Simplifying MyApi [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#8e967ac0c4ea4323b280e7a665825903)

Now that you have a resilient version of `IOrganizationService` you don't need to have any Polly code in `MyApi`. Remove it and simplify:

```
<span>public</span>&nbsp;<span>class</span>&nbsp;<span>MyApi</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>IOrganizationService</span>&nbsp;service;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>MyApi</span>(<span>IOrganizationService</span>&nbsp;<span>service</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.service&nbsp;=&nbsp;<span>service</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>List</span>&lt;<span>string</span>&gt;&nbsp;<span>GetSomething</span>(<span>QueryByAttribute</span>&nbsp;<span>query</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>result</span>&nbsp;=&nbsp;service.<span>RetrieveMultiple</span>(<span>query</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>result</span>.Entities.<span>Cast</span>&lt;<span>string</span>&gt;().<span>ToList</span>();
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

As promised, there's almost nothing left of it now, but I'll remind you that I consider the second line of `GetSomething` as a stand-in for something more complicated that you might need to test. As it is now, though, testing it is trivial:

```
[<span>Theory</span>]
[<span>InlineData</span>(<span>"foo"</span>,&nbsp;<span>"bar"</span>,&nbsp;<span>"baz"</span>)]
[<span>InlineData</span>(<span>"qux"</span>,&nbsp;<span>"quux"</span>,&nbsp;<span>"corge"</span>)]
[<span>InlineData</span>(<span>"grault"</span>,&nbsp;<span>"garply"</span>,&nbsp;<span>"waldo"</span>)]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>GetSomething</span>(<span>params</span>&nbsp;<span>string</span>[]&nbsp;<span>expected</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>service</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>Mock</span>&lt;<span>IOrganizationService</span>&gt;();
&nbsp;&nbsp;&nbsp;&nbsp;<span>service</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.<span>Setup</span>(<span>s</span>&nbsp;=&gt;&nbsp;<span>s</span>.<span>RetrieveMultiple</span>(<span>new</span>&nbsp;<span>QueryByAttribute</span>()))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.<span>Returns</span>(<span>new</span>&nbsp;<span>QueryResult</span>(<span>expected</span>));
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>MyApi</span>(<span>service</span>.Object);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>sut</span>.<span>GetSomething</span>(<span>new</span>&nbsp;<span>QueryByAttribute</span>());
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>(<span>expected</span>,&nbsp;<span>actual</span>);
}
```

The larger point, however, is that not only have you now managed to keep third-party dependencies out of your application code, you've also simplified it and made it easier to test.

### Composition [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#c9dd80b39e234c6595ef31de1fea30c2)

You can still create a resilient `MyApi` object in your [Composition Root](https://blog.ploeh.dk/2011/07/28/CompositionRoot):

```
<span>var</span>&nbsp;<span>service</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>ResilientOrganizationService</span>(<span>pipeline</span>,&nbsp;<span>inner</span>);
<span>var</span>&nbsp;<span>myApi</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>MyApi</span>(<span>service</span>);
```

Decomposing the problem in this way, you decouple your application code from third-party dependencies. You can define `ResilientOrganizationService` in the application's Composition Root, which also keeps the Polly dependency there. Even so, you can implement `MyApi` as part of your application layer.

![Three circles arranged in layers. In the outer layer, there's a box labelled 'ResilientOrganizationService' and another box labelled 'Polly'. An arrow points from 'ResilientOrganizationService' to 'Polly'. In the second layer in there's a box labelled 'MyApi'. The inner circle is empty.](https://blog.ploeh.dk/content/binary/polly-in-outer-shell.png)

I usually illustrate [Ports and Adapters](https://blog.ploeh.dk/2013/12/03/layers-onions-ports-adapters-its-all-the-same), or, if you will, [Clean Architecture](https://blog.ploeh.dk/ref/clean-architecture) as concentric circles, but in this diagram I've skewed the circles to make space for the boxes. In other words, the diagram is 'not to scale'. Ideally, the outermost layer is much smaller and thinner than any of the the other layers. I've also included an inner green layer which indicates the architecture's Domain Model, but since I assume that `MyApi` is part of some application layer, I've left the Domain Model empty.

### Reasons to decouple [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#ba7304112c214dd6be84011aea811dbf)

Why is it important to decouple application code from Polly? First, keep in mind that in this discussion Polly is just a stand-in for any third-party dependency. It's up to you as a software architect to decide how you'll structure your code, but third-party dependencies are one of the first things I look for. A third-party component changes with time, and often independently of your base platform. You may have to deal with breaking changes or security patches at inopportune times. The organization that maintains the component may cease to operate. This happens to commercial entities and open-source contributors alike, although for different reasons.

Second, even a top-tier library like Polly will undergo changes. If your time horizon is five to ten years, you'll be surprised how much things change. You may protest that no-one designs software systems with such a long view, but I think that if you ask the business people involved with your software, they most certainly expect your system to last a long time.

I believe that I heard on a podcast that some Microsoft teams had taken a dependency on Polly. Assuming, for the sake of argument, that this is true, while we may not wish to depend on some random open-source component, depending on Polly is safe, right? In the long run, it isn't. Five years ago, you had the same situation with [Json.NET](https://www.newtonsoft.com/json), but then Microsoft hired James Newton-King and had him make a JSON API as part of the .NET base library. While Json.NET isn't dead by any means, now you have two competing JSON libraries, and Microsoft uses their own in the frameworks and libraries that they release.

Deciding to decouple your application code from a third-party component is ultimately a question of risk management. It's up to you to make the bet. Do you pay the up-front cost of decoupling, or do you postpone it, hoping it'll never be necessary?

I usually do the former, because the cost is low, and there are other benefits as well. As I've already touched on, unit testing becomes easier.

### Configuration [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#f126ff285a014a6d85cff276436321c8)

Since Polly only lives in the Composition Root, you'll also need to define the `ResiliencePipeline` there. You can write the code that creates that pieline wherever you like, but it might be natural to make it a creation function on the `ResilientOrganizationService` class:

```
<span>public</span>&nbsp;<span>static</span>&nbsp;<span>ResiliencePipeline</span>&nbsp;<span>CreatePipeline</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>ResiliencePipelineBuilder</span>()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.<span>AddRetry</span>(<span>new</span>&nbsp;<span>RetryStrategyOptions</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MaxRetryAttempts&nbsp;=&nbsp;4
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;})
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.<span>AddTimeout</span>(<span>TimeSpan</span>.<span>FromSeconds</span>(1))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.<span>Build</span>();
}
```

That's just an example, and perhaps not what you'd like to do. Perhaps you rather want some of these values to be defined in a configuration file. Thus, this isn't what you _have_ to do, but rather what you _could_ do.

If you use this option, however, you could take the return value of this method and inject it into the `ResilientOrganizationService` constructor.

### Conclusion [#](https://blog.ploeh.dk/2024/09/02/keeping-cross-cutting-concerns-out-of-application-code/#11bbc9df98474c33a6ce0902b13178d4)

Cross-cutting concerns, like caching, logging, security, or, in this case, fault tolerance, are usually best addressed with the Decorator pattern. In this article, you saw an example of using the Decorator pattern to decouple the concern of fault tolerance from the consumer of the service that you need to handle in a fault-tolerant manner.

The specific example dealt with the Polly library, but the point isn't that Polly is a particularly nasty third-party component that you need to protect yourself against. Rather, it just so happened that I came across a Stack Overflow question that used Polly, and I though it was a a nice example.

As far as I can tell, Polly is actually one of the top .NET open-source packages, so this article is not a denouncement of Polly. It's just a sketch of how to move useful dependencies around in your code base to make sure that they impact your application code as little as possible.
