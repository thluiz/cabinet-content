---
created: 2024-06-27T17:53:08 (UTC -03:00)
tags: []
source: https://code-maze.com/semaphore-introduction-csharp/?ref=dailydev
author: Georgios Panagopoulos
---

# Working With The Semaphore Class in C# - Best Practices

> ## Excerpt
> In this article, we'll introduce the Semaphore class in C#. Comparing the Semaphore and SemaphoreSlim classes.

---
In this article, we’ll introduce the semaphore class in C#. We’ll compare the semaphore and semaphoreSlim classes and discuss the best practices for using semaphores.

To download the source code for this article, you can visit our [GitHub repository](https://github.com/CodeMazeBlog/CodeMazeGuides/tree/main/threads-csharp/IntroductionToSemaphoreInCsharp).

Let’s start.

## What is a Semaphore Class in C#?

The [Semaphore](https://learn.microsoft.com/en-us/dotnet/api/system.threading.semaphore?view=net-8.0) class in C# is another mechanism to synchronize a thread’s access to shared resources. It’s more flexible than lock and [mutex](https://code-maze.com/csharp-how-to-use-mutex-class/) because it’s not confined to the context that defines it. In other words, the thread or process that acquires the synchronization handle can differ from the one that releases it. For this reason, **semaphores support a wider range of synchronization scenarios**.

Let’s analyze this class in more detail.

## Initializing Semaphore C# Class

When using semaphores, we can define the maximum number of threads our code will allow to access a protected resource or critical section of code. This is an improvement over lock and mutex which allow only a single thread. When defining a semaphore we can specify these settings:

Support Code Maze on Patreon to get rid of ads and get the best discounts on our products!

[![Become a patron at Patreon!](https://code-maze.com/wp-content/plugins/patron-plugin-pro/plugin/lib/patron-button-and-widgets-by-codebard/images/become_a_patron_button.png)](https://www.patreon.com/oauth2/become-patron?response_type=code&min_cents=100&client_id=9_akhcsDQMGo-FTlVmNpM_uxSV4fbW3vnrz7CBRV9RxwjMPCLfWgodhrcE0UuHH4&scope=identity%20identity[email]&redirect_uri=https://code-maze.com/patreon-authorization/&state=eyJmaW5hbF9yZWRpcmVjdF91cmkiOiJodHRwczpcL1wvY29kZS1tYXplLmNvbVwvaGF0ZW9hcy1hc3BuZXQtY29yZS13ZWItYXBpXC8ifQ%3D%3D&utm_source=https%3A%2F%2Fcode-maze.com%2Fhateoas-aspnet-core-web-api%2F&utm_medium=patreon_wordpress_plugin&utm_campaign=11086160&utm_term=&utm_content=post_unlock_button)

var semaphore = new Semaphore(initialCount: 2, maximumCount: 3);

Here, we create a semaphore that allows a maximum of three threads to access shared resources in parallel and we initially set the counter to two. This is like a section of code allowing access to three ticket holders.

After creating a semaphore, we can call `semaphore.WaitOne()` for the running thread to access the protected code. This will also reduce the semaphore’s internal counter by one.

Once the thread is done with the critical operations, our code can call `semaphore.Release()` to free up a synchronization handle. This will increase the semaphore’s internal counter. 

There is also a `semaphore.Release(int count)` variation of this method to increase the counter more than once.

If the semaphore’s internal counter is zero, any thread that calls `semaphore.WaitOne()` will wait for another thread to call `semaphore.Release()`. In scenarios where access is frequently blocked, inefficiencies can occur because the semaphore will block synchronously any waiting threads.

Everything that we’ve discussed so far works for threads of a single process, and we refer to these types of semaphores as local ones.

## Named Semaphores

Named semaphores allow us to achieve synchronization of threads of different processes. This is similar to what we can achieve with mutex. 

We can create a named semaphore by setting the name as a constructor parameter:

var namedSemaphore = new Semaphore(initialCount: 3, maximumCount: 3, "SemaphoreName");

Is this material useful to you? Consider subscribing and get **ASP.NET Core Web API Best Practices** eBook for [FREE!](https://code-maze.com/free-ebook-aspnetcore-webapi-best-practices/)

**All other features of local semaphores also apply in the case of named semaphores**. 

Let’s see how a local semaphore performs thread synchronization:

Here, we initialize a semaphore and set both the initial count and maximum count to three. The method `AccessWithSemaphoreAsync()` uses a loop to create several `Tasks` that will execute concurrently. It also adds them to a `Task[]` and waits for their completion.

Each thread calls the `WorkerWithSemaphoreAsync()` method once. The commands `_semaphore.WaitOne()` and `_semaphore.Release()` control access to the shared resource that is a [ConcurrentQueue](https://code-maze.com/csharp-concurrentqueue/) that holds values that each concurrent task generates. We also call `await Task.Delay(processParams.SleepDelay)` to mock a long-running operation as if our application was making a network call.

Let’s run our code and see the output:

We observe here that tasks complete their execution in groups of three. This is because our semaphore allows up to three threads to gain access to the critical section of code. Such parallelism is not possible when using lock or mutex. Generally speaking, the more parallelism we can allow, the better execution times we’ll get.

However, as we mentioned, every time a thread calls `semaphore.WaitOne()` and the semaphore’s counter is zero, the execution will be blocked and our program will feel less responsive. For this reason, we can opt to use the `SemaphoreSlim` class instead, which we’ll be looking at next.

## The SemaphoreSlim C# Class

SemaphoreSlim is a variation of semaphore that enables us to instruct threads to wait asynchronously when other threads have gained access to the protected resource. SemaphoreSlim works with a [Task](https://code-maze.com/csharp-tasks-vs-threads/)\-based API allowing us to use [async and await](https://code-maze.com/asynchronous-programming-with-async-and-await-in-asp-net-core/). 

Is this material useful to you? Consider subscribing and get **ASP.NET Core Web API Best Practices** eBook for [FREE!](https://code-maze.com/free-ebook-aspnetcore-webapi-best-practices/)

As its name also indicates, semaphoreSlim is a **lightweight type of semaphore that only works for threads of a single application**. SemaphoreSlim does not support named semaphores and different processes cannot share the same semaphoreSlim synchronization handle.

### Initializing SemaphoreSlim Class

We can create a semaphoreSlim object supplying the initial count and maximum count:

var semaphoreSlim = new SemaphoreSlim(initialCount: 3, maxCount: 3);

Here, we create a semaphoreSlim object which allows a maximum of three threads to access shared resources in parallel and we initially set the counter to three.

A thread can enter the protected code by calling `await semaphoreSlim.WaitAsync()`. This will also decrease the semaphore’s internal counter. More importantly, when the counter is zero the executing thread will have to wait asynchronously**.**  This is a great benefit for our code as the .NET runtime can use the thread to execute other pieces of code while waiting for another thread to increase the counter of the `semaphoreSlim` object by calling `semaphoreSlim.Release()`.

Let’s see how semaphoreSlim manages synchronization:

Here, we initialize a `SemaphoreSlim` object with three threads. The method `AccessWithSemaphoreSlimAsync()` uses a loop to create several task objects that will run in parallel. It also adds them to a  `Task[]`  object to refer to them collectively later.

Each task is created by the method `WorkerWithSemaphoreSlimAsync()` that encloses the protected code. Here, all code is non-blocking. We are making use of the await keyword to call `_semaphoreSlim.WaitAsync()` and `await Task.Delay(processParams.SleepDelay)` to mock a long-running operation. Like before, the concurrent tasks write a value in the `ConcurrentQueue`.

Is this material useful to you? Consider subscribing and get **ASP.NET Core Web API Best Practices** eBook for [FREE!](https://code-maze.com/free-ebook-aspnetcore-webapi-best-practices/)

Let’s run our code and observe the output:

Here, all threads completed their execution in a similar time as in our semaphore example. This is because again we allow three threads to gain access to the critical section of code. It’s not apparent in a simple code example but the big difference is that on `await _semaphoreSlim.WaitAsync()` our program’s threads are not blocked. The runtime can use the waiting threads in other parts of the system, allowing for better usage of the system’s resources.

## The SemaphoreFullException Error

When calling `semaphore.Release()`, it’s our responsibility to ensure that we can still increase the internal counter without violating the maximum limit that we set up the semaphore object with. 

Let’s see how calling `semaphore.Release()` multiple times can cause an error:

Here, we create a semaphore object with an `initialCount` and `maximumCount` of two. 

We then call `semaphore.WaitOne()` and the internal counter is reduced to one. Following that, we call `semaphore.Release()`, increasing the counter’s value to two.

Finally, we call `semaphore.Release()` once more and we see that our code throws a `SemaphoreFullException` as the counter cannot increase more than the maximum count. 

## Semaphores Best Practices

Let’s also see the best practices we should follow with semaphores to achieve optimal performance, avoid traps, and promote code maintainability. 

### **Proper Initialization**

We should choose the maximum and initial count wisely. A high maximum count will result in a high degree of parallelization and better performance. The trade-off is that, as we increase the number of threads we also increase the chances of race conditions and other concurrency issues.

### **Choose the Right Semaphore Type**

We should use the right semaphore type (Semaphore, SemaphoreSlim, Local, and Named Semaphores) depending on the needs of our code. We should be cautious when using semaphores that span multiple processes (named Semaphores) as they limit the scalability of our system. This is because all servers in our cluster share the same lock.

### **Wait and Release Correctly**

We should always ensure that `WaitOne`/`WaitAsync` is matched with a `Release`. This way the semaphore count will remain accurate. We can use try/finally blocks to ensure that we call `Release` even if an exception occurs.

### **Avoid Deadlocks**

As semaphores instruct threads to block and wait they can easily cause deadlocks. We should ensure that all code paths that acquire a semaphore also release it.

In addition, we should avoid nested semaphores where possible, as acquiring multiple semaphores in different orders can lead to deadlocks.  This will simplify the understanding of our code and will increase its maintainability. 

### **Limit the Scope of Semaphores**

For optimal performance, we should keep the semaphore’s scope as narrow as possible to avoid contention. We should use the semaphore to protect the least amount of code possible, exiting the block as fast as we can. We should prefer to use local or instance-level semaphores instead of global ones unless necessary. 

### **Give Semaphore Objects Good Names**

We should give semaphores names that document their purpose and behavior in our code to make it easier for others (and future us) to understand and maintain.  

## Conclusion

In this article, we introduced the C# semaphore class that allows us to synchronize the access of threads and processes to critical pieces of code. We also looked at the semaphoreSlim class and drew comparisons with the semaphore class.

Is this material useful to you? Consider subscribing and get **ASP.NET Core Web API Best Practices** eBook for [FREE!](https://code-maze.com/free-ebook-aspnetcore-webapi-best-practices/)

Finally, we explored the best practices when using Semaphores in C# and .NET.

Liked it? Take a second to support Code Maze on Patreon and get the ad free reading experience!

[![Become a patron at Patreon!](https://code-maze.com/wp-content/plugins/patron-plugin-pro/plugin/lib/patron-button-and-widgets-by-codebard/images/become_a_patron_button.png)](https://www.patreon.com/oauth2/become-patron?response_type=code&min_cents=100&client_id=9_akhcsDQMGo-FTlVmNpM_uxSV4fbW3vnrz7CBRV9RxwjMPCLfWgodhrcE0UuHH4&scope=identity%20identity[email]&redirect_uri=https://code-maze.com/patreon-authorization/&state=eyJmaW5hbF9yZWRpcmVjdF91cmkiOiJodHRwczpcL1wvY29kZS1tYXplLmNvbVwvc2VtYXBob3JlLWludHJvZHVjdGlvbi1jc2hhcnBcLyJ9&utm_source=https%3A%2F%2Fcode-maze.com%2Fsemaphore-introduction-csharp%2F&utm_medium=patreon_wordpress_plugin&utm_campaign=11086160&utm_term=&utm_content=post_unlock_button)
