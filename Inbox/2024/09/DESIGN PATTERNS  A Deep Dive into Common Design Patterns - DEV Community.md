---
created: 2024-09-06T14:31:34 (UTC -03:00)
tags: [systemdesign,designpatterns,webdev,java,software,coding,development,engineering,inclusive,community]
source: https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev
author: 
---

# DESIGN PATTERNS : A Deep Dive into Common Design Patterns - DEV Community

> ## Excerpt
> What is a design pattern?   Design patterns are solutions to complex problems. Design...

---
## [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#what-is-a-design-pattern)What is a design pattern?

Design patterns are solutions to complex problems. Design patterns are all about crafting your classes and interfaces in a way that solves a particular design problem. Usually, while designing a system, we encounter some issues, and for those problems, we have a set of design patterns. Design patterns are generally templates that involve classes, interfaces, and the relationships between those classes.

## [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#types-of-design-patterns)Types of design patterns:

### [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#creational-design-patterns)Creational design patterns:

These types of patterns deal with the creation of objects in a way that is compatible with the given situation.  
At the creational level, we can determine how specific parts of our systems can be created independently or composed together, ensuring flexibility and compatibility.  
The list of design patterns that fall under this category are:

-   **Singleton:** In this design pattern, we just have only one instance and that instance will be used throughout our application.

**Essentials of singleton design pattern:**

1.  **private constructor:** Marking constructor as private is very crucial, as we want to ensure that only one instance of the class is created.
2.  **Private Static Instance:** We use private access modifier,because we want to ensure that we only have one instance of the object in the class memory.
3.  **Public Static Method (Accessor):** This is a global access point to the single instance. This method basically creates the instance if it doesn't exist else return the same instance if it already exists.

**Example of Singleton design pattern**  

```
<span>public</span> <span>class</span> <span>Singleton</span> <span>{</span>
    <span>// Private static instance of the class</span>
    <span>private</span> <span>static</span> <span>Singleton</span> <span>instance</span><span>;</span>
    <span>private</span> <span>int</span> <span>count</span><span>;</span>

    <span>// Private constructor to prevent instantiation</span>
    <span>private</span> <span>Singleton</span><span>()</span> <span>{</span>
        <span>// initialization code</span>
    <span>}</span>

    <span>// Public static method to provide access to the instance</span>
    <span>public</span> <span>static</span> <span>synchronized</span> <span>Singleton</span> <span>getInstance</span><span>()</span> <span>{</span>
        <span>if</span> <span>(</span><span>instance</span> <span>==</span> <span>null</span><span>)</span> <span>{</span>
            <span>instance</span> <span>=</span> <span>new</span> <span>Singleton</span><span>();</span>
        <span>}</span>
        <span>return</span> <span>instance</span><span>;</span>
    <span>}</span>

    <span>// Example method</span>
    <span>public</span> <span>void</span> <span>getCount</span><span>()</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"The value of count is: "</span> <span>+</span> <span>count</span><span>);</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>increaseCount</span><span>()</span> <span>{</span>
        <span>count</span><span>++;</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>decreaseCount</span><span>()</span> <span>{</span>
        <span>count</span><span>--;</span>
    <span>}</span>
<span>}</span>

<span>public</span> <span>class</span> <span>Main</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>// Get the single instance of Singleton</span>
        <span>Singleton</span> <span>singleton</span> <span>=</span> <span>Singleton</span><span>.</span><span>getInstance</span><span>();</span>
        <span>singleton</span><span>.</span><span>increaseCount</span><span>();</span>
        <span>singleton</span><span>.</span><span>getCount</span><span>();</span>  <span>// Output: The value of count is: 1</span>

        <span>// Get the same instance of Singleton</span>
        <span>Singleton</span> <span>anotherSingleton</span> <span>=</span> <span>Singleton</span><span>.</span><span>getInstance</span><span>();</span>
        <span>anotherSingleton</span><span>.</span><span>decreaseCount</span><span>();</span>
        <span>anotherSingleton</span><span>.</span><span>getCount</span><span>();</span>  <span>// Output: The value of count is: 0</span>

        <span>// Both singleton and anotherSingleton refer to the same instance</span>
    <span>}</span>
<span>}</span>

```

-   **Builder:** In the Builder pattern, we define a way to create an object step-by-step. This pattern also provides flexibility, allowing different versions of the same object to be created using the same construction process.

**Key essentials of the builder pattern:**

1.  **Product:** It is the complex object that is being constructed.
2.  **Builder Interface:** Defines the methods to create different parts of the Product. These methods typically return the builder object itself to allow method chaining.
3.  **Concrete Builder:**Implements the Builder interface and provides specific implementations for creating parts of the Product.

**Example of builder pattern:**  
This example shows how to use the Builder Design Pattern to make a chocolate spread bread by adding ingredients step by step.  

```
<span>// Product Class</span>
<span>class</span> <span>Bread</span> <span>{</span>
    <span>private</span> <span>String</span> <span>bread</span><span>;</span>
    <span>private</span> <span>String</span> <span>spread</span><span>;</span>
    <span>private</span> <span>String</span> <span>chiaSeeds</span><span>;</span>
    <span>private</span> <span>String</span> <span>pumpkinSeeds</span><span>;</span>

    <span>public</span> <span>void</span> <span>setBread</span><span>(</span><span>String</span> <span>bread</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>bread</span> <span>=</span> <span>bread</span><span>;</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>setSpread</span><span>(</span><span>String</span> <span>spread</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>spread</span> <span>=</span> <span>spread</span><span>;</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>setChiaSeeds</span><span>(</span><span>String</span> <span>chiaSeeds</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>chiaSeeds</span> <span>=</span> <span>chiaSeeds</span><span>;</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>setPumpkinSeeds</span><span>(</span><span>String</span> <span>pumpkinSeeds</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>pumpkinSeeds</span> <span>=</span> <span>pumpkinSeeds</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>String</span> <span>toString</span><span>()</span> <span>{</span>
        <span>return</span> <span>"Bread with "</span> <span>+</span> <span>spread</span> <span>+</span> <span>", topped with "</span> <span>+</span> <span>chiaSeeds</span> <span>+</span> <span>" and "</span> <span>+</span> <span>pumpkinSeeds</span><span>;</span>
    <span>}</span>
<span>}</span>

<span>// Builder Interface</span>
<span>interface</span> <span>BreadBuilder</span> <span>{</span>
    <span>BreadBuilder</span> <span>addBread</span><span>();</span>
    <span>BreadBuilder</span> <span>addChocolateSpread</span><span>();</span>
    <span>BreadBuilder</span> <span>addChiaSeeds</span><span>();</span>
    <span>BreadBuilder</span> <span>addPumpkinSeeds</span><span>();</span>
    <span>Bread</span> <span>build</span><span>();</span>
<span>}</span>

<span>// Concrete Builder</span>
<span>class</span> <span>ChocolateBreadBuilder</span> <span>implements</span> <span>BreadBuilder</span> <span>{</span>
    <span>private</span> <span>Bread</span> <span>bread</span> <span>=</span> <span>new</span> <span>Bread</span><span>();</span>

    <span>@Override</span>
    <span>public</span> <span>BreadBuilder</span> <span>addBread</span><span>()</span> <span>{</span>
        <span>bread</span><span>.</span><span>setBread</span><span>(</span><span>"Whole grain bread"</span><span>);</span>
        <span>return</span> <span>this</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>BreadBuilder</span> <span>addChocolateSpread</span><span>()</span> <span>{</span>
        <span>bread</span><span>.</span><span>setSpread</span><span>(</span><span>"Chocolate spread"</span><span>);</span>
        <span>return</span> <span>this</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>BreadBuilder</span> <span>addChiaSeeds</span><span>()</span> <span>{</span>
        <span>bread</span><span>.</span><span>setChiaSeeds</span><span>(</span><span>"Chia seeds"</span><span>);</span>
        <span>return</span> <span>this</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>BreadBuilder</span> <span>addPumpkinSeeds</span><span>()</span> <span>{</span>
        <span>bread</span><span>.</span><span>setPumpkinSeeds</span><span>(</span><span>"Pumpkin seeds"</span><span>);</span>
        <span>return</span> <span>this</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>Bread</span> <span>build</span><span>()</span> <span>{</span>
        <span>return</span> <span>bread</span><span>;</span>
    <span>}</span>
<span>}</span>

<span>// Client Code</span>
<span>public</span> <span>class</span> <span>Main</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>// Create a builder and build the chocolate spread bread</span>
        <span>BreadBuilder</span> <span>builder</span> <span>=</span> <span>new</span> <span>ChocolateBreadBuilder</span><span>();</span>
        <span>Bread</span> <span>myBread</span> <span>=</span> <span>builder</span><span>.</span><span>addBread</span><span>()</span>
                               <span>.</span><span>addChocolateSpread</span><span>()</span>
                               <span>.</span><span>addChiaSeeds</span><span>()</span>
                               <span>.</span><span>addPumpkinSeeds</span><span>()</span>
                               <span>.</span><span>build</span><span>();</span>

        <span>// Output the result</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>myBread</span><span>);</span>
    <span>}</span>
<span>}</span>

```

-   **Factory Method:** In the Factory Method pattern, we define a way to create objects, but we allow subclasses to decide the specific type of object that will be created.

**Key essentials of factory pattern:**

1.  **Product Interface:** Defines the common interface for all products.
2.  **Concrete Products**: Implement the Product interface.
3.  **Creator:** Declares the factory method.
4.  **Concrete Creators:** Implement the factory method to return different concrete products.

```
<span>// Product Interface</span>
<span>interface</span> <span>Juice</span> <span>{</span>
    <span>void</span> <span>serve</span><span>();</span>
<span>}</span>

<span>// Concrete Product 1</span>
<span>class</span> <span>OrangeJuice</span> <span>implements</span> <span>Juice</span> <span>{</span>
    <span>@Override</span>
    <span>public</span> <span>void</span> <span>serve</span><span>()</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Serving Orange Juice."</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Concrete Product 2</span>
<span>class</span> <span>MangoJuice</span> <span>implements</span> <span>Juice</span> <span>{</span>
    <span>@Override</span>
    <span>public</span> <span>void</span> <span>serve</span><span>()</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Serving Mango Juice."</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Creator Abstract Class</span>
<span>abstract</span> <span>class</span> <span>JuiceFactory</span> <span>{</span>
    <span>// Factory method</span>
    <span>public</span> <span>abstract</span> <span>Juice</span> <span>createJuice</span><span>();</span>
<span>}</span>

<span>// Concrete Creator 1</span>
<span>class</span> <span>OrangeJuiceFactory</span> <span>extends</span> <span>JuiceFactory</span> <span>{</span>
    <span>@Override</span>
    <span>public</span> <span>Juice</span> <span>createJuice</span><span>()</span> <span>{</span>
        <span>return</span> <span>new</span> <span>OrangeJuice</span><span>();</span>
    <span>}</span>
<span>}</span>

<span>// Concrete Creator 2</span>
<span>class</span> <span>MangoJuiceFactory</span> <span>extends</span> <span>JuiceFactory</span> <span>{</span>
    <span>@Override</span>
    <span>public</span> <span>Juice</span> <span>createJuice</span><span>()</span> <span>{</span>
        <span>return</span> <span>new</span> <span>MangoJuice</span><span>();</span>
    <span>}</span>
<span>}</span>

<span>// Client Code</span>
<span>public</span> <span>class</span> <span>Main</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>// Create an Orange Juice using its factory</span>
        <span>JuiceFactory</span> <span>orangeJuiceFactory</span> <span>=</span> <span>new</span> <span>OrangeJuiceFactory</span><span>();</span>
        <span>Juice</span> <span>orangeJuice</span> <span>=</span> <span>orangeJuiceFactory</span><span>.</span><span>createJuice</span><span>();</span>
        <span>orangeJuice</span><span>.</span><span>serve</span><span>();</span>  <span>// Output: Serving Orange Juice.</span>

        <span>// Create a Mango Juice using its factory</span>
        <span>JuiceFactory</span> <span>mangoJuiceFactory</span> <span>=</span> <span>new</span> <span>MangoJuiceFactory</span><span>();</span>
        <span>Juice</span> <span>mangoJuice</span> <span>=</span> <span>mangoJuiceFactory</span><span>.</span><span>createJuice</span><span>();</span>
        <span>mangoJuice</span><span>.</span><span>serve</span><span>();</span>  <span>// Output: Serving Mango Juice.</span>
    <span>}</span>
<span>}</span>

```

### [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#structural-design-patterns)Structural design patterns

This design patterns mainly focuses on how classes and objects are composed to form larger structures. They focus on the organization and relationships between objects and classes, simplifying the structure, enhancing flexibility, and promoting maintainability.

-   **Adapter pattern:** In this pattern, we allow objects with incompatible interfaces to work together. It acts as a bridge between two incompatible interfaces, enabling them to communicate without altering their existing code.

**Key Essentials of the adapter pattern:**

1.  **Target Interface:** It is an interface that will solve the problem (bridging the gap between the incompatible interfaces).
2.  **Client:** The class or code that interacts with the target interface.
3.  **Adaptee:** This is the interface which is not compatible with the current client requirements.
4.  **Adapter:** Implements the target interface and contains an instance of the adaptee. It translates requests from the target interface to the adaptee’s interface, making them compatible.

```
<span>// Target Interface (Menu)</span>
<span>interface</span> <span>Menu</span> <span>{</span>
    <span>void</span> <span>orderDish</span><span>(</span><span>String</span> <span>dish</span><span>);</span>
<span>}</span>

<span>// Adaptee (Chef)</span>
<span>class</span> <span>Chef</span> <span>{</span>
    <span>public</span> <span>void</span> <span>prepareDish</span><span>(</span><span>String</span> <span>dishName</span><span>)</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Chef is preparing "</span> <span>+</span> <span>dishName</span> <span>+</span> <span>"."</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Adapter (Waiter)</span>
<span>class</span> <span>Waiter</span> <span>implements</span> <span>Menu</span> <span>{</span>
    <span>private</span> <span>Chef</span> <span>chef</span><span>;</span>

    <span>public</span> <span>Waiter</span><span>(</span><span>Chef</span> <span>chef</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>chef</span> <span>=</span> <span>chef</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>void</span> <span>orderDish</span><span>(</span><span>String</span> <span>dish</span><span>)</span> <span>{</span>
        <span>chef</span><span>.</span><span>prepareDish</span><span>(</span><span>dish</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Client Code</span>
<span>public</span> <span>class</span> <span>Restaurant</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>Chef</span> <span>chef</span> <span>=</span> <span>new</span> <span>Chef</span><span>();</span>
        <span>Menu</span> <span>waiter</span> <span>=</span> <span>new</span> <span>Waiter</span><span>(</span><span>chef</span><span>);</span>

        <span>// Customer places an order via the waiter</span>
        <span>waiter</span><span>.</span><span>orderDish</span><span>(</span><span>"Spaghetti Carbonara"</span><span>);</span>  <span>// Output: Chef is preparing Spaghetti Carbonara.</span>
    <span>}</span>
<span>}</span>

```

-   **Facade pattern:** Simplifies the interaction with a complex system by providing a unified interface (facade). Instead of directly calling several different methods across various objects, the client interacts with the facade, which internally manages those operations.

**Key essentials of the facade design pattern:**

1.  **Facade:** It is an interface that wraps all the complex subsystem interfaces and delegates the complex tasks to the subsystems that actually perform the work.
2.  **Subsystem Classes:** These are the classes that acutally perform the work.

**An example of facade design pattern:**  
The example illustrates the Facade Pattern which simplifies the process of washing, drying, and pressing clothes. It hides the complexity of interacting with multiple subsystems behind a single, unified interface.  

```
<span>// Subsystem Classes</span>
<span>class</span> <span>WashingMachine</span> <span>{</span>
    <span>public</span> <span>void</span> <span>wash</span><span>()</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Washing clothes."</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>class</span> <span>Dryer</span> <span>{</span>
    <span>public</span> <span>void</span> <span>dry</span><span>()</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Drying clothes."</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>class</span> <span>Iron</span> <span>{</span>
    <span>public</span> <span>void</span> <span>press</span><span>()</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Pressing clothes."</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Facade Class</span>
<span>class</span> <span>LaundryFacade</span> <span>{</span>
    <span>private</span> <span>WashingMachine</span> <span>washingMachine</span><span>;</span>
    <span>private</span> <span>Dryer</span> <span>dryer</span><span>;</span>
    <span>private</span> <span>Iron</span> <span>iron</span><span>;</span>

    <span>public</span> <span>LaundryFacade</span><span>(</span><span>WashingMachine</span> <span>washingMachine</span><span>,</span> <span>Dryer</span> <span>dryer</span><span>,</span> <span>Iron</span> <span>iron</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>washingMachine</span> <span>=</span> <span>washingMachine</span><span>;</span>
        <span>this</span><span>.</span><span>dryer</span> <span>=</span> <span>dryer</span><span>;</span>
        <span>this</span><span>.</span><span>iron</span> <span>=</span> <span>iron</span><span>;</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>doLaundry</span><span>()</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Starting the laundry process..."</span><span>);</span>
        <span>washingMachine</span><span>.</span><span>wash</span><span>();</span>
        <span>dryer</span><span>.</span><span>dry</span><span>();</span>
        <span>iron</span><span>.</span><span>press</span><span>();</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Laundry process complete."</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Client Code</span>
<span>public</span> <span>class</span> <span>Main</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>WashingMachine</span> <span>washingMachine</span> <span>=</span> <span>new</span> <span>WashingMachine</span><span>();</span>
        <span>Dryer</span> <span>dryer</span> <span>=</span> <span>new</span> <span>Dryer</span><span>();</span>
        <span>Iron</span> <span>iron</span> <span>=</span> <span>new</span> <span>Iron</span><span>();</span>

        <span>LaundryFacade</span> <span>laundryFacade</span> <span>=</span> <span>new</span> <span>LaundryFacade</span><span>(</span><span>washingMachine</span><span>,</span> <span>dryer</span><span>,</span> <span>iron</span><span>);</span>

        <span>// Use the facade to do the laundry</span>
        <span>laundryFacade</span><span>.</span><span>doLaundry</span><span>();</span>
    <span>}</span>
<span>}</span>

```

### [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#behavioral-design-patterns)Behavioral design patterns

The patterns that fall under this category mainly deals with communication between objects and how they interact with each other.

-   **Iterator pattern:** In the Iterator Pattern, we define a way to sequentially access elements of a collection without needing to use conventional methods, such as for loops or direct indexing. Instead, the pattern provides a standard interface (usually methods like next() and hasNext()) to traverse the collection. This approach abstracts the iteration process, allowing the client to navigate through the collection without needing to understand its internal structure or use traditional iteration methods.

#### [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#key-essentials-of-this-pattern-are)Key essentials of this pattern are:

1.  **Iterator Interface:** We define all the methods such as next(), hasNext(), and currentItem().These are used to traverse the collection.
2.  **Concrete Iterator:** This is the concrete implementation of the iterator interface.
3.  **Aggregate Interface:** In this interface,we define methods to create iterators.All the methods returns an instance of the Iterator.
4.  **Concrete Aggregate:** It's just a concrete implementation of the aggregate interface.

**Example of iterator pattern:**  
This example demostrates a simple usecase of iterators a employees object using iterator pattern.  

```
<span>// Iterator Interface</span>
<span>interface</span> <span>Iterator</span> <span>{</span>
    <span>boolean</span> <span>hasNext</span><span>();</span>
    <span>Object</span> <span>next</span><span>();</span>
<span>}</span>

<span>// Aggregate Interface </span>
<span>interface</span> <span>Aggregate</span> <span>{</span>
    <span>Iterator</span> <span>createIterator</span><span>();</span>
<span>}</span>

<span>// Employee Class</span>
<span>class</span> <span>Employee</span> <span>{</span>
    <span>public</span> <span>String</span> <span>Name</span><span>;</span>
    <span>public</span> <span>int</span> <span>Age</span><span>;</span>
    <span>public</span> <span>String</span> <span>Department</span><span>;</span>
    <span>public</span> <span>int</span> <span>EmployeeId</span><span>;</span>

    <span>public</span> <span>Employee</span><span>(</span><span>String</span> <span>name</span><span>,</span> <span>int</span> <span>age</span><span>,</span> <span>String</span> <span>department</span><span>,</span> <span>int</span> <span>employeeId</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>Name</span> <span>=</span> <span>name</span><span>;</span>
        <span>this</span><span>.</span><span>Age</span> <span>=</span> <span>age</span><span>;</span>
        <span>this</span><span>.</span><span>Department</span> <span>=</span> <span>department</span><span>;</span>
        <span>this</span><span>.</span><span>EmployeeId</span> <span>=</span> <span>employeeId</span><span>;</span>
    <span>}</span>
<span>}</span>

<span>// Concrete Aggregate</span>
<span>class</span> <span>EmployeeCollection</span> <span>implements</span> <span>Aggregate</span> <span>{</span>
    <span>private</span> <span>Employee</span><span>[]</span> <span>employees</span><span>;</span>

    <span>public</span> <span>EmployeeCollection</span><span>(</span><span>Employee</span><span>[]</span> <span>employees</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>employees</span> <span>=</span> <span>employees</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>Iterator</span> <span>createIterator</span><span>()</span> <span>{</span>
        <span>return</span> <span>new</span> <span>EmployeeIterator</span><span>(</span><span>this</span><span>.</span><span>employees</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Concrete Iterator</span>
<span>class</span> <span>EmployeeIterator</span> <span>implements</span> <span>Iterator</span> <span>{</span>
    <span>private</span> <span>Employee</span><span>[]</span> <span>employees</span><span>;</span>
    <span>private</span> <span>int</span> <span>position</span> <span>=</span> <span>0</span><span>;</span>

    <span>public</span> <span>EmployeeIterator</span><span>(</span><span>Employee</span><span>[]</span> <span>employees</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>employees</span> <span>=</span> <span>employees</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>boolean</span> <span>hasNext</span><span>()</span> <span>{</span>
        <span>return</span> <span>position</span> <span>&lt;</span> <span>employees</span><span>.</span><span>length</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>Object</span> <span>next</span><span>()</span> <span>{</span>
        <span>return</span> <span>hasNext</span><span>()</span> <span>?</span> <span>employees</span><span>[</span><span>position</span><span>++].</span><span>Name</span> <span>:</span> <span>null</span><span>;</span>
    <span>}</span>
<span>}</span>

<span>// Client Code</span>
<span>public</span> <span>class</span> <span>Main</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>// Creating employee array</span>
        <span>Employee</span><span>[]</span> <span>employees</span> <span>=</span> <span>{</span>
            <span>new</span> <span>Employee</span><span>(</span><span>"John"</span><span>,</span> <span>28</span><span>,</span> <span>"Engineering"</span><span>,</span> <span>101</span><span>),</span>
            <span>new</span> <span>Employee</span><span>(</span><span>"Jane"</span><span>,</span> <span>32</span><span>,</span> <span>"Marketing"</span><span>,</span> <span>102</span><span>),</span>
            <span>new</span> <span>Employee</span><span>(</span><span>"Tom"</span><span>,</span> <span>25</span><span>,</span> <span>"Sales"</span><span>,</span> <span>103</span><span>)</span>
        <span>};</span>

        <span>// Creating employee collection and iterator</span>
        <span>EmployeeCollection</span> <span>employeeCollection</span> <span>=</span> <span>new</span> <span>EmployeeCollection</span><span>(</span><span>employees</span><span>);</span>
        <span>Iterator</span> <span>iterator</span> <span>=</span> <span>employeeCollection</span><span>.</span><span>createIterator</span><span>();</span>

        <span>// Iterating through employees</span>
        <span>while</span> <span>(</span><span>iterator</span><span>.</span><span>hasNext</span><span>())</span> <span>{</span>
            <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>iterator</span><span>.</span><span>next</span><span>());</span>
        <span>}</span>
    <span>}</span>
<span>}</span>

```

-   **Strategy pattern:** In this pattern we define a family of algorithms, and at the runtime we choose the algorithm.Instead of implementing a single algorithm directly, the code receives runtime instructions on which algorithm to use from a family of algorithms. This pattern allows the algorithm to vary independently from the clients that use it.

#### [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#key-essentials-of-this-pattern-are)Key essentials of this pattern are:

1.**Strategy Interface:** Defines the common interface for all supported algorithms.  
2.**Concrete Strategies:** Implement the Strategy interface with specific algorithms.  
3.**Context:** Uses a Strategy to execute the algorithm.

**Example of strategy pattern:**  
Imagine we are building an encoding system where we may need to use different encoding algorithms depending on the situation. We will demonstrate this system using the Strategy Pattern.  

```
<span>// Strategy Interface</span>
<span>interface</span> <span>EncoderStrategy</span> <span>{</span>
    <span>void</span> <span>encode</span><span>(</span><span>String</span> <span>string</span><span>);</span>
<span>}</span>

<span>// Concrete Strategy for Base64 Encoding</span>
<span>class</span> <span>Base64Encoder</span> <span>implements</span> <span>EncoderStrategy</span> <span>{</span>
    <span>@Override</span>
    <span>public</span> <span>void</span> <span>encode</span><span>(</span><span>String</span> <span>string</span><span>)</span> <span>{</span>
        <span>// Implement Base64 encoding logic here</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"This method uses Base64 encoding algorithm for: "</span> <span>+</span> <span>string</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Concrete Strategy for MD5 Encoding</span>
<span>class</span> <span>MD5Encoder</span> <span>implements</span> <span>EncoderStrategy</span> <span>{</span>
    <span>@Override</span>
    <span>public</span> <span>void</span> <span>encode</span><span>(</span><span>String</span> <span>string</span><span>)</span> <span>{</span>
        <span>// Implement MD5 encoding logic here</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"This method uses MD5 encoding algorithm for: "</span> <span>+</span> <span>string</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Context Class</span>
<span>class</span> <span>EncoderContext</span> <span>{</span>
    <span>private</span> <span>EncoderStrategy</span> <span>strategy</span><span>;</span>

    <span>public</span> <span>void</span> <span>setEncoderMethod</span><span>(</span><span>EncoderStrategy</span> <span>strategy</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>strategy</span> <span>=</span> <span>strategy</span><span>;</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>encode</span><span>(</span><span>String</span> <span>string</span><span>)</span> <span>{</span>
        <span>strategy</span><span>.</span><span>encode</span><span>(</span><span>string</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Usage</span>
<span>public</span> <span>class</span> <span>Main</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>EncoderContext</span> <span>context</span> <span>=</span> <span>new</span> <span>EncoderContext</span><span>();</span>

        <span>// Use Base64 encoding method</span>
        <span>context</span><span>.</span><span>setEncoderMethod</span><span>(</span><span>new</span> <span>Base64Encoder</span><span>());</span>
        <span>context</span><span>.</span><span>encode</span><span>(</span><span>"A34937ifdsuhfweiur"</span><span>);</span>

        <span>// Use MD5 encoding method</span>
        <span>context</span><span>.</span><span>setEncoderMethod</span><span>(</span><span>new</span> <span>MD5Encoder</span><span>());</span>
        <span>context</span><span>.</span><span>encode</span><span>(</span><span>"89743297dfhksdhWOJO"</span><span>);</span>
    <span>}</span>
<span>}</span>

```

**Explanation:**

1.  Firstly, we define the interface for the
2.  Next, we create concrete implementations of the interfaces that we have defined.
3.  Finally, we use these implementations to observe how a change in the subject also updates its dependents.

-   **Observer pattern:** behavioral design pattern that establishes a one-to-many dependency between objects. This means that when one object (the subject) changes its state, all its dependent objects (observers) are notified and updated automatically. This pattern is particularly useful for implementing distributed event-handling systems in event-driven software.

#### [](https://dev.to/niharikaa/design-patterns-a-deep-dive-into-common-design-patterns-31b9?ref=dailydev#key-essentials-of-this-pattern-are)Key essentials of this pattern are:

1.  **Subject:** It is an object which holds the state and informs the observers when it updates it's state.
2.  **Observer:** An interface or abstract class that defines the update method, which is called when the subject’s state changes.
3.  **Concrete Subject:** A class that implements the Subject interface and maintains the state of interest to observers.
4.  **Concrete Observer:** A class that implements the Observer interface and updates its state to match the subject’s state.

**Example of observer pattern:**  
In a stock trading application, the stock ticker acts as the subject. Whenever the price of a stock is updated, various observers—such as investors and regulatory bodies—are notified of the change. This allows them to respond to price fluctuations in real-time.  

```
<span>import</span> <span>java.util.ArrayList</span><span>;</span>
<span>import</span> <span>java.util.List</span><span>;</span>

<span>// Observer interface</span>
<span>interface</span> <span>Observer</span> <span>{</span>
    <span>void</span> <span>update</span><span>(</span><span>String</span> <span>stockSymbol</span><span>,</span> <span>double</span> <span>stockPrice</span><span>);</span>
<span>}</span>

<span>// Subject interface</span>
<span>interface</span> <span>Subject</span> <span>{</span>
    <span>void</span> <span>register</span><span>(</span><span>Observer</span> <span>o</span><span>);</span>
    <span>void</span> <span>remove</span><span>(</span><span>Observer</span> <span>o</span><span>);</span>
    <span>void</span> <span>notify</span><span>();</span>
<span>}</span>

<span>// Concrete Subject</span>
<span>class</span> <span>Stock</span> <span>implements</span> <span>Subject</span> <span>{</span>
    <span>private</span> <span>List</span><span>&lt;</span><span>Observer</span><span>&gt;</span> <span>observers</span><span>;</span>
    <span>private</span> <span>String</span> <span>stockSymbol</span><span>;</span>
    <span>private</span> <span>double</span> <span>stockPrice</span><span>;</span>

    <span>public</span> <span>Stock</span><span>()</span> <span>{</span>
        <span>observers</span> <span>=</span> <span>new</span> <span>ArrayList</span><span>&lt;&gt;();</span>
    <span>}</span>

    <span>public</span> <span>void</span> <span>setStock</span><span>(</span><span>String</span> <span>stockSymbol</span><span>,</span> <span>double</span> <span>stockPrice</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>stockSymbol</span> <span>=</span> <span>stockSymbol</span><span>;</span>
        <span>this</span><span>.</span><span>stockPrice</span> <span>=</span> <span>stockPrice</span><span>;</span>
        <span>notify</span><span>();</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>void</span> <span>register</span><span>(</span><span>Observer</span> <span>o</span><span>)</span> <span>{</span>
        <span>observers</span><span>.</span><span>add</span><span>(</span><span>o</span><span>);</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>void</span> <span>remove</span><span>(</span><span>Observer</span> <span>o</span><span>)</span> <span>{</span>
        <span>observers</span><span>.</span><span>remove</span><span>(</span><span>o</span><span>);</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>void</span> <span>notify</span><span>()</span> <span>{</span>
        <span>for</span> <span>(</span><span>Observer</span> <span>observer</span> <span>:</span> <span>observers</span><span>)</span> <span>{</span>
            <span>observer</span><span>.</span><span>update</span><span>(</span><span>stockSymbol</span><span>,</span> <span>stockPrice</span><span>);</span>
        <span>}</span>
    <span>}</span>
<span>}</span>

<span>// Concrete Observer</span>
<span>class</span> <span>StockTrader</span> <span>implements</span> <span>Observer</span> <span>{</span>
    <span>private</span> <span>String</span> <span>traderName</span><span>;</span>

    <span>public</span> <span>StockTrader</span><span>(</span><span>String</span> <span>traderName</span><span>)</span> <span>{</span>
        <span>this</span><span>.</span><span>traderName</span> <span>=</span> <span>traderName</span><span>;</span>
    <span>}</span>

    <span>@Override</span>
    <span>public</span> <span>void</span> <span>update</span><span>(</span><span>String</span> <span>stockSymbol</span><span>,</span> <span>double</span> <span>stockPrice</span><span>)</span> <span>{</span>
        <span>System</span><span>.</span><span>out</span><span>.</span><span>println</span><span>(</span><span>"Trader "</span> <span>+</span> <span>traderName</span> <span>+</span> <span>" notified. Stock: "</span> <span>+</span> <span>stockSymbol</span> <span>+</span> <span>" is now $"</span> <span>+</span> <span>stockPrice</span><span>);</span>
    <span>}</span>
<span>}</span>

<span>// Usage</span>
<span>public</span> <span>class</span> <span>Main</span> <span>{</span>
    <span>public</span> <span>static</span> <span>void</span> <span>main</span><span>(</span><span>String</span><span>[]</span> <span>args</span><span>)</span> <span>{</span>
        <span>Stock</span> <span>stock</span> <span>=</span> <span>new</span> <span>Stock</span><span>();</span>

        <span>StockTrader</span> <span>trader1</span> <span>=</span> <span>new</span> <span>StockTrader</span><span>(</span><span>"Niharika"</span><span>);</span>
        <span>StockTrader</span> <span>trader2</span> <span>=</span> <span>new</span> <span>StockTrader</span><span>(</span><span>"Goulikar"</span><span>);</span>

        <span>stock</span><span>.</span><span>register</span><span>(</span><span>trader1</span><span>);</span>
        <span>stock</span><span>.</span><span>register</span><span>(</span><span>trader2</span><span>);</span>

        <span>stock</span><span>.</span><span>setStock</span><span>(</span><span>"Niha"</span><span>,</span> <span>9500.00</span><span>);</span>
        <span>stock</span><span>.</span><span>setStock</span><span>(</span><span>"Rika"</span><span>,</span> <span>2800.00</span><span>);</span>
    <span>}</span>
<span>}</span>
```

**Explanation:**

-   Firstly, we define the interface for the subject which is responsible for sending updates. Similarly, we also define an interface for the observer, which is responsible for receiving updates.
-   Next, we create concrete implementations of the interfaces that we have defined.
-   Finally, we use these implementations to observe how a change in the subject also updates its dependents.
