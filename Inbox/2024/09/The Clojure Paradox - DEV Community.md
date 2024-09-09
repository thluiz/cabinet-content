---
created: 2024-08-29T12:43:03 (UTC -03:00)
tags: [aws,clojure,python,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest
author: 
---

# The Clojure Paradox - DEV Community

> ## Excerpt
> Several years ago, I came across The Python Paradox by Paul Graham. In it, Graham argues that Lisp...

---
Several years ago, I came across The [Python Paradox](https://paulgraham.com/pypar.html) by Paul Graham. In it, Graham argues that **Lisp and Python programmers are often those who genuinely care about programming** and, as a result, tend to do it better than, for instance, Java programmers. This perspective fundamentally changed how I viewed Python as a language.

At first, I found Python’s lack of semicolons and reliance on indentation to be strange and uncomfortable. I even saw Python as a tool for building only basic applications. However, **with the rise of serverless computing, machine learning, and data science, the immense power of Python has become increasingly apparent**. The language is getting faster, and its ecosystem is rapidly growing. Libraries like FastAPI and Pandas are truly remarkable, allowing us to solve problems succinctly.

`As programmers, our job is to solve problems, and since we read more code than we write, having fewer lines of code reduces the surface area for bugs to hide and helps us avoid cognitive overload.   `

When I started working with AWS's Boto3, **I realized that tasks that previously took me 30 lines of Java could now be done in just 3 lines of Python**. It was mind-blowing. Don’t get me wrong, Java is still one of my favorite programming languages, and with its new release cadence, it’s only getting better. But the amount of ceremony required to accomplish basic tasks in Java is something sometimes we’d all prefer to avoid.

Recently, I've been experimenting with Go. Although it prides itself on simplicity, IMHO _I find it too verbose_. I know that excellent tools have been built with Go, and there are certain ideas and applications that should be developed with it. **Its compilation speed and efficient memory usage make Go a strong contender**, it might even be the best combination of developer experience and performance, which is becoming increasingly important in modern, cloud-native applications.

[![Clojure 1](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbzeomju06z8bdhfy57m1.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fbzeomju06z8bdhfy57m1.png)

However, after 10 years in the industry and having deployed applications in several languages, I remain a fan of Clojure. Despite its niche status, Clojure incorporates ideas from other languages, such as Go’s goroutines. It’s a Lisp dialect, inherently immutable, and designed with concurrency in mind. What stands out most to me is how Clojure allows you to **focus on solving problems without the burden of unnecessary ceremony**. The majority of the code is about the problem itself; it’s data-oriented, and I often find that it helps me enter a _Flow state_ (Happiness) where programming becomes truly enjoyable.

With Go, I currently have mixed feelings. While it brings many good ideas to the table in terms of concurrency and simplicity, I find that the codebases tend to be larger and more ceremonious. Clojure, on the other hand, tends to produce code that is less brittle and primarily composed of pure functions.

## [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#timeless-ideas)Timeless ideas

I’ve always been a fan of timeless ideas because they are often the most important and foundational, yet they are also the most overlooked and I feel that Clojure fully embraces of all them.

-   Programs composed mostly of pure functions are more robust and easier to test.
-   Immutability reduces complexity.
-   Smaller programs have fewer bugs.
-   Software development is fundamentally about composition.
-   We should minimize
-   Incidental complexity can make a system harder to understand, maintain, and extend.

## [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#code-examples)Code examples

In this example, I read a book from an online text file and perform basic processing to illustrate how the same problem can be approached in different programming languages and paradigms.

### [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#go)Go

**My opinion**: The absence of `sets` and `higher-order functions` makes the problem more difficult to solve. Basic tasks, such as filtering, often need to be done in an imperative manner.  

```
<span>package</span> <span>main</span>

<span>import</span> <span>(</span>
    <span>"fmt"</span>
    <span>"io"</span>
    <span>"net/http"</span>
    <span>"regexp"</span>
    <span>"sort"</span>
    <span>"strings"</span>
<span>)</span>

<span>var</span> <span>commonWords</span> <span>=</span> <span>map</span><span>[</span><span>string</span><span>]</span><span>struct</span><span>{}{</span>
    <span>"a"</span><span>:</span> <span>{},</span> <span>"able"</span><span>:</span> <span>{},</span> <span>"about"</span><span>:</span> <span>{},</span> <span>"across"</span><span>:</span> <span>{},</span> <span>"after"</span><span>:</span> <span>{},</span> <span>"all"</span><span>:</span> <span>{},</span> <span>"almost"</span><span>:</span> <span>{},</span> <span>"also"</span><span>:</span> <span>{},</span> <span>"am"</span><span>:</span> <span>{},</span> <span>"among"</span><span>:</span> <span>{},</span>
    <span>"an"</span><span>:</span> <span>{},</span> <span>"and"</span><span>:</span> <span>{},</span> <span>"any"</span><span>:</span> <span>{},</span> <span>"are"</span><span>:</span> <span>{},</span> <span>"as"</span><span>:</span> <span>{},</span> <span>"at"</span><span>:</span> <span>{},</span> <span>"be"</span><span>:</span> <span>{},</span> <span>"because"</span><span>:</span> <span>{},</span> <span>"been"</span><span>:</span> <span>{},</span> <span>"but"</span><span>:</span> <span>{},</span> <span>"by"</span><span>:</span> <span>{},</span>
    <span>"can"</span><span>:</span> <span>{},</span> <span>"cannot"</span><span>:</span> <span>{},</span> <span>"could"</span><span>:</span> <span>{},</span> <span>"dear"</span><span>:</span> <span>{},</span> <span>"did"</span><span>:</span> <span>{},</span> <span>"do"</span><span>:</span> <span>{},</span> <span>"does"</span><span>:</span> <span>{},</span> <span>"either"</span><span>:</span> <span>{},</span> <span>"else"</span><span>:</span> <span>{},</span> <span>"ever"</span><span>:</span> <span>{},</span>
    <span>"every"</span><span>:</span> <span>{},</span> <span>"for"</span><span>:</span> <span>{},</span> <span>"from"</span><span>:</span> <span>{},</span> <span>"get"</span><span>:</span> <span>{},</span> <span>"got"</span><span>:</span> <span>{},</span> <span>"had"</span><span>:</span> <span>{},</span> <span>"has"</span><span>:</span> <span>{},</span> <span>"have"</span><span>:</span> <span>{},</span> <span>"he"</span><span>:</span> <span>{},</span> <span>"her"</span><span>:</span> <span>{},</span> <span>"hers"</span><span>:</span> <span>{},</span>
    <span>"him"</span><span>:</span> <span>{},</span> <span>"his"</span><span>:</span> <span>{},</span> <span>"how"</span><span>:</span> <span>{},</span> <span>"however"</span><span>:</span> <span>{},</span> <span>"i"</span><span>:</span> <span>{},</span> <span>"if"</span><span>:</span> <span>{},</span> <span>"in"</span><span>:</span> <span>{},</span> <span>"into"</span><span>:</span> <span>{},</span> <span>"is"</span><span>:</span> <span>{},</span> <span>"it"</span><span>:</span> <span>{},</span> <span>"its"</span><span>:</span> <span>{},</span>
    <span>"just"</span><span>:</span> <span>{},</span> <span>"least"</span><span>:</span> <span>{},</span> <span>"let"</span><span>:</span> <span>{},</span> <span>"like"</span><span>:</span> <span>{},</span> <span>"likely"</span><span>:</span> <span>{},</span> <span>"may"</span><span>:</span> <span>{},</span> <span>"me"</span><span>:</span> <span>{},</span> <span>"might"</span><span>:</span> <span>{},</span> <span>"most"</span><span>:</span> <span>{},</span> <span>"must"</span><span>:</span> <span>{},</span>
    <span>"my"</span><span>:</span> <span>{},</span> <span>"neither"</span><span>:</span> <span>{},</span> <span>"no"</span><span>:</span> <span>{},</span> <span>"nor"</span><span>:</span> <span>{},</span> <span>"not"</span><span>:</span> <span>{},</span> <span>"of"</span><span>:</span> <span>{},</span> <span>"off"</span><span>:</span> <span>{},</span> <span>"often"</span><span>:</span> <span>{},</span> <span>"on"</span><span>:</span> <span>{},</span> <span>"only"</span><span>:</span> <span>{},</span> <span>"or"</span><span>:</span> <span>{},</span>
    <span>"other"</span><span>:</span> <span>{},</span> <span>"our"</span><span>:</span> <span>{},</span> <span>"own"</span><span>:</span> <span>{},</span> <span>"rather"</span><span>:</span> <span>{},</span> <span>"said"</span><span>:</span> <span>{},</span> <span>"says"</span><span>:</span> <span>{},</span> <span>"she"</span><span>:</span> <span>{},</span> <span>"should"</span><span>:</span> <span>{},</span> <span>"since"</span><span>:</span> <span>{},</span> <span>"so"</span><span>:</span> <span>{},</span>
    <span>"some"</span><span>:</span> <span>{},</span> <span>"than"</span><span>:</span> <span>{},</span> <span>"that"</span><span>:</span> <span>{},</span> <span>"the"</span><span>:</span> <span>{},</span> <span>"their"</span><span>:</span> <span>{},</span> <span>"them"</span><span>:</span> <span>{},</span> <span>"then"</span><span>:</span> <span>{},</span> <span>"there"</span><span>:</span> <span>{},</span> <span>"these"</span><span>:</span> <span>{},</span> <span>"they"</span><span>:</span> <span>{},</span>
    <span>"this"</span><span>:</span> <span>{},</span> <span>"those"</span><span>:</span> <span>{},</span> <span>"through"</span><span>:</span> <span>{},</span> <span>"to"</span><span>:</span> <span>{},</span> <span>"too"</span><span>:</span> <span>{},</span> <span>"more"</span><span>:</span> <span>{},</span> <span>"upon"</span><span>:</span> <span>{},</span> <span>"us"</span><span>:</span> <span>{},</span> <span>"wants"</span><span>:</span> <span>{},</span> <span>"was"</span><span>:</span> <span>{},</span>
    <span>"we"</span><span>:</span> <span>{},</span> <span>"were"</span><span>:</span> <span>{},</span> <span>"what"</span><span>:</span> <span>{},</span> <span>"when"</span><span>:</span> <span>{},</span> <span>"where"</span><span>:</span> <span>{},</span> <span>"which"</span><span>:</span> <span>{},</span> <span>"while"</span><span>:</span> <span>{},</span> <span>"who"</span><span>:</span> <span>{},</span> <span>"whom"</span><span>:</span> <span>{},</span> <span>"why"</span><span>:</span> <span>{},</span>
    <span>"will"</span><span>:</span> <span>{},</span> <span>"with"</span><span>:</span> <span>{},</span> <span>"would"</span><span>:</span> <span>{},</span> <span>"yet"</span><span>:</span> <span>{},</span> <span>"you"</span><span>:</span> <span>{},</span> <span>"your"</span><span>:</span> <span>{},</span> <span>"shall"</span><span>:</span> <span>{},</span> <span>"before"</span><span>:</span> <span>{},</span> <span>"now"</span><span>:</span> <span>{},</span> <span>"one"</span><span>:</span> <span>{},</span>
    <span>"even"</span><span>:</span> <span>{},</span>
<span>}</span>

<span>func</span> <span>getBook</span><span>()</span> <span>string</span> <span>{</span>
    <span>resp</span><span>,</span> <span>err</span> <span>:=</span> <span>http</span><span>.</span><span>Get</span><span>(</span><span>"https://www.gutenberg.org/cache/epub/84/pg84.txt"</span><span>)</span>
    <span>if</span> <span>err</span> <span>!=</span> <span>nil</span> <span>{</span>
        <span>panic</span><span>(</span><span>err</span><span>)</span>
    <span>}</span>
    <span>defer</span> <span>resp</span><span>.</span><span>Body</span><span>.</span><span>Close</span><span>()</span>

    <span>body</span><span>,</span> <span>err</span> <span>:=</span> <span>io</span><span>.</span><span>ReadAll</span><span>(</span><span>resp</span><span>.</span><span>Body</span><span>)</span>
    <span>if</span> <span>err</span> <span>!=</span> <span>nil</span> <span>{</span>
        <span>panic</span><span>(</span><span>err</span><span>)</span>
    <span>}</span>
    <span>return</span> <span>string</span><span>(</span><span>body</span><span>)</span>
<span>}</span>

<span>func</span> <span>getWords</span><span>(</span><span>book</span> <span>string</span><span>)</span> <span>[]</span><span>string</span> <span>{</span>
    <span>re</span> <span>:=</span> <span>regexp</span><span>.</span><span>MustCompile</span><span>(</span><span>`[\w’]+`</span><span>)</span>
    <span>return</span> <span>re</span><span>.</span><span>FindAllString</span><span>(</span><span>book</span><span>,</span> <span>-</span><span>1</span><span>)</span>
<span>}</span>

<span>func</span> <span>filterWords</span><span>(</span><span>words</span> <span>[]</span><span>string</span><span>)</span> <span>[]</span><span>string</span> <span>{</span>
    <span>var</span> <span>result</span> <span>[]</span><span>string</span>
    <span>for</span> <span>_</span><span>,</span> <span>word</span> <span>:=</span> <span>range</span> <span>words</span> <span>{</span>
        <span>w</span> <span>:=</span> <span>strings</span><span>.</span><span>ToLower</span><span>(</span><span>word</span><span>)</span>
        <span>_</span><span>,</span> <span>ok</span> <span>:=</span> <span>commonWords</span><span>[</span><span>w</span><span>]</span>
        <span>if</span> <span>!</span><span>ok</span> <span>{</span>
            <span>result</span> <span>=</span> <span>append</span><span>(</span><span>result</span><span>,</span> <span>w</span><span>)</span>
        <span>}</span>
    <span>}</span>
    <span>return</span> <span>result</span>
<span>}</span>

<span>func</span> <span>getFrequentWords</span><span>(</span><span>words</span> <span>[]</span><span>string</span><span>,</span> <span>n</span> <span>int</span><span>)</span> <span>map</span><span>[</span><span>string</span><span>]</span><span>int</span> <span>{</span>
    <span>var</span> <span>filteredWords</span> <span>[]</span><span>string</span>
    <span>for</span> <span>_</span><span>,</span> <span>word</span> <span>:=</span> <span>range</span> <span>words</span> <span>{</span>
        <span>_</span><span>,</span> <span>ok</span> <span>:=</span> <span>commonWords</span><span>[</span><span>word</span><span>]</span>
        <span>if</span> <span>!</span><span>ok</span> <span>{</span>
            <span>filteredWords</span> <span>=</span> <span>append</span><span>(</span><span>filteredWords</span><span>,</span> <span>strings</span><span>.</span><span>ToLower</span><span>(</span><span>word</span><span>))</span>
        <span>}</span>
    <span>}</span>
    <span>var</span> <span>unorderedWords</span> <span>=</span> <span>make</span><span>(</span><span>map</span><span>[</span><span>string</span><span>]</span><span>int</span><span>)</span>
    <span>for</span> <span>_</span><span>,</span> <span>word</span> <span>:=</span> <span>range</span> <span>words</span> <span>{</span>
        <span>unorderedWords</span><span>[</span><span>word</span><span>]</span><span>++</span>
    <span>}</span>
    <span>type</span> <span>wordFrequency</span> <span>struct</span> <span>{</span>
        <span>word</span>  <span>string</span>
        <span>count</span> <span>int</span>
    <span>}</span>
    <span>var</span> <span>wordFrequencies</span> <span>[]</span><span>wordFrequency</span>
    <span>for</span> <span>word</span><span>,</span> <span>count</span> <span>:=</span> <span>range</span> <span>unorderedWords</span> <span>{</span>
        <span>wordFrequencies</span> <span>=</span> <span>append</span><span>(</span><span>wordFrequencies</span><span>,</span> <span>wordFrequency</span><span>{</span><span>word</span><span>,</span> <span>count</span><span>})</span>
    <span>}</span>
    <span>sort</span><span>.</span><span>Slice</span><span>(</span><span>wordFrequencies</span><span>,</span> <span>func</span><span>(</span><span>i</span><span>,</span> <span>j</span> <span>int</span><span>)</span> <span>bool</span> <span>{</span>
        <span>return</span> <span>wordFrequencies</span><span>[</span><span>i</span><span>]</span><span>.</span><span>count</span> <span>&gt;</span> <span>wordFrequencies</span><span>[</span><span>j</span><span>]</span><span>.</span><span>count</span>
    <span>})</span>

    <span>topN</span> <span>:=</span> <span>make</span><span>(</span><span>map</span><span>[</span><span>string</span><span>]</span><span>int</span><span>)</span>
    <span>for</span> <span>i</span> <span>:=</span> <span>0</span><span>;</span> <span>i</span> <span>&lt;</span> <span>len</span><span>(</span><span>wordFrequencies</span><span>)</span> <span>&amp;&amp;</span> <span>i</span> <span>&lt;</span> <span>n</span><span>;</span> <span>i</span><span>++</span> <span>{</span>
        <span>topN</span><span>[</span><span>wordFrequencies</span><span>[</span><span>i</span><span>]</span><span>.</span><span>word</span><span>]</span> <span>=</span> <span>wordFrequencies</span><span>[</span><span>i</span><span>]</span><span>.</span><span>count</span>
    <span>}</span>

    <span>return</span> <span>topN</span>
<span>}</span>

<span>func</span> <span>main</span><span>()</span> <span>{</span>
    <span>book</span> <span>:=</span> <span>getBook</span><span>()</span>
    <span>words</span> <span>:=</span> <span>getWords</span><span>(</span><span>book</span><span>)</span>
    <span>filteredWords</span> <span>:=</span> <span>filterWords</span><span>(</span><span>words</span><span>)</span>
    <span>fmt</span><span>.</span><span>Println</span><span>(</span><span>"Total words:"</span><span>,</span> <span>len</span><span>(</span><span>words</span><span>))</span>
    <span>fmt</span><span>.</span><span>Println</span><span>(</span><span>"Frequent words:"</span><span>,</span> <span>getFrequentWords</span><span>(</span><span>filteredWords</span><span>,</span> <span>10</span><span>))</span>

<span>}</span>

```

### [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#java)Java

**My opinion**: It does the job. Since Java 8, the language has been getting better. Even though it has some verbosity, you'll find that we now have collectors and functions to perform tasks without issues. The tedious part is having to put everything into classes just to solve a problem.  

```
<span>package</span> <span>jorgetovar.book</span><span>;</span>

<span>import</span> <span>org.springframework.web.client.RestTemplate</span><span>;</span>

<span>import</span> <span>java.util.*</span><span>;</span>
<span>import</span> <span>java.util.regex.Matcher</span><span>;</span>
<span>import</span> <span>java.util.regex.Pattern</span><span>;</span>
<span>import</span> <span>java.util.stream.Collectors</span><span>;</span>

<span>public</span> <span>class</span> <span>Book</span> <span>{</span>

    <span>private</span> <span>static</span> <span>final</span> <span>Set</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>commonWords</span> <span>=</span> <span>Set</span><span>.</span><span>of</span><span>(</span>
            <span>"a"</span><span>,</span> <span>"able"</span><span>,</span> <span>"about"</span><span>,</span> <span>"across"</span><span>,</span> <span>"after"</span><span>,</span> <span>"all"</span><span>,</span> <span>"almost"</span><span>,</span> <span>"also"</span><span>,</span> <span>"am"</span><span>,</span> <span>"among"</span><span>,</span> <span>"an"</span><span>,</span>
            <span>"and"</span><span>,</span> <span>"any"</span><span>,</span> <span>"are"</span><span>,</span> <span>"as"</span><span>,</span> <span>"at"</span><span>,</span> <span>"be"</span><span>,</span> <span>"because"</span><span>,</span> <span>"been"</span><span>,</span> <span>"but"</span><span>,</span> <span>"by"</span><span>,</span> <span>"can"</span><span>,</span> <span>"cannot"</span><span>,</span>
            <span>"could"</span><span>,</span> <span>"dear"</span><span>,</span> <span>"did"</span><span>,</span> <span>"do"</span><span>,</span> <span>"does"</span><span>,</span> <span>"either"</span><span>,</span> <span>"else"</span><span>,</span> <span>"ever"</span><span>,</span> <span>"every"</span><span>,</span> <span>"for"</span><span>,</span> <span>"from"</span><span>,</span>
            <span>"get"</span><span>,</span> <span>"got"</span><span>,</span> <span>"had"</span><span>,</span> <span>"has"</span><span>,</span> <span>"have"</span><span>,</span> <span>"he"</span><span>,</span> <span>"her"</span><span>,</span> <span>"hers"</span><span>,</span> <span>"him"</span><span>,</span> <span>"his"</span><span>,</span> <span>"how"</span><span>,</span> <span>"however"</span><span>,</span>
            <span>"i"</span><span>,</span> <span>"if"</span><span>,</span> <span>"in"</span><span>,</span> <span>"into"</span><span>,</span> <span>"is"</span><span>,</span> <span>"it"</span><span>,</span> <span>"its"</span><span>,</span> <span>"just"</span><span>,</span> <span>"least"</span><span>,</span> <span>"let"</span><span>,</span> <span>"like"</span><span>,</span> <span>"likely"</span><span>,</span>
            <span>"may"</span><span>,</span> <span>"me"</span><span>,</span> <span>"might"</span><span>,</span> <span>"most"</span><span>,</span> <span>"must"</span><span>,</span> <span>"my"</span><span>,</span> <span>"neither"</span><span>,</span> <span>"no"</span><span>,</span> <span>"nor"</span><span>,</span> <span>"not"</span><span>,</span> <span>"of"</span><span>,</span> <span>"off"</span><span>,</span>
            <span>"often"</span><span>,</span> <span>"on"</span><span>,</span> <span>"only"</span><span>,</span> <span>"or"</span><span>,</span> <span>"other"</span><span>,</span> <span>"our"</span><span>,</span> <span>"own"</span><span>,</span> <span>"rather"</span><span>,</span> <span>"said"</span><span>,</span> <span>"says"</span><span>,</span> <span>"she"</span><span>,</span>
            <span>"should"</span><span>,</span> <span>"since"</span><span>,</span> <span>"so"</span><span>,</span> <span>"some"</span><span>,</span> <span>"than"</span><span>,</span> <span>"that"</span><span>,</span> <span>"the"</span><span>,</span> <span>"their"</span><span>,</span> <span>"them"</span><span>,</span> <span>"then"</span><span>,</span>
            <span>"there"</span><span>,</span> <span>"these"</span><span>,</span> <span>"they"</span><span>,</span> <span>"this"</span><span>,</span> <span>"those"</span><span>,</span> <span>"through"</span><span>,</span> <span>"to"</span><span>,</span> <span>"too"</span><span>,</span> <span>"more"</span><span>,</span> <span>"upon"</span><span>,</span>
            <span>"us"</span><span>,</span> <span>"wants"</span><span>,</span> <span>"was"</span><span>,</span> <span>"we"</span><span>,</span> <span>"were"</span><span>,</span> <span>"what"</span><span>,</span> <span>"when"</span><span>,</span> <span>"where"</span><span>,</span> <span>"which"</span><span>,</span> <span>"while"</span><span>,</span> <span>"who"</span><span>,</span>
            <span>"whom"</span><span>,</span> <span>"why"</span><span>,</span> <span>"will"</span><span>,</span> <span>"with"</span><span>,</span> <span>"would"</span><span>,</span> <span>"yet"</span><span>,</span> <span>"you"</span><span>,</span> <span>"your"</span><span>,</span> <span>"shall"</span><span>,</span> <span>"before"</span><span>,</span> <span>"now"</span><span>,</span> <span>"one"</span><span>,</span>
            <span>"even"</span>
    <span>);</span>

    <span>public</span> <span>static</span> <span>String</span> <span>getBook</span><span>()</span> <span>{</span>
        <span>RestTemplate</span> <span>restTemplate</span> <span>=</span> <span>new</span> <span>RestTemplate</span><span>();</span>
        <span>String</span> <span>bookUrl</span> <span>=</span> <span>"https://www.gutenberg.org/cache/epub/84/pg84.txt"</span><span>;</span>
        <span>return</span> <span>restTemplate</span><span>.</span><span>getForObject</span><span>(</span><span>bookUrl</span><span>,</span> <span>String</span><span>.</span><span>class</span><span>);</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>getWords</span><span>(</span><span>String</span> <span>book</span><span>)</span> <span>{</span>
        <span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>words</span> <span>=</span> <span>new</span> <span>ArrayList</span><span>&lt;&gt;();</span>
        <span>Pattern</span> <span>wordPattern</span> <span>=</span> <span>Pattern</span><span>.</span><span>compile</span><span>(</span><span>"[\\w’]+"</span><span>);</span>
        <span>Matcher</span> <span>matcher</span> <span>=</span> <span>wordPattern</span><span>.</span><span>matcher</span><span>(</span><span>book</span><span>);</span>
        <span>while</span> <span>(</span><span>matcher</span><span>.</span><span>find</span><span>())</span> <span>{</span>
            <span>words</span><span>.</span><span>add</span><span>(</span><span>matcher</span><span>.</span><span>group</span><span>());</span>
        <span>}</span>
        <span>return</span> <span>words</span><span>;</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>List</span><span>&lt;</span><span>Map</span><span>.</span><span>Entry</span><span>&lt;</span><span>String</span><span>,</span> <span>Long</span><span>&gt;&gt;</span> <span>getFrequentWords</span><span>(</span><span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>words</span><span>,</span> <span>int</span> <span>takeN</span><span>)</span> <span>{</span>
        <span>return</span> <span>words</span><span>.</span><span>stream</span><span>()</span>
                <span>.</span><span>map</span><span>(</span><span>String:</span><span>:</span><span>toLowerCase</span><span>)</span>
                <span>.</span><span>filter</span><span>(</span><span>word</span> <span>-&gt;</span> <span>!</span><span>commonWords</span><span>.</span><span>contains</span><span>(</span><span>word</span><span>))</span>
                <span>.</span><span>collect</span><span>(</span><span>Collectors</span><span>.</span><span>groupingBy</span><span>(</span><span>word</span> <span>-&gt;</span> <span>word</span><span>,</span> <span>Collectors</span><span>.</span><span>counting</span><span>()))</span>
                <span>.</span><span>entrySet</span><span>()</span>
                <span>.</span><span>stream</span><span>()</span>
                <span>.</span><span>sorted</span><span>((</span><span>e1</span><span>,</span> <span>e2</span><span>)</span> <span>-&gt;</span> <span>Long</span><span>.</span><span>compare</span><span>(</span><span>e2</span><span>.</span><span>getValue</span><span>(),</span> <span>e1</span><span>.</span><span>getValue</span><span>()))</span>
                <span>.</span><span>limit</span><span>(</span><span>takeN</span><span>)</span>
                <span>.</span><span>map</span><span>(</span><span>e</span> <span>-&gt;</span> <span>Map</span><span>.</span><span>entry</span><span>(</span><span>e</span><span>.</span><span>getKey</span><span>(),</span> <span>e</span><span>.</span><span>getValue</span><span>()))</span>
                <span>.</span><span>toList</span><span>();</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>Map</span><span>&lt;</span><span>Integer</span><span>,</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;&gt;</span> <span>getLongestWords</span><span>(</span><span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>words</span><span>,</span> <span>int</span> <span>takeN</span><span>)</span> <span>{</span>
        <span>return</span> <span>words</span><span>.</span><span>stream</span><span>()</span>
                <span>.</span><span>map</span><span>(</span><span>String:</span><span>:</span><span>toLowerCase</span><span>)</span>
                <span>.</span><span>distinct</span><span>()</span>
                <span>.</span><span>sorted</span><span>(</span><span>getLongestWord</span><span>())</span>
                <span>.</span><span>limit</span><span>(</span><span>takeN</span><span>)</span>
                <span>.</span><span>collect</span><span>(</span><span>Collectors</span><span>.</span><span>groupingBy</span><span>(</span><span>String:</span><span>:</span><span>length</span><span>));</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>boolean</span> <span>isPalindrome</span><span>(</span><span>String</span> <span>word</span><span>)</span> <span>{</span>
        <span>return</span> <span>word</span><span>.</span><span>contentEquals</span><span>(</span><span>new</span> <span>StringBuilder</span><span>(</span><span>word</span><span>).</span><span>reverse</span><span>());</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>getLongestPalindromes</span><span>(</span><span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>words</span><span>,</span> <span>int</span> <span>takeN</span><span>)</span> <span>{</span>
        <span>return</span> <span>words</span><span>.</span><span>stream</span><span>()</span>
                <span>.</span><span>map</span><span>(</span><span>String:</span><span>:</span><span>toLowerCase</span><span>)</span>
                <span>.</span><span>filter</span><span>(</span><span>word</span> <span>-&gt;</span> <span>!</span><span>commonWords</span><span>.</span><span>contains</span><span>(</span><span>word</span><span>))</span>
                <span>.</span><span>distinct</span><span>()</span>
                <span>.</span><span>filter</span><span>(</span><span>Book:</span><span>:</span><span>isPalindrome</span><span>)</span>
                <span>.</span><span>sorted</span><span>(</span><span>getLongestWord</span><span>())</span>
                <span>.</span><span>limit</span><span>(</span><span>takeN</span><span>)</span>
                <span>.</span><span>toList</span><span>();</span>
    <span>}</span>

    <span>private</span> <span>static</span> <span>Comparator</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>getLongestWord</span><span>()</span> <span>{</span>
        <span>return</span> <span>Comparator</span><span>.</span><span>comparingInt</span><span>(</span><span>String:</span><span>:</span><span>length</span><span>).</span><span>reversed</span><span>();</span>
    <span>}</span>

    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>String</span> <span>book</span> <span>=</span> <span>getBook</span><span>();</span>
        <span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>words</span> <span>=</span> <span>getWords</span><span>(</span><span>book</span><span>);</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Total Words: "</span> <span>+</span> <span>words</span><span>.</span><span>size</span><span>());</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Most Frequent Words:"</span><span>);</span>
        <span>getFrequentWords</span><span>(</span><span>words</span><span>,</span> <span>10</span><span>).</span><span>forEach</span><span>(</span><span>entry</span> <span>-&gt;</span> <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>entry</span><span>.</span><span>getKey</span><span>()</span> <span>+</span> <span>": "</span> <span>+</span> <span>entry</span><span>.</span><span>getValue</span><span>()));</span>

        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"\nLongest Words Grouped by Length:"</span><span>);</span>
        <span>getLongestWords</span><span>(</span><span>words</span><span>,</span> <span>10</span><span>).</span><span>forEach</span><span>((</span><span>length</span><span>,</span> <span>group</span><span>)</span> <span>-&gt;</span> <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Length "</span> <span>+</span> <span>length</span> <span>+</span> <span>": "</span> <span>+</span> <span>group</span><span>));</span>

        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"\nLongest Palindromes:"</span><span>);</span>
        <span>getLongestPalindromes</span><span>(</span><span>words</span><span>,</span> <span>3</span><span>).</span><span>forEach</span><span>(</span><span>System</span><span>.</span><span>out</span><span>::</span><span>println</span><span>);</span>
    <span>}</span>


<span>}</span>

```

### [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#kotlin)Kotlin

**My opinion**: For me, this could be the most fun and robust enterprise language. It has good support for functions and immutability.  

```
<span>package</span> <span>jorgetovar.book</span>

<span>import</span> <span>org.springframework.web.client.RestTemplate</span>


<span>val</span> <span>commonWords</span> <span>=</span> <span>setOf</span><span>(</span>
    <span>"a"</span><span>,</span> <span>"able"</span><span>,</span> <span>"about"</span><span>,</span> <span>"across"</span><span>,</span> <span>"after"</span><span>,</span> <span>"all"</span><span>,</span> <span>"almost"</span><span>,</span> <span>"also"</span><span>,</span> <span>"am"</span><span>,</span> <span>"among"</span><span>,</span> <span>"an"</span><span>,</span>
    <span>"and"</span><span>,</span> <span>"any"</span><span>,</span> <span>"are"</span><span>,</span> <span>"as"</span><span>,</span> <span>"at"</span><span>,</span> <span>"be"</span><span>,</span> <span>"because"</span><span>,</span> <span>"been"</span><span>,</span> <span>"but"</span><span>,</span> <span>"by"</span><span>,</span> <span>"can"</span><span>,</span> <span>"cannot"</span><span>,</span>
    <span>"could"</span><span>,</span> <span>"dear"</span><span>,</span> <span>"did"</span><span>,</span> <span>"do"</span><span>,</span> <span>"does"</span><span>,</span> <span>"either"</span><span>,</span> <span>"else"</span><span>,</span> <span>"ever"</span><span>,</span> <span>"every"</span><span>,</span> <span>"for"</span><span>,</span> <span>"from"</span><span>,</span>
    <span>"get"</span><span>,</span> <span>"got"</span><span>,</span> <span>"had"</span><span>,</span> <span>"has"</span><span>,</span> <span>"have"</span><span>,</span> <span>"he"</span><span>,</span> <span>"her"</span><span>,</span> <span>"hers"</span><span>,</span> <span>"him"</span><span>,</span> <span>"his"</span><span>,</span> <span>"how"</span><span>,</span> <span>"however"</span><span>,</span>
    <span>"i"</span><span>,</span> <span>"if"</span><span>,</span> <span>"in"</span><span>,</span> <span>"into"</span><span>,</span> <span>"is"</span><span>,</span> <span>"it"</span><span>,</span> <span>"its"</span><span>,</span> <span>"just"</span><span>,</span> <span>"least"</span><span>,</span> <span>"let"</span><span>,</span> <span>"like"</span><span>,</span> <span>"likely"</span><span>,</span>
    <span>"may"</span><span>,</span> <span>"me"</span><span>,</span> <span>"might"</span><span>,</span> <span>"most"</span><span>,</span> <span>"must"</span><span>,</span> <span>"my"</span><span>,</span> <span>"neither"</span><span>,</span> <span>"no"</span><span>,</span> <span>"nor"</span><span>,</span> <span>"not"</span><span>,</span> <span>"of"</span><span>,</span> <span>"off"</span><span>,</span>
    <span>"often"</span><span>,</span> <span>"on"</span><span>,</span> <span>"only"</span><span>,</span> <span>"or"</span><span>,</span> <span>"other"</span><span>,</span> <span>"our"</span><span>,</span> <span>"own"</span><span>,</span> <span>"rather"</span><span>,</span> <span>"said"</span><span>,</span> <span>"says"</span><span>,</span> <span>"she"</span><span>,</span>
    <span>"should"</span><span>,</span> <span>"since"</span><span>,</span> <span>"so"</span><span>,</span> <span>"some"</span><span>,</span> <span>"than"</span><span>,</span> <span>"that"</span><span>,</span> <span>"the"</span><span>,</span> <span>"their"</span><span>,</span> <span>"them"</span><span>,</span> <span>"then"</span><span>,</span>
    <span>"there"</span><span>,</span> <span>"these"</span><span>,</span> <span>"they"</span><span>,</span> <span>"this"</span><span>,</span> <span>"those"</span><span>,</span> <span>"through"</span><span>,</span> <span>"to"</span><span>,</span> <span>"too"</span><span>,</span> <span>"more"</span><span>,</span> <span>"upon"</span><span>,</span>
    <span>"us"</span><span>,</span> <span>"wants"</span><span>,</span> <span>"was"</span><span>,</span> <span>"we"</span><span>,</span> <span>"were"</span><span>,</span> <span>"what"</span><span>,</span> <span>"when"</span><span>,</span> <span>"where"</span><span>,</span> <span>"which"</span><span>,</span> <span>"while"</span><span>,</span> <span>"who"</span><span>,</span>
    <span>"whom"</span><span>,</span> <span>"why"</span><span>,</span> <span>"will"</span><span>,</span> <span>"with"</span><span>,</span> <span>"would"</span><span>,</span> <span>"yet"</span><span>,</span> <span>"you"</span><span>,</span> <span>"your"</span><span>,</span> <span>"shall"</span><span>,</span> <span>"before"</span><span>,</span> <span>"now"</span><span>,</span> <span>"one"</span><span>,</span>
    <span>"even"</span>
<span>)</span>

<span>fun</span> <span>getBook</span><span>():</span> <span>String</span> <span>{</span>
    <span>val</span> <span>restTemplate</span> <span>=</span> <span>RestTemplate</span><span>()</span>
    <span>val</span> <span>bookUrl</span> <span>=</span> <span>"https://www.gutenberg.org/cache/epub/84/pg84.txt"</span>
    <span>return</span> <span>restTemplate</span><span>.</span><span>getForObject</span><span>(</span><span>bookUrl</span><span>,</span> <span>String</span><span>::</span><span>class</span><span>.</span><span>java</span><span>)</span> <span>?:</span> <span>""</span>
<span>}</span>

<span>fun</span> <span>getWords</span><span>(</span><span>book</span><span>:</span> <span>String</span><span>):</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>{</span>
    <span>return</span> <span>"[\\w’]+"</span><span>.</span><span>toRegex</span><span>().</span><span>findAll</span><span>(</span><span>book</span><span>).</span><span>map</span> <span>{</span> <span>it</span><span>.</span><span>value</span> <span>}.</span><span>toList</span><span>()</span>
<span>}</span>

<span>fun</span> <span>getFrequentWords</span><span>(</span><span>words</span><span>:</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;,</span> <span>takeN</span><span>:</span> <span>Int</span><span>):</span> <span>List</span><span>&lt;</span><span>Pair</span><span>&lt;</span><span>String</span><span>,</span> <span>Int</span><span>&gt;&gt;</span> <span>{</span>
    <span>val</span> <span>filteredWords</span> <span>=</span> <span>words</span>
        <span>.</span><span>map</span> <span>{</span> <span>it</span><span>.</span><span>lowercase</span><span>()</span> <span>}</span>
        <span>.</span><span>filter</span> <span>{</span> <span>it</span> <span>!</span><span>in</span> <span>commonWords</span> <span>}</span>

    <span>return</span> <span>filteredWords</span>
        <span>.</span><span>groupingBy</span> <span>{</span> <span>it</span> <span>}</span>
        <span>.</span><span>eachCount</span><span>()</span>
        <span>.</span><span>toList</span><span>()</span>
        <span>.</span><span>sortedByDescending</span> <span>{</span> <span>it</span><span>.</span><span>second</span> <span>}</span>
        <span>.</span><span>take</span><span>(</span><span>takeN</span><span>)</span>

<span>}</span>

<span>fun</span> <span>getLongestWords</span><span>(</span><span>words</span><span>:</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;,</span> <span>takeN</span><span>:</span> <span>Int</span><span>):</span> <span>Map</span><span>&lt;</span><span>Int</span><span>,</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;&gt;</span> <span>{</span>
    <span>val</span> <span>uniqueWords</span> <span>=</span> <span>words</span>
        <span>.</span><span>map</span> <span>{</span> <span>it</span><span>.</span><span>lowercase</span><span>()</span> <span>}</span>
        <span>.</span><span>distinct</span><span>()</span>
    <span>return</span> <span>uniqueWords</span>
        <span>.</span><span>sortedByDescending</span> <span>{</span> <span>it</span><span>.</span><span>length</span> <span>}</span>
        <span>.</span><span>take</span><span>(</span><span>takeN</span><span>)</span>
        <span>.</span><span>groupBy</span> <span>{</span> <span>it</span><span>.</span><span>length</span> <span>}</span>
<span>}</span>

<span>fun</span> <span>isPalindrome</span><span>(</span><span>word</span><span>:</span> <span>String</span><span>):</span> <span>Boolean</span> <span>{</span>
    <span>return</span> <span>word</span> <span>==</span> <span>word</span><span>.</span><span>reversed</span><span>()</span>
<span>}</span>

<span>fun</span> <span>getLongestPalindromes</span><span>(</span><span>words</span><span>:</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;,</span> <span>takeN</span><span>:</span> <span>Int</span><span>):</span> <span>List</span><span>&lt;</span><span>String</span><span>&gt;</span> <span>{</span>
    <span>val</span> <span>uniqueWords</span> <span>=</span> <span>words</span>
        <span>.</span><span>map</span> <span>{</span> <span>it</span><span>.</span><span>lowercase</span><span>()</span> <span>}</span>
        <span>.</span><span>filter</span> <span>{</span> <span>it</span> <span>!</span><span>in</span> <span>commonWords</span> <span>}</span>
        <span>.</span><span>distinct</span><span>()</span>
    <span>val</span> <span>palindromes</span> <span>=</span> <span>uniqueWords</span>
        <span>.</span><span>filter</span> <span>{</span> <span>isPalindrome</span><span>(</span><span>it</span><span>)</span> <span>}</span>
    <span>return</span> <span>palindromes</span>
        <span>.</span><span>sortedByDescending</span> <span>{</span> <span>it</span><span>.</span><span>length</span> <span>}.</span><span>take</span><span>(</span><span>takeN</span><span>)</span>
<span>}</span>


<span>fun</span> <span>main</span><span>()</span> <span>{</span>
    <span>val</span> <span>book</span> <span>=</span> <span>getBook</span><span>()</span>
    <span>val</span> <span>words</span> <span>=</span> <span>getWords</span><span>(</span><span>book</span><span>)</span>
    <span>println</span><span>(</span><span>"Total Words: ${words.size}"</span><span>)</span>
    <span>println</span><span>(</span><span>"Most Frequent Words:"</span><span>)</span>
    <span>println</span><span>(</span><span>getFrequentWords</span><span>(</span><span>words</span><span>,</span> <span>10</span><span>))</span>
    <span>println</span><span>(</span><span>"Longest Words Grouped by Length:"</span><span>)</span>
    <span>println</span><span>(</span><span>getLongestWords</span><span>(</span><span>words</span><span>,</span> <span>5</span><span>))</span>
    <span>println</span><span>(</span><span>"Longest Palindromes:"</span><span>)</span>
    <span>println</span><span>(</span><span>getLongestPalindromes</span><span>(</span><span>words</span><span>,</span> <span>3</span><span>))</span>
<span>}</span>

```

### [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#python)Python

**My opinion**: I really like using this language. Sometimes it can get messy because it's too permissive and allows you to mutate variables, etc. But in general, you'll find that list comprehensions are really good for solving these kinds of problems. I don’t like the result when using classes, but for this example, it was just enough.  

```
<span>import</span> <span>requests</span>
<span>import</span> <span>re</span>

<span>from</span> <span>collections</span> <span>import</span> <span>Counter</span><span>,</span> <span>defaultdict</span>


<span>def</span> <span>get_book</span><span>():</span>
    <span>book</span> <span>=</span> <span>requests</span><span>.</span><span>get</span><span>(</span><span>"</span><span>https://www.gutenberg.org/cache/epub/84/pg84.txt</span><span>"</span><span>)</span>
    <span>return</span> <span>book</span><span>.</span><span>text</span>


<span>def</span> <span>get_words</span><span>(</span><span>book</span><span>):</span>
    <span>return</span> <span>re</span><span>.</span><span>findall</span><span>(</span><span>r</span><span>"</span><span>[a-zA-Z0-9’]+</span><span>"</span><span>,</span> <span>book</span><span>)</span>


<span>common_words</span> <span>=</span> <span>{</span>
    <span>"</span><span>a</span><span>"</span><span>,</span> <span>"</span><span>able</span><span>"</span><span>,</span> <span>"</span><span>about</span><span>"</span><span>,</span> <span>"</span><span>across</span><span>"</span><span>,</span> <span>"</span><span>after</span><span>"</span><span>,</span> <span>"</span><span>all</span><span>"</span><span>,</span> <span>"</span><span>almost</span><span>"</span><span>,</span> <span>"</span><span>also</span><span>"</span><span>,</span> <span>"</span><span>am</span><span>"</span><span>,</span> <span>"</span><span>among</span><span>"</span><span>,</span> <span>"</span><span>an</span><span>"</span><span>,</span>
    <span>"</span><span>and</span><span>"</span><span>,</span> <span>"</span><span>any</span><span>"</span><span>,</span> <span>"</span><span>are</span><span>"</span><span>,</span> <span>"</span><span>as</span><span>"</span><span>,</span> <span>"</span><span>at</span><span>"</span><span>,</span> <span>"</span><span>be</span><span>"</span><span>,</span> <span>"</span><span>because</span><span>"</span><span>,</span> <span>"</span><span>been</span><span>"</span><span>,</span> <span>"</span><span>but</span><span>"</span><span>,</span> <span>"</span><span>by</span><span>"</span><span>,</span> <span>"</span><span>can</span><span>"</span><span>,</span> <span>"</span><span>cannot</span><span>"</span><span>,</span>
    <span>"</span><span>could</span><span>"</span><span>,</span> <span>"</span><span>dear</span><span>"</span><span>,</span> <span>"</span><span>did</span><span>"</span><span>,</span> <span>"</span><span>do</span><span>"</span><span>,</span> <span>"</span><span>does</span><span>"</span><span>,</span> <span>"</span><span>either</span><span>"</span><span>,</span> <span>"</span><span>else</span><span>"</span><span>,</span> <span>"</span><span>ever</span><span>"</span><span>,</span> <span>"</span><span>every</span><span>"</span><span>,</span> <span>"</span><span>for</span><span>"</span><span>,</span> <span>"</span><span>from</span><span>"</span><span>,</span>
    <span>"</span><span>get</span><span>"</span><span>,</span> <span>"</span><span>got</span><span>"</span><span>,</span> <span>"</span><span>had</span><span>"</span><span>,</span> <span>"</span><span>has</span><span>"</span><span>,</span> <span>"</span><span>have</span><span>"</span><span>,</span> <span>"</span><span>he</span><span>"</span><span>,</span> <span>"</span><span>her</span><span>"</span><span>,</span> <span>"</span><span>hers</span><span>"</span><span>,</span> <span>"</span><span>him</span><span>"</span><span>,</span> <span>"</span><span>his</span><span>"</span><span>,</span> <span>"</span><span>how</span><span>"</span><span>,</span> <span>"</span><span>however</span><span>"</span><span>,</span>
    <span>"</span><span>i</span><span>"</span><span>,</span> <span>"</span><span>if</span><span>"</span><span>,</span> <span>"</span><span>in</span><span>"</span><span>,</span> <span>"</span><span>into</span><span>"</span><span>,</span> <span>"</span><span>is</span><span>"</span><span>,</span> <span>"</span><span>it</span><span>"</span><span>,</span> <span>"</span><span>its</span><span>"</span><span>,</span> <span>"</span><span>just</span><span>"</span><span>,</span> <span>"</span><span>least</span><span>"</span><span>,</span> <span>"</span><span>let</span><span>"</span><span>,</span> <span>"</span><span>like</span><span>"</span><span>,</span> <span>"</span><span>likely</span><span>"</span><span>,</span>
    <span>"</span><span>may</span><span>"</span><span>,</span> <span>"</span><span>me</span><span>"</span><span>,</span> <span>"</span><span>might</span><span>"</span><span>,</span> <span>"</span><span>most</span><span>"</span><span>,</span> <span>"</span><span>must</span><span>"</span><span>,</span> <span>"</span><span>my</span><span>"</span><span>,</span> <span>"</span><span>neither</span><span>"</span><span>,</span> <span>"</span><span>no</span><span>"</span><span>,</span> <span>"</span><span>nor</span><span>"</span><span>,</span> <span>"</span><span>not</span><span>"</span><span>,</span> <span>"</span><span>of</span><span>"</span><span>,</span> <span>"</span><span>off</span><span>"</span><span>,</span>
    <span>"</span><span>often</span><span>"</span><span>,</span> <span>"</span><span>on</span><span>"</span><span>,</span> <span>"</span><span>only</span><span>"</span><span>,</span> <span>"</span><span>or</span><span>"</span><span>,</span> <span>"</span><span>other</span><span>"</span><span>,</span> <span>"</span><span>our</span><span>"</span><span>,</span> <span>"</span><span>own</span><span>"</span><span>,</span> <span>"</span><span>rather</span><span>"</span><span>,</span> <span>"</span><span>said</span><span>"</span><span>,</span> <span>"</span><span>says</span><span>"</span><span>,</span> <span>"</span><span>she</span><span>"</span><span>,</span>
    <span>"</span><span>should</span><span>"</span><span>,</span> <span>"</span><span>since</span><span>"</span><span>,</span> <span>"</span><span>so</span><span>"</span><span>,</span> <span>"</span><span>some</span><span>"</span><span>,</span> <span>"</span><span>than</span><span>"</span><span>,</span> <span>"</span><span>that</span><span>"</span><span>,</span> <span>"</span><span>the</span><span>"</span><span>,</span> <span>"</span><span>their</span><span>"</span><span>,</span> <span>"</span><span>them</span><span>"</span><span>,</span> <span>"</span><span>then</span><span>"</span><span>,</span>
    <span>"</span><span>there</span><span>"</span><span>,</span> <span>"</span><span>these</span><span>"</span><span>,</span> <span>"</span><span>they</span><span>"</span><span>,</span> <span>"</span><span>this</span><span>"</span><span>,</span> <span>"</span><span>those</span><span>"</span><span>,</span> <span>"</span><span>through</span><span>"</span><span>,</span> <span>"</span><span>to</span><span>"</span><span>,</span> <span>"</span><span>too</span><span>"</span><span>,</span> <span>"</span><span>more</span><span>"</span><span>,</span> <span>"</span><span>upon</span><span>"</span><span>,</span>
    <span>"</span><span>us</span><span>"</span><span>,</span> <span>"</span><span>wants</span><span>"</span><span>,</span> <span>"</span><span>was</span><span>"</span><span>,</span> <span>"</span><span>we</span><span>"</span><span>,</span> <span>"</span><span>were</span><span>"</span><span>,</span> <span>"</span><span>what</span><span>"</span><span>,</span> <span>"</span><span>when</span><span>"</span><span>,</span> <span>"</span><span>where</span><span>"</span><span>,</span> <span>"</span><span>which</span><span>"</span><span>,</span> <span>"</span><span>while</span><span>"</span><span>,</span> <span>"</span><span>who</span><span>"</span><span>,</span>
    <span>"</span><span>whom</span><span>"</span><span>,</span> <span>"</span><span>why</span><span>"</span><span>,</span> <span>"</span><span>will</span><span>"</span><span>,</span> <span>"</span><span>with</span><span>"</span><span>,</span> <span>"</span><span>would</span><span>"</span><span>,</span> <span>"</span><span>yet</span><span>"</span><span>,</span> <span>"</span><span>you</span><span>"</span><span>,</span> <span>"</span><span>your</span><span>"</span><span>,</span> <span>"</span><span>shall</span><span>"</span><span>,</span> <span>"</span><span>before</span><span>"</span><span>,</span> <span>"</span><span>now</span><span>"</span><span>,</span> <span>"</span><span>one</span><span>"</span><span>,</span>
    <span>"</span><span>even</span><span>"</span>
<span>}</span>


<span>def</span> <span>get_frequent_words</span><span>(</span><span>words</span><span>,</span> <span>take_n</span><span>):</span>
    <span>frequent_words</span> <span>=</span> <span>[</span><span>word</span><span>.</span><span>lower</span><span>()</span> <span>for</span> <span>word</span> <span>in</span> <span>words</span> <span>if</span> <span>word</span><span>.</span><span>lower</span><span>()</span> <span>not</span> <span>in</span> <span>common_words</span><span>]</span>
    <span>word_frequencies</span> <span>=</span> <span>Counter</span><span>(</span><span>frequent_words</span><span>)</span>
    <span>return</span> <span>word_frequencies</span><span>.</span><span>most_common</span><span>(</span><span>take_n</span><span>)</span>


<span>def</span> <span>get_longest_words</span><span>(</span><span>words</span><span>,</span> <span>take_n</span><span>):</span>
    <span>unique_words</span> <span>=</span> <span>set</span><span>(</span><span>word</span><span>.</span><span>lower</span><span>()</span> <span>for</span> <span>word</span> <span>in</span> <span>words</span><span>)</span>
    <span>longest_groups</span> <span>=</span> <span>defaultdict</span><span>(</span><span>list</span><span>)</span>
    <span>sorted_works</span> <span>=</span> <span>sorted</span><span>(</span><span>unique_words</span><span>,</span> <span>key</span><span>=</span><span>len</span><span>,</span> <span>reverse</span><span>=</span><span>True</span><span>)[:</span><span>take_n</span><span>]</span>
    <span>for</span> <span>word</span> <span>in</span> <span>sorted_works</span><span>:</span>
        <span>longest_groups</span><span>[</span><span>len</span><span>(</span><span>word</span><span>)].</span><span>append</span><span>(</span><span>word</span><span>)</span>
    <span>return</span> <span>dict</span><span>(</span><span>longest_groups</span><span>)</span>


<span>def</span> <span>is_palindrome</span><span>(</span><span>word</span><span>):</span>
    <span>return</span> <span>word</span> <span>==</span> <span>word</span><span>[::</span><span>-</span><span>1</span><span>]</span>


<span>def</span> <span>get_longest_palindromes</span><span>(</span><span>words</span><span>,</span> <span>take_n</span><span>):</span>
    <span>unique_words</span> <span>=</span> <span>set</span><span>(</span><span>word</span><span>.</span><span>lower</span><span>()</span> <span>for</span> <span>word</span> <span>in</span> <span>words</span> <span>if</span> <span>word</span><span>.</span><span>lower</span><span>()</span> <span>not</span> <span>in</span> <span>common_words</span><span>)</span>
    <span>palindromes</span> <span>=</span> <span>[</span><span>word</span> <span>for</span> <span>word</span> <span>in</span> <span>unique_words</span> <span>if</span> <span>is_palindrome</span><span>(</span><span>word</span><span>)]</span>
    <span>palindromes</span><span>.</span><span>sort</span><span>(</span><span>key</span><span>=</span><span>len</span><span>,</span> <span>reverse</span><span>=</span><span>True</span><span>)</span>
    <span>return</span> <span>palindromes</span><span>[:</span><span>take_n</span><span>]</span>


<span>def</span> <span>main</span><span>():</span>
    <span>book</span> <span>=</span> <span>get_book</span><span>()</span>
    <span>words</span> <span>=</span> <span>get_words</span><span>(</span><span>book</span><span>)</span>
    <span>print</span><span>(</span><span>"</span><span>Total words:</span><span>"</span><span>,</span> <span>len</span><span>(</span><span>words</span><span>))</span>
    <span>print</span><span>(</span><span>get_frequent_words</span><span>(</span><span>words</span><span>,</span> <span>10</span><span>))</span>
    <span>print</span><span>(</span><span>get_longest_words</span><span>(</span><span>words</span><span>,</span> <span>10</span><span>))</span>
    <span>print</span><span>(</span><span>get_longest_palindromes</span><span>(</span><span>words</span><span>,</span> <span>3</span><span>))</span>


<span>if</span> <span>__name__</span> <span>==</span> <span>"</span><span>__main__</span><span>"</span><span>:</span>
    <span>main</span><span>()</span>

```

### [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#clojure)Clojure

**My opinion**: The problem with Clojure is its niche nature. It’s usually difficult to understand the basics of the language and its philosophy. The amount of parentheses is unattractive to a lot of people, but in general, I find it the most beautiful implementation.  

```
<span>
</span><span>(</span><span>ns</span><span> </span><span>clojure-book.core</span><span>
  </span><span>[</span><span>:require</span><span> </span><span>[</span><span>clojure.string</span><span> </span><span>:as</span><span> </span><span>str</span><span>]]</span><span>
  </span><span>(</span><span>:gen-class</span><span>))</span><span>

</span><span>(</span><span>def</span><span> </span><span>book</span><span> </span><span>(</span><span>slurp</span><span> </span><span>"https://www.gutenberg.org/cache/epub/84/pg84.txt"</span><span>))</span><span>

</span><span>(</span><span>def</span><span> </span><span>words</span><span> </span><span>(</span><span>re-seq</span><span> </span><span>#</span><span>"[\w|’]+"</span><span> </span><span>book</span><span>))</span><span>

</span><span>(</span><span>def</span><span> </span><span>common-words</span><span>
  </span><span>#</span><span>{</span><span>"a"</span><span> </span><span>"able"</span><span> </span><span>"about"</span><span> </span><span>"across"</span><span> </span><span>"after"</span><span> </span><span>"all"</span><span> </span><span>"almost"</span><span> </span><span>"also"</span><span> </span><span>"am"</span><span> </span><span>"among"</span><span> </span><span>"an"</span><span>
    </span><span>"and"</span><span> </span><span>"any"</span><span> </span><span>"are"</span><span> </span><span>"as"</span><span> </span><span>"at"</span><span> </span><span>"be"</span><span> </span><span>"because"</span><span> </span><span>"been"</span><span> </span><span>"but"</span><span> </span><span>"by"</span><span> </span><span>"can"</span><span> </span><span>"cannot"</span><span>
    </span><span>"could"</span><span> </span><span>"dear"</span><span> </span><span>"did"</span><span> </span><span>"do"</span><span> </span><span>"does"</span><span> </span><span>"either"</span><span> </span><span>"else"</span><span> </span><span>"ever"</span><span> </span><span>"every"</span><span> </span><span>"for"</span><span> </span><span>"from"</span><span>
    </span><span>"get"</span><span> </span><span>"got"</span><span> </span><span>"had"</span><span> </span><span>"has"</span><span> </span><span>"have"</span><span> </span><span>"he"</span><span> </span><span>"her"</span><span> </span><span>"hers"</span><span> </span><span>"him"</span><span> </span><span>"his"</span><span> </span><span>"how"</span><span> </span><span>"however"</span><span>
    </span><span>"i"</span><span> </span><span>"if"</span><span> </span><span>"in"</span><span> </span><span>"into"</span><span> </span><span>"is"</span><span> </span><span>"it"</span><span> </span><span>"its"</span><span> </span><span>"just"</span><span> </span><span>"least"</span><span> </span><span>"let"</span><span> </span><span>"like"</span><span> </span><span>"likely"</span><span>
    </span><span>"may"</span><span> </span><span>"me"</span><span> </span><span>"might"</span><span> </span><span>"most"</span><span> </span><span>"must"</span><span> </span><span>"my"</span><span> </span><span>"neither"</span><span> </span><span>"no"</span><span> </span><span>"nor"</span><span> </span><span>"not"</span><span> </span><span>"of"</span><span> </span><span>"off"</span><span>
    </span><span>"often"</span><span> </span><span>"on"</span><span> </span><span>"only"</span><span> </span><span>"or"</span><span> </span><span>"other"</span><span> </span><span>"our"</span><span> </span><span>"own"</span><span> </span><span>"rather"</span><span> </span><span>"said"</span><span> </span><span>"says"</span><span> </span><span>"she"</span><span>
    </span><span>"should"</span><span> </span><span>"since"</span><span> </span><span>"so"</span><span> </span><span>"some"</span><span> </span><span>"than"</span><span> </span><span>"that"</span><span> </span><span>"the"</span><span> </span><span>"their"</span><span> </span><span>"them"</span><span> </span><span>"then"</span><span>
    </span><span>"there"</span><span> </span><span>"these"</span><span> </span><span>"they"</span><span> </span><span>"this"</span><span> </span><span>"those"</span><span> </span><span>"through"</span><span> </span><span>"to"</span><span> </span><span>"too"</span><span> </span><span>"more"</span><span> </span><span>"upon"</span><span>
    </span><span>"us"</span><span> </span><span>"wants"</span><span> </span><span>"was"</span><span> </span><span>"we"</span><span> </span><span>"were"</span><span> </span><span>"what"</span><span> </span><span>"when"</span><span> </span><span>"where"</span><span> </span><span>"which"</span><span> </span><span>"while"</span><span> </span><span>"who"</span><span>
    </span><span>"whom"</span><span> </span><span>"why"</span><span> </span><span>"will"</span><span> </span><span>"with"</span><span> </span><span>"would"</span><span> </span><span>"yet"</span><span> </span><span>"you"</span><span> </span><span>"your"</span><span> </span><span>"shall"</span><span> </span><span>"before"</span><span> </span><span>"now"</span><span> </span><span>"one"</span><span>
    </span><span>"even"</span><span>
    </span><span>})</span><span>

</span><span>(</span><span>defn</span><span> </span><span>palindrome?</span><span> </span><span>[</span><span>word</span><span>]</span><span>
  </span><span>(</span><span>=</span><span> </span><span>(</span><span>seq</span><span> </span><span>word</span><span>)</span><span> </span><span>(</span><span>reverse</span><span> </span><span>(</span><span>seq</span><span> </span><span>word</span><span>)))</span><span>
  </span><span>)</span><span>

</span><span>(</span><span>defn</span><span> </span><span>frequent-words</span><span> </span><span>[</span><span>take-n</span><span>]</span><span>
  </span><span>(</span><span>-&gt;&gt;</span><span> </span><span>words</span><span>
       </span><span>(</span><span>map</span><span> </span><span>str/lower-case</span><span>)</span><span>
       </span><span>(</span><span>remove</span><span> </span><span>common-words</span><span>)</span><span>
       </span><span>(</span><span>frequencies</span><span>)</span><span>
       </span><span>(</span><span>sort-by</span><span> </span><span>val</span><span>)</span><span>
       </span><span>(</span><span>take-last</span><span> </span><span>take-n</span><span>))</span><span>
  </span><span>)</span><span>

</span><span>(</span><span>defn</span><span> </span><span>longest-words</span><span> </span><span>[</span><span>take-n</span><span>]</span><span>
  </span><span>(</span><span>-&gt;&gt;</span><span> </span><span>words</span><span>
       </span><span>(</span><span>map</span><span> </span><span>str/lower-case</span><span>)</span><span>
       </span><span>(</span><span>distinct</span><span>)</span><span>
       </span><span>(</span><span>sort-by</span><span> </span><span>count</span><span>)</span><span>
       </span><span>(</span><span>take-last</span><span> </span><span>take-n</span><span>)</span><span>
       </span><span>(</span><span>group-by</span><span> </span><span>count</span><span>)</span><span>
       </span><span>)</span><span>
  </span><span>)</span><span>

</span><span>(</span><span>defn</span><span> </span><span>longest-palindromes</span><span> </span><span>[</span><span>take-n</span><span>]</span><span>
  </span><span>(</span><span>-&gt;&gt;</span><span> </span><span>words</span><span>
       </span><span>(</span><span>map</span><span> </span><span>str/lower-case</span><span>)</span><span>
       </span><span>(</span><span>distinct</span><span>)</span><span>
       </span><span>(</span><span>filter</span><span> </span><span>palindrome?</span><span>)</span><span>
       </span><span>(</span><span>sort-by</span><span> </span><span>count</span><span>)</span><span>
       </span><span>(</span><span>take-last</span><span> </span><span>take-n</span><span>)</span><span>
       </span><span>)</span><span>
  </span><span>)</span><span>

</span><span>(</span><span>defn</span><span> </span><span>-main</span><span>
  </span><span>[</span><span>&amp;</span><span> </span><span>args</span><span>]</span><span>
  </span><span>(</span><span>println</span><span> </span><span>(</span><span>str</span><span> </span><span>"Total words:"</span><span> </span><span>(</span><span>count</span><span> </span><span>words</span><span>)))</span><span>
  </span><span>(</span><span>println</span><span> </span><span>(</span><span>take</span><span> </span><span>10</span><span> </span><span>words</span><span>))</span><span>
  </span><span>(</span><span>println</span><span> </span><span>(</span><span>frequent-words</span><span> </span><span>10</span><span>))</span><span>
  </span><span>(</span><span>println</span><span> </span><span>(</span><span>longest-words</span><span> </span><span>10</span><span>))</span><span>
  </span><span>(</span><span>println</span><span> </span><span>(</span><span>longest-palindromes</span><span> </span><span>3</span><span>))</span><span>

  </span><span>)</span><span>

</span>
```

## [](https://dev.to/jorgetovar/the-clojure-paradox-41ke?context=digest#conclusion)Conclusion

Software is constantly evolving, and client expectations for the programs they use and build are growing. However, our focus should remain on solving problems, eliminating incidental complexity, and taking pride in our craft. There is no 'best' programming language—only tools that help us address specific problems. Even when working with legacy systems, we have the opportunity to make a positive impact through good naming conventions, best practices, improving the architecture, and generally putting the project in a better state

**There has never been a better time to be an engineer and create value in society through software.**

-   [LinkedIn](https://www.linkedin.com/in/jorgetovar-sa)
-   [Twitter](https://twitter.com/jorgetovar621)
-   [GitHub](https://github.com/jorgetovar)

If you enjoyed the articles, visit my blog [jorgetovar.dev](https://jorgetovar.dev/)
