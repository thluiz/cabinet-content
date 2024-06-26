---
created: 2024-06-03T09:36:02 (UTC -03:00)
tags: []
source: https://craftbettersoftware.com/p/tdd-5-test-smells-5-solutions?ref=dailydev
author: Daniel Moka
---

# TDD: 5 test smells - 5 solutions - by Daniel Moka

> ## Excerpt
> 5 practical tips to improve the quality of your tests

---
## Motivation

_Hey TDD friend! This is FREE lesson from my upcoming [Test-Driven Development course](https://craftbettersoftware.com/p/launching-my-tdd-course-in-2-weeks). I launch it next Monday on 3rd June with a 50% discount. The discount will be valid only for 3 days, so stay tuned._

You can’t have clean code without clean tests. Treat your tests as first-class citizens. Writing clean tests is as important as writing clean production code.

When I help teams to improve their testing practices, I always see the same 5 test smells. Here’s what they are, how to detect them, and how you can fix them:

## 1\. Fragile tests

#### ❌ Problem

Your tests fail to compile when you change the production code.

#### 🔃 Cause

Your tests are coupled to low-level implementation details such as internal functions or classes.

As you see in this example below, the test is coupled to implementation details like _**FindById(..)**_ and _**Store(..)**_ methods.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1fa7fe9e-2ad4-4267-8097-2ac6a4465585_1089x573.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1fa7fe9e-2ad4-4267-8097-2ac6a4465585_1089x573.png)

#### ✅ Solution

**Don't couple your tests to implementation details or code structure. Instead, couple them to the behaviors of the public APIs.**

The following test uses fake to verify state changes without being coupled to the internal functions:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c9c41aa-8373-4a32-8c16-aefcc4889243_775x595.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c9c41aa-8373-4a32-8c16-aefcc4889243_775x595.png)

## 2\. Eager tests

#### ❌ Problem

The test is hard to understand as it verifies too much functionality in a single test method.

#### 🔃 Cause

Having multiple Act and Assert parts within a test.

The following test is an Eager Test because it covers multiple behaviors: bank account creation, deposit, invalid withdrawal, and successful withdrawal.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84289e22-61de-4d0f-bc26-69287f7759b2_2736x1855.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F84289e22-61de-4d0f-bc26-69287f7759b2_2736x1855.png)

#### ✅ Solution

You can fix the Eager Test smell **by splitting up the test into multiple test cases.** Each test should verify a single condition:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbed12fbd-a5cb-4d0a-b7ac-8e021988ae75_2318x1970.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbed12fbd-a5cb-4d0a-b7ac-8e021988ae75_2318x1970.png)

## 3\. Mistery **Guest**

#### ❌ Problem

The reader must look outside the test to understand the behavior being verified.

#### 🔃 Cause

Some global context is used to initialize data for reusing objects and avoiding duplication.

In the following test, we don’t have any info about the _**User**_ as it is initialized outside the test context:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb7fad793-3362-4c86-9d70-971c684e33c7_2850x1060.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb7fad793-3362-4c86-9d70-971c684e33c7_2850x1060.png)

#### ✅ Solution

**Include everything needed in the test to understand the tested behavior.** The Don't Repeat Yourself principle is less important in test code. Prefer readability over removing duplication:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93b9edc7-0912-4f18-9bfd-06d0d42a5cad_3019x2232.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93b9edc7-0912-4f18-9bfd-06d0d42a5cad_3019x2232.png)

## 4\. Test with irrelevant data

#### ❌ Problem

It's hard to tell which data affects the test result.

#### 🔃 Cause

There is too much irrelevant information in the test case.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6098740a-13e5-4fbd-bfd4-62b3a117eea5_6194x1676.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6098740a-13e5-4fbd-bfd4-62b3a117eea5_6194x1676.png)

#### ✅ Solution

**Don't pollute your tests with irrelevant test data!** Irrelevant data increases noise and decreases readability. A test should read like a story with only the essential information.

There are two great ways to hide irrelevant data:

-   abstract irrelevant information into a method
    
-   use the builder pattern
    

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F717c639f-b551-48d2-8268-40e946553977_4711x2105.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F717c639f-b551-48d2-8268-40e946553977_4711x2105.png)

## 5\. Test with logic

#### ❌ Problem

Tests are too complex to understand and prone to bugs.

#### 🔃 Cause

You use logic in tests such as if-else statements, loops, or switch cases.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44985c13-96ee-4a47-b625-c168e2df325e_2889x2166.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F44985c13-96ee-4a47-b625-c168e2df325e_2889x2166.png)

#### ✅ Solution

**Avoid any logic in your test code!** Once you feel the need for these, it's a smell that you test more than one thing. You can get rid of test logic by splitting up your tests into multiple test cases.

Another option is to use parameterized tests to declare your test cases and to avoid duplication:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7e19a537-6e67-4e6f-b9d4-0110bd02da43_2580x1583.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7e19a537-6e67-4e6f-b9d4-0110bd02da43_2580x1583.png)

## Conclusion

Writing quality tests is a skill worth its weight in gold. In my upcoming TDD course, I will cover many quality testing topics like:

-   Other test patterns and anti-patterns
    
-   How to write clean tests
    
-   The 5 different test doubles
    
-   The two schools of TDD
    
-   The power of mutation testing
    
-   3 real-world projects in C#, TypeScript and Rust, built with TDD
    

**Launch in one week on 3rd June!** If you have any questions, drop it below as a comment. See you soon, my friend.

### Subscribe to Craft Better Software

Master Test-Driven Development (TDD) and Extreme Programming (XP)
