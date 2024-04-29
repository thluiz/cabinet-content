---
created: 2024-01-19T20:26:45 (UTC -03:00)
tags: []
source: https://medium.com/globant/javascript-optimization-techniques-20d8d167dadd
author: Rafael Rojas
---

# Javascript Optimization Techniques | by Rafael Rojas | Globant | Jan, 2024 | Medium

> ## Excerpt
> When we begin a project, we tend to focus on things like scalability, usability, availability, security, and others. But, as the application grows, we may observe a decline in its speed and…

---
[

![Rafael Rojas](https://miro.medium.com/v2/da:true/resize:fill:44:44/0*aWMEZd7RCL0rWhYc)



](https://medium.com/@rafael.rojas.gdev?source=post_page-----20d8d167dadd--------------------------------)[

![Globant](https://miro.medium.com/v2/resize:fill:24:24/1*0Wnn42G23dANSroOAFr7vQ.png)



](https://medium.com/globant?source=post_page-----20d8d167dadd--------------------------------)

![](https://miro.medium.com/v2/resize:fit:700/0*Bdz6s4_QyPto9ddw)

Photo by [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

When we begin a project, we tend to focus on things like scalability, usability, availability, security, and others. But, as the application grows, we may observe a decline in its speed and performance. It is often only at this point that we recognize the need for optimization.

In this article, we will present some of the most common techniques for optimizing code, which can be implemented in any application; we will also show optimization techniques using sample code written in [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) and [React](https://react.dev/). The following techniques are gonna be covered:

-   Debouncing
-   Throttling
-   Memoization
-   Pure Components
-   Lazy Loading
-   Virtualization (or Windowing)
-   Error Boundaries
-   Inline Functions

There are many more techniques available, but in this article, we will focus on the ones already mentioned.

## **Debouncing**

Debouncing is a programming technique used to optimize the processing of functions that consume a lot of execution time. This technique involves preventing those functions from executing repeatedly without control, which helps improve the performance of our applications.

In the case of applications that must respond to certain user actions, we often cannot avoid certain functions from being executed repeatedly. For example, events such as `mousemove` or `window.resize` can trigger hundreds of calls to these functions with a simple mouse movement or browser window resizing. It is in these cases that we resort to techniques like Debouncing to limit these calls and solve performance issues that may be caused by such events or functions.

The operation of Debouncing is quite simple. It involves creating a function that acts as an interceptor to limit the call to the callback function we want to optimize. This function will have at least two parameters: `time` and `callback`. The `time` parameter is used to indicate to the Debounce how long the function should wait before being called, and the `callback` parameter is the function that will be conditioned to this time limit. Once the control mechanism is created, the `debounce` function returns a new optimized function that will serve in place of the original function.

It is worth noting that in Debouncing, if multiple calls to the callback occur within the defined time window, only the last call will be considered for execution and the previous ones will be discarded. Additionally, while this is happening, the time window will also be renewed each time a call occurs. For example, if we define the time as 2 seconds, the callback defined in the `debounce` function will only be executed after 2 seconds. If multiple calls occur within the time window, the time will be renewed for the same period, and only the last function that entered the `debounce` function will be executed once the defined time is met.

Here is a simple example of how to implement Debouncing in code using JavaScript:

```
<span id="c74a" data-selectable-paragraph=""><br><br><span>const</span> <span>debounce</span> = (<span>callback, time = <span>300</span></span>) =&gt; {<br>  <span>let</span> timer;<br><br>  <span>return</span> <span>() =&gt;</span> {<br>    <span>clearTimeout</span>(timer);<br>    timer = <span>setTimeout</span>(callback, time);<br>  };<br>};</span>
```

In this simplified version, the `debounce` function returns another function that will handle the debounce. The returned function clears the timer variable of any previously created timeout and sets a new timeout with the `callback` it received as a parameter. Each time the new debounced function is executed, it will access the same timer variable, clear it, and replace the timeout each time.

Thus, we have created our debounce, and the way to use it would be as follows:

```
<span id="cb5d" data-selectable-paragraph=""><br><br><br><span>const</span> <span>showMessage</span> = () =&gt; <span>console</span>.<span>log</span>(<span>"Hello Message"</span>);<br><br><br><span>const</span> debouncedMessage = <span>debounce</span>(showMessage, <span>1000</span>);<br><br><br><span>for</span> (<span>let</span> i = <span>0</span>; i &lt; <span>10000</span>; i++) {<br>  <span>setTimeout</span>(debouncedMessage, i);<br>}</span>
```

In this example, the `debouncedMessage` function will be called 10,000 times in a `for` loop. However, due to the debounce, the message will only be displayed once instead of 10,000 times.

## **Throttling**

Throttling is a technique similar to debouncing, as both are used to limit the frequency of function calls. The difference is that throttling does not clear the timer every time the function is called, but instead uses a pause condition to avoid creating new timers. In other words, while the function is being called, it will not wait until the last call to execute, but will only call the function if it enters the time interval where the pause is disabled.

Let’s see an example of this to better understand it:

```
<span id="7645" data-selectable-paragraph=""><br><br><span>const</span> <span>throttle</span> = (<span>callback, time = <span>300</span></span>) =&gt; {<br>  <span>let</span> pause = <span>false</span>;<br><br><br>  <span>return</span> <span>() =&gt;</span> {<br>    <span>if</span> (pause) <span>return</span>;<br>    pause = <span>true</span>;<br>    <span>callback</span>();<br>    <span>setTimeout</span>(<span>() =&gt;</span> {<br>      pause = <span>false</span>;<br>    }, time);<br>  };<br>};</span>
```

In the above example, we can see that unlike debounce, we now have a `pause` variable instead of a timer, and the returned function now has different behavior. We can see that it now checks if the `pause` is `true` to return, which is basically to prevent the rest of the code from executing, it is an escape clause. In case the execution is not interrupted in that condition, what we will do is activate the pause, so that subsequent calls to this function will not be executed because the pause will be activated. Then, we have the call to the `callback`, and finally, we close the throttling process leaving a `setTimeout` to disable the pause when the time we have defined is fulfilled.

What will happen with throttling, as we explained earlier, is that we will be limiting the calling of our functions, but as long as they continue to be called, they will be executed every certain amount of time. On the other hand, with «_debouncing_», what happens is that it will be waiting for the last call to be able to execute the function only once.

For a better understanding, let’s see the following example:

```
<span id="4c90" data-selectable-paragraph=""><br><br><br><span>const</span> <span>showMessage</span> = () =&gt; <span>console</span>.<span>log</span>(<span>"Hello Message"</span>);<br><br><br><span>const</span> throttledMessage = <span>throttle</span>(showMessage, <span>1000</span>);<br><br><br><span>for</span> (<span>let</span> i = <span>0</span>; i &lt; <span>10000</span>; i++) {<br>  <span>setTimeout</span>(throttledMessage, i);<br>}</span>
```

In the case of debouncing, the message was only displayed once because, in each execution, the timeout was renewed until the last call lasted 10 seconds (10,000 milliseconds). In contrast, for throttling, the message was displayed 10 times, once every second (1000 milliseconds), due to the pause that conditions its execution. As you can see, both techniques have the same goal but are slightly different in code and behavior. This slight difference allows us to choose one mechanism or another depending on the circumstances. That is why it is important to know these two techniques to know when to implement one or the other and thus correctly optimize our code.

## **Memoization**

Memoization is a technique that involves storing the result of a function in a memory space that allows for later retrieval. This is done to avoid having to recalculate the result every time the function is called with the same parameters. This technique is used for functions that consume a lot of resources. In such cases, memoization can improve performance and speed in obtaining results.

This technique can be used in React as well, we can make use of the following memoization features in React:

-   `React.memo`
-   `useMemo`
-   `useCallback`

However, before explaining how these features are used in React, let’s first discuss memoization in JavaScript using the algorithm for calculating the factorial of a number as an example. The implementation would look something like this:

```
<span id="a4bc" data-selectable-paragraph=""><br><br><span>const</span> <span>getFactorial</span> = (<span>n</span>) =&gt; {<br>  <span>if</span> (n === <span>1</span> || n === <span>0</span>) <span>return</span> <span>1</span>;<br>  <span>return</span> n * <span>getFactorial</span>(n - <span>1</span>);<br>};</span>
```

At first glance, this algorithm looks simple. However, being a recursive function, we must be careful when passing it very large numbers, as depending on the value we provide, this function can be very expensive in terms of processing. As you may know, the factorial of a number is simply the recursive multiplication by its predecessors until it reaches 1, whose formula would be as follows: **_n! = n_ ·** **_(n -1)_ · _(n — 2)_ · _…_ · _1._**

For example, if we want to calculate the factorial of 6, it can be calculated like this: **6! = 6 · 5 · 4 · 3 · 2 · 1.** The thing with this algorithm is that many of those multiplications could be done only once and then save their results in a lookup table or an object. This way, when we need to obtain those values again, we will no longer have to recalculate them. One way to memoize this function would be like this:

```
<span id="9334" data-selectable-paragraph=""><br><br><span>const</span> <span>memoize</span> = fn =&gt; {<br>  <span>const</span> cache = <span>new</span> <span>Map</span>();<br>  <span>return</span> <span>(<span>...args</span>) =&gt;</span> {<br>    <span>const</span> key = args.<span>join</span>(<span>"-"</span>);<br>    <span>if</span> (cache.<span>has</span>(key)) <span>return</span> cache.<span>get</span>(key);<br>    <span>const</span> result = <span>fn</span>(...args);<br>    cache.<span>set</span>(key, result);<br>    <br>    <span>return</span> result;<br>  };<br>};<br><br><span>const</span> getFactorial = <span>memoize</span>(<span><span>n</span> =&gt;</span> {<br>  <span>if</span> (n === <span>1</span> || n === <span>0</span>) <span>return</span> <span>1</span>;<br>  <span>return</span> n * <span>getFactorial</span>(n - <span>1</span>);<br>});<br><br><br><span>getFactorial</span>(<span>100</span>);<br><br><br><span>getFactorial</span>(<span>99</span>);</span>
```

In this example, a`memoize` function is defined first, which takes a function as an argument and returns a new function that memoizes the results of the original function. This new function uses a `Map` object to cache previously calculated results. When the memoized function is called with certain arguments, it first checks if there is already a result stored for those arguments in the cache. If so, it returns the stored result. If not, it calculates the result by calling the original function with those arguments, stores the result in the cache, and returns it.

Then, the `getFactorial` function is passed to the `memoize` function and the result is assigned to a new variable. The memoized version of `getFactorial` can now be called like any other function but stores its previous results in the cache, making it more efficient for repeated calculations.

In other words, when we calculate the factorial of 100, the function will run 100 times. However, by that time, we will have already stored all the calculations in the `cache` object. So when we run the function again with the same value or a value below 100, it will no longer be necessary to recalculate everything again, but we will obtain its value from the `cache` object that we have created; consequently, the function will only be executed once.

In this way, we optimized this function to avoid re-calculations and improve the response speed. In summary, memoization is a good technique for optimizing components. This is one of the biggest advantages of using optimization techniques.

## **React.memo**

This is a useful tool for avoiding unnecessary renders of components when the props they receive do not change. The purpose of `[React.memo](https://react.dev/reference/react/memo)` is to improve application performance. However, it is important to note that React.memo will not prevent component renders caused by state or context (`[React.Context](https://legacy.reactjs.org/docs/context.html)`) updates, as it only considers props to avoid re-renders.

Using `React.memo` is pretty straightforward to implement. Below is an example of how to use it:

```
<span id="ccff" data-selectable-paragraph=""><br><br><span>import</span> { memo } <span>from</span> <span>"react"</span>;<br><span>import</span> { <span>MyComponent</span> } <span>from</span> <span>"./MyComponent"</span>;<br><br><span>export</span> <span>const</span> <span>App</span> = () =&gt; {<br>  <br>  <span>const</span> <span>MemoizedComponent</span> = <span>memo</span>(<span>MyComponent</span>);<br><br>  <br>  <span>const</span> <span>MemoizedComponent2</span> = <span>memo</span>(<span>function</span> (<span>{ data }</span>) {<br>    <span>return</span> &lt;div&gt;Some interesting {data}&lt;/div&gt;;<br>  });<br><br>  <br>  <span>const</span> <span>MemoizedComponent3</span> = <span>memo</span>(<span>(<span>{ data }</span>) =&gt;</span> {<br>    <span>return</span> &lt;div&gt;Some interesting {data}&lt;/div&gt;;<br>  });<br><br>  <span>const</span> someDataPassedAsProp = {};<br><br>  <span>return</span> &lt;MemoizedComponent data={someDataPassedAsProp} /&gt;;<br>};</span>
```

This way, as long as the props maintain the same value over time, the component will not be re-rendered. This is especially useful in applications with components that have a large number of children or in situations where props do not change frequently. By using, unnecessary updates can be avoided and application performance can be improved.

## **React.useMemo**

This [hook](https://react.dev/reference/react/useMemo), allows us to cache the results of a costly function between component renders, and only re-execute the function if its dependencies change. But be careful, this hook should only be used within components or other hooks, not within loops or conditionals.

To understand how it works, let’s take the `expensiveCalculationFn` function as an example, which theoretically requires performing a series of complex and costly calculations. In our component, we can use `useMemo` to prevent it from being executed on every component render, which can slow down the application. In this way, the hook will return the last cached value and only update that value if the dependencies change.

Here’s an example of how to use `useMemo` in a component:

```
<span id="3d09" data-selectable-paragraph=""><br><br><span>import</span> { useMemo, useState } <span>from</span> <span>"react"</span>;<br><br><span>const</span> <span>expensiveCalculationFn</span> = (<span>a, b, c</span>) =&gt; {<br>  <br>  <span>return</span> a*b*c;<br>};<br><br><span>export</span> <span>const</span> <span>MyComponent</span> = (<span>props</span>) =&gt; {<br>  <span>const</span> { a, b, c } = props;<br><br>  <br>  <br>  <span>const</span> result = <span>useMemo</span>(<span>() =&gt;</span> <span>expensiveCalculationFn</span>(a,b,c), [<br>    props<br>  ]);<br><br>  <span>return</span> &lt;h1&gt;{result}&lt;/h1&gt;;<br>};</span>
```

In this example, we use _useMemo_ to avoid the costly execution of `expensiveCalculationFn` on every component render. If the dependencies, which in this case are `props`, do not change, the hook will return the last cached value. Additionally, it is recommended that the function used with `useMemo` be a _pure function_.

If you want to learn more about this hook, we recommend reviewing the [official documentation](https://beta.reactjs.org/reference/react/useMemo#skipping-expensive-recalculations), which contains several examples of use cases to better understand its functionality and applicability in different situations.

Please take in mind that useMemo is not a general javascript function for memoization, it’s only a built-in hook in React that is used for memoization, also its use it’s recommended in only few cases, here you can [read more about it](https://react.dev/reference/react/useMemo#should-you-add-usememo-everywhere).

## **React.useCallback**

The `[useCallback](https://react.dev/reference/react/useCallback)` hook is very similar to the previous one, but it differs in that this hook caches the function definition and not the resulting value of its execution. Its update will occur only if the defined dependencies change in value. It is important to note that `useCallback` does not execute functions, it only saves and updates their definition to be executed later by us. In contrast, `React.useMemo` executes functions, saving and updating in cache only their resulting value.

This hook can be useful, for example, to cache callbacks that are passed to child components as props, of which we want to avoid re-rendering due to a render of the parent component. By caching the function references, and leaving a callback cached, it will remain the same in case it is not updated, and therefore, in case our child component is encapsulated with `React.memo`, it will not trigger a render in the component where it was passed as a prop since the function did not change, maintaining its previous reference. To see how `useCallback` can be used, we will optimize the following component:

```
<span id="f370" data-selectable-paragraph=""><br><br><span>import</span> { useEffect, useState} <span>from</span> <span>"react"</span>;<br><br><br><span>const</span> <span>ChildComponent</span> = (<span>{ callback }</span>) =&gt; {<br>  <span>console</span>.<span>log</span>(<span>"Child was rendered..."</span>);<br>  <span>return</span> &lt;button onClick={callback}&gt;Click me!&lt;/button&gt;;<br>};<br><br><span>export</span> <span>const</span> <span>MyComponent</span> = (<span>props</span>) =&gt; {<br>  <br>  <span>const</span> [color, setColor] = <span>useState</span>(<span>"#ff22ff"</span>);<br>  <span>const</span> [otherState, setOtherState] = <span>useState</span>(<span>false</span>);<br>  <span>const</span> otherVariables = <span>"other variables..."</span>;<br><br>  <br>  <span>const</span> <span>callbackFn</span> = () =&gt; {<br>    <span>console</span>.<span>log</span>(<span>"Hello, you clicked!"</span>);<br>  };<br><br>  <br>  <span>useEffect</span>(<span>() =&gt;</span> {<br>    <span>setTimeout</span>(<span>() =&gt;</span> {<br>      <span>setColor</span>(<span>"#00ddd2"</span>);<br>    }, <span>3000</span>);<br>  }, []);<br><br>  <br>  <span>return</span> &lt;ChildComponent callback={callbackFn} /&gt;;<br>};</span>
```

Here we have a normal component, without optimization, which has a few states, and variables, and also renders a child component that receives the function `callbackFn` as a prop. In this case, every time we update the state of our component, by default, our child component will re-render. If we want to avoid our child component from re-rendering, we need to encapsulate it within `React.memo`, as you may remember, `React.memo` avoids the re-render of a component if its props do not change. So let’s do that:

```
<span id="a524" data-selectable-paragraph=""><br><br><span>import</span> { useEffect, useState, memo } <span>from</span> <span>"react"</span>;<br><br><br><span>const</span> <span>ChildComponent</span> = <span>memo</span>(<span>(<span>{ callback }</span>) =&gt;</span> {<br>  <span>console</span>.<span>log</span>(<span>"Child was rendered..."</span>);<br>  <span>return</span> &lt;button onClick={callback}&gt;Click me!&lt;/button&gt;;<br>});<br><br><span>export</span> <span>const</span> <span>MyComponent</span> = (<span>props</span>) =&gt; {<br>  <br>  <span>const</span> [color, setColor] = <span>useState</span>(<span>"#ff22ff"</span>);<br>  <span>const</span> [otherState, setOtherState] = <span>useState</span>(<span>false</span>);<br>  <span>const</span> otherVariables = <span>"other variables..."</span>;<br><br>  <br>  <span>const</span> <span>callbackFn</span> = () =&gt; {<br>    <span>console</span>.<span>log</span>(<span>"Hello, you clicked!"</span>);<br>  };<br><br>  <br>  <span>useEffect</span>(<span>() =&gt;</span> {<br>    <span>setTimeout</span>(<span>() =&gt;</span> {<br>      <span>setColor</span>(<span>"#00ddd2"</span>);<br>    }, <span>3000</span>);<br>  }, []);<br><br>  <br>  <span>return</span> &lt;ChildComponent callback={callbackFn} /&gt;;<br>};</span>
```

As you can see, we have now cached our component with `React.memo`. However, we haven’t fully solved the problem yet. The function inside our `MyComponent` that we are passing to our `ChildComponent` is actually a reference, so every time `MyComponent` updates the reference to that function will also update. To prevent this from causing unnecessary re-renders in `ChildComponent`, we can use `useCallback`. By using `useCallback`, the function will retain its definition and reference as long as its dependencies remain unchanged. This will prevent the props of `ChildComponent` from being updated and therefore avoid unnecessary re-renders. Here’s the final version of the component:

```
<span id="6cf4" data-selectable-paragraph=""><br><br><span>import</span> { useEffect, useState, memo, useCallback } <span>from</span> <span>"react"</span>;<br><br><br><span>const</span> <span>ChildComponent</span> = <span>memo</span>(<span>(<span>{ callback }</span>) =&gt;</span> {<br>  <span>console</span>.<span>log</span>(<span>"Child was rendered..."</span>);<br>  <span>return</span> &lt;button onClick={callback}&gt;Click me!&lt;/button&gt;;<br>});<br><br><span>export</span> <span>const</span> <span>MyComponent</span> = (<span>props</span>) =&gt; {<br>  <br>  <span>const</span> [color, setColor] = <span>useState</span>(<span>"#ff22ff"</span>);<br>  <span>const</span> [otherState, setOtherState] = <span>useState</span>(<span>false</span>);<br>  <span>const</span> otherVariables = <span>"other variables..."</span>;<br><br>  <br>  <span>const</span> callbackFn = <span>useCallback</span>(<span>() =&gt;</span> {<br>    <span>console</span>.<span>log</span>(<span>"Hello, you clicked!"</span>);<br>  }, [props, otherState, otherVariables]); <br><br>  <br>  <span>useEffect</span>(<span>() =&gt;</span> {<br>    <span>setTimeout</span>(<span>() =&gt;</span> {<br>      <span>setColor</span>(<span>"#00ddd2"</span>);<br>    }, <span>3000</span>);<br>  }, []);<br><br>  <br>  <span>return</span> &lt;ChildComponent callback={callbackFn} /&gt;;<br>};</span>
```

If you want to learn more about the usage of `useCallback`, you can continue reading its [official documentation](https://beta.reactjs.org/reference/react/useCallback), where more use cases are explained.

## **Pure Components**

A React [component](https://react.dev/learn/your-first-component) is considered pure if it renders the same output for the same state and props. For this type of component, React provides the `PureComponent` class. Class components that extend the [PureComponent](https://react.dev/reference/react/PureComponent) class are treated as pure components. Pure components have some performance improvements and render optimizations since React implements the `shouldComponentUpdate()` method for them with a shallow comparison for props and state.

However, nowadays, functional components are more commonly used. If you are already using class components and even class pure components, you might want to read about [how to migrate](https://beta.reactjs.org/reference/react/PureComponent#alternatives) from pure class components to functional components. But apart from that, it is always good to know how pure components work and how we can use them as classes. Let’s see some examples of pure components.

```
<span id="1d90" data-selectable-paragraph=""><br><br><span>import</span> <span>React</span>, { <span>Component</span>, <span>PureComponent</span>, memo } <span>from</span> <span>"react"</span>;<br><br><br><span>class</span> <span>MyComponent</span> <span>extends</span> <span>Component</span> {<br>  <span>render</span>(){<br>    <span>return</span> (<br>      &lt;div&gt;My Component&lt;/div&gt;<br>    );<br>  }<br>}<br><br><br><span>class</span> <span>MyComponent</span> <span>extends</span> <span>PureComponent</span> {<br>  <span>render</span>(){<br>    <span>return</span> (<br>      &lt;div&gt;My Component&lt;/div&gt;<br>    );<br>  }<br>}<br><br><br><span>const</span> <span>MyComponent</span> = (<span>props</span>) =&gt; &lt;div&gt;My Component&lt;/div&gt;;<br><br><br><span>const</span> <span>MyComponent</span> = <span>memo</span>(<span>(<span>props</span>) =&gt;</span> &lt;div&gt;My Component&lt;/div&gt;);</span>
```

By the way, we can use the `[shouldComponentUpdate](https://legacy.reactjs.org/docs/react-component.html#shouldcomponentupdate)()` method which is a lifecycle method that is called by React to determine if a component should re-render. By default, it always returns true, meaning that the component will always re-render when its state or props change.

This means we can define the `shouldComponentUpdate()` method in a class Component (not PureComponent) to optimize performance. This method receives two parameters: the `nextProps` and the `nextState`. You can compare these values with the current `props` _and_ `state` **and return false if you determine that the component does not need to re-render**.

In a PureComponent, React automatically implements `shouldComponentUpdate()` for you with a shallow comparison of `props` and `state` . This means that if props and state are the same as in the previous render, the component won’t re-render.

In functional components, the equivalent of `shouldComponentUpdate()` is using the `React.memo` or the `useMemo` hook, which can help prevent unnecessary re-renders by performing a shallow comparison of props, in combination with `useState`, to handle the state of those components.

## **Lazy Loading**

`[React.lazy](https://es.reactjs.org/docs/code-splitting.html#reactlazy)` is a React technique that allows us to lazily import our components until the moment they are first rendered. This can be quite useful when we don’t want to import the entire bundle of components at once, as it can slow down the application’s load time. The idea behind `React.lazy` is to speed up the loading of a specific page by importing only what the page needs at that moment. This can be especially helpful in large or complex applications that contain many components.

Here’s how we can use lazy loading:

```
<span id="5a48" data-selectable-paragraph=""><br><br><span>import</span> { lazy } <span>from</span> <span>'react'</span>;<br><br><br><span>import</span> <span>MyNormalComponent</span> <span>from</span> <span>'./MyNormalComponent'</span>;<br><br><br><span>const</span> <span>MyLazyComponent</span> = <span>lazy</span>(<span>() =&gt;</span> <span>import</span>(<span>'./MyLazyComponent'</span>));</span>
```

It is recommended that components imported using `React.lazy` be encapsulated within another React component called [Suspense](https://es.reactjs.org/docs/code-splitting.html#suspense). This component allows us to display a fallback while we wait for the lazy component to load. In this fallback, we can display a message or loading animation to let the user know that something is being loaded. Here’s an example of how to use a lazy component with `React.Suspense`:

```
<span id="5919" data-selectable-paragraph=""><br><br><span>import</span> { lazy, <span>Suspense</span> } <span>from</span> <span>'react'</span>;<br><span>import</span> <span>LoadingAnimation</span> <span>from</span> <span>'./LoadingAnimation'</span>;<br><br><span>const</span> <span>MyLazyComponent</span> = <span>lazy</span>(<span>() =&gt;</span> <span>import</span>(<span>'./MyLazyComponent'</span>));<br><br><span>const</span> <span>MyApp</span> = () =&gt; {<br>  <span>return</span> (<br>    &lt;Suspense fallback={&lt;LoadingAnimation /&gt;}&gt;<br>      &lt;MyLazyComponent /&gt;<br>    &lt;/Suspense&gt;<br>  );<br>};</span>
```

It’s important to note that `React.lazy` can only be used with default exports. Therefore, if your component was not exported in this way, you will have to modify the export of your component so that `React.lazy` can take it without any problems. You can read more about this [here](https://reactjs.org/docs/code-splitting.html#named-exports).

Another important point to highlight is that if you have your application built with **_create-react-app_** you can use `React.lazy` in conjunction with [React Router](https://reactrouter.com/en/main), which optimizes the navigation of your application by generating a code splitting by routes automatically. Instead of having to load the entire bundle, the bundle is loaded as you navigate through the application.

This is because `create-react-app` has webpack configured out of the box already, otherwise, you will have to configure it [manually](https://webpack.js.org/guides/code-splitting/) in order to enable the code-splitting feature. Here’s an example of how to use lazy loading with React Router:

```
<span id="95bc" data-selectable-paragraph=""><br><br><span>import</span> { lazy, <span>Suspense</span> } <span>from</span> <span>'react'</span>;<br><span>import</span> { <span>BrowserRouter</span> <span>as</span> <span>Router</span>, <span>Routes</span>, <span>Route</span> } <span>from</span> <span>'react-router-dom'</span>;<br><span>import</span> <span>LoadingAnimation</span> <span>from</span> <span>'./LoadingAnimation'</span>;<br><br><br><span>const</span> <span>Home</span> = <span>lazy</span>(<span>() =&gt;</span> <span>import</span>(<span>'./Home'</span>));<br><span>const</span> <span>Login</span> = <span>lazy</span>(<span>() =&gt;</span> <span>import</span>(<span>'./Login'</span>));<br><span>const</span> <span>Register</span> = <span>lazy</span>(<span>() =&gt;</span> <span>import</span>(<span>'./Register'</span>));<br><span>const</span> <span>About</span> = <span>lazy</span>(<span>() =&gt;</span> <span>import</span>(<span>'./About'</span>));<br><br><span>const</span> <span>MyApp</span> = () =&gt; {<br>  <span>return</span> (<br>    &lt;Router&gt;<br>      &lt;Suspense fallback={&lt;LoadingAnimation /&gt;}&gt;<br>        &lt;Routes&gt;<br>          &lt;Route path="/" element={&lt;Home /&gt;} /&gt;<br>          &lt;Route path="/login" element={&lt;Login /&gt;} /&gt;<br>          &lt;Route path="/register" element={&lt;Register /&gt;} /&gt;<br>          &lt;Route path="/about" element={&lt;About /&gt;} /&gt;<br>        &lt;/Routes&gt;<br>      &lt;/Suspense&gt;<br>    &lt;/Router&gt;<br>  );<br>};</span>
```

By using this technique with routes, we separate our bundle and generate smaller bundles called «_chunks_» in which each route is contained in each one. Therefore, when it’s time to load a route, instead of downloading the entire bundle at once, the bundle is loaded as it’s needed. As mentioned before, this technique is known as code splitting, and you can learn more about it [here](https://reactjs.org/docs/code-splitting.html). Ultimately, this optimizes the loading speed of our applications, making them smoother and providing a better user experience.

## **Virtualization (Windowing)**

React virtualization is a powerful technique for creating high-performing user interfaces that only display the elements currently in use. Whether you use a React virtualized library or build your own algorithm, the technique is specifically designed to handle large datasets and improve UI performance. By selectively rendering only the elements needed at any given time, virtualization in React leads to faster load times and a smoother user experience overall.

To better understand this technique, consider the following example:

![](https://miro.medium.com/v2/resize:fit:650/0*mSZz3jEhxmiQj1gn)

Figure 1: List virtualization compared to the regular list.

Virtualization can also be applied to tables, where both rows and columns can be virtualized to achieve significant performance improvements. This technique is especially useful for components that display large amounts of data, such as tables. By only rendering the rows and columns that are currently visible, virtualization in React can greatly improve the performance of these components. To illustrate this further, let’s take a look at the following example:

![](https://miro.medium.com/v2/resize:fit:432/0*PIzNioZgsxHVelKR)

Figure 2: Table virtualization.

If you would like to implement your own virtualized tables or lists, I recommend this [react-window](https://github.com/bvaughn/react-window) package, which provides all the necessary tools to virtualize your components.

## **Error Boundaries**

Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. [Error boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary) catch errors during rendering, lifecycle methods, and constructors of the whole tree below them. It’s important to note that error boundaries do not catch errors for event handlers, asynchronous code, or errors that occur in the error boundary itself. You can find more details about error boundaries [here](https://reactjs.org/docs/error-boundaries.html).

Here’s an example of how you can implement error boundaries in your components using React:

```
<span id="f215" data-selectable-paragraph=""><br><br><span>import</span> { <span>PureComponent</span> } <span>from</span> <span>'react'</span>;<br><br><span>export</span> <span>class</span> <span>ErrorBoundaries</span> <span>extends</span> <span>PureComponent</span> {<br>  <span>constructor</span>(<span>props</span>) {<br>    <span>super</span>(props);<br>    <span>this</span>.<span>state</span> = { <span>hasError</span>: <span>false</span>, <span>errorInfo</span>: <span>null</span> };<br>  }<br><br>  <span>static</span> <span>getDerivedStateFromError</span>(<span>error</span>) {<br>    <br>    <br>    <span>return</span> { <span>hasError</span>: <span>true</span>, <span>errorInfo</span>: error };<br>  }<br><br>  <span>componentDidCatch</span>(<span>error, errorInfo</span>) {<br>    <br>    <span>console</span>.<span>log</span>(error, errorInfo, <span>this</span>.<span>state</span>.<span>errorInfo</span>);<br>  }<br><br>  <span>render</span>() {<br>    <span>if</span> (<span>this</span>.<span>state</span>.<span>hasError</span>) {<br>      <br>      <span>return</span> &lt;h1&gt;Something went wrong.&lt;/h1&gt;;<br>    }<br><br>    <br>    <span>return</span> ( <br>      &lt;div&gt;<br>        This is the main returned component with no error yet.<br>      &lt;/div&gt;<br>    ) <br>  }<br>}</span>
```

With this implementation, you can wrap any component that might throw an error with the ErrorBoundary component, and it will catch and handle any errors that occur within it or its child components. The [getDerivedStateFromError](https://beta.reactjs.org/reference/react/Component#static-getderivedstatefromerror) method updates the state to show a fallback UI when an error occurs, and the [componentDidCatch](https://beta.reactjs.org/reference/react/Component#componentdidcatch) method logs the error to the console or an error reporting service.

## **Inline Functions**

There are several reasons why it is recommended to avoid using inline functions in React. Here are some of them:

-   **Performance**: Every time a component is rendered in React, all inline functions are recreated. This can lead to slower performance in your application, especially if you have many components with inline functions.
-   **Maintenance**: Inline functions are defined within the component and cannot be reused in other parts of the application. This can make the code more difficult to maintain as the application grows.
-   **Readability**: Inline functions can make code more difficult to read and understand, especially if they are long or have many arguments.

In the example below, we will see inline functions for each button. However, as previously mentioned, this can lead to performance issues, which may not be noticeable in this example, but can have a significant impact on larger applications. Therefore, it is recommended to optimize these types of things at a larger scale.

```
<span id="9d5c" data-selectable-paragraph=""><br><br><span>import</span> { useState } <span>from</span> <span>"react"</span>;<br><br><span>export</span> <span>const</span> <span>InlineFunctions</span> = () =&gt; {<br>  <span>const</span> [counter, setCounter] = <span>useState</span>(<span>0</span>);<br><br>  <span>return</span> (<br>    &lt;div&gt;<br>      &lt;h1&gt;{counter}&lt;/h1&gt;<br>      &lt;button<br>        onClick={() =&gt; {<br>          setCounter((prevCount) =&gt; prevCount + 1);<br>        }}<br>      &gt;<br>        Increase Counter<br>      &lt;/button&gt;<br>      &lt;button<br>        onClick={() =&gt; {<br>          setCounter((prevCount) =&gt; prevCount - 1);<br>        }}<br>      &gt;<br>        Decrease Counter<br>      &lt;/button&gt;<br>    &lt;/div&gt;<br>  );<br>};</span>
```

A better approach to handle click events, in this case, would be to define a separate function for handling the click event and then use that function as a callback for the `onClick` event of each button. This way, the function is not recreated every time the component is rendered, improving performance. Here’s an updated example using a separate function:

```
<span id="f486" data-selectable-paragraph=""><br><br><span>import</span> { useState } <span>from</span> <span>"react"</span>;<br><br><span>export</span> <span>const</span> <span>InlineFunctions</span> = () =&gt; {<br>  <span>const</span> [counter, setCounter] = <span>useState</span>(<span>0</span>);<br><br>  <span>const</span> <span>handleClick</span> = (<span>value</span>) =&gt; <span>() =&gt;</span> {<br>    <span>setCounter</span>(<span>(<span>prevCount</span>) =&gt;</span> prevCount + value);<br>  };<br><br>  <span>return</span> (<br>    &lt;div&gt;<br>      &lt;h1&gt;{counter}&lt;/h1&gt;<br>      &lt;button onClick={handleClick(1)}&gt;Increase Counter&lt;/button&gt;<br>      &lt;button onClick={handleClick(-1)}&gt;Decrease Counter&lt;/button&gt;<br>    &lt;/div&gt;<br>  );<br>};</span>
```

In conclusion, while inline functions can be useful in small components or for quick prototyping, it is recommended to avoid using them in larger applications or components that are rendered frequently. By extracting inline functions into separate functions that can be reused and optimized, you can improve the performance, maintenance, and readability of your code. In the example provided, we were able to optimize the code by creating a separate function to handle the button click events, which reduces the number of inline functions and improves the overall performance of the component.

## Summary

In conclusion, we have covered a range of topics related to optimizing React applications. Debouncing, throttling, and memoization are all techniques that can help improve performance by reducing unnecessary rendering and processing. Pure components are another important optimization, ensuring that components only re-render when necessary.

Lazy loading and virtualization are techniques that can help improve the initial load time of an application, as well as its overall performance. Lazy loading allows us to load only the necessary components or resources when they are actually needed, while virtualization (or windowing) allows us to render only the visible portion of a large dataset, avoiding unnecessary rendering and processing of off-screen elements.

Error boundaries provide a way to handle errors that might otherwise crash our application. By using error boundaries, we can log and handle errors gracefully, providing a fallback UI instead of a complete application crash.

Finally, we discussed the importance of avoiding inline functions whenever possible due to their impact on performance, maintenance, and readability.

Overall, by implementing these techniques in our Javascript or React applications, we can create more efficient, responsive, and scalable applications. If you want to learn more about any of these topics, I encourage you to explore the React documentation, and we recommend having a look at the other articles that Globant has on [Medium](https://medium.com/globant).
