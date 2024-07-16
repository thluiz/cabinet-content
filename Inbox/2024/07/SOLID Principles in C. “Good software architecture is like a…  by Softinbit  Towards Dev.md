---
created: 2024-07-15T14:08:55 (UTC -03:00)
tags: []
source: https://towardsdev.com/solidifying-your-c-code-a-comprehensive-guide-to-the-solid-principles-de95337a515f
author: Softinbit
---

# SOLID Principles in C#. “Good software architecture is like a… | by Softinbit | Towards Dev

> ## Excerpt
> The SOLID principles are an acronym for five key design principles: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion. C#, .Net, .Net Core

---
[

![Softinbit](https://miro.medium.com/v2/resize:fill:88:88/1*VV7niUg3Hj1jFCcbBON-yg.png)



](https://softinbit.medium.com/?source=post_page-----de95337a515f--------------------------------)[

![Towards Dev](https://miro.medium.com/v2/resize:fill:48:48/1*c2OaLMtxURd1SJZOGHALWA.png)



](https://towardsdev.com/?source=post_page-----de95337a515f--------------------------------)

> “Good software architecture is like a good friend: it supports you, listens to you, and helps you achieve your goals. The SOLID principles provide the blueprint for building these kinds of relationships between your code components.”

![](https://miro.medium.com/v2/resize:fit:875/1*vNagDPWMSOnSc_VAdmOPzw.jpeg)

The SOLID principles are an acronym for five key design principles: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion. Let’s dive into each of these principles in detail, along with some examples and tips on how to use them in your C# projects.

**_S — Single Responsibility Principle_**

The Single Responsibility Principle (SRP) states that a class should have only one reason to change. In other words, a class should be responsible for one and only one thing. This helps keep classes focused and makes them easier to maintain and test.

For example, consider a class that is responsible for both user authentication and data retrieval. This violates the SRP because if we need to modify the authentication logic, we would also need to modify the data retrieval logic. Instead, we should create separate classes for authentication and data retrieval, each with its own responsibility.

Here’s an example of how to apply the SRP in C#:

```
<span id="813a" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>UserRepository</span><br>{<br>    <span><span>public</span> <span>bool</span> <span>Authenticate</span>(<span><span>string</span> username, <span>string</span> password</span>)</span><br>    {<br>        <br>    }<br><br>    <span><span>public</span> List&lt;User&gt; <span>GetUsers</span>()</span><br>    {<br>        <br>    }<br>}<br><br><br><span>public</span> <span>class</span> <span>UserAuthentication</span><br>{<br>    <span><span>public</span> <span>bool</span> <span>Authenticate</span>(<span><span>string</span> username, <span>string</span> password</span>)</span><br>    {<br>        <br>    }<br>}<br><br><span>public</span> <span>class</span> <span>UserRepository</span><br>{<br>    <span><span>public</span> List&lt;User&gt; <span>GetUsers</span>()</span><br>    {<br>        <br>    }<br>}</span>
```

**_O — Open/Closed Principle_**

The Open/Closed Principle (OCP) states that a class should be open for extension but closed for modification. In other words, we should be able to add new functionality to a class without changing its existing code.

For example, consider a class that calculates the total cost of an order. If we need to add a new discount for a specific type of customer, we should be able to do so without modifying the existing code.

Here’s an example of how to apply the OCP in C#:The Open/Closed Principle (OCP) states that a class should be open for extension but closed for modification. In other words, we should be able to add new functionality to a class without changing its existing code.

For example, consider a class that calculates the total cost of an order. If we need to add a new discount for a specific type of customer, we should be able to do so without modifying the existing code.

Here’s an example of how to apply the OCP in C#:

```
<span id="0499" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>Order</span><br>{<br>    <span><span>public</span> <span>decimal</span> <span>CalculateTotalCost</span>()</span><br>    {<br>        <br>    }<br><br>    <span><span>public</span> <span>decimal</span> <span>CalculateTotalCostWithDiscountForLoyalCustomers</span>()</span><br>    {<br>        <br>    }<br>}<br><br><br><span>public</span> <span>abstract</span> <span>class</span> <span>Order</span><br>{<br>    <span><span>public</span> <span>decimal</span> <span>CalculateTotalCost</span>()</span><br>    {<br>        <br>    }<br><br>    <span><span>public</span> <span>abstract</span> <span>decimal</span> <span>CalculateDiscount</span>()</span>;<br>}<br><br><span>public</span> <span>class</span> <span>LoyalCustomerOrder</span> : <span>Order</span><br>{<br>    <span><span>public</span> <span>override</span> <span>decimal</span> <span>CalculateDiscount</span>()</span><br>    {<br>        <br>    }<br>}</span>
```

**_L — Liskov Substitution Principle_**

The Liskov Substitution Principle (LSP) states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In other words, a subclass should be able to substitute its parent class without causing any unexpected behavior.

For example, consider a class that calculates the area of a rectangle. If we create a subclass that calculates the area of a square, it should still work correctly when substituted for the rectangle class.

Here’s an example of how to apply the LSP in C#:

```
<span id="4101" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>Rectangle</span>{<br><span>public</span> <span>virtual</span> <span>int</span> Width { <span>get</span>; <span>set</span>; }<br><span>public</span> <span>virtual</span> <span>int</span> Height { <span>get</span>; <span>set</span>; }<br><br><span><span>public</span> <span>int</span> <span>CalculateArea</span>()</span><br>{<br>    <span>return</span> Width * Height;<br>}<br><br>}<br><br><span>public</span> <span>class</span> <span>Square</span> : <span>Rectangle</span><br>{<br><span>public</span> <span>override</span> <span>int</span> Width<br>{<br><span>get</span> { <span>return</span> <span>base</span>.Width; }<br><span>set</span> { <span>base</span>.Width = <span>value</span>; <span>base</span>.Height = <span>value</span>; }<br>}<br><span>public</span> <span>override</span> <span>int</span> Height<br>{<br>    <span>get</span> { <span>return</span> <span>base</span>.Height; }<br>    <span>set</span> { <span>base</span>.Height = <span>value</span>; <span>base</span>.Width = <span>value</span>; }<br>}<br><br>}<br><br><br><span>public</span> <span>abstract</span> <span>class</span> <span>Shape</span><br>{<br><span><span>public</span> <span>abstract</span> <span>int</span> <span>CalculateArea</span>()</span>;<br>}<br><br><span>public</span> <span>class</span> <span>Rectangle</span> : <span>Shape</span><br>{<br><span>public</span> <span>int</span> Width { <span>get</span>; <span>set</span>; }<br><span>public</span> <span>int</span> Height { <span>get</span>; <span>set</span>; }<br><span><span>public</span> <span>override</span> <span>int</span> <span>CalculateArea</span>()</span><br>{<br>    <span>return</span> Width * Height;<br>}<br>}<br><br><span>public</span> <span>class</span> <span>Square</span> : <span>Shape</span><br>{<br><span>public</span> <span>int</span> Side { <span>get</span>; <span>set</span>; }<br><span><span>public</span> <span>override</span> <span>int</span> <span>CalculateArea</span>()</span><br>{<br>    <span>return</span> Side * Side;<br>}<br>}<br><br></span>
```

**_I — Interface Segregation Principle_**

The Interface Segregation Principle (ISP) states that a class should not be forced to implement interfaces it does not use. In other words, we should avoid creating bloated interfaces that contain unnecessary methods. For example, consider a class that implements an interface with many methods, but only needs to use a few of them.

This violates the ISP because the class is forced to implement unnecessary methods.

Here’s an example of how to apply the ISP in C#:

```
<span id="6202" data-selectable-paragraph=""><br><span>public</span> <span>interface</span> <span>IAnimal</span><br>{<br>    <span><span>void</span> <span>Walk</span>()</span>;<br>    <span><span>void</span> <span>Swim</span>()</span>;<br>    <span><span>void</span> <span>Fly</span>()</span>;<br>}<br><br><span>public</span> <span>class</span> <span>Dog</span> : <span>IAnimal</span><br>{<br>    <span><span>public</span> <span>void</span> <span>Walk</span>()</span><br>    {<br>        <br>    }<br><br>    <span><span>public</span> <span>void</span> <span>Swim</span>()</span><br>    {<br>        <span>throw</span> <span>new</span> NotImplementedException();<br>    }<br><br>    <span><span>public</span> <span>void</span> <span>Fly</span>()</span><br>    {<br>        <span>throw</span> <span>new</span> NotImplementedException();<br>    }<br>}<br><br><br><span>public</span> <span>interface</span> <span>IWalkableAnimal</span><br>{<br>    <span><span>void</span> <span>Walk</span>()</span>;<br>}<br><br><span>public</span> <span>interface</span> <span>ISwimmableAnimal</span><br>{<br>    <span><span>void</span> <span>Swim</span>()</span>;<br>}<br><br><span>public</span> <span>interface</span> <span>IFlyableAnimal</span><br>{<br>    <span><span>void</span> <span>Fly</span>()</span>;<br>}<br><br><span>public</span> <span>class</span> <span>Dog</span> : <span>IWalkableAnimal</span><br>{<br>    <span><span>public</span> <span>void</span> <span>Walk</span>()</span><br>    {<br>        <br>    }<br>}</span>
```

**_D — Dependency Inversion Principle_**

The Dependency Inversion Principle (DIP) states that high-level modules should not depend on low-level modules. Instead, both should depend on abstractions. In other words, we should depend on abstractions, not on concrete implementations.

For example, consider a class that depends on a concrete implementation of a database. If we want to switch to a different type of database, we would need to modify the class. Instead, we should depend on an abstraction of the database, which can be implemented by different types of databases.

Here’s an example of how to apply the DIP in C#:

```
<span id="5905" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>UserRepository</span><br>{<br>    <span>private</span> SqlDatabase _database;<br><br>    <span><span>public</span> <span>UserRepository</span>()</span><br>    {<br>        _database = <span>new</span> SqlDatabase();<br>    }<br><br>    <span><span>public</span> List&lt;User&gt; <span>GetUsers</span>()</span><br>    {<br>        <br>    }<br>}<br><br><br><span>public</span> <span>interface</span> <span>IDatabase</span><br>{<br>    <span>List&lt;User&gt; <span>GetUsers</span>()</span>;<br>}<br><br><span>public</span> <span>class</span> <span>SqlDatabase</span> : <span>IDatabase</span><br>{<br>    <span><span>public</span> List&lt;User&gt; <span>GetUsers</span>()</span><br>    {<br>        <br>    }<br>}<br><br><span>public</span> <span>class</span> <span>UserRepository</span><br>{<br>    <span>private</span> IDatabase _database;<br><br>    <span><span>public</span> <span>UserRepository</span>(<span>IDatabase database</span>)</span><br>    {<br>        _database = database;<br>    }<br><br>    <span><span>public</span> List&lt;User&gt; <span>GetUsers</span>()</span><br>{<br><span>return</span> _database.GetUsers();<br>}<br>}</span>
```

Tips and Tricks

— Keep your classes and methods focused on a single responsibility.

— Use interfaces to abstract away implementation details and increase flexibility.

— Favor composition over inheritance.

— Write tests to ensure that your code follows the SOLID principles.

— Be careful not to over-engineer your code.

Sometimes a simple solution is the best one. Conclusion The SOLID principles provide guidelines for writing maintainable, flexible, and robust code. By following these principles, we can create code that is easy to understand, modify, and extend. C# is a great language for implementing these principles, thanks to its support for interfaces, abstract classes, and dependency injection.

Remember, the SOLID principles are not rules set in stone, but rather guidelines that should be adapted to the needs of each project. So, always keep in mind the principles of SOLID, but don’t be afraid to deviate from them when necessary.

And, as a final note, always remember that even the best-designed code can’t make up for a lack of testing, proper debugging, or user feedback. So, keep an open mind, listen to your users, and iterate on your code to make it better over time.

Happy coding!
