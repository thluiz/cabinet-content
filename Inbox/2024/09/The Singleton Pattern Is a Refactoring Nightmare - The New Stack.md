---
created: 2024-09-06T14:35:44 (UTC -03:00)
tags: []
source: https://thenewstack.io/unmasking-the-singleton-anti-pattern/?ref=dailydev
author: Eyal Halpern Shalev
---

# The Singleton Pattern Is a Refactoring Nightmare - The New Stack

> ## Excerpt
> Dependency injection fosters a more modular, testable and adaptable codebase than the singleton design pattern for managing shared resources.

---
The [singleton pattern](https://en.wikipedia.org/wiki/Singleton_pattern), a design pattern that restricts a class to a single instance, is often touted as a solution for managing shared resources or global state. While seemingly convenient, the singleton pattern often introduces subtle complexities and drawbacks that can undermine code maintainability and [testability](https://thenewstack.io/software-testing/). This article delves into these issues, illustrating how [dependency injection (DI)](https://en.wikipedia.org/wiki/Dependency_injection) offers a more robust and flexible alternative.

## The Singleton’s Deceptive Allure: A Testing Quagmire

Consider a common scenario: You’re developing an authentication module with [unit tests](https://thenewstack.io/unit-tests-are-overrated-rethinking-testing-strategies/) ensuring its correctness. Your “happy path” test, which verifies successful authentication, passes without a hitch:  

<table><tbody><tr><td data-settings="show"></td><td><div><p><span>def</span><span> </span><span>test_user_authorized</span>():</p><p><span>&nbsp;&nbsp;</span><span>with</span><span> </span><span>clean_db</span>()<span> </span><span>as</span><span> </span><span>connection</span>:</p><p><span>&nbsp;&nbsp;&nbsp;&nbsp;</span><span>app</span>.<span>create_user</span>(<span>"bob"</span>,<span> </span><span>"secret"</span>)</p><p><span>&nbsp;&nbsp;&nbsp;&nbsp;</span><span>assert</span><span> </span><span>app</span>.<span>is_authorized</span>(<span>"bob"</span>,<span> </span><span>"secret"</span>)</p></div></td></tr></tbody></table>

  
However, the subsequent test, designed to assert failed authorization, unexpectedly fails:  

<table><tbody><tr><td data-settings="show"></td><td><div><p><span>def</span><span> </span><span>test_user_unauthorized</span>():</p><p><span>&nbsp;&nbsp;</span><span>with</span><span> </span><span>clean_db</span>()<span> </span><span>as</span><span> </span><span>connection</span>:</p><p><span>&nbsp;&nbsp;&nbsp;&nbsp;</span><span>assert</span><span> </span><span>not</span><span> </span><span>app</span>.<span>is_authorized</span>(<span>"bob"</span>,<span> </span><span>"secret"</span>)</p></div></td></tr></tbody></table>

  
Upon investigation, you discover that someone named Jerry introduced a cache to enhance login performance. This cache, residing in the global scope as a simple dictionary, retains state across tests, thus polluting subsequent test runs and causing false negatives.

Your initial attempt to mitigate this issue involves resetting the global cache before each test:  

<table><tbody><tr><td data-settings="show"></td><td><div><p><span>def</span><span> </span><span>test_user_authorized</span>():</p><p><span>&nbsp;&nbsp;</span><span>global</span><span> </span><span>cache</span></p><p><span>&nbsp;&nbsp;</span><span>cache</span><span> </span>=<span> </span><span>dict</span>()<span>&nbsp;&nbsp;</span><span># Reset the global cache</span></p><p><span>&nbsp;&nbsp;</span><span># ... rest of the test</span></p></div></td></tr></tbody></table>

<table><tbody><tr><td data-settings="show"></td><td><div><p><span>def</span><span> </span><span>test_user_unauthorized</span>():</p><p><span>&nbsp;&nbsp;</span><span>global</span><span> </span><span>cache</span></p><p><span>&nbsp;&nbsp;</span><span>cache</span><span> </span>=<span> </span><span>dict</span>()<span>&nbsp;&nbsp;</span><span># Reset the global cache</span></p><p><span>&nbsp;&nbsp;</span><span># ... rest of the test</span></p></div></td></tr></tbody></table>

  
While this workaround temporarily restores test integrity, it’s a fragile solution, highlighting the perils of globals in testing. However, it does demonstrate that with globals, you retain some control — the ability to reset or replace the reference can serve as a temporary bandage.

TRENDING STORIES

1.  [5 API Testing Best Practices to Tame the Wild West](https://thenewstack.io/reining-in-the-api-wild-west-5-api-testing-best-practices/)
2.  [Shift Left on a Budget: Cost-Savvy Testing for Microservices](https://thenewstack.io/shift-left-on-a-budget-cost-savvy-testing-for-microservices/)
3.  [Why Staging Doesn't Scale for Microservice Testing](https://thenewstack.io/why-staging-doesnt-scale-for-microservice-testing/)
4.  [The Struggle To Test Microservices Before Merging](https://thenewstack.io/the-struggle-to-test-microservices-before-merging/)
5.  [Cutting the High Cost of Testing Microservices](https://thenewstack.io/cutting-the-high-cost-of-testing-microservices/)

## The Singleton’s Downfall: A Refactoring Nightmare

A month passes and you get an angry message from Jerry that your tests are failing for his urgent pull request (PR) — and you need to fix them!

Looking at his PR, you discover that he changed the cache implementation from a simple dictionary to a dedicated AuthCache class, implemented as a singleton. This seemingly innocuous change, however, triggers a cascade of test failures.

Now the previously straightforward task of resetting the cache becomes a complex ordeal. The singleton’s private state is no longer easily accessible or replaceable. Attempts to manipulate the singleton’s internals through reflection or other invasive techniques undermine the very principles of encapsulation and abstraction that the [refactor](https://thenewstack.io/refactoring-is-not-bad-until-it-is/) aimed to uphold.

The once-simple tests, designed to be independent and self-contained, now rely on intricate mocking or patching mechanisms to isolate the singleton’s effects. This increased complexity not only makes the tests harder to understand and maintain but also exposes the inherent fragility of singletons in a testing environment.

In this scenario, the singleton’s allure of simplicity and global accessibility has morphed into a testing nightmare. The very feature that made it convenient for sharing state across the application now hinders the ability to write reliable and maintainable tests. The singleton, initially intended to simplify the codebase, has become a source of complexity and frustration.

## Dependency Injection: The Solution to Testing Woes

The root cause of these issues lies in the inherent statefulness of globals (and singletons especially). They violate the principle of isolation, making tests unpredictable and prone to failure. DI provides an elegant solution; by explicitly passing dependencies into objects, it thereby eliminates hidden state and ensures test isolation.  

<table><tbody><tr><td data-settings="show"></td><td><div><p><span>_default_auth_cache</span><span> </span>=<span> </span><span>AuthCache</span>()</p><p><span>class</span><span> </span><span>App</span>:</p><p><span>&nbsp;&nbsp;</span><span>def</span><span> </span><span>__init__</span>(<span>self</span>,<span> </span><span>auth_cache</span>:<span> </span><span>AuthCache</span><span> </span>=<span> </span><span>None</span>,<span> </span>...):</p><p><span>&nbsp;&nbsp;&nbsp;&nbsp;</span><span>self</span>.<span>_auth_cache</span><span> </span>=<span> </span><span>auth_cache </span><span>or</span><span> </span><span>_default_auth_cache</span></p><p><span>&nbsp;&nbsp;&nbsp;&nbsp;</span><span># ...</span></p></div></td></tr></tbody></table>

<table><tbody><tr><td data-settings="show"></td><td><div><p><span># In your tests:</span></p><p><span>def</span><span> </span><span>test_user_authorized</span>():</p><p><span>&nbsp;&nbsp;</span><span>app</span><span> </span>=<span> </span><span>App</span>(<span>auth_cache</span>=<span>AuthCache</span>())<span>&nbsp;&nbsp;</span><span># Inject a fresh cache</span></p><p><span>&nbsp;&nbsp;</span><span># ...</span></p><p><span>def</span><span> </span><span>test_user_unauthorized</span>():</p><p><span>&nbsp;&nbsp;</span><span>app</span><span> </span>=<span> </span><span>App</span>(<span>auth_cache</span>=<span>AuthCache</span>())<span>&nbsp;&nbsp;</span><span># Inject a fresh cache</span></p><p><span>&nbsp;&nbsp;</span><span># ...</span></p></div></td></tr></tbody></table>

  
With DI, each test receives a fresh, isolated AuthCache instance, guaranteeing consistent and predictable behavior. This [architectural](https://roadmap.sh/software-design-architecture) shift not only strengthens your tests but also confers additional advantages:

-   **Enhanced modularity:** DI promotes loose coupling between components, facilitating codebase evolution.
-   **Improved testability:** Injecting mock dependencies simplifies unit testing by enabling isolated testing of individual components.
-   **Simplified configuration:** Dependencies can be easily swapped or configured through injection.

## Beyond Singletons: Embracing Flexibility and Control

The misconception that DI precludes true singletons is a common pitfall. In fact, DI offers greater flexibility, allowing for both single and multiple instances as needed. This adaptability proves invaluable in testing, enabling fine-grained control over scenarios and uncovering hidden interactions. While genuine singletons might be warranted in specific cases, DI frequently provides a more versatile and maintainable approach.

## Conclusion

While singletons might seem enticing for managing shared resources, their inherent drawbacks can lead to brittle tests, complex refactoring scenarios and reduced code flexibility. Dependency injection, by contrast, fosters a more modular, testable and adaptable codebase. By embracing DI, you empower your software with the resilience and flexibility required to thrive in a constantly evolving environment.
