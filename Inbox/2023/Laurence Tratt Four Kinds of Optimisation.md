---
created: 2023-11-15T21:13:30 (UTC -03:00)
tags: []
source: https://tratt.net/laurie/blog/2023/four_kinds_of_optimisation.html?utm_source=tldrnewsletter
author: 
---

# Laurence Tratt: Four Kinds of Optimisation

> ## Excerpt
> Premature optimisation might be the root of all evil, but
overdue optimisation is the root of all frustration. No matter
how fast hardware becomes, we find it easy to write programs which run too
slow. Often this is not immediately apparent. Users can go for years
without considering a program's performance to be an issue before it
suddenly becomes so — often in the space of a single working day.

---
Premature optimisation might be the root of all evil, but overdue optimisation is the root of all frustration. No matter how fast hardware becomes, we find it easy to write programs which run too slow. Often this is not immediately apparent. Users can go for years without considering a program's performance to be an issue before it suddenly becomes so — often in the space of a single working day.

I have devoted more of my life to optimisation than I care to think about, and that experience has led me to make two observations:

1.  Human optimism leads us to believe that we can easily know where a program spends most of its time.
2.  Human optimism leads us to believe that we can easily know how to make the slow parts of a program run faster.

You will not be surprised to learn that I think both forms of optimism misplaced. Partly this is because, as hardware and software have become more sophisticated, it has become harder to understand their effects on performance. But, perhaps more fundamentally, we tend to overestimate how much we know about the software we're working on. We overemphasise the parts of the system we've personally worked on, particularly those we've most recently worked on. We downplay other parts of the system, including the impact of dependencies (e.g. libraries).

The solution to the first of these observations is fairly widely known — one should rigorously profile a program before assuming one knows where it is spending the majority of its time. I deliberately say "rigorously profile" because people often confuse "I have profiled a program once" with "I have built up a good model of a program's performance in a variety of situations". Sometimes, a quick profiling job is adequate, but it can also mislead. Often it is necessary to profile a program with different inputs, sometimes on different machines or network configurations, and to use a variety of sampling and non-sampling approaches \[1\].

However, the multiple solutions, and their inevitable trade-offs, to the second observation are, I believe, underappreciated. I tend to think that there are four main solutions:

1.  Use a better algorithm.
2.  Use a better data-structure.
3.  Use a lower-level system.
4.  Accept a less precise solution.

In the rest of this post I'm going to go through each of these and give some suggestions for the trade-offs involved.

### Use a better algorithm

Let's imagine – and I've genuinely seen this happen! – that after careful profiling of a Python program, I find that I'm spending most of my time in a function which looks like this:

```
def f1(l):
  while True:
    c = False
    for i in range(0, len(l) - 1):
      if l[i+1] < l[i]:
        t = l[i]
        l[i] = l[i+1]
        l[i+1] = t
        c = True
    if not c: return l
```

It's a [bubble sort](https://en.wikipedia.org/wiki/Bubble_sort)! At this point, many people will start guffawing, because it's an obviously slow way of sorting elements. However, bubble sort has an often-forgotten advantage over many "better" algorithms: it runs in constant memory \[2\]. I could gamble that my program doesn't need to use constant memory, but if I'm unsure, I can use an alternative algorithm which preserves this property. Let's try a [selection sort](https://en.wikipedia.org/wiki/Selection_sort):

```
def f2(l):
  for i in range(0, len(l) - 1):
    m = i
    for j in range(i + 1, len(l)):
      if l[j] < l[m]: m = j
    if m != i:
      t = l[i]
      l[i] = l[m]
      l[m] = t
  return l
```

If I use this quick testing code:

```
import random, time
l = [random.random() for _ in range(1000)]
before = time.time()
l1 = f1(l[:])
print(time.time() - before)
before = time.time()
l2 = f2(l[:])
print(time.time() - before)
```

and run it on CPython 3.11 on a Linux server I consistently get timings along the lines of:

```
0.0643463134765625
0.020025014877319336

```

In other words, selection sort is about three times faster than bubble sort in my test.

You don't need me to tell you that selection sort isn't the fastest possible sorting algorithm, but "fastest" is a more slippery concept than it first appears. For example, the selection sort algorithm above is faster than the bubble sort for random data, but the bubble sort is much faster for sorted data \[3\]. The relationship between inputs and algorithmic performance can be subtle. Famously, if you choose an unfortunate "pivot" when implementing [quicksort](https://en.wikipedia.org/wiki/Quicksort), you'll find that it is very non-quick (e.g. you can make it as slow on already-sorted data as the selection sort above).

We can generalise from this that "use a better algorithm" requires understanding the wider context of your system and the nature of the algorithm you're thinking of using. For example, I've often seen people conflate an algorithm's best-case, average-case, and worst-case performance — but the differences between those three pieces of information can be vital when I'm optimising a program. Sometimes I might know something about my program (e.g. the nature of its inputs) that makes me confident that the worst case can't happen, or I don't consider the worst case to be a problem (e.g. its a batch job and no-one will notice occasional latency). But, generally, I care more about the worst case than the best case, and I select algorithms accordingly.

It's also not uncommon that algorithms that have good theoretical performance have poor real-world performance (big O notation can hide many sins). If in doubt, I try gradually more test data until I feel I have truly understood the practical consequences of different choices.

It's also easy to overlook complexity. Fundamentally, faster algorithms are faster because they observe that some steps in a calculation can be side-stepped. I can still remember the first time I read the [description for timsort](https://github.com/python/cpython/blob/main/Objects/listsort.txt): the beauty of its algorithmic observations has stayed with me ever since. But verifying those observations is harder than we imagine — even timsort, created by one of the greatest programmers I have ever come across, had a subtle bug in it \[4\].

When us mortals implement faster algorithms, they are often slightly incorrect, particularly when newly implemented, either producing wrong results or not having the expected performance characteristics \[5\]. For example, parallelising an algorithm can often lead to huge speedups, particularly as CPUs gain more cores, but how many of us understand the C11 memory model well enough to feel confident of the consequences or parallelisation?

The combination of (in)correctness and the difficulty in understanding the context in which an algorithm is fast means that I frequently encourage people to start with a simple algorithm and only move to something "faster" if they really find they need to. Picking (and, if necessary, implementing) the right algorithm for the task at hand is a surprisingly difficult skill!

### Use a better data-structure

Let's imagine that I profile another program and find that I spend most of my time in the following function:

```
def f3(l, e):
  for x in l:
    if x == e: return True
  return False
```

It's an existence check function! Optimising these can be quite interesting, because my choices will depend on how the lists passed to this function are used. I could change the list to a binary tree, for example. But if I can tell, as is not uncommon, that we repeatedly check for the existence of elements in a list that is never mutated after initial creation, I might be able to get away with a very simple data-structure: a sorted list. That might sound odd, because "sorted list" doesn't sound like much of a data-structure, but that then allows me to do a [binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm). For anything but the smallest lists \[6\], binary search is much quicker than the linear search above.

Just as with "use a better algorithm", "use a better data-structure" requires careful thought and measurement \[7\]. In general, while I often find it necessary to implement my own "better algorithms", I rarely find it necessary to implement my own "better data-structures". Partly this is laziness on my part, but it's mostly because data-structures are more easily packaged in a library than better algorithms \[8\].

There is an important tactical variant on "better data-structures" that is perhaps best thought of as "put your structs/classes on a diet". If a program is allocating vast numbers of a given struct/class, the size of that struct/class in bytes can become a significant cost in its own right. When I was working on [error recovery in grmtools](https://tratt.net/laurie/blog/2020/automatic_syntax_error_recovery.html), I found that simply reducing the most commonly allocated struct by 8 bytes in size improved total program performance by 5% — a trick that, from memory, I repeated twice!

There are many similar tactics to this, for example reducing "pointer chasing" (typically by folding multiple structs/classes into one), encouraging memory locality and so on. However, while it's easy to measure the size of a struct/class and how often it's allocated, it's difficult to measure the indirect impact of things like memory locality — I have heard such factors blamed for poor performance much more often than I have seen such factors proven as responsible for poor performance. In general, I only look to such factors when I'm getting desperate.

### Use a lower-level system

A time-honoured tradition is to rewrite parts of a program in a lower-level programming language. Let's rewrite our Python bubble sort into Rust:

```
use std::cmp::PartialOrd;
fn f1<T: Copy + PartialOrd>(l: &mut Vec<T>) {
  loop {
    let mut c = false;
    for i in 0..l.len() - 1 {
      if l[i + 1] < l[i] {
        let t = l[i];
        l[i] = l[i + 1];
        l[i + 1] = t;
        c = true;
      }
    }
    if !c {
      return;
    }
  }
}
```

I mildly adopted my Python program from earlier to save out 1000 random floating point numbers, and added this testing code in Rust:

```
use {env::args, fs::read_to_string, time::Instant};
fn main() {
  let mut l = read_to_string(args().nth(1).unwrap())
    .unwrap()
    .lines()
    .map(|x| x.parse::().unwrap())
    .collect::>();
  let before = Instant::now();
  f1(&mut l);
  println!("{}", (Instant::now() - before).as_secs_f64());
}
```

My Rust bubble sort runs in 0.001s, about 60x faster than the Python version. This looks like a great success for "rewrite in a lower-level programming language" — but you may have noticed that I titled this section "Use a lower-level system".

Instead of spending 15 minutes writing the Rust code, it would have been smarter of me to recognise that my Python bubble sort is likely to emphasise CPython's (the most common implementation of Python) weaknesses. In particular, CPython will represent what I conceptually thought of as a list of floating point numbers as an array of pointers to individually heap-allocated Python objects. That representation has the virtue of generality, but not efficiency.

Although it's often forgotten, CPython isn't the only implementation of Python. Amongst the alternatives is [PyPy](https://pypy.org/), which just so happens to [represent lists of floats as efficiently as Rust](https://tratt.net/laurie/research/pubs/html/bolz_diekmann_tratt__storage_strategies_for_collections_in_dynamically_typed_languages/). Simply typing `pypy` instead of `python` speeds my bubble sort up by 4x! There are few changes I can make that give me such a big performance improvement for such little effort. That's not to say that PyPy runs my program as fast as Rust (PyPy is still about 15x slower) but it may well be fast enough, which is what really matters.

I have seen multiple organisations make the mistake of trying to solve performance problems by rewriting their software in lower-level programming languages, when they would have got sufficient benefit from working out how to run their existing software a little faster. There are often multiple things one can do here, from using different language implementations, to checking that you've got compiler optimisations turned on \[9\], to using faster libraries or databases, and so on. Sometimes rewriting in a lower-level programming language really is the right thing to do, but it is rarely a quick job, and it inevitably introduces a period of instability while bugs are shaken out of the new version.

### Accept a less precise solution

A common problem we face is that we have _n_ elements of something and we want to understand the best subset or ordering of those for our situation. Let's imagine that I've implemented a compiler and 30 separate optimisation passes. I know that some optimisation passes are more effective if they run after other optimisation passes, but I don't know what the most effective ordering of all the passes is.

I could write a program to enumerate all the permutations of those 30 passes, run them against a benchmark suite I possess, and then select the fastest permutation. But if my benchmark suite takes 1 second to run then it will take roughly 2<sup>82</sup> years to evaluate all the possibilities — which is rather longer than the current age of the universe. Clearly I can't wait that long for an answer: I can only run a subset of all the permutations. In situations such as this, I have to accept that I'll never be able to know for sure what the best possible answer is: but, that said, I can at least make sure I end up with a better answer than not trying anything at all.

There are various ways of tackling this but most boil down to [local search](https://en.wikipedia.org/wiki/Local_search_(optimization)). In essence, we define a metric (in our running example, how fast our benchmark suite runs) that allows us to compare two solutions (in our case, faster is better) and discard the worst. We then need a way of generating a neighbour solution to the one we already have, at which point we recalculate the metric and discard the worse of the old and new solution. After either a fixed time-limit, or if we can't find solutions which improve our metric, we return the best solution we've found. The effectiveness of this simple technique (the core algorithm is a few lines of code) tends to stun newcomers, since the obvious problem of [local optima](https://en.wikipedia.org/wiki/Local_optimum) seems like it should undermine the whole idea.

As typically implemented, local search as I've outlined it above produces correct but possibly non-optimal solutions. Sometimes, however, we're prepared to accept an answer which is less precise in the sense that it is possibly "incorrect". By this I don't mean that the program is buggy, but that the program may deliberately produce outputs that do not fully match what we would consider the "full and proper" answer.

Exactly what constitutes "correct" varies from one situation to another. For example, [fast inverse square root](https://en.wikipedia.org/wiki/Fast_inverse_square_root) approximates multiplicative inverse: for situations such as games, its fast nearly-correct answer is a better trade-off than a slow definitely-correct answer. A [Bloom filter](https://en.wikipedia.org/wiki/Bloom_filter) can give false positives: accepting that possibility allows it to be exceptionally frugal with memory. JPEG image compression deliberately throws away some of an image's fine details in order to make the image more compressible. Unlike other image compression approaches I cannot recover the original imagine perfectly from a JPEG, but by foregoing a little bit of image quality, I end up with much smaller files to transmit.

I think that, in general, most programmers struggle to accept that correctness can sometimes be traded-off — personally, it offends a deep internal conviction of mine that programs should be correct. Probably because of that, I think the technique is used less often than it should be.

Recently, though, we've become much more willing to accept incorrect answers thanks to the explosion of ML (Machine Learning). Whereas local search requires us to explicitly state how to create new solutions, ML is trained on previous data, and then generates new solutions from that data. This can be a very powerful technique, but ML's inevitable "hallucinations" are really just a form of incorrectness.

We can thus see that there are two different ways of accepting imprecise solutions: possibly non-optimal; and possibly incorrect. I've come to realise that many people think they're the same thing, but possible incorrectness more often causes problems. I might be happy trading off a bit of image-quality for better compression, but if an ML system rewrites my code and leaves off a "not" I'm unhappy. My rule of thumb is that unless you are convinced you can tolerate incorrectness, you're best off assuming that you can't.

### Summary

I've listed the four optimisation approaches above in the frequency with which I've seen them used (from most to least used).

It will probably not surprise you that my least favourite approach is "rewrite in a lower-level programming language", in the sense that it tends to offer the poorest ratio of improvement/cost. That doesn't mean that it's always the wrong approach, but we tend to reach for it before we've adequately considered cheaper alternatives. In contrast, I think that until recently we have too rarely reached for "accept a less precise solution", though the ML explosion has rapidly changed that.

Personally, when I'm trying to optimise a program I tend to reach for the simplest tricks first. One thing that I've found surprises people is how often my first attempt at optimisation will be to hunt for places to use hashmaps — only rarely do I go hunting for exotic data-structures to use. I less often turn to clever algorithms. Of those clever algorithms I tend to implement myself, I suspect that binary search is the one I use the most often, and I probably do so at most once or twice a year — each time I implement it, I have to look up the correct way to do so \[10\]!

Ultimately, having written this post, I've come to realise that there are three lessons that cut across all of the approaches.

First, when correctness can be sacrificed for performance, it's a powerful technique — but we often sacrifice correctness for performance unintentionally. When we need to optimise a program, it's best to use the least complex optimisation that will give us the performance we want, because that's likely to introduce the fewest bugs.

Second, human time matters. Because us programmers enjoy complexity so much, it's tempting for us to reach for the complex optimisations too soon. Even if they succeed in improving performance – which they often don't! – they tend to consume much more time than is necessary for the performance improvement we needed.

Third, I think that breadth of optimisation knowledge is more important than depth of optimisation knowledge. Within each of the approaches I've listed in this post I have a couple of tricks that I regularly deploy. That has helped give me a reasonable intuition about what the most appropriate overall approach to my current performance woes might be, even if I don't know the specifics.

**Acknowledgements:** Thanks to [Carl Friedrich Bolz-Tereick](https://cfbolz.de/), and [Jake Hughes](https://jakehughes.uk/) for comments.

**Update (2023-11-14)**: My original phrasing of a Bloom filter could be read in a way that seemed to be a contradiction. I've tweak the phrasing to avoid this.

### Footnotes

\[1\]Often, when I'm first looking at performance, I only need a rough sense of what's going on, as I narrow down what parts I need to investigate in detail. I often start with normal Unix `time`, move to something like [multitime](https://tratt.net/laurie/src/multitime), then perhaps a sampling profiler like Linux'x `perf`, before finally using a more detailed profiler like Valgrind. Each of these tools takes a bit more time to run and understand than the previous, which is why I don't start with (say) Valgrind.

A harder challenge is what inputs to use. It is always tempting to benchmark a program with a single input, and I fall into that trap often, despite knowing that it is a trap.  
\[2\]In other words: the amount of memory the algorithm uses is not affected by the size or nature of the input.  
\[3\]Formally speaking the bubble sort is O(n) for sorted data whereas the selection sort is O(n<sup>2</sup>). That's a rather big difference!  
\[4\]The initial [paper analysing timsort](https://drops.dagstuhl.de/opus/volltexte/2018/9467/pdf/LIPIcs-ESA-2018-4.pdf) isn't even the end of this story: see [this subsequent Python issue](https://bugs.python.org/issue34561) for even more!  
\[5\]Carl-Friedrich Bolz pointed out something which I have seen many times, but never thought to classify specifically: it's really easy to implement algorithms in a way that's accidentally quadratic. A classic case is string concatenation in languages where strings are immutable: each concatenation looks harmless from a performance perspective, but if you generate a large string by gradually concatenating a small starting string, you can end up in trouble. [This blog has many more examples](https://accidentallyquadratic.tumblr.com/)!  
\[6\]In my experience, binary search becomes faster than linear search after about 8-12 elements.  
\[7\]Indeed, "better data-structures" generally implies "better algorithm" somewhere.  
\[8\]For example, I have implemented some fiercely complex algorithms, but I've never written a tree rebalancer.  
\[9\]I'm continually surprised by how often I see people run software that hasn't been compiled with optimisations.  
\[10\]Rust's `binary_search_by` method has largely stopped my annual confusion over how to pick the next midpoint correctly.  

### Comments
