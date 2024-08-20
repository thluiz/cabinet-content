---
created: 2024-08-20T17:24:31 (UTC -03:00)
tags: []
source: https://blog.mihaioltean.com/the-hollywood-principle
author: Mihai Oltean
---

# The Hollywood Principle

> ## Excerpt
> Oh boy, when I first understood this principle, I felt like a rockstar.  ⭐️
I remember first reading about it in Head First Design Patterns. It was so catchy that it stuck with me since then. If you haven't heard of it, you might know it as Inversion...

---
Oh boy, when I first understood this principle, I felt like a rockstar. ⭐️

I remember first reading about it in Head First Design Patterns. It was so catchy that it stuck with me since then. If you haven't heard of it, you might know it as Inversion of Control.

## The Hollywood Principle

**The Hollywood Principle** is simple:

> Don't call us, we'll call you.

It’s a powerful yet polite saying used by organizations in their hiring processes when they are not considering an applicant for the job.

In parallel to Hollywood, this usually comes from casting directors informing actors that they are not interested in their services after an audition.

But where does this principle apply in our lives as "superstar" software developers?

## Inversion of control (IoC)

A very fancy term describing a very simple concept.

What IoC means is that the framework will give control to custom code and then regain it at the end of execution.

Framework? Custom code? Let’s better visualize this with a diagram:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721991280910/38c95826-6d01-46d9-93f6-c5c037db26ce.png?auto=compress,format&format=webp)

The framework or container works as a higher-level component in this case, while the custom code acts as a lower-level component. This way, you maintain a high degree of decoupling between the framework and the custom code.

Returning to our Hollywood example, the director maintains a decoupled relationship with the actors. The director will call the actor only when needed, keeping options open for other actors (custom code).

For a comparison, let’s see how the code would look without IoC. Imagine we have a shopping cart class that processes payments, but we have different payment providers.

```
<span>// A high-level component that creates its own service instance</span>
<span>class</span> ShoppingCart {
    <span>private</span> paymentService: PaymentService;

    <span>constructor</span>(<span>paymentServiceType: <span>string</span></span>) {
        <span>// ShoppingCart directly creates instances of PaymentService implementations</span>
        <span>if</span> (paymentServiceType === <span>'paypal'</span>) {
            <span>this</span>.paymentService = <span>new</span> PaypalService();
        } <span>else</span> <span>if</span> (paymentServiceType === <span>'stripe'</span>) {
            <span>this</span>.paymentService = <span>new</span> StripeService();
        } <span>else</span> {
            <span>throw</span> <span>new</span> <span>Error</span>(<span>'Invalid payment service type'</span>);
        }
    }

    checkout(amount: <span>number</span>): <span>void</span> {
        <span>// The high-level component calls the low-level component</span>
        <span>this</span>.paymentService.processPayment(amount);
    }
}
```

In our case, the `ShoppingCart` acts as the framework, and the `PaymentService` acts as the custom code.

```
<span>// No dependency injection: ShoppingCart decides the payment service implementation</span>
<span>const</span> cart = <span>new</span> ShoppingCart(<span>'paypal'</span>);
cart.checkout(<span>100.0</span>);
```

There are some significant drawbacks to this approach:

-   The code is more open to modification than extension. Each time we add a new service provider, we will need to modify the framework.
    
-   Adding services as concrete classes makes us responsible for their dependencies as well.
    
-   Testing becomes challenging; you will have to rely on concrete classes instead of mocks.
    

___

Let’s refactor this to follow the Hollywood-style code.

First, we need to decouple the `ShoppingCart` from the `PaymentService`.

We’ll do this by defining an interface and relying on that, instead of the concrete service type:

```
<span>// Define an interface for a service</span>
<span>interface</span> PaymentService {
    processPayment(amount: <span>number</span>): <span>void</span>;
}


<span>// A high-level component that uses the service</span>
<span>class</span> ShoppingCart {
    <span>private</span> paymentService: PaymentService;

    <span>constructor</span>(<span>paymentService: PaymentService</span>) {
        <span>this</span>.paymentService = paymentService;
    }

    checkout(amount: <span>number</span>): <span>void</span> {
        <span>// The high-level component calls the low-level component</span>
        <span>this</span>.paymentService.processPayment(amount);
    }
}
```

By applying this refactor, we’ve transformed our code from being open to modification to being open for extension and closed to modification.

Now, we can add different implementations of `PaymentService`:

```
<span>class</span> PaypalService <span>implements</span> PaymentService {
    processPayment(amount: <span>number</span>): <span>void</span> {
        <span>console</span>.log(<span>`Processing payment of $<span>${amount}</span> through PayPal.`</span>);
    }
}

<span>class</span> StripeService <span>implements</span> PaymentService {
    processPayment(amount: <span>number</span>): <span>void</span> {
        <span>console</span>.log(<span>`Processing payment of $<span>${amount}</span> through Stripe.`</span>);
    }
}
```

Need to integrate with new payment providers? No worries. We won’t need to change the framework; we just need to add a new service implementation, and that’s it.

```
<span>const</span> paypalService = <span>new</span> PaypalService(); 
<span>const</span> cart = <span>new</span> ShoppingCart(paypalService);
cart.checkout(<span>100.0</span>);
```

What we’ve done is move the dependencies higher in the hierarchy. Now, dependency management is handled outside of `ShoppingCart`.

We can definitely see some improvements here:

-   `ShoppingCart` is now loosely coupled by using interfaces instead of concrete classes.
    
-   `ShoppingCart` is more flexible. With a new service provider, no changes are needed in `ShoppingCart`.
    
-   Testing `ShoppingCart` is easier because we can mock the service implementation.
    

Several design patterns effectively utilize the Hollywood Principle, including the Template, Strategy, and Factory patterns. I encourage you to explore them in [_Head First Design Patterns: A Brain-Friendly Guide_](https://www.amazon.com/Head-First-Design-Patterns-Brain-Friendly/dp/0596007124) for more examples and use cases.

Now go be a Hollywood superstar developer and remember — _Don’t call us, we’ll call you._
