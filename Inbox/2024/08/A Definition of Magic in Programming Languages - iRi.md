---
created: 2024-08-21T15:13:32 (UTC -03:00)
tags: []
source: https://jerf.org/iri/post/2024/magic/?ref=dailydev#generic
author: 
---

# A Definition of Magic in Programming Languages - iRi

> ## Excerpt
> The term “magic” is commonly thrown about in the programming world without
a definition. This post gives a definition for it.

---
The term “magic” is commonly thrown about in the programming world without a definition. This post gives a definition for it.

Not _the_ definition, just _a_ definition. As a long-standing fuzzy term, I can’t necessarily capture all uses of it in the wild, but I believe this captures a lot of the practical value.

A piece of code is **magic** in proportion to:

1.  How many other places in the code must be consulted for a human to understand what it does.
2.  How difficult it is to know what code to consult based on an examination of the code in question.

You can think of it as being proportional to the amount of time that someone skilled in the language, but unfamiliar with the code base, would need to be _sure_ they know what a given piece of code is doing, from the full source code alone. That is, no debugger or similar tools.

## A Continuum, Not A Binary

This definition creates a _continuum_ of magic. It is reasonable to talk about whether one bit of code is “more” magic than another, but for the most part, everything has some degree of magic in it.

Why is that? Because it isn’t even hardly _possible_ to state everything relevant to a piece of code inline with the code. Even an assembler instruction may require knowing what CPU flags are currently set to know what _exactly_ it is going to do. There isn’t much code in the world that truly stands alone.

Indeed, to some extent this is the exact point. If we wanted to think about every single processor instruction and all possible CPU flags, we’d use raw assembler for everything, and some hypothetical assembler that requires a full specification of all flags for all instructions or something. The point of a programming language environment is to provide at least _some_ magic. If nothing else, compiler optimizations generally mean even the authors of a compiler must ask the compiler what it turns certain code into.

## Generic

This is actually a generic definition, in the programming language sense of the term generic; it is instantiated on the exact level of understanding that a human is seeking at the moment.

If a human is seeking to know what the code is doing at a very low level, the “magic” will all be about how hard it is to figure out what numbers in RAM are getting twiddled in which way.

All the way on the other end of the abstraction layers, if a human is trying to figure out what the code is doing at an architectural level, the relevant definition of magic may be quite different.

It is entirely possible to take a language like C that is not very amenable to low-level magic and build a program that is very magical at the architecture layer, with indirection and tables everywhere.

It is entirely possible to take a language like Ruby that enables almost every kind of syntax-level magic imaginable, and use it to build a DSL that turns the architectural level into a highly straight-forward matter of simply reading the code.

It may be harder to see what I mean by that, but imagine some code that is rigidly specifying how data flows through a system by specifying the exact message bus and channels that two microservices can use to communicate with each other. While lower levels implementing this specification may have lots of magic or not much at all, if this architectural specification is accurate and the reader can rely on it, the architectural level has a low level of magic with regard to the architecture itself. Magic would be things like the name of the channel having hidden effects, or if the user can’t rely on the specification because other channels are specified elsewhere.

While I believe the concept can be applied at all levels, it is certainly most common to apply it to lower levels.

## Magic Is Not “Good” Or “Bad”

Magic as defined here is a _cost_. The way I’ve written this definition, high degrees of magic certainly impose a burden on a programmer in understanding the code at a deep level. This is a cost, certainly.

But costs in isolation are not intrinsically good or bad.

The question is, do you get benefits commensurate to the costs?

If you are reacting emotionally to what I’ve said so far, it is likely that you believe that I either am, or are about to, sneak in some sort of idea that more magic is “worse” than less magic or something, but I promise you I intend no such thing. I am just defining a measure here, and while many people will have varying opinions about how magic correlates to “goodness”, I’m not pushing one here. It is possible to have magic that pays off far out of proportion to its costs and it is possible to have magic that costs through the nose and provides almost no value. That’s a different attribute of the code.

## Examples

Consider the psuedo-code

> x = someFunc(a, b.c, d.e(f))

in the context of many different languages.

A generally low-magic language like C has only a limited number of ways that can be read. `b.c` is dereferencing into a structure. someFunc is a type of thing resolvable by the local manifest types, most likely a function.

There are many ways to increase the amount of magic in this expression. This is not an exhaustive list by any means, just some examples to get you thinking about the magic:

## Implicit Methods

A language such as Python permits the overloading of the dot (`.`) operator through a number of mechanisms, such as `__getattr__` definitions, `__getattribute__` definitions, properties, the [descriptor protocol](https://docs.python.org/3/howto/descriptor.html), and this isn’t even an exhaustive list.

All of these permit `b.c` to be not just a simple field load, but an arbitrary amount of code. Nothing in Python stops `b.c` from becoming a full web request of some sort. This might be arguably bad design, but if you want to be _sure_ you know what is going on, you need to check for these things.

It is intrinsically more magical than the C equivalent.

## Inheritance

Object orientation generally incorporates a concept of subclasses and inheritance. For a given class, there may be a list of parents (languages that forbid multiple inheritance), a tree of parents (languages that do not forbid multiple inheritance), or in particularly exciting cases, what is technically a nearly-arbitrary graph of superclass relationships (Python technically fits here).

I’ve certainly had the experience of seeing a method call on a some value, even a value of a known concrete type, and spending a good bit of time just trying to find where the implementation is in the complex tree of parents. GUIs like QT can send you chasing up half-a-dozen superclasses to find some high-level method on a `QWindow` or something. The more complicated the inheritance a language permits, the more magic may be involved in finding a method.

## Polymorphism

Polymorphism means you have some sort of mechanism for having a value in hand that you do not have a concrete type for, be it through a class type that permits subclass value types, or an interface/trait/whatever your local name for this concept is. When examining a function which has such a polymorphic value, it becomes even more difficult to know what all possible resolutions of the method may be.

A concrete manifestation of this that frequently bites me is that if you “Jump to Definition” in your favorite IDE while pointed at one of these variables, all it can really do is send you to the polymorphic definition, even if you personally know… or _think_ you know… what the concrete type is.

Even if you are working in a language where you know method names and implementations can’t change, and you can easily locate all methods named some particular name, determining which of these types may actually appear as values in this function may be quite difficult.

In dynamically-typed languages, technically all values become polymorphic like this. In practice this means that reading code in these languages involves a lot of convention and “best practices” and patterns and the fervent hope that the original author of the code is doing a good job of conveying the situation accurately, because behind any of those incoming function parameters could _technically_ be anything. In theory, this is catastrophic nightmare. In practice, it works better than that, but if you program in such a language, it’s a good idea to stay within the guard rails of best practice as much as possible.

## Monkeypatching

The most dynamic languages permit arbitrary fiddling with the interpreter’s symbol table at runtime. In the worst case scenario, this could mean that looking at a bit of code and determining what it does from the source code is simply impossible, even with a complete set of all source code used. In principle, a monkeypatching language can examine the environment at run time and construct entirely new methods from scratch, such as loading in a database schema from a live running database. This means that seeing `user.userID` may technically not even _have_ a `userID` method _anywhere_ in the code… it may be completely constructed at run time.

This doesn’t mean every use is automatically this complicated, but it does mean that in a language where this is possible, a programmer reading any source code must always at least _wonder_ if there’s some monkeypatch necessary to know about to determine what a given bit of code is doing.

## Arbitrary Compositions

If there’s anything programmers do, it’s compose things together. Nothing stops you from monkeypatching in a method onto a superclass of a value being passed in polymorphically that changes one of the value’s implicit methods to rewrite what a field lookup does.

Trying to unravel the result of this simply by staring at the source code can be effectively impossible.

## Conclusion

In conclusion, I’d like to remind you that this is just a definition, and value neutral. Magic is not good or bad on its own terms. There is no criticism of any particular magic intended here. Even observations about the difficulty a particular bit of magic can create is still only an observation about the cost side of the equation; to determine the _value_ of such magic requires a much deeper analysis of the benefits of the approach and the various cost/benefit analyses of the alternatives. This is just to give a definition that is both concrete enough to get a cognitive handle on for discussion purposes, while covering most (but not all) of the uses of the term I see in the wild.

The term has suffered for lacking a concrete handle on it that admits rational discussion of the topic, so conversations revolving around magic generally become emotion-driven flamewars rather than sensible engineering discussions of the wide variety of costs and benefits that magic has.
