---
created: 2024-08-11T22:52:23 (UTC -03:00)
tags: []
source: https://towardsdev.com/prototype-design-pattern-in-c-copying-objects-without-dependency-63fae9db9083
author: Softinbit
---

# Prototype Design Pattern in C#: Copying Objects without Dependency | by Softinbit | Towards Dev

> ## Excerpt
> Implement the Prototype Design Pattern in C# for copting objects without dependency and its application in .NET and .NET Core. Master programming with design patterns.

---
[

![Softinbit](https://miro.medium.com/v2/resize:fill:88:88/1*VV7niUg3Hj1jFCcbBON-yg.png)



](https://softinbit.medium.com/?source=post_page-----63fae9db9083--------------------------------)[

![Towards Dev](https://miro.medium.com/v2/resize:fill:48:48/1*c2OaLMtxURd1SJZOGHALWA.png)



](https://towardsdev.com/?source=post_page-----63fae9db9083--------------------------------)

> “Prototype pattern is like a magic wand that lets you create new objects with a flick of a wrist, without worrying about the complexity of their construction.”

![](https://miro.medium.com/v2/resize:fit:875/1*VRcV_eQKC6fMzzE5SKF5Ew.jpeg)

_🔍Attention:_

_Before we start talking about design patterns, it’s important to be clear about what you can expect. These posts are like treasure maps, showing you where the gold is buried, but not handing you the shovel._

_We’ll give you a peek into the world of design patterns, but we won’t give you all the instructions for building something right away. Instead, we’ll explain the ideas behind them._

_Stay tuned for the next blog post where we’ll show you how to put these ideas into action. Until then, enjoy the journey and keep your eyes open for more insights! 🔍_

Hello fellow developers! Today, we’re going to talk about one of the most useful creational design patterns out there: the Prototype pattern. I’ve had my fair share of experience with creating complex solutions, and I can tell you that Prototype is a pattern that you’ll definitely want to have in your toolbox.

But what is the Prototype pattern, and why is it so useful? Well, let me give you a quick overview. The Prototype pattern allows you to copy existing objects without making your code dependent on their classes. This means that you can create new objects based on existing ones, without having to know anything about their internal structure or implementation. It’s like cloning an object, but with the ability to customize it to your specific needs.

Now, let’s get into some technical details. In C#, the Prototype pattern is implemented using the ICloneable interface. This interface defines a single method, Clone(), which returns a shallow copy of the current object. Shallow copying means that only the values of value-type fields and references to reference-type fields are copied. This means that if you have a reference-type field that points to another object, that object is not cloned.

Let’s take a look at an example. Suppose you have a class called Employee that looks like this:

```
<span id="54dd" data-selectable-paragraph=""><span>public</span> <span>class</span> <span>Employee</span> : <span>ICloneable</span><br>{<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> Age { <span>get</span>; <span>set</span>; }<br>    <span>public</span> Department Department { <span>get</span>; <span>set</span>; }<br><br>    <span><span>public</span> <span>object</span> <span>Clone</span>()</span><br>    {<br>        <span>return</span> MemberwiseClone();<br>    }<br>}</span>
```

As you can see, the Employee class implements the ICloneable interface and provides a Clone() method that simply calls MemberwiseClone(). This method creates a new object of the same type as the current object and copies all the fields from the current object to the new one.

Now, let’s say you have an Employee object named John that you want to clone. You can do this using the Clone() method like so:

```
<span id="7167" data-selectable-paragraph=""><span>Employee</span> <span>john</span> <span>=</span> <span>new</span> <span>Employee</span> { Name = <span>"John"</span>, Age = <span>30</span>, Department = <span>new</span> <span>Department</span> { Name = <span>"IT"</span> } };<br><span>Employee</span> <span>johnClone</span> <span>=</span> (Employee)john.Clone();</span>
```

In this example, we create an Employee object named John and set some properties on it. We also set the Department property to a new Department object. When we call the Clone() method on John, we get a new Employee object named JohnClone that has the same properties as John, including a reference to the same Department object.

Now, let’s modify the Department object in JohnClone and see what happens:

```
<span id="be32" data-selectable-paragraph=""><span>johnClone.Department.Name</span> = <span>"HR"</span></span>
```

If we now print out the Name property of the Department object in both John and JohnClone, we’ll see that they’re the same:

```
<span id="0909" data-selectable-paragraph="">Console<span>.WriteLine</span>(john.Department.Name);      <br>Console<span>.WriteLine</span>(johnClone.Department.Name); </span>
```

This is because the Department object was not cloned when we cloned the Employee object. Instead, both John and JohnClone have a reference to the same Department object.

So, what can we do to fix this? One solution is to implement deep cloning, which means that we create a new object for every reference-type field and recursively clone its sub-objects. Here’s an updated version of the Employee class that does deep cloning:

```
<span id="a6d5" data-selectable-paragraph=""><span>public</span> <span>class</span> <span>Employee</span> : <span>ICloneable</span><br>{<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> Age { <span>get</span>; <span>set</span>; }<br>    <span>public</span> Department Department { <span>get</span>; <span>set</span>; }<br><br>    <span><span>public</span> <span>object</span> <span>Clone</span>()</span><br>    {<br>        Employee clone = (Employee)MemberwiseClone();<br>        clone.Department = (Department)Department.Clone();<br>        <span>return</span> clone;<br>    }<br>}</span>
```

In this updated version of the Employee class, we first call MemberwiseClone() to create a shallow copy of the current object. We then create a new Department object by calling the Clone() method on the existing Department object, and assign it to the clone’s Department property. This ensures that we get a new Department object that’s completely independent of the original one.

Now, when we clone John and modify the Department object in the clone, we get the expected behavior:

```
<span id="8ad6" data-selectable-paragraph=""><span>Employee</span> <span>john</span> <span>=</span> <span>new</span> <span>Employee</span> { Name = <span>"John"</span>, Age = <span>30</span>, Department = <span>new</span> <span>Department</span> { Name = <span>"IT"</span> } };<br><span>Employee</span> <span>johnClone</span> <span>=</span> (Employee)john.Clone();<br>johnClone.Department.Name = <span>"HR"</span>;<br><br>Console.WriteLine(john.Department.Name);      <br>Console.WriteLine(johnClone.Department.Name); </span>
```

And there you have it, folks! That’s the Prototype pattern in action. By using the ICloneable interface and implementing deep cloning, we can easily create new objects based on existing ones without having to know anything about their internal structure or implementation. This makes our code more flexible, maintainable, and reusable.

The Prototype pattern has the following structure:

1.  Prototype: This is an interface or an abstract class that defines the basic methods for cloning itself. The clone method is typically the most important method in this class, and it allows the client to create a copy of the prototype object without knowing its concrete class.
2.  Concrete Prototype: This is a concrete implementation of the Prototype interface or abstract class. It provides the specific implementation details for cloning the object.
3.  Client: This is the class that creates new objects by cloning existing prototypes. The client does not need to know the concrete class of the prototype object, only its interface or abstract class.
4.  Client’s Context: This is the context in which the client operates. It typically contains a reference to the prototype object and calls the clone method to create new objects.

Here is a UML diagram that shows the structure of the Prototype pattern:

```
<span id="3e28" data-selectable-paragraph="">  +<br>  |   Prototype  |&lt;<br>  +<br>  | + Clone()    |                    | + Clone()           |<br>  +<br>          ^                                         ^<br>          |                                         |<br>          +<br>          |   Client     |              | ConcretePrototypeB  |<br>          +<br>          | + Context    |              | + Clone()           |<br>          +</span>
```

**_How to Implement It?_**

Here are the general steps for implementing the Prototype pattern in C#:

Define the Prototype interface or abstract class: This class should define the basic methods for cloning itself. The clone method is typically the most important method in this class, and it allows the client to create a copy of the prototype object without knowing its concrete class.

```
<span id="3120" data-selectable-paragraph=""><br><span>public</span> <span>interface</span> <span>IPrototype</span><br>{<br>    <span>IPrototype <span>Clone</span>()</span>;<br>}</span>
```

Implement the Concrete Prototype classes: These classes are concrete implementations of the Prototype interface or abstract class. They provide the specific implementation details for cloning the object.

```
<span id="1bbe" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>ConcretePrototypeA</span> : <span>IPrototype</span><br>{<br>    <span><span>public</span> IPrototype <span>Clone</span>()</span><br>    {<br>        <br>        <span>return</span> <span>new</span> ConcretePrototypeA();<br>    }<br>}<br><br><span>public</span> <span>class</span> <span>ConcretePrototypeB</span> : <span>IPrototype</span><br>{<br>    <span><span>public</span> IPrototype <span>Clone</span>()</span><br>    {<br>        <br>        <span>return</span> <span>new</span> ConcretePrototypeB();<br>    }<br>}</span>
```

Create a Client class: This class uses the Prototype interface or abstract class to create new objects by calling the Clone method on the prototype object. The client does not need to know the concrete class of the prototype object, only its interface or abstract class.

```
<span id="831a" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>Client</span><br>{<br>    <span>private</span> IPrototype _prototype;<br><br>    <span><span>public</span> <span>Client</span>(<span>IPrototype prototype</span>)</span><br>    {<br>        _prototype = prototype;<br>    }<br><br>    <span><span>public</span> IPrototype <span>CreatePrototype</span>()</span><br>    {<br>        <br>        <span>return</span> _prototype.Clone();<br>    }<br>}</span>
```

Use the Prototype pattern to create new objects: To create a new object, the client first creates an instance of the Concrete Prototype class and then calls the Clone method on that object to create a copy of it.

```
<span id="8f72" data-selectable-paragraph=""><br><br><span>ConcretePrototypeA</span> <span>prototypeA</span> <span>=</span> <span>new</span> <span>ConcretePrototypeA</span>();<br><br><br><span>Client</span> <span>clientA</span> <span>=</span> <span>new</span> <span>Client</span>(prototypeA);<br><br><br><span>IPrototype</span> <span>clonedA</span> <span>=</span> clientA.CreatePrototype();<br><br><br><span>ConcretePrototypeB</span> <span>prototypeB</span> <span>=</span> <span>new</span> <span>ConcretePrototypeB</span>();<br><br><br><span>Client</span> <span>clientB</span> <span>=</span> <span>new</span> <span>Client</span>(prototypeB);<br><br><br><span>IPrototype</span> <span>clonedB</span> <span>=</span> clientB.CreatePrototype();</span>
```

**_And what is it like?_**

The Prototype pattern could be like a cake recipe. Imagine you have a recipe for a delicious chocolate cake. You can use this recipe as a prototype to create multiple cakes without having to modify the original recipe. Each time you want to make a new cake, you simply clone the recipe and modify it as needed. For example, you might change the amount of sugar, add nuts, or substitute dark chocolate for milk chocolate.

Similarly, in software development, you can use the Prototype pattern to create new objects based on existing prototypes. Just like a recipe, a prototype object defines the basic structure and behavior of the new objects, but you can modify the cloned objects without affecting the original prototypes. This can be especially useful when you need to create multiple objects with similar properties or when you want to avoid tightly coupling your code to specific object classes.

**_When to use it?_**

1.  When creating objects is expensive: If creating a new object is time-consuming or resource-intensive, you can use the Prototype pattern to create new objects by cloning existing prototypes. This can save time and resources by avoiding the need to create new objects from scratch.
2.  When creating objects with complex internal structures: If an object has a complex internal structure that is difficult to recreate, the Prototype pattern can be useful for creating new objects that share the same structure. By cloning an existing prototype, you can ensure that the new object has the same internal structure without needing to recreate it from scratch.
3.  When you want to avoid tightly coupling your code to specific object classes: The Prototype pattern can be useful for creating objects without tightly coupling your code to specific object classes. By using a Prototype interface or abstract class, you can create new objects without knowing the specific concrete class of the prototype object.
4.  When you want to customize the creation of objects: The Prototype pattern allows you to customize the creation of new objects by modifying the properties of the cloned object. This can be useful for creating variations on existing objects without needing to create entirely new objects.

**_What it makes easier:_**

1.  Enables the creation of new objects by copying existing ones without having to know the concrete class of the object.
2.  Helps to reduce the number of classes required in a system, as the same code can be used to create many different objects.
3.  Allows for adding and removing object parts at runtime.
4.  Helps to encapsulate object creation and allows for flexible system design.

**_What it makes harder:_**

1.  Can make the code more complex, as it involves creating a hierarchy of classes and interfaces.
2.  Can lead to increased memory usage, as each clone of the prototype can have its own set of data and state.
3.  Can be difficult to maintain, as it requires that all prototype classes implement the clone method correctly.
4.  May require additional work to ensure that all dependencies and references are properly cloned.

**_Real World Examples :_**

Let’s say we’re working on a Warehouse Management System that allows users to define different types of storage locations (e.g. shelves, racks, bins) and configure their properties (e.g. size, weight capacity, temperature range). Instead of creating each storage location from scratch every time a user wants to add a new one, we can use the Prototype pattern to clone existing storage locations and make modifications as needed. This can save a lot of time and effort, especially when dealing with large warehouses with many different types of storage locations.

```
<span id="4af9" data-selectable-paragraph=""><br><span>public</span> <span>abstract</span> <span>class</span> <span>StorageLocation</span> : <span>ICloneable</span><br>{<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> Width { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> Height { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> Depth { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> WeightCapacity { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> TemperatureRange { <span>get</span>; <span>set</span>; }<br><br>    <br>    <span><span>public</span> <span>object</span> <span>Clone</span>()</span><br>    {<br>        <span>return</span> <span>this</span>.MemberwiseClone();<br>    }<br>}<br><br><br><span>public</span> <span>class</span> <span>Shelf</span> : <span>StorageLocation</span><br>{<br>    <span>public</span> <span>int</span> NumLevels { <span>get</span>; <span>set</span>; }<br>}<br><br><br><span>public</span> <span>class</span> <span>Rack</span> : <span>StorageLocation</span><br>{<br>    <span>public</span> <span>int</span> NumColumns { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>int</span> NumRows { <span>get</span>; <span>set</span>; }<br>}<br><br><br><span>public</span> <span>static</span> <span>class</span> <span>StorageLocationPrototype</span><br>{<br>    <span>public</span> <span>static</span> StorageLocation ShelfPrototype { <span>get</span>; } = <span>new</span> Shelf { Name = <span>"Standard Shelf"</span>, Width = <span>36</span>, Height = <span>72</span>, Depth = <span>18</span>, WeightCapacity = <span>500</span>, TemperatureRange = <span>70</span>, NumLevels = <span>5</span> };<br>    <span>public</span> <span>static</span> StorageLocation RackPrototype { <span>get</span>; } = <span>new</span> Rack { Name = <span>"Standard Rack"</span>, Width = <span>48</span>, Height = <span>84</span>, Depth = <span>24</span>, WeightCapacity = <span>1000</span>, TemperatureRange = <span>60</span>, NumColumns = <span>5</span>, NumRows = <span>3</span> };<br>}<br><br><br><span><span>public</span> <span>static</span> <span>void</span> <span>AddNewStorageLocation</span>(<span>StorageLocation prototype, <span>string</span> name, <span>int</span> width, <span>int</span> height, <span>int</span> depth, <span>int</span> weightCapacity, <span>int</span> temperatureRange</span>)</span><br>{<br>    <span>var</span> newLocation = (StorageLocation)prototype.Clone();<br>    newLocation.Name = name;<br>    newLocation.Width = width;<br>    newLocation.Height = height;<br>    newLocation.Depth = depth;<br>    newLocation.WeightCapacity = weightCapacity;<br>    newLocation.TemperatureRange = temperatureRange;<br>    <br>}</span>
```

Have a great coding!
