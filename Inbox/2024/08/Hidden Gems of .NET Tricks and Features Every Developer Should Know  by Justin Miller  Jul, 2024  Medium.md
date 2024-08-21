---
created: 2024-08-21T09:03:02 (UTC -03:00)
tags: []
source: https://medium.com/@justinamiller_52480/hidden-gems-of-net-tricks-and-features-every-developer-should-know-ca6e006288d2
author: Justin Miller
---

# Hidden Gems of .NET: Tricks and Features Every Developer Should Know | by Justin Miller | Jul, 2024 | Medium

> ## Excerpt
> .NET is a powerful and versatile framework that has been a mainstay for developers across the globe. While its core capabilities are widely known, there are some hidden features and tricks that can…

---
[

![Justin Miller](https://miro.medium.com/v2/da:true/resize:fill:88:88/0*rJSQQw1-Qb2EZw1Q)



](https://medium.com/@justinamiller_52480?source=post_page-----ca6e006288d2--------------------------------)

.NET is a powerful and versatile framework that has been a mainstay for developers across the globe. While its core capabilities are widely known, there are some hidden features and tricks that can significantly enhance your development experience and efficiency. Let’s dive into some of these gems that every .NET developer should know about.

**1\. Global Using Directives (C# 10 and beyond)** Tired of repeating `using` statements in every file? C# 10 introduces Global Using Directives. Declare them once in a project file, and they'll automatically apply to all files, reducing clutter and improving maintainability:

```
<span id="71b9" data-selectable-paragraph=""><br>&lt;ItemGroup&gt;<br>    &lt;Using Include=<span>"System"</span> /&gt;<br>    &lt;Using Include=<span>"System.Collections.Generic"</span> /&gt;<br>    &lt;Using Include=<span>"System.Linq"</span> /&gt; <br>&lt;/ItemGroup&gt;</span>
```

**2\. String Interpolation Enhancements**

String interpolation is even more expressive! You can use variables directly within interpolated strings, create more complex expressions, and even format numbers with precision:

```
<span id="b7c9" data-selectable-paragraph=""><span>double</span> price = <span>19.99</span>;<br><span>int</span> quantity = <span>3</span>;<br><span>string</span> message = <span>$"You ordered <span>{quantity}</span> items. Total: <span>{price * quantity:C2}</span>"</span>; </span>
```

**3\. The Power of Span<T>**`Span<T>` and its read-only counterpart, `ReadOnlySpan<T>`, offer efficient ways to work with slices of arrays and strings without allocating memory. This is invaluable for high-performance scenarios:

```
<span id="e0d9" data-selectable-paragraph="">ReadOnlySpan&lt;<span>char</span>&gt; text = <span>"Hello, world!"</span>.<span>AsSpan</span>(); <br>ReadOnlySpan&lt;<span>char</span>&gt; substring = text.<span>Slice</span>(<span>0</span>, <span>5</span>); </span>
```

**4\. Top-Level Statements (C# 9 and beyond)** Streamline your programs by eliminating boilerplate. Top-level statements let you write the core logic of a program without wrapping it in a namespace or a class:

```
<span id="5894" data-selectable-paragraph="">// No namespace or class required!<br>Console<span>.WriteLine</span>("Hello <span>from</span> <span>a</span> <span>top</span>-level statement!");</span>
```

**5\. Source Generators** Source generators are powerful tools that can generate C# code at compile time. This allows for dynamic code generation, custom build steps, and improved performance through compile-time optimizations.

**6\. File-Scoped Namespaces (C# 10 and beyond)** Another feature to reduce verbosity is file-scoped namespaces:

```
<span id="b993" data-selectable-paragraph=""><br><span>namespace</span> MyProject.Utilities;</span>
```

**7\. Nullable Reference Types (C# 8 and beyond)** Enhance the safety and reliability of your code by explicitly marking variables as nullable or non-nullable:

Nullable reference types, introduced in C# 8.0, help you avoid the dreaded `NullReferenceException`. By enabling this feature, you can explicitly specify which variables can be null and which cannot.

Add the following line to your `.csproj` file:

`<Nullable>enable</Nullable>`

Now, reference types are non-nullable by default, and you must use the `?` suffix for nullable reference types:

```
<span id="c015" data-selectable-paragraph=""><span>string</span>? name = <span>null</span>; <br><span>string</span> message = <span>"Hello!"</span>; </span>
```

**8\. Custom Attributes for Everything** Attributes are not just for decorating classes and methods. You can use them to add metadata to fields, parameters, enums, and more, unlocking custom behavior in your code.

**9\. Records (C# 9 and beyond)** Records are a concise way to create immutable reference types, perfect for data models and scenarios where you want to enforce value equality:

```
<span id="600f" data-selectable-paragraph=""><span><span>public</span> <span>record</span> <span>Person</span>(<span><span>string</span> FirstName, <span>string</span> LastName</span>)</span>;</span>
```

**10\. Explore .NET Interactive Notebooks** If you haven’t yet, dive into .NET Interactive Notebooks. They offer an engaging way to combine code, visualizations, and documentation for learning, prototyping, or showcasing your projects.

## 11\. Pattern Matching Enhancements

C# 9.0 and beyond have introduced several pattern-matching enhancements, making your code more concise and expressive.

```
<span id="b37f" data-selectable-paragraph=""><span><span>public</span> <span>string</span> <span>GetDescription</span>(<span><span>object</span> obj</span>)</span> =&gt;<br>    obj <span>switch</span><br>    {<br>        Person p =&gt; <span>$"Person: <span>{p.FirstName}</span> <span>{p.LastName}</span>"</span>,<br>        <span>int</span> i =&gt; <span>$"Integer: <span>{i}</span>"</span>,<br>        _ =&gt; <span>"Unknown"</span><br>    };</span>
```

This switch expression simplifies complex conditional logic.

## 12\. Records and Init-Only Setters

C# 9.0 introduces `init`\-only setters, which allow you to create immutable objects while using object initializers.

```
<span id="5f65" data-selectable-paragraph=""><span>public</span> <span>class</span> <span>Product</span><br>{<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>init</span>; }<br>    <span>public</span> <span>decimal</span> Price { <span>get</span>; <span>init</span>; }<br>}</span>
```

```
<span id="e836" data-selectable-paragraph="">var product = new Product { Name = "Laptop", Price = 999.99m };</span>
```

This feature ensures the properties can only be set during object initialization.

## 13\. System.Text.Json

System.Text.Json, introduced in .NET Core 3.0, provides a high-performance JSON serialization library. It’s more efficient and lightweight compared to `Newtonsoft.Json`.

```
<span id="5364" data-selectable-paragraph=""><span>var</span> options = <span>new</span> JsonSerializerOptions { WriteIndented = <span>true</span> };<br><span>var</span> jsonString = JsonSerializer.Serialize(product, options);<br>Console.WriteLine(jsonString);</span>
```

```
<span id="579e" data-selectable-paragraph="">var deserializedProduct = JsonSerializer.Deserialize&lt;Product&gt;(jsonString);</span>
```

This library supports various customization options for serialization and deserialization.

## 14\. Benchmarking with BenchmarkDotNet

BenchmarkDotNet is a powerful library for benchmarking .NET code. It helps you measure and optimize performance.

Install the `BenchmarkDotNet` package and create a benchmark class:

```
<span id="320b" data-selectable-paragraph="">[<span>MemoryDiagnoser</span>]<br><span>public</span> <span>class</span> <span>StringConcatBenchmark</span><br>{<br>    [<span>Benchmark</span>]<br>    <span><span>public</span> <span>string</span> <span>PlusOperator</span>()</span> =&gt; <span>"Hello"</span> + <span>" "</span> + <span>"World"</span>;<br><br>    [<span>Benchmark</span>]<br>    <span><span>public</span> <span>string</span> <span>StringConcat</span>()</span> =&gt; <span>string</span>.Concat(<span>"Hello"</span>, <span>" "</span>, <span>"World"</span>);<br>}<br><span>class</span> <span>Program</span><br>{<br>    <span><span>static</span> <span>void</span> <span>Main</span>(<span><span>string</span>[] args</span>)</span><br>    {<br>        BenchmarkRunner.Run&lt;StringConcatBenchmark&gt;();<br>    }<br>}</span>
```

Run your benchmarks and analyze the results to identify performance bottlenecks.

## 15\. ValueTask for Reducing Overhead in Async Operations

The `Task` type is ubiquitous in asynchronous programming within .NET. However, it introduces some overhead that can be unnecessary in certain scenarios. Enter `ValueTask`.

-   **ValueTask**: This structure reduces the overhead associated with `Task` when a method frequently returns synchronously. It avoids allocations, making it a more efficient choice for high-performance applications.

```
<span id="f383" data-selectable-paragraph=""><span><span>public</span> <span>ValueTask</span>&lt;<span>int</span>&gt; <span>GetDataAsync</span>()</span> <br>{     <br><span>if</span> (dataAvailable)    <br> {        <br> <span>return</span> <span>new</span> ValueTask&lt;<span>int</span>&gt;(data);     <br>}     <br><span>else</span>   <br>  {         <br><span>return</span> <span>new</span> ValueTask&lt;<span>int</span>&gt;(FetchDataAsync());   <br>  }<br> }</span>
```

## 16\. Channels for High-Throughput Data Processing

For high-performance, thread-safe data processing, `System.Threading.Channels` provides a powerful tool.

-   **Channels**: These are designed for scenarios requiring high-throughput and low-latency communication between producers and consumers. They offer more control and performance compared to traditional `BlockingCollection` or `ConcurrentQueue`.

```
<span id="edef" data-selectable-paragraph=""><span>var</span> channel = Channel.CreateUnbounded&lt;<span>int</span>&gt;();<br><span>await</span> channel.Writer.WriteAsync(<span>42</span>); <br><span>var</span> <span>value</span> = <span>await</span> channel.Reader.ReadAsync();</span>
```

## 17\. MemoryCache for Efficient Caching

Caching is a well-known technique for improving performance, and `MemoryCache` in .NET provides a flexible and efficient way to implement it.

-   **MemoryCache**: This class allows you to store data in memory, reducing the need for repeated data retrieval operations. It supports expiration policies and cache dependencies, making it a robust choice for performance optimization.

```
<span id="7e17" data-selectable-paragraph=""><span>var</span> cache = <span>new</span> <span>MemoryCache</span>(<span>new</span> <span>MemoryCacheOptions</span>()); <br>cache.<span>Set</span>(<span>"key"</span>, value, <span>TimeSpan</span>.<span>FromMinutes</span>(<span>5</span>)); <br><span>var</span> cachedValue = cache.<span>Get</span>(<span>"key"</span>);</span>
```

## 18\. Using Structs and Avoiding Boxing

Boxing and unboxing can introduce significant overhead, by using`structs` instead of classes for value types can help avoid this.

-   **Structs**: These are value types and can be more efficient in terms of memory usage and performance. However, use them judiciously as they can also lead to increased memory pressure if not used appropriately.

```
<span id="e250" data-selectable-paragraph=""><span>public</span> <span>struct</span> Point<br>{     <br>  <span>public</span> <span>int</span> X { <span>get</span>; <span>set</span>; }     <br>  <span>public</span> <span>int</span> Y { <span>get</span>; <span>set</span>; } <br>}</span>
```

-   **Boxing** is the process of converting a value type (like `int`, `bool`, `struct`, etc.) into a reference type (like `object`). When you box a value type:

```
<span id="875d" data-selectable-paragraph=""><span>int</span> num = <span>42</span>;      // Value <span>type</span> (<span>int</span>)<br><span>object</span> obj = num;   // Boxing (<span>int</span> <span>is</span> converted to <span>object</span>)</span>
```

**Unboxing** is the reverse process of boxing. It extracts the value type from an object reference. Unboxing involves:

```
<span id="65d9" data-selectable-paragraph=""><span>object</span> obj = <span>42</span>;    // Assuming obj <span>is</span> a boxed <span>int</span><br><span>int</span> num = (<span>int</span>)obj; // Unboxing (<span>object</span> <span>is</span> converted back to <span>int</span>)</span>
```

## Conclusion

These hidden gems of .NET can significantly improve your development experience and productivity. By leveraging these features, you can write cleaner, more efficient code and stay ahead in the ever-evolving world of .NET development. Happy coding!

Feel free to share your own .NET tips and tricks in the comments below!
