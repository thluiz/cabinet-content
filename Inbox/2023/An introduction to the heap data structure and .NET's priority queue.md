---
created: 2024-03-06T08:49:48 (UTC -03:00)
tags: []
source: https://andrewlock.net/an-introduction-to-the-heap-data-structure-and-dotnets-priority-queue/
author: Andrew Lock
---

# An introduction to the heap data structure and .NET's priority queue

> ## Excerpt
> Hi, my name is Andrew, or ‘Sock’ to most people. This blog is where I share my experiences as I journey into ASP.NET Core.

---
March 05, 2024 ~8 min read

In this post I provide an introduction to the [heap](https://en.wikipedia.org/wiki/Heap_(data_structure)) data structure, describe why it's useful, and show how it's used in .NET's `PriorityQueue` type.

## [What is a heap?](https://andrewlock.net/an-introduction-to-the-heap-data-structure-and-dotnets-priority-queue/#what-is-a-heap-)

You may be familiar with the phrase "heap" in .NET [when discussing memory-management](https://learn.microsoft.com/en-us/aspnet/core/performance/memory?view=aspnetcore-8.0#how-garbage-collection-gc-works-in-net-core), but in this post I'm talking about something completely unrelated.

A _heap_ is a data structure that is typically represented as a tree, and which satisfies one the following rules, depending on whether it's a _max-heap_ or a _min-heap_

-   **Min-heap**—For every node in the tree, the node's value is _less_ than (or equal to) the value of its children.
-   **Max-heap**—For every node in the tree, the node's value is _greater_ than (or equal to) the value of its children.

That may be tricky to visualize, so the following diagram shows a visualization of a min-heap, including some common nomenclature.

![An example of a min-heap describing relationships](https://andrewlock.net/content/images/2024/min_heap.svg)

Generally relationships are described as though the heap represents a family tree:

-   **root node**—The node at the top of the heap. In a min-heap, the root node has the smallest value.
-   **sibling**—Nodes that share a common parent.
-   **cousings**—Nodes that share a common grand-parent.

In the diagram above you can see that for every node, the parent value is smaller than (or equal to) the child. This is the important characteristic property of the heap, termed the "heap property".

However, you can also see that there is _no_ implied relationship between siblings (other than the fact they are both larger than their parent). Similarly, there is no relationship between cousins, other than the fact they are guaranteed larger than their shared grandparent (in a min-heap).

> Note that this is different to [binary search tree](https://en.wikipedia.org/wiki/Binary_search_tree) in which there _is_ a relationship between siblings and other nodes.

The diagram above _conceptually_ represents the heap data structure, but how is this implemented in practice in code?

Heaps are usually implemented as an array in which each element in the array represents a node in the heap. The position inside the array defines the relationship between the nodes, as shown in the following example:

![How a min-heap maps to a backing array](https://andrewlock.net/content/images/2024/heap_implementation.svg)

In this example :

-   The element at index 0 is the root node. So for a min-heap it contains the minimum value in the heap.
-   The elements at position 1 and 2 are the children of the root node
-   The elements at position 3 and 4 are the children of the node at position 1, 5 and 6 are the children of node 2 etc.

The heap shown in the examples above are all _binary_ heaps, in that every node has up to two children (and two siblings). This heap is described as having an _arity_ of 2. You can have higher arity heaps such ternary (arity of 3) or quaternary (arity of 4) as we'll see later.

So that's all well and good, but why should you care? What can you use heaps for?

## [Why is a heap useful?](https://andrewlock.net/an-introduction-to-the-heap-data-structure-and-dotnets-priority-queue/#why-is-a-heap-useful-)

The main use for the heap data structure is to implement a [_priority queue_](https://en.wikipedia.org/wiki/Priority_queue). A "normal" [queue](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)) data structure stores elements much like a queue at a shop—you _enqueue_ elements by adding them at one end, and you _dequeue_ elements by removing them from the other end. It's a first-in-first-out (FIFO) data structure, so you get the elements back in the same order you added them.

The _priority queue_ is a variation on the standard queue. You can still enqueue and dequeue elements, but instead of getting the elements back in the order you added them, you always get the _smallest_ of the remaining elements back.

To give a concrete example, the following uses the .NET 6+ [`PriorityQueue` type](https://github.com/dotnet/runtime/blob/00474fcb63082b51a0615039c8c1b5d4666f91b2/src/libraries/System.Collections/src/System/Collections/Generic/PriorityQueue.cs), enqueues a collection of elements, and then dequeues them one by one:

```csharp
// Create a priority queue that stores a string, and uses an int priority value var queue = new PriorityQueue<string, int>([ ("A", 15), // ("B", 7), // ("C", 23), // Add the unordered elements ("D", 2), // ("E", 22), // ]); // Remove the elements one at a time, and print the result while (queue.TryDequeue(out var element, out var priority)) { Console.WriteLine($"{element}: {priority}"); } // Prints // D: 2 // B: 7 // A: 15 // E: 22 // C: 23
```

As you can see above, the elements are dequeued in order of smallest to largest. The heap data structure is the common way to implement a priority queue (though you _can_ implement a priority queue in lots of other ways).

The priority queue has many applications, but one of the most well known is its use in graph algorithms such as in [Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) for finding the shortest distance between two nodes. I'm not going to cover that here, but we'll take a look at this in a subsequent post.

## [The .NET `PriorityQueue` uses a heap](https://andrewlock.net/an-introduction-to-the-heap-data-structure-and-dotnets-priority-queue/#the-net-priorityqueue-uses-a-heap)

The `PriorityQueue` type I showed in the previous section uses an "array-backed quaternary min-heap" according to the documentation. So that means each node in the heap has 4 children. For example, if we take the binary min-heap I showed at the start of this post and re-draw it as a quaternary min-heap (i.e. a d-ary min-heap with arity 4), we get something that looks like this:

![An example of a quaternary min-heap showing that each node has 4 children](https://andrewlock.net/content/images/2024/d-ary_heap.svg)

As you can see, the rules for binary min-heaps still apply here:

-   The root node contains the smallest value
-   Every node is greater than its parent value
-   There is no implied relationship between siblings

Binary and d-ary heaps have slightly different runtime characteristics (i.e. different [big _O_](https://en.wikipedia.org/wiki/Big_O_notation) characteristics), with binary heaps being slightly faster at removing the root node, while d-ary heaps are faster at some other operations. In general, d-ary heaps tend to have [better runtime performances](https://academic.oup.com/comjnl/article/34/5/428/553938?login=false) than binary heaps due to the way memory caching works, so are often preferred.

Both binary and d-ary array-backed heaps contain the same number of elements—it's the same as the number of nodes— they're just arranged a little differently. For example, the heap above would be laid out something like the following for a quaternary min-heap:

![An example of a quaternary min-heap showing that each node has 4 children](https://andrewlock.net/content/images/2024/d-ary_implementation.svg)

It's worth noting that if you add all these elements to a priority queue, they won't necessarily be stored in the specific slots shown above. The only thing you can guarantee is that the heap property holds; that is, for a min-heap, every parent node is smaller than all its children.

## [Using .NET's `PriorityQueue` implementation](https://andrewlock.net/an-introduction-to-the-heap-data-structure-and-dotnets-priority-queue/#using-net-s-priorityqueue-implementation)

.NET's `PriorityQueue` has a bunch of methods available that are typical for a priority queue implementation. The following code demonstrates some of these methods, describes what each of them do, and gives their common "computer science" names where they apply.

```csharp
// Create a priority queue that stores a string, and uses an int priority value var queue = new PriorityQueue<string, int>([ ("A", 15), // ("B", 7), // Create a priority queue from the unordered elements, ("C", 23), // sorting them into a heap internally ("D", 2), // Also called "heapify" on a heap ("E", 22), // ]); // Find the minimum element in the queue and return it // Don't remove the value from the queue // Also called "find-min" on a heap string peekResult = queue.Peek(); // "D" // Try to fetch the minimum element and priority from the heap // Returns true if the queue has any elements, false otherwise if (queue.TryPeek(out string? result, out int priority)) { // result = "D", priority = 2 } // Find the minimum element in the queue (i.e. the root node), // return it, and remove it from the queue. // // Removing the root node makes the heap violate the min-heap rule, // so it must be rebalanced by performing a "sift down" or "sink" // operation. // // Dequeue is also called "extract-min" or "pop" on a heap string dequeueResult = queue.Dequeue(); // "D" // Try to fetch and remove the minimum element and priority. // Returns true if the queue has any elements, false otherwise if (queue.TryDequeue(out string? result2, out int priority2)) { // result2 = "B", priority = 7 } // Add a new element and priority to the queue, in the first // available space. // // Adding a new node will likely make the heap violate the // min-heap rule, so it must be rebalanced by performing a // "sift up" or "swim" operation // // Enqueue is also called "insert" or "push" on a heap queue.Enqueue(element: "F", priority: 42); // As above, but add multiple elements sequentially queue.EnqueueRange([("G", 3), ("H", 13)]); // Remove the root node, and them immediately add a new node. // // The heap will need to be rebalanced, but using DequeueEnqueue() // is more efficient then calling Dequeue() and Enqueue() sequentially, // as rebalancing only needs to be done once, instead of twice. // // DequeueEnqueue is also called "replace" on a heap string dequeued1 = queue.DequeueEnqueue(element: "I", priority: 19); // "G" // Add a new node, and then immediately remove the root node. // // The heap will need to be rebalanced, but using EnqueueDequeue() // is more efficient then calling Enqueue() and Dequeue() sequentially, // as rebalancing only needs to be done once, instead of twice. string dequeued2 = queue.EnqueueDequeue(element: "J", priority: 31); // "H"
```

The methods shown above represent the common ways to interact with a priority queue, and hence the underlying heap structure. The `PriorityQueue` has several other helper methods for operating on the queue, and controlling how it works:

```csharp
// Provides access to the underlying items, allowing you // to enumerate them. As the name suggests, the items are // returned in no specific order. var unorderedItems = queue.UnorderedItems; // The number of nodes in the priority queue var count = queue.Count; // Remove and discard all the items from the queue queue.Clear(); // By default, the priority queue uses the default comparer // Comparer<TPriority>.Default, but you can also provide a // custom comparer to use instead. // // The following comparer turns the priority queue into a // max-heap instead of min-heap by reversing the comparer. var inverseComparer = Comparer<int>.Create((a, b) => 0 - a.CompareTo(b)); var maxQueue = new PriorityQueue<string, int>(inverseComparer); // Reduce the capacity of the array used backing the heap as // long as this would reduce the capacity to less than 90% // of the previous capacity. queue.TrimExcess();
```

That covers all the APIs available in `PriorityQueue` up to .NET 8; .NET 9 adds an additional API, `Remove()` which I'll discuss in a later post.

In the next post, we'll look in even more detail at the `PriorityQueue` type, to see how the various APIs are implemented behind the scenes.

## [Summary](https://andrewlock.net/an-introduction-to-the-heap-data-structure-and-dotnets-priority-queue/#summary)

In this post I provided an introduction to the heap data structure. I described some of the terminology used with the heap data structure and how the tree structure is typically mapped to an array. I then showed how the binary heap structure can be expanded to a d-ary form, such as a quaternary structure as is used in the .NET `PriorityQueue` type. Finally, I showed the methods available on `PriorityQueue` and how many of them map to standard operations on heap data structures. In the next post, we'll take a look at how some of these methods are implemented.

 [![Using Unix domain sockets with ASP.NET Core and HttpClient](https://andrewlock.net/content/images/2024/uds.webp)Previous Using Unix domain sockets with ASP.NET Core and HttpClient](https://andrewlock.net/using-unix-domain-sockets-with-aspnetcore-and-httpclient/)

Andrew Lock | .Net Escapades

![](https://andrewlock.net/assets/img/icons/apple/apple-touch-icon-180x180.png) Want an email when  
there's new posts?
