---
created: 2024-07-07T12:56:42 (UTC -03:00)
tags: []
source: https://code-maze.com/csharp-local-function-lambda-expressions/?ref=dailydev
author: Bozo Spoljaric
---

# Local Functions vs Lambda Expressions in C# - Code Maze

> ## Excerpt
> Discover the key differences in performance and use cases of local functions vs. lambda expressions in C#.

---
In this article, we’ll discuss the differences between local functions and lambda expressions in C#. 

To download the source code for this article, you can visit our [GitHub repository](https://github.com/CodeMazeBlog/CodeMazeGuides/tree/main/csharp-intermediate-topics/LocalFunctionsVsLambdaExpressionsInCSharp).

Let’s start.

Support Code Maze on Patreon to get rid of ads and get the best discounts on our products!

[![Become a patron at Patreon!](https://code-maze.com/wp-content/plugins/patron-plugin-pro/plugin/lib/patron-button-and-widgets-by-codebard/images/become_a_patron_button.png)](https://www.patreon.com/oauth2/become-patron?response_type=code&min_cents=100&client_id=9_akhcsDQMGo-FTlVmNpM_uxSV4fbW3vnrz7CBRV9RxwjMPCLfWgodhrcE0UuHH4&scope=identity%20identity[email]&redirect_uri=https://code-maze.com/patreon-authorization/&state=eyJmaW5hbF9yZWRpcmVjdF91cmkiOiJodHRwczpcL1wvY29kZS1tYXplLmNvbVwvaGF0ZW9hcy1hc3BuZXQtY29yZS13ZWItYXBpXC8ifQ%3D%3D&utm_source=https%3A%2F%2Fcode-maze.com%2Fhateoas-aspnet-core-web-api%2F&utm_medium=patreon_wordpress_plugin&utm_campaign=11086160&utm_term=&utm_content=post_unlock_button)

**Lambda expressions are anonymous functions. They are short blocks of code that accept parameters and return a value.** We can assign the lambda expression to a variable or pass it as a parameter. We define lambda expression using `=>` operator. On the left side are parameters, and on the right side is an expression body:

Here, our lambda expression accepts the integer variable `x` as an input parameter. In its body, it calculates the square of the input parameter value. We store the lambda expression in the `lambda` variable. Next, we output the result of the call to the lambda expression with argument value 3.  

If you’re new to Lambda Expression, and want to learn more about the lambda expressions, read our article [Lambda Expressions in C#](https://code-maze.com/lambda-expressions-in-csharp/).

## Local Functions

**Local functions are methods that are nested in other methods.** We can call local functions only from their containing method:

The `Square()` local function is nested in the `PrintSquare()` method. It accepts the integer variable `x` and returns the square number of the input parameter value.

Local functions can improve your code readability if used wisely, if you want to explore local functions further, read our article [Local Functions in C#](https://code-maze.com/csharp-local-functions/).

## Local Functions vs Lambda Expressions Performance Considerations

**When we can use either a local function or lambda expression, there is a good reason to decide on a local function, which is performance.**

Let’s create a benchmark test using a simple example of calculating the square of an integer: 

In the benchmark, we decorate `SquareAsLambda()` and `SquareAsLocal()` methods with the `[Benchmark]` attribute to enable running it. Both methods calculate the square of the input number, but the first implements lambda expression, and the second implements local function.

Now, let’s take a look at the benchmark results:

The local function outperforms the lambda expression for a significant time. 

**The compiler creates delegates for lambda expressions, which require object instantiation.** Local functions do not require delegate allocation unless explicitly captured by a delegate. Additionally, lambda expressions usually capture a local variable into a class. On the other hand, local functions can use a struct. Finally, the compiler inlines the local functions. 

Is this material useful to you? Consider subscribing and get **ASP.NET Core Web API Best Practices** eBook for [FREE!](https://code-maze.com/free-ebook-aspnetcore-webapi-best-practices/)

**Lambda expressions always require heap allocation.** Local functions can avoid heap allocation. For them, heap allocation is necessary only when we convert them to a delegate, or some variables the local functions capture are also captured by lambda expressions or local functions that are converted to a delegate.

**All these differences contribute to better performance of local functions than lambda expressions.**

## Recursive Functions

**Both local functions and lambda expressions can be recursive.** But local functions are naturally recursive, while lambda expressions require some workaround:

We implement the calculation of the factorial of the `input` integer variable. The `FactorialAsLocalFunction()` method calculates the factorial using the local function and the `FactorialAsLambdaExpression()` method uses a lambda expression.

The `FactorialAsLocalFunction()` method’s code using a local function is simple, as local functions can directly call themselves. The `FactorialAsLambdaExpression()` method with a lambda expression requires declaring the `factorial` delegate variable first and then assign the lambda expression to enable the lambda to reference itself. 

## Generic Local Functions

**Unlike lambda expressions, local functions can be generic**: 

Here, we implement the `Swap()` local function, which can exchange two objects in a generic way.

**This is not possible with lambda expression.** We can’t do something like:

Is this material useful to you? Consider subscribing and get **ASP.NET Core Web API Best Practices** eBook for [FREE!](https://code-maze.com/free-ebook-aspnetcore-webapi-best-practices/)

In this case, we end up with compiler error. 

## Iterator Local Functions

**We can implement local functions as iterators.** They can use `yield return` to produce values sequence or `yield break` to stop iteration: 

Here, we accept a list of integers and return an iterator that returns their absolute values. These values will be calculated only when the enumeration is materialized.

**Such an implementation is not possible with lambda expressions**, as they don’t allow a `yield return` statement.

## Other Differences Between Local Functions and Lambda Expressions

Local functions have named parameters that are visible on the caller side. We define lambda expressions as delegates and they don’t expose parameter names on the caller’s side. Besides that, local functions have names, while lambda expressions are anonymous, and we must assign them to a variable first.

We can define local functions anywhere inside the owning method. We can define them before or after we call them. Local functions can even be after the `return` statement. On the other hand, we must define lambda expressions before we use them. 

## Local Functions vs Lambda Expressions Features

Let’s summarize the differences between lambda expression and local functions:

| Feature | Local Functions | Lambda Expressions |
| --- | --- | --- |
| Performance | Faster | Slower |
| Recursivity | Yes, native | Yes, with workaround |
| Generic | Yes | No |
| Iterator | Yes | No |
| Named parameters | Yes | No |
| Order matters | No | Yes |

## Conclusion

There are some cases when we must use lambda expressions. These include passing the function as a delegate and using LINQ or a similar API that requires a delegate. Sometimes, we may prefer more concise syntax for inline functions. 

Is this material useful to you? Consider subscribing and get **ASP.NET Core Web API Best Practices** eBook for [FREE!](https://code-maze.com/free-ebook-aspnetcore-webapi-best-practices/)

Conversely, when we want to use methods as iterators or generic functions, we have to use local functions, as lambda expressions do not have these capabilities. 

**When we have a choice, local functions are better performance-wise and have more straightforward code.** Even when we can use either, such as in recursive functions, we consider local functions more readable and easier to understand. The same is true when considering parameters’ names or return types. 

Liked it? Take a second to support Code Maze on Patreon and get the ad free reading experience!

[![Become a patron at Patreon!](https://code-maze.com/wp-content/plugins/patron-plugin-pro/plugin/lib/patron-button-and-widgets-by-codebard/images/become_a_patron_button.png)](https://www.patreon.com/oauth2/become-patron?response_type=code&min_cents=100&client_id=9_akhcsDQMGo-FTlVmNpM_uxSV4fbW3vnrz7CBRV9RxwjMPCLfWgodhrcE0UuHH4&scope=identity%20identity[email]&redirect_uri=https://code-maze.com/patreon-authorization/&state=eyJmaW5hbF9yZWRpcmVjdF91cmkiOiJodHRwczpcL1wvY29kZS1tYXplLmNvbVwvY3NoYXJwLWxvY2FsLWZ1bmN0aW9uLWxhbWJkYS1leHByZXNzaW9uc1wvIn0%3D&utm_source=https%3A%2F%2Fcode-maze.com%2Fcsharp-local-function-lambda-expressions%2F&utm_medium=patreon_wordpress_plugin&utm_campaign=11086160&utm_term=&utm_content=post_unlock_button)
