---
created: 2024-04-15T12:19:54 (UTC -03:00)
tags: []
source: https://www.infoworld.com/article/3714840/avoid-using-enums-in-the-domain-layer-in-c-sharp.html?ref=dailydev
author: Joydip Kanjilal
---

# Avoid using enums in the domain layer in C# | InfoWorld

> ## Excerpt
> Understand the pitfalls of using enumeration types in the domain layer of your .NET applications and the advantages of using record types instead.

---
[![Joydip Kanjilal](https://images.idgesg.net/images/article/2017/06/joydipkanjilal-100724852-byline.jpg?auto=webp&quality=85,70)](https://www.infoworld.com/author/Joydip-Kanjilal/ "Joydip Kanjilal")

By , Contributor, InfoWorld | Mar 21, 2024 2:00 am PDT

### Understand the pitfalls of using enumeration types in the domain layer of your .NET applications and the advantages of using record types instead.

![wrong way road sign neonbrand cc0 via unsplash 1200x800](https://images.idgesg.net/images/idge/imported/imageapi/2022/06/14/10/wrong_way_road_sign_neonbrand_cc0_via_unsplash_1200x800-100753508-large-100929050-large.jpg?auto=webp&quality=85,70)

When working on applications, you will often need to represent a group of constants in the business logic and even in the domain layers. However, you should avoid using enumeration types, or enums, in the domain layer and instead use alternatives such as record types.

Why? In this article, we’ll explain the downsides of using enumerations in the domain layer and the advantages of using record types instead.

## Create a console application project in Visual Studio

First off, let’s create a .NET Core console application project in Visual Studio. Assuming Visual Studio 2022 is installed in your system, follow the steps outlined below to create a new .NET Core console application project.

1.  Launch the Visual Studio IDE.
2.  Click on “Create new project.”
3.  In the “Create new project” window, select “Console App (.NET Core)” from the list of templates displayed.
4.  Click Next.
5.  In the “Configure your new project” window, specify the name and location for the new project.
6.  Click Next.
7.  In the “Additional information” window shown next, choose “.NET 8.0 (Long Term Support)” as the framework version you would like to use.
8.  Click Create.

We’ll use this .NET 8 console application project to work with the code examples shown in the subsequent sections of this article.

## What’s wrong with enums?

While enumeration types can provide flexibility in maintaining a set of constant values in your application, they often introduce tight coupling between the domain model and the code that uses it, thus limiting your ability to evolve the domain model.

Consider the following enum used to define user and administrator roles.

```
<span>public</span><span> </span><span>enum</span><span> </span><span>Roles</span><span>
</span><span>{</span><span>
&nbsp;&nbsp;&nbsp; </span><span>User</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>Administrator</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>Reviewer</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>SuperAdmin</span><span>
</span><span>}</span>
```

You can also use an enum to define a group of constants as shown in the following code snippet.

```
<span>public</span><span> </span><span>enum</span><span> </span><span>Roles</span><span>
</span><span>{</span><span>
&nbsp;&nbsp;&nbsp; </span><span>User</span><span> </span><span>=</span><span> </span><span>1</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>Administrator</span><span> </span><span>=</span><span> </span><span>2</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>Reviewer</span><span> </span><span>=</span><span> </span><span>3</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>SuperAdmin</span><span> </span><span>=</span><span> </span><span>4</span><span>
</span><span>}</span>
```

## Problem 1: Encapsulation smells

Now, suppose you need to know whether a particular role relates to an administrator role. The following code snippet illustrates an extension method named IsAdmin that checks if a particular role is an Administrator or a SuperAdmin.

```
<span>public</span><span> </span><span>static</span><span> </span><span>class</span><span> </span><span>RolesExtensions</span><span>
</span><span>{</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>bool</span><span> </span><span>IsAdmin</span><span>(</span><span>this</span><span> </span><span>Roles</span><span> roles</span><span>)</span><span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span><span>=&gt;</span><span> roles </span><span>==</span><span> </span><span>Roles</span><span>.</span><span>Administrator</span><span> </span><span>||</span><span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; roles </span><span>==</span><span> </span><span>Roles</span><span>.</span><span>SuperAdmin</span><span>;</span><span>
</span><span>}</span>
```

This lands us on the first problem with using enums in the domain layer. Although you can operate on the enum using extension methods, your code breaks the encapsulation principle because the logic for querying the model and the model you created are separate. In other words, the logic that checks the model is not within the same class. This is an anti-pattern, and a model of this type is often known as an anemic model.

## Problem 2: Spaghetti code

Another problem with enums: You might often need to use explicit casts in your application’s code to retrieve a value from an enumeration. The following line of code illustrates this.

```
<span>int</span><span> role </span><span>=</span><span> </span><span>(</span><span>int</span><span>)</span><span>Roles</span><span>.</span><span>User</span><span>;</span>
```

Explicit casts are not a good approach. They are always costly in terms of performance, and they imply that you’ve used incompatible types in your application, or that the types have not been properly defined. Using enums in your domain layer could lead to using explicit casts throughout your application, cluttering up the code and making it harder to read and maintain.

## Problem 3: Naming constraints

Remember, you cannot include space characters in the names of enumeration constants in C#. Hence, the following code is not valid in C#.

```
<span>public</span><span> </span><span>enum</span><span> </span><span>Roles</span><span>
</span><span>{</span><span>
&nbsp;&nbsp;&nbsp; </span><span>Admin</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>Super</span><span> </span><span>Admin</span><span>
</span><span>}</span>
```

You can take advantage of attributes to overcome this limitation.

```
<span>using</span><span> </span><span>System</span><span>.</span><span>ComponentModel</span><span>.</span><span>DataAnnotations</span><span>;</span><span>
</span><span>public</span><span> </span><span>enum</span><span> </span><span>Roles</span><span>
</span><span>{</span><span>
&nbsp;&nbsp;&nbsp; </span><span>Admin</span><span>,</span><span>
&nbsp;&nbsp;&nbsp; </span><span>[</span><span>Display</span><span>(</span><span>Name</span><span> </span><span>=</span><span> </span><span>"Super Admin"</span><span>)]</span><span>
&nbsp;&nbsp;&nbsp; </span><span>SuperAdmin</span><span>
</span><span>}</span>
```

However, you will run into problems when your application needs to provide support for different locales.

## Use record types instead of enums

A better alternative is to use [record types](https://www.infoworld.com/article/3607372/how-to-work-with-record-types-in-csharp-9.html). You can take advantage of record types to create an immutable type as shown in the code snippet given below.

```
<span>public</span><span> record </span><span>Roles</span><span>(</span><span>int</span><span> </span><span>Id</span><span>)</span><span>
</span><span>{</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>User</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>1</span><span>);</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>Administrator</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>2</span><span>);</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>Reviewer</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>3</span><span>);</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>SuperAdmin</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>4</span><span>);</span><span>
</span><span>}</span>
```

An immutable object is an object that, once instantiated, cannot be altered. Hence records possess intrinsic thread-safety and immunity to race conditions. Immutable objects also make your code more readable and easier to maintain.

A significant benefit of using record types is preserving encapsulation because any extension method you write can be a part of the model itself. Remember, you cannot include any methods inside an enum. 

Further, records make it easy to provide meaningful names, as the following code illustrates.

```
<span>public</span><span> record </span><span>Roles</span><span>(</span><span>int</span><span> </span><span>Id</span><span>,</span><span> </span><span>string</span><span> </span><span>Name</span><span>)</span><span>
</span><span>{</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>User</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>1</span><span>,</span><span> </span><span>"User"</span><span>);</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>Administrator</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>2</span><span>,</span><span> </span><span>"Administrator"</span><span>);</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>Reviewer</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>3</span><span>,</span><span> </span><span>"Reviewer"</span><span>);</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>static</span><span> </span><span>Roles</span><span> </span><span>SuperAdmin</span><span> </span><span>{</span><span> </span><span>get</span><span>;</span><span> </span><span>}</span><span> </span><span>=</span><span> </span><span>new</span><span>(</span><span>4</span><span>,</span><span> </span><span>"Super Admin"</span><span>);</span><span>
&nbsp;&nbsp;&nbsp; </span><span>public</span><span> </span><span>override</span><span> </span><span>string</span><span> </span><span>ToString</span><span>()</span><span> </span><span>=&gt;</span><span> </span><span>Name</span><span>;</span><span>
</span><span>}</span>
```

You can now access the constants of the Roles record in much the same way you can access enumeration constants.

```
<span>Roles</span><span> admin </span><span>=</span><span> </span><span>Roles</span><span>.</span><span>Administrator</span><span>;</span><span>
</span><span>Roles</span><span> user </span><span>=</span><span> </span><span>Roles</span><span>.</span><span>User</span><span>;</span><span>
</span><span>Roles</span><span> reviewer </span><span>=</span><span> </span><span>Roles</span><span>.</span><span>Reviewer</span><span>;</span><span>
</span><span>Roles</span><span> superAdmin </span><span>=</span><span> </span><span>Roles</span><span>.</span><span>SuperAdmin</span><span>;</span>
```

When you invoke the ToString() method, the name of the constant will be displayed at the console window as shown in Figure 1.

[![record constants](https://images.idgesg.net/images/article/2024/03/record-constants-100962866-large.jpg?auto=webp&quality=85,70)](https://images.idgesg.net/images/article/2024/03/record-constants-100962866-orig.jpg?auto=webp&quality=85,70 "<div class='credit'>IDG</div>
<p>Figure 1: Displaying the name of the record constant at the console window.</p>
") <small>IDG</small>

Figure 1: Displaying the name of the record constant at the console window.

Alternatively, you could use a class instead of a record type and then define the constants you need. However, I would always prefer a record type for performance reasons. Record types are lightweight types due to which they are much faster than classes. A record is itself a reference type but it uses its own built-in equality check, which checks by value and not by reference.

Joydip Kanjilal is a Microsoft MVP in ASP.NET, as well as a speaker and author of several books and articles. He has more than 20 years of experience in IT including more than 16 years in Microsoft .NET and related technologies.

Copyright © 2024 IDG Communications, Inc.
