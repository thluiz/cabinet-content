---
created: 2024-09-05T12:14:56 (UTC -03:00)
tags: [webdev,javascript,beginners,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/dharamgfx/bye-bye-jsonstringify-and-obj-hello-structuredclone-2e4c?context=digest
author: 
---

# Bye-Bye `JSON.stringify()` and ``, Hello `structuredClone()`! - DEV Community

> ## Excerpt
> What is structuredClone()?    structuredClone() is a global function introduced in 2022 that...

---
[![Cover image for Bye-Bye `JSON.stringify()` and `{...obj}`, Hello `structuredClone()`!](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fot0fgg7spvr2qmc18wq6.png)](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fot0fgg7spvr2qmc18wq6.png)

-   **What is `structuredClone()`?**
    
    -   `structuredClone()` is a global function introduced in 2022 that enables deep cloning of JavaScript objects. Unlike traditional methods like `JSON.stringify()` and `JSON.parse()`, which struggle with complex structures and circular references, `structuredClone()` handles these challenges effortlessly.
-   **Why is it a game-changer?**
    
    -   It’s a robust tool for creating true deep clones, preserving the integrity of nested objects and circular references without the need for extra logic or workarounds. Plus, it's available in modern environments, including Web Workers.

___

#### [](https://dev.to/dharamgfx/bye-bye-jsonstringify-and-obj-hello-structuredclone-2e4c?context=digest#1-simple-object-cloning-the-basics)**1\. Simple Object Cloning: The Basics**

-   **Using `{...obj}` (Shallow Copy)**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span><span>,</span> <span>details</span><span>:</span> <span>{</span> <span>age</span><span>:</span> <span>25</span> <span>}</span> <span>};</span>
  <span>const</span> <span>shallowCopy</span> <span>=</span> <span>{</span> <span>...</span><span>original</span> <span>};</span>

  <span>shallowCopy</span><span>.</span><span>details</span><span>.</span><span>age</span> <span>=</span> <span>30</span><span>;</span>

  <span>console</span><span>.</span><span>log</span><span>(</span><span>original</span><span>.</span><span>details</span><span>.</span><span>age</span><span>);</span> <span>// 30</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>shallowCopy</span><span>.</span><span>details</span><span>.</span><span>age</span><span>);</span> <span>// 30</span>
```

-   **What's happening?**
    
    -   The spread operator `{...obj}` only creates a shallow copy. The `details` object is not deeply cloned, so changes to `shallowCopy.details` affect the original `details` as well.
    -   **Using `JSON.stringify()` + `JSON.parse()` (Deep Copy)**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span><span>,</span> <span>details</span><span>:</span> <span>{</span> <span>age</span><span>:</span> <span>25</span> <span>}</span> <span>};</span>
  <span>const</span> <span>deepCopy</span> <span>=</span> <span>JSON</span><span>.</span><span>parse</span><span>(</span><span>JSON</span><span>.</span><span>stringify</span><span>(</span><span>original</span><span>));</span>

  <span>deepCopy</span><span>.</span><span>details</span><span>.</span><span>age</span> <span>=</span> <span>30</span><span>;</span>

  <span>console</span><span>.</span><span>log</span><span>(</span><span>original</span><span>.</span><span>details</span><span>.</span><span>age</span><span>);</span> <span>// 25</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>deepCopy</span><span>.</span><span>details</span><span>.</span><span>age</span><span>);</span> <span>// 30</span>
```

-   **What's happening?**
    
    -   This method creates a deep copy, but it has limitations: it cannot handle functions, `undefined`, or circular references.
    -   **Using `structuredClone()` (Deep Copy)**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span><span>,</span> <span>details</span><span>:</span> <span>{</span> <span>age</span><span>:</span> <span>25</span> <span>}</span> <span>};</span>
  <span>const</span> <span>clone</span> <span>=</span> <span>structuredClone</span><span>(</span><span>original</span><span>);</span>

  <span>clone</span><span>.</span><span>details</span><span>.</span><span>age</span> <span>=</span> <span>30</span><span>;</span>

  <span>console</span><span>.</span><span>log</span><span>(</span><span>original</span><span>.</span><span>details</span><span>.</span><span>age</span><span>);</span> <span>// 25</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>clone</span><span>.</span><span>details</span><span>.</span><span>age</span><span>);</span> <span>// 30</span>
```

-   **What's happening?**
    -   `structuredClone()` creates a deep clone, preserving the structure without any of the limitations of `JSON.stringify()` and handling complex data types like circular references and `undefined`.

___

#### [](https://dev.to/dharamgfx/bye-bye-jsonstringify-and-obj-hello-structuredclone-2e4c?context=digest#2-handling-circular-references-a-challenge)**2\. Handling Circular References: A Challenge**

-   **Circular Reference with `{...obj}`**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span> <span>};</span>
  <span>original</span><span>.</span><span>self</span> <span>=</span> <span>original</span><span>;</span>

  <span>// This will cause an error:</span>
  <span>const</span> <span>shallowCopy</span> <span>=</span> <span>{</span> <span>...</span><span>original</span> <span>};</span> <span>// TypeError: Converting circular structure to JSON</span>
```

-   **What's happening?**
    
    -   `{...obj}` can’t handle circular references, resulting in an error.
    -   **Circular Reference with `JSON.stringify()`**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span> <span>};</span>
  <span>original</span><span>.</span><span>self</span> <span>=</span> <span>original</span><span>;</span>

  <span>// This will cause an error:</span>
  <span>const</span> <span>jsonCopy</span> <span>=</span> <span>JSON</span><span>.</span><span>parse</span><span>(</span><span>JSON</span><span>.</span><span>stringify</span><span>(</span><span>original</span><span>));</span> <span>// TypeError: Converting circular structure to JSON</span>
```

-   **What's happening?**
    
    -   `JSON.stringify()` also fails with circular references, throwing an error.
    -   **Circular Reference with `structuredClone()`**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span> <span>};</span>
  <span>original</span><span>.</span><span>self</span> <span>=</span> <span>original</span><span>;</span>

  <span>const</span> <span>clone</span> <span>=</span> <span>structuredClone</span><span>(</span><span>original</span><span>);</span>

  <span>console</span><span>.</span><span>log</span><span>(</span><span>clone</span> <span>!==</span> <span>original</span><span>);</span> <span>// true</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>clone</span><span>.</span><span>self</span> <span>===</span> <span>clone</span><span>);</span> <span>// true</span>
```

-   **What's happening?**
    -   `structuredClone()` seamlessly handles circular references, creating a proper deep clone without errors.

___

#### [](https://dev.to/dharamgfx/bye-bye-jsonstringify-and-obj-hello-structuredclone-2e4c?context=digest#3-cloning-with-functions-and-raw-undefined-endraw-another-test)**3\. Cloning with Functions and `undefined`: Another Test**

-   **Using `{...obj}`**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span><span>,</span> <span>greet</span><span>:</span> <span>()</span> <span>=&gt;</span> <span>"</span><span>Hello!</span><span>"</span><span>,</span> <span>value</span><span>:</span> <span>undefined</span> <span>};</span>
  <span>const</span> <span>shallowCopy</span> <span>=</span> <span>{</span> <span>...</span><span>original</span> <span>};</span>

  <span>console</span><span>.</span><span>log</span><span>(</span><span>shallowCopy</span><span>.</span><span>greet</span><span>());</span> <span>// "Hello!"</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>shallowCopy</span><span>.</span><span>value</span><span>);</span> <span>// undefined</span>
```

-   **What's happening?**
    
    -   `{...obj}` copies functions and `undefined` as expected, but only shallowly.
    -   **Using `JSON.stringify()`**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span><span>,</span> <span>greet</span><span>:</span> <span>()</span> <span>=&gt;</span> <span>"</span><span>Hello!</span><span>"</span><span>,</span> <span>value</span><span>:</span> <span>undefined</span> <span>};</span>
  <span>const</span> <span>jsonCopy</span> <span>=</span> <span>JSON</span><span>.</span><span>parse</span><span>(</span><span>JSON</span><span>.</span><span>stringify</span><span>(</span><span>original</span><span>));</span>

  <span>console</span><span>.</span><span>log</span><span>(</span><span>jsonCopy</span><span>.</span><span>greet</span><span>);</span> <span>// undefined</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>jsonCopy</span><span>.</span><span>value</span><span>);</span> <span>// undefined</span>
```

-   **What's happening?**
    
    -   `JSON.stringify()` cannot serialize functions or `undefined`, resulting in their loss in the cloned object.
    -   **Using `structuredClone()`**

```
  <span>const</span> <span>original</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>"</span><span>Alice</span><span>"</span><span>,</span> <span>greet</span><span>:</span> <span>()</span> <span>=&gt;</span> <span>"</span><span>Hello!</span><span>"</span><span>,</span> <span>value</span><span>:</span> <span>undefined</span> <span>};</span>
  <span>const</span> <span>clone</span> <span>=</span> <span>structuredClone</span><span>(</span><span>original</span><span>);</span>

  <span>console</span><span>.</span><span>log</span><span>(</span><span>clone</span><span>.</span><span>greet</span><span>);</span> <span>// undefined</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>clone</span><span>.</span><span>value</span><span>);</span> <span>// undefined</span>
```

-   **What's happening?**
    -   `structuredClone()` also does not clone functions but preserves `undefined` values, making it more reliable than `JSON.stringify()` for complex objects.

___

#### [](https://dev.to/dharamgfx/bye-bye-jsonstringify-and-obj-hello-structuredclone-2e4c?context=digest#4-speed-and-efficiency-a-performance-note)**4\. Speed and Efficiency: A Performance Note**

-   **Efficiency with Large Data**

```
  <span>const</span> <span>largeArray</span> <span>=</span> <span>new</span> <span>Array</span><span>(</span><span>1</span><span>e6</span><span>).</span><span>fill</span><span>({</span> <span>key</span><span>:</span> <span>"</span><span>value</span><span>"</span> <span>});</span>

  <span>console</span><span>.</span><span>time</span><span>(</span><span>"</span><span>structuredClone</span><span>"</span><span>);</span>
  <span>const</span> <span>clone</span> <span>=</span> <span>structuredClone</span><span>(</span><span>largeArray</span><span>);</span>
  <span>console</span><span>.</span><span>timeEnd</span><span>(</span><span>"</span><span>structuredClone</span><span>"</span><span>);</span>

  <span>console</span><span>.</span><span>time</span><span>(</span><span>"</span><span>JSON.stringify + JSON.parse</span><span>"</span><span>);</span>
  <span>const</span> <span>jsonCopy</span> <span>=</span> <span>JSON</span><span>.</span><span>parse</span><span>(</span><span>JSON</span><span>.</span><span>stringify</span><span>(</span><span>largeArray</span><span>));</span>
  <span>console</span><span>.</span><span>timeEnd</span><span>(</span><span>"</span><span>JSON.stringify + JSON.parse</span><span>"</span><span>);</span>
```

-   **What's happening?**
    -   `structuredClone()` is often faster than `JSON.stringify()` + `JSON.parse()` for large, complex data, and avoids the pitfalls of serializing and deserializing.

___

#### [](https://dev.to/dharamgfx/bye-bye-jsonstringify-and-obj-hello-structuredclone-2e4c?context=digest#5-conclusion-why-raw-structuredclone-endraw-is-the-future)**5\. Conclusion: Why `structuredClone()` is the Future**

-   **Reliability**: Handles circular references, functions, and `undefined` values more predictably.
-   **Efficiency**: Performs deep cloning faster for large datasets and doesn’t require workarounds.
-   **Simplicity**: One method to rule them all—no more choosing between `{...obj}`, `JSON.stringify()`, or custom deep clone functions.
