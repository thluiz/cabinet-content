---
created: 2024-01-03T09:21:32 (UTC -03:00)
tags: [cleancode,golang,avitotech,book,robert martin]
source: https://habr.com/en/articles/782920/?ref=dailydev
author: Anton Iusupov
---

# Thoughts and short notes (in go) after reading «Clean Code» / Habr

> ## Excerpt
> 1. Clean CodeThe gist: Clean code is more than just working code; it's code that other developers can easily read, understand, and modify.// Not so clean func...

---
**1\. Clean Code**

_The gist_: Clean code is more than just working code; it's code that other developers can easily read, understand, and modify.

```
<span>// Not so clean</span>
<span><span>func</span> <span>p</span><span>(a []<span>int</span>)</span></span> {
  <span>// ...some cryptic code...</span>
}
<span>// Clean</span>
<span><span>func</span> <span>sortNumbers</span><span>(numbers []<span>int</span>)</span></span> {
  <span>// sorting logic...</span>
}
```

**2\. Meaningful Names**

_The gist_: Use descriptive and specific names that reveal your intention.

```
<span>// Confusing</span>
<span>var</span> d <span>int</span> <span>// elapsed time in days</span>
<span>// Clear</span>
<span>var</span> elapsedTimeInDays <span>int</span>
<span>// Even clearer in context</span>
<span><span>func</span> <span>calculateElapsedTime</span><span>(startDate, endDate time.Time)</span> <span>int</span></span> {
  <span>return</span> <span>int</span>(endDate.Sub(startDate).Hours() / <span>24</span>)
}
```

**3\. Functions**

_The gist_: Functions should do one thing, be small, and have no side effects.

```
<span>// Too complex</span>
<span><span>func</span> <span>handleHttpRequest</span><span>(r http.Request)</span></span> {
<span>// parsing, logging, and handling...</span>
}
<span>// Decomposed into focused functions</span>
<span><span>func</span> <span>parseRequest</span><span>(r http.Request)</span> <span>RequestData</span></span> {...}
<span><span>func</span> <span>logRequest</span><span>(data RequestData)</span></span> {...}
<span><span>func</span> <span>handleRequest</span><span>(data RequestData)</span> <span>Response</span></span> {...}
```

**4\. Comments**

_The gist_: Good code mostly speaks for itself.

```
<span>// Unnecessary</span>
<span>// Check if user is valid</span>
<span>if</span> user.IsValid() {...}
<span>// Clear code</span>
<span>if</span> user.IsValid() {...} <span>// No comment needed</span>
```

**5\. Formatting**

_The gist_: Consistent and thoughtful formatting makes your code easier to read and understand.

```
<span>// Inconsistent</span>
<span><span>func</span> <span>foo</span><span>()</span></span> {
  <span>var</span> x <span>int</span>
  x=<span>3</span>
  fmt.Println(x)
}
<span>// Consistent</span>
<span><span>func</span> <span>formatNumber</span><span>(number <span>int</span>)</span> <span>string</span></span> {
  formatted := fmt.Sprintf(<span>"%03d"</span>, number)
  <span>return</span> formatted
}
```

**6\. Objects and Data Structures**

_The gist_: Objects hide their data behind abstractions and expose functions to operate on that data. Data structures expose their data and have no significant behavior.

```
<span>// Data structure</span>
<span>type</span> Rectangle <span>struct</span> {
  Width  <span>float64</span>
  Height <span>float64</span>
}
<span>// Object-like behavior</span>
<span>type</span> Shape <span>interface</span> {
  Area() <span>float64</span>
}
<span><span>func</span> <span>(r Rectangle)</span> <span>Area</span><span>()</span> <span>float64</span></span> {
  <span>return</span> r.Width * r.Height
}
```

**7\. Error Handling**

_The gist_: Treat error handling as a primary concern, not an afterthought.

```
<span><span>func</span> <span>connectDatabase</span><span>(connectionString <span>string</span>)</span> <span>(*sql.DB, error)</span></span> {
  db, err := sql.Open(<span>"postgres"</span>, connectionString)
  <span>if</span> err != <span>nil</span> {
    <span>return</span> <span>nil</span>, fmt.Errorf(<span>"error connecting to database: %v"</span>, err)
  }
  <span>// Additional connection verification...</span>
  <span>return</span> db, <span>nil</span>
}
```

**8\. Boundaries**

_The gist_: Respect boundaries between different layers and services by keeping interchanges well-defined and clean.

```
<span>// Interfacing with an external package</span>
<span><span>func</span> <span>readConfigFile</span><span>(path <span>string</span>)</span> <span>(Config, error)</span></span> {
  <span>var</span> config Config
  data, err := ioutil.ReadFile(path)
  <span>if</span> err != <span>nil</span> {
    <span>return</span> config, err
  }
  err = json.Unmarshal(data, &amp;config)
  <span>return</span> config, err
}
```

**9\. Unit Tests**

_The gist_: Tests keep your code flexible, maintainable, and understandable.

```
<span><span>func</span> <span>TestCalculateTotal</span><span>(t *testing.T)</span></span> {
  total := calculateTotal([]<span>int</span>{<span>10</span>, <span>20</span>, <span>30</span>})
  <span>if</span> total != <span>60</span> {
    t.Errorf(<span>"Expected 60, got %d"</span>, total)
  }
}
```

**10\. Classes (Structs and Interfaces in Go)**

_The gist_: Keep your data structures small, focused, and with a clear purpose.

```
<span>type</span> Server <span>interface</span> {
  Start()
  Stop()
}
<span>type</span> HttpServer <span>struct</span> {
  <span>// Server configuration fields</span>
}
<span><span>func</span> <span>(h *HttpServer)</span> <span>Start</span><span>()</span></span> {
  <span>// start server</span>
}
<span><span>func</span> <span>(h *HttpServer)</span> <span>Stop</span><span>()</span></span> {
  <span>// stop server</span>
}
```

**11\. Systems**

_The gist_: Building clean systems involves organizing code into layers and managing dependencies carefully.  
_Conceptual Example_:  
Organize your code into packages with clear purposes. Use interfaces to abstract implementation details and dependencies.

**12\. Emergence**

_The gist_: Simple design, refactoring, and a focus on building well-crafted code lead to emergent behavior.  
_Conceptual Example_:  
Regularly refactor and simplify your code. Remove duplication, improve names, and break large functions into smaller, more focused ones.

**13\. Concurrency**

_The gist_: Concurrency is tricky; keep it separate and well-managed.

```
<span><span>func</span> <span>processConcurrently</span><span>(data []<span>int</span>)</span></span> {
  <span>var</span> wg sync.WaitGroup
  <span>for</span> _, value := <span>range</span> data {
    wg.Add(<span>1</span>)
    <span>go</span> <span><span>func</span><span>(val <span>int</span>)</span></span> {
      <span>defer</span> wg.Done()
      processValue(val)
    }(value)
  }
  wg.Wait()
}
```

**14\. Successive Refinement**

_The gist_: Code should be continuously improved upon.  
_Conceptual Example_:  
Take an existing function or module and improve it: clarify names, split large functions, reduce dependencies, and add tests.

**15\. JUnit Internals**

_The gist_: Understanding how good frameworks and libraries work can inspire you to write better code.  
_Conceptual Example_:  
Look into the source code of a well-respected Go package to understand its structure and design decisions.

**16\. Refactoring SerialDate**

_The gist_: Refactoring is an essential tool for maintaining and improving the structure of existing code.  
_Conceptual Example_:  
Identify a complex module or function in your codebase and step through a refactoring process to improve it.

**17\. Smells and Heuristics**

_The gist_: Recognize common "smells" in your code that indicate problems and understand heuristics for solving them.  
_Conceptual Example_:  
Review your code for "smells" such as rigidity, needless complexity, and duplication. Apply heuristics like the Single Responsibility Principle and DRY (Don't Repeat Yourself) to improve it.

**Conclusion**

Applying clean code principles isn't just about following rules; it's about developing a mindset. The more you practice writing clear, concise, and maintainable code, the more natural it will become. Just making a program that works isn't enough; it should also be tidy and understandable. Remember, clean code benefits your future self and your teammates, so always aim for clarity and simplicity. Keep it tidy!
