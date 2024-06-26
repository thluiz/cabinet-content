---
created: 2024-05-08T08:43:37 (UTC -03:00)
tags: [typescript,programming,architecture,braziliandevs,software,coding,development,engineering,inclusive,community]
source: https://dev.to/kevin-uehara/solid-the-simple-way-to-understand-47im
author: Kevin Toshihiro Uehara
---

# SOLID - The Simple Way To Understand - DEV Community

> ## Excerpt
> Hi there!!! How have you been doing? Are you all right? I hope so!  Today I'm going to talk about a...

---
Hi there!!! How have you been doing? Are you all right? I hope so!

Today I'm going to talk about a theme that's everyone talks or write about. But sometimes it's difficult to undersand every principle. I'm talking about SOLID.

A lot of people, when I ask about SOLID, propably always remember of the first principle (Single Responsability Principle). But when I ask about another, some people don't remember or feel difficult to explain. AND I UNDERSTAND.

Really, it's difficult to explain, without coding or remender the definition of each principle. But in this article, I want to present each principle on easy way. So I will use Typescript to exemplify.

So let's begin!

## [](https://dev.to/kevin-uehara/solid-the-simple-way-to-understand-47im#single-responsability-principle-srp)Single Responsability Principle - SRP

The easier principle to understand and remember.  
When whe are coding, it's easy to identify when we are forgetting the principle.

Let's imagine that we have a TaskManager class:  

```
<span>class</span> <span>TaskManager</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>connectAPI</span><span>():</span> <span>void</span> <span>{}</span>
  <span>createTask</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Create Task</span><span>"</span><span>);</span>
  <span>}</span>
  <span>updateTask</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Update Task</span><span>"</span><span>);</span>
  <span>}</span>
  <span>removeTask</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Remove Task</span><span>"</span><span>);</span>
  <span>}</span>
  <span>sendNotification</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Send Notification</span><span>"</span><span>);</span>
  <span>}</span>
  <span>sendReport</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Send Report</span><span>"</span><span>);</span>
  <span>}</span>
<span>}</span>
```

All right! Probably do you notice thee problem, isnt'it?  
The class TaskManager have a lot of responsabilities that don't belong to her. For example: sendNotification and sendReport methods.

Now, let's refact and apply the solution:  

```
<span>class</span> <span>APIConnector</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>connectAPI</span><span>():</span> <span>void</span> <span>{}</span>
<span>}</span>

<span>class</span> <span>Report</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>sendReport</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Send Report</span><span>"</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>Notificator</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>sendNotification</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Send Notification</span><span>"</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>TaskManager</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>createTask</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Create Task</span><span>"</span><span>);</span>
  <span>}</span>
  <span>updateTask</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Update Task</span><span>"</span><span>);</span>
  <span>}</span>
  <span>removeTask</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Remove Task</span><span>"</span><span>);</span>
  <span>}</span>
<span>}</span>
```

Simple, isnt'it? We just separete the notification and report in specified classes. Now we are respecting the Single Principle Responsability!

The definition: `Each class must have one, and only one, reason to change`.

## [](https://dev.to/kevin-uehara/solid-the-simple-way-to-understand-47im#open-closed-principle-ocp)Open Closed Principle - OCP

The second principle. Also, I consider easy to understand. A tip for you, if you notice that you have a lot of conditions in some method to verify something, perhaps you are in case of the OCP.

Let's imagine the following example of Exam Class:  

```
<span>type</span> <span>ExamType</span> <span>=</span> <span>{</span>
  <span>type</span><span>:</span> <span>"</span><span>BLOOD</span><span>"</span> <span>|</span> <span>"</span><span>XRay</span><span>"</span><span>;</span>
<span>};</span>

<span>class</span> <span>ExamApprove</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>approveRequestExam</span><span>(</span><span>exam</span><span>:</span> <span>ExamType</span><span>):</span> <span>void</span> <span>{</span>
    <span>if </span><span>(</span><span>exam</span><span>.</span><span>type</span> <span>===</span> <span>"</span><span>BLOOD</span><span>"</span><span>)</span> <span>{</span>
      <span>if </span><span>(</span><span>this</span><span>.</span><span>verifyConditionsBlood</span><span>(</span><span>exam</span><span>))</span> <span>{</span>
        <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Blood Exam Approved</span><span>"</span><span>);</span>
      <span>}</span>
    <span>}</span> <span>else</span> <span>if </span><span>(</span><span>exam</span><span>.</span><span>type</span> <span>===</span> <span>"</span><span>XRay</span><span>"</span><span>)</span> <span>{</span>
      <span>if </span><span>(</span><span>this</span><span>.</span><span>verifyConditionsXRay</span><span>(</span><span>exam</span><span>))</span> <span>{</span>
        <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>XRay Exam Approved!</span><span>"</span><span>);</span>
      <span>}</span>
    <span>}</span>
  <span>}</span>

  <span>verifyConditionsBlood</span><span>(</span><span>exam</span><span>:</span> <span>ExamType</span><span>):</span> <span>boolean</span> <span>{</span>
    <span>return</span> <span>true</span><span>;</span>
  <span>}</span>
  <span>verifyConditionsXRay</span><span>(</span><span>exam</span><span>:</span> <span>ExamType</span><span>):</span> <span>boolean</span> <span>{</span>
    <span>return</span> <span>false</span><span>;</span>
  <span>}</span>
<span>}</span>
```

Yeah, propably you already saw this code several times. First we are breaking the first principle SRP and making a lot of conditions.

Now imagine if another type of examination appears, for example, ultrasound. We need to add anonther method to verify and another condition.

Let's refact this code:  

```
<span>type</span> <span>ExamType</span> <span>=</span> <span>{</span>
  <span>type</span><span>:</span> <span>"</span><span>BLOOD</span><span>"</span> <span>|</span> <span>"</span><span>XRay</span><span>"</span><span>;</span>
<span>};</span>

<span>interface</span> <span>ExamApprove</span> <span>{</span>
  <span>approveRequestExam</span><span>(</span><span>exam</span><span>:</span> <span>NewExamType</span><span>):</span> <span>void</span><span>;</span>
  <span>verifyConditionExam</span><span>(</span><span>exam</span><span>:</span> <span>NewExamType</span><span>):</span> <span>boolean</span><span>;</span>
<span>}</span>

<span>class</span> <span>BloodExamApprove</span> <span>implements</span> <span>ExamApprove</span> <span>{</span>
  <span>approveRequestExam</span><span>(</span><span>exam</span><span>:</span> <span>ExamApprove</span><span>):</span> <span>void</span> <span>{</span>
    <span>if </span><span>(</span><span>this</span><span>.</span><span>verifyConditionExam</span><span>(</span><span>exam</span><span>))</span> <span>{</span>
      <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Blood Exam Approved</span><span>"</span><span>);</span>
    <span>}</span>
  <span>}</span>
  <span>verifyConditionExam</span><span>(</span><span>exam</span><span>:</span> <span>ExamApprove</span><span>):</span> <span>boolean</span> <span>{</span>
    <span>return</span> <span>true</span><span>;</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>RayXExamApprove</span> <span>implements</span> <span>ExamApprove</span> <span>{</span>
  <span>approveRequestExam</span><span>(</span><span>exam</span><span>:</span> <span>ExamApprove</span><span>):</span> <span>void</span> <span>{</span>
    <span>if </span><span>(</span><span>this</span><span>.</span><span>verifyConditionExam</span><span>(</span><span>exam</span><span>))</span> <span>{</span>
      <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>RayX Exam Approved</span><span>"</span><span>);</span>
    <span>}</span>
  <span>}</span>
  <span>verifyConditionExam</span><span>(</span><span>exam</span><span>:</span> <span>NewExamType</span><span>):</span> <span>boolean</span> <span>{</span>
    <span>return</span> <span>true</span><span>;</span>
  <span>}</span>
<span>}</span>
```

Woow, much better! Now if another type of examination appears we just implements the interface `ExamApprove`. And if another type of verification for the exam comes up, we only update the interface.

Definition: `Software entities (such as classes and methods) must be open for extension but closed for modification`

## [](https://dev.to/kevin-uehara/solid-the-simple-way-to-understand-47im#liskov-substitution-principle-lsp)Liskov Substitution Principle - LSP

One of more complicated to understand and examplain. But how I said, I will make easier to you understand.

Imagine that you have an university and two types of students. Student and Post Graduated Student.  

```
<span>class</span> <span>Student</span> <span>{</span>
  <span>constructor</span><span>(</span><span>public</span> <span>name</span><span>:</span> <span>string</span><span>)</span> <span>{}</span>

  <span>study</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>`</span><span>${</span><span>this</span><span>.</span><span>name</span><span>}</span><span> is studying`</span><span>);</span>
  <span>}</span>

  <span>deliverTCC</span><span>()</span> <span>{</span>
    <span>/** Problem: Post graduate Students don't delivery TCC */</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>PostgraduateStudent</span> <span>extends</span> <span>Student</span> <span>{</span>
  <span>study</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>`</span><span>${</span><span>this</span><span>.</span><span>name</span><span>}</span><span> is studying and searching`</span><span>);</span>
  <span>}</span>
<span>}</span>
```

We have a problem here, we are extending of Student, but the Post Graduated Student don't need to deliver a TCC. He only study and search.

How we can resolve this problem? Simple! Let's create a class Student and separete the Student of graduation and Post Graduation:  

```
<span>class</span> <span>Student</span> <span>{</span>
  <span>constructor</span><span>(</span><span>public</span> <span>name</span><span>:</span> <span>string</span><span>)</span> <span>{}</span>

  <span>study</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>`</span><span>${</span><span>this</span><span>.</span><span>name</span><span>}</span><span> is studying`</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>StudentGraduation</span> <span>extends</span> <span>Student</span> <span>{</span>
  <span>study</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>`</span><span>${</span><span>this</span><span>.</span><span>name</span><span>}</span><span> is studying`</span><span>);</span>
  <span>}</span>

  <span>deliverTCC</span><span>()</span> <span>{}</span>
<span>}</span>

<span>class</span> <span>StudentPosGraduation</span> <span>extends</span> <span>Student</span> <span>{</span>
  <span>study</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>`</span><span>${</span><span>this</span><span>.</span><span>name</span><span>}</span><span> is studying and searching`</span><span>);</span>
  <span>}</span>
<span>}</span>
```

Now we have a better way to approach to separete their respective responsabilities. The name of this principle can be scary but its principle is simple.

Definition: `Derived classes (or child classes) must be able to replace their base classes (or parent classes)`

## [](https://dev.to/kevin-uehara/solid-the-simple-way-to-understand-47im#interface-segregation-principle-isp)Interface Segregation Principle - ISP

To understand this principle, the trick is remember of the definition. A class shound not be forced to implement methods that will not be used.

So imagine that you have a class the implement a interface that its never be used.

Let's imagine a scenario with an Seller and a Recepsionist of some shop. Both seller and recepsionist have a sallary, but only a seller have a commission.

Let's see the problem:  

```
<span>interface</span> <span>Employee</span> <span>{</span>
  <span>salary</span><span>():</span> <span>number</span><span>;</span>
  <span>generateCommission</span><span>():</span> <span>void</span><span>;</span>
<span>}</span>

<span>class</span> <span>Seller</span> <span>implements</span> <span>Employee</span> <span>{</span>
  <span>salary</span><span>():</span> <span>number</span> <span>{</span>
    <span>return</span> <span>1000</span><span>;</span>
  <span>}</span>
  <span>generateCommission</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Generating Commission</span><span>"</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>Receptionist</span> <span>implements</span> <span>Employee</span> <span>{</span>
  <span>salary</span><span>():</span> <span>number</span> <span>{</span>
    <span>return</span> <span>1000</span><span>;</span>
  <span>}</span>
  <span>generateCommission</span><span>():</span> <span>void</span> <span>{</span>
    <span>/** Problem: Receptionist don't have commission  */</span>
  <span>}</span>
<span>}</span>
```

Both implements the Employee interface, but the receptionist don't have comission. So we are force to implement a method that never it will be used.

So the solution:  

```
<span>interface</span> <span>Employee</span> <span>{</span>
  <span>salary</span><span>():</span> <span>number</span><span>;</span>
<span>}</span>

<span>interface</span> <span>Commissionable</span> <span>{</span>
  <span>generateCommission</span><span>():</span> <span>void</span><span>;</span>
<span>}</span>

<span>class</span> <span>Seller</span> <span>implements</span> <span>Employee</span><span>,</span> <span>Commissionable</span> <span>{</span>
  <span>salary</span><span>():</span> <span>number</span> <span>{</span>
    <span>return</span> <span>1000</span><span>;</span>
  <span>}</span>

  <span>generateCommission</span><span>():</span> <span>void</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Generating Commission</span><span>"</span><span>);</span>
  <span>}</span>
<span>}</span>

<span>class</span> <span>Receptionist</span> <span>implements</span> <span>Employee</span> <span>{</span>
  <span>salary</span><span>():</span> <span>number</span> <span>{</span>
    <span>return</span> <span>1000</span><span>;</span>
  <span>}</span>
<span>}</span>
```

Easy beasy! Now we have two interfaces! The employer class and the comissionable interface. Now only the Seller will implement the two interfaces where it will have the commmission. The receptionist don't only implements the employee. So the Receptionist don't be forced to implement the method that will never be used.

Definition: `A class should not be forced to implement interfaces and methods that will not be used.`

## [](https://dev.to/kevin-uehara/solid-the-simple-way-to-understand-47im#dependency-inversion-principle-dip)Dependency Inversion Principle - DIP

The last one! By name you can think that is hard to remember! But probably you already see this principle every time.

Imagine that you have a Service class that integrates with a Repository class that will call the Database, for example a Postgress. But if the repository class change and the database change for a MongoDB, for example.

Let's see the example:  

```
<span>interface</span> <span>Order</span> <span>{</span>
  <span>id</span><span>:</span> <span>number</span><span>;</span>
  <span>name</span><span>:</span> <span>string</span><span>;</span>
<span>}</span>

<span>class</span> <span>OrderRepository</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>saveOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>)</span> <span>{}</span>
<span>}</span>

<span>class</span> <span>OrderService</span> <span>{</span>
  <span>private</span> <span>orderRepository</span><span>:</span> <span>OrderRepository</span><span>;</span>

  <span>constructor</span><span>()</span> <span>{</span>
    <span>this</span><span>.</span><span>orderRepository</span> <span>=</span> <span>new</span> <span>OrderRepository</span><span>();</span>
  <span>}</span>

  <span>processOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>orderRepository</span><span>.</span><span>saveOrder</span><span>(</span><span>order</span><span>);</span>
  <span>}</span>
<span>}</span>
```

We notice that the repository is OrderService class is directly coupled to the concrete implementation of OrderRepository class.

Let's refact this example:  

```
<span>interface</span> <span>Order</span> <span>{</span>
  <span>id</span><span>:</span> <span>number</span><span>;</span>
  <span>name</span><span>:</span> <span>string</span><span>;</span>
<span>}</span>

<span>class</span> <span>OrderRepository</span> <span>{</span>
  <span>constructor</span><span>()</span> <span>{}</span>
  <span>saveOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>)</span> <span>{}</span>
<span>}</span>

<span>class</span> <span>OrderService</span> <span>{</span>
  <span>private</span> <span>orderRepository</span><span>:</span> <span>OrderRepository</span><span>;</span>

  <span>constructor</span><span>(</span><span>repository</span><span>:</span> <span>OrderRepository</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>orderRepository</span> <span>=</span> <span>repository</span><span>;</span>
  <span>}</span>

  <span>processOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>)</span> <span>{</span>
    <span>this</span><span>.</span><span>orderRepository</span><span>.</span><span>saveOrder</span><span>(</span><span>order</span><span>);</span>
  <span>}</span>
<span>}</span>
```

Nice! Much better! Now we receive the repository as parameter on the constructor to instanciate and use. Now we depend of the abstraction and we don't need to know what repository we are using.

Definition: `depend on abstractions rather than concrete implementations`

## [](https://dev.to/kevin-uehara/solid-the-simple-way-to-understand-47im#finishing)Finishing

So how you feeling now? I hope that with this easy examples you can remember and understand what and why to use this principles in your code. It makes easier to undestand and scale, besides you are applying clean code.

I hope you that you liked!  
Thank you so much and stay well always!

Contacts:  
Linkedin: [https://www.linkedin.com/in/kevin-uehara/](https://www.linkedin.com/in/kevin-uehara/)  
Instagram: [https://www.instagram.com/uehara\_kevin/](https://www.instagram.com/uehara_kevin/)  
Twitter: [https://twitter.com/ueharaDev](https://twitter.com/ueharaDev)  
Github: [https://github.com/kevinuehara](https://github.com/kevinuehara)  
dev.to: [https://dev.to/kevin-uehara](https://dev.to/kevin-uehara)  
Youtube: [https://www.youtube.com/@ueharakevin/](https://www.youtube.com/@ueharakevin/)
