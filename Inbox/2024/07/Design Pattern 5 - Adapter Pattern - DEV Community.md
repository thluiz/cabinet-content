---
created: 2024-07-15T14:28:19 (UTC -03:00)
tags: [javascript,architecture,learning,webdev,software,coding,development,engineering,inclusive,community]
source: https://dev.to/superviz/design-pattern-5-adapter-pattern-4gif?context=digest
author: 
---

# Design Pattern #5 - Adapter Pattern - DEV Community

> ## Excerpt
> Over the past few weeks, I have shared some of the trending design patterns, like the PubSub and the...

---
Over the past few weeks, I have shared some of the trending design patterns, like the [PubSub](https://dev.to/superviz/design-pattern-4-publishersubscriber-pattern-4jg9) and the [Singleton](https://dev.to/superviz/design-pattern-1-singleton-for-frontend-developers-14p9) ones. Today, I'm going to share one more article of this series, but please comment below and tell me which design pattern I should cover next!

## [](https://dev.to/superviz/design-pattern-5-adapter-pattern-4gif?context=digest#the-adapter-pattern)The Adapter Pattern

The Adapter Pattern is a structural design pattern that allows objects with incompatible interfaces to collaborate. It's often used when you want to make existing classes work with others without modifying their source code. This pattern is particularly useful when the interface of an existing class does not match the one you need.

### [](https://dev.to/superviz/design-pattern-5-adapter-pattern-4gif?context=digest#real-case-scenario)Real Case Scenario

Let's consider a real-life example. You've been tasked to integrate a third-party video player into your application. However, the video player functions differently and has a different method interface than what your application expects. In this case, you could use the Adapter Pattern to create a wrapper class around the video player, making the third-party code compatible with your existing application code.

Here is the code you'd use in this case:  

```
<span>// Adapter class</span>
<span>class</span> <span>VideoPlayerAdapter</span> <span>{</span>
    <span>constructor</span><span>()</span> <span>{</span>
        <span>this</span><span>.</span><span>externalPlayer</span> <span>=</span> <span>new</span> <span>ThirdPartyVideoPlayer</span><span>({</span>
            <span>// some configuration</span>
        <span>});</span>
    <span>}</span>

    <span>play</span><span>()</span> <span>{</span>
        <span>const</span> <span>video</span> <span>=</span> <span>this</span><span>.</span><span>externalPlayer</span><span>.</span><span>getVideo</span><span>();</span>
        <span>this</span><span>.</span><span>externalPlayer</span><span>.</span><span>playVideo</span><span>(</span><span>video</span><span>,</span> <span>{</span>
            <span>// additional parameters</span>
        <span>});</span>
    <span>}</span>
<span>}</span>

<span>// Your application code</span>
<span>class</span> <span>Application</span> <span>{</span>
    <span>constructor</span><span>()</span> <span>{</span>
        <span>this</span><span>.</span><span>videoPlayer</span> <span>=</span> <span>new</span> <span>VideoPlayerAdapter</span><span>();</span>
    <span>}</span>

    <span>start</span><span>()</span> <span>{</span>
        <span>// Play video using your application code</span>
        <span>this</span><span>.</span><span>videoPlayer</span><span>.</span><span>play</span><span>();</span>
    <span>}</span>
<span>}</span>

```

Let's break down the code above:

1.  `ThirdPartyVideoPlayer` is a hypothetical external library that your application wants to use. However, its interface might not be compatible with your application.
2.  `VideoPlayerAdapter` is the adapter class. It wraps around `ThirdPartyVideoPlayer`. The adapter's interface is compatible with your application. When the adapter's `play()` method is called, it internally calls the necessary methods on `ThirdPartyVideoPlayer`.
3.  `Application` is your application code. It creates an instance of `VideoPlayerAdapter` and uses it as if it were a regular video player. When it calls the `play()` method on the adapter, the adapter translates that into the appropriate calls to `ThirdPartyVideoPlayer`.

This way, the `Application` class doesn't need to know anything about how `ThirdPartyVideoPlayer` works. If you ever need to replace `ThirdPartyVideoPlayer` with a different library, you only need to write a new adapter — the `Application` class can stay the same. This is the main benefit of the Adapter pattern: it decouples your application code from the specifics of third-party libraries.

### [](https://dev.to/superviz/design-pattern-5-adapter-pattern-4gif?context=digest#differences-between-adapter-pattern-and-facade-pattern)Differences Between Adapter Pattern and Facade Pattern

While the Adapter Pattern and the [Facade Pattern](https://dev.to/superviz/design-pattern-2-facade-pattern-1dhl) might seem similar, they serve different purposes and are used in different contexts:

1.  **Purpose**:
    -   **Adapter Pattern**: The primary purpose of the Adapter Pattern is to make two incompatible interfaces compatible with each other. It allows an existing class with a different interface to be used as if it implemented a different interface.
    -   **Facade Pattern**: The main purpose of the Facade Pattern is to provide a simplified interface to a complex subsystem. It hides the complexities of the subsystem and provides a higher-level interface that makes the subsystem easier to use.
2.  **Usage**:
    -   **Adapter Pattern**: It is used when you need to integrate a new class or library that does not match the existing class or interface in your application. The Adapter Pattern is about adapting a particular interface to an expected interface.
    -   **Facade Pattern**: It is used when you want to simplify interactions with a complex subsystem. The Facade provides a straightforward method for interacting with the system, hiding its complexity.
3.  **Design**:
    -   **Adapter Pattern**: Typically involves creating a new class (the Adapter) that implements the interface expected by the client and translates calls to the adapted class.
    -   **Facade Pattern**: Involves creating a Facade class that provides simplified methods to the client, often aggregating multiple functionalities from the subsystem.

In summary, while both patterns provide a way to work with existing code, the Adapter Pattern focuses on interface compatibility, whereas the Facade Pattern focuses on simplifying the interaction with a complex system.

### [](https://dev.to/superviz/design-pattern-5-adapter-pattern-4gif?context=digest#super-invitation-win-5000)Super Invitation - Win $5.000

So, while you are here, let me invite you to participate in our upcoming [Superthis August](https://hackathon.superviz.com/)!

This remote event gives you the chance to showcase your skills and creativity by tackling the challenge to transform your virtual interactions with our real-time communication tools. With SuperViz, you stand a chance to win a prize of $5,000.

[Register now](https://hackathon.superviz.com/) to receive updates, tips and resources and get ready to hack!
