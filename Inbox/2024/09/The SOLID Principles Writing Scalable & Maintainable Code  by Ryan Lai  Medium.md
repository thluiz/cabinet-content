---
created: 2024-09-10T11:28:10 (UTC -03:00)
tags: []
source: https://forreya.medium.com/the-solid-principles-writing-scalable-maintainable-code-13040ada3bca
author: Ryan Lai
---

# The SOLID Principles: Writing Scalable & Maintainable Code | by Ryan Lai | Medium

> ## Excerpt
> Well if you have, there’s really nothing to be ashamed about. We all write flawed code as we learn. The good news is, it’s fairly straightforward to make improvements- but only if you’re willing. One…

---
[

![Ryan Lai](https://miro.medium.com/v2/resize:fill:88:88/1*VeL5UbDUoKNj1AjKTjw6EA.png)



](https://forreya.medium.com/?source=post_page-----13040ada3bca--------------------------------)

Has anyone ever told you that you write _“bad code?”_

Well if you have, there’s really nothing to be ashamed about. We all write flawed code as we learn. The good news is, it’s fairly straightforward to make improvements- but only if you’re willing.

One of the best ways to improve your code is by learning some programming design principles. You can think of programming principles as a general guide to becoming a better programmer- the raw philosophies of code, one could say. Now, there are a whole array of principles out there (one could argue there might even be an overabundance) but I will cover five essential ones which go under the acronym SOLID.

_Note: I will be using Python in my examples but these concepts are easily transferrable to other languages such as Java._

## **1\. First off… ‘S’ in SOLID stands for Single Responsibility**

![](https://miro.medium.com/v2/resize:fit:700/1*enX4PJGzy647ibd-GpSkBQ.png)

This principle teaches us to:

> **Break our code into modules of one responsibility each**.

Let’s take a look at this `Person` class which performs unrelated tasks such as sending emails and calculating taxes.

```
<span id="0116" data-selectable-paragraph=""><span>class</span> <span>Person</span>:<br>  <span>def</span> <span>__init__</span>(<span>self, name, age</span>):<br>    self.name = name<br>    self.age = age<br>  <br>    <span>def</span> <span>send_email</span>(<span>self, message</span>):<br>        <br>        <span>print</span>(<span>f"Sending email to <span>{self.name}</span>: <span>{message}</span>"</span>)<br><br>    <span>def</span> <span>calculate_tax</span>(<span>self</span>):<br>        <br>        tax = self.age * <span>100</span><br>        <span>print</span>(<span>f"<span>{self.name}</span>'s tax: <span>{tax}</span>"</span>)</span>
```

According to the **Single Responsibility Principle**, we should split the `Person` class up into several smaller classes to avoid violating the principle.

```
<span id="4056" data-selectable-paragraph=""><span>class</span> <span>Person</span>:<br>    <span>def</span> <span>__init__</span>(<span>self, name, age</span>):<br>        self.name = name<br>        self.age = age<br><br><span>class</span> <span>EmailSender</span>:<br>    <span>def</span> <span>send_email</span>(<span>person, message</span>):<br>        <br>        <span>print</span>(<span>f"Sending email to <span>{person.name}</span>: <span>{message}</span>"</span>)<br><br><span>class</span> <span>TaxCalculator</span>:<br>    <span>def</span> <span>calculate_tax</span>(<span>person</span>):<br>        <br>        tax = person.age * <span>100</span><br>        <span>print</span>(<span>f"<span>{person.name}</span>'s tax: <span>{tax}</span>"</span>)</span>
```

It’s more lines- sure- but now we can more easily identify what each section of the code is trying to accomplish, test it more cleanly, and reuse parts of it elsewhere (without worrying about irrelevant methods).

## **2\. Next up is ‘O’… or the Open/Closed Principle**

![](https://miro.medium.com/v2/resize:fit:700/1*xWwO2v121J8_FaBDmD_aUg.png)

This principle suggests that we design our modules to be able to:

> **Add new functionality in the future without directly modifying our existing code**.

Once a module is in use, it’s essentially locked, and this reduces the chances of any new additions breaking your code.

This is one of the hardest of the 5 principles to fully grasp due to its contradictory nature, so let’s take a look at an example:

```
<span id="6474" data-selectable-paragraph=""><span>class</span> <span>Shape</span>:<br>    <span>def</span> <span>__init__</span>(<span><span>self</span>, shape_type, width, height</span>):<br>        <span>self</span>.shape_type = shape_type<br>        <span>self</span>.width = width<br>        <span>self</span>.height = height<br><br>    <span>def</span> <span>calculate_area</span>(<span><span>self</span></span>):<br>        <span>if</span> <span>self</span>.shape_type == <span>"rectangle"</span>:<br>            <br>        elif <span>self</span>.shape_type == <span>"triangle"</span>:<br>            </span>
```

In the example above, the `Shape` class handles different shape types directly within its `calculate_area()` method. This violates the **Open/Closed Principle** because we are modifying the existing code instead of extending it.

This design is problematic because as more shape types are added, the `calculate_area()` method becomes more complex and harder to maintain. It violates the principle of separating responsibilities and makes the code less flexible and extensible. Let’s take a look at one way we could resolve this issue.

```
<span id="2176" data-selectable-paragraph=""><span>class</span> <span>Shape</span>:<br>    <span>def</span> <span>__init__</span>(<span><span>self</span>, width, height</span>):<br>        <span>self</span>.width = width<br>        <span>self</span>.height = height<br><br>    <span>def</span> <span>calculate_area</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>Rectangle</span>(Shape):<br>    <span>def</span> <span>calculate_area</span>(<span><span>self</span></span>):<br>        <br><br><span>class</span> <span>Triangle</span>(Shape):<br>    <span>def</span> <span>calculate_area</span>(<span><span>self</span></span>):<br>        </span>
```

In the example above, we define the base class `Shape`, whose sole purpose is to allow more specific shape classes to inherit its properties. For example, the `Triangle` class extends onto the `calculate_area()` method to calculate and return the area of a triangle.

By following the **Open/Closed Principle**, we can add new shapes without modifying the existing `Shape` class. This allows us to extend the functionality of the code without the need to change its core implementation.

## 3\. Now for ‘L’… the Liskov Substitution Principle (LSP)

![](https://miro.medium.com/v2/resize:fit:700/1*2PqRROjYtpjevNO2R0oa3w.png)

In this principle, Liskov is basically trying to tell us the following:

> **Subclasses should be able to be used interchangeably with their superclasses without breaking the functionality of the program.**

Now what does this actually mean? Let’s consider a `Vehicle` class with a method called `start_engine()`.

```
<span id="97bd" data-selectable-paragraph=""><span>class</span> <span>Vehicle</span>:<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>Car</span>(Vehicle):<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Car engine started."</span>)<br><br><span>class</span> <span>Motorcycle</span>(Vehicle):<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Motorcycle engine started."</span>)</span>
```

According to the **Liskov Substitution Principle,** any subclass of `Vehicle` should also be able to start the engine without any issues.

But if, for example, we added a `Bicycle` class. We obviously would no longer be able to start the engine because bicycles don’t have engines. Below demonstrates the incorrect way to resolve this issue.

```
<span id="1763" data-selectable-paragraph=""><span>class</span> <span>Bicycle</span>(Vehicle):<br>    <span>def</span> <span>ride</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Riding the bike."</span>)<br><br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>         <br>        raise NotImplementedError(<span>"Bicycle does not have an engine."</span>)</span>
```

To properly adhere to the LSP, we could take two routes. Let’s take a look at the first one.

**Solution 1:** `Bicycle` becomes its own class (without inheritance) to ensure that all `Vehicle` subclasses behave consistently with their superclass.

```
<span id="cd0e" data-selectable-paragraph=""><span>class</span> <span>Vehicle</span>:<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>Car</span>(Vehicle):<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Car engine started."</span>)<br><br><span>class</span> <span>Motorcycle</span>(Vehicle):<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Motorcycle engine started."</span>)<br><br><span>class</span> <span>Bicycle</span>():<br>    <span>def</span> <span>ride</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Riding the bike."</span>)</span>
```

**Solution 2:** The superclass `Vehicle` is split into two, one for vehicles with engines and another one for the latter. All subclasses can then be used interchangeably with their superclass without altering the expected behavior or introducing exceptions.

```
<span id="ce2b" data-selectable-paragraph=""><span>class</span> <span>VehicleWithEngines</span>:<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>VehicleWithoutEngines</span>:<br>    <span>def</span> <span>ride</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>Car</span>(VehicleWithEngines):<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Car engine started."</span>)<br><br><span>class</span> <span>Motorcycle</span>(VehicleWithEngines):<br>    <span>def</span> <span>start_engine</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Motorcycle engine started."</span>)<br><br><span>class</span> <span>Bicycle</span>(VehicleWithoutEngines):<br>    <span>def</span> <span>ride</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Riding the bike."</span>)</span>
```

## 4\. Next in line… ‘I’ for Interface Segregation

![](https://miro.medium.com/v2/resize:fit:700/1*zEgWeTUeyWssTLnyXjBALA.png)

The general definition states that our modules shouldn’t be forced to worry about functionalities that they don’t use. That’s a bit ambiguous though. Let’s transform this obscure sentence into a more concrete set of instructions:

> Client-specific interfaces are better than general-purpose ones. This means that classes should not be forced to depend on interfaces they don’t use. Instead, they should rely on smaller, more specific interfaces.

Let’s say we have an `Animal` interface with methods like `walk()`, `swim()`, and `fly()`.

```
<span id="d73f" data-selectable-paragraph=""><span>class</span> <span>Animal</span>:<br>    <span>def</span> <span>walk</span>(<span><span>self</span></span>):<br>        pass<br><br>    <span>def</span> <span>swim</span>(<span><span>self</span></span>):<br>        pass<br><br>    <span>def</span> <span>fly</span>(<span><span>self</span></span>):<br>        pass</span>
```

The thing is, not all animals can perform all these actions.

**For example:** Dogs can’t swim or fly and therefore both these methods which are inherited from the `Animal` interface are made redundant.

```
<span id="f5a8" data-selectable-paragraph=""><span>class</span> <span>Dog</span>(Animal):<br>    <br>    <span>def</span> <span>walk</span>(<span><span>self</span></span>):<br>        print(<span>"Dog is walking."</span>)<br><br><span>class</span> <span>Fish</span>(Animal):<br>    <br>    <span>def</span> <span>swim</span>(<span><span>self</span></span>):<br>        print(<span>"Fish is swimming."</span>)<br><br><span>class</span> <span>Bird</span>(Animal):<br>    <br>    <span>def</span> <span>walk</span>(<span><span>self</span></span>):<br>        print(<span>"Bird is walking."</span>)<br><br>    <span>def</span> <span>fly</span>(<span><span>self</span></span>):<br>        print(<span>"Bird is flying."</span>)</span>
```

We need to break our `Animal` interface down into smaller, more specific sub-categories, which we can then use to compose an exact set of functionality that each animal requires.

```
<span id="a1de" data-selectable-paragraph=""><span>class</span> <span>Walkable</span>:<br>    <span>def</span> <span>walk</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>Swimmable</span>:<br>    <span>def</span> <span>swim</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>Flyable</span>:<br>    <span>def</span> <span>fly</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>Dog</span>(Walkable):<br>    <span>def</span> <span>walk</span>(<span><span>self</span></span>):<br>        print(<span>"Dog is walking."</span>)<br><br><span>class</span> <span>Fish</span>(Swimmable):<br>    <span>def</span> <span>swim</span>(<span><span>self</span></span>):<br>        print(<span>"Fish is swimming."</span>)<br><br><span>class</span> <span>Bird</span>(Walkable, Flyable):<br>    <span>def</span> <span>walk</span>(<span><span>self</span></span>):<br>        print(<span>"Bird is walking."</span>)<br><br>    <span>def</span> <span>fly</span>(<span><span>self</span></span>):<br>        print(<span>"Bird is flying."</span>)</span>
```

By doing this, we achieve a design where classes only rely on the interfaces they need, **reducing unnecessary dependencies**. This becomes especially useful when testing, as it allows us to mock out only the functionality that each module requires.

## 5\. Which brings us to… ‘D’ for Dependency Inversion

![](https://miro.medium.com/v2/resize:fit:700/1*quGa1hK4EiHlPAUb7EcsTg.png)

This one is pretty straightforward to explain, it states:

> High-level modules should not rely directly on low-level modules. Instead, both should rely on abstractions (interfaces or abstract classes).

Once again, let’s look at an example. Suppose we have a `ReportGenerator` class that, naturally, generates reports. To perform this action, it needs to fetch data from a database first.

```
<span id="dcca" data-selectable-paragraph=""><span>class</span> <span>SQLDatabase</span>:<br>    <span>def</span> <span>fetch_data</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Fetching data from SQL database..."</span>)<br><br><span>class</span> <span>ReportGenerator</span>:<br>    <span>def</span> <span>__init__</span>(<span><span>self</span>, <span>database:</span> SQLDatabase</span>):<br>        <span>self</span>.database = database<br><br>    <span>def</span> <span>generate_report</span>(<span><span>self</span></span>):<br>        data = <span>self</span>.database.fetch_data()<br>        <br>        print(<span>"Generating report..."</span>)</span>
```

In this example, the `ReportGenerator` class directly depends on the concrete `SQLDatabase` class.

This works fine for now, but what if we wanted to switch to a different database such as MongoDB? This tight coupling would make it difficult to swap out the database implementation without modifying the `ReportGenerator` class.

To adhere to the Dependency Inversion Principle, we would introduce an abstraction (or interface) that both the `SQLDatabase` and `MongoDatabase` classes can depend on.

```
<span id="7a08" data-selectable-paragraph=""><span>class</span> <span>Database</span>():<br>    <span>def</span> <span>fetch_data</span>(<span><span>self</span></span>):<br>        pass<br><br><span>class</span> <span>SQLDatabase</span>(Database):<br>    <span>def</span> <span>fetch_data</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Fetching data from SQL database..."</span>)<br><br><span>class</span> <span>MongoDatabase</span>(Database):<br>    <span>def</span> <span>fetch_data</span>(<span><span>self</span></span>):<br>        <br>        print(<span>"Fetching data from Mongo database..."</span>)</span>
```

Note that the `ReportGenerator` class would now also depend on the new `Database` interface through its constructor.

```
<span id="75b7" data-selectable-paragraph=""><span>class</span> <span>ReportGenerator</span>:<br>    <span>def</span> <span>__init__</span>(<span><span>self</span>, <span>database:</span> Database</span>):<br>        <span>self</span>.database = database<br><br>    <span>def</span> <span>generate_report</span>(<span><span>self</span></span>):<br>        data = <span>self</span>.database.fetch_data()<br>        <br>        print(<span>"Generating report..."</span>)</span>
```

The high-level module (`ReportGenerator`) now does not depend on low-level modules (`SQLDatabase` or `MongoDatabase`) directly. Instead, they both depend on the interface (`Database`).

**Dependency Inversion** means our modules won’t need to know what implementation they are getting- only that they will receive certain inputs and return certain outputs.

## Conclusion

![](https://miro.medium.com/v2/resize:fit:700/1*Uj5STPDFeqNvmv32pdnRAA.png)

Nowadays I see a lot of discussion online about the SOLID design principles and whether they have withstood the test of time. In this modern world of multi-paradigm programming, cloud computing, and machine learning… **is SOLID still relevant?**

Personally, I believe that the SOLID principles will always be the basis of good code design. Sometimes the benefits of these principles may not be obvious when working with small applications, but once you start to work on larger-scale projects, the difference in code quality is way worth the effort of learning them. The modularity that SOLID promotes still makes these principles the foundation for modern software architecture, and personally, I don’t think that’s going to change anytime soon.

Thank you for reading.
