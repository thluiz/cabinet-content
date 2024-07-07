---
created: 2024-07-06T13:52:37 (UTC -03:00)
tags: [solidprinciples,javascript,softwaredevelopment,softwareengineering,software,coding,development,engineering,inclusive,community]
source: https://dev.to/alisamirali/mastering-solid-principles-1aa6?ref=dailydev
author: 
---

# Mastering SOLID Principles ✅ - DEV Community

> ## Excerpt
> The SOLID principles are design principles in object-oriented programming that help developers create...

---
The SOLID principles are design principles in object-oriented programming that help developers create more understandable, flexible, and maintainable software.

Let's dive into each principle and see how they can be applied using JavaScript.

___

## [](https://dev.to/alisamirali/mastering-solid-principles-1aa6?ref=dailydev#1-single-responsibility-principle-srp)📌 1. Single Responsibility Principle (SRP)

**Definition:** A class should have only one reason to change, meaning it should have only one job or responsibility.  

```
<span>class</span> <span>User</span> <span>{</span>
  <span>constructor</span><span>(</span><span>name</span><span>,</span> <span>email</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>name</span> <span>=</span> <span>name</span><span>;</span>
    <span>this</span><span>.</span><span>email</span> <span>=</span> <span>email</span><span>;</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>UserService</span> <span>{</span>
  <span>createUser</span><span>(</span><span>user</span><span>)</span> <span>{</span>
    <span>// logic to create user</span>
  <span>}</span>

  <span>getUser</span><span>(</span><span>id</span><span>)</span> <span>{</span>
    <span>// logic to get user</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>UserNotificationService</span> <span>{</span>
  <span>sendWelcomeEmail</span><span>(</span><span>user</span><span>)</span> <span>{</span>
    <span>// logic to send email</span>
  <span>}</span>
<span>}</span>

<span>const</span> <span>user</span> <span>=</span> <span>new</span> <span>User</span><span>(</span><span>'</span><span>John Doe</span><span>'</span><span>,</span> <span>'</span><span>john.doe@example.com</span><span>'</span><span>);</span>
<span>const</span> <span>userService</span> <span>=</span> <span>new</span> <span>UserService</span><span>();</span>
<span>userService</span><span>.</span><span>createUser</span><span>(</span><span>user</span><span>);</span>

<span>const</span> <span>notificationService</span> <span>=</span> <span>new</span> <span>UserNotificationService</span><span>();</span>
<span>notificationService</span><span>.</span><span>sendWelcomeEmail</span><span>(</span><span>user</span><span>);</span>
```

Here, `User` handles the user data, `UserService` handles user-related operations, and `UserNotificationService` handles notifications. Each class has a single responsibility.

___

## [](https://dev.to/alisamirali/mastering-solid-principles-1aa6?ref=dailydev#2-openclosed-principle-ocp)📌 2. Open/Closed Principle (OCP)

**Definition:** Software entities should be open for extension but closed for modification.  

```
<span>class</span> <span>Rectangle</span> <span>{</span>
  <span>constructor</span><span>(</span><span>width</span><span>,</span> <span>height</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>width</span> <span>=</span> <span>width</span><span>;</span>
    <span>this</span><span>.</span><span>height</span> <span>=</span> <span>height</span><span>;</span>
  <span>}</span>

  <span>area</span><span>()</span> <span>{</span>
    <span>return</span> <span>this</span><span>.</span><span>width</span> <span>*</span> <span>this</span><span>.</span><span>height</span><span>;</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>Circle</span> <span>{</span>
  <span>constructor</span><span>(</span><span>radius</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>radius</span> <span>=</span> <span>radius</span><span>;</span>
  <span>}</span>

  <span>area</span><span>()</span> <span>{</span>
    <span>return</span> <span>Math</span><span>.</span><span>PI</span> <span>*</span> <span>Math</span><span>.</span><span>pow</span><span>(</span><span>this</span><span>.</span><span>radius</span><span>,</span> <span>2</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>const</span> <span>shapes</span> <span>=</span> <span>[</span><span>new</span> <span>Rectangle</span><span>(</span><span>4</span><span>,</span> <span>5</span><span>),</span> <span>new</span> <span>Circle</span><span>(</span><span>3</span><span>)];</span>

<span>const</span> <span>totalArea</span> <span>=</span> <span>shapes</span><span>.</span><span>reduce</span><span>((</span><span>sum</span><span>,</span> <span>shape</span><span>)</span> <span>=&gt;</span> <span>sum</span> <span>+</span> <span>shape</span><span>.</span><span>area</span><span>(),</span> <span>0</span><span>);</span>
<span>console</span><span>.</span><span>log</span><span>(</span><span>totalArea</span><span>);</span>
```

In this example, each shape class's `area` method (like `Rectangle` and `Circle`) can be extended without modifying the existing code of the shape classes. This allows for adding new shapes in the future without changing the existing ones.

___

## [](https://dev.to/alisamirali/mastering-solid-principles-1aa6?ref=dailydev#3-liskov-substitution-principle-lsp)📌 3. Liskov Substitution Principle (LSP)

**Definition:** Subtypes must be substitutable for their base types without altering the correctness of the program.  

```
<span>class</span> <span>Bird</span> <span>{</span>
  <span>fly</span><span>()</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>I can fly</span><span>'</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>Duck</span> <span>extends</span> <span>Bird</span> <span>{}</span>

<span>class</span> <span>Ostrich</span> <span>extends</span> <span>Bird</span> <span>{</span>
  <span>fly</span><span>()</span> <span>{</span>
    <span>throw</span> <span>new</span> <span>Error</span><span>(</span><span>'</span><span>I cannot fly</span><span>'</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>function</span> <span>makeBirdFly</span><span>(</span><span>bird</span><span>)</span> <span>{</span>
  <span>bird</span><span>.</span><span>fly</span><span>();</span>
<span>}</span>

<span>const</span> <span>duck</span> <span>=</span> <span>new</span> <span>Duck</span><span>();</span>
<span>makeBirdFly</span><span>(</span><span>duck</span><span>);</span> <span>// Works fine</span>

<span>const</span> <span>ostrich</span> <span>=</span> <span>new</span> <span>Ostrich</span><span>();</span>
<span>makeBirdFly</span><span>(</span><span>ostrich</span><span>);</span> <span>// Throws error</span>
```

In this example, `Ostrich` violates LSP because it changes the expected behavior of the `fly` method. To comply with LSP, we should ensure that subclasses do not change the behavior expected by the base class.

___

## [](https://dev.to/alisamirali/mastering-solid-principles-1aa6?ref=dailydev#4-interface-segregation-principle-isp)📌 4. Interface Segregation Principle (ISP)

**Definition:** Clients should not be forced to depend on interfaces they do not use.  

```
<span>class</span> <span>Printer</span> <span>{</span>
  <span>print</span><span>()</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>Printing document</span><span>'</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>Scanner</span> <span>{</span>
  <span>scan</span><span>()</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>Scanning document</span><span>'</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>MultiFunctionPrinter</span> <span>{</span>
  <span>print</span><span>()</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>Printing document</span><span>'</span><span>);</span>
  <span>}</span>

  <span>scan</span><span>()</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>'</span><span>Scanning document</span><span>'</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>const</span> <span>printer</span> <span>=</span> <span>new</span> <span>Printer</span><span>();</span>
<span>printer</span><span>.</span><span>print</span><span>();</span>

<span>const</span> <span>scanner</span> <span>=</span> <span>new</span> <span>Scanner</span><span>();</span>
<span>scanner</span><span>.</span><span>scan</span><span>();</span>

<span>const</span> <span>multiFunctionPrinter</span> <span>=</span> <span>new</span> <span>MultiFunctionPrinter</span><span>();</span>
<span>multiFunctionPrinter</span><span>.</span><span>print</span><span>();</span>
<span>multiFunctionPrinter</span><span>.</span><span>scan</span><span>();</span>
```

Here, `Printer` and `Scanner` classes provide specific functionalities without forcing clients to implement methods they don't need. The `MultiFunctionPrinter` can use both functionalities, adhering to the ISP.

___

## [](https://dev.to/alisamirali/mastering-solid-principles-1aa6?ref=dailydev#5-dependency-inversion-principle-dip)📌 5. Dependency Inversion Principle (DIP)

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.  

```
<span>class</span> <span>NotificationService</span> <span>{</span>
  <span>constructor</span><span>(</span><span>sender</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>sender</span> <span>=</span> <span>sender</span><span>;</span>
  <span>}</span>

  <span>sendNotification</span><span>(</span><span>message</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>sender</span><span>.</span><span>send</span><span>(</span><span>message</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>EmailSender</span> <span>{</span>
  <span>send</span><span>(</span><span>message</span><span>)</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>`Sending email: </span><span>${</span><span>message</span><span>}</span><span>`</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>SMSSender</span> <span>{</span>
  <span>send</span><span>(</span><span>message</span><span>)</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>`Sending SMS: </span><span>${</span><span>message</span><span>}</span><span>`</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>const</span> <span>emailSender</span> <span>=</span> <span>new</span> <span>EmailSender</span><span>();</span>
<span>const</span> <span>notificationService</span> <span>=</span> <span>new</span> <span>NotificationService</span><span>(</span><span>emailSender</span><span>);</span>
<span>notificationService</span><span>.</span><span>sendNotification</span><span>(</span><span>'</span><span>Hello via Email</span><span>'</span><span>);</span>

<span>const</span> <span>smsSender</span> <span>=</span> <span>new</span> <span>SMSSender</span><span>();</span>
<span>const</span> <span>notificationServiceWithSMS</span> <span>=</span> <span>new</span> <span>NotificationService</span><span>(</span><span>smsSender</span><span>);</span>
<span>notificationServiceWithSMS</span><span>.</span><span>sendNotification</span><span>(</span><span>'</span><span>Hello via SMS</span><span>'</span><span>);</span>
```

In this example, the `NotificationService` depends on an abstraction (`sender`), allowing it to work with any sender implementation (like `EmailSender` or `SMSSender`). This adheres to DIP by making the high-level module (`NotificationService`) depend on abstractions rather than concrete implementations.

___

___

## [](https://dev.to/alisamirali/mastering-solid-principles-1aa6?ref=dailydev#conclusion)Conclusion ✅

By adhering to the SOLID principles, you can design JavaScript applications that are more robust, maintainable, and scalable.

These principles help to ensure that your codebase remains clean and flexible, making it easier to manage and extend as your application grows.

Applying these principles consistently can significantly improve the quality of your software.

___

**_Happy Coding!_** 🔥

**[LinkedIn](https://www.linkedin.com/in/dev-alisamir)**, **[X (Twitter)](https://twitter.com/dev_alisamir)**, **[Telegram](https://t.me/the_developer_guide)**, **[YouTube](https://www.youtube.com/@DevGuideAcademy)**, **[Discord](https://discord.gg/s37uutmxT2)**, **[Facebook](https://www.facebook.com/alisamir.dev)**, **[Instagram](https://www.instagram.com/alisamir.dev)**
