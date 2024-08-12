---
created: 2024-07-29T18:52:58 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/
author: Mark Seemann
---

# Using Ports and Adapters to persist restaurant table configurations

> ## Excerpt
> A data architecture example in C# and ASP.NET.

---
_A data architecture example in C# and ASP.NET._

This is part of a [small article series on data architectures](https://blog.ploeh.dk/2024/07/25/three-data-architectures-for-the-server). In the first instalment, you'll see the outline of a Ports and Adapters implementation. As the introductory article explains, the example code shows how to create a new restaurant table configuration, or how to display an existing resource. The sample code base is an ASP.NET 8.0 [REST](https://en.wikipedia.org/wiki/REST) API.

Keep in mind that while the sample code does store data in a relational database, the term _table_ in this article mainly refers to physical tables, rather than database tables.

While Ports and Adapters architecture diagrams are usually drawn as concentric circles, you can also draw (subsets of) it as more traditional layered diagrams:

![Three-layer architecture diagram showing TableDto, Table, and TableEntity as three vertically stacked boxes, with arrows between them.](https://blog.ploeh.dk/content/binary/three-layer-table-architecture.png)

Here, the arrows indicate mappings, not dependencies.

### HTTP interaction [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#5e95ae96906c43f9aee53cfbd680a378)

A client can create a new table with a `POST` HTTP request:

```
POST /tables HTTP/1.1
content-type: application/json

{&nbsp;<span>"communalTable"</span>:&nbsp;{&nbsp;<span>"capacity"</span>:&nbsp;16&nbsp;}&nbsp;}
```

Which might elicit a response like this:

```
HTTP/1.1 201 Created
Location: https://example.com/Tables/844581613e164813aa17243ff8b847af
```

Clients can later use the address indicated by the `Location` header to retrieve a representation of the resource:

```
GET /Tables/844581613e164813aa17243ff8b847af HTTP/1.1
accept: application/json
```

Which would result in this response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{<span>"communalTable"</span>:{<span>"capacity"</span>:16}}
```

By default, ASP.NET handles and returns JSON. Later in this article you'll see how well it deals with other data formats.

### Boundary [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#95914d7bd8c845d2b30d8ddc029c1711)

ASP.NET supports some variation of the [model-view-controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) (MVC) pattern, and Controllers handle HTTP requests. At the outset, the _action method_ that handles the `POST` request looks like this:

```
[<span>HttpPost</span>]
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&lt;<span>IActionResult</span>&gt;&nbsp;<span>Post</span>(<span>TableDto</span>&nbsp;<span>dto</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>ArgumentNullException</span>.<span>ThrowIfNull</span>(<span>dto</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>id</span>&nbsp;=&nbsp;<span>Guid</span>.<span>NewGuid</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>await</span>&nbsp;<span>repository</span>.<span>Create</span>(<span>id</span>,&nbsp;<span>dto</span>.<span>ToTable</span>()).<span>ConfigureAwait</span>(<span>false</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>CreatedAtActionResult</span>(<span>nameof</span>(<span>Get</span>),&nbsp;<span>null</span>,&nbsp;<span>new</span>&nbsp;{&nbsp;id&nbsp;=&nbsp;<span>id</span>.<span>ToString</span>(<span>"N"</span>)&nbsp;},&nbsp;<span>null</span>);
}
```

As is [idiomatic](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic) in ASP.NET, input and output are modelled by [data transfer objects](https://en.wikipedia.org/wiki/Data_transfer_object) (DTOs), in this case called `TableDto`. I've already covered this little object model in the article [Serializing restaurant tables in C#](https://blog.ploeh.dk/2023/12/25/serializing-restaurant-tables-in-c), so I'm not going to repeat it here.

The `ToTable` method, on the other hand, is a good example of how trying to cut corners lead to more complicated code:

```
<span>internal</span>&nbsp;<span>Table</span>&nbsp;<span>ToTable</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>candidate</span>&nbsp;=
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>Table</span>.<span>TryCreateSingle</span>(SingleTable?.Capacity&nbsp;??&nbsp;-1,&nbsp;SingleTable?.MinimalReservation&nbsp;??&nbsp;-1);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>candidate</span>&nbsp;<span>is</span>&nbsp;{&nbsp;})
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>candidate</span>.Value;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>candidate</span>&nbsp;=&nbsp;<span>Table</span>.<span>TryCreateCommunal</span>(CommunalTable?.Capacity&nbsp;??&nbsp;-1);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>candidate</span>&nbsp;<span>is</span>&nbsp;{&nbsp;})
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>candidate</span>.Value;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>throw</span>&nbsp;<span>new</span>&nbsp;<span>InvalidOperationException</span>(<span>"Invalid&nbsp;TableDto."</span>);
}
```

Compare it to the `TryParse` method in the [Serializing restaurant tables in C#](https://blog.ploeh.dk/2023/12/25/serializing-restaurant-tables-in-c) article. That one is simpler, and less error-prone.

I think that I wrote `ToTable` that way because I didn't want to deal with error handling in the Controller, and while I test-drove the code, I never wrote a test that supply malformed input. I should have, and so should you, but this is demo code, and I never got around to it.

Enough about that. The other action method handles `GET` requests:

```
[<span>HttpGet</span>(<span>"</span><span>{</span>id<span>}</span><span>"</span>)]
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&lt;<span>IActionResult</span>&gt;&nbsp;<span>Get</span>(<span>string</span>&nbsp;<span>id</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(!<span>Guid</span>.<span>TryParseExact</span>(<span>id</span>,&nbsp;<span>"N"</span>,&nbsp;<span>out</span>&nbsp;<span>var</span>&nbsp;<span>guid</span>))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>BadRequestResult</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>table</span>&nbsp;=&nbsp;<span>await</span>&nbsp;<span>repository</span>.<span>Read</span>(<span>guid</span>).<span>ConfigureAwait</span>(<span>false</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>table</span>&nbsp;<span>is</span>&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>NotFoundResult</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>OkObjectResult</span>(<span>TableDto</span>.<span>From</span>(<span>table</span>.Value));
}
```

The static `TableDto.From` method is identical to the `ToDto` method from the [Serializing restaurant tables in C#](https://blog.ploeh.dk/2023/12/25/serializing-restaurant-tables-in-c) article, just with a different name.

To summarize so far: At the boundary of the application, Controller methods receive or return `TableDto` objects, which are mapped to and from the Domain Model named `Table`.

### Domain Model [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#b5cbc5042c01488da08a856d84d3f17e)

The Domain Model `Table` is also identical to the code shown in [Serializing restaurant tables in C#](https://blog.ploeh.dk/2023/12/25/serializing-restaurant-tables-in-c). In order to comply with the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) (DIP), mapping to and from `TableDto` is defined on the latter. The DTO, being an implementation detail, may depend on the abstraction (the Domain Model), but not the other way around.

In the same spirit, conversions to and from the database are defined entirely within the `repository` implementation.

### Data access layer [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#84b01fa88e8541f5afbb9aa04f454645)

Keeping the example consistent, the code base also models data access with C# classes. It uses [Entity Framework](https://learn.microsoft.com/ef) to read from and write to [SQL Server](https://en.wikipedia.org/wiki/Microsoft_SQL_Server). The class that models a row in the database is also a kind of DTO, even though here it's idiomatically called an _entity:_

```
<span>public</span>&nbsp;<span>partial</span>&nbsp;<span>class</span>&nbsp;<span>TableEntity</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;Id&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>Guid</span>&nbsp;PublicId&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;Capacity&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>?&nbsp;MinimalReservation&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
}
```

I had a command-line tool scaffold the code for me, and since [I don't usually live in that world](https://blog.ploeh.dk/2023/09/18/do-orms-reduce-the-need-for-mapping), I don't know why it's a `partial class`. It seems to be working, though.

The `SqlTablesRepository` class implements the mapping between `Table` and `TableEntity`. For instance, the `Create` method looks like this:

```
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&nbsp;<span>Create</span>(<span>Guid</span>&nbsp;<span>id</span>,&nbsp;<span>Table</span>&nbsp;<span>table</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>entity</span>&nbsp;=&nbsp;<span>table</span>.<span>Accept</span>(<span>new</span>&nbsp;<span>TableToEntityConverter</span>(<span>id</span>));
&nbsp;&nbsp;&nbsp;&nbsp;<span>await</span>&nbsp;context.Tables.<span>AddAsync</span>(<span>entity</span>).<span>ConfigureAwait</span>(<span>false</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>await</span>&nbsp;context.<span>SaveChangesAsync</span>().<span>ConfigureAwait</span>(<span>false</span>);
}
```

That looks simple, but is only because all the work is done by the `TableToEntityConverter`, which is a nested class:

```
<span>private</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>TableToEntityConverter</span>&nbsp;:&nbsp;<span>ITableVisitor</span>&lt;<span>TableEntity</span>&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>Guid</span>&nbsp;id;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>TableToEntityConverter</span>(<span>Guid</span>&nbsp;<span>id</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.id&nbsp;=&nbsp;<span>id</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>TableEntity</span>&nbsp;<span>VisitCommunal</span>(<span>NaturalNumber</span>&nbsp;<span>capacity</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>TableEntity</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PublicId&nbsp;=&nbsp;id,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Capacity&nbsp;=&nbsp;(<span>int</span>)<span>capacity</span>,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>TableEntity</span>&nbsp;<span>VisitSingle</span>(
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>NaturalNumber</span>&nbsp;<span>capacity</span>,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>NaturalNumber</span>&nbsp;<span>minimalReservation</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>TableEntity</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PublicId&nbsp;=&nbsp;id,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Capacity&nbsp;=&nbsp;(<span>int</span>)<span>capacity</span>,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MinimalReservation&nbsp;=&nbsp;(<span>int</span>)<span>minimalReservation</span>,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

Mapping the other way is easier, so the `SqlTablesRepository` does it inline in the `Read` method:

```
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&lt;<span>Table</span>?&gt;&nbsp;<span>Read</span>(<span>Guid</span>&nbsp;<span>id</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>entity</span>&nbsp;=&nbsp;<span>await</span>&nbsp;context.Tables
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.<span>SingleOrDefaultAsync</span>(<span>t</span>&nbsp;=&gt;&nbsp;<span>t</span>.PublicId&nbsp;<span>==</span>&nbsp;<span>id</span>).<span>ConfigureAwait</span>(<span>false</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>entity</span>&nbsp;<span>is</span>&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>null</span>;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>entity</span>.MinimalReservation&nbsp;<span>is</span>&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>Table</span>.<span>TryCreateCommunal</span>(<span>entity</span>.Capacity);
&nbsp;&nbsp;&nbsp;&nbsp;<span>else</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>Table</span>.<span>TryCreateSingle</span>(
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>entity</span>.Capacity,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>entity</span>.MinimalReservation.Value);
}
```

Similar to the case of the DTO, mapping between `Table` and `TableEntity` is the responsibility of the `SqlTablesRepository` class, since data persistence is an implementation detail. According to the DIP it shouldn't be part of the Domain Model, and it isn't.

### XML formats [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#77dda87b99ac49bf91d08079e252fc50)

That covers the basics, but how well does this kind of architecture stand up to changing requirements?

One axis of variation is when a service needs to support multiple representations. In this example, I'll imagine that the service also needs to support not just one, but two, XML formats.

Granted, you may not run into that particular requirement that often, but it's typical of a kind of change that you're likely to run into. In REST APIs, for example, [you should use content negotiation for versioning](https://blog.ploeh.dk/2015/06/22/rest-implies-content-negotiation), and that's the same kind of problem.

To be fair, application code also changes for a variety of other reasons, including new features, changes to business logic, etc. I can't possibly cover all, though, and many of these are much better described than changes in wire formats.

As described in the [introduction article](https://blog.ploeh.dk/2024/07/25/three-data-architectures-for-the-server), ideally the XML should support a format implied by these examples:

```
<span>&lt;</span><span>communal-table</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>capacity</span><span>&gt;</span>12<span>&lt;/</span><span>capacity</span><span>&gt;</span>
<span>&lt;/</span><span>communal-table</span><span>&gt;</span>

<span>&lt;</span><span>single-table</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>capacity</span><span>&gt;</span>4<span>&lt;/</span><span>capacity</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>minimal-reservation</span><span>&gt;</span>3<span>&lt;/</span><span>minimal-reservation</span><span>&gt;</span>
<span>&lt;/</span><span>single-table</span><span>&gt;</span>
```

Notice that while these two examples have different root elements, they're still considered to both represent _a table_. Although [at the boundaries, static types are illusory](https://blog.ploeh.dk/2023/10/16/at-the-boundaries-static-types-are-illusory) we may still, loosely speaking, consider both of those XML documents as belonging to the same 'type'.

To be honest, if there's a way to support this kind of [schema](https://blog.ploeh.dk/2024/04/15/services-share-schema-and-contract-not-class) by defining DTOs to be serialized and deserialized, I don't know what it looks like. That's not meant to imply that it's impossible. There's often an epistemological problem associated with proving things impossible, so I'll just leave it there.

To be clear, it's not that I don't know how to support that kind of schema at all. I do, as the article _Using only a Domain Model to persist restaurant table configurations_ will show. I just don't know how to do it with DTO-based serialisation.

### Element-biased XML [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#6b0409d4-67fd-4e86-9a83-538fa5b690d4)

Instead of the above XML schema, I will, instead explore how hard it is to support a variant schema, implied by these two examples:

```
<span>&lt;</span><span>table</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>type</span><span>&gt;</span>communal<span>&lt;/</span><span>type</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>capacity</span><span>&gt;</span>12<span>&lt;/</span><span>capacity</span><span>&gt;</span>
<span>&lt;/</span><span>table</span><span>&gt;</span>

<span>&lt;</span><span>table</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>type</span><span>&gt;</span>single<span>&lt;/</span><span>type</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>capacity</span><span>&gt;</span>4<span>&lt;/</span><span>capacity</span><span>&gt;</span>
<span>&nbsp;&nbsp;&lt;</span><span>minimal-reservation</span><span>&gt;</span>3<span>&lt;/</span><span>minimal-reservation</span><span>&gt;</span>
<span>&lt;/</span><span>table</span><span>&gt;</span>
```

This variation shares the same `<table>` root element and instead distinguishes between the two kinds of table with a `<type>` discriminator.

This kind of schema we can define with a DTO:

```
[<span>XmlRoot</span>(<span>"table"</span>)]
<span>public</span>&nbsp;<span>class</span>&nbsp;<span>ElementBiasedTableXmlDto</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;[<span>XmlElement</span>(<span>"type"</span>)]
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>string</span>?&nbsp;Type&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;[<span>XmlElement</span>(<span>"capacity"</span>)]
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;Capacity&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;[<span>XmlElement</span>(<span>"minimal-reservation"</span>)]
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>?&nbsp;MinimalReservation&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>bool</span>&nbsp;<span>ShouldSerializeMinimalReservation</span>()&nbsp;=&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MinimalReservation.HasValue;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;Mapping&nbsp;methods&nbsp;not&nbsp;shown&nbsp;here...</span>
}
```

As you may have already noticed, however, this isn't the same type as `TableJsonDto`, so how are we going to implement the Controller methods that receive and send objects of this type?

### Posting XML [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#28e522bca7b04f94a68c00f15f0d7243)

The service should still accept JSON as shown above, but now, additionally, it should also support HTTP requests like this one:

```
POST /tables HTTP/1.1
content-type: application/xml

<span>&lt;</span><span>table</span><span>&gt;&lt;</span><span>type</span><span>&gt;</span>communal<span>&lt;/</span><span>type</span><span>&gt;&lt;</span><span>capacity</span><span>&gt;</span>12<span>&lt;/</span><span>capacity</span><span>&gt;&lt;/</span><span>table</span><span>&gt;</span>
```

How do you implement this new feature?

My first thought was to add a `Post` overload to the Controller:

```
[<span>HttpPost</span>]
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&lt;<span>IActionResult</span>&gt;&nbsp;<span>Post</span>(<span>ElementBiasedTableXmlDto</span>&nbsp;<span>dto</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>ArgumentNullException</span>.<span>ThrowIfNull</span>(<span>dto</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>id</span>&nbsp;=&nbsp;<span>Guid</span>.<span>NewGuid</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>await</span>&nbsp;<span>repository</span>.<span>Create</span>(<span>id</span>,&nbsp;<span>dto</span>.<span>ToTable</span>()).<span>ConfigureAwait</span>(<span>false</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>CreatedAtActionResult</span>(
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>nameof</span>(<span>Get</span>),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>null</span>,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>new</span>&nbsp;{&nbsp;id&nbsp;=&nbsp;<span>id</span>.<span>ToString</span>(<span>"N"</span>)&nbsp;},
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>null</span>);
}
```

I just copied and pasted the original `Post` method and changed the type of the `dto` parameter. I also had to add a `ToTable` conversion to `ElementBiasedTableXmlDto`:

```
<span>internal</span>&nbsp;<span>Table</span>&nbsp;<span>ToTable</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(Type&nbsp;==&nbsp;<span>"single"</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>t</span>&nbsp;=&nbsp;<span>Table</span>.<span>TryCreateSingle</span>(Capacity,&nbsp;MinimalReservation&nbsp;??&nbsp;0);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>t</span>&nbsp;<span>is</span>&nbsp;{&nbsp;})
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>t</span>.Value;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(Type&nbsp;==&nbsp;<span>"communal"</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>t</span>&nbsp;=&nbsp;<span>Table</span>.<span>TryCreateCommunal</span>(Capacity);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>t</span>&nbsp;<span>is</span>&nbsp;{&nbsp;})
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>t</span>.Value;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>throw</span>&nbsp;<span>new</span>&nbsp;<span>InvalidOperationException</span>(<span>"Invalid&nbsp;Table&nbsp;DTO."</span>);
}
```

While all of that compiles, it doesn't work.

When you attempt to `POST` a request against the service, the ASP.NET framework now throws an `AmbiguousMatchException` indicating that "The request matched multiple endpoints". Which is understandable.

This lead me to the first round of [Framework Whac-A-Mole](https://blog.ploeh.dk/2023/10/02/dependency-whac-a-mole). What I'd like to do is to select the appropriate action method based on `content-type` or `accept` headers. How does one do that?

After some web searching, I came across [a Stack Overflow answer](https://stackoverflow.com/a/1045616/126014) that seemed to indicate a way forward.

### Selecting the right action method [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#5f262b2b42c542148b62223f0fb3d9a9)

One way to address the issue is to implement a custom [ActionMethodSelectorAttribute](https://learn.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.actionconstraints.actionmethodselectorattribute):

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>SelectTableActionMethodAttribute</span>&nbsp;:&nbsp;<span>ActionMethodSelectorAttribute</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>override</span>&nbsp;<span>bool</span>&nbsp;<span>IsValidForRequest</span>(<span>RouteContext</span>&nbsp;<span>routeContext</span>,&nbsp;<span>ActionDescriptor</span>&nbsp;<span>action</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>action</span>&nbsp;<span>is</span>&nbsp;<span>not</span>&nbsp;<span>ControllerActionDescriptor</span>&nbsp;<span>cad</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>false</span>;
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>cad</span>.Parameters.Count&nbsp;!=&nbsp;1)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>false</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>dtoType</span>&nbsp;=&nbsp;<span>cad</span>.Parameters[0].ParameterType;
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;Demo&nbsp;code&nbsp;only.&nbsp;This&nbsp;doesn't&nbsp;take&nbsp;into&nbsp;account&nbsp;a&nbsp;possible&nbsp;charset</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;parameter.&nbsp;See&nbsp;RFC&nbsp;9110,&nbsp;section&nbsp;8.3</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;(https://www.rfc-editor.org/rfc/rfc9110#field.content-type)&nbsp;for&nbsp;more</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;information.</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>routeContext</span>?.HttpContext.Request.ContentType&nbsp;==&nbsp;<span>"application/json"</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>dtoType</span>&nbsp;<span>==</span>&nbsp;<span>typeof</span>(<span>TableJsonDto</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>routeContext</span>?.HttpContext.Request.ContentType&nbsp;==&nbsp;<span>"application/xml"</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>dtoType</span>&nbsp;<span>==</span>&nbsp;<span>typeof</span>(<span>ElementBiasedTableXmlDto</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>false</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

As the code comment suggests, this isn't as robust as it should be. A `content-type` header may also look like this:

```
Content-Type: application/json; charset=utf-8
```

The exact string equality check shown above would fail in such a scenario, suggesting that a more sophisticated implementation is warranted. I'll skip that for now, since this demo code already compromises on the overall XML schema. For an example of more robust [content negotiation](https://en.wikipedia.org/wiki/Content_negotiation) implementations, see _Using only a Domain Model to persist restaurant table configurations_.

Adorn both `Post` action methods with this custom attribute, and the service now handles both formats:

```
[<span>HttpPost</span>,&nbsp;<span>SelectTableActionMethod</span>]
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&lt;<span>IActionResult</span>&gt;&nbsp;<span>Post</span>(<span>TableJsonDto</span>&nbsp;<span>dto</span>)
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;...</span>
 
[<span>HttpPost</span>,&nbsp;<span>SelectTableActionMethod</span>]
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&lt;<span>IActionResult</span>&gt;&nbsp;<span>Post</span>(<span>ElementBiasedTableXmlDto</span>&nbsp;<span>dto</span>)
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;...</span>
```

While that handles `POST` requests, it doesn't implement content negotiation for `GET` requests.

### Getting XML [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#4cc040875e3349d996b3711a6fea20c0)

In order to `GET` an XML representation, clients can supply an `accept` header value:

```
GET /Tables/153f224c91fb4403988934118cc14024 HTTP/1.1
accept: application/xml
```

which will reply with

```
HTTP/1.1 200 OK
Content-Length: 59
Content-Type: application/xml; charset=utf-8

<span>&lt;</span><span>table</span><span>&gt;&lt;</span><span>type</span><span>&gt;</span>communal<span>&lt;/</span><span>type</span><span>&gt;&lt;</span><span>capacity</span><span>&gt;</span>12<span>&lt;/</span><span>capacity</span><span>&gt;&lt;/</span><span>table</span><span>&gt;</span>
```

How do we implement that?

Keep in mind that since this data-architecture variation uses two different DTOs to model JSON and XML, respectively, an action method can't just return an object of a single type and hope that the ASP.NET framework takes care of the rest. Again, I'm aware of middleware that'll deal nicely with this kind of problem, but not in this architecture; see _Using only a Domain Model to persist restaurant table configurations_ for such a solution.

The best I could come up with, given the constraints I'd imposed on myself, then, was this:

```
[<span>HttpGet</span>(<span>"</span><span>{</span>id<span>}</span><span>"</span>)]
<span>public</span>&nbsp;<span>async</span>&nbsp;<span>Task</span>&lt;<span>IActionResult</span>&gt;&nbsp;<span>Get</span>(<span>string</span>&nbsp;<span>id</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(!<span>Guid</span>.<span>TryParseExact</span>(<span>id</span>,&nbsp;<span>"N"</span>,&nbsp;<span>out</span>&nbsp;<span>var</span>&nbsp;<span>guid</span>))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>BadRequestResult</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>table</span>&nbsp;=&nbsp;<span>await</span>&nbsp;<span>repository</span>.<span>Read</span>(<span>guid</span>).<span>ConfigureAwait</span>(<span>false</span>);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>table</span>&nbsp;<span>is</span>&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>NotFoundResult</span>();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;Demo&nbsp;code&nbsp;only.&nbsp;This&nbsp;doesn't&nbsp;take&nbsp;into&nbsp;account&nbsp;quality&nbsp;values.</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>accept</span>&nbsp;=
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>httpContextAccessor</span>?.HttpContext?.Request.Headers.Accept.<span>ToString</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>accept</span>&nbsp;==&nbsp;<span>"application/json"</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>OkObjectResult</span>(<span>TableJsonDto</span>.<span>From</span>(<span>table</span>.Value));
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>accept</span>&nbsp;==&nbsp;<span>"application/xml"</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>OkObjectResult</span>(<span>ElementBiasedTableXmlDto</span>.<span>From</span>(<span>table</span>.Value));
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>StatusCodeResult</span>((<span>int</span>)<span>HttpStatusCode</span>.NotAcceptable);
}
```

As the comment suggests, this is once again code that barely passes the few tests that I have, but really isn't production-ready. An `accept` header may also look like this:

```
accept: application/xml; q=1.0,application/json; q=0.5
```

Given such an `accept` header, the service ought to return an XML representation with the `application/xml` content type, but instead, this `Get` method returns `406 Not Acceptable`.

As I've already outlined, I'm not going to fix this problem, as this is only an exploration. It seems that we can already conclude that this style of architecture is ill-suited to deal with this kind of problem. If that's the conclusion, then why spend time fixing outstanding problems?

### Attribute-biased XML [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#6db23995819543c3920bc2e7c5d16bb0)

Even so, just to punish myself, apparently, I also tried to add support for an alternative XML format that use attributes to record primitive values. Again, I couldn't make the schema described in the [introductory article](https://blog.ploeh.dk/2024/07/25/three-data-architectures-for-the-server) work, but I did manage to add support for XML documents like these:

```
<span>&lt;</span><span>table</span><span>&nbsp;</span><span>type</span><span>=</span>"<span>communal</span>"<span>&nbsp;</span><span>capacity</span><span>=</span>"<span>12</span>"<span>&nbsp;/&gt;</span>

<span>&lt;</span><span>table</span><span>&nbsp;</span><span>type</span><span>=</span>"<span>single</span>"<span>&nbsp;</span><span>capacity</span><span>=</span>"<span>4</span>"<span>&nbsp;</span><span>minimal-reservation</span><span>=</span>"<span>3</span>"<span>&nbsp;/&gt;</span>
```

The code is similar to what I've already shown, so I'll only list the DTO:

```
[<span>XmlRoot</span>(<span>"table"</span>)]
<span>public</span>&nbsp;<span>class</span>&nbsp;<span>AttributeBiasedTableXmlDto</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;[<span>XmlAttribute</span>(<span>"type"</span>)]
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>string</span>?&nbsp;Type&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;[<span>XmlAttribute</span>(<span>"capacity"</span>)]
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;Capacity&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;[<span>XmlAttribute</span>(<span>"minimal-reservation"</span>)]
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;MinimalReservation&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>bool</span>&nbsp;<span>ShouldSerializeMinimalReservation</span>()&nbsp;=&gt;&nbsp;0&nbsp;&lt;&nbsp;MinimalReservation;
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;Mapping&nbsp;methods&nbsp;not&nbsp;shown&nbsp;here...</span>
}
```

This DTO looks a lot like the `ElementBiasedTableXmlDto` class, only it adorns properties with `XmlAttribute` rather than `XmlElement`.

### Evaluation [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#b3b79b4075b1457dbe92a1a4aae944c6)

Even though I had to compromise on essential goals, I wasted an appalling amount of time and energy on [yak shaving](https://projects.csail.mit.edu/gsb/old-archive/gsb-archive/gsb2000-02-11.html) and [Framework Whac-A-Mole](https://blog.ploeh.dk/2023/10/02/dependency-whac-a-mole). The DTO-based approach to modelling external resources really doesn't work when you need to do substantial content negotiation.

Even so, a DTO-based Ports and Adapters architecture may be useful when that's not a concern. If, instead of a REST API, you're developing a web site, you'll typically not need to vary representation independently of resource. In other words, a web page is likely to have at most one underlying model.

Compared to other large frameworks I've run into, ASP.NET is fairly unopinionated. Even so, the idiomatic way to use it is based on DTOs. DTOs to represent external data. DTOs to represent UI components. DTOs to represent database rows (although they're often called _entities_ in that context). You'll find a ton of examples using this data architecture, so it's incredibly well-described. If you run into problems, odds are that someone has blazed a trail before you.

Even outside of .NET, [this kind of architecture is well-known](https://alistair.cockburn.us/hexagonal-architecture/). While I've learned a thing or two from experience, I've picked up a lot of my knowledge about software architecture from people like [Martin Fowler](https://martinfowler.com/) and [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin).

When you also apply the Dependency Inversion Principle, you'll get good separations of concerns. This aspect of Ports and Adapters is most explicitly described in [Clean Architecture](https://blog.ploeh.dk/ref/clean-architecture). For example, a change to the UI generally doesn't affect the database. You may find that example ridiculous, because why should it, but consult the article _Using a Shared Data Model to persist restaurant table configurations_ to see how this may happen.

The major drawbacks of the DTO-based data architecture is that much mapping is required. With three different DTOs (e.g. JSON DTO, Domain Model, and [ORM](https://en.wikipedia.org/wiki/Object%E2%80%93relational_mapping) Entity), you need four-way translation as indicated in the above figure. People often complain about all that mapping, and no: [ORMs don't reduce the need for mapping](https://blog.ploeh.dk/2023/09/18/do-orms-reduce-the-need-for-mapping).

Another problem is that this style of architecture is _complicated_. As I've [argued elsewhere](https://blog.ploeh.dk/2016/03/18/functional-architecture-is-ports-and-adapters), Ports and Adapters often constitute an unstable equilibrium. While you can make it work, it requires a level of sophistication and maturity among team members that is not always present. And when it goes wrong, it may quickly deteriorate into a [Big Ball of Mud](https://wiki.c2.com/?BigBallOfMud).

### Conclusion [#](https://blog.ploeh.dk/2024/07/29/using-ports-and-adapters-to-persist-restaurant-table-configurations/#162f6985f9674d6397869620b904de1f)

A DTO-based Ports and Adapters architecture is well-described and has good separation of concerns. In this article, however, we've seen that it doesn't deal successfully with realistic content negotiation. While that may seem like a shortcoming, it's a drawback that you may be able to live with. Particularly if you don't need to do content negotiation at all.

This way of organizing code around data is quite common. It's often the default data architecture, and I sometimes get the impression that a development team has 'chosen' to use it without considering alternatives.

It's not a bad architecture despite evidence to the contrary in this article. The scenario examined here may not be relevant. The main drawback of having all these objects playing different roles is all the mapping that's required.

The next data architecture attempts to address that concern.

**Next:** Using a Shared Data Model to persist restaurant table configurations.
