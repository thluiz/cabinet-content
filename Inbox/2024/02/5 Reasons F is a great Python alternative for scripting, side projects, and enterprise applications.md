---
created: 2024-03-25T15:22:41 (UTC -03:00)
tags: []
source: https://hamy.xyz/labs/2024-03_fsharp-python
author: 
---

# 5 Reasons F# is a great Python alternative for scripting, side projects, and enterprise applications

> ## Excerpt
> F# and Python are similar in many ways. They're both minimal languages that excel at getting things done quickly with minimal boilerplate / mental overhead.

---
F# and Python are similar in many ways. They're both minimal languages that excel at getting things done quickly with minimal boilerplate / mental overhead.

However Python often struggles at scale due to its lack of native type safety and caps on possible performance. F#'s unique design solves for these gaps - allowing you to build programs in the same lightweight paradigm you're used to while also scaling well with advanced, native type safety, and very strong performance profiles (rivaling Go, C#, and Java).

In this post we'll explore 5 reasons F# is a great Python alternative and why you might want to try it for your next ad hoc script, side project, or large enterprise app.

<iframe width="560" height="315" src="https://www.youtube.com/embed/b12MFKsOVC8?si=fDOyIVRwWlB_aa9_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen=""></iframe>

## Low Boilerplate

People often choose Python because its syntax is very minimal and clean. It's very easy to pick up a Python program, read it, and understand what it's doing.

A lot of this is due to its minimal syntax but also its bias towards low boilerplate. Python programs are usually very to-the-point and don't have the dozens of layers of abstraction that's common in more "enterprisey" apps.

Example Python function (via [Python2Fsharp](https://github.com/knocte/2fsharp/blob/master/python2fsharp.md)):

```
def give_me_the_length(input):
    result = len(input)
    return result
```

F# is similarly low boilerplate - it focuses on simple data structures, pure functions, and list operations often leading to very simple programs with minimal layers of abstraction.

Example F# function (via [Python2Fsharp](https://github.com/knocte/2fsharp/blob/master/python2fsharp.md)):

```
let GiveMeTheLength(input) =
    let result = input.Length
    result
```

## Large Ecosystem

Another reason people choose Python is its very large ecosystem - particularly for building web apps and data / ml operations. This ecosystem has been built up over decades of Python being a top 10 language in the industry.

[F# ranks 37th in the world in terms of usage](https://hamy.xyz/labs/2023-06-state-of-fsharp) and thus has a comparatively small ecosystem. But F# runs on dotnet and thus can leverage libraries targeting dotnet - which basically means it can use anything written in C# which itself is a top 10 language.

F# may not be able to beat out Python for data operations but it has a huge advantage in the world of scalable, enterprise web apps due to decades of investment from Microsoft and the C# community.

## Fast

F# runs on dotnet and dotnet is very fast. In comparison, Python is pretty slow.

[For most apps performance is rarely the bottleneck](https://hamy.xyz/labs/2023-10-your-programming-language-benchmark-is-wrong) but if performance does become the bottleneck it can be hard to fix this without a major rewrite of your app.

Choosing a faster technology at the outset can help ensure you don't run into these costly problems later on.

For some data:

-   [F# is just as fast as C#](https://hamy.xyz/labs/2023-05-csharp-vs-fsharp-performance) (rivaling Java, Golang, and Kotlin)
-   Python is typically 2-5x slower depending on the workload (closer to Ruby, Javascript, and Elixir)

_Caveat: All benchmarks have asterisks. Performance is highly dependent on workload, implementation, and goals so take w grain of salt. I'm pulling results for ~most popular framework that also look reasonable to try and avoid outliers._

[Web Frameworks Benchmarks](https://web-frameworks-benchmark.netlify.app/result?l=python,fsharp,javascript,ruby,go,kotlin,java,elixir) (higher is better)

-   **Java / Vertx** - 174k
-   **C# / ASP.NET minimal api** - 129k
-   **F# / Falco** - 126k
-   **Go / Chi** - 110k
-   **Kotlin / Http4k** - 78k
-   **JavaScript / NestJs on Fastify** - 67k
-   **Elixir / Phoenix Bandit** - 52k
-   **Python / Fastapi** - 16k
-   **Ruby / Sinatra** - 11k

[Tech Empower Benchmark - Fortunes](https://www.techempower.com/benchmarks/#section=data-r21&hw=ph&test=fortune&l=x84vwd-cn2) (higher is better)

-   **Java / Jooby** - 296k
-   [F# / Giraffe](https://hamy.xyz/labs/2022-12-simple-fsharp-web-api-giraffe) - 220k
-   **Kotlin / Http4k** - 217k
-   **C# / ASP.NET Core** - 212k
-   **Golang / Gin** - 116k
-   **Javascript / Fastify** - 71k
-   **Elixir / Phoenix** - 59k
-   **Python / Fastapi** - 50k
-   **Ruby / Rails** - 10k

## Type-safe

One of the major problems with Python is it's not very type-safe. Yes recent versions have started allowing for type checking but the truth is much of it is bolted on which means finnicky lint setups that often miss things and inability to have strong, advanced type checks at build time. This is a problem because [type-safety is one of the best ways to prevent common bugs and speed up development in large organizations](https://hamy.xyz/labs/2024-03_typesafe-vs-dynamic-programming-languages).

F# on the other hand has an excellent, native type system. This type system allows for Discriminated Unions, exhaustive pattern matching, and it can infer most types. This means it's easy to write precise types and there's very little tedious boilerplate involved - if the compiler accepts it, it's unlikely you'll have any runtime type issues.

This excellent, easy-to-use type system makes F# a great choice for defining domain models and thus is excellent for [Domain Driven Design for any scale](https://hamy.xyz/labs/2024-01_best-book-ddd).

I could write books on how important type-safety is but if you're interested in more, I'd recommend the [Why use F#?](https://fsharpforfunandprofit.com/why-use-fsharp/) blog post series.

## Fun to Use

Ultimately most people choose Python because it gets out of their way and lets them do what they want quickly and easily. The point of technology is to aid us in our endeavors and ultimately Python does that pretty well - at least at small scale. This idea of supporting you while getting out of the way often makes tech fun to use - you're not artificially held back by bad design decisions.

[Where F# shines is that it's fun both at small scale and at large scale](https://hamy.xyz/labs/2024-02_fsharp-is-fun). It's fun at small scale for the same reasons Python is - it's easy to do things and it gets out of your way. It's fun at large scale cause it's still easy to do things - the type system prevents you from shooting yourself in the foot without the complexity and bloat most OO type systems tend to incur.

## Next

[F# has been my favorite programming language since I found it several years ago](https://hamy.xyz/labs/2024-01_how-i-got-into-fsharp). It takes the best parts from many different languages and tosses the parts that suck. For these reasons I think F# makes a great replacement for Python for many different applications.

If you liked this post you might also like:

-   [The best way to get started learning and building with F#](https://hamy.xyz/labs/2024-01_best-way-to-learn-fsharp)
-   [Why F# is a fun programming language](https://hamy.xyz/labs/2024-02_fsharp-is-fun)
-   [Build a simple F# web API with Giraffe](https://hamy.xyz/labs/2022-12-simple-fsharp-web-api-giraffe)
