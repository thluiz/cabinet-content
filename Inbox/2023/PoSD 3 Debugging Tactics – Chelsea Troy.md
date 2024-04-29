---
created: 2024-03-06T08:57:56 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2020/01/06/posd-2-debugging-tactics/
author: Chelsea
---

# PoSD 3: Debugging Tactics – Chelsea Troy

> ## Excerpt
> I recently read John Ousterhout’s book, Philosophy of Software Design. The previous post on the book shamelessly conscripts its content into a fiery treatise on debugging. In this post, I&#82…

---
Reading Time: 13 minutes

I recently read [John Ousterhout](http://web.stanford.edu/~ouster/cgi-bin/home.php)‘s book, _Philosophy of Software Design._ The [previous post](https://chelseatroy.com/2019/12/30/posd-2-what-causes-insidious-bugs) on the book shamelessly conscripts its content into a fiery treatise on debugging. In this post, I…uh, keep doing that. Sorry, Dr. Ousterhout.

### How do we search for hard-to-find bugs?

![A Bugs Life.jpg](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/A-Bugs-Life.jpg?resize=555%2C416&ssl=1)

_Philosophy of Software Design_ doesn’t directly say. However, it describes a framework for solving a _different_ kind of problem that we can repurpose for finding insidious bugs.

### Designing for Performance

Chapter 20 provides advice for changing of a piece of software whose performance is unacceptable. The steps also come in handy when an app’s behavior is unacceptable.

### **1\. Measure before modifying.**

The book entreats developers not to rush into code bases and start changing things based on their personal hunches. Instead, it makes a case for measuring first.

>  The best way to learn which things are expensive is to run micro-benchmarks (small programs that measure the cost of a single operation in isolation). In the RAMCloud project, we created a… program that provides a framework for microbenchmarks. It took a few days to create the framework, but the framework makes it possible to add new micro-benchmarks in five or ten minutes. This has allowed us to accumulate dozens of micro-benchmarks, We use these both to **understand the performance of existing libraries** used in RAMCloud, and also to **measure the performance of new classes** written for RAMCloud.

Let’s borrow the uses for the micro-benchmarking framework and substitute the word “behavior” instead of “performance.”

### **Use #1: Measure the behavior of new classes.**

A framework for asserting on our code’s behavior allows us to prevent bugs by catching invalid assumptions.

What kind of framework could we use for this? Automated testing is a great start. In writing tests, we _document_ our assumptions. Then when they fail and we implement code to make them pass, we find out whether our assumptions are true. And sometimes they’re not.

Take a look at this unit test for the `KeyValueStore` class in my Raft implementation [project](http://chelseatroy.com/tag/raft):

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Python" data-tagsearch-path="test_keyValueStore.py"><tbody><tr><td id="file-test_keyvaluestore-py-L1" data-line-number="1"></td><td id="file-test_keyvaluestore-py-LC1">from src.key_value_store import KeyValueStore</td></tr><tr><td id="file-test_keyvaluestore-py-L2" data-line-number="2"></td><td id="file-test_keyvaluestore-py-LC2"></td></tr><tr><td id="file-test_keyvaluestore-py-L3" data-line-number="3"></td><td id="file-test_keyvaluestore-py-LC3">TEST_LOG_PATH = "tests/test_kvs_logs.txt"</td></tr><tr><td id="file-test_keyvaluestore-py-L4" data-line-number="4"></td><td id="file-test_keyvaluestore-py-LC4"></td></tr><tr><td id="file-test_keyvaluestore-py-L5" data-line-number="5"></td><td id="file-test_keyvaluestore-py-LC5">class TestKeyValueStore():</td></tr><tr><td id="file-test_keyvaluestore-py-L6" data-line-number="6"></td><td id="file-test_keyvaluestore-py-LC6"></td></tr><tr><td id="file-test_keyvaluestore-py-L7" data-line-number="7"></td><td id="file-test_keyvaluestore-py-LC7">…</td></tr><tr><td id="file-test_keyvaluestore-py-L8" data-line-number="8"></td><td id="file-test_keyvaluestore-py-LC8"></td></tr><tr><td id="file-test_keyvaluestore-py-L9" data-line-number="9"></td><td id="file-test_keyvaluestore-py-LC9">def test_write_to_log(self):</td></tr><tr><td id="file-test_keyvaluestore-py-L10" data-line-number="10"></td><td id="file-test_keyvaluestore-py-LC10">self.clean_up_file(TEST_LOG_PATH)</td></tr><tr><td id="file-test_keyvaluestore-py-L11" data-line-number="11"></td><td id="file-test_keyvaluestore-py-LC11"></td></tr><tr><td id="file-test_keyvaluestore-py-L12" data-line-number="12"></td><td id="file-test_keyvaluestore-py-LC12">kvs = KeyValueStore(server_name="DorianGray")</td></tr><tr><td id="file-test_keyvaluestore-py-L13" data-line-number="13"></td><td id="file-test_keyvaluestore-py-LC13">kvs.write_to_log("set Sibyl cruelty", TEST_LOG_PATH)</td></tr><tr><td id="file-test_keyvaluestore-py-L14" data-line-number="14"></td><td id="file-test_keyvaluestore-py-LC14">self.assert_on_file(path=TEST_LOG_PATH, length=1, lines="0 set Sibyl cruelty")</td></tr><tr><td id="file-test_keyvaluestore-py-L15" data-line-number="15"></td><td id="file-test_keyvaluestore-py-LC15"></td></tr><tr><td id="file-test_keyvaluestore-py-L16" data-line-number="16"></td><td id="file-test_keyvaluestore-py-LC16">kvs.write_to_log("Set Basil wrath", TEST_LOG_PATH)</td></tr><tr><td id="file-test_keyvaluestore-py-L17" data-line-number="17"></td><td id="file-test_keyvaluestore-py-LC17">self.assert_on_file(path=TEST_LOG_PATH, length=2, lines=["0 set Sibyl cruelty", "0 set Basil wrath"])</td></tr></tbody></table>

Say we have a class called `KeyValueStore` that puts all commands into a log file.

Will this version of that method make this test pass?

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Python" data-tagsearch-path="KeyValueStore.py"><tbody><tr><td id="file-keyvaluestore-py-L1" data-line-number="1"></td><td id="file-keyvaluestore-py-LC1">class KeyValueStore:</td></tr><tr><td id="file-keyvaluestore-py-L2" data-line-number="2"></td><td id="file-keyvaluestore-py-LC2"></td></tr><tr><td id="file-keyvaluestore-py-L3" data-line-number="3"></td><td id="file-keyvaluestore-py-LC3">…</td></tr><tr><td id="file-keyvaluestore-py-L4" data-line-number="4"></td><td id="file-keyvaluestore-py-LC4"></td></tr><tr><td id="file-keyvaluestore-py-L5" data-line-number="5"></td><td id="file-keyvaluestore-py-LC5">def write_to_log(self, string_operation, path_to_logs=''):</td></tr><tr><td id="file-keyvaluestore-py-L6" data-line-number="6"></td><td id="file-keyvaluestore-py-LC6">if path_to_logs == '':</td></tr><tr><td id="file-keyvaluestore-py-L7" data-line-number="7"></td><td id="file-keyvaluestore-py-LC7">path_to_logs = "logs/" + self.server_name + "_log.txt"</td></tr><tr><td id="file-keyvaluestore-py-L8" data-line-number="8"></td><td id="file-keyvaluestore-py-LC8">f = open(path_to_logs, "a+")</td></tr><tr><td id="file-keyvaluestore-py-L9" data-line-number="9"></td><td id="file-keyvaluestore-py-LC9">f.write(string_operation + '\n')</td></tr><tr><td id="file-keyvaluestore-py-L10" data-line-number="10"></td><td id="file-keyvaluestore-py-LC10">f.close()</td></tr></tbody></table>

Should, right?

Doesn’t:

![Test Failure](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/Screen-Shot-2019-12-24-at-9.09.25-AM.png?resize=723%2C291&ssl=1)Check out why. We expected _one_ line to have been written to the file, but in fact it looks like _two_ lines have been written to the file, with the last one having an empty string in it.

Let’s investigate why that’s happening.

Here’s how the file looks:

![test_kvs_logs.txt](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/Screen-Shot-2019-12-24-at-9.16.57-AM.png?resize=720%2C156&ssl=1)

And when we remove the first assertion, the second assertion fails as a result of the same issue (but not repeated—there’s still just one extra line, not two):

![Test Failure](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/Screen-Shot-2019-12-24-at-9.18.53-AM.png?resize=723%2C257&ssl=1)

Here’s the file after that:

![tests/test_kvs_logs.tex](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/Screen-Shot-2019-12-24-at-9.22.24-AM.png?resize=676%2C200&ssl=1)

As you can see, the file writes fine. Our assertion fails because our `assert_on_file()` method splits the file’s contents like this:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Python" data-tagsearch-path="test_keyValueStore.py"><tbody><tr><td id="file-test_keyvaluestore-py-L1" data-line-number="1"></td><td id="file-test_keyvaluestore-py-LC1">…</td></tr><tr><td id="file-test_keyvaluestore-py-L2" data-line-number="2"></td><td id="file-test_keyvaluestore-py-LC2"></td></tr><tr><td id="file-test_keyvaluestore-py-L3" data-line-number="3"></td><td id="file-test_keyvaluestore-py-LC3">def assert_on_file(self, path, length, lines):</td></tr><tr><td id="file-test_keyvaluestore-py-L4" data-line-number="4"></td><td id="file-test_keyvaluestore-py-LC4">with open(path, "r") as file:</td></tr><tr><td id="file-test_keyvaluestore-py-L5" data-line-number="5"></td><td id="file-test_keyvaluestore-py-LC5">lines = file.read().split("\n")</td></tr><tr><td id="file-test_keyvaluestore-py-L6" data-line-number="6"></td><td id="file-test_keyvaluestore-py-LC6">assert len(lines) == length</td></tr><tr><td id="file-test_keyvaluestore-py-L7" data-line-number="7"></td><td id="file-test_keyvaluestore-py-LC7">for index, line in enumerate(lines):</td></tr><tr><td id="file-test_keyvaluestore-py-L8" data-line-number="8"></td><td id="file-test_keyvaluestore-py-LC8">assert lines[index] == line</td></tr></tbody></table>

which leaves an empty string at the end of the collection, since the file’s contents end with a newline.

It needs to be this:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Python" data-tagsearch-path="test_keyValueStore.py"><tbody><tr><td id="file-test_keyvaluestore-py-L1" data-line-number="1"></td><td id="file-test_keyvaluestore-py-LC1">…</td></tr><tr><td id="file-test_keyvaluestore-py-L2" data-line-number="2"></td><td id="file-test_keyvaluestore-py-LC2"></td></tr><tr><td id="file-test_keyvaluestore-py-L3" data-line-number="3"></td><td id="file-test_keyvaluestore-py-LC3">def assert_on_file(self, path, length, lines):</td></tr><tr><td id="file-test_keyvaluestore-py-L4" data-line-number="4"></td><td id="file-test_keyvaluestore-py-LC4">with open(path, "r") as file:</td></tr><tr><td id="file-test_keyvaluestore-py-L5" data-line-number="5"></td><td id="file-test_keyvaluestore-py-LC5">lines = file.read().splitlines()</td></tr><tr><td id="file-test_keyvaluestore-py-L6" data-line-number="6"></td><td id="file-test_keyvaluestore-py-LC6">assert len(lines) == length</td></tr><tr><td id="file-test_keyvaluestore-py-L7" data-line-number="7"></td><td id="file-test_keyvaluestore-py-LC7">for index, line in enumerate(lines):</td></tr><tr><td id="file-test_keyvaluestore-py-L8" data-line-number="8"></td><td id="file-test_keyvaluestore-py-LC8">assert lines[index] == line</td></tr></tbody></table>

and then the test passes:

![Screen Shot 2019-12-24 at 9.29.25 AM.png](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2019/12/Screen-Shot-2019-12-24-at-9.29.25-AM.png?resize=723%2C232&ssl=1)

_But that’s just a mistake in a test, Chelsea! That doesn’t affect the code itself._

Sure, except that that assertion method reads from the file the same way that the `KeyValueStore` code does (line 14):

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Python" data-tagsearch-path="key_value_store.py"><tbody><tr><td id="file-key_value_store-py-L1" data-line-number="1"></td><td id="file-key_value_store-py-LC1">class KeyValueStore:</td></tr><tr><td id="file-key_value_store-py-L2" data-line-number="2"></td><td id="file-key_value_store-py-LC2"></td></tr><tr><td id="file-key_value_store-py-L3" data-line-number="3"></td><td id="file-key_value_store-py-LC3">…</td></tr><tr><td id="file-key_value_store-py-L4" data-line-number="4"></td><td id="file-key_value_store-py-LC4"></td></tr><tr><td id="file-key_value_store-py-L5" data-line-number="5"></td><td id="file-key_value_store-py-LC5">def catch_up(self, path_to_logs=''):</td></tr><tr><td id="file-key_value_store-py-L6" data-line-number="6"></td><td id="file-key_value_store-py-LC6">if path_to_logs == '':</td></tr><tr><td id="file-key_value_store-py-L7" data-line-number="7"></td><td id="file-key_value_store-py-LC7">path_to_logs = "logs/" + self.server_name + "_log.txt"</td></tr><tr><td id="file-key_value_store-py-L8" data-line-number="8"></td><td id="file-key_value_store-py-LC8"></td></tr><tr><td id="file-key_value_store-py-L9" data-line-number="9"></td><td id="file-key_value_store-py-LC9">if os.path.exists(path_to_logs):</td></tr><tr><td id="file-key_value_store-py-L10" data-line-number="10"></td><td id="file-key_value_store-py-LC10">f = open(path_to_logs, "r")</td></tr><tr><td id="file-key_value_store-py-L11" data-line-number="11"></td><td id="file-key_value_store-py-LC11">log = f.read()</td></tr><tr><td id="file-key_value_store-py-L12" data-line-number="12"></td><td id="file-key_value_store-py-LC12">f.close()</td></tr><tr><td id="file-key_value_store-py-L13" data-line-number="13"></td><td id="file-key_value_store-py-LC13"></td></tr><tr><td id="file-key_value_store-py-L14" data-line-number="14"></td><td id="file-key_value_store-py-LC14">for command in log.split('\n'):</td></tr><tr><td id="file-key_value_store-py-L15" data-line-number="15"></td><td id="file-key_value_store-py-LC15">self.execute(command, term_absent=False, write=False)</td></tr><tr><td id="file-key_value_store-py-L16" data-line-number="16"></td><td id="file-key_value_store-py-LC16"></td></tr><tr><td id="file-key_value_store-py-L17" data-line-number="17"></td><td id="file-key_value_store-py-LC17">self.catch_up_successful = True</td></tr></tbody></table>

Such that the `execute` method needs to _ignore_ empty strings that shouldn’t be there in the first place:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="Python" data-tagsearch-path="key_value_store.py"><tbody><tr><td id="file-key_value_store-py-L1" data-line-number="1"></td><td id="file-key_value_store-py-LC1">class KeyValueStore:</td></tr><tr><td id="file-key_value_store-py-L2" data-line-number="2"></td><td id="file-key_value_store-py-LC2"></td></tr><tr><td id="file-key_value_store-py-L3" data-line-number="3"></td><td id="file-key_value_store-py-LC3">…</td></tr><tr><td id="file-key_value_store-py-L4" data-line-number="4"></td><td id="file-key_value_store-py-LC4"></td></tr><tr><td id="file-key_value_store-py-L5" data-line-number="5"></td><td id="file-key_value_store-py-LC5">def execute(self, string_operation, term_absent, write=True, path_to_logs=''):</td></tr><tr><td id="file-key_value_store-py-L6" data-line-number="6"></td><td id="file-key_value_store-py-LC6">if len(string_operation) == 0:</td></tr><tr><td id="file-key_value_store-py-L7" data-line-number="7"></td><td id="file-key_value_store-py-LC7">return</td></tr><tr><td id="file-key_value_store-py-L8" data-line-number="8"></td><td id="file-key_value_store-py-LC8"></td></tr><tr><td id="file-key_value_store-py-L9" data-line-number="9"></td><td id="file-key_value_store-py-LC9">…</td></tr></tbody></table>

And if we want the tests to work for the `KeyValueStore` written as is, they all have to assert that the number of lines in the file is one greater than it is. **Which is confusing, because when we open this file, it looks like the right number of lines.**

And this, people, is _exactly_ the kind of thing that will have a developer tearing her hair out for days to figure out why something isn’t working. (Anecdotally, any kind of parsing functionality is _exactly_ where you want tests catching your inaccurate assumptions).

Unit tests help us find discrepancies like this one. Discrepancies also often occur in the interactions between classes. Risk-oriented automated testing is a whole topic in itself, which I talk about more [right over here](https://chelseatroy.com/2018/12/18/risk-oriented-testing-from-rubytapas-screencast/).

Once we discover that the code isn’t doing what we think it should. We’re no longer _preventing_ unwanted functionality: we’re now _diagnosing_ and _correcting_ the functionality.

Here’s where Use #2 of RAMCloud’s micro-benchmarking framework becomes important to us:

### **Use #2: Understand the behavior of existing libraries.**

**Key point: we need to acknowledge that we do not already understand the behavior of our code.** This sounds like an obvious detail, but we often get it wrong.

Failing to acknowledge that we do not understand the behavior of our code most of the time is precisely how we, as an industry:

-   do not have a unified praxis or pedagogy for debugging
-   force everybody to learn debugging skills by themselves from individual experience like we’re all in solitary witch training like [Kiki’s Delivery Service](https://theface.com/culture/kikis-delivery-service-burnout-witches-anime)
-   fail to build one one another’s work or research to get better at it
-   somehow don’t see a massive problem with that approach
-   while simultaneously wondering why all our software has so many bugs
-   and acting like it’s an endearing quality of software that it usually doesn’t quite work the way it should.

![](https://i0.wp.com/thumbs.gfycat.com/BigWateryLacewing-size_restricted.gif?w=723&ssl=1)

**And here’s why:** when we start with the _inaccurate_ assumption that we understand our code, we’re drawn to practices that align with that assumption but don’t actually work. As we gain experience, we learn to _ignore_ these temptations and do something else, even if we don’t fully understand why.

If we instead approach debugging from the perspective that we do _not_ understand our code, we’re drawn _instead_ to practices that _work_, rather than forcing ourselves to learn from experience to do the opposite of what our inaccurate assumptions compel us to do.

### What tactics might we use to build an understanding of our code?

Often, we’re tempted to rush in and change the thing that we think is causing our bug. We’ll talk more about this in the next post about debugging strategies (complete with pictures and more fightin’ words), but for now I’ll say: this strategy stops working when we don’t understand our code’s behavior, as is often the case with bugs.

**Instead, we need tactics to gather information.** Because we don’t understand what’s going on, we need tactics that enable us to explore. We’ll want to interact with our code, and possibly try different things. The tactics that best facilitate this are those that give us a **tight feedback loop**: we can check some stuff out in a short period of time, then take what we learn there to try something else.

**Some tactics:**

-   **Automated Tests:** We’ve talked about tests. Tests allow us to run a series of small feedback loops simultaneously. We can check lots of paths through our code quickly and all at once. Tests aren’t inherently a more “moral” way to develop software or some baloney like that. They just do really well on the key metric that matters to us: the tight feedback loop.
-   **Manual run-throughs:** Developers start doing this almost as soon as they start to write code, and we continue to do it when we want to check things out.
-   **Break Points:** We can stop the code at a specific line, then open a console to look at the variables in scope at that point. We can even run methods in scope from the command line to see what happens. XCode and JetBrains IDEs allow us to set break points by clicking on the line we where we want to break. These work more reliably in compiled languages than interpreted ones, in my experience (and not 100% reliably in either). Some interpreted languages have their own solutions: Ruby has `pry` and Python has `pdb`, for example.
-   **Print Statements:** If break points aren’t working, or if the code is multithreaded or asynchronous in such a way that we don’t know whether the buggy code will run before or after our break point, print statements come in really handy. There seems to be a sort of stigma against using them sometimes, which I find odd: even the most seasoned programmers I have paired with use these ([here’s proof](https://www.youtube.com/watch?v=N5s3FlALykA&feature=youtu.be)).One thing worth noting: print statements usually log to standard OUT, which is **buffered** ([here’s why](https://eklitzke.org/stdout-buffering)). By contrast, exception descriptions usually log to standard ERROR, which is **unbuffered**_._ For this reason, it is possible for a print statement that was called _before_ the Exception was thrown to appear _after_ the exception message, creating the illusion that the process kept running after the exception regardless of whether it actually did. (Thanks to Avdi for pointing this out in our [live stream](https://www.youtube.com/watch?v=N5s3FlALykA&feature=youtu.be)).
-   **Logging:** For deployed code or code where we cannot access standard out, we may need to use more robust logging instead. Bonus: a more permanent logging framework within our code can help us diagnose issues after the fact.
-   **Changing small things:** If I think I know how a variable works, I can change its value a little bit and see if the program reacts the way I expect it to. This helps to establish my understanding of what’s in scope and which code is affecting what.

Each of these tactics permit us to run tiny experiments on our code. We **experiment** to gather firsthand information about phenomena that we don’t already understand. We start doing this as babies, often (to our parent’s dismay) by testing the hypothesis that we can eat things. So we already know _how_ to do it. But by adding a little rigor to our process, we enable ourselves to more efficiently accumulate information through our debugging experiments.

### Exercise: Stating Our Hypotheses

Get an empty notebook or text file to use as your “insidious bug hunting journal.” Though you don’t have to use it for every bug, it’s a helpful tool for heavy-duty bug hunts, as well as for documenting and developing a killer set of framework-agnostic debugging skills for yourself. This is the first exercise for your bug hunting journal, but we’ll introduce other exercises in future posts, too.

Each time you insert a print statement, logging statement, or break point, or do a manual run-through or make an experimental code change, write down what you did and what you expect that the result _should_ be (Examples: _I expect this variable not to be nil. I expect the pop-up to appear on the homepage._) Then run the experiment and document whether or not the result matched your expectation. This does two things for you:

1.  For this individual bug, it gives you a running list of clues to help you systematically narrow down where the bug could be hiding.
2.  For yourself, it creates a record of your perceptions about how the software should work, as well as information about how often those perceptions are correct or incorrect. You can search this information later for patterns of accuracy or inaccuracy, that point you toward what you need to learn more about.

### Once we have identified the cause of a bug, what can we do?

Here, _Philosophy of Software Design_‘s performance improvement recommendations again come in handy.

First, it recommends making some **fundamental change**. This is the most efficient way to resolve a bug that has a single cause. When a lot of little things are happening to make a system brittle and unpredictable, we can apply the advice that the book provides for cases where poor performance has no single cause, but rather lots of little causes:

> Start off by asking yourself what is the smallest amount of code that must be executed to carry out the desired task in the common case. Disregard any existing code structure. Imagine instead that you are writing a new method that implements just the…most common case. The current code is probably cluttered with special cases: ignore them in this exercise. The current code might pass through several method calls on the critical path; imagine instead that you could put all the relevant code in a single method. The current code may also use a variety of variables and data strictures; consider only the data needed for the critical path, and assume whatever data structure is most convenient for the critical path…Assume that you could completely redesign the system in order to minimize the code that must be executed for the critical path. Let’s call this code “the ideal.”

I cannot stop laughing at the idea of calling _any_ code “the ideal,” but the chapter makes a sound point. It continues:

> Ideally, there will be a single `if` statement at the beginning, which detects all special cases with one test. In the normal case, only this one test will need to be made, after which the critical path can be executed with no additional tests for special cases. If the initial test fails (which means a special case has occurred) the code can branch to a separate place off the critical path to handle it. Performance isn’t as important for special cases, so you can structure the special-case code for simplicity rather than performance.

This idea is also useful in rehabilitating our code’s behavior and preventing future bugs. We ask ourselves: _what is the smallest number of questions we can ask to determine if this is the most common case or a special case?_

Then, we separate the common case from the special cases—which, at the very least, makes bugs with one or the other easier to distinguish.

### Conclusion

We’re talking about a broader praxis for debugging. We discussed where bugs come from and the value of automated tests for preventing them. We talked about the importance of acknowledging when we don’t understand what our software is doing, then listed tactics to develop that understanding and an exercise to enhance our own debugging skills. Then we repurposed advice from _Philosophy of Software Design_ to mine recommendations for what to do once we have identified the cause of a bug.

### If you liked this piece, you might also like:

[The Leveling Up Series](http://chelseatroy.com/category/leveling-up) (I guess I’d call this series “popular”)

[The SICP series](http://chelseatroy.com/tag/sicp) (for the truly programming nerdery inclined)

[The Crafting Interpreters Series](http://chelseatroy.com/tag/crafting-interpreters) (ongoing)
