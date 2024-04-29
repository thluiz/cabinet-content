---
created: 2023-11-03T09:59:32 (UTC -03:00)
tags: []
source: https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev
author: 
---

# Drawbacks of the Singleton Design Pattern | Baeldung

> ## Excerpt
> Learn the general drawbacks of the Singleton design pattern and check out some alternatives.

---
## 1\. Overview[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#overview)

Singleton is one of the [creational design patterns](https://www.baeldung.com/creational-design-patterns) published by the Gang of Four in 1994.

Because of its simple implementation, we tend to overuse it. Therefore, nowadays, it’s considered to be an anti-pattern. Before introducing it in our code, we should ask ourselves if we really need the functionality it provides.

In this tutorial, we’ll discuss the general drawbacks of the [Singleton](https://www.baeldung.com/java-singleton) design pattern and see some alternatives we can use instead.

## 2\. Code Example[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#code-example)

First, let’s create a class we’ll use in our examples:

```
public class Logger {
    private static Logger instance;

    private PrintWriter fileWriter;

    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger();
        }
        return instance;
    }

    private Logger() {
        try {
            fileWriter = new PrintWriter(new FileWriter("app.log", true));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public void log(String message) {
        String log = String.format("[%s]- %s", LocalDateTime.now(), message);
        fileWriter.println(log);
        fileWriter.flush();
    }
}
```

The class above represents a simplified class for logging into the file. We implemented it as a singleton, using the lazy initialization approach.

## 3\. Disadvantages of Singleton[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#disadvantages-of-singleton)

By definition, the Singleton pattern ensures a class has only one instance and, additionally, provides global access to this instance. Therefore, we should use it in cases where we need both of those things.

**Looking at its definition, we can notice it violates the [Single Responsibility Principle](https://www.baeldung.com/java-single-responsibility-principle).** The principle states one class should have only one responsibility.

However, the Singleton pattern has at least two responsibilities – it ensures the class has only one instance and contains business logic.

In the next sections, we’ll discuss some other pitfalls of this design pattern.

### 3.1. Global State[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#1-global-state)

We know [global states](https://www.baeldung.com/cs/global-variables) are considered to be a bad practice and, thus, should be avoided.

Although it may not be obvious, a singleton introduces global variables in our code, but they’re encapsulated within a class.

**Since they’re global, everyone can access and use them. Moreover, if they aren’t immutable, everyone can change them as well.**

Suppose we use the _Logger_ class in several places in our code. Everyone can access and modify its values.

Now, if we encounter a problem in one method that uses it and discover the problem is in the singleton itself, we need to check the entire codebase and every method that uses it to find the impact of the problem.

This can quickly become a bottleneck for our application.

### 3.2. Code Flexibility[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#2-code-flexibility)

Next, in terms of software development, the only certainty lies in the fact our code will likely change in the future.

When a project is in the early stages of development, we can make the assumption there will be no more than one instance of certain classes and define them using the Singleton design pattern.

**However, if requirements change and our assumption turns out to be incorrect, we’d need to put a big effort into refactoring our code.**

Let’s discuss the problem above in our working example.

We assumed we’d only need one instance of our _Logger_ class. What if, in the future, we decide one file isn’t enough?

For instance, we might need separate files for errors and info messages. Additionally, one instance of a class wouldn’t be enough anymore. Next, in order to make the modification possible, we’d need to refactor our entire codebase and remove the singleton, which would require a lot of effort.

**With singletons, we’re making our code tightly coupled and less flexible.**

### 3.3. Dependency Hiding[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#3-dependency-hiding)

Moving forward, singleton promotes hidden dependencies.

**To put it differently, when we’re using them in other classes, we’re hiding the fact these classes depend on a singleton instance.**

Let’s consider the _sum()_ method:

```
public static int sum(int a, int b){
    Logger logger = Logger.getInstance();
    logger.log("A simple message");
    return a + b;
}
```

If we don’t look directly at the implementation of the _sum()_ method, we have no way of knowing it uses the _Logger_ class.

We didn’t pass the dependencies as usual, as arguments to the constructor or a method.

### 3.4. Multithreading[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#4-multithreading)

Next, in a multithreaded environment, singletons can be tricky to implement.

**The main problem is that the global variables are visible to all threads in our code.** Moreover, each thread is unaware of the activities other threads make on the same instance.

Therefore, we can end up facing different problems, such as [race conditions](https://www.baeldung.com/cs/race-conditions) and other synchronization issues.

Our earlier implementation of the _Logger_ class won’t work well in a multithreaded environment. Nothing in our method prevents multiple threads from accessing the _getInstance()_ method at the same time. As a result, we can end up having more than one instance of the _Logger_ class.

Let’s modify the _getInstance()_ method with the [_synchronized_](https://www.baeldung.com/java-synchronized) keyword:

```
public static Logger getInstance() {
    synchronized (Logger.class) {
        if (instance == null) {
            instance = new Logger();
        }
    }
    return instance;
}
```

We’re now forcing every thread to wait its turn. However, we should be aware having synchronization is expensive. In addition, we are introducing an overhead to our method.

If necessary, one of the ways we can solve our problem is by applying the [double-checking locking](https://www.baeldung.com/java-singleton-double-checked-locking) mechanism:

```
private static volatile Logger instance;

public static Logger getInstance() {
    if (instance == null) {
        synchronized (Logger.class) {
            if (instance == null) {
                instance = new Logger();
            }
        }
    }
    return instance;
}
```

**However, we should keep in mind the [JVM](https://www.baeldung.com/jvm-vs-jre-vs-jdk#jvm) allows access to partially constructed objects, which may lead to unexpected behaviors of our program**. Therefore, it’s required to add the [_volatile_](https://www.baeldung.com/java-volatile) keyword to the _instance_ variable.

Other alternatives we might consider include:

-   an eagerly created instance rather than a lazy one
-   an [Enum](https://www.baeldung.com/a-guide-to-java-enums) Singleton
-   the Bill Pugh Singleton

### 3.5. Testing[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#5-testing)

Going further, we can notice the downsides of a singleton when it comes to testing our code.

**Unit tests should test only a small portion of our code and shouldn’t depend on the other services that could fail, causing our test to fail as well.**

Let’s test our _sum()_ method:

```
@Test
void givenTwoValues_whenSum_thenReturnCorrectResult() {
    SingletonDemo singletonDemo = new SingletonDemo();
    int result = singletonDemo.sum(12, 4);
    assertEquals(16, result);
}
```

Even though our test passes, it creates a file with logs since the _sum()_ method uses the _Logger_ class.

If something were wrong with our _Logger_ class, our test would fail. Now, how should we prevent logging from happening?

If applicable, one solution would be to mock the static _getInstance()_ method using [Mockito](https://www.baeldung.com/java-mockito-singleton):

```
@Test
void givenMockedLogger_whenSum_thenReturnCorrectResult() {
    Logger logger = mock(Logger.class);

    try (MockedStatic<Logger> loggerMockedStatic = mockStatic(Logger.class)) {
        loggerMockedStatic.when(Logger::getInstance).thenReturn(logger);
        doNothing().when(logger).log(any());

        SingletonDemo singletonDemo = new SingletonDemo();
        int result = singletonDemo.sum(12, 4);
        Assertions.assertEquals(16, result);
    }
}
```

## 4\. Alternatives to Singleton[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#alternatives-to-singleton)

Finally, let’s discuss some alternatives.

In cases where we need only one instance, **we could use dependency injection.** **In other words, we can create only one instance and pass it as an argument where it’s needed.** This way, we’d raise the awareness of dependencies a method or another class needs in order to function properly.

Additionally, if we need multiple instances in the future, we’d change our code more easily.

Moreover, we can use the [Factory pattern](https://www.baeldung.com/java-factory-pattern) for long-living objects.

## 5\. Conclusion[](https://www.baeldung.com/java-patterns-singleton-cons?ref=dailydev#conclusion)

In this article, we looked at the main drawbacks of the Singleton design pattern.

To sum up, we should use this pattern only when we really need it. Overusing it introduces unnecessary restrictions in cases where we don’t actually need a single instance. As an alternative, we can simply use dependency injection and pass the object as an argument.

As always, the code of all examples is available [over on GitHub](https://github.com/eugenp/tutorials/tree/master/patterns-modules/design-patterns-creational-2).
