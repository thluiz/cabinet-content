---
created: 2024-07-15T14:22:06 (UTC -03:00)
tags: []
source: https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev
author: Ravindre Ramjiawan
---

# Making Web Workers enjoyable - iO tech_hub

> ## Excerpt
> In a single threaded environment Web Workers allow for offloading intensive tasks to keep the main thread free and responsive.

---
## Table of Contents

-   [Background](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#background)
-   [Web Workers](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#web-workers)
-   [Comlink](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#comlink)
-   [Conclusion](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#conclusion)

## Background

In the dynamic and ever-evolving landscape of web development, developers universally strive to create responsive applications. The goal is to deliver a seamless user experience, maintaining a minimum of 60 frames per second, a benchmark set by the highly responsive<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-1" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-1">1</a></sup> mobile apps that users have become accustomed to. Furthermore, unhandled user input can lead to a subpar user experience, underscoring the importance of comprehensive input handling in the quest for optimal responsiveness. However, this can often present a significant challenge due to the single-threaded<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-2" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-2">2</a></sup> nature of JavaScript.

In the current era, with an abundance of frontend libraries at our disposal, it’s increasingly easy to compromise on performance and responsiveness as these libraries consume substantial resources. This implies that when JavaScript is tasked with heavy computations or data processing, it can result in an unresponsive or janky user interface (UI), leading to user dissatisfaction. Therefore, it is crucial to manage resources effectively to ensure a smooth and responsive UI.

## Web Workers

Web Workers<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-3" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-3">3</a></sup> were first published by the World Wide Web Consortium (W3C)<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-17" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-17">4</a></sup> and the Web Hypertext Application Technology Working Group (WHATWG)<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-18" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-18">5</a></sup> on April 3, 2009. The W3C and the WHATWG conceptualize web workers as scripts that operate continuously. These scripts are designed to run without being disrupted by other scripts that react to user interactions such as clicks. By ensuring these workers are not interrupted by user activities, web pages can maintain their responsiveness while concurrently executing extensive tasks in the background. This approach allows for a smoother and more efficient user experience.

Web Workers are not to be confused with Service Workers<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-19" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-19">6</a></sup>. Service Workers serve a different purpose and act as proxy server to enable offline experiences by intercepting network requests. Service Workers also run in a worker context meaning that they run in a separate thread.

### How to create a Web Worker

To make use of a Web Worker you have to create a new Worker by calling the `Worker()` constructor and passing the URL of a script file that will be executed in the Worker thread.

```typescript
// Vanilla const worker = new Worker('heavy-calculation-script.js')
```

Nowadays with the usage of web bundlers such as Webpack<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-4" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-4">7</a></sup> or Vite<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-5" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-5">8</a></sup> Web Workers require a relative module url.

```typescript
// Bundlers const worker = new Worker(new URL('./heavy-calcluation-script.js', import.meta.url))
```

### How to communicate with a Web Worker

Once you have created your Web Worker you are now able to send and receive messages from it. To send messages you make use of the `postMessage()` method and to listen for messages you can use `addEventListener()` or set a callback directly on the `onmessage` property.

```typescript
const worker = new Worker('heavy-calculation-script.js') // Sending a message to the Web Worker worker.postMessage('Hello from main thread!') // Listening for messages from the Web Worker worker.onmessage = ({ data }: MessageEvent) => { console.log(data) // Logs: Hello from worker! } // Using event listeners worker.addEventListener('message', ({ data }: MessageEvent) => { console.log(data) // Logs: Hello from worker! })
```

You can send anything that is serializable<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-6" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-6">9</a></sup> when using `postMessage()`. JavaScript uses the structured clone algorithm<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-7" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-7">10</a></sup> to perform copying complex objects.

```typescript
// heavy-calculation-script.js Web Worker // Sending a message to the Main Thread self.postMessage('Hello from worker!') self.addEventListener('message', ({ data }: MessageEvent) => { console.log(data) // Logs: Hello from main thread! })
```

In the context of a web worker `self`<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-8" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-8">11</a></sup> refers to an object that points to current Web Worker context. It is a reliable way to reference the worker context, unlike the `this` keyword, which can behave unpredictably in various situations. Normally when using `this` in the global execution context<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-9" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-9">12</a></sup> will refer to the `Window`<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-10" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-10">13</a></sup> object.

### Drawbacks of Web Worker communication

Whilst the Web Worker API to send and receive messages gets the job done it is not very developer friendly because of its low-level API. It requires a lot of manual management of message routing and payload marshalling<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-11" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-11">14</a></sup>. There are certain patterns that seem to work nicely with `postMessage()` such as the Flux<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-12" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-12">15</a></sup> pattern but there are better libraries out there that can make the use Web Workers a lot more enjoyable and intuitive.

## Comlink

Comlink<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-13" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-13">16</a></sup> is a tiny library developed by Google. Its primary function is to simplify the process of working with Web Workers by eliminating the complexities associated with using `postMessage()`. It achieves this by adopting an RPC (Remote Procedure Call)<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-14" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-14">17</a></sup> style for message transmission and leveraging JavaScript Proxies<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-15" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-15">18</a></sup> that maintain a reference to the original target. In essence, Comlink enables seamless access to any element from the Main Thread within a Web Worker and vice versa. This bidirectional accessibility significantly enhances the developer experience. Furthermore, when used together with TypeScript<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-16" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-16">19</a></sup> it supports autocomplete features, making coding even more efficient and enjoyable.

### How to use Comlink

Comlink provides a set of functions that help connect the main thread to the Web Worker and vice versa is fairly straight forward as shown below. The `wrap()` function wraps the Worker and takes the other end of a Message Channel<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-21" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-21">20</a></sup> as an argument and returns a proxy. This proxy will have all properties and functions of the exposed value from the other thread. However, access to these properties and function invocations are inherently asynchronous. This means that a function that would normally return a number will now return a `Promise()`<sup><a href="https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fn-20" aria-describedby="footnote-label" data-footnote-ref="true" id="user-content-fnref-20">21</a></sup> for a number.

```typescript
import { wrap } from 'comlink' // Create Web Worker const worker = new Worker('heavy-calculation-script.js') // Wrap the Web Worker using the wrap method from Comlink const wrappedWorker = wrap(worker) // Call any exposed methods from your Web Worker // Since its a Promise you can use either await or then wrappedWorker.exposedMethod().then(console.log) // Logs: 5
```

The `expose()` method from Comlink is used to make a local object available to the other end of the Message Channel. It can be viewed as the Comlink equivalent of `export`. This method takes an object and exposes it to the other thread, allowing the other thread to access its properties and methods.

```typescript
// heavy-calculation-script.js Web Worker import { expose } from 'comlink' const api = { exposedMethod() { return 5 }, } // Call expose from Comlink to expose anything you like for the Main Thread to have access to expose(api)
```

## Conclusion

I hope this gives a better understanding on how Web Workers can help with offloading heavy computations, intensive tasks or long-running pieces of code. This allows for the Main Thread to run as efficient and responsive as possible for not only a better user experience but also a great developer experience when using libraries such as Comlink.

1.  [https://en.wikipedia.org/wiki/Responsiveness](https://en.wikipedia.org/wiki/Responsiveness) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-1)
    
2.  [https://en.wikipedia.org/wiki/Thread\_(computing)](https://en.wikipedia.org/wiki/Thread_(computing)) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-2)
    
3.  [https://en.wikipedia.org/wiki/Web\_worker](https://en.wikipedia.org/wiki/Web_worker) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-3)
    
4.  [https://www.w3.org/](https://www.w3.org/) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-17)
    
5.  [https://whatwg.org/](https://whatwg.org/) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-18)
    
6.  [https://developer.mozilla.org/en-US/docs/Web/API/Service\_Worker\_API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-19)
    
7.  [https://webpack.js.org/guides/web-workers/](https://webpack.js.org/guides/web-workers/) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-4)
    
8.  [https://v3.vitejs.dev/guide/features.html#web-workers](https://v3.vitejs.dev/guide/features.html#web-workers) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-5)
    
9.  [https://en.wikipedia.org/wiki/Serialization](https://en.wikipedia.org/wiki/Serialization) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-6)
    
10.  [https://developer.mozilla.org/en-US/docs/Web/API/Web\_Workers\_API/Structured\_clone\_algorithm](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-7)
    
11.  [https://developer.mozilla.org/en-US/docs/Web/API/WorkerGlobalScope/self](https://developer.mozilla.org/en-US/docs/Web/API/WorkerGlobalScope/self) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-8)
    
12.  [https://en.wikipedia.org/wiki/Scope\_(computer\_science)](https://en.wikipedia.org/wiki/Scope_(computer_science)) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-9)
    
13.  [https://developer.mozilla.org/en-US/docs/Web/API/Window](https://developer.mozilla.org/en-US/docs/Web/API/Window) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-10)
    
14.  [https://en.wikipedia.org/wiki/Marshalling\_(computer\_science)](https://en.wikipedia.org/wiki/Marshalling_(computer_science)) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-11)
    
15.  [https://facebookarchive.github.io/flux/](https://facebookarchive.github.io/flux/) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-12)
    
16.  [https://github.com/GoogleChromeLabs/comlink](https://github.com/GoogleChromeLabs/comlink) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-13)
    
17.  [https://en.wikipedia.org/wiki/Remote\_procedure\_call](https://en.wikipedia.org/wiki/Remote_procedure_call) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-14)
    
18.  [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-15)
    
19.  [https://en.wikipedia.org/wiki/TypeScript](https://en.wikipedia.org/wiki/TypeScript) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-16)
    
20.  [https://developer.mozilla.org/en-US/docs/Web/API/MessageChannel](https://developer.mozilla.org/en-US/docs/Web/API/MessageChannel) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-21)
    
21.  [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) [↩](https://techhub.iodigital.com/articles/making-web-workers-enjoyable?ref=dailydev#user-content-fnref-20)
    

___

## Share

-   [](https://www.linkedin.com/sharing/share-offsite?url=https://techhub.iodigital.com/articles/making-web-workers-enjoyable "LinkedIn")
-   [](https://twitter.com/intent/tweet?text=https://techhub.iodigital.com/articles/making-web-workers-enjoyable "Twitter")
-   [](https://www.facebook.com/sharer/sharer.php?u=https://techhub.iodigital.com/articles/making-web-workers-enjoyable "Facebook")
-   [](mailto:?body=https://techhub.iodigital.com/articles/making-web-workers-enjoyable "Email")
