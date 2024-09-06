---
created: 2024-09-05T17:44:02 (UTC -03:00)
tags: []
source: https://www.bytedrum.com/posts/bloom-filters/
author: Tomas Stropus
---

# Bloom Filters: The Unsung Heroes of Computer Science | ByteDrum

> ## Excerpt
> In the world of Bloom Filters, false positives are features, not bugs! Learn how this probabilistic data structure can save your RAM from a nervous breakdown while keeping your lookups lightning-fast.

---
Picture this: It’s 2 AM, and my desk is a chaotic landscape of pens, notebooks, various devices, and a tangle of cables. The warm glow of my lamp bathes the room in a soft light, its color tone carefully adjusted to reduce eye strain during these late-night sessions. My trusty coffee mug, now empty, stands guard over my latest caffeine-fueled endeavor.

My orange cat, Quindim, is curled up on my closed laptop, purring contentedly as he watches my nocturnal activities with sleepy curiosity. I’m not planning the next tech startup or debugging my home network. No, I’m doing something far geekier - I’m relearning data structures and algorithms.

As I sketch out a particularly intricate binary tree, Quindim stretches and repositions himself, clearly unimpressed by my sudden fascination with computer science fundamentals. But me? I’m loving every caffeine-fueled minute of it. There’s something oddly thrilling about revisiting these concepts with years of real-world experience under my belt.

![I can't help it, DSA doesn't deserve all the bad rep it gets, considering how useful and fascinating it can be](https://www.bytedrum.com/_astro/dsa.DhUcSZ0R_gD7zm.webp)

## From Lectures to Late Nights[](https://www.bytedrum.com/posts/bloom-filters/#from-lectures-to-late-nights)

About ten years ago, I was a university student, eagerly absorbing knowledge about binary trees, graph algorithms, and optimization techniques. I found myself captivated by these topics from the very beginning. I vividly remember spending extra hours after our Intro to Programming 101 class, diving deep into the Traveling Salesman Problem and exploring different variants of Ant Colony Systems.

These algorithms, especially the heuristic ones, were inherently fascinating. The way they mimicked natural processes to solve complex problems resonated with me. I’d stay up late, coding different variations and tweaking parameters, marveling at how small changes could significantly improve efficiency.

Fast forward to a few weeks ago. While organizing my Google Drive — a digital attic of sorts — I stumbled upon a folder of old university PowerPoint presentations and homework assignments. Opening these files triggered a rush of memories. There they were — my early attempts at implementing sorting algorithms, first forays into graph theory, and pages of notes on data structure trade-offs — reigniting the passion I had felt as a studentEach file served as a reminder of the excitement and challenges of learning these fundamental concepts. Margin notes, half-solved problems, and ambitious project ideas transported me back to late nights in the computer lab and lively discussions with classmates and professors.

Scrolling through these digital artifacts, I smiled at my younger self’s enthusiasm. Complex diagrams decoding recursive algorithms and meticulously commented code for my first balanced tree implementation all testified to my genuine curiosity and dedication to the subject. This trip down memory lane wasn’t just about nostalgia. It sparked a renewed interest in revisiting these foundational concepts, now armed with years of practical experience. I was eager to bridge the gap between university theory and real-world problems I’ve encountered in my career.

## Rediscovering the Foundations[](https://www.bytedrum.com/posts/bloom-filters/#rediscovering-the-foundations)

Revisiting these fundamental concepts, I’m struck by how my perspective has evolved. Years of practical experience have illuminated how these theoretical building blocks shape the software we use daily.

In this blog series, I’m inviting you along on my journey of rediscovery. Consider me your study buddy, sharing ‘aha!’ moments, real-world applications, and yes, even occasional confusion when tackling tricky concepts.

Here’s what you can expect from this series:

-   Real-world examples that illustrate how these concepts are applied in modern software development
-   Clear explanations that bridge the gap between theory and practice
-   Code samples that you can actually use and adapt for your own projects
-   My thoughts on why these concepts matter in our fast-paced tech industry
-   Diagrams and visualizations to help clarify complex ideas

Let’s set some expectations: this series isn’t a greatest hits album of data structures and algorithms. We won’t deep dive into depth-first search, breadth-first search, binary trees, or hash maps. These are crucial, but well-covered elsewhere. If you need a refresher, check out the resources in the footnote.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-2" id="user-content-fnref-2" data-footnote-ref="" aria-describedby="footnote-label">1</a></sup>

Instead, we’re venturing into the road less traveled — exploring fascinating, useful concepts that often fly under the radar. These unsung heroes of data structures and algorithms don’t always make it into CS syllabi or coding interviews, but they’re vital for building efficient, scalable systems.

First stop: the _Bloom Filter_<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-3" id="user-content-fnref-3" data-footnote-ref="" aria-describedby="footnote-label">2</a></sup> — a data structure so clever, it can tell you with certainty when it doesn’t know something. Intrigued? You should be.

## Enter the Bloom Filter[](https://www.bytedrum.com/posts/bloom-filters/#enter-the-bloom-filter)

I chose this data structure to kick off these series because it beautifully demonstrates how a seemingly simple concept can have profound implications in real-world applications.

![This is exactly what a Bloom Filter is](https://www.bytedrum.com/_astro/bloom-filter.CGhqpKNd_ZxIVDo.webp)

For years, I had a surface-level understanding of Bloom Filters. I knew they were used in various systems for efficient set membership queries, but the details of _how they actually worked_ remained fuzzy. It wasn’t until I encountered a specific problem in one of my projects that I truly appreciated the elegance and power of this data structure. Let’s start by looking at a real-world problem that Bloom Filters are particularly well-suited to solve.

## The Spotify Playlist Dilemma[](https://www.bytedrum.com/posts/bloom-filters/#the-spotify-playlist-dilemma)

Let’s consider a real-world problem that many music streaming services face: efficiently checking if a song is in a user’s playlist. This might seem straightforward at first glance, but when you’re dealing with millions of users, each with multiple playlists, the challenge becomes significant.

![The Spotify Playlist Predicament: A Bloom Filter's Playground](https://www.bytedrum.com/_astro/spotify_problem.O6ZBQJFV_2hXrfz.svg)

Here are the key requirements for this system:

1.  **Speed**: The lookup needs to be incredibly fast. Users expect instant responses when interacting with their playlists. Even a delay of a few hundred milliseconds can negatively impact user experience.
2.  **Memory is precious**: With millions of users, each with multiple playlists, we can’t afford to keep all that data in memory.
3.  **Scalability**: This solution needs to work just as smoothly for the “I only listen to lo-fi beats to study/relax to” user with 10 songs as it does for the music aficionado with 10,000 tracks in their “Bass Testing” playlist.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-4" id="user-content-fnref-4" data-footnote-ref="" aria-describedby="footnote-label">3</a></sup>

Let’s break down some potential approaches and their limitations:

-   **Arrays?** A simple array of song IDs would require a linear search for each lookup, resulting in O(n) time complexity. This becomes prohibitively slow for large playlists.
-   **Hash Tables?** While offering O(1) average-case lookup time, they require storing all song IDs in memory. For millions of users, this would consume an enormous amount of RAM.
-   **Database Queries?**<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-7" id="user-content-fnref-7" data-footnote-ref="" aria-describedby="footnote-label">4</a></sup> Your latency graphs would look like a mountain range, and not the gentle rolling hills kind.

As we consider these options, it becomes clear that we need a solution that offers a better balance between speed and memory usage. We need something that can provide near-instant lookups without requiring us to keep full playlist data in memory for every user. Sounds too good to be true? Well, it sort of it, but it’s _mostly_ just perfect. This is where probabilistic data structures come into play. Specifically, this is a perfect use case for a Bloom Filter. A Bloom Filter can tell us with certainty when a song is not in a playlist, and it can give us a highly probable “yes” when a song is in the playlist, all while using a fraction of the memory that would be required to store the full playlist data.

By using a Bloom Filter, we could:

1.  Quickly determine if a song is definitely not in a playlist without querying the database.
2.  For positive results (which could include false positives), we could then do a more expensive lookup in the actual database to confirm.

This two-step process can significantly reduce unnecessary database queries, improving overall system performance and user experience.

## Bloom Filter 101: The Nuts and Bolts[](https://www.bytedrum.com/posts/bloom-filters/#bloom-filter-101-the-nuts-and-bolts)

Now that we’ve established the challenge of efficient playlist lookups, it’s time to get our hands dirty and understand the innards of a Bloom Filter.

At its heart, a Bloom Filter is a space-efficient probabilistic data structure designed to answer one question: is this element a member of the set? It provides a definitive ‘_no_’ for elements not in the set and a highly probable ‘_yes_’ for those that are.

### Components[](https://www.bytedrum.com/posts/bloom-filters/#components)

At its core, a Bloom Filter is elegantly simple, consisting of just two components:

1.  **A bit array** (also known as a bit vector, or bucket array) of m bits. This is the backbone of our Bloom Filter.
2.  **Hash functions**. This is where the magic happens. We’ll use a set of k hash functions to map our data into the bit array.

These two simple components combine to create a powerful data structure capable of answering the question, “Have I seen this before?”

![Basics of Bloom Filter](https://www.bytedrum.com/_astro/bloom_filter_basic.CNfSrq8t_ZWI3Wl.svg)

### The Algorithm[](https://www.bytedrum.com/posts/bloom-filters/#the-algorithm)

Here’s how a Bloom Filter operates:

1.  **Initialization**: Set all bits in the bit array to 0.
2.  **Adding an element**:
    -   Take your element.
    -   Apply each of the k hash functions to the element.
    -   Each hash function will give you a number. Use these numbers as indices in your bit array and flip those bits to 1
3.  **Checking for an element**:
    -   Take the element you’re looking for.
    -   Apply each of the k hash functions to the element.
    -   Check if all the bits at the indices given by the hash functions are 1. If any of them are 0, the element is **definitely** not in the Bloom Filter. If all of them are 1, the element _might_ be in the Bloom Filter.

### The Magic (and the Catch)[](https://www.bytedrum.com/posts/bloom-filters/#the-magic-and-the-catch)

Bloom Filters have a unique trait: they can tell you with 100% certainty when something is _not_ in the set. However, they can’t be equally certain when something _is_ in the set.

This leads to two key characteristics:

-   **False positives are possible**: The filter might indicate an element is “probably in the set” for something you haven’t actually added. It’s like that friend who always says ‘Yeah, I’ve seen that show’ when you know they haven’t. Sometimes they’re right, but you can never be 100% sure.
-   **False negatives are impossible**: If the filter says an element isn’t there, it definitely isn’t.

False positives occur when the bits corresponding to an element’s hash values are all set to 1 by other elements, even though the element itself was never added to the filter.

This will become obvious in the animation below.

### Action! The Bloom Filter in Motion[](https://www.bytedrum.com/posts/bloom-filters/#action-the-bloom-filter-in-motion)

The best way to understand how Bloom Filters actually work, is to first visualize them. Let’s start with a simple example that I’ve spend an ungodly amount of time making. You’re welcome.

The hash functions in this example are just `(n + 1) * (indexOfHashFunction + 1) % bitArraySize`. For example, for the number 37:

1.  `(37 + 1) * (0 + 1) % 10 = 8`
2.  `(37 + 1) * (1 + 1) % 10 = 6`
3.  `(37 + 1) * (2 + 1) % 10 = 4`

If you’ve added items 25 and 37 to the set, you’ll notice our Bloom Filter will indicate that element 63 is _probably_ in the set. This false positive occurs because the hash outputs for 63 coincide with those of 25 and 37.

### Tuning Your Bloom Filter[](https://www.bytedrum.com/posts/bloom-filters/#tuning-your-bloom-filter)

The performance of a Bloom Filter hinges on three key factors:

1.  **Bit array size (m)**: A larger array generally means fewer false positives, but consumes more memory.
2.  **Number of hash functions (k)**: More hash functions can reduce false positives but increase computation time.
3.  **Expected number of elements (n)**: This helps determine the optimal values for m and k.

Balancing these factors is crucial for optimizing the Bloom Filter’s performance. The goal is to minimize space usage while maintaining acceptable false positive rates and computation speed.

## The Good, the Bad, and the Bloom-y[](https://www.bytedrum.com/posts/bloom-filters/#the-good-the-bad-and-the-bloom-y)

### The Good: Why Bloom Filters Rock[](https://www.bytedrum.com/posts/bloom-filters/#the-good-why-bloom-filters-rock)

1.  **Space Efficiency**: Bloom Filters can represent a large set of elements using only a fraction of the space required by conventional data structures. This makes them ideal for scenarios where memory is at a premium, like in-memory databases or network routers.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-8" id="user-content-fnref-8" data-footnote-ref="" aria-describedby="footnote-label">5</a></sup>
2.  **Constant-time Operations**: Both adding elements and checking for their presence happen in O(k) time, where k is the number of hash functions. This is essentially constant time, as k is typically small and fixed. In practice, this means blazing-fast performance, even as your dataset grows.
3.  **Scalability**: Bloom Filters handle large amounts of data with grace. As your dataset grows, the performance degrades gracefully, maintaining its speed advantages over other data structures.
4.  **No False Negatives**: When a Bloom Filter says an element isn’t in the set, you can bet your last energy drink on it. This property makes Bloom Filters particularly useful in scenarios where you need to quickly rule out possibilities.

### The Bad: Every Rose Has Its Thorn[](https://www.bytedrum.com/posts/bloom-filters/#the-bad-every-rose-has-its-thorn)

1.  **False Positives**: Bloom Filters may incorrectly indicate an element is in the set. While tuning can minimize this, it can’t be entirely eliminated.
2.  **No Deletion**: Once you add an element to a standard Bloom Filter, it’s there to stay. There’s no built-in mechanism for removing elements. There is, however, a way to overcome this limitation, and we’ll get to that in a bit.
3.  **No Count**: Bloom Filters can tell you if an element might be in the set, but they can’t tell you how many times it’s been added. If you need to count occurrences, you’ll need to look elsewhere.
4.  **Tuning Required**: To get the best performance out of a Bloom Filter, you need to tune it based on your expected number of elements and desired false positive rate. This requires some upfront knowledge and calculation.

### The Bloom-y: When to Use (and When Not to Use) Bloom Filters[](https://www.bytedrum.com/posts/bloom-filters/#the-bloom-y-when-to-use-and-when-not-to-use-bloom-filters)

**Use Bloom Filters when:**

-   You need quick set membership checks and can tolerate some false positives.
-   Memory is limited and you’re dealing with large datasets.
-   Probabilistic answers are acceptable for your use case.

**Think twice before using Bloom Filters when:**

-   You absolutely cannot tolerate false positives. If lives depend on the accuracy of your data structure, maybe stick to something more deterministic.
-   You need to store additional data along with your elements. Bloom Filters aren’t key-value stores.
-   You need to remove elements regularly. Unless you’re ready to dive into the world of Counting Bloom Filters, stick to structures that support deletion.

### The Bloom Filter Sweet Spot[](https://www.bytedrum.com/posts/bloom-filters/#the-bloom-filter-sweet-spot)

Consider our Spotify playlist scenario. A Bloom Filter can quickly tell you if a song is definitely not in the playlist. For the “_maybes_,” you can then do a more expensive lookup in the actual database.

In essence, Bloom Filters shine in scenarios where you need to rapidly exclude impossibilities before proceeding with more resource-intensive operations. They efficiently eliminate definite non-matches before allowing potential matches to proceed to more thorough checks.

## Dispelling Bloom Filter Myths: My Journey from Misconception to Understanding[](https://www.bytedrum.com/posts/bloom-filters/#dispelling-bloom-filter-myths-my-journey-from-misconception-to-understanding)

Like any new concept, Bloom Filters come with their share of misunderstandings. I’ll admit, I had a few of my own. Let’s clear up some common myths and set the record straight.

### Myth 1: “Bloom Filters are just a less accurate hash table”[](https://www.bytedrum.com/posts/bloom-filters/#myth-1-bloom-filters-are-just-a-less-accurate-hash-table)

When I first encountered Bloom Filters, I thought, “Why not just use a hash table? It’s accurate and still pretty fast.” This misconception stems from not fully appreciating the space efficiency of Bloom Filters.

**Reality**: Hash tables offer perfect accuracy but at the cost of storing each element or its full hash. Bloom Filters, however, represent a set using only a few bits per element, regardless of size. This space efficiency is a game-changer for massive datasets or constrained environments.

### Myth 2: “False positives make Bloom Filters unreliable”[](https://www.bytedrum.com/posts/bloom-filters/#myth-2-false-positives-make-bloom-filters-unreliable)

I initially worried that false positives would render Bloom Filters useless in practical applications. After all, how can you trust a data structure that sometimes lies?

**Reality**: Bloom Filters’ genius lies in their probabilistic nature. The occasional false positive is often an acceptable trade-off for massive gains in space efficiency and speed. Plus:

-   The false positive rate can be tuned to suit specific needs.
-   In critical systems, Bloom Filters serve as a first-pass filter, with positive results verified against a more authoritative (but slower) data source.

### Myth 3: “Bloom Filters can’t handle dynamic data sets”[](https://www.bytedrum.com/posts/bloom-filters/#myth-3-bloom-filters-cant-handle-dynamic-data-sets)

I once thought Bloom Filters were only suitable for static data sets, believing that once you’ve added all your elements, you’re stuck with that filter forever.

**Reality**: Standard Bloom Filters don’t support deletion, but variants like Counting Bloom Filters and Cuckoo Filters do. Even with standard Bloom Filters, techniques exist for handling growing datasets:

-   Using multiple filters
-   Periodically rebuilding the filter

### Myth 4: “Bloom Filters are only useful for huge data sets”[](https://www.bytedrum.com/posts/bloom-filters/#myth-4-bloom-filters-are-only-useful-for-huge-data-sets)

I used to think Bloom Filters were overkill for anything but massive, Google-scale data sets. Why bother with probabilistic data structures for smaller problems?

**Reality**: While Bloom Filters excel with large datasets, they’re valuable even for moderate-sized collections. They’re particularly useful in:

-   Memory-constrained environments like embedded systems
-   Scenarios where minimizing network traffic is crucial Their space savings and quick lookups benefit a wide range of applications.

### Myth 5: “Implementing a Bloom Filter is complex and error-prone”[](https://www.bytedrum.com/posts/bloom-filters/#myth-5-implementing-a-bloom-filter-is-complex-and-error-prone)

Since it’s a _probabilistic_ data structure, I thought implementing one correctly would be a daunting task. I imagined it required a deep understanding of probability theory and a lot of careful coding.

**Reality**: Basic Bloom Filters are surprisingly simple to implement. With just a bit array and a few hash functions, you can create a functional Bloom Filter in a few dozen lines of code. While optimizing and fine-tuning a Bloom Filter for production use requires more expertise, getting started with them is quite accessible to any programmer.

## DIY Bloom: Implementing Your Own Bloom Filter[](https://www.bytedrum.com/posts/bloom-filters/#diy-bloom-implementing-your-own-bloom-filter)

Code is worth more than a thousand words, so let’s write some actual code. I’ll use Python for this example, but the concepts apply to any language and the implementation is straightforward. I will use only Python’s standard library for simplicity.

```
<span><span>import</span><span> math</span></span>
<span><span>from</span><span> hashlib </span><span>import</span><span> md5</span></span>
<span></span>
<span></span>
<span><span>def</span><span> _hash</span><span>(</span><span>item</span><span>,</span><span> seed</span><span>)</span><span>:</span></span>
<span><span>    hash_result </span><span>=</span><span> md5</span><span>(</span><span>item.</span><span>encode</span><span>(</span><span>'</span><span>utf-8</span><span>'</span><span>))</span></span>
<span><span>    hash_result</span><span>.</span><span>update</span><span>(</span><span>str</span><span>(</span><span>seed</span><span>)</span><span>.</span><span>encode</span><span>(</span><span>'</span><span>utf-8</span><span>'</span><span>))</span></span>
<span><span>    return</span><span> int</span><span>(</span><span>hash_result.</span><span>hexdigest</span><span>()</span><span>,</span><span> 16</span><span>)</span></span>
<span></span>
<span></span>
<span><span>class</span><span> BloomFilter</span><span>:</span></span>
<span><span>    def</span><span> __init__</span><span>(</span><span>self</span><span>,</span><span> size</span><span>,</span><span> hash_count</span><span>)</span><span>:</span></span>
<span><span>        self</span><span>.</span><span>size </span><span>=</span><span> size</span></span>
<span><span>        self</span><span>.</span><span>hash_count </span><span>=</span><span> hash_count</span></span>
<span><span>        self</span><span>.</span><span>bit_array </span><span>=</span><span> [</span><span>False</span><span>]</span><span> *</span><span> size</span></span>
<span></span>
<span><span>    def</span><span> add</span><span>(</span><span>self</span><span>,</span><span> item</span><span>)</span><span>:</span></span>
<span><span>        for</span><span> i </span><span>in</span><span> range</span><span>(</span><span>self</span><span>.hash_count</span><span>):</span></span>
<span><span>            index </span><span>=</span><span> _hash</span><span>(</span><span>item</span><span>,</span><span> i</span><span>)</span><span> %</span><span> self</span><span>.</span><span>size</span></span>
<span><span>            self</span><span>.</span><span>bit_array</span><span>[</span><span>index</span><span>]</span><span> =</span><span> True</span></span>
<span></span>
<span><span>    def</span><span> check</span><span>(</span><span>self</span><span>,</span><span> item</span><span>)</span><span>:</span></span>
<span><span>        for</span><span> i </span><span>in</span><span> range</span><span>(</span><span>self</span><span>.hash_count</span><span>):</span></span>
<span><span>            index </span><span>=</span><span> _hash</span><span>(</span><span>item</span><span>,</span><span> i</span><span>)</span><span> %</span><span> self</span><span>.</span><span>size</span></span>
<span><span>            if</span><span> not</span><span> self</span><span>.</span><span>bit_array</span><span>[</span><span>index</span><span>]:</span></span>
<span><span>                return</span><span> False</span></span>
<span><span>        return</span><span> True</span></span>
<span></span>
<span></span>
<span><span>bloom </span><span>=</span><span> BloomFilter</span><span>(</span><span>size</span><span>=</span><span>100</span><span>,</span><span> hash_count</span><span>=</span><span>3</span><span>)</span></span>
<span><span>bloom</span><span>.</span><span>add</span><span>(</span><span>"</span><span>hello</span><span>"</span><span>)</span></span>
<span><span>bloom</span><span>.</span><span>add</span><span>(</span><span>"</span><span>world</span><span>"</span><span>)</span></span>
<span></span>
<span><span>print</span><span>(</span><span>bloom.</span><span>check</span><span>(</span><span>"</span><span>hello</span><span>"</span><span>))</span><span>  #</span><span> True</span></span>
<span><span>print</span><span>(</span><span>bloom.</span><span>check</span><span>(</span><span>"</span><span>world</span><span>"</span><span>))</span><span>  #</span><span> True</span></span>
<span><span>print</span><span>(</span><span>bloom.</span><span>check</span><span>(</span><span>"</span><span>recursion</span><span>"</span><span>))</span><span>  #</span><span> False</span></span>
<span></span>
```

### Breaking Down the Implementation[](https://www.bytedrum.com/posts/bloom-filters/#breaking-down-the-implementation)

-   We’re only using `math` for some calculations and `hashlib` for our MD5 hash function.
-   **`BloomFilter`**: This is where the magic happens. We initialize our filter with a size and the number of hash functions we want to use. Notice we’re using a simple list of booleans for our bit array.
-   **`_hash`**: This is our custom hash function using MD5. It’s not the most efficient, but it gets the job done for demonstration purposes. We use the seed to create multiple hash functions from one.
-   **`add`**: The `add` method inserts items into our filter. By using our `_hash` function with different seeds, we simulate multiple hash functions, a key aspect of Bloom Filters.
-   **`check`**: The `check` method determines if an item might be in the set. A `True` result indicates a possible match (subject to false positives), while `False` definitively means the item is not in the set.

### The Art of Tuning[](https://www.bytedrum.com/posts/bloom-filters/#the-art-of-tuning)

Optimizing a Bloom Filter involves balancing space efficiency and false positive rate by adjusting its size and number of hash functions. The key parameters we need to consider are:

1.  $m$: the size of the bit array
2.  $n$: the number of items expected to be stored
3.  $k$: the number of hash functions
4.  $p$: the desired false positive probability

The relationship between these parameters is governed by the following formulas<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-1" id="user-content-fnref-1" data-footnote-ref="" aria-describedby="footnote-label">6</a></sup>:

1.  **Bit Array Size ($m$)**: The formula $m = -\frac{n \ln p}{(\ln 2)^2}$ shows that the size of the bit array grows linearly with the number of items ($n$) and logarithmically with the inverse of the false positive rate ($p$). This means that to significantly decrease the false positive rate, you need to increase the size of the bit array substantially.
    
2.  **Number of Hash Functions ($k$)**: The formula $k = \frac{m}{n} \ln 2$ demonstrates that the optimal number of hash functions is proportional to the ratio of the bit array size to the number of items. This makes intuitive sense: a larger bit array can accommodate more hash functions without excessive collisions.
    

It’s important to note some nuances:

-   These formulas assume that the hash functions are independent and uniformly distributed, which may not always be the case in practice.
-   The actual false positive rate may deviate from the theoretical prediction due to various factors like the quality of hash functions and the specific data being stored.
-   There’s a trade-off between space efficiency and computational efficiency. While more hash functions can reduce the false positive rate, they also increase the time needed for insertions and queries.
-   In practice, it’s often beneficial to round $k$ to the nearest integer, as fractional hash functions aren’t practical to implement.

When implementing a Bloom Filter, you typically start with your desired false positive rate ($p$) and expected number of items ($n$), then use these formulas to calculate the optimal $m$ and $k$. However, remember that these are theoretical optimums. In real-world scenarios, you may need to adjust based on practical constraints like memory availability or computational resources.

### Tips and Tricks from the Trenches[](https://www.bytedrum.com/posts/bloom-filters/#tips-and-tricks-from-the-trenches)

1.  **Hash Function Choice**: In this example, we’re using MD5 for simplicity. In a production environment, you might want something faster, like [MurmurHash](https://pypi.org/project/mmh3/) or [xxHash](https://pypi.org/project/xxhash/) for their speed and distribution quality.
2.  **Bit Array Implementation**: We’re using a list of booleans here, which is memory-inefficient. In a real-world scenario, you’d probably use a [bit array](https://pypi.org/project/bitarray/) or a specialized library.
3.  **False Positive Rate**: Decreasing the false positive rate means increasing the size of your bit array. If you’re trying to decrease the false positive rate too much, your bit array will get huge. It’s all about finding that sweet spot between accuracy and efficiency.
4.  **Testing**: Create diverse test sets including both present and absent elements. Test edge cases like empty filters, filters at capacity, and elements that hash to similar positions. Systematically verify both true positives and true negatives, and measure your actual false positive rate against the theoretical expectation.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-testing-bloom-filters" id="user-content-fnref-testing-bloom-filters" data-footnote-ref="" aria-describedby="footnote-label">7</a></sup>
5.  **Scalability**: Always plan for growth. If your data set size might increase significantly, consider implementing a Scalable Bloom Filter that can grow dynamically.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-15" id="user-content-fnref-15" data-footnote-ref="" aria-describedby="footnote-label">8</a></sup>

### The Nuanced Reality[](https://www.bytedrum.com/posts/bloom-filters/#the-nuanced-reality)

While Bloom Filters are powerful, they’re not a silver bullet. In some cases, the space savings might not justify the added complexity and potential for false positives. In many cases a simple hash set would suffice, even though the allure of the Bloom Filter can sometimes be too strong to resist.

Moreover, in distributed systems, Bloom Filters can sometimes lead to inconsistencies. If different nodes have slightly different filters, you might end up with confusing situations where an item appears to exist on one node but not another.

Lastly, remember that Bloom Filters are probabilistic. In critical systems where false positives could lead to serious issues, you might need to pair your Bloom Filter with a secondary, deterministic check.

Implementing your own Bloom Filter is a great way to understand its inner workings. But in a production environment, consider using well-tested libraries like [rbloom](https://github.com/KenanHanke/rbloom) or [pybloomfiltermmap3](https://github.com/prashnts/pybloomfiltermmap3) for Python. They’ve likely encountered and solved edge cases that you haven’t even thought of yet.

## Real-World Applications: Bloom Filters in the Wild[](https://www.bytedrum.com/posts/bloom-filters/#real-world-applications-bloom-filters-in-the-wild)

Now that we’ve dissected the Bloom Filter and understood its inner workings, let’s explore how these probabilistic powerhouses are being used in the wild. Spoiler alert: they’re everywhere, quietly making your digital life smoother without you even realizing it.

### 1\. Web Browsers[](https://www.bytedrum.com/posts/bloom-filters/#1-web-browsers)

Remember the last time you visited a sketchy website and your browser threw up a big red warning? You can thank Bloom Filters for that quick save. From the late 2000s to the mid-2010s, major browsers like Google Chrome and Mozilla Firefox extensively used Bloom Filters for their Safe Browsing feature. Here’s how it worked:

1.  Google maintained a master list of malicious URLs, which was massive and updated frequently.
2.  This list was compressed into a Bloom Filter, typically around 2MB in size.
3.  Browsers downloaded this compact Bloom Filter periodically (usually every 30 minutes).
4.  When you typed in a URL, the browser would check it against this local Bloom Filter.
5.  If the filter returned a “maybe,” the browser would then make a full query to Google’s servers for verification.

![Bloom Filters in Web Browsers](https://www.bytedrum.com/_astro/web_browsers.BnmYtvNy_ZrrUYa.svg)

This approach allowed browsers to protect users without storing a massive database of bad URLs on the device or sending every visited URL to Google’s servers. It was a clever balance between privacy, efficiency, and security.

As web grew more complex and security threats evolved, Bloom Filters were slowly phased out and replaced by [more sophisticated systems](https://cloud.google.com/web-risk/docs/update-api).<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-9" id="user-content-fnref-9" data-footnote-ref="" aria-describedby="footnote-label">9</a></sup>

### 2\. Databases[](https://www.bytedrum.com/posts/bloom-filters/#2-databases)

In large-scale databases, especially those dealing with big data, query performance is crucial. One common scenario where Bloom Filters shine is in optimizing join operations, particularly in column-oriented databases like Apache Cassandra or Apache HBase.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-10" id="user-content-fnref-10" data-footnote-ref="" aria-describedby="footnote-label">10</a></sup>

Let’s consider a specific example using a simplified e-commerce database:

Imagine you have two tables:

1.  `orders` (millions of rows)
2.  `customers` (hundreds of thousands of rows)

You want to find all orders for customers in a specific city. A naive approach might join these entire tables, which would be extremely costly.

Here’s how a Bloom Filter optimizes this:

1.  **Filter Creation:**
    
    -   The database creates a Bloom Filter for the `city` column in the `customers` table.
    -   This filter represents all unique cities in the `customers` table.
2.  **Query Execution:**
    
    -   When you run a query like “Find all orders for customers in [Vilnius](https://vilniusgspot.com/),” the database first checks the Bloom Filter.
    -   If the filter says “Vilnius” is definitely not in the `customers` table, the query can stop immediately, saving a costly join operation.
3.  **Join Optimization:**
    
    -   If “Vilnius” might be in the `customers` table, the database proceeds with the join.
    -   However, it first applies the Bloom Filter to the `orders` table, quickly eliminating orders associated with customer IDs that definitely don’t have “Vilnius” as their city.
4.  **Performance Impact:**
    
    -   This can reduce the number of rows involved in the join operation by orders of magnitude.
    -   For example, if only 1% of customers are from Vilnius, this approach could potentially eliminate 99% of the orders from consideration before the expensive join operation.

In practice, this might look like:

```
<span><span>SELECT</span><span> o</span><span>.</span><span>order_id</span><span>, </span><span>o</span><span>.</span><span>amount</span></span>
<span><span>FROM</span><span> orders o</span></span>
<span><span>JOIN</span><span> customers c </span><span>ON</span><span> o</span><span>.</span><span>customer_id</span><span> =</span><span> c</span><span>.</span><span>id</span></span>
<span><span>WHERE</span><span> c</span><span>.</span><span>city</span><span> =</span><span> '</span><span>Vilnius</span><span>'</span></span>
<span></span>
```

Without a Bloom Filter, this query might scan millions of orders. With a Bloom Filter, it might only need to consider thousands, dramatically reducing I/O operations and computation time.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-11" id="user-content-fnref-11" data-footnote-ref="" aria-describedby="footnote-label">11</a></sup>

![Bloom Filters in Database Joins](https://www.bytedrum.com/_astro/database_join.DeZBLhMP_20Uyiq.svg)

The beauty of this approach is its scalability. As your `orders` table grows from millions to billions of rows, the Bloom Filter’s effectiveness in reducing unnecessary data processing grows proportionally, keeping your queries snappy even as your data expands.

### 3\. Networking[](https://www.bytedrum.com/posts/bloom-filters/#3-networking)

In content-centric networking, the focus shifts from where data is stored to what the data is. This paradigm is particularly useful for distributing popular content efficiently across a network. Bloom Filters play a crucial role in making this approach scalable and efficient.

Let’s consider a specific example: a large-scale content delivery network (CDN) for streaming video.

1.  **Content Representation:** Each router in the network maintains a Bloom Filter for each of its interfaces (network connections). These filters represent the content available through that interface.
    
2.  **Filter Population:** When a piece of content (let’s say, the latest episode of “Succession”) becomes available through a particular path, all routers along that path add the content’s identifier to their respective Bloom Filters for the appropriate interface.
    
3.  **Routing Process:** When a user requests the episode, here’s what happens:
    
    -   The request reaches the nearest router.
    -   The router checks its Bloom Filters for each interface.
    -   If a filter indicates the content might be available through a particular interface, the request is forwarded that way.
    -   This process repeats at each router until the content is found.
4.  **Efficiency Gains:**
    
    -   Routers don’t need to store full content directories, which would be massive for a large CDN.
    -   Routing decisions can be made quickly by checking the compact Bloom Filters.
    -   False positives might occasionally send a request in the wrong direction, but this is generally preferable to the alternative of storing complete content maps at each router.
5.  **Adaptive Behavior:** As content popularity changes, the network can adapt:
    
    -   Popular content will be represented in more Bloom Filters across the network.
    -   Less popular content might only be represented in filters closer to its source.

![Bloom Filters in CDNs](https://www.bytedrum.com/_astro/packet_routing.DrF34GDn_ZxVUM.svg)

This approach allows the CDN to efficiently route content requests without maintaining enormous routing tables at each node. It’s particularly effective for handling flash crowds, where a piece of content suddenly becomes very popular (imagine a new “Baby Yoda” scene going viral).

The trade-off is the possibility of occasional misdirected requests due to false positives, but the overall gain in efficiency and scalability makes this a worthwhile compromise for large-scale content delivery systems.

### 4\. Spell Checkers[](https://www.bytedrum.com/posts/bloom-filters/#4-spell-checkers)

Spell checkers are an everyday application of Bloom Filters that most of us interact with without realizing it.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-12" id="user-content-fnref-12" data-footnote-ref="" aria-describedby="footnote-label">12</a></sup> They’re the silent guardians of our digital writing, catching our typos before they make it into that important email or blog post. Because let’s be honest, without spell checkers, half of us would still be arguing whether it’s ‘definately’ or ‘definatly’. (Spoiler: It’s neither.)

Let’s break down how a Bloom Filter-based spell checker might work:

1.  **Dictionary Loading**: First, the spell checker loads a dictionary of correctly spelled words into a Bloom Filter. For English, this might be around 170,000 words.
    
2.  **Filter Creation**: The Bloom Filter is created with, say, 1,000,000 bits and 5 hash functions. These parameters are chosen to balance memory usage and false positive rate.
    
3.  **Word Checking**: As you type “definately” in your document:
    
    -   The spell checker applies the 5 hash functions to “definately”.
    -   It checks the corresponding bits in the Bloom Filter.
    -   Since at least one of these bits is 0, the filter confidently says, “This word is not in the dictionary.”
    -   Your spell checker underlines “definately” in red.
4.  **Handling False Positives**: You then type “algorithmically”. The Bloom Filter might say this word is probably in the dictionary (even if it isn’t), due to false positives. In this case, the spell checker doesn’t flag it.
    
5.  **Performance**: This process happens in milliseconds, allowing real-time spell checking even in large documents.
    
6.  **Memory Efficiency**: Instead of storing the entire dictionary (which could be megabytes), the Bloom Filter might use only about 125 KB of memory (1,000,000 bits).
    

This approach allows spell checkers to work quickly and efficiently, even with large dictionaries, without consuming too much memory. It’s like having a very fast, slightly fallible editor looking over your shoulder, catching most of your typos without slowing down your writing flow or hogging your computer’s resources.

The trade-off? Occasionally, a misspelled word might slip through due to a false positive. But for most users, the benefits of speed and memory efficiency far outweigh the rare uncaught typo.

![Bloom Filters in Spell Chekers](https://www.bytedrum.com/_astro/spell_checker.BEbQHZ1I_15kW6z.svg)

### 5.Cache Filtering[](https://www.bytedrum.com/posts/bloom-filters/#5cache-filtering)

Many systems use Bloom Filters to optimize caching strategies, particularly in distributed systems where reducing network calls is crucial. Let’s consider a specific example: a distributed caching system for a social media platform.

Imagine you’re building the backend for “FriendZone,” a totally-not-Facebook social network. You have user data spread across multiple servers, with a caching layer to speed up frequent requests.

Here’s how you might implement Bloom Filter-based cache filtering:

1.  For each cache server, maintain a Bloom Filter representing the user IDs present in that cache.
2.  When a request comes in for a user’s profile:
    -   Check the Bloom Filters of all cache servers first.
    -   If all filters return “definitely not here,” go straight to the database.
    -   If any filter returns “maybe here,” check that specific cache server.

Let’s put some numbers to it:

-   You have 10 cache servers, each holding about 1 million user profiles.
    
-   A typical request flow without Bloom Filters:
    
    1.  Check Cache Server 1 (miss)
    2.  Check Cache Server 2 (miss)
    3.  …
    4.  Check Cache Server 10 (miss)
    5.  Finally, query the database
-   With Bloom Filters:
    
    1.  Check all 10 Bloom Filters (fast, in-memory operation)
    2.  If all say “no,” go straight to database
    3.  If one says “maybe,” check only that cache server

In the worst case (when the data is actually in the last cache server), you’ve added one extra step. But in the common case where the data isn’t cached, you’ve replaced 10 cache checks with 10 quick Bloom Filter checks plus one database query.

The result? Significantly reduced latency for cache misses, less load on your cache servers, and happier FriendZone users who can stalk their exes’ profiles just a little bit faster.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-13" id="user-content-fnref-13" data-footnote-ref="" aria-describedby="footnote-label">13</a></sup>

![Bloom Filters in Cache Lookup Optimization](https://www.bytedrum.com/_astro/cache_filtering.CF9hEPyv_yWHJf.svg)

This approach is particularly powerful in large-scale systems where the cost of network calls between servers is high. By using Bloom Filters, you’re essentially creating a quick, memory-efficient index of your entire distributed cache, allowing you to make intelligent decisions about where to look for data.

### 6\. URL Shortener with Existence Checking[](https://www.bytedrum.com/posts/bloom-filters/#6-url-shortener-with-existence-checking)

Imagine you’re building the next big URL shortener, aiming to give TinyURL a run for its money. You need to check if a short code already exists before assigning it. Typically, this involves a database query for each potential code. A Bloom Filter could quickly check if a short code might already exist, reducing database queries significantly.

```
<span><span>import</span><span> random</span></span>
<span><span>import</span><span> string</span></span>
<span></span>
<span><span>class</span><span> URLShortener</span><span>:</span></span>
<span><span>    def</span><span> __init__</span><span>(</span><span>self</span><span>,</span><span> expected_urls</span><span>,</span><span> false_positive_rate</span><span>)</span><span>:</span></span>
<span><span>        self</span><span>.</span><span>used_codes </span><span>=</span><span> BloomFilter</span><span>(</span><span>expected_urls</span><span>,</span><span> false_positive_rate</span><span>)</span></span>
<span><span>        self</span><span>.</span><span>url_database </span><span>=</span><span> {}</span></span>
<span></span>
<span><span>    def</span><span> generate_short_code</span><span>(</span><span>self</span><span>)</span><span>:</span></span>
<span><span>        while</span><span> True</span><span>:</span></span>
<span><span>            code </span><span>=</span><span> ''</span><span>.</span><span>join</span><span>(</span><span>random.</span><span>choices</span><span>(</span><span>string.ascii_letters </span><span>+</span><span> string.digits</span><span>,</span><span> k</span><span>=</span><span>6</span><span>))</span></span>
<span><span>            if</span><span> code </span><span>not</span><span> in</span><span> self</span><span>.</span><span>used_codes</span><span>:</span></span>
<span><span>                return</span><span> code</span></span>
<span></span>
<span><span>    def</span><span> shorten_url</span><span>(</span><span>self</span><span>,</span><span> long_url</span><span>)</span><span>:</span></span>
<span><span>        short_code </span><span>=</span><span> self</span><span>.</span><span>generate_short_code</span><span>()</span></span>
<span><span>        self</span><span>.</span><span>url_database</span><span>[</span><span>short_code</span><span>]</span><span> =</span><span> long_url</span></span>
<span><span>        self</span><span>.</span><span>used_codes</span><span>.</span><span>add</span><span>(</span><span>short_code</span><span>)</span></span>
<span><span>        return</span><span> f</span><span>"https://short.url/</span><span>{</span><span>short_code</span><span>}</span><span>"</span></span>
<span></span>
<span><span>shortener </span><span>=</span><span> URLShortener</span><span>(</span><span>expected_urls</span><span>=</span><span>1000000</span><span>,</span><span> false_positive_rate</span><span>=</span><span>0.01</span><span>)</span></span>
<span><span>print</span><span>(</span><span>shortener.</span><span>shorten_url</span><span>(</span><span>"</span><span>https://www.example.com/very/long/url</span><span>"</span><span>))</span></span>
<span></span>
```

This approach significantly reduces database lookups, especially as your service grows. However, it’s worth noting that you’ll still need occasional database checks to handle false positives.

The idea of using Bloom Filters in URL shorteners was popularized by bit.ly in the early 2010s<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-bitly" id="user-content-fnref-bitly" data-footnote-ref="" aria-describedby="footnote-label">14</a></sup>. They used Bloom Filters to quickly check if a randomly generated short code had been used before, significantly reducing database lookups. It’s fascinating to see how a simple probabilistic data structure can solve real-world scalability issues in such elegant ways.

### 7\. Efficient Caching of API Responses[](https://www.bytedrum.com/posts/bloom-filters/#7-efficient-caching-of-api-responses)

In the world microservices, where API calls fly back and forth like a game of digital ping pong, we often need to grapple with the challenge of efficient caching. Bloom Filters can help you decide whether to check your cache or go straight to the source, potentially saving valuable time and resources.

```
<span><span>from</span><span> functools </span><span>import</span><span> wraps</span></span>
<span></span>
<span><span>class</span><span> APICache</span><span>:</span></span>
<span><span>    def</span><span> __init__</span><span>(</span><span>self</span><span>,</span><span> expected_items</span><span>,</span><span> false_positive_rate</span><span>)</span><span>:</span></span>
<span><span>        self</span><span>.</span><span>cache_filter </span><span>=</span><span> BloomFilter</span><span>(</span><span>expected_items</span><span>,</span><span> false_positive_rate</span><span>)</span></span>
<span><span>        self</span><span>.</span><span>cache </span><span>=</span><span> {}</span></span>
<span></span>
<span><span>    def</span><span> cache_response</span><span>(</span><span>self</span><span>,</span><span> func</span><span>)</span><span>:</span></span>
<span><span>        @</span><span>wraps</span><span>(</span><span>func</span><span>)</span></span>
<span><span>        def</span><span> wrapper</span><span>(</span><span>*</span><span>args</span><span>,</span><span> **</span><span>kwargs</span><span>)</span><span>:</span></span>
<span><span>            cache_key </span><span>=</span><span> hash</span><span>(</span><span>str</span><span>(</span><span>args</span><span>)</span><span> +</span><span> str</span><span>(</span><span>kwargs</span><span>))</span></span>
<span></span>
<span><span>            if</span><span> cache_key </span><span>not</span><span> in</span><span> self</span><span>.</span><span>cache_filter</span><span>:</span></span>
<span><span>                #</span><span> Definitely not in cache, call the function</span></span>
<span><span>                result </span><span>=</span><span> func</span><span>(</span><span>*</span><span>args</span><span>,</span><span> **</span><span>kwargs</span><span>)</span></span>
<span><span>                self</span><span>.</span><span>cache</span><span>[</span><span>cache_key</span><span>]</span><span> =</span><span> result</span></span>
<span><span>                self</span><span>.</span><span>cache_filter</span><span>.</span><span>add</span><span>(</span><span>cache_key</span><span>)</span></span>
<span><span>                return</span><span> result</span></span>
<span></span>
<span><span>            #</span><span> Might be in cache, check actual cache</span></span>
<span><span>            if</span><span> cache_key </span><span>in</span><span> self</span><span>.</span><span>cache</span><span>:</span></span>
<span><span>                print</span><span>(</span><span>"</span><span>Cache hit!</span><span>"</span><span>)</span></span>
<span><span>                return</span><span> self</span><span>.</span><span>cache</span><span>[</span><span>cache_key</span><span>]</span></span>
<span></span>
<span><span>            #</span><span> False positive, call the function</span></span>
<span><span>            result </span><span>=</span><span> func</span><span>(</span><span>*</span><span>args</span><span>,</span><span> **</span><span>kwargs</span><span>)</span></span>
<span><span>            self</span><span>.</span><span>cache</span><span>[</span><span>cache_key</span><span>]</span><span> =</span><span> result</span></span>
<span><span>            return</span><span> result</span></span>
<span></span>
<span><span>        return</span><span> wrapper</span></span>
<span></span>
<span><span>api_cache </span><span>=</span><span> APICache</span><span>(</span><span>expected_items</span><span>=</span><span>1000000</span><span>,</span><span> false_positive_rate</span><span>=</span><span>0.01</span><span>)</span></span>
<span></span>
<span><span>@</span><span>api_cache</span><span>.</span><span>cache_response</span></span>
<span><span>def</span><span> get_user_profile</span><span>(</span><span>user_id</span><span>)</span><span>:</span></span>
<span><span>    #</span><span> Simulate expensive API call</span></span>
<span><span>    return</span><span> f</span><span>"Profile data for user </span><span>{</span><span>user_id</span><span>}</span><span>"</span></span>
<span></span>
<span><span>#</span><span> First call, not cached</span></span>
<span><span>print</span><span>(</span><span>get_user_profile</span><span>(</span><span>42</span><span>))</span></span>
<span></span>
<span><span>#</span><span> Second call, cached</span></span>
<span><span>print</span><span>(</span><span>get_user_profile</span><span>(</span><span>42</span><span>))</span></span>
<span></span>
```

This approach can significantly reduce unnecessary cache checks, especially in distributed systems where cache lookups might involve network calls. However, be aware that the effectiveness of this method depends on your cache hit rate and the cost of cache lookups versus function calls.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-api-caching" id="user-content-fnref-api-caching" data-footnote-ref="" aria-describedby="footnote-label">15</a></sup>

### 7\. Duplicate Detection in Data Processing Pipelines[](https://www.bytedrum.com/posts/bloom-filters/#7-duplicate-detection-in-data-processing-pipelines)

One area where Bloom Filters are particularly useful is in building data pipelines for processing large volumes of events. Imagine you’re building a data pipeline for a streaming service, processing millions of user interactions daily. You need to quickly identify and filter out duplicate events without breaking the bank on memory usage.

```
<span><span>class</span><span> EventProcessor</span><span>:</span></span>
<span><span>    def</span><span> __init__</span><span>(</span><span>self</span><span>,</span><span> expected_events</span><span>,</span><span> false_positive_rate</span><span>)</span><span>:</span></span>
<span><span>        self</span><span>.</span><span>processed_events </span><span>=</span><span> BloomFilter</span><span>(</span><span>expected_events</span><span>,</span><span> false_positive_rate</span><span>)</span></span>
<span></span>
<span><span>    def</span><span> process_event</span><span>(</span><span>self</span><span>,</span><span> event_id</span><span>)</span><span>:</span></span>
<span><span>        if</span><span> event_id </span><span>in</span><span> self</span><span>.</span><span>processed_events</span><span>:</span></span>
<span><span>            print</span><span>(</span><span>f</span><span>"Event </span><span>{</span><span>event_id</span><span>}</span><span> likely duplicate, skipping."</span><span>)</span></span>
<span><span>            return</span></span>
<span></span>
<span><span>        #</span><span> Process the event here</span></span>
<span><span>        print</span><span>(</span><span>f</span><span>"Processing event: </span><span>{</span><span>event_id</span><span>}</span><span>"</span><span>)</span></span>
<span></span>
<span><span>        #</span><span> Add to Bloom Filter after processing</span></span>
<span><span>        self</span><span>.</span><span>processed_events</span><span>.</span><span>add</span><span>(</span><span>event_id</span><span>)</span></span>
<span></span>
<span><span>processor </span><span>=</span><span> EventProcessor</span><span>(</span><span>expected_events</span><span>=</span><span>1000000</span><span>,</span><span> false_positive_rate</span><span>=</span><span>0.01</span><span>)</span></span>
<span><span>for</span><span> event </span><span>in</span><span> [</span><span>'</span><span>play_song_123</span><span>'</span><span>,</span><span> '</span><span>skip_ad_456</span><span>'</span><span>,</span><span> '</span><span>play_song_123</span><span>'</span><span>,</span><span> '</span><span>like_song_789</span><span>'</span><span>]</span><span>:</span></span>
<span><span>    processor</span><span>.</span><span>process_event</span><span>(</span><span>event</span><span>)</span></span>
<span></span>
```

This approach can dramatically reduce the memory footprint compared to storing all processed event IDs, while maintaining the inherent trade-off of potential false positives. Depending on your use case, you might need to implement a secondary check for potential duplicates. Some systems<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-spark-bloom-filters" id="user-content-fnref-spark-bloom-filters" data-footnote-ref="" aria-describedby="footnote-label">16</a></sup> use a combination of Bloom Filters and time-based partitioning for more effective event deduplication. By using separate Bloom Filters for different time windows, you can maintain a longer deduplication history without increasing the false positive rate.<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-time-based-deduplication" id="user-content-fnref-time-based-deduplication" data-footnote-ref="" aria-describedby="footnote-label">17</a></sup>

### The Bloom Filter Philosophy[](https://www.bytedrum.com/posts/bloom-filters/#the-bloom-filter-philosophy)

What all these applications have in common is a philosophy: it’s often better to be fast and mostly right than slow and always right. Bloom Filters embody the idea that in many real-world scenarios, a quick “_probably_” or “_definitely not_” is more valuable than a slow “_definitely_.”

___

As I’ve leaned more of these applications, I couldn’t help but feel a bit like a kid in a candy store. Each example sparked ideas for how I could potentially use Bloom Filters in my own projects. The elegance of trading a small probability of false positives for significant gains in speed and memory efficiency is just… _chef’s kiss_. I found myself looking at every data problem with my Bloom-tinted glasses, wondering, “Could a Bloom Filter help here?” It’s like when you learn a new word and suddenly start hearing it everywhere. Except in this case, I was seeing potential Bloom Filter applications in every nook and cranny of my codebase.

After familiarising yourself with these applications, I hope you’re feeling a bit like me. You’re not just knowledgeable about Bloom Filters, you’re enthusiastic about finding the perfect use case for this elegant data structure and finally being able to say “It’s ~Morbin~ Bloomin’ time!”

While our friend the Bloom Filter has been hogging the spotlight, it’s got some pretty impressive relatives. These cousins have taken the basic idea of the Bloom Filter and evolved it in various ways to address specific needs or overcome certain limitations. I will only cover the most popular ones, but for the interested, I recommend checking out the [Extensions and applications](https://en.wikipedia.org/wiki/Bloom_filter#Extensions_and_applications) section on the trusty Wikipedia.

### 1\. Counting Bloom Filter: Because Sometimes You Need to Count Your Chickens[](https://www.bytedrum.com/posts/bloom-filters/#1-counting-bloom-filter-because-sometimes-you-need-to-count-your-chickens)

Remember how I said you can’t remove elements from a standard Bloom Filter? Well, the Counting Bloom Filter said “hold my data structure” and solved that problem.

**How it works:**

-   Instead of using a bit array, it uses an array of small counters (usually 3-4 bits each).
-   When you add an element, you increment the counters instead of just setting bits to 1.
-   To remove an element, you decrement the counters.
-   If a counter drops to zero, you know that bit position is definitely not set by any element.

**Pros:**

-   Supports deletion of elements
-   Can track the number of times an element was added (up to the counter’s maximum value)

**Cons:**

-   Uses more space than a standard Bloom Filter
-   Still susceptible to false positives

**Real-world use:** Counting Bloom Filters are great for network routers that need to track flows and quickly adapt to changing network conditions.

![Counting Bloom Filter](https://www.bytedrum.com/_astro/counting_bloom_filter.DACqQGsG_Z1guiV0.svg)

Counting Bloom Filters have an interesting trade-off: while they allow for deletions, they can suffer from saturation if elements are added and removed frequently. This is because counters might reach their maximum value and “stick” there, leading to an increase in false positives over time. Some implementations use variable-sized counters to mitigate this issue.

### 2\. Cuckoo Filter: The Filter That Kicks Out Intruders[](https://www.bytedrum.com/posts/bloom-filters/#2-cuckoo-filter-the-filter-that-kicks-out-intruders)

The Cuckoo Filter<sup><a href="https://www.bytedrum.com/posts/bloom-filters/#user-content-fn-16" id="user-content-fnref-16" data-footnote-ref="" aria-describedby="footnote-label">18</a></sup> is like a Bloom Filter that went to assertiveness training.

**How it works:**

-   Uses a [cuckoo hash table](https://en.wikipedia.org/wiki/Cuckoo_hashing) instead of a bit array
-   Each item has two possible locations in the table
-   If both locations are occupied, it kicks out one of the existing items and moves it to its alternate location

**Pros:**

-   Supports deletion without the space overhead of Counting Bloom Filters
-   Often has better lookup performance than standard Bloom Filters
-   Can have lower false positive rates for the same space

**Cons:**

-   Insertions can be slower, especially as the filter gets full
-   There’s a small chance of insertion failure if the filter gets too full

**Real-world use:** Cuckoo Filters are used in some database systems for efficient set membership testing and join optimization.

![Cuckoo Filter](https://www.bytedrum.com/_astro/cuckoo_filter.C4Z7Nmzw_1zBIuF.svg)

### 3\. Quotient Filter: The Space-Saving Savant[](https://www.bytedrum.com/posts/bloom-filters/#3-quotient-filter-the-space-saving-savant)

If Bloom Filters and hash tables had a baby, and that baby was really good at managing space, you’d get a Quotient Filter.

**How it works:**

-   Stores fingerprints of elements instead of setting bits
-   Uses a clever encoding scheme to pack these fingerprints efficiently
-   Allows for resizing and merging of filters

**Pros:**

-   More space-efficient than Bloom Filters for low false positive rates
-   Supports deletions and counting
-   Can be easily resized without rebuilding from scratch

**Cons:**

-   Lookups can be slower than Bloom Filters, especially as the filter gets full
-   Implementation is (_much_) more complex than a standard Bloom Filter

**Real-world use:** Quotient Filters are used in some bioinformatics applications for efficient storage and querying of k-mer databases.

![Quotient Filter](https://www.bytedrum.com/_astro/quotient_filter._St7KZNo_Z10LUdi.svg)

### The Family Reunion[](https://www.bytedrum.com/posts/bloom-filters/#the-family-reunion)

Each of these structures takes the core idea of the Bloom Filter - space-efficient probabilistic representation of a set - and tweaks it to solve specific problems or add capabilities. They’re like the various tools in a Swiss Army knife, each designed for a particular task but all sharing a common heritage.

Choosing between these structures is like picking the right tool for a job. A standard Bloom Filter might be perfect for a spell checker, but if you’re building a network flow monitor, you might reach for a Counting Bloom Filter. Building a distributed cache? A Cuckoo Filter might be your best bet.

## Conclusion: “Filtering Out the Noise”[](https://www.bytedrum.com/posts/bloom-filters/#conclusion-filtering-out-the-noise)

Bloom Filters, in their essence, are a testament to the idea that sometimes, a little uncertainty can lead to enormous gains in efficiency. They remind us that in the world of computer science, as in life, absolutes are rare, but clever approximations can take us far.

Let’s recap the key points of our Bloom Filter adventure:

1.  **Simplicity is Power**: With just a bit array and a few hash functions, Bloom Filters manage to solve complex problems in remarkably efficient ways. It’s a reminder that sometimes, the most elegant solutions are also the simplest.
2.  **Trades Offs Are Everywhere**: Bloom Filters trade absolute certainty for speed and space efficiency. This fundamental trade-off is at the heart of many engineering decisions, and Bloom Filters exemplify it beautifully.
3.  **Practical Magic**: From web browsers to databases, from spell checkers to CDNs, Bloom Filters are quietly working behind the scenes, making our digital experiences faster and more efficient.
4.  **Adaptability**: The variations we’ve seen - Counting Bloom Filters, Cuckoo Filters, Quotient Filters - show how a good idea can be adapted and evolved to meet different needs.
5.  **The Power of Probability**: Bloom Filters teach us that in many real-world scenarios, “_probably_” is good enough, and the gains in efficiency from this approach can be enormous.

I hope you’ve gained not just knowledge about a specific data structure, but also a deeper appreciation for the clever tricks and trade-offs that power our digital world. Bloom Filters are more than just a tool in a programmer’s toolkit - they’re a philosophy, a way of thinking about problems that values pragmatism and efficiency.

The next time you’re faced with a seemingly impossible task of querying a massive dataset, or when you need to quickly check if something might exist without the luxury of unlimited memory, remember the humble Bloom Filter. It might just be the right tool for the job.

And who knows? Maybe the next time you’re up at 2 AM, surrounded by energy drink cans and wrestling with a thorny coding problem, you’ll have a moment of clarity. You’ll think back to this blog post, smile, and say to yourself, “You know what? I think a Bloom Filter might just do the trick.”

After all, in the grand algorithm of life, we’re all just trying to filter out the noise and find the signal. And sometimes, a little probabilistic magic is all we need to get there.

Happy coding, and may your false positives be few and your lookups lightning-fast!

1.  I’m not affiliated with any of these resources, I just found them to be the best resources for learning algorithms and data structures.
    
    -   [Introduction to Algorithms](https://www.amazon.com/Introduction-Algorithms-3rd-MIT-Press/dp/0262033844) by Cormen, Leiserson, Rivest, and Stein. This is probably the most recommended book for learning algorithms and data structures, and rightfully so. It’s a bit dense, but it’s a great resource for a deep dive into the subject.
    -   [MIT OpenCourseWare’s “Introduction to Algorithms” course](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/). This is a free online course that covers the same material as the book, but in a more interactive format.
    -   [Striver’s SDE Sheet](https://takeuforward.org/interviews/strivers-sde-sheet-top-coding-interview-problems). I mostly used this as a guide to find LeetCode problems of the structures I was learning.
    -   [The Last Algorithms Course You’ll Need](https://frontendmasters.com/courses/algorithms/) by ThePrimeagen. Primeagen is a popular YouTuber who has a knack for making complex concepts easy to understand. This course covers mostly the fundamentals of the algorithms and data structures that you’ll need to know for your interviews.
    -   [Graph Theory Playlist](https://www.youtube.com/watch?v=DgXR2OWQnLc&list=PLDV1Zeh2NRsDGO4--qE8yH72HFL1Km93P) by William Fiset. This is an absolutely fantastic resource for anyone looking to learn anything about graph theory.
    
    [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-2)
2.  Bloom, Burton H. (1970). “[Space/Time Trade-offs in Hash Coding with Allowable Errors](https://dl.acm.org/doi/10.1145/362686.362692)”. Interestingly, Bloom Filters were actually invented for an application in hyphenation algorithms, not for the database and networking applications they’re commonly used for today. This illustrates how fundamental computer science concepts often find unexpected applications far beyond their original purpose. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-3)
    
3.  As of 2024, Spotify has [reported](https://newsroom.spotify.com/company-info/) to having over 100 million tracks in its library and more than 626 million users. That’s a lot of playlists to keep track of! [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-4)
    
4.  Database queries often involve disk I/O, which is much slower than in-memory operations. Network latency adds further delay in distributed systems. Frequent queries can lead to database bottlenecks and increased response times. A study by Amazon found that every 100ms of latency cost them 1% in sales. (Source: [Latency Is Everywhere And It Costs You Sales - How To Crush It](https://highscalability.com/latency-is-everywhere-and-it-costs-you-sales-how-to-crush-it/), High Scalability, 2009) [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-7)
    
5.  A Bloom Filter typically uses 8-16 bits per element, regardless of the element’s size. For comparison, storing full elements or even hashes would require significantly more space. For instance, a SHA-256 hash alone uses 256 bits per element. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-8)
    
6.  The derivation of these formulas involves probability theory and calculus. For a detailed proof, refer to the paper “[Network Applications of Bloom Filters: A Survey](https://www.eecs.harvard.edu/~michaelm/postscripts/im2005b.pdf)” by Broder and Mitzenmacher (2004) [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-1)
    
7.  I like to use book names or song titles - it makes debugging a bit more entertaining. “Why is ‘The Hitchhiker’s Guide to the Galaxy’ showing up as a false positive? Don’t panic!“. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-testing-bloom-filters)
    
8.  [Scalable Bloom Filters](https://www.sciencedirect.com/science/article/abs/pii/S0020019006003127) use a series of Bloom Filters of increasing size and tightening false positive probability to accommodate growing datasets. This clever approach maintains a bounded false positive probability while allowing for unlimited growth [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-15)
    
9.  The transition away from Bloom Filters in web browsers wasn’t just about evolving security threats. It also related to privacy concerns and the need for more dynamic, real-time protection. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-9)
    
10.  Some databases, like Apache Cassandra, use Bloom Filters not just for join optimization, but also to quickly determine if a particular row exists in a given SSTable (Sorted String Table) file. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-10)
    
11.  The effectiveness of Bloom Filters in database joins can vary depending on the data distribution. They’re particularly powerful for “needle in a haystack” scenarios where the joining column has high cardinality. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-11)
    
12.  The first computerized spell checker, developed for the UNIX operating system in the 1970s, used a simple hash table rather than a Bloom Filter (Source: [Computer Programs for Detecting and Correcting Spelling Errors](http://simson.net/ref/2006/csci_e-180/ref/spelling-p676-peterson.pdf) by James L. Peterson). Bloom Filters became popular for this application later due to their memory efficiency. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-12)
    
13.  The effectiveness of Bloom Filters in cache systems can be further enhanced by using Counting Bloom Filters, which allow for dynamic removal of items from the filter as they’re evicted from the cache. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-13)
    
14.  [dablooms - an open source, scalable, counting bloom filter library](https://word.bitly.com/post/28558800777/dablooms-an-open-source-scalable-counting) bit.ly Engineering Blog, 2012. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-bitly)
    
15.  The effectiveness of Bloom Filters in caching scenarios can vary. In systems with very high cache hit rates, the additional Bloom Filter check might actually introduce more overhead than it saves. It’s crucial to benchmark your specific use case. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-api-caching)
    
16.  Apache Spark uses a combination of Bloom Filters and time-based partitioning for deduplication in its Structured Streaming module. This approach is particularly useful for handling large-scale, real-time data streams. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-spark-bloom-filters)
    
17.  Time-partitioned Bloom Filters can be particularly effective for handling temporal data streams. By rotating filters based on time windows, you can effectively implement a sliding window approach to deduplication, which can be crucial in scenarios like fraud detection or real-time analytics [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-time-based-deduplication)
    
18.  The name “Cuckoo Filter” comes from Cuckoo Hashing, which in turn is named after the cuckoo bird’s habit of kicking other birds’ eggs out of their nests. It’s basically the ‘_There can be only one!_’ of data structures. Interestingly, Cuckoo Filters can actually outperform Bloom Filters in terms of space efficiency when the target false positive rate is less than 3%, making them particularly useful for applications requiring very low error rates. [↩](https://www.bytedrum.com/posts/bloom-filters/#user-content-fnref-16)
