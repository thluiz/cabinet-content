---
created: 2023-12-04T10:15:03 (UTC -03:00)
tags: []
source: https://www.c-sharpcorner.com/blogs/exploring-c-sharp-12-features?utm_source=Camp&utm_campaign=Newletter&utm_medium=Email&utm_content=
author: Prasad Raveendran
---

# Exploring C# 12 Features

> ## Excerpt
> As technology evolves, programming languages continue to advance, offering developers new tools and features to enhance their productivity and simplify complex tasks. In the world of .NET development, C# remains a prominent language, constantly evolving to meet the needs of modern software development. C# 12, the latest iteration of this language, introduces several compelling features that streamline code, improve readability, and empower developers. In this blog post, we'll delve into five thrilling enhancements in C# 12 that particularly captivate my interest.

---
## Introduction

As technology evolves, programming languages continue to advance, offering developers new tools and features to enhance their productivity and simplify complex tasks. In the world of .NET development, C# remains a prominent language, constantly evolving to meet the needs of modern software development. C# 12, the latest iteration of this language, introduces several compelling features that streamline code, improve readability, and empower developers. In this blog post, we'll delve into five thrilling enhancements in C# 12 that particularly captivate my interest.

Prerequires need to be set up for reviewing C# 12 features.

1.  [VS 2022  version 17.7](https://visualstudio.microsoft.com/vs/)  Or
2.  [.NET 8.0 SDK](https://dotnet.microsoft.com/en-us/download)

The source code can be downloaded from [GitHub](https://github.com/Prasadnair/CSharp12Features).

##  1. Primary Constructors in C# 12

This feature simplifies the initialization of properties in a class by allowing you to declare them directly within the constructor parameters, reducing boilerplate code. Here's a quick example.

This concise syntax eliminates the need for separate property declarations and assignments within the constructor body, making the code more readable and maintainable.

Please refer to [Microsoft Learning](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/tutorials/primary-constructors) for more information on Primary Constructor.

## 2\. Collection Expression in C# 12

C# 12 introduces collection expressions, enabling the creation and initialization of collections directly within object initializers and collection initializers. It allows for a more compact and readable syntax when working with collections. For instance.

This enhancement simplifies the initialization of arrays, lists, dictionaries, and other collections, reducing verbosity and improving code clarity.

Please refer the [Microsoft Learning](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-12.0/collection-expressions) for more details on Collection Expression.

## 3\. Ref Readonly Parameters in C# 12

Ref readonly parameters provide a way to pass parameters by reference while ensuring they cannot be modified within the method. This feature enhances performance by avoiding unnecessary copying of large structs while maintaining immutability. Here's an example.

Ref readonly parameters offer a balance between performance and immutability, improving code safety and efficiency.

Please refer [Microsoft Learning](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-12.0/ref-readonly-parameters) for more information on Ref Readonly parameters.

## 4\. Default Lambda Parameters in C# 12

C# 12 introduces default values for lambda parameters, allowing developers to define default arguments within lambda expressions. This feature enhances flexibility when working with lambdas by providing default values for parameters. For example.

This enables functions to be invoked with fewer arguments when default values are provided, reducing code redundancy.

Please refer [Microsoft Learning](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-12.0/lambda-method-group-defaults) for more information.

## 5\. Alias Any Type in C# 12

The latest update in C# 12 has eased the use of alias directives, enabling the aliasing of various types beyond just named types. This expansion allows for the aliasing of tuples, pointers, array types, generic types, and more. Consequently, instead of employing the complete structural representation of a tuple, you can now assign it a concise, descriptive name and utilize it across your codebase. For example

Please refer to [Microsoft Learning](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-12.0/using-alias-types) for more information

There are a few more features, such as [Inline Arrays](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-12.0/inline-arrays), [Experimental Attribute](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/proposals/csharp-12.0/experimental-attribute), and [Interceptors](https://learn.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-12#interceptors) ( which are still in the preview stage).
