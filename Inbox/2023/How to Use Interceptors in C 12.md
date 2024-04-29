---
created: 2024-02-26T17:37:13 (UTC -03:00)
tags: []
source: https://code-maze.com/how-to-use-interceptors-in-c-12/
author: Code Maze
---

# How to Use Interceptors in C# 12

> ## Excerpt
> In this article, we will demonstrate how to introduce interceptors into our code and explore the use cases for this feature.

---
In C# 12, **interceptors are a new experimental compiler feature** **that enables rerouting specific method calls to different code**.

Typically, developers use interceptors with code generators to modify the behavior of existing code without altering the code itself. In this article, we will demonstrate how to introduce interceptors into our code and explore the use cases for this feature.

To download the source code for this article, you can visit our [GitHub repository](https://github.com/CodeMazeBlog/CodeMazeGuides/tree/main/csharp-advanced-topics/InterceptorsInCSharp).

Let’s get started.

## Introducing Interceptors Into Our Code

To introduce interceptors into our application, we need to:

-   Enable the interceptors feature
-   Declare the `InterceptsLocationAttribute` class
-   Create a static class that will include the interceptor method
-   Add the `InterceptsLocationAttribute` attribute to the interceptor method
-   Introduce a prebuild command to our `.csproj` file

### **Enabling the interceptors feature**

Let’s start by creating a simple console application. To enable the interceptors feature, we have to explicitly add the feature flag in our `.csproj` file by adding `<LangVersion>preview</LangVersion>` and `<Features>InterceptorsPreview</Features>` under the `PropertyGroup` tag:

Support Code Maze on Patreon to get rid of ads and get the best discounts on our products!

[![Become a patron at Patreon!](https://code-maze.com/wp-content/plugins/patron-plugin-pro/plugin/lib/patron-button-and-widgets-by-codebard/images/become_a_patron_button.png)](https://www.patreon.com/oauth2/become-patron?response_type=code&min_cents=100&client_id=9_akhcsDQMGo-FTlVmNpM_uxSV4fbW3vnrz7CBRV9RxwjMPCLfWgodhrcE0UuHH4&scope=identity%20identity[email]&redirect_uri=https://code-maze.com/patreon-authorization/&state=eyJmaW5hbF9yZWRpcmVjdF91cmkiOiJodHRwczpcL1wvY29kZS1tYXplLmNvbVwvaGF0ZW9hcy1hc3BuZXQtY29yZS13ZWItYXBpXC8ifQ%3D%3D&utm_source=https%3A%2F%2Fcode-maze.com%2Fhateoas-aspnet-core-web-api%2F&utm_medium=patreon_wordpress_plugin&utm_campaign=11086160&utm_term=&utm_content=post_unlock_button)

In our console application, let’s first create an `Example` class. This class has a `GetText()` method that accepts a string and returns a string. The `GetText()` method will represent **the method that we want to intercept** in our application so that it returns a different string:

Then, in our `Program` class, let’s create an instance of the `Example` class, call the `GetText()` method and finally print the result. Once we run our application, the console will print “Hello, World!” as expected:

### Declaring the InterceptsLocationAttribute in the Project

The `InterceptsLocationAttribute` class is a C# 12 attribute designed to **label methods as interceptors**. However, as of the publication of this article, interceptors are still in preview, and the attribute hasn’t yet been included in .NET 8. Consequently, we need to declare it manually within our project under the `System.Runtime.CompilerService` namespace:

The `InterceptsLocationAttribute` class is an attribute that targets methods exclusively and accepts three arguments, which are the file path, line number, and the starting character column number of the method we want to intercept.

Also, we did not assign the file path, line number, and the starting character column number anywhere since the attribute serves as a marker that the compiler reads during compilation, making it irrelevant to set values for runtime use.

### Creating the Interceptor Method

Next, let’s create a static `GeneratedCode` class that will include the interceptor method to which our call will be rerouted. The interceptor method **should be declared as an extension method** because it will replace the original method in the `Example` class:

Note that the interceptor method **must have the exact same signature** as the intercepted method; otherwise, when we run the application, it will throw a runtime exception.

### Adding the InterceptsLocation attribute to the Interceptor Method

The next step is to **import the** `InterceptsLocationAttribute` **class** from the `System.Runtime.CompilerServices` namespace and **add the attribute to the interceptor method**. Here we provide the absolute path to the `Program.cs` file, the line number, and the starting character column number of our `GetText()` call:

The string `'absolute/path/to/program.cs'` **will throw a compilation error** since it is not the correct absolute path. We will discuss how to solve this issue in the next section.

Note that to get the line number and the starting character column number, we can use Visual Studio by placing the cursor before the starting character of the `GetText()` method and checking the bottom right corner of the editor window (check the image below for reference):

Wanna join Code Maze Team, help us produce more awesome .NET/C# content and **get paid?** [>> JOIN US! <<](https://code-maze.com/write-for-codemaze/?source=content)

[![interceptors method line and character](https://code-maze.com/wp-content/uploads/2023/11/method-line-and-character-1.png)](https://code-maze.com/wp-content/uploads/2023/11/method-line-and-character-1.png)

### **Introducing the Prebuild Command to Our .csproj File**

At this point, if we try to build our project, the build will fail, and the compiler will throw this compilation error:

CS9139 Cannot intercept: compilation does not contain a file with path 'absolute/path/to/program.cs'

That is because the compiler does not recognize the string `'absolute/path/to/program.cs'` as a valid path to the `program.cs` file.

One way to solve this issue is to hardcode the absolute path in the interceptor attribute. However, this might cause an issue when compiling the application on another machine since the absolute path to the `program.cs` file would be different.

A better solution would be to **add a** [prebuild event](https://learn.microsoft.com/en-us/visualstudio/ide/how-to-specify-build-events-csharp?view=vs-2022) **to our project, which replaces the invalid path with the correct one** before compiling the application. This event will run a PowerShell script that takes care of the string replacement.

First, we have to make sure that we install the cross-platform version of PowerShell on our machine. PowerShell supports all major platforms such as [Windows](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.3), [Linux](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-linux?view=powershell-7.3), and [MacOS](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-macos?view=powershell-7.3).

Second, we need to add a PowerShell script `pre-build-event.ps1` to our project folder:

Wanna join Code Maze Team, help us produce more awesome .NET/C# content and **get paid?** [>> JOIN US! <<](https://code-maze.com/write-for-codemaze/?source=content)

The purpose of this script is to replace all the `'absolute/path/to/program.cs'` strings by the actual `Program.cs` file path in the `GeneratedCode.cs` file.

Then, we can add the prebuild command to our `.csproj` file:

Once we build our code, we will see that the string `'absolute/path/to/program.cs'` is replaced by the actual absolute path of the `program.cs` file.

Now, if we run the application, the compiler will reroute the call to our interceptor method, and it will print “Hello, Code Maze!” instead of “Hello, World!”.

### Allow Intercepting Multiple Calls

We can even **add multiple** `InterceptsLocation` **attributes to the interceptor method** to intercept multiple calls. We need to **set the** `AllowMultiple` **attribute of the** `InterceptsLocationAttribute` **class to** `true`:

Let’s add another call to the `GetText()` method in the `Example` class and print the result. At this point, if we run the application, the second call will return “Greetings, World!” as expected:

Next, let’s **add another** `InterceptsLocation` **attribute** to the `InterceptorMethod()` extension method with the correct file path, line, and character of the second call. Note that the prebuild event will replace all `'absolute/path/to/program.cs'` strings in the file:

If we run the application, the output will appear as follows:

Wanna join Code Maze Team, help us produce more awesome .NET/C# content and **get paid?** [>> JOIN US! <<](https://code-maze.com/write-for-codemaze/?source=content)

Interceptors are a powerful tool that we can use to automate tasks. This can help to improve the quality, maintainability, and performance of our code.

We can use the interceptors feature to **automatically log method calls and their parameters**. This helps in tracking down bugs or auditing user activity. Another use for interceptors is to **cache the results of method calls**, which improves the overall performance of our application. We can also use interceptors to **collect metrics about method calls** such as how many times we call the method, the call duration, and the exceptions that occur during execution. Another use case of interceptors is to **mock our method calls**, which can be useful in unit and integration testing.

## Conclusion

With the release of C# 12, we can see that Microsoft is clearly trying to boost the ahead-of-time (AOT) compilation in C# applications. And with features like interceptors, overriding legacy code will become easier for developers.

However, interceptors are an experimental feature that might be changed or removed in later stages and thus **should not be used for production**. You can click [here](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-12) to learn more about C# 12’s new features.

Liked it? Take a second to support Code Maze on Patreon and get the ad free reading experience!

[![Become a patron at Patreon!](https://code-maze.com/wp-content/plugins/patron-plugin-pro/plugin/lib/patron-button-and-widgets-by-codebard/images/become_a_patron_button.png)](https://www.patreon.com/oauth2/become-patron?response_type=code&min_cents=100&client_id=9_akhcsDQMGo-FTlVmNpM_uxSV4fbW3vnrz7CBRV9RxwjMPCLfWgodhrcE0UuHH4&scope=identity%20identity[email]&redirect_uri=https://code-maze.com/patreon-authorization/&state=eyJmaW5hbF9yZWRpcmVjdF91cmkiOiJodHRwczpcL1wvY29kZS1tYXplLmNvbVwvaG93LXRvLXVzZS1pbnRlcmNlcHRvcnMtaW4tYy0xMlwvIn0%3D&utm_source=https%3A%2F%2Fcode-maze.com%2Fhow-to-use-interceptors-in-c-12%2F&utm_medium=patreon_wordpress_plugin&utm_campaign=11086160&utm_term=&utm_content=post_unlock_button)
