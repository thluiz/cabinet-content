---
created: 2024-03-05T14:03:53 (UTC -03:00)
tags: []
source: https://www.hillelwayne.com/post/graph-types/?utm_source=tldrnewsletter
author: 
---

# The Hunt for the Missing Data Type

> ## Excerpt
> A (directed) graph is a set of nodes, connected by arrows (edges). The nodes and edges may contain data. Here are some graphs:

 
All graphs made with graphviz (source)



Graphs are ubiquitous in software engineering:
 Package dependencies form directed graphs, as do module imports. The internet is a graph of links between webpages. Model checkers analyze software by exploring the “state space” of all possible configurations.

---
A (directed) graph is a set of nodes, connected by arrows (edges). The nodes and edges may contain data. Here are some graphs:

![](https://www.hillelwayne.com/post/graph-types/img/graph-examples.gv.png "Four graphs, with circles for nodes and arrows for edges.")

All graphs made with graphviz [(source)](https://www.hillelwayne.com/post/graph-types/img/graph-examples.gv)

Graphs are ubiquitous in software engineering:

1.  Package dependencies form directed graphs, as do module imports.
2.  The internet is a graph of links between webpages.
3.  Model checkers analyze software by exploring the “state space” of all possible configurations. Nodes are states, edges are valid transitions between states.
4.  Relational databases are graphs where the nodes are records and the edges are foreign keys.
5.  Graphs are a generalization of linked lists, binary trees, and hash tables.<sup id="fnref:hash-tables"><a href="https://www.hillelwayne.com/post/graph-types/?utm_source=tldrnewsletter#fn:hash-tables">1</a></sup>

Graphs are also widespread in business logic. Whitepapers with references form graphs of citations. Transportation networks are graphs of routes. Social networks are graphs of connections. If you work in software development long enough, you will end up encountering graphs _somewhere_.

I see graphs everywhere and use them to analyze all sorts of systems. At the same time, I dread actually using graphs in my code. There is almost no graph support in any mainstream language. None have it as a built-in type, very few have them in the standard library, and many don’t have a robust third-party library in the ecosystem. Most of the time, I have to roll graphs from scratch. There’s a gap between how often software engineers could use graphs and how little our programming ecosystems support them. Where are all the graph types?

As I ran into more and more graphs in my work, this question became more and more intriguing to me. So late last year I finally looked for an answer. I put a [call out](https://buttondown.email/hillelwayne/archive/if-you-work-on-a-big-language-id-like-to-talk/) on [my newsletter](https://buttondown.email/hillelwayne/) asking for people with relevant expertise— graph algorithm inventors, language committee members, graph library maintainers— to reach out. I expected to interview a dozen people, but in the end I only needed to talk to four:

1.  **Zayenz**: Former core developer of the [Gecode constraint solver](https://www.gecode.org/), and who has “implemented every graph algorithm there is”
2.  **Bradford**: Author of the [Nosey Parker](https://github.com/praetorian-inc/noseyparker/) security library and inventor of several new graph algorithms
3.  **[Nicole](https://ntietz.com/)**: Former graph database engineer
4.  **Kelly**: Maintainer on the [NetworkX](https://networkx.org/) python graph library and [compiler developer](https://github.com/boothby/repiet).

After these four people all gave similar answers, I stopped interviewing and start writing.

## The reasons

### There are too many design choices

So far I’ve been describing _directed_ graphs. There are also _undirected_ graphs, where edges don’t have a direction. Both directed and undirected graphs can either be simple graphs, where there is a maximum of one edge between two nodes, or multigraphs, where there can be many edges. And then for each of _those_ types we have hypergraphs, where an edge can connect three or more nodes, and ubergraphs, where edges can point to other edges. For each possible variation you have more choices to make: do you assign ids to edges or just to nodes? What data can be stored in a node, and what can be stored in an edge? That’s a lot of decisions for a library to make!

But wait, do these distinctions matter at all? A simple graph is just a degenerate multigraph, and and undirected edge can be losslessly transformed into two directed edges. A language could just provide directed hyperubermultigraphs and let users restrict it however they want.

There are two problems with this. First of all, it changes the interface, like whether various operations return single values or lists. Second, as I’ll discuss later, graph algorithm performance is a serious consideration and the special cases _really matter_. Kelly raised the example of [maximum weight matching](https://en.wikipedia.org/wiki/Maximum_weight_matching). If you know that your graph is “bipartite”, you can use a particular fast algorithm to find a matching, while for other graphs you need to use a slow, more general algorithm.

![](https://www.hillelwayne.com/post/graph-types/img/bipartite.gv.png "A bipartite graph")

A bipartite graph [(source)](https://www.hillelwayne.com/post/graph-types/img/bipartite.gv)

> \[It\] ties back to the “algorithm dispatch problem.” Given a Problem P, a Graph G, and Algorithms A, B, C to solve P on G… which one do you run? If we don’t know that G is bipartite, and Algorithm C only works on bipartite graphs, how much time can we afford to determine whether or not G is bipartite? — _Kelly_

The perfect graph library would support a lot of different kinds of graphs. But that takes time away from supporting what people want to _do_ with graphs. Graph algorithms are notoriously hard to get right. In [this essay](https://www.python.org/doc/essays/graphs/), the inventor of Python implemented his own `find_shortest_path` algorithm. It had to be updated with corrections five times!

> Every single implementation of pagerank that I compared to was wrong. — _Nicole_

So which algorithms should come with the library? “The amount of things people want to do with graphs is absurd,” Kelly told me. That matches my experience, and the experiences of all my interviewees. It sometimes seems like graphs are _too powerful_, that all their possibilities are beyond my understanding. “The question is,” Kelly said, “where do you draw the line?”

For NetworkX, “the line” is approximately 500 distinct graph algorithms, by themselves making up almost 60,000 lines of code. By comparison, the entire Python standard library, composed of 300 packages, is just under 600,000 lines.<sup id="fnref:derivation"><a href="https://www.hillelwayne.com/post/graph-types/?utm_source=tldrnewsletter#fn:derivation">2</a></sup>

With all that, it’s unsurprising that you don’t see graphs in standard libraries. The language maintainers would have to decide which types of graphs to support, what topologies to special-case, and what algorithms to include. It makes sense to push this maintenance work onto third parties. This is already the mainstream trend in language development; even Python, famous for being “batteries included”, is [removing 20 batteries](https://peps.python.org/pep-0594/).

Third parties can make opinionated decisions on how to design graphs and what algorithms to include. But then they’re faced with the next problem: once you have a graph interface, how do you represent it?

### There are too many implementation choices

Let’s imagine we’re supporting only barebones simple directed graphs: nodes have identities, edges do not, neither has any associated data. How do we encode this graph?

![](https://www.hillelwayne.com/post/graph-types/img/many-encodings.gv.png "A graph diagram of `a -> b -> c -> {a, b}`")

[(source)](https://www.hillelwayne.com/post/graph-types/img/many-encodings.gv)

Here are four possible ways a programming language could internally store it:

1.  Edge list: `[[a, b], [b, c], [c, a], [c, b]]`
2.  Adjacency list: `[[b], [c], [a, b]]`
3.  Adjacency matrix: `[0 1 0; 0 0 1; 1 1 0]`
4.  A set of three structs with references to each other

Different graph operations have different performance characteristics on different representations. Take a directed graph with 100 nodes and 200 edges. If we use an adjacency matrix representation, we need a 100×100 matrix containing 200 ones and 9,800 zeros. If we instead use an edge list we need only 200 pairs of nodes. Depending on your PL and level of optimizations that could be a memory difference of 20x or more.

Now instead take a graph with 100 nodes and 8,000 edges and try to find whether an edge exists between node 0 and node 93. In the matrix representation, that’s an O(1) lookup on `graph[0][93]`. In the edge list representation, that’s an O(|edge|) iteration through all 8,000 edges.<sup id="fnref:variations"><a href="https://www.hillelwayne.com/post/graph-types/?utm_source=tldrnewsletter#fn:variations">3</a></sup>

Graphs with only a few edges are sparse and graphs with almost all edges are dense. The same program may need to do both operations on both kinds of graph topologies: if you’re constructing a graph from external data, you could start out with a sparse graph and later have a dense one. There’s no “good option” for the internal graph representation.

And all this trouble is just for the most barebones directed graph! What about implementing node data? Edge data? Different types of nodes and edges? Most third party libraries roughly fall in one of two categories:

1.  Offer a single rich datatype that covers all use-cases at the cost of efficiency. NetworkX stores graph as a dict of dicts of dicts, so that both nodes and edges can have arbitrary data.<sup id="fnref:networkx-representations"><a href="https://www.hillelwayne.com/post/graph-types/?utm_source=tldrnewsletter#fn:networkx-representations">4</a></sup>
    
2.  Offer separate graph types for each representation, and rely on the user to store node and edge data separately from the graph type.
    

An example of the second case would be [Petgraph](https://docs.rs/petgraph/latest/petgraph/index.html), the most popular graph library for Rust. Petgraph has `graph`, `graphmap`, and `matrix_graph` for different use-cases. Bradford used Petgraph for [Nosey Parker](https://github.com/praetorian-inc/noseyparker/), a security tool that scans for secrets across an entire history of a git repo. His benchmarking graph is CPython, which has 250k commits and 1.3M objects but only a few edges per commit node. He went with an adjacency list.

Supporting many representations has a serious downside: you have to do a lot more work to add algorithms. If you write a separate version of the algorithm for each graph representation, you’re tripling or quadrupling the maintenance burden. If you instead write a generic abstraction over polymorphic types, then your library is less performant. One programmer I talked to estimated that a hand-rolled graph algorithm can be 20x faster or more than a generic algorithm.

And this gets into every interviewee’s major complaint.

### Performance is too important

> A “generic” graph implementation often doesn’t cut it. _— Bradford_

This is the big one.

Many, many graph algorithms are NP-complete or harder.<sup id="fnref:np"><a href="https://www.hillelwayne.com/post/graph-types/?utm_source=tldrnewsletter#fn:np">5</a></sup> While NP-complete is often tractable [for large problems](https://www.hillelwayne.com/post/np-hard/), graphs can be _enormous_ problems. The choice of representation plays a big role in how fast you can complete it, as do the specifics of your algorithm implementation.

Everyone I talked to had stories about this. In Nosey Parker, Bradford needed to reconstruct a snapshot of the filesystem for each commit, which meant traversing the object graph. None of the [four provided graph walkers](https://docs.rs/petgraph/latest/petgraph/visit/index.html) scaled to his use case. Instead he had to design a “semi-novel” [graph traversal algorithm](https://github.com/praetorian-inc/noseyparker/blob/aaacceaa4baf0fb6a9c98c95b9b063ed74654349/crates/input-enumerator/src/git_metadata_graph.rs#L337) on the fly, which reduced the memory footprint by a factor of a thousand.

> I was able to get working a proof of concept pretty quickly with \[petgraph\], but then… this is one of those cases where the performance constraints end up meeting reality. _— Bradford_

Zayenz raised a different problem: what if the graph is simply too big to work with? He gave the example of finding a solution to the [15 puzzle](https://en.wikipedia.org/wiki/15_Puzzle). This is done by running a [A\* search](https://en.wikipedia.org/wiki/A*_search_algorithm) on the state space. A state space with [over 20 trillion states](https://mediatum.ub.tum.de/doc/1283911/60434.pdf).

> If you generate all the nodes, you’ve lost already. — _Zayenz_

Zayenz oversaw one research project to add graphs to the Gecode constraint solver. They eventually found that a generic graph type simply couldn’t compete with handpicking the representation for the problem.

Even graph databases, designed entirely around running complex graph algorithms, struggle with this problem. Nicole, the graph database engineer, told me about some of the challenges with optimizing even basic graph operations.

> If you’re doing a traversal, you either have to limit your depth or accept you’re going to visit the entire graph. When you do a depth search, like “go out three steps from this and find the path if it exists”, then you’re just committing to visiting quite a bit of data. _— Nicole_

After leaving that job, she worked as a graph query performance consultant. This usually meant migrating off the graph database. She told me about one such project: to speed the graph queries up, she left one computation as-is and rewrote the rest as MapReduce procedures. “Which was a lot harder to understand,” she said, “But would actually finish overnight.”

All of this means that if you have graph problems you want to solve, you need a lot of control over the specifics of your data representation and algorithm. You simply cannot afford to leave performance on the table.

## It was unanimous

So, the reasons we don’t have widespread graph support:

-   There are many different kinds of graphs
-   There are many different representations of each kind of graph
-   There are many different graph algorithms
-   Graph algorithm performance is very sensitive to graph representation and implementation details
-   People run very expensive algorithms on very big graphs.

This explains why languages don’t support graphs in their standard libraries: too many design decisions, too many tradeoffs, and too much maintenance burden. It explains why programmers might avoid third party graph libraries, because they’re either too limited or too slow. And it explains why programmers might not want to think about things in terms of graphs except in extreme circumstances: it’s just too hard to work with them.

Since starting this research, I’ve run into several new graph problems in my job. I still appreciate analyzing systems as graphs and dread implementing them. But now I know why everybody else dreads them, too. Thank you for reading!

_Thanks to [Predrag Gruevski](https://predr.ag/) for research help, [Lars Hupel](https://lars.hupel.info/), [Predrag Gruevski](https://predr.ag/), [Dan Luu](https://www.danluu.com/), and [Marianne Bellotti](https://medium.com/@bellmar) for feedback, and to all of the people who agreed to do interviews. If you liked this post, come join my [newsletter](https://buttondown.email/hillelwayne/)! I write new essays there every week._

_I train companies in formal methods, making software development faster, cheaper, and safer. Learn more [here](https://www.hillelwayne.com/consulting/)._

___

## Appendix: Languages with Graph Types

### Graph Querying Languages

Graph querying languages (GQLs)<sup id="fnref:GQL"><a href="https://www.hillelwayne.com/post/graph-types/?utm_source=tldrnewsletter#fn:GQL">6</a></sup> are to graph databases what SQL is to relational databases. There is no widely-used standard, but two of the most popular are [SPARQL](https://www.w3.org/TR/sparql11-query/) for querying [RDF triples](https://en.wikipedia.org/wiki/Resource_Description_Framework) and Neo4j’s [cypher](https://neo4j.com/product/cypher-graph-query-language/). Ironically, [GraphQL](https://graphql.org/) is [_not_](https://graphql.org/faq/#is-graphql-a-database-language-like-sql) a graph querying language, instead being named for its connection to the [Facebook Graph Search](https://en.wikipedia.org/wiki/Facebook_Graph_Search). I considered graph databases themselves mostly distinct from graphs in programming languages, but their query languages show how graphs could work in a PL.

The main difference between all GQLs and SQL is that the “joins” (relationships) are first-class entities. Imagine a dataset of movies and people, where people act in, direct, or produce movies. In SQL you’d implement each relationship as a many-to-many tables, which makes it easy to query “who acted in movie X” but hard to query “who had any role in movie Y, and what was that role”. In SPARQL relationships are just edges, making the same query easy.

```sparql
PREFIX mv: <your_movie_ontology_URL> SELECT ?person ?role WHERE { ?person ?role mv:casablanca. }
```

Cypher has a similar construct. GQLs can also manipulate edges: reverse them, compose them together, take the transitive closure, etc. If we wanted to find all actors with some degree of separation from Kevin Bacon, we could write

```sparql
PREFIX mv: <your_movie_ontology_URL> SELECT ?a WHERE { mv:kbacon (:acted_in/^:acted_in)+ ?a. # a/b = join two lookups # ^a = reverse a # a+ = transitive closure }
```

SPARQL cannot give the length of the path nor do computation _along_ the path, like collecting the chain of movies linking two actors. GQLs that support this are significantly more complicated.

My main takeaway from looking at GQLs is that there’s a set of useful traversal primitives that a PL with graph support would need to provide. Interestingly, the formal specification language [Alloy](https://alloytools.org/) has all of these primitives for its “relation” datatype. For this reason I find working with a graph representation in Alloy much easier than in a proper programming language. That said, these all work with labeled edges and may not work for other graph representations.

### Mainstream Languages with Graphs in the Standard Library

**Python** added a [graphlib](https://docs.python.org/3/library/graphlib.html) in 2020. Based on the discussion [here](https://bugs.python.org/issue17005), it was because topological sorting is a “fundamental algorithm” and it would be useful for “pure Python implementations of MRO \[Method Resolution Order\] logic”. Graphlib has no other methods besides `TopologicalSorter`, which only takes graphs represented as node dicts. Unusually, the direction of the node dict is _reversed_: the graph `a -> b` is represented as `{b: [a]}`.

As of 2023, nothing in [CPython uses graphlib](https://github.com/search?q=repo%3Apython%2Fcpython%20TopologicalSorter&type=code) and there are [fewer than 900 files referencing it on Github](https://github.com/search?q=%2F%28from%7Cimport%29+graphlib%2F+language%3Apython+NOT+is%3Afork&type=code). By comparison, another package added in 2020, zoneinfo, appears in over 6,000 files, and the term `def topological_sort(` appears in 4,000. I’d guess a lot of these are from before 2020, though. Some skimming suggests that all of these custom topological sorts take different graph representations than graphlib, so they wouldn’t be convertable regardless. Graph representation matters.

There are two other languages I found with graph types: [Erlang](https://www.erlang.org/doc/man/digraph.html#) and [SWI-Prolog](https://www.swi-prolog.org/pldoc/man?section=ugraphs). I don’t know either language and cannot tell when they were added; with Erlang, at least, it was before 2008. I reached out to a person on the Erlang core language committee but did not hear back.

### Graph languages

Programming languages where “everything is a graph” in the same way that everything in bash a string and everything in lisp is a list. Some examples include [GP2](https://github.com/UoYCS-plasma/GP2) and [Grape](http://jenshweber.github.io/grape/). Based on some correspondence with people in the field, right now this is still highly academic.

### Mathematics Software Languages

Mathematica, MATLAB, Maple, etc all have graph libraries of some form or another. I am not paying the thousands of dollars in licensing needed to learn more.
