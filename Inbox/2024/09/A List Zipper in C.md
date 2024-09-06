---
created: 2024-09-05T14:50:07 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c/
author: Mark Seemann
---

# A List Zipper in C#

> ## Excerpt
> A port of a Haskell example, just because.

---
_A port of a Haskell example, just because._

This article is part of [a series about Zippers](https://blog.ploeh.dk/2024/08/19/zippers). In this one, I port the `ListZipper` data structure from the [Learn You a Haskell for Great Good!](https://learnyouahaskell.com/) article also called [Zippers](https://learnyouahaskell.com/zippers).

A word of warning: I'm assuming that you're familiar with the contents of that article, so I'll skip the pedagogical explanations; I can hardly do it better that it's done there.

The code shown in this article is [available on GitHub](https://github.com/ploeh/CSharpZippers).

### Initialization and structure [#](https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c/#04e3cad425414735aff6a3a0507a9855)

In the Haskell code, `ListZipper` is just a type alias, but C# doesn't have that, so instead, we'll have to introduce a class.

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;&nbsp;:&nbsp;<span>IEnumerable</span>&lt;<span>T</span>&gt;
```

Since it implements `IEnumerable<T>`, it may be used like any other sequence, but it also comes with some special operations that enable client code to move forward and backward, as well as inserting and removing values.

The class has the following fields, properties, and constructors:

```
<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;values;
<span>public</span>&nbsp;<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;Breadcrumbs&nbsp;{&nbsp;<span>get</span>;&nbsp;}
 
<span>private</span>&nbsp;<span>ListZipper</span>(<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;<span>values</span>,&nbsp;<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;<span>breadcrumbs</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.values&nbsp;=&nbsp;<span>values</span>;
&nbsp;&nbsp;&nbsp;&nbsp;Breadcrumbs&nbsp;=&nbsp;<span>breadcrumbs</span>;
}
 
<span>public</span>&nbsp;<span>ListZipper</span>(<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;<span>values</span>)&nbsp;:&nbsp;<span>this</span>(<span>values</span>,&nbsp;[])
{
}
 
<span>public</span>&nbsp;<span>ListZipper</span>(<span>params</span>&nbsp;<span>T</span>[]&nbsp;<span>values</span>)&nbsp;:&nbsp;<span>this</span>(<span>values</span>.<span>AsEnumerable</span>())
{
}
```

It uses constructor chaining to initialize a `ListZipper` object with proper [encapsulation](https://blog.ploeh.dk/encapsulation-and-solid). Notice that the master constructor is private. This prevents client code from initializing an object with arbitrary `Breadcrumbs`. Rather, the `Breadcrumbs` (the log, if you will) is going to be the result of various operations performed by client code, and only the `ListZipper` class itself can use this constructor.

You may consider the constructor that takes a single `IEnumerable<T>` as the 'main' `public` constructor, and the other one as a convenience that enables a client developer to write code like `new ListZipper<string>("foo", "bar", "baz")`.

The class' `IEnumerable<T>` implementation only enumerates the `values`:

```
<span>public</span>&nbsp;<span>IEnumerator</span>&lt;<span>T</span>&gt;&nbsp;<span>GetEnumerator</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;values.<span>GetEnumerator</span>();
}
```

In other words, when enumerating a `ListZipper`, you only get the 'forward' `values`. Client code may still examine the `Breadcrumbs`, since this is a `public` property, but it should have little need for that.

(I admit that making `Breadcrumbs` public is a concession to testability, since it enabled me to write assertions against this property. It's a form of [structural inspection](https://blog.ploeh.dk/2013/04/04/structural-inspection), which is a technique that I use much less than I did a decade ago. Still, in this case, while you may argue that it violates [information hiding](https://en.wikipedia.org/wiki/Information_hiding), it at least doesn't allow client code to put an object in an invalid state. Had the `ListZipper` class been a part of a reusable library, I would probably have hidden that data, too, but since this is exercise code, I found this an acceptable compromise. Notice, too, that in the original Haskell code, the breadcrumbs are available to client code.)

Regular readers of this blog may be aware that [I usually favour IReadOnlyCollection<T> over IEnumerable<T>](https://blog.ploeh.dk/2013/07/20/linq-versus-the-lsp). Here, on the other hand, I've allowed `values` to be any `IEnumerable<T>`, which includes infinite sequences. I decided to do that because Haskell lists, too, may be infinite, and as far as I can tell, `ListZipper` actually does work with infinite sequences. I have, at least, written a few tests with infinite sequences, and they pass. (I may still have missed an edge case or two. I can't rule that out.)

### Movement [#](https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c/#908d0fe3cf5d453da3541127ae365d00)

It's not much fun just being able to initialize an object. You also want to be able to do something with it, such as moving forward:

```
<span>public</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;?&nbsp;<span>GoForward</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>head</span>&nbsp;=&nbsp;values.<span>Take</span>(1);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(!<span>head</span>.<span>Any</span>())
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>null</span>;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>tail</span>&nbsp;=&nbsp;values.<span>Skip</span>(1);
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;(<span>tail</span>,&nbsp;<span>head</span>.<span>Concat</span>(Breadcrumbs));
}
```

You can move forward through any `IEnumerable`, so why make things so complicated? The benefit of this `GoForward` method ([function](https://en.wikipedia.org/wiki/Pure_function), really) is that it records where it came from, which means that moving backwards becomes an option:

```
<span>public</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;?&nbsp;<span>GoBack</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>head</span>&nbsp;=&nbsp;Breadcrumbs.<span>Take</span>(1);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(!<span>head</span>.<span>Any</span>())
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>null</span>;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>tail</span>&nbsp;=&nbsp;Breadcrumbs.<span>Skip</span>(1);
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;(<span>head</span>.<span>Concat</span>(values),&nbsp;<span>tail</span>);
}
```

This test may serve as an example of client code that makes use of those two operations:

```
[<span>Fact</span>]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>GoBack1</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>int</span>&gt;(1,&nbsp;2,&nbsp;3,&nbsp;4);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>sut</span>.<span>GoForward</span>()?.<span>GoForward</span>()?.<span>GoForward</span>()?.<span>GoBack</span>();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>([3,&nbsp;4],&nbsp;<span>actual</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>([2,&nbsp;1],&nbsp;<span>actual</span>?.Breadcrumbs);
}
```

Going forward takes the first element off `values` and adds it to the front of `Breadcrumbs`. Going backwards is nearly symmetrical: It takes the first element off the `Breadcrumbs` and adds it back to the front of the `values`. Used in this way, `Breadcrumbs` works as a [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)).

Notice that both `GoForward` and `GoBack` admit the possibility of failure. If `values` is empty, you can't go forward. If `Breadcrumbs` is empty, you can't go back. In both cases, the functions return `null`, which are also indicated by the `ListZipper<T>?` return types; notice the question mark, [which indicates that the value may be null](https://learn.microsoft.com/dotnet/csharp/nullable-references). If you're working in a context or language where that feature isn't available, you may instead consider taking advantage of the [Maybe monad](https://blog.ploeh.dk/2022/04/25/the-maybe-monad) (which is also what you'd [idiomatically](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic) do in Haskell).

To be clear, the [Zippers article](https://learnyouahaskell.com/zippers) does discuss handling failures using Maybe, but only applies it to its binary tree example. Thus, the error handling shown here is my own addition.

### Modifications [#](https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c/#704f23586ead4b199b171baa50dfd1da)

In addition to moving back and forth in the list, we can also modify it. The following operations are also not in the Zippers article, but are rather my own contributions. Adding a new element is easy:

```
<span>public</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;&nbsp;<span>Insert</span>(<span>T</span>&nbsp;<span>value</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;(values.<span>Prepend</span>(<span>value</span>),&nbsp;Breadcrumbs);
}
```

Notice that this operation is always possible. Even if the list is empty, we can `Insert` a value. In that case, it just becomes the list's first and only element.

A simple test demonstrates usage:

```
[<span>Fact</span>]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>InsertAtFocus</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>string</span>&gt;(<span>"foo"</span>,&nbsp;<span>"bar"</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>sut</span>.<span>GoForward</span>()?.<span>Insert</span>(<span>"ploeh"</span>).<span>GoBack</span>();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>NotNull</span>(<span>actual</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>([<span>"foo"</span>,&nbsp;<span>"ploeh"</span>,&nbsp;<span>"bar"</span>],&nbsp;<span>actual</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Empty</span>(<span>actual</span>.Breadcrumbs);
}
```

Likewise, we may attempt to remove an element from the list:

```
<span>public</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;?&nbsp;<span>Remove</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(!values.<span>Any</span>())
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>null</span>;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;(values.<span>Skip</span>(1),&nbsp;Breadcrumbs);
}
```

Contrary to `Insert`, the `Remove` operation will fail if `values` is empty. Notice that this doesn't necessarily imply that the list as such is empty, but only that the focus is at the end of the list (which, of course, never happens if `values` is infinite):

```
[<span>Fact</span>]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>RemoveAtEnd</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>string</span>&gt;(<span>"foo"</span>,&nbsp;<span>"bar"</span>).<span>GoForward</span>()?.<span>GoForward</span>();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>sut</span>?.<span>Remove</span>();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Null</span>(<span>actual</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>NotNull</span>(<span>sut</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Empty</span>(<span>sut</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>([<span>"bar"</span>,&nbsp;<span>"foo"</span>],&nbsp;<span>sut</span>.Breadcrumbs);
}
```

In this example, the focus is at the end of the list, so there's nothing to remove. The list, however, is not empty, but all the data currently reside in the `Breadcrumbs`.

Finally, we can combine insertion and removal to implement a replacement operation:

```
<span>public</span>&nbsp;<span>ListZipper</span>&lt;<span>T</span>&gt;?&nbsp;<span>Replace</span>(<span>T</span>&nbsp;<span>newValue</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>Remove</span>()?.<span>Insert</span>(<span>newValue</span>);
}
```

As the name implies, this operation replaces the value currently in focus with a completely different value. Here's an example:

```
[<span>Fact</span>]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>ReplaceAtFocus</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>string</span>&gt;(<span>"foo"</span>,&nbsp;<span>"bar"</span>,&nbsp;<span>"baz"</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>sut</span>.<span>GoForward</span>()?.<span>Replace</span>(<span>"qux"</span>)?.<span>GoBack</span>();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>NotNull</span>(<span>actual</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>([<span>"foo"</span>,&nbsp;<span>"qux"</span>,&nbsp;<span>"baz"</span>],&nbsp;<span>actual</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Empty</span>(<span>actual</span>.Breadcrumbs);
}
```

Once more, this may fail if the current focus is empty, so `Replace` also returns a nullable value.

### Conclusion [#](https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c/#5979ae4ab42543f79df4e572f7f5c2c3)

For a C# developer, the `ListZipper<T>` class looks odd. Why would you ever want to use this data structure? Why not just use [List<T>](https://learn.microsoft.com/dotnet/api/system.collections.generic.list-1)?

As I hope I've made clear in the [introduction article](https://blog.ploeh.dk/2024/08/19/zippers), I can't, indeed, think of a good reason.

I've gone through this exercise [to hone my skills](https://blog.ploeh.dk/2020/01/13/on-doing-katas), and to prepare myself for the more intimidating exercise it is to implement a binary tree Zipper.

**Next:** A Binary Tree Zipper in C#.
