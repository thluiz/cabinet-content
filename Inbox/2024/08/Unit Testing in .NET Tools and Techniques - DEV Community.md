---
created: 2024-08-21T14:42:18 (UTC -03:00)
tags: [dotnet,csharp,unittest,software,coding,development,engineering,inclusive,community]
source: https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619
author: 
---

# Unit Testing in .NET: Tools and Techniques - DEV Community

> ## Excerpt
> Introduction   Imagine you are building a LEGO structure. As you add more pieces you want to...

---
### [](https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619#introduction)Introduction

Imagine you are building a LEGO structure. As you add more pieces you want to make sure each new piece fits perfectly without compromising the entire structure. In software development unit testing is like checking each LEGO piece before adding it to ensure the whole structure remains strong.

Unit testing is a fundamental practice that help us to ensure that each part of our code works correctly. In this article I will try to explain what unit testing is, why it is important and how we can perform unit testing in .NET using simple tools and techniques.

### [](https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619#what-is-unit-testing)What is Unit testing?

Unit testing involves testing individual units of code like functions or methods to verify that they perform as expected. Think of a unit test as a small, automated checklist that confirms whether a specific part of your program works correctly.

For example, if you have a method that calculates the division of two numbers, a unit test would check if the method returns the correct result when given specific inputs.

### [](https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619#why-is-unit-testing-important)Why is Unit Testing Important?

Lets shift our analogy and please imagine we are building a car. We have to test individual components like the engine, brakes, lights and so on to ensure they function properly on their own. Unit testing serves a similar purpose in software development.

There are several reasons why this is important, but the most significant are:

-   Catching bugs early, Much like identifying a faulty car component early on, unit testing helps us detect errors in our code before they escalate into more significant issues
-   Easier maintenance, As we keep adding or changing features in our application unit tests ensure that new changes don't break existing functionality
-   Confidence in code, Unit tests give us confidence that our code works as intended, making it easier to refactor and extend our application

### [](https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619#getting-started-with-unit-testing-in-net)Getting Started with Unit Testing in .NET

Enough with the theory and lets take a look in a practical example, after all it's well known that we developers prefer code over the theory that comes with it 😁

For my tests I will use the xUnit framework.

1.  Setting up a Unit Test Project In .NET, setting up unit testing is straightforward. When we create a new project in our favorite IDE, we can add a unit test project alongside it.
    -   Create a new project, In our IDE we select to Create a new project and the we have to choose the desirable framework "xUnit" or "NUnit", at the moment these are the most popular testing frameworks in .NET.
    -   Add a reference, Ensure that unit test project references the project containing the code you want to test.
2.  Writing our first Unit Test Let's say we have a simple method in our code that divides two numbers

```
<span>public</span> <span>class</span> <span>Calculator</span>
<span>{</span>
    <span>public</span> <span>int</span> <span>Div</span><span>(</span><span>int</span> <span>numerator</span><span>,</span> <span>int</span> <span>denominator</span><span>)</span>
    <span>{</span>
        <span>return</span> <span>numerator</span> <span>/</span> <span>denominator</span><span>;</span>
    <span>}</span>
<span>}</span>
```

To test this method we will write a unit test like the following:  

```
<span>public</span> <span>class</span> <span>CalculatorTests</span>
<span>{</span>
    <span>[</span><span>Fact</span><span>]</span>
    <span>public</span> <span>void</span> <span>Div_ReturnsCorrectResult</span><span>()</span>
    <span>{</span>
        <span>// Arrange</span>
        <span>Calculator</span> <span>calculator</span> <span>=</span> <span>new</span> <span>Calculator</span><span>();</span>

        <span>int</span> <span>numerator</span> <span>=</span> <span>10</span><span>;</span>
        <span>int</span> <span>denominator</span> <span>=</span> <span>2</span><span>;</span>

        <span>// Act</span>
        <span>int</span> <span>result</span> <span>=</span> <span>calculator</span><span>.</span><span>Div</span><span>(</span><span>numerator</span><span>,</span> <span>denominator</span><span>);</span>

        <span>// Assert</span>
        <span>Assert</span><span>.</span><span>Equal</span><span>(</span><span>5</span><span>,</span> <span>result</span><span>);</span>
    <span>}</span>
<span>}</span>
```

Lets break it down into pieces so we can better understand it:

First of all, as you can easily see, I have deliberately divided the test into three parts: _Arrange_, _Act_, and _Assert_. The reason for this is that it is considered good practice in our tests to use the AAA pattern. This way, we keep our tests better organized, making them easier to read and understand.

-   Arrange, in this section we prepare any objects or data needed for the test, like creating an instance of the calculator object or assign values to the `numerator` and `denominator` variables
-   Act, here we are calling the method that we want to test, `Div` in our case
-   Assert, we check if the result is what we expected, in our case if `10 / 2` should equal `5`

### [](https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619#testing-multiple-inputs)Testing Multiple Inputs

In the example above, we created our test and ran it with a single set of inputs. But how can we run the test again using a different set of inputs? .NET testing frameworks offer a way to accomplish this without having to rewrite the entire test.

Example with the use of xUnit  

```
<span>[</span><span>Theory</span><span>]</span>
<span>[</span><span>InlineData</span><span>(</span><span>10</span><span>,</span> <span>2</span><span>,</span> <span>5</span><span>)]</span>     <span>// Test case: 10 / 2 = 5</span>
<span>[</span><span>InlineData</span><span>(</span><span>20</span><span>,</span> <span>4</span><span>,</span> <span>5</span><span>)]</span>     <span>// Test case: 20 / 4 = 5</span>
<span>[</span><span>InlineData</span><span>(</span><span>0</span><span>,</span> <span>1</span><span>,</span> <span>0</span><span>)]</span>      <span>// Test case: 0 / 1 = 0</span>
<span>[</span><span>InlineData</span><span>(-</span><span>10</span><span>,</span> <span>-</span><span>2</span><span>,</span> <span>5</span><span>)]</span>   <span>// Test case: -10 / -2 = 5</span>
<span>public</span> <span>void</span> <span>Div_ReturnsCorrectResult</span><span>(</span>
    <span>int</span> <span>numerator</span><span>,</span>
    <span>int</span> <span>denominator</span><span>,</span>
    <span>int</span> <span>expected</span><span>)</span>
    <span>{</span>
        <span>// Arrange</span>
        <span>Calculator</span> <span>calculator</span> <span>=</span> <span>new</span> <span>Calculator</span><span>();</span>

        <span>// Act</span>
        <span>int</span> <span>result</span> <span>=</span> <span>calculator</span><span>.</span><span>Div</span><span>(</span><span>numerator</span><span>,</span> <span>denominator</span><span>);</span>

        <span>// Assert</span>
        <span>Assert</span><span>.</span><span>Equal</span><span>(</span><span>expected</span><span>,</span> <span>result</span><span>);</span>
    <span>}</span>
```

Here we are using `[Theory]` and `[InlineData]` instead of the `[Fact]` for parameterized tests. Each `[InlineData]` attribute provides a different set of inputs to the test method

Similarly in `NUnit` we can run tests with multiple inputs by using the `[TestCase]` attribute  

```
<span>[</span><span>TestCase</span><span>(</span><span>10</span><span>,</span> <span>2</span><span>,</span> <span>5</span><span>)]</span>     <span>// Test case: 10 / 2 = 5</span>
<span>[</span><span>TestCase</span><span>(</span><span>20</span><span>,</span> <span>4</span><span>,</span> <span>5</span><span>)]</span>     <span>// Test case: 20 / 4 = 5</span>
<span>[</span><span>TestCase</span><span>(</span><span>0</span><span>,</span> <span>1</span><span>,</span> <span>0</span><span>)]</span>      <span>// Test case: 0 / 1 = 0</span>
<span>[</span><span>TestCase</span><span>(-</span><span>10</span><span>,</span> <span>-</span><span>2</span><span>,</span> <span>5</span><span>)]</span>   <span>// Test case: -10 / -2 = 5</span>
<span>public</span> <span>void</span> <span>Div_ReturnsCorrectResult</span><span>(</span>
    <span>int</span> <span>numerator</span><span>,</span>
    <span>int</span> <span>denominator</span><span>,</span>
    <span>int</span> <span>expected</span><span>)</span>
    <span>{</span>
        <span>// Arrange</span>
        <span>Calculator</span> <span>calculator</span> <span>=</span> <span>new</span> <span>Calculator</span><span>();</span>

        <span>// Act</span>
        <span>int</span> <span>result</span> <span>=</span> <span>calculator</span><span>.</span><span>Div</span><span>(</span><span>numerator</span><span>,</span> <span>denominator</span><span>);</span>

        <span>// Assert</span>
        <span>Assert</span><span>.</span><span>AreEqual</span><span>(</span><span>expected</span><span>,</span> <span>result</span><span>);</span>
    <span>}</span>
```

### [](https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619#techniques-for-effective-unit-testing)Techniques for Effective Unit Testing

-   **Test-Driven Development (TDD)**  
    Think of designing a car where you first create a detailed blueprint before starting the assembly. In Test-Driven Development, we write the unit tests before developing the actual code. This approach ensures that our code is built according to the specifications set by the tests, leading to a more reliable and testable software.
    
-   **Mocking Dependencies**  
    Sometimes, a method we want to test depends on external resources, like needing a specific part that’s not yet available when building our car. In these situations, we can use `mocking` to create a simulated version of the required component. This allow us to test our car’s assembly process without relying on the actual, unavailable part.
    
    For example, if a method retrieves data from a database, we can "mock" the database to return a specific value without actually querying the real database.
    
    Libraries like [Moq](https://github.com/devlooped/moq) or [NSubstitute](https://nsubstitute.github.io/) are popular choices for mocking in .NET.
    
-   **Testing Both Happy and Sad Paths**  
    When testing our code it is important to check not only the ideal scenarios (happy paths) but also how our code behaves under less that ideal conditions (sad paths).
    
    -   Happy Path Testing: This is like making sure that all the pieces fit perfectly when you follow the instructions step by step. We always want to verify that our code works as expected when everything goes right. For example, if our method is supposed to divide two numbers we write a test to ensure it returns the correct result.
    -   Sad Path Testing: In our example, we create a test method that examines the `Div` method, assuming the inputs are always valid. However, what happens if the denominator is zero? Sad path testing ensures that our code does not crash in such scenarios and either manages the error gracefully or provides a meaningful error message.
    
    Here is an example on how we might test it:  
    
    ```
    <span>[</span><span>Fact</span><span>]</span>
    <span>public</span> <span>void</span> <span>Div_DivideByZero_ThrowsDivideByZeroException</span><span>()</span>
    <span>{</span>
        <span>// Arrange</span>
        <span>Calculator</span> <span>calculator</span> <span>=</span> <span>new</span> <span>Calculator</span><span>();</span>
    
        <span>// Act &amp; Assert</span>
        <span>Assert</span><span>.</span><span>Throws</span><span>&lt;</span><span>DivideByZeroException</span><span>&gt;(()</span> <span>=&gt;</span> <span>calculator</span><span>.</span><span>Div</span><span>(</span><span>10</span><span>,</span> <span>0</span><span>));</span>
    <span>}</span>
    ```
    
    Testing both paths ensures that our code is robust and can handle real world scenarios where users may not always follow the “happy path.”
    
-   **Writing Clean and Maintainable Tests**
    
    -   Keep Tests Simple: Each test should focus on one thing. Think of it as testing one car component at a time.
    -   Name Tests Clearly: Use descriptive names so that anyone reading your tests can easily understand what they do. For example, `Div_ReturnsCorrectResult` clearly indicates that the test checks whether the Div method returns the correct result.
    -   Avoid Duplicating Code: If multiple tests require the same setup, consider using helper methods or a test setup method to avoid duplicating code.

### [](https://dev.to/tkarropoulos/unit-testing-in-net-tools-and-techniques-nei?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=journey-through-the-net-world&_bhlid=6ebfb17e883b90100c5d621fd977e4d14e861619#conclusion)Conclusion

Unit testing in .NET is like building a LEGO structure piece by piece, ensuring each part fits perfectly before moving on to the next. By catching bugs early, simplifying maintenance, and boosting confidence in our code, unit testing becomes an invaluable tool in our development process.

Remember to test both the happy and sad paths. While happy path testing confirms that our application works under ideal conditions, sad path testing checks its resilience and ability to handle unexpected scenarios. Together, they ensure that our application is robust, secure, and ready for real-world use.

Start small, use the tools available in .NET like `xUnit` or `NUnit`, and incorporate best practices like TDD, mocking, and comprehensive path testing. With these techniques, you'll be well on your way to building robust, reliable applications that stand the test of time—just like a well-built LEGO masterpiece 😃

If you enjoyed my article and are interested feel free to share it in. If you want to read more about Unit testing you can visit this [link](https://learn.microsoft.com/en-us/dotnet/core/testing/).

Happy coding! 🚀💻
