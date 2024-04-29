---
created: 2024-03-06T13:56:57 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2019/05/23/time-and-space-efficiency-an-applied-example-part-1/
author: Chelsea
---

# Time and Space Efficiency: An Applied Example, Part 1 – Chelsea Troy

> ## Excerpt
> Let’s talk about time and space efficiency in software. We’ll do it by talking through a sample problem. This is the first post in a three-part series: Implementing Get, Set, Count, and…

---
Reading Time: 9 minutes

![Screen Shot 2019-05-23 at 12.50.27 PM](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/05/Screen-Shot-2019-05-23-at-12.50.27-PM.png?resize=723%2C236&ssl=1)

Actual footage of the in-memory database we’re about to build.

Let’s talk about time and space efficiency in software. We’ll do it by talking through a sample problem. This is the first post in a three-part series:

1.  **Implementing Get, Set, Count, and Delete (this post)**
2.  [Adding Transaction Support (as long as our computer has tons of memory)](https://chelseatroy.com/2019/06/02/time-and-space-efficiency-an-applied-example-part-2/)
3.  [Improving Space Efficiency of our Transaction Support](https://chelseatroy.com/2019/06/11/time-and-space-efficiency-an-applied-example-part-3/)

This series is a _supplement_ to theoretical resources, not a replacement. If you learn best from specific examples rather than general explanations, this post might help you.

This post is _not_ an introduction to big O notation. If that’s what you’re looking for, you want:

-   [this book](https://pragprog.com/book/jwdsal/a-common-sense-guide-to-data-structures-and-algorithms) for an approachable foundation, or
-   [this book](http://algorithmics.lsi.upc.edu/docs/Dasgupta-Papadimitriou-Vazirani.pdf) for a more comprehensive resource.

### Our Example Problem

We will implement an **in-memory database** ([full code available here](https://github.com/chelseatroy/fun_with_transactions)). Denis Anikin [describes this well](https://medium.com/@denisanikin/what-an-in-memory-database-is-and-how-it-persists-data-efficiently-f43868cff4c1):

> An in-memory database…keeps the whole dataset in RAM.
> 
> Each time you query a database or update data, you only access the main memory. So, there’s no disk involved into these operations.

As we implement this database, we’ll talk through additional tradeoffs in implementation details that affect:

-   speed of record retrieval
-   speed of adding, changing, removing records
-   amount of memory the database takes up

Let’s get started.

### Part I: The Get and Set Operations

We’ll write our database in Ruby, and we’ll interact with it in a command line console.

First, we want to get and set items in our database, like this:

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/05/Screen-Shot-2019-05-19-at-11.05.38-PM.png?resize=460%2C486&ssl=1)

We start out in our `test_database.rb` file. If you’ve ever heard me open my mouth before on any subject remotely engineering related, you already know that I think testing is important.

Our tests will help us drive out the API we want. Then, if we’ll be regularly rethinking our implementation choices over performance concerns, our tests will ensure that our API stays consistent and our database still works.

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Ruby" data-tagsearch-path="test_database.rb"><tbody><tr><td id="file-test_database-rb-L1" data-line-number="1"></td><td id="file-test_database-rb-LC1">require_relative "database"</td></tr><tr><td id="file-test_database-rb-L2" data-line-number="2"></td><td id="file-test_database-rb-LC2">require "test/unit"</td></tr><tr><td id="file-test_database-rb-L3" data-line-number="3"></td><td id="file-test_database-rb-LC3">require 'objspace'</td></tr><tr><td id="file-test_database-rb-L4" data-line-number="4"></td><td id="file-test_database-rb-LC4"></td></tr><tr><td id="file-test_database-rb-L5" data-line-number="5"></td><td id="file-test_database-rb-LC5"></td></tr><tr><td id="file-test_database-rb-L6" data-line-number="6"></td><td id="file-test_database-rb-LC6">class TestDatabase &lt; Test::Unit::TestCase</td></tr><tr><td id="file-test_database-rb-L7" data-line-number="7"></td><td id="file-test_database-rb-LC7"></td></tr><tr><td id="file-test_database-rb-L8" data-line-number="8"></td><td id="file-test_database-rb-LC8">def test_get_set_happy_path</td></tr><tr><td id="file-test_database-rb-L9" data-line-number="9"></td><td id="file-test_database-rb-LC9">#These two methods are tested together because isolating them would require opening direct access to database objects.</td></tr><tr><td id="file-test_database-rb-L10" data-line-number="10"></td><td id="file-test_database-rb-LC10">db = Database.new</td></tr><tr><td id="file-test_database-rb-L11" data-line-number="11"></td><td id="file-test_database-rb-LC11"></td></tr><tr><td id="file-test_database-rb-L12" data-line-number="12"></td><td id="file-test_database-rb-LC12">db.set "Crested", "Cardinal"</td></tr><tr><td id="file-test_database-rb-L13" data-line-number="13"></td><td id="file-test_database-rb-LC13">assert_equal("Cardinal", db.get("Crested"))</td></tr><tr><td id="file-test_database-rb-L14" data-line-number="14"></td><td id="file-test_database-rb-LC14">end</td></tr><tr><td id="file-test_database-rb-L15" data-line-number="15"></td><td id="file-test_database-rb-LC15"></td></tr><tr><td id="file-test_database-rb-L16" data-line-number="16"></td><td id="file-test_database-rb-LC16">def test_get_no_value</td></tr><tr><td id="file-test_database-rb-L17" data-line-number="17"></td><td id="file-test_database-rb-LC17">db = Database.new</td></tr><tr><td id="file-test_database-rb-L18" data-line-number="18"></td><td id="file-test_database-rb-LC18"></td></tr><tr><td id="file-test_database-rb-L19" data-line-number="19"></td><td id="file-test_database-rb-LC19">assert_equal("NULL", db.get("NoKey"))</td></tr><tr><td id="file-test_database-rb-L20" data-line-number="20"></td><td id="file-test_database-rb-LC20">end</td></tr><tr><td id="file-test_database-rb-L21" data-line-number="21"></td><td id="file-test_database-rb-LC21">end</td></tr></tbody></table>

So now we arrive at some implementation decisions.

**What sort of data structure would make sense for this database?**

Options:

1.   Use an array. Allow the array indices to index the objects, and iterate through them as needed. Row-oriented databases, like Postgres, and column-oriented databases, like Cassandra, do a (very, very) fancy version of this. Which one you choose depends on which axis you need to retrieve quickly. We touched on the tradeoffs of that decision [in this post](https://chelseatroy.com/2019/03/14/applied-data-science-case-study-part-3-exploratory-analysis/).
2.  Use a hash. Treat the keys as the indices to our values, and grab them out by those keys. Hash key lookup is _fast AF_. [This post offers a pretty spectacular rundown on how and why](https://www.engineyard.com/blog/hash-lookup-in-ruby-why-is-it-so-fast). The tradeoff here: we’re no longer grouping our data by the rows or the columns. For us, right now, not an issue: we do not currently have a use case for iterating or grouping. We will in a second.

Let’s store our data as a hash:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Ruby" data-tagsearch-path="database.rb"><tbody><tr><td id="file-database-rb-L1" data-line-number="1"></td><td id="file-database-rb-LC1">class Database</td></tr><tr><td id="file-database-rb-L2" data-line-number="2"></td><td id="file-database-rb-LC2">def initialize</td></tr><tr><td id="file-database-rb-L3" data-line-number="3"></td><td id="file-database-rb-LC3">@db = Hash.new()</td></tr><tr><td id="file-database-rb-L4" data-line-number="4"></td><td id="file-database-rb-LC4">end</td></tr><tr><td id="file-database-rb-L5" data-line-number="5"></td><td id="file-database-rb-LC5"></td></tr><tr><td id="file-database-rb-L6" data-line-number="6"></td><td id="file-database-rb-LC6"># CRUD COMMANDS</td></tr><tr><td id="file-database-rb-L7" data-line-number="7"></td><td id="file-database-rb-LC7"></td></tr><tr><td id="file-database-rb-L8" data-line-number="8"></td><td id="file-database-rb-LC8">def set(key, value)</td></tr><tr><td id="file-database-rb-L9" data-line-number="9"></td><td id="file-database-rb-LC9">@db[key] = value</td></tr><tr><td id="file-database-rb-L10" data-line-number="10"></td><td id="file-database-rb-LC10"></td></tr><tr><td id="file-database-rb-L11" data-line-number="11"></td><td id="file-database-rb-LC11">return nil</td></tr><tr><td id="file-database-rb-L12" data-line-number="12"></td><td id="file-database-rb-LC12">end</td></tr><tr><td id="file-database-rb-L13" data-line-number="13"></td><td id="file-database-rb-LC13"></td></tr><tr><td id="file-database-rb-L14" data-line-number="14"></td><td id="file-database-rb-LC14">def get(key)</td></tr><tr><td id="file-database-rb-L15" data-line-number="15"></td><td id="file-database-rb-LC15">@db.fetch(key, "NULL")</td></tr><tr><td id="file-database-rb-L16" data-line-number="16"></td><td id="file-database-rb-LC16">end</td></tr><tr><td id="file-database-rb-L17" data-line-number="17"></td><td id="file-database-rb-LC17">end</td></tr></tbody></table>

### Part 2: The Count Operation

Now that we can set keys and values, we want to be able to count how many times a given _value_ exists in our database. (This is not necessary for keys because setting a new value to a key will overwrite the old value. This is fine as long as database indices are unique, which is usually what developers expect).

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/05/Screen-Shot-2019-05-22-at-11.16.53-PM.png?resize=562%2C284&ssl=1)

These tests describe the functionality we’re trying to get:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Ruby" data-tagsearch-path="test_database.rb"><tbody><tr><td id="file-test_database-rb-L1" data-line-number="1"></td><td id="file-test_database-rb-LC1">…</td></tr><tr><td id="file-test_database-rb-L2" data-line-number="2"></td><td id="file-test_database-rb-LC2"></td></tr><tr><td id="file-test_database-rb-L3" data-line-number="3"></td><td id="file-test_database-rb-LC3">def test_count_happy_path</td></tr><tr><td id="file-test_database-rb-L4" data-line-number="4"></td><td id="file-test_database-rb-LC4">db = Database.new</td></tr><tr><td id="file-test_database-rb-L5" data-line-number="5"></td><td id="file-test_database-rb-LC5"></td></tr><tr><td id="file-test_database-rb-L6" data-line-number="6"></td><td id="file-test_database-rb-LC6">db.set "Snowy", "Owl"</td></tr><tr><td id="file-test_database-rb-L7" data-line-number="7"></td><td id="file-test_database-rb-LC7">assert_equal(1, db.count("Owl"))</td></tr><tr><td id="file-test_database-rb-L8" data-line-number="8"></td><td id="file-test_database-rb-LC8">db.set "GreatHorned", "Owl"</td></tr><tr><td id="file-test_database-rb-L9" data-line-number="9"></td><td id="file-test_database-rb-LC9">assert_equal(2, db.count("Owl"))</td></tr><tr><td id="file-test_database-rb-L10" data-line-number="10"></td><td id="file-test_database-rb-LC10">end</td></tr><tr><td id="file-test_database-rb-L11" data-line-number="11"></td><td id="file-test_database-rb-LC11"></td></tr><tr><td id="file-test_database-rb-L12" data-line-number="12"></td><td id="file-test_database-rb-LC12">def test_count_no_value</td></tr><tr><td id="file-test_database-rb-L13" data-line-number="13"></td><td id="file-test_database-rb-LC13">db = Database.new</td></tr><tr><td id="file-test_database-rb-L14" data-line-number="14"></td><td id="file-test_database-rb-LC14"></td></tr><tr><td id="file-test_database-rb-L15" data-line-number="15"></td><td id="file-test_database-rb-LC15">assert_equal(0, db.count("NoKey"))</td></tr><tr><td id="file-test_database-rb-L16" data-line-number="16"></td><td id="file-test_database-rb-LC16">end</td></tr><tr><td id="file-test_database-rb-L17" data-line-number="17"></td><td id="file-test_database-rb-LC17"></td></tr><tr><td id="file-test_database-rb-L18" data-line-number="18"></td><td id="file-test_database-rb-LC18">…</td></tr></tbody></table>

Ding-dangit, we _just_ made a storage decision based on _not_ needing to group stuff, and now you’re telling me we need to aggregate the _counts_ of these values?

Well, that’s going to be slow. It’s going to be slow because, to do it, we have to iterate over all the values in the database and check that each one matches the thing we want a count of, and increment some counter.

Maybe. But maybe not.

What if, instead, we traded in some extra space to save a bunch of time on this call? What if, for example, we kept a second hash with counts in it?

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Ruby" data-tagsearch-path="database.rb"><tbody><tr><td id="file-database-rb-L1" data-line-number="1"></td><td id="file-database-rb-LC1">class Database</td></tr><tr><td id="file-database-rb-L2" data-line-number="2"></td><td id="file-database-rb-LC2">def initialize</td></tr><tr><td id="file-database-rb-L3" data-line-number="3"></td><td id="file-database-rb-LC3">@count = Hash.new(0)</td></tr><tr><td id="file-database-rb-L4" data-line-number="4"></td><td id="file-database-rb-LC4">@db = Hash.new()</td></tr><tr><td id="file-database-rb-L5" data-line-number="5"></td><td id="file-database-rb-LC5">end</td></tr><tr><td id="file-database-rb-L6" data-line-number="6"></td><td id="file-database-rb-LC6"></td></tr><tr><td id="file-database-rb-L7" data-line-number="7"></td><td id="file-database-rb-LC7"># CRUD COMMANDS</td></tr><tr><td id="file-database-rb-L8" data-line-number="8"></td><td id="file-database-rb-LC8"></td></tr><tr><td id="file-database-rb-L9" data-line-number="9"></td><td id="file-database-rb-LC9">def set(key, value)</td></tr><tr><td id="file-database-rb-L10" data-line-number="10"></td><td id="file-database-rb-LC10">@db[key] = value</td></tr><tr><td id="file-database-rb-L11" data-line-number="11"></td><td id="file-database-rb-LC11">@count[value] += 1</td></tr><tr><td id="file-database-rb-L12" data-line-number="12"></td><td id="file-database-rb-LC12"></td></tr><tr><td id="file-database-rb-L13" data-line-number="13"></td><td id="file-database-rb-LC13">return nil</td></tr><tr><td id="file-database-rb-L14" data-line-number="14"></td><td id="file-database-rb-LC14">end</td></tr><tr><td id="file-database-rb-L15" data-line-number="15"></td><td id="file-database-rb-LC15"></td></tr><tr><td id="file-database-rb-L16" data-line-number="16"></td><td id="file-database-rb-LC16">def get(key)</td></tr><tr><td id="file-database-rb-L17" data-line-number="17"></td><td id="file-database-rb-LC17">@db.fetch(key, "NULL")</td></tr><tr><td id="file-database-rb-L18" data-line-number="18"></td><td id="file-database-rb-LC18">end</td></tr><tr><td id="file-database-rb-L19" data-line-number="19"></td><td id="file-database-rb-LC19"></td></tr><tr><td id="file-database-rb-L20" data-line-number="20"></td><td id="file-database-rb-LC20">def count(value)</td></tr><tr><td id="file-database-rb-L21" data-line-number="21"></td><td id="file-database-rb-LC21">@count.fetch(value, 0)</td></tr><tr><td id="file-database-rb-L22" data-line-number="22"></td><td id="file-database-rb-LC22">end</td></tr><tr><td id="file-database-rb-L23" data-line-number="23"></td><td id="file-database-rb-LC23">end</td></tr></tbody></table>

How much extra space would that take up?

Would it _double_ our database footprint? Probably not. Why not:

-   We only add key-value pairs here for unique values, not _all_ values in the database
-   The value for each `@count` key is an integer. These do not take up much space in the grand scheme of data structure space requirements.

As the database grows exponentially, the size of the `@count` hash relative to the `@db` hash will continue to shrink. How fast it shrinks depends on how often duplicate values are added as well as how complex the values in the database are.

Also as the database grows, we have _solutions_ for space. We can _buy_ more space. We can shard the database to spread it across more space. Advances in computing hardware offer us more space.

Time? None of that is true. You can’t buy time. And if a customer leaves your service because the calls are too slow, it’s really expensive to buy them back, or to buy back their friends from the bad word of mouth they’ve heard. All else equal, space inefficiency is far cheaper than time inefficiency. (There are exceptions to this: microprocessors working in isolated environments, for example. The “all else equal” is there for a reason. This is a rule of thumb, not a commandment carved in stone).

### Part 3: The Delete Operation

Now we need to be able to delete keys from the database.

![](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/05/Screen-Shot-2019-05-23-at-12.08.23-PM.png?resize=470%2C142&ssl=1)

**A Sidenote on API Design:** is it weird that I’m returning the string “NULL” here? Yes. I’m doing that so you can quickly and clearly tell, in the irb console, where my database has returned a null value and where I’m calling a method that always has no return value (see the `db.delete` call in the screenshot above). To have the database return `nil` itself in the case of no value, change the fetch default on line 17 of our current database implementation from “NULL” to `nil`.

Here’s how we want `delete` to work in test format:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Ruby" data-tagsearch-path="test_database.rb"><tbody><tr><td id="file-test_database-rb-L1" data-line-number="1"></td><td id="file-test_database-rb-LC1">…</td></tr><tr><td id="file-test_database-rb-L2" data-line-number="2"></td><td id="file-test_database-rb-LC2"></td></tr><tr><td id="file-test_database-rb-L3" data-line-number="3"></td><td id="file-test_database-rb-LC3">def test_delete_happy_path</td></tr><tr><td id="file-test_database-rb-L4" data-line-number="4"></td><td id="file-test_database-rb-LC4">db = Database.new</td></tr><tr><td id="file-test_database-rb-L5" data-line-number="5"></td><td id="file-test_database-rb-LC5"></td></tr><tr><td id="file-test_database-rb-L6" data-line-number="6"></td><td id="file-test_database-rb-LC6">db.set "Tufted", "Titmouse"</td></tr><tr><td id="file-test_database-rb-L7" data-line-number="7"></td><td id="file-test_database-rb-LC7">assert_equal("Titmouse", db.get("Tufted"))</td></tr><tr><td id="file-test_database-rb-L8" data-line-number="8"></td><td id="file-test_database-rb-LC8"></td></tr><tr><td id="file-test_database-rb-L9" data-line-number="9"></td><td id="file-test_database-rb-LC9">db.delete "Tufted"</td></tr><tr><td id="file-test_database-rb-L10" data-line-number="10"></td><td id="file-test_database-rb-LC10">assert_equal("NULL", db.get("Tufted"))</td></tr><tr><td id="file-test_database-rb-L11" data-line-number="11"></td><td id="file-test_database-rb-LC11">end</td></tr><tr><td id="file-test_database-rb-L12" data-line-number="12"></td><td id="file-test_database-rb-LC12"></td></tr><tr><td id="file-test_database-rb-L13" data-line-number="13"></td><td id="file-test_database-rb-LC13">def test_delete_absent_key</td></tr><tr><td id="file-test_database-rb-L14" data-line-number="14"></td><td id="file-test_database-rb-LC14">db = Database.new</td></tr><tr><td id="file-test_database-rb-L15" data-line-number="15"></td><td id="file-test_database-rb-LC15"></td></tr><tr><td id="file-test_database-rb-L16" data-line-number="16"></td><td id="file-test_database-rb-LC16">assert_equal("That key isn't in the database!", db.delete("NoKey"))</td></tr><tr><td id="file-test_database-rb-L17" data-line-number="17"></td><td id="file-test_database-rb-LC17">end</td></tr><tr><td id="file-test_database-rb-L18" data-line-number="18"></td><td id="file-test_database-rb-LC18"></td></tr><tr><td id="file-test_database-rb-L19" data-line-number="19"></td><td id="file-test_database-rb-LC19">…</td></tr></tbody></table>

**Another Sidenote on API Design:** Notice that my database API is on the _forgiving_ end of the spectrum. Looking for a count on a value that isn’t there? You get 0. Looking to get a key that isn’t there? You get “NULL.” Looking to delete a key that isn’t there? You get “That key isn’t in the database!” I’m not throwing exceptions.

Here is my choice situated among some alternatives, ordered from most forgiving to least forgiving:

1.  I could have executed with _no_ message (like bash’s `$rm -rf somedirectory`)
2.  I could have executed with _a_ message (this is what I chose)
3.  I could have returned a tuple of values: the first a success response, the second an error. Swift APIs often do this, and some HTTP APIs try to facilitate this with a `result` key and an `error` key in the response.
4.  I could have raised an error in the weird cases. Python dictionaries do this in their `dict["somekey"]` syntax: if “somekey” isn’t there I get a `KeyError`.

“Forgiving” sounds like a positive word, but here it’s a descriptor—not a normative judgement. There are tradeoffs. The more forgiving the API, the less likely it is that a harmless anomaly interrupts the code flow, but the more likely it is that a problem slips through unnoticed.

It is possible, for example, to `$rm -rf /` on your console without so much as an error message (FOR THE LOVE OF GOD, DO NOT RUN THAT). That’s probably _too_ forgiving of an API.

That said, having a database API that is relatively unforgiving, in the context of transactions, can lead to problems too. Maybe a transaction needs to make 10,000 changes and one of them is a little messed up. Instead of running the other 9999, the database rolls back, and the one issue has to be hunted down and fixed before the _whole transaction runs again_. We talked about how to write to a database in that exact situation [right here](https://chelseatroy.com/2018/09/20/adventures-in-process-design-a-case-study-on-data-import/).

So, for this example, I chose something pretty forgiving. The “right” choice here depends on your use case.

OK, let’s look at our implementation of delete:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Ruby" data-tagsearch-path="database.rb"><tbody><tr><td id="file-database-rb-L1" data-line-number="1"></td><td id="file-database-rb-LC1">class Database</td></tr><tr><td id="file-database-rb-L2" data-line-number="2"></td><td id="file-database-rb-LC2">def initialize</td></tr><tr><td id="file-database-rb-L3" data-line-number="3"></td><td id="file-database-rb-LC3">@count = Hash.new(0)</td></tr><tr><td id="file-database-rb-L4" data-line-number="4"></td><td id="file-database-rb-LC4">@db = Hash.new()</td></tr><tr><td id="file-database-rb-L5" data-line-number="5"></td><td id="file-database-rb-LC5">end</td></tr><tr><td id="file-database-rb-L6" data-line-number="6"></td><td id="file-database-rb-LC6"></td></tr><tr><td id="file-database-rb-L7" data-line-number="7"></td><td id="file-database-rb-LC7"># CRUD COMMANDS</td></tr><tr><td id="file-database-rb-L8" data-line-number="8"></td><td id="file-database-rb-LC8"></td></tr><tr><td id="file-database-rb-L9" data-line-number="9"></td><td id="file-database-rb-LC9">def set(key, value)</td></tr><tr><td id="file-database-rb-L10" data-line-number="10"></td><td id="file-database-rb-LC10">@db[key] = value</td></tr><tr><td id="file-database-rb-L11" data-line-number="11"></td><td id="file-database-rb-LC11">@count[value] += 1</td></tr><tr><td id="file-database-rb-L12" data-line-number="12"></td><td id="file-database-rb-LC12"></td></tr><tr><td id="file-database-rb-L13" data-line-number="13"></td><td id="file-database-rb-LC13">return nil</td></tr><tr><td id="file-database-rb-L14" data-line-number="14"></td><td id="file-database-rb-LC14">end</td></tr><tr><td id="file-database-rb-L15" data-line-number="15"></td><td id="file-database-rb-LC15"></td></tr><tr><td id="file-database-rb-L16" data-line-number="16"></td><td id="file-database-rb-LC16">def get(key)</td></tr><tr><td id="file-database-rb-L17" data-line-number="17"></td><td id="file-database-rb-LC17">@db.fetch(key, "NULL")</td></tr><tr><td id="file-database-rb-L18" data-line-number="18"></td><td id="file-database-rb-LC18">end</td></tr><tr><td id="file-database-rb-L19" data-line-number="19"></td><td id="file-database-rb-LC19"></td></tr><tr><td id="file-database-rb-L20" data-line-number="20"></td><td id="file-database-rb-LC20">def count(value)</td></tr><tr><td id="file-database-rb-L21" data-line-number="21"></td><td id="file-database-rb-LC21">@count.fetch(value, 0)</td></tr><tr><td id="file-database-rb-L22" data-line-number="22"></td><td id="file-database-rb-LC22">end</td></tr><tr><td id="file-database-rb-L23" data-line-number="23"></td><td id="file-database-rb-LC23"></td></tr><tr><td id="file-database-rb-L24" data-line-number="24"></td><td id="file-database-rb-LC24">def delete(key)</td></tr><tr><td id="file-database-rb-L25" data-line-number="25"></td><td id="file-database-rb-LC25">value = @db[key]</td></tr><tr><td id="file-database-rb-L26" data-line-number="26"></td><td id="file-database-rb-LC26">if value</td></tr><tr><td id="file-database-rb-L27" data-line-number="27"></td><td id="file-database-rb-LC27">@db.delete(key)</td></tr><tr><td id="file-database-rb-L28" data-line-number="28"></td><td id="file-database-rb-LC28">@count[value] -= 1</td></tr><tr><td id="file-database-rb-L29" data-line-number="29"></td><td id="file-database-rb-LC29"></td></tr><tr><td id="file-database-rb-L30" data-line-number="30"></td><td id="file-database-rb-LC30">return nil</td></tr><tr><td id="file-database-rb-L31" data-line-number="31"></td><td id="file-database-rb-LC31">else</td></tr><tr><td id="file-database-rb-L32" data-line-number="32"></td><td id="file-database-rb-LC32">return "That key isn't in the database!"</td></tr><tr><td id="file-database-rb-L33" data-line-number="33"></td><td id="file-database-rb-LC33">end</td></tr><tr><td id="file-database-rb-L34" data-line-number="34"></td><td id="file-database-rb-LC34"></td></tr><tr><td id="file-database-rb-L35" data-line-number="35"></td><td id="file-database-rb-LC35">end</td></tr><tr><td id="file-database-rb-L36" data-line-number="36"></td><td id="file-database-rb-LC36">end</td></tr></tbody></table>

### Conclusion: We have our CRUD commands!

1.  Create: `db.set "My", "Value"`
2.  Read: `db.get "My"`
3.  Update: `db.set "My", "Different Value"`
4.  Delete: `db.delete "My"`

We’ve made a _few_ performance decisions so far. The performance decisions get a little more interesting when we start adding _transaction support_.

This post is getting a bit long, though, so we will add transaction support [in the next post of this series](https://chelseatroy.com/2019/06/02/time-and-space-efficiency-an-applied-example-part-2/).

### If you’re enjoying this series, you might also like:

[This case study on migrating away from a large transaction in a database import](https://chelseatroy.com/2018/09/20/adventures-in-process-design-a-case-study-on-data-import/) (example in Ruby)

[This post on modifying authentication behavior in devise](https://chelseatroy.com/2019/04/08/modifying-authentication-behavior-in-devise/) (a Ruby auth gem)

[This post on mentors and sponsors](https://chelseatroy.com/2019/04/02/leveling-up-20th-post-corollary-mentors-and-sponsors/) (I don’t know…I’m proud of this one, and maybe sometimes you like a break from reading code?)
