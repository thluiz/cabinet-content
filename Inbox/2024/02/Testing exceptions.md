---
created: 2024-02-27T09:59:56 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2024/02/26/testing-exceptions/
author: Mark Seemann
---

# Testing exceptions

> ## Excerpt
> Some thoughts on testing past the happy path.

---
_Some thoughts on testing past the happy path._

Test-driven development is a great development technique that enables you to get rapid feedback on design and implementation ideas. It enables you to rapidly move towards a working solution.

The emphasis on the _happy path_, however, can make you forget about all the things that may go wrong. Sooner or later, though, you may realize that the implementation can fail for a number of reasons, and, wanting to make things more robust, you may want to also subject your error-handling code to automated testing.

This doesn't have to be difficult, but can raise some interesting questions. In this article, I'll try to address a few.

### Throwing exceptions with a dynamic mock [#](https://blog.ploeh.dk/2024/02/26/testing-exceptions/#ead73eb4bc4b45eba0bde0cf61269814)

In [a question to another article](https://blog.ploeh.dk/2023/08/14/replacing-mock-and-stub-with-a-fake#0afe67b375254fe193a3fd10234a1ce9) AmirB asks how to use a [Fake Object](http://xunitpatterns.com/Fake%20Object.html) to test exceptions. Specifically, since [a Fake is a Test Double with a coherent contract](https://blog.ploeh.dk/2023/11/13/fakes-are-test-doubles-with-contracts) it'll be inappropriate to let it throw exceptions that relate to different implementations.

Egads, that was quite abstract, so let's consider a concrete example.

[The original article](https://blog.ploeh.dk/2023/08/14/replacing-mock-and-stub-with-a-fake) that AmirB asked about used this interface as an example:

```
<span>public</span>&nbsp;<span>interface</span>&nbsp;<span>IUserRepository</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;User&nbsp;<span>Read</span>(<span>int</span>&nbsp;<span>userId</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>void</span>&nbsp;<span>Create</span>(<span>int</span>&nbsp;<span>userId</span>);
}
```

Granted, this interface is a little odd, but it should be good enough for the present purpose. As AmirB wrote:

> "In scenarios where dynamic mocks (like Moq) are employed, we can mock a method to throw an exception, allowing us to test the expected behavior of the System Under Test (SUT)."

Specifically, this might look like this, using [Moq](https://github.com/devlooped/moq):

```
[Fact]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>CreateThrows</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>td</span>&nbsp;=&nbsp;<span>new</span>&nbsp;Mock&lt;IUserRepository&gt;();
&nbsp;&nbsp;&nbsp;&nbsp;td.Setup(<span>r</span>&nbsp;=&gt;&nbsp;r.Read(1234)).Returns(<span>new</span>&nbsp;User&nbsp;{&nbsp;Id&nbsp;=&nbsp;0&nbsp;});
&nbsp;&nbsp;&nbsp;&nbsp;td.Setup(<span>r</span>&nbsp;=&gt;&nbsp;r.Create(It.IsAny&lt;<span>int</span>&gt;())).Throws(MakeSqlException());
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;SomeController(td.Object);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;sut.GetUser(1234);
 
&nbsp;&nbsp;&nbsp;&nbsp;Assert.NotNull(actual);
}
```

It's just an example, but the point is that since you can make a dynamic mock do anything that you can define in code, you can also use it to simulate database exceptions. This test pretends that the `IUserRepository` throws a [SqlException](https://learn.microsoft.com/dotnet/api/system.data.sqlclient.sqlexception) from the `Create` method.

Perhaps the `GetUser` implementation now looks like this:

```
<span>public</span>&nbsp;User&nbsp;<span>GetUser</span>(<span>int</span>&nbsp;<span>userId</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>u</span>&nbsp;=&nbsp;<span>this</span>.userRepository.Read(userId);
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(u.Id&nbsp;==&nbsp;0)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>try</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.userRepository.Create(userId);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>catch</span>&nbsp;(SqlException)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;u;
}
```

If you find the example contrived, I don't blame you. The `IUserRepository` interface, the `User` class, and the `GetUser` method that orchestrates them are all degenerate in various ways. I originally created this little code example to discuss [data flow verification](https://blog.ploeh.dk/2013/10/23/mocks-for-commands-stubs-for-queries), and I'm now stretching it beyond any reason. I hope that you can look past that. The point I'm making here is more general, and doesn't hinge on particulars.

### Fake [#](https://blog.ploeh.dk/2024/02/26/testing-exceptions/#ed58e2e387234a7ebd3c97a384841d9f)

[The article](https://blog.ploeh.dk/2023/08/14/replacing-mock-and-stub-with-a-fake) also suggests a `FakeUserRepository` that is small enough that I can repeat it here.

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>FakeUserRepository</span>&nbsp;:&nbsp;Collection&lt;User&gt;,&nbsp;IUserRepository
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>void</span>&nbsp;<span>Create</span>(<span>int</span>&nbsp;<span>userId</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Add(<span>new</span>&nbsp;User&nbsp;{&nbsp;Id&nbsp;=&nbsp;userId&nbsp;});
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;User&nbsp;<span>Read</span>(<span>int</span>&nbsp;<span>userId</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>user</span>&nbsp;=&nbsp;<span>this</span>.SingleOrDefault(<span>u</span>&nbsp;=&gt;&nbsp;u.Id&nbsp;==&nbsp;userId);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(user&nbsp;==&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;User&nbsp;{&nbsp;Id&nbsp;=&nbsp;0&nbsp;};
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;user;
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

The question is how to use something like this when you want to test exceptions? It's possible that this little class may produce [errors that I've failed to predict](https://blog.ploeh.dk/2024/01/29/error-categories-and-category-errors), but it certainly doesn't throw any `SqlExceptions`!

Should we inflate `FakeUserRepository` by somehow also giving it the capability to throw particular exceptions?

### Throwing exceptions from Test Doubles [#](https://blog.ploeh.dk/2024/02/26/testing-exceptions/#c116b036864348b7938dfb6805e0c2dd)

I understand why AmirB asks that question, because it doesn't seem right. As a start, it would go against the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle). The `FakeUserRepository` would then have more than reason to change: You'd have to change it if the `IUserRepository` interface changes, but you'd also have to change it if you wanted to simulate a different error situation.

Good coding practices apply to test code as well. Test code is code that you have to read and maintain, so all the good practices that keep production code in good shape also apply to test code. This may include [the SOLID principles](https://en.wikipedia.org/wiki/SOLID), unless you're of the mind that [SOLID ought to be a thing of the past](https://dannorth.net/cupid-for-joyful-coding/).

If you really _must_ throw exceptions from a [Test Double](https://martinfowler.com/bliki/TestDouble.html), perhaps a dynamic mock object as shown above is the best option. No-one says that if you use a Fake Object for most of your tests you can't use a dynamic mock library for truly one-off test cases.Or perhaps a one-off Test Double that throws the desired exception.

I would, however, consider it a code smell if this happens too often. Not a test smell, but a code smell.

### Is the exception part of the contract? [#](https://blog.ploeh.dk/2024/02/26/testing-exceptions/#302a9e4462744a55974b8fdab6f70054)

You may ask yourself whether a particular exception type is part of an object's contract. As I always do, when I use the word _contract_, I refer to a set of invariants, pre-, and postconditions, taking a cue from [Object-Oriented Software Construction](https://blog.ploeh.dk/ref/oosc). See also my video course [Encapsulation and SOLID](https://blog.ploeh.dk/encapsulation-and-solid) for more details.

You can _imply_ many things about a contract when you have a static type system at your disposal, but there are always rules that you can't express that way. Parts of a contract are implicitly understood, or communicated in other ways. Code comments, [docstrings](https://en.wikipedia.org/wiki/Docstring), or similar, are good options.

What may you infer from the `IUserRepository` interface? What should you _not_ infer?

I'd expect the `Read` method to return a `User` object. This code example hails us [from 2013](https://blog.ploeh.dk/2013/10/23/mocks-for-commands-stubs-for-queries), before C# had [nullable reference types](https://learn.microsoft.com/dotnet/csharp/nullable-references). Around that time I'd begun using [Maybe](https://blog.ploeh.dk/2018/03/26/the-maybe-functor) to signal that the return value might be missing. This is a _convention_, so the reader needs to be aware of it in order to correctly infer that part of the contract. Since the `Read` method does _not_ return `Maybe<User>` I might infer that a non-null `User` object is guaranteed; that's a post-condition.

These days, I'd also use [asynchronous APIs to hint that I/O is involved](https://blog.ploeh.dk/2020/07/27/task-asynchronous-programming-as-an-io-surrogate), but again, the example is so old and simplified that this isn't the case here. Still, regardless of how this is communicated to the reader, if an interface (or base class) is intended for I/O, we may expect it to fail at times. In most languages, such errors manifest as exceptions.

At least two questions arise from such deliberations:

-   Which exception types may the methods throw?
-   Can you even handle such exceptions?

Should `SqlException` even be part of the contract? Isn't that an implementation detail?

The `FakeUserRepository` class neither uses SQL Server nor throws `SqlExceptions`. You may imagine other implementations that use a document database, or even just another relational database than SQL Server (Oracle, MySQL, PostgreSQL, etc.). Those wouldn't throw `SqlExceptions`, but perhaps other exception types.

According to the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle),

> "Abstractions should not depend upon details. Details should depend upon abstractions."

If we make `SqlException` part of the contract, an implementation detail becomes part of the contract. Not only that: With an implementation like the above `GetUser` method, which catches `SqlException`, we've also violated the [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle). If you injected another implementation, one that throws a different type of exception, the code would no longer work as intended.

Loosely coupled code shouldn't look like that.

Many specific exceptions are of [a kind that you can't handle anyway](https://blog.ploeh.dk/2024/01/29/error-categories-and-category-errors). On the other hand, if you do decide to handle particular error scenarios, make it part of the contract, or, as Michael Feathers puts it, [extend the domain](https://youtu.be/AnZ0uTOerUI?si=1gJXYFoVlNTSbjEt).

### Integration testing [#](https://blog.ploeh.dk/2024/02/26/testing-exceptions/#ed86f41415724219a0afbf9d669ec1b7)

How should we unit test specific exception? [Mu](https://en.wikipedia.org/wiki/Mu_(negative)), we shouldn't.

> "Personally, I avoid using try-catch blocks in repositories or controllers and prefer handling exceptions in middleware (e.g., ErrorHandler). In such cases, I write separate unit tests for the middleware. Could this be a more fitting approach?"

That is, I think, an excellent approach to those exceptions that that you've decided to not handle explicitly. Such middleware would typically log or otherwise notify operators that a problem has arisen. You could also write some general-purpose middleware that performs retries or implements the [Circuit Breaker](https://martinfowler.com/bliki/CircuitBreaker.html) pattern, but reusable libraries that do that already exist. Consider using one.

Still, you may have decided to implement a particular feature by leveraging a capability of a particular piece of technology, and the code you intent to write is complicated enough, or important enough, that you'd like to have good test coverage. How do you do that?

I'd suggest an integration test.

I don't have a good example lying around that involves throwing specific exceptions, but something similar may be of service. The example code base that accompanies my book [Code That Fits in Your Head](https://blog.ploeh.dk/code-that-fits-in-your-head) pretends to be an online restaurant reservation system. Two concurrent clients may compete for the last table on a particular date; a typical race condition.

There are more than one way to address such a concern. As implied in [a previous article](https://blog.ploeh.dk/2024/01/29/error-categories-and-category-errors), you may decide to rearchitect the entire application to be able to handle such edge cases in a robust manner. For the purposes of the book's example code base, however, I considered a _lock-free architecture_ out of scope. Instead, I had in mind dealing with that issue by taking advantage of .NET and SQL Server's support for lightweight transactions via a [TransactionScope](https://learn.microsoft.com/dotnet/api/system.transactions.transactionscope). While this is a handy solution, it's utterly dependent on the technology stack. It's a good example of an implementation detail that I'd rather not expose to a unit test.

Instead, I wrote a [self-hosted integration test](https://blog.ploeh.dk/2021/01/25/self-hosted-integration-tests-in-aspnet) that runs against a real SQL Server instance (automatically deployed and configured on demand). It tests _behaviour_ rather than implementation details:

```
[Fact]
<span>public</span>&nbsp;<span>async</span>&nbsp;Task&nbsp;<span>NoOverbookingRace</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>start</span>&nbsp;=&nbsp;DateTimeOffset.UtcNow;
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>timeOut</span>&nbsp;=&nbsp;TimeSpan.FromSeconds(30);
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>i</span>&nbsp;=&nbsp;0;
&nbsp;&nbsp;&nbsp;&nbsp;<span>while</span>&nbsp;(DateTimeOffset.UtcNow&nbsp;-&nbsp;start&nbsp;&lt;&nbsp;timeOut)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>await</span>&nbsp;PostTwoConcurrentLiminalReservations(start.DateTime.AddDays(++i));
}
 
<span>private</span>&nbsp;<span>static</span>&nbsp;<span>async</span>&nbsp;Task&nbsp;<span>PostTwoConcurrentLiminalReservations</span>(DateTime&nbsp;<span>date</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;date&nbsp;=&nbsp;date.Date.AddHours(18.5);
&nbsp;&nbsp;&nbsp;&nbsp;<span>using</span>&nbsp;<span>var</span>&nbsp;<span>service</span>&nbsp;=&nbsp;<span>new</span>&nbsp;RestaurantService();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>task1</span>&nbsp;=&nbsp;service.PostReservation(<span>new</span>&nbsp;ReservationDtoBuilder()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.WithDate(date)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.WithQuantity(10)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.Build());
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>task2</span>&nbsp;=&nbsp;service.PostReservation(<span>new</span>&nbsp;ReservationDtoBuilder()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.WithDate(date)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.WithQuantity(10)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.Build());
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>await</span>&nbsp;Task.WhenAll(task1,&nbsp;task2);
 
&nbsp;&nbsp;&nbsp;&nbsp;Assert.Single(actual,&nbsp;<span>msg</span>&nbsp;=&gt;&nbsp;msg.IsSuccessStatusCode);
&nbsp;&nbsp;&nbsp;&nbsp;Assert.Single(
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actual,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>msg</span>&nbsp;=&gt;&nbsp;msg.StatusCode&nbsp;==&nbsp;HttpStatusCode.InternalServerError);
}
```

This test attempts to make two concurrent reservations for ten people. This is also the maximum capacity of the restaurant: It's impossible to seat twenty people. We'd like for one of the requests to win that race, while the server should reject the loser.

This test is only concerned with the behaviour that clients can observe, and since this code base contains hundreds of other tests that inspect HTTP response messages, this one only looks at the status codes.

The implementation handles the potential overbooking scenario like this:

```
<span>private</span>&nbsp;<span>async</span>&nbsp;Task&lt;ActionResult&gt;&nbsp;<span>TryCreate</span>(Restaurant&nbsp;<span>restaurant</span>,&nbsp;Reservation&nbsp;<span>reservation</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>using</span>&nbsp;<span>var</span>&nbsp;<span>scope</span>&nbsp;=&nbsp;<span>new</span>&nbsp;TransactionScope(TransactionScopeAsyncFlowOption.Enabled);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>reservations</span>&nbsp;=&nbsp;<span>await</span>&nbsp;Repository.ReadReservations(restaurant.Id,&nbsp;reservation.At);
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>now</span>&nbsp;=&nbsp;Clock.GetCurrentDateTime();
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(!restaurant.MaitreD.WillAccept(now,&nbsp;reservations,&nbsp;reservation))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;NoTables500InternalServerError();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>await</span>&nbsp;Repository.Create(restaurant.Id,&nbsp;reservation);
 
&nbsp;&nbsp;&nbsp;&nbsp;scope.Complete();
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;Reservation201Created(restaurant.Id,&nbsp;reservation);
}
```

Notice the `TransactionScope`.

I'm under the illusion that I could radically change this implementation detail without breaking the above test. Granted, unlike [another experiment](https://blog.ploeh.dk/2023/09/04/decomposing-ctfiyhs-sample-code-base), this hypothesis isn't one I've put to the test.

### Conclusion [#](https://blog.ploeh.dk/2024/02/26/testing-exceptions/#9659f21863e74c288d5c2d36534eaa37)

How does one automatically test error branches? Most unit testing frameworks come with APIs that makes it easy to verify that specific exceptions were thrown, so that's not the hard part. If a particular exception is part of the System Under Test's contract, just test it like that.

On the other hand, when it comes to objects composed with other objects, implementation details may easily leak through in the shape of specific exception types. I'd think twice before writing a test that verifies whether a piece of client code (such as the above `SomeController`) handles a particular exception type (such as `SqlException`).

If such a test is difficult to write because all you have is a Fake Object (e.g. `FakeUserRepository`), that's only good. The rapid feedback furnished by test-driven development strikes again. Listen to your tests.

You should probably not write that test at all, because there seems to be an issue with the planned structure of the code. Address _that_ problem instead.
