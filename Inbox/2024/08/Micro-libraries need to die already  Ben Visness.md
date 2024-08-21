---
created: 2024-08-21T14:41:35 (UTC -03:00)
tags: []
source: https://bvisness.me/microlibraries/?utm_source=tldrnewsletter
author: Ben Visness
---

# Micro-libraries need to die already | Ben Visness

> ## Excerpt
> Somehow people are still putting tiny libraries on npm, and it really needs to stop.

---
August 18, 2024

It is the year 2024. It has been eight years since `left-pad` first made people realize that, hey, maybe it’s not a great idea to outsource trivial functionality to random people on the internet.

But in the year 2024, I still see some people arguing that actually, micro-libraries are good, and we should do more of them. We should make packages smaller and use even more dependencies. The problem with npm is really that we just haven’t made the packages small enough yet.

This is so unbelievably wrong that it should not even be up for debate. But because there is a debate regardless, someone needs to exhaustively and painfully explain why bad practices are bad.

Here is my thesis: **Micro-libraries should never be used. They should either be copy-pasted into your codebase, or not used at all.**

However, my actual goal with this article is to break down the way I think about the costs and benefits of dependencies. I won’t be _quantifying_ these costs and benefits, but I hope that by explaining the way I think about dependencies, it will be clear why I think micro-libraries are all downside and no upside.

## Costs and benefits

Everyone knows that programming is all about [tradeoffs](https://bvisness.me/tradeoffs/). You gotta use the right tool for the job, you know?

Well, you can’t make a reasonable tradeoff unless you can actually articulate the costs and benefits. So let’s examine the pros and cons of libraries in general, starting with the benefits:

-   **It saves development time.** This is the most obvious benefit of a library, especially if the problem it solves is complicated.
-   **The code is (hopefully) more robust.** Library authors have presumably thought a lot about the problem, and if their implementation is mature, it might handle more edge cases and subtle pitfalls. Their implementation may also be more “future-proof”, anticipating future use cases. This property is strongest when the library has lots of users—it may not be _good_, but it is less likely to be _wrong_.
-   **You can upgrade to get features, bug fixes, or security updates.** This extends the first point: not only do other people write the code for you, but other people _maintain_ it for you. In the best case, an upgrade can simply make your life better without breaking compatibility.

That’s about it for benefits. Unfortunately, there are also many costs associated with dependencies—more than most people account for:

-   **The library may be a bad fit for your problem.** This often cancels out the primary benefit of libraries. No, you don’t have to write the code, but you _do_ have to adapt your problem to fit the library, and adapt the library’s results to fit your app again. This cost can be very extreme!
    
    For example, at my last job, we tried using Amazon’s Simple Notification Service to send push notifications to both iOS and Android devices. In theory, we could have saved time by targeting just one API instead of two. But after juggling AWS auth and device registration and SQS queues and SNS topics and API incompatibilities, it turned out to be much easier to just target the Apple and Google push APIs directly.
    
-   **The library may be poorly written.** Programmers typically assume that library code is higher-quality than their code. This is often just not true. Any random person can publish an npm package, and many npm packages are just bad. This is even true of very popular packages; the correlation between popularity and quality is extremely weak.
    
    Using libraries also often results in performance penalties, even when “well-written”. Libraries are for everyone, therefore they are optimized for no one.
    
-   **Third-party code is inherently risky.** The library may have critical bugs, or the author may be overtly malicious. It is very hard to properly audit everything, and the more complex the library, the more opportunities there are for mistakes or attacks. On the other hand, if you write the code yourself, you know there’s nothing malicious about it, and you have an opportunity to vet it for bugs.
-   **Every dependency is a supply chain attack vector.** Any package, from the biggest framework to the tiniest utility, could be compromised, and would have equal access to sensitive resources. The more packages you have, and the more maintainers there are, the more opportunities there are to get pwned.
-   **The library may have a large footprint.** Libraries are often just way bigger than you need. This bloat can come from multiple sources: features you never use, metadata in `node_modules`, duplicate versions of the same package, and of course piles and piles of transitive dependencies. Furthermore, this footprint is highly variable—a routine update can invisibly quadruple the footprint of a package.
    
    This footprint negatively affects all stages of the process: increased install times, increased build times, larger bundle sizes for users. This problem is so prevalent in the JavaScript ecosystem that many common packages have a total footprint in the hundreds of megabytes, a truly shocking amount of waste. The issue has gotten so bad that there is now an initiative called [e18e](https://e18e.dev/) that is attempting to clean things up.
    
-   **Updates are not free.** In theory, updating a package should be fine as long as the version is compatible. But in practice, updating causes all kinds of problems: breaking changes, deprecated functions (and associated rewrites), performance regressions, bundle size bloat, new bugs. This cost is unpredictable; you never really know what might have changed upstream.
-   **Libraries may have lots of transitive dependencies.** Transitive dependencies are also dependencies. Everything in your `package_lock.json` is a real dependency with real costs that you cannot ignore. Transitive dependencies increase the risk of bad code, increase the risk of security issues, and of course increase the footprint of your application.

This is how I look at dependencies—and clearly, in my view, there are many more costs than benefits. The benefits can be very strong, but you cannot make a wise decision without considering the costs as well. Beginners in particular tend to ignore the costs—but their libraries will betray them eventually.

## `is-number`: a case study

Let’s examine a popular micro-library, [`is-number`](https://www.npmjs.com/package/is-number). This is an npm package with just a single function, `isNumber`. It takes a value and tells you if it’s a finite number, or a finite non-empty numeric string. This spectacularly useful function exemplifies all the problems with micro-libraries in the JS ecosystem.

Let’s examine the benefits of this package:

-   You can write `isNumber(foo)` instead of `typeof foo === "number"`.

That’s quite a list.

But seriously, test it against our possible benefits from earlier:

-   **Does it save development time? Barely.** Assuming you do, for some reason, need to check if a value is a finite number or a finite non-empty numeric string, this library may save you a few minutes. But in practice this function is almost entirely useless, as we’ll see below.
-   **Is it more robust than what you could write? No.** The code is extremely straightforward and easy to verify.
-   **Would future updates be useful? No.** The library is so simple that any change to the logic would be breaking, and it is already clear that there are no bugs.

And now we get to the costs:

-   **Is it a good fit for your problem? Almost certainly not.** 99% of the time, all you need is `typeof foo === "number"`. 0.9% of the time, all you need is `foo == Number(foo)` (which will include numeric strings and exclude `NaN`). At most 0.1% of the time do you _also_ need to exclude empty strings and `Infinity`. These are trivial to write and should be familiar to any JavaScript programmer. Therefore, `is-number` is almost always just bloat, performing unnecessary checks out of paranoia and likely breaking some optimizations that the JS engine could otherwise make.
-   **Are updates breaking? Yes.** Incredibly, `is-number` is already on major version 7.0.0. This is an amazing number of breaking changes for such a simple function.
    
    Why all these new versions? Many reasons, none of them good. Sometimes the author arbitrarily changes his mind about what’s a “number”—for example, `NaN` used to be considered a number; now it’s not. One breaking change simply upgraded the minimum supported Node version from 0.10.0 to 0.12.0 and changed _nothing else_. And sometimes he just calls it a breaking change because he feels like it.
    
-   **Is it bloated? Kinda.** Although the actual code is a mere 245 bytes, the size when installed is 9.62 kB. That is, the footprint on your computer is 39x larger than necessary, due to metadata like the README, LICENSE, and package.json. Thankfully this shouldn’t affect build times or bundle sizes, but this amazing level of waste adds up across the thousands of packages installed in people’s `node_modules`. Furthermore, because the author releases so many major versions, it’s common to find multiple copies of the library in your `node_modules` for no good reason.
-   **Is it risky? Yes.** It’s a supply chain attack opportunity like any other, and because it updates fairly frequently, updates are less likely to trigger scrutiny.

So, by my count, we have **zero upsides** and **several downsides**. So much for that tradeoff.

## Copy-paste: a case study

Suppose that, for whatever incomprehensible reason, we actually did need to check if a JS value was either a finite number or a finite non-empty numeric string. Instead of installing an npm package, we could just copy-paste the entirety of `is-number` into our program:

```
<span><span><span>function</span> <span>isNumber</span><span>(</span><span>num</span><span>)</span> <span>{</span>
</span></span><span><span>  <span>if</span> <span>(</span><span>typeof</span> <span>num</span> <span>===</span> <span>'number'</span><span>)</span> <span>{</span>
</span></span><span><span>    <span>return</span> <span>num</span> <span>-</span> <span>num</span> <span>===</span> <span>0</span><span>;</span>
</span></span><span><span>  <span>}</span>
</span></span><span><span>
</span></span><span><span>  <span>if</span> <span>(</span><span>typeof</span> <span>num</span> <span>===</span> <span>'string'</span> <span>&amp;&amp;</span> <span>num</span><span>.</span><span>trim</span><span>()</span> <span>!==</span> <span>''</span><span>)</span> <span>{</span>
</span></span><span><span>    <span>return</span> <span>Number</span><span>.</span><span>isFinite</span> <span>?</span> <span>Number</span><span>.</span><span>isFinite</span><span>(</span><span>+</span><span>num</span><span>)</span> <span>:</span> <span>isFinite</span><span>(</span><span>+</span><span>num</span><span>);</span>
</span></span><span><span>  <span>}</span>
</span></span><span><span>
</span></span><span><span>  <span>return</span> <span>false</span><span>;</span>
</span></span><span><span><span>}</span></span></span>
```

This mitigates all the remaining downsides. We still **save development time** by copy-pasting. It **cannot cause breakage** in the future, since it will never change. It has a **smaller footprint**, since it doesn’t include unnecessary metadata, and it will never mysteriously duplicate itself. It is **not risky**, since its functionality is obvious and it cannot be attacked via the supply chain.

And of course, we probably could have just written `typeof foo === "number"`.

## What about duplication?

One of the purported benefits of tiny libraries is reduced duplication across your whole app. Say your app has a utility like `isNumber`, and several libraries have utilities like `isNumber`—wouldn’t it be better to reduce all the duplication and let them all share one version of the utility?

In practice this is obviously not what happens. Look at the dependency graphs for popular projects and you will see amazing amounts of duplication. Often there are multiple packages performing similar functions, but also there often multiple major versions of the _same package_.

It should be obvious why this happens: not all users have the exact same requirements. Wherever the requirements differ, there will be a different implementation. We wouldn’t expect every copy-pasted utility to be exactly the same, so why would we expect the duplication to disappear when using a package manager instead?

But really, it’s worse than that, because when multiple major versions of `is-number` are installed in your `node_modules`, something else is clearly wrong. Semantic versioning simply isn’t the sharp tool people think it is. If you’re on Node 20, it is _not a breaking change_ for the library to raise the minimum Node version from 0.10.0 to 0.12.0. There would be no need for a duplicate version, but your tools do not know this. Likewise if the package author releases a major version because of an edge case. Technically breaking for _some_ users, but not for you.

And finally—many use cases of tiny libraries can literally be replaced with one-liners. In this case you don’t have to worry about duplication at all anyway. Your code will still be small. It will be a tiny amount of source code and just a handful of bytecode ops. You don’t need a package manager to solve this problem for you.

## Stop already!

I could have written the same analysis about `left-pad` eight years ago, or [many](https://www.npmjs.com/package/shebang-regex) [other](https://www.npmjs.com/package/is-regexp) [packages](https://www.npmjs.com/package/run-applescript) today. **Tiny utilities simply should not be libraries.**

There is absolutely nothing wrong with copy-pasting code into your project. Sometimes it really is useful to grab a snippet of code from Stack Overflow, but there is literally zero benefit to installing these things through a package manager. You are exposing yourself to a whole world of pain, which you can trivially avoid by just copy-pasting.

I have talked a lot about the costs of libraries, and I do hope people are more cautious about them. But there’s one factor I left out from my previous discussion. I think there’s one more reason why people use libraries: **fear**.

Programmers are afraid of causing bugs. Afraid of making mistakes. Afraid of missing edge cases. Afraid that they won’t be able to understand how things work. In their fear they fall back on libraries. “Thank goodness someone else has solved the problem; surely I _never_ would have been able to.”

But they should not be afraid! Libraries are not magic. They are just code someone else wrote. After all, I pasted the entirety of `is-number` above, and nothing in there is too mysterious. And beyond libraries—languages are not magic, operating systems are not magic, nothing is magic. Dig into the source code and you will find code you can read and understand. This attitude is fundamental to the [Handmade ethos](https://handmade.network/manifesto) and I endorse it fully.

If you are a proponent of tiny libraries, I encourage you to overcome your fear and try writing the code yourself. You are more capable than you think.
