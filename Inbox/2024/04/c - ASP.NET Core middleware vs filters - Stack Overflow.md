---
created: 2024-04-30T19:29:36 (UTC -03:00)
tags: []
source: https://stackoverflow.com/questions/42582758/asp-net-core-middleware-vs-filters/42583583#42583583
author: Saber
        
            5,43044 gold badges3333 silver badges4444 bronze badges
---

# c# - ASP.NET Core middleware vs filters - Stack Overflow

> ## Excerpt
> After reading about ASP.NET Core middleware, I am confused about when I should use filters and when I should use middleware as they seem to achieve the same goal.
When should middleware be used ins...

---
After reading about ASP.NET Core middleware, I am confused about when I should use filters and when I should use middleware as they seem to achieve the same goal. When should middleware be used instead of filters?

[

![Majid Parvin's user avatar](https://i.stack.imgur.com/L4uTr.png?s=64&g=1)

](https://stackoverflow.com/users/7818013/majid-parvin)

[Majid Parvin](https://stackoverflow.com/users/7818013/majid-parvin)

4,7625 gold badges31 silver badges49 bronze badges

asked Mar 3, 2017 at 15:29

[

![Saber's user avatar](https://www.gravatar.com/avatar/e522984cf50241d715f32a6eeb2ea707?s=64&d=identicon&r=PG)

](https://stackoverflow.com/users/1262198/saber)

There is a video about this on channel 9: [ASP.NET Monsters #91: Middleware vs. Filters](https://channel9.msdn.com/Series/aspnetmonsters/ASPNET-Monsters-91-Middleware-vs-Filters). To summarize the video:  

The execution of request starts and we have a middleware, and another middleware, think of it like the "Russian dolls inside of dolls" and eventually the routing middleware kicks in and then request goes into the MVC pipline. [![enter image description here](https://i.stack.imgur.com/31siT.jpg)](https://i.stack.imgur.com/31siT.jpg) So if you don't require the context of MVC (let's say you're concerned about flow and execution, like responding to headers some pre-routing mechanism, etc.) then use **middlewares**.  
But if you require the context of MVC and you want to operate against actions then use **filters**.

[

![Aage's user avatar](https://i.stack.imgur.com/J6JFc.png?s=64&g=1)

](https://stackoverflow.com/users/2877982/aage)

[Aage](https://stackoverflow.com/users/2877982/aage)

6,1122 gold badges33 silver badges58 bronze badges

answered Mar 3, 2017 at 16:11

[

![Saber's user avatar](https://www.gravatar.com/avatar/e522984cf50241d715f32a6eeb2ea707?s=64&d=identicon&r=PG)

](https://stackoverflow.com/users/1262198/saber)

[Saber](https://stackoverflow.com/users/1262198/saber)

5,4304 gold badges33 silver badges44 bronze badges

Middleware operate on the level of ASP.NET Core and can act on every single request that comes in to the application.

MVC filters on the other hand only run for requests that come to MVC.

So for example, if I wanted to enforce that all requests must be done over HTTPS, I would have to use a middleware for that. If I made an MVC filter that did that, users could still request e.g. static files over HTTP.

But then on the other hand something that logs request durations in MVC controllers could absolutely be an action filter.

answered Mar 3, 2017 at 15:32

[

![juunas's user avatar](https://www.gravatar.com/avatar/4e4748ed2fb97e8a8d4dd6d439625135?s=64&d=identicon&r=PG)

](https://stackoverflow.com/users/1658906/juunas)

[juunas](https://stackoverflow.com/users/1658906/juunas)

56.6k13 gold badges121 silver badges158 bronze badges

The execution of `middleware` occurs before the MVC context becomes available in the pipeline. That is, `middleware` does not have access to the `ActionExecutingContext` or the `ActionExecutedContext` in the case of an ActionFilter for example. What you do have access to is the `HttpContext`, which will allow you to perform actions on the request as well as the response. Since model binding hasn’t occurred yet, using middleware would not be suited to running a validation function or modifying values. `Middleware` will also run on every request regardless of which controller or action is called.

On the other hand, `filters` will only run on specified actions and controllers unless you register the filter globally in the startup. Since you have full access to the context you can also access the controller and action itself.

Source and example: [thetechplatform.com](https://www.thetechplatform.com/post/middleware-and-filters-power-in-asp-net-core)

answered May 22, 2020 at 14:19

[

![Majid Parvin's user avatar](https://i.stack.imgur.com/L4uTr.png?s=64&g=1)

](https://stackoverflow.com/users/7818013/majid-parvin)

[Majid Parvin](https://stackoverflow.com/users/7818013/majid-parvin)

4,7625 gold badges31 silver badges49 bronze badges

Filter pipeline is similar to middleware pipeline in many ways, but they also have some differences that should be considered when deciding which approach to be used.

**What are the similarities:**

-   Incoming requests pass through a middleware component and outgoing responses to those requests are passing again through the same middleware when going to the client. Some filters (resource, action, and result) are also two-way, others (authorization and exception) run only once for a request, and page filters (Razor pages specific) run three times.
-   Middlewares can short-circuit a request by directly returning a response, instead of executing the entire middleware pipeline and not passing the request down to later middlewares. Filters can also short-circuit the filter pipeline by directly returning a response.
-   Middlewares are used for cross-cutting concerns (eg: logging, performance profiling or exception handling). Filters are also used for dealing with cross-cutting concerns.

**What are the differences:**

-   A middleware can run for all requests while filters will only run for requests that reach the `EndpointMiddleware` and execute an action from an API Controller or a Razor Page.
-   Filters have access to MVC components (eg: `ModelState` or `IActionResults`). Middleware works at a lower level compared to filters and is independent of MVC and Razor Pages, so it can't use any of those related components.
-   Filters are designed to be applied to a subset of requests, not to all of them. For instance, we can apply a filter on a single controller or a single Razor Page. In contrast, middlewares don't have this kind of design.

So, when trying to figure out what should use: filters or middlewares, the answer should come from the above comparison. As a TL;DR:

-   middleware is the more general concept, which operates on lower-level abstractions like `HttpContext`, so can be applied in a wider area. **Also we should be aware of the fact that the functionality we need to implement has no MVC-specific requirements**
-   filters can enable us the usage of MVC-constructs and can be used for implementing custom and specific behavior for some MVC actions. **It is not so general as middleware.**

answered Feb 8, 2022 at 13:45

[

![Dina Bogdan's user avatar](https://i.stack.imgur.com/844pQ.jpg?s=64&g=1)

](https://stackoverflow.com/users/6237026/dina-bogdan)

[Dina Bogdan](https://stackoverflow.com/users/6237026/dina-bogdan)

4,5445 gold badges30 silver badges58 bronze badges

According to my experience Middleware can be used for the entire request pipeline but Filters is only used within the Routing Middleware where we have an MVC pipeline so Middleware operates at the level of ASP.NET Core but Filters executes only when requests come to the MVC pipeline.

answered Jan 13, 2023 at 4:52

[

![mayur hole's user avatar](https://lh3.googleusercontent.com/a/AEdFTp7UAgCmbjU28CKVMU755ApAiL5EYPGfsBLylnAf=k-s64)

](https://stackoverflow.com/users/20997269/mayur-hole)

### Sign up or [log in](https://stackoverflow.com/users/login?ssrc=question_page&returnurl=https%3a%2f%2fstackoverflow.com%2fquestions%2f42582758%2fasp-net-core-middleware-vs-filters%23new-answer)

Sign up using Google

Sign up using Facebook

Sign up using Email and Password

### Post as a guest

Email

Required, but never shown
