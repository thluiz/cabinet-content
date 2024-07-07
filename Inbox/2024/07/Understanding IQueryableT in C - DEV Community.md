---
created: 2024-07-06T17:59:19 (UTC -03:00)
tags: [csharp,dotnet,learning,database,software,coding,development,engineering,inclusive,community]
source: https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev
author: 
---

# Understanding IQueryable<T> in C# - DEV Community

> ## Excerpt
> Hi There! 👋🏻   You've probably used IEnumerable<T>, and most certainly used...

---
## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#hi-there)Hi There! 👋🏻

You've probably used `IEnumerable<T>`, and most certainly used `List<T>` if you've coded in C# before. These two are very popular and are often presented early to you when you're learning the language. But have you heard of `IQueryable<T>`? This one is a little more advanced, but it's got so much power that you should know about, so you can make good use of it.  
Ladies and gents, we're going on a journey to demystify the `IQueryable` interface, so... Grab a coffee and let's get started! ☕️

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#whats-raw-iquyerablelttgt-endraw-)What's `IQuyerable<T>`? 🤔

The `IQueryable` interface is a cornerstone of LINQ, one of C#'s most powerful features. This interface is specifically designed for querying data from various sources that implement `IQueryable<T>`, such as SQL databases or in-memory collections like `List<T>`. What sets `IQueryable` apart are its compelling features that make it versatile and efficient for data querying.

Let's begin by highlighting the features of this interface that do give it this speciality and uniqueness.

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#1-deferred-execution)1: Deferred execution 🦥

Deferred execution, which happens to be a feature of `IEnumerable`, also inherited by `IQueryable`, is in simple words, delaying the execution of the query until the data is actually needed. Didn't click? Keep reading.

Let's look at this basic code snippet, to see what deferred execution means in a more hands-on way:  

```

<span>List</span><span>&lt;</span><span>FamousPerson</span><span>&gt;</span> <span>famousPeople</span> <span>=</span>
<span>[</span>
    <span>new</span> <span>FamousPerson</span><span>(</span><span>1</span><span>,</span> <span>"Sandy Cheeks"</span><span>,</span> <span>false</span><span>),</span>
    <span>new</span> <span>FamousPerson</span><span>(</span><span>2</span><span>,</span> <span>"Tony Stark"</span><span>,</span> <span>true</span><span>),</span>
    <span>new</span> <span>FamousPerson</span><span>(</span><span>3</span><span>,</span> <span>"Captain Marvel"</span><span>,</span> <span>true</span><span>),</span>
    <span>new</span> <span>FamousPerson</span><span>(</span><span>4</span><span>,</span> <span>"Captain America"</span><span>,</span> <span>true</span><span>),</span>
    <span>new</span> <span>FamousPerson</span><span>(</span><span>5</span><span>,</span> <span>"SpongeBob SquarePants"</span><span>,</span> <span>false</span><span>),</span>
    <span>new</span> <span>FamousPerson</span><span>(</span><span>6</span><span>,</span> <span>"Hulk"</span><span>,</span> <span>false</span><span>)</span>
<span>];</span>

<span>IQueryable</span><span>&lt;</span><span>FamousPerson</span><span>&gt;</span> <span>famousAndCanFly</span> <span>=</span> <span>famousPeople</span>
    <span>.</span><span>AsQueryable</span><span>()</span>
    <span>.</span><span>Where</span><span>(</span><span>x</span> <span>=&gt;</span> <span>x</span><span>.</span><span>CanFly</span><span>);</span>

<span>foreach</span> <span>(</span><span>var</span> <span>fp</span> <span>in</span> <span>famousAndCanFly</span><span>)</span>
<span>{</span>
    <span>Console</span><span>.</span><span>WriteLine</span><span>(</span><span>$"</span><span>{</span><span>fp</span><span>.</span><span>Name</span><span>}</span><span> can FLY!"</span><span>);</span>
<span>}</span>

<span>record</span> <span>FamousPerson</span><span>(</span><span>int</span> <span>Id</span><span>,</span> <span>string</span> <span>Name</span><span>,</span> <span>bool</span> <span>CanFly</span><span>);</span>

```

I want you to copy the code, and use the debugger to see what the value of `famousAndCanFly` evaluates to. You might think it'll be a collection of 3 people, but actually it's not. You will see that the value doesn't carry any data, but once you step inside the `foreach` loop, the results are carried out. This is simply what deferred execution means, the execution is delayed until the data is actually needed (i.e the enumerating of the query results inside the `foreach` loop).

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#2-expression-trees)2: Expression trees 🌳

I've chained some additional query filters to the previous query, so now it looks like this:  

```

<span>IQueryable</span><span>&lt;</span><span>FamousPerson</span><span>&gt;</span> <span>famousAndCanFly</span> <span>=</span> <span>famousPeople</span>
    <span>.</span><span>AsQueryable</span><span>()</span>
    <span>.</span><span>Where</span><span>(</span><span>x</span> <span>=&gt;</span> <span>x</span><span>.</span><span>CanFly</span><span>);</span>

<span>famousAndCanFly</span> <span>=</span> <span>famousAndCanFly</span>
    <span>.</span><span>Where</span><span>(</span><span>x</span> <span>=&gt;</span> <span>x</span><span>.</span><span>Id</span> <span>&lt;</span> <span>3</span><span>);</span>

<span>famousAndCanFly</span> <span>=</span> <span>famousAndCanFly</span><span>.</span>
    <span>Where</span><span>(</span><span>x</span> <span>=&gt;</span> <span>x</span><span>.</span><span>Name</span><span>.</span><span>Contains</span><span>(</span><span>"s"</span><span>,</span> <span>StringComparison</span><span>.</span><span>OrdinalIgnoreCase</span><span>));</span>

<span>Console</span><span>.</span><span>WriteLine</span><span>(</span><span>famousAndCanFly</span><span>.</span><span>Expression</span><span>);</span>

```

It's just basic LINQ extension method calls here, but the thing to note is, the console log on the last line. What is the `Expression` property of `IQueryable`? This is basically a tree of expressions, which `IQueryable` puts together as you compose the query. It allows the data source provider, to grab the query expression tree, and translate it into something that can be used against that data source. For that reason, the data source provider has to provide an implementation for `IQueryable`.

If you were to run the previous code, it'd print something like this:  
`System.Collections.Generic.List1[FamousPerson].Where(x => x.CanFly).Where(x => (x.Id < 3)).Where(x => x.Name.Contains("s", OrdinalIgnoreCase))`

See, it's just a bunch of chained query expressions and where statements, which here in our case, the data source happens to be an in memory collection, and since it implements `IQueryable`, you can bet that it knows how to properly translate that into something the in-memory collection can understand.

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#examples-of-raw-iqueryable-endraw-functionality)Examples of `IQueryable` Functionality

I said earlier that `IQueryable` is a part of `System.Linq` namespace, and everything LINQ you can do with other collections, can be done on this interface too. From using `Where`, `OrderBy`, `Select` and literally anything else, you can keep on chaining method calls to compose the most complex query you could ever imagine. You're literally just confined by how much LINQ you know.  

```

<span>var</span> <span>filtered</span> <span>=</span> <span>query</span><span>.</span><span>Where</span><span>(</span><span>x</span> <span>=&gt;</span> <span>x</span><span>.</span><span>Age</span> <span>&gt;</span> <span>30</span><span>);</span>

```

```

<span>var</span> <span>orderedDesc</span> <span>=</span> <span>query</span><span>.</span><span>OrderByDescending</span><span>(</span><span>x</span> <span>=&gt;</span> <span>x</span><span>.</span><span>Name</span><span>);</span>

```

```

 <span>var</span> <span>projected</span> <span>=</span> <span>query</span><span>.</span><span>Select</span><span>(</span><span>x</span> <span>=&gt;</span> <span>new</span> <span>{</span> <span>x</span><span>.</span><span>Name</span><span>,</span> <span>x</span><span>.</span><span>Age</span> <span>});</span>

```

```

<span>var</span> <span>firstOrDefault</span> <span>=</span> <span>query</span><span>.</span><span>FirstOrDefault</span><span>();</span>
<span>var</span> <span>lastOrDefault</span> <span>=</span> <span>query</span><span>.</span><span>LastOrDefault</span><span>();</span>
<span>var</span> <span>single</span> <span>=</span> <span>query</span><span>.</span><span>Single</span><span>();</span> 

```

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#3-so-much-optimization)3: So Much Optimization ⚙️

Because the query is structured into a **tree** of expressions, the provider (such as **Entity Framework Core**) can take that expression tree and translate it into a query language appropriate for the data store, such as **SQL** for **SQL Server** or **PostgreSQL**, or **LINQ** for in-memory data stores.

Since the translation is handled by the provider, it can optimize the query for better performance and efficiency. This optimization might involve translating the query into a more efficient SQL statement, applying indexes, or other database-specific optimizations. This allows you to query the data store efficiently without needing to manually optimize each query.

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#extending-iqueryable)Extending IQueryable

I was coding a repository for a blog project, and I wanted to add sorting, pagination, and filter by title to the `GetLatestPosts` method. Now while it's possible to cram them in the same method, it'd be much nicer if there's a way to place those methods into a centric place, and just chain call them to compose that perfect query. Enter Extension methods!

> ⚠️ Extension methods are not something specific to IQueryable only, they can be used to extend any type.

Before I show you the extension methods code, I'd like to show you the refactored code, and how the improved version actually looks like:  

```

<span>public</span> <span>async</span> <span>Task</span><span>&lt;</span><span>PaginatedReadOnlyCollection</span><span>&lt;</span><span>Post</span><span>&gt;&gt;</span>
    <span>GetLatestPostsAsync</span><span>(</span><span>int</span> <span>pageNumber</span><span>,</span> <span>int</span> <span>pageSize</span><span>,</span> <span>string</span><span>?</span> <span>title</span><span>,</span> <span>PostSortOption</span> <span>sortOption</span><span>,</span> <span>CancellationToken</span> <span>cancellationToken</span><span>)</span>
<span>{</span>
    <span>try</span>
    <span>{</span>
        <span>var</span> <span>query</span> <span>=</span> <span>db</span><span>.</span><span>Posts</span><span>.</span><span>AsQueryable</span><span>();</span>

        <span>var</span> <span>filteredQuery</span> <span>=</span> <span>query</span><span>.</span><span>ApplyFilter</span><span>(</span><span>title</span><span>);</span>
        <span>var</span> <span>sortedQuery</span> <span>=</span> <span>filteredQuery</span><span>.</span><span>ApplySorting</span><span>(</span><span>sortOption</span><span>);</span>

        <span>var</span> <span>totalCount</span> <span>=</span> <span>await</span> <span>filteredQuery</span><span>.</span><span>CountAsync</span><span>(</span><span>cancellationToken</span><span>);</span>
        <span>var</span> <span>posts</span> <span>=</span> <span>await</span> <span>sortedQuery</span>
            <span>.</span><span>ApplyPagination</span><span>(</span><span>pageNumber</span><span>,</span> <span>pageSize</span><span>)</span>
            <span>.</span><span>Execute</span><span>(</span><span>cancellationToken</span><span>);</span>

        <span>var</span> <span>paginatedPosts</span> <span>=</span> <span>new</span> <span>PaginatedReadOnlyCollection</span><span>&lt;</span><span>Post</span><span>&gt;(</span>
            <span>totalCount</span><span>,</span>
            <span>pageNumber</span><span>,</span>
            <span>pageSize</span><span>,</span>
            <span>posts</span><span>.</span><span>AsReadOnly</span><span>()</span>
        <span>);</span>

        <span>return</span> <span>paginatedPosts</span><span>;</span>
    <span>}</span>
    <span>catch</span> <span>(</span><span>OperationCanceledException</span><span>)</span>
    <span>{</span>
        <span>logger</span><span>.</span><span>LogInformation</span><span>(</span><span>"Loading posts was cancelled"</span><span>);</span>
        <span>return</span> <span>PaginatedReadOnlyCollection</span><span>&lt;</span><span>Post</span><span>&gt;.</span><span>Empty</span><span>(</span><span>pageNumber</span><span>,</span> <span>pageSize</span><span>);</span>
    <span>}</span>
<span>}</span>

```

This is a lot of code I know, but the main focus here is the 4 methods that aren't LINQ methods.  
`ApplyFilter(string)`, `ApplySorting(PostSortOption)`, `ApplyPagination(int, int)`, and `Execute(CancellationToken)`.

All these methods are extension methods I wrote to extend on `IQueryable<Post>`, which makes it much cleaner and more concise to write and compose large queries on the `Post` data model.

Here's the code inside `PostsQueryExtensions.cs` which hosts those extension methods we just discussed:  

```

<span>public</span> <span>static</span> <span>class</span> <span>PostsQueryExtensions</span>
<span>{</span>
    <span>public</span> <span>static</span> <span>IQueryable</span><span>&lt;</span><span>Post</span><span>&gt;</span> <span>ApplyFilter</span><span>(</span><span>this</span> <span>IQueryable</span><span>&lt;</span><span>Post</span><span>&gt;</span> <span>query</span><span>,</span> <span>string</span><span>?</span> <span>title</span><span>)</span>
    <span>{</span>
        <span>if</span> <span>(!</span><span>string</span><span>.</span><span>IsNullOrEmpty</span><span>(</span><span>title</span><span>))</span>
        <span>{</span>
            <span>query</span> <span>=</span> <span>query</span><span>.</span><span>Where</span><span>(</span><span>post</span> <span>=&gt;</span> <span>post</span><span>.</span><span>Title</span><span>.</span><span>Contains</span><span>(</span><span>title</span><span>,</span> <span>StringComparison</span><span>.</span><span>OrdinalIgnoreCase</span><span>));</span>
        <span>}</span>

        <span>return</span> <span>query</span><span>;</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>IOrderedQueryable</span><span>&lt;</span><span>Post</span><span>&gt;</span> <span>ApplySorting</span><span>(</span><span>this</span> <span>IQueryable</span><span>&lt;</span><span>Post</span><span>&gt;</span> <span>query</span><span>,</span> <span>PostSortOption</span> <span>sortOption</span><span>)</span>
    <span>{</span>
        <span>return</span> <span>sortOption</span> <span>switch</span>
        <span>{</span>
            <span>PostSortOption</span><span>.</span><span>MostComments</span> <span>=&gt;</span> <span>query</span><span>.</span><span>OrderByDescending</span><span>(</span><span>p</span> <span>=&gt;</span> <span>p</span><span>.</span><span>Comments</span><span>.</span><span>Count</span><span>),</span>
            <span>PostSortOption</span><span>.</span><span>MostLiked</span> <span>=&gt;</span> <span>query</span><span>.</span><span>OrderByDescending</span><span>(</span><span>p</span> <span>=&gt;</span> <span>p</span><span>.</span><span>LikeCount</span><span>),</span>
            <span>PostSortOption</span><span>.</span><span>MostViews</span> <span>=&gt;</span> <span>query</span><span>.</span><span>OrderByDescending</span><span>(</span><span>p</span> <span>=&gt;</span> <span>p</span><span>.</span><span>Views</span><span>),</span>
            <span>_</span> <span>=&gt;</span> <span>query</span><span>.</span><span>OrderByDescending</span><span>(</span><span>p</span> <span>=&gt;</span> <span>p</span><span>.</span><span>PublishedOn</span><span>)</span>
        <span>};</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>IQueryable</span><span>&lt;</span><span>Post</span><span>&gt;</span> <span>ApplyPagination</span><span>(</span><span>this</span> <span>IQueryable</span><span>&lt;</span><span>Post</span><span>&gt;</span> <span>query</span><span>,</span> <span>int</span> <span>pageNumber</span><span>,</span> <span>int</span> <span>pageSize</span><span>)</span>
    <span>{</span>
        <span>return</span> <span>query</span>
            <span>.</span><span>Skip</span><span>((</span><span>pageNumber</span> <span>-</span> <span>1</span><span>)</span> <span>*</span> <span>pageSize</span><span>)</span>
            <span>.</span><span>Take</span><span>(</span><span>pageSize</span><span>);</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>async</span> <span>Task</span><span>&lt;</span><span>List</span><span>&lt;</span><span>Post</span><span>&gt;&gt;</span> <span>Execute</span><span>(</span><span>this</span> <span>IQueryable</span><span>&lt;</span><span>Post</span><span>&gt;</span> <span>query</span><span>,</span> <span>CancellationToken</span> <span>cancellationToken</span><span>)</span>
        <span>=&gt;</span> <span>await</span> <span>query</span><span>.</span><span>ToListAsync</span><span>(</span><span>cancellationToken</span><span>);</span>
<span>}</span>

```

As you can see, all these methods extend on the query and return an updated version of it, essentially, composing the entire query before finally the `Execute` method pulls the trigger, and calls `ToListAsync` which in turn, grabs the results of the entire query and enumerates them such that the calling code can read and display the results.

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#conclusion)Conclusion ✅

In this post, we went over the `IQuyerable` interface, from the basics, highlighting key features of it, showing you code samples of how it's used, and lastly we saw how we can extend on this interface for a given data model so that we can facilitate and make writing complex querying code less redundant, more fluent and concise.  
I hope that post was a good introductory to this powerful interface, I hope it was useful and you ended up learning something!

If you got any feedback on the code provided in the post, please feel free to point them out!

## [](https://dev.to/rasheedmozaffar/understanding-iqueryable-in-c-4n37?ref=dailydev#thanks-for-reading)**Thanks for reading!**
