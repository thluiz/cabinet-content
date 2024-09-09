---
created: 2024-09-05T14:50:02 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2024/08/19/zippers/
author: Mark Seemann
---

# Zippers

> ## Excerpt
> Some functional programming examples ported to C#, just because.

---
_Some functional programming examples ported to C#, just because._

Many algorithms rely on data structures that enable the implementation to move in more than one way. A simple example is a [doubly-linked list](https://en.wikipedia.org/wiki/Doubly_linked_list), where an algorithm can move both forward and backward from a given element. Other examples are various tree-based algorithms, such as [red-black trees](https://en.wikipedia.org/wiki/Red%E2%80%93black_tree) where certain operations trigger reorganization of the tree. Yet other data structures, such as [Fibonacci heaps](https://en.wikipedia.org/wiki/Fibonacci_heap), combine doubly-linked lists with trees that allow navigation in more than one direction.

In an imperative programming language, you can easily implement such data structures, as long as the language allows data mutation. Here's a simple example:

```
<span>var</span>&nbsp;<span>node1</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>Node</span>&lt;<span>string</span>&gt;(<span>"foo"</span>);
<span>var</span>&nbsp;<span>node2</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>Node</span>&lt;<span>string</span>&gt;(<span>"bar"</span>)&nbsp;{&nbsp;Previous&nbsp;=&nbsp;<span>node1</span>&nbsp;};
<span>node1</span>.Next&nbsp;=&nbsp;<span>node2</span>;
```

It's possible to double-link `node1` to `node2` by first creating `node1`. At that point, `node2` still doesn't exist, so you can't yet assign `node1.Next`, but once you've initialized `node2`, you can mutate the state of `node1` by changing its `Next` property.

When data structures are immutable (as they must be in functional programming) this is no longer possible. How may you get around that limitation?

### Alternatives [#](https://blog.ploeh.dk/2024/08/19/zippers/#3b3c3d4cba1f4ae8bef462b28047860a)

Some languages get around this problem in various ways. [Haskell](https://www.haskell.org/), because of its lazy evaluation, enables a technique called [tying the knot](https://wiki.haskell.org/Tying_the_Knot) that, frankly, makes my head hurt.

Even though I write a decent amount of Haskell code, that's not something that I make use of. Usually, it turns out, you can solve most problems by thinking about them differently. By choosing another perspective, and another data structure, you can often arrive at a good, functional solution to your problem.

One family of general-purpose data structures are called Zippers. The general idea is that the data structure has a natural 'focus' (e.g. the head of a list), but it also keeps a record of 'breadcrumbs', that is, where the caller has previously been. This enables client code to 'go back' or 'go up', if the natural direction is to 'go forward' or 'go down'. It's a bit like [Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html), in that every operation leaves a log entry that can later be used to reconstruct what happened. [Repeatable Execution](https://blog.ploeh.dk/2020/03/23/repeatable-execution) also comes to mind, although it's not quite the same.

For an introduction to Zippers, I recommend the excellent and highly readable article [Zippers](https://learnyouahaskell.com/zippers). In this article series, I'm going to assume that you're familiar with the contents of that article.

### C# ports [#](https://blog.ploeh.dk/2024/08/19/zippers/#8ec371f87d2f468ea6ebbc3a2e420cbb)

While I may add more articles to this series in the future, as I'm writing this, I have nothing more planned than writing about how it's possible to implement the article's three Zippers in C#.

-   [A List Zipper in C#](https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c)
-   A Binary Tree Zipper in C#
-   FSZipper in C#

Why would you want to do this?

To be honest, for production code, I can't think of a good reason. I did it for a few reasons, most of them didactic. Additionally, [writing code for exercise](https://blog.ploeh.dk/2020/01/13/on-doing-katas) helps you improve. If you know enough Haskell to understand what's going on in the [Zippers article](https://learnyouahaskell.com/zippers), you may consider porting some of it to your favourite language, as an exercise.

It may help you [grokking](https://blog.ploeh.dk/ref/stranger-in-a-strange-land) functional programming.

That's really it, though. There's no reason to use Zippers in a language like C#, which [idiomatically](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic) makes use of mutation. If you want a doubly-linked list, you can just write code as shown in the beginning of this article.

If you're interested in an [F#](https://fsharp.org/) perspective on Zippers, [Tomas Petricek](https://tomasp.net/) has a cool article: [Processing trees with F# zipper computation](https://tomasp.net/blog/tree-zipper-query.aspx/).

### Conclusion [#](https://blog.ploeh.dk/2024/08/19/zippers/#8a124e3b10aa4b0b889efe866f63dc91)

Zippers constitute a family of data structures that enables you to move in multiple directions. Left and right in a list. Up or down in a tree. For an imperative programmer, that's literally just another day at the office, but in disciplined functional programming, making cyclic graphs can be surprisingly tricky.

Even in functional programming, I rarely reach for a Zipper, since I can often find a library with a higher level of abstraction that does what I need it to do. Still, learning of new ways to solve problems never seems a waste to me.

In the next three articles, I'll go through the examples from [the Zipper article](https://learnyouahaskell.com/zippers) and show how I ported them to C#. While that article starts with a [binary tree](https://en.wikipedia.org/wiki/Binary_tree), I'll instead begin with the doubly-linked list, since it's the simplest of the three.

**Next:** [A List Zipper in C#](https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c).
