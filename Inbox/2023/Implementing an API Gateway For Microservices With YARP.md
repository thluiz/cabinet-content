---
created: 2024-01-12T21:26:38 (UTC -03:00)
tags: []
source: https://www.milanjovanovic.tech/blog/implementing-an-api-gateway-for-microservices-with-yarp?utm_source=Twitter&utm_campaign=08.01.2024&utm_medium=socail
author: Milan Jovanović
---

# Implementing an API Gateway For Microservices With YARP

> ## Excerpt
> Large Microservice-based systems can consist of tens or even hunders of individual services. A client application needs to have all of this information to be able to make requests to the relevant microservice directly.
However, this has numerous issues such as security concerns, complexity, and coupling.
We can solve this by introducing an API gateway that acts as a reverse proxy to accept API calls from the client application, forwarding this traffic to the appropriate service.
The API gateway also enforces security and ensures scalability and high availability.
In this week's newsletter, I'll show you how to implement an API gateway using the YARP reverse proxy.
Here's what we will cover: - Difference between API gateway and reverse proxy - Installing and configuring YARP - Creating an API gateway with YARP - Authentication and rate limiting on the API gateway
Let's get started.

---
Thank you to our sponsors who keep this newsletter free to the reader:

Today's issue is sponsored by [**Treblle.**](https://www.treblle.com/?utm_source=MailInf&utm_medium=Milan) Treblle is a lightweight SDK that helps engineering and product teams [**build, ship & maintain REST-based APIs faster.**](https://www.treblle.com/?utm_source=MailInf&utm_medium=Milan) Simple integration for all popular languages & frameworks, including .NET 6.

And by [**IcePanel.**](https://u.icepanel.io/1eec0e6f) IcePanel realigns your engineering & product teammates on the technical decisions made across the business. [**Explain your complex software systems with an interactive map**](https://u.icepanel.io/1eec0e6f) that teammates can drill down to the level of detail they need.

Large **Microservice-based** systems can consist of tens or even hundreds of individual services. A client application needs to have all of this information to be able to make requests to the relevant **microservice** directly.

However, this has numerous issues, such as security concerns, increased complexity, and coupling.

We can solve this by introducing an **API gateway** that acts as a **reverse proxy** to accept API calls from the client application and forward them to the appropriate service.

The **API gateway** also enforces security and ensures scalability and high availability.

In this week's newsletter, I'll show you how to implement an **API gateway** for your **microservices system** using the **YARP reverse proxy**.

Here's what we will cover:

-   Difference between **API gateway** and **reverse proxy**
-   Installing and configuring **YARP**
-   Creating an **API gateway** with **YARP**
-   **Authentication** and **rate limiting** on the **API gateway**

Let's dive in.

## [What's The Difference Between an API Gateway And a Reverse Proxy?](https://www.milanjovanovic.tech/blog/implementing-an-api-gateway-for-microservices-with-yarp?utm_source=Twitter&utm_campaign=08.01.2024&utm_medium=socail#whats-the-difference-between-an-api-gateway-and-a-reverse-proxy)

A **reverse proxy** and an **API gateway** are similar concepts, but they serve different purposes.

A **reverse proxy** acts as an intermediary between clients and servers. The clients can only call the backend servers through the **reverse proxy**, which forwards the request to the appropriate server. It hides the implementation details of individual servers inside the internal network.

A **reverse proxy** is commonly used for:

-   Load balancing
-   Caching
-   Security
-   SSL termination

![](https://www.milanjovanovic.tech/blogs/mnw_045/reverse_proxy.png?imwidth=3840)

An **API gateway** is a specific type of **reverse proxy** designed for managing APIs. It acts as a single entry point for API consumers to the various backend services.

The key characteristics of an **API gateway** are:

-   Request routing and composition
-   Request/response transformations
-   Authentication and authorization
-   Rate limiting
-   Monitoring

Also, note that an **API gateway** can perform **load balancing** and other functionalities mentioned for reverse proxies.

Now let's see how to use a **reverse proxy** to implement an **API gateway**.

## [Installing And Configuring YARP](https://www.milanjovanovic.tech/blog/implementing-an-api-gateway-for-microservices-with-yarp?utm_source=Twitter&utm_campaign=08.01.2024&utm_medium=socail#installing-and-configuring-yarp)

**YARP** (Yet Another Reverse Proxy) is a library developed by Microsoft to address the needs of various teams needing to build a **reverse proxy**. It's open source and built with .NET, so it integrates nicely with the existing ecosystem.

Let's install `Yarp.ReverseProxy` **NuGet** package to get started:

```powershell
Install-Package Yarp.ReverseProxy
```

Next, we're going to call:

-   `AddReverseProxy` to add the required services for **YARP**
-   `LoadFromConfig` to load the **reverse proxy** configuration from application settings
-   `MapReverseProxy` to introduce the **reverse proxy** middleware

```csharp
var builder = WebApplication.CreateBuilder(args); builder.Services.AddReverseProxy() .LoadFromConfig(builder.Configuration.GetSection("ReverseProxy")); var app = builder.Build(); app.MapReverseProxy(); app.Run();
```

We need to tell the **YARP** reverse proxy how to route the incoming requests to the individual microservices.

**YARP** uses the concept of `Routes` to represent request patterns for the proxy and `Clusters` to represent the services to forward those requests.

```json
{ "ReverseProxy": { "Routes": { ... }, "Clusters": { ... } } }
```

Here's an example **YARP configuration** with a `{**catch-all}` pattern that will route all incoming requests to the one destination server.

```json
{ "ReverseProxy": { "Routes": { "ROUTE_NAME": { "ClusterId": "CLUSTER_NAME", "Match": { "Path": "{**catch-all}" } } }, "Clusters": { "CLUSTER_NAME": { "Destinations": { "destination1": { "Address": "https://www.milanjovanovic.tech/" } } } } } }
```

## [Implementing an API Gateway With YARP](https://www.milanjovanovic.tech/blog/implementing-an-api-gateway-for-microservices-with-yarp?utm_source=Twitter&utm_campaign=08.01.2024&utm_medium=socail#implementing-an-api-gateway-with-yarp)

We can use **YARP** to build an **API gateway** by providing the configuration for the services we want to route traffic to.

I created a sample [API gateway implementation with YARP](https://github.com/m-jovanovic/yarp-api-gateway-sample) on GitHub, so you can give it a try. The system has two services, the `Users.Api` and `Products.Api`, which are .NET 7 applications.

If a request comes in matching the `/users-service/{**catch-all}`, for example `/users-service/users`, it will be routed to the `users-cluster`. The same logic applies for the `products-cluster`. We can apply more advanced transformations through the `Transforms` section.

```json
{ "ReverseProxy": { "Routes": { "users-route": { "ClusterId": "users-cluster", "Match": { "Path": "/users-service/{**catch-all}" }, "Transforms": [{ "PathPattern": "{**catch-all}" }] }, "products-route": { "ClusterId": "products-cluster", "Match": { "Path": "/products-service/{**catch-all}" }, "Transforms": [{ "PathPattern": "{**catch-all}" }] } }, "Clusters": { "users-cluster": { "Destinations": { "destination1": { "Address": "https://localhost:5201/" } } }, "products-cluster": { "Destinations": { "destination1": { "Address": "https://localhost:5101/" } } } } } }
```

We now have a functioning **API gateway** built with **YARP**, routing requests to two individual services.

But what else can we do with **YARP**?

## [Adding Authentication](https://www.milanjovanovic.tech/blog/implementing-an-api-gateway-for-microservices-with-yarp?utm_source=Twitter&utm_campaign=08.01.2024&utm_medium=socail#adding-authentication)

The **API gateway** can enforce **authentication** and **authorization** at the entry point to the system before letting authenticated requests proceed.

And **YARP** supports integrating with the existing authentication & authorization middleware.

You first need to define an **authorization policy**:

```csharp
builder.Services.AddAuthorization(options => { options.AddPolicy("authenticated", policy => policy.RequireAuthenticatedUser()); });
```

And call `UseAuthentication` and `UseAuthorization` to add the respective middleware to the request pipeline. It's important to add them before calling `MapReverseProxy`.

```csharp
app.UseAuthentication(); app.UseAuthorization(); app.MapReverseProxy();
```

Now all you have to do is add the `AuthorizationPolicy` section to the reverse proxy configuration:

```json
"users-route": { "ClusterId": "users-cluster", "AuthorizationPolicy": "authenticated", "Match": { "Path": "/users-service/{**catch-all}" }, "Transforms": [ { "PathPattern": "{**catch-all}" } ] }
```

**YARP** will forward most credentials to the proxied services, such as cookies or bearer tokens because it might be important to identify the user in the individual microservices.

## [Adding Rate Limiting](https://www.milanjovanovic.tech/blog/implementing-an-api-gateway-for-microservices-with-yarp?utm_source=Twitter&utm_campaign=08.01.2024&utm_medium=socail#adding-rate-limiting)

You can also use an **API gateway** to introduce **rate limiting** to your system. It's a technique to limit the number of requests to your API to **improve security** and reduce the load on the servers.

You can learn more about [how to use rate limiting in .NET here.](https://www.milanjovanovic.tech/blog/how-to-use-rate-limiting-in-aspnet-core)

As you can already guess, **YARP** supports the native **rate limiting** mechanism added in .NET 7.

All you need to do is define a **rate limit policy**:

```csharp
builder.Services.AddRateLimiter(rateLimiterOptions => { rateLimiterOptions.AddFixedWindowLimiter("fixed", options => { options.Window = TimeSpan.FromSeconds(10); options.PermitLimit = 5; }); });
```

Then you need to call `UseRateLimiter` to add the rate limiter middleware to the request pipeline. It's important to do it before calling `MapReverseProxy`.

```csharp
app.UseRateLimiter(); app.MapReverseProxy();
```

And then, you can apply rate limiting to the desired route using the `RateLimiterPolicy` section in the reverse proxy configuration:

```json
"products-route": { "ClusterId": "products-cluster", "RateLimiterPolicy": "fixed", "Match": { "Path": "/products-service/{**catch-all}" }, "Transforms": [ { "PathPattern": "{**catch-all}" } ] }
```

## [In Summary](https://www.milanjovanovic.tech/blog/implementing-an-api-gateway-for-microservices-with-yarp?utm_source=Twitter&utm_campaign=08.01.2024&utm_medium=socail#in-summary)

An **API gateway** is a critical component for a robust **microservices system** implementation.

And **YARP** is an excellent option if you want to build an **API gateway** with .NET.

I created a sample API gateway implementation with YARP, which you can find [here.](https://github.com/m-jovanovic/yarp-api-gateway-sample) The system consists of two APIs, and the API gateway is configured to route requests between them. It also implements:

-   [Authentication](https://microsoft.github.io/reverse-proxy/articles/authn-authz.html)
-   [Rate limiting](https://www.milanjovanovic.tech/blog/how-to-use-rate-limiting-in-aspnet-core)

In this newsletter, we only scratched the surface of what's possible with **YARP**.

Here are some useful resources if you want to learn more:

-   [YARP docs](https://microsoft.github.io/reverse-proxy/articles/index.html)
-   [Load balancing](https://microsoft.github.io/reverse-proxy/articles/load-balancing.html)
-   [Session affinity](https://microsoft.github.io/reverse-proxy/articles/session-affinity.html)
-   [Request/response transformations](https://microsoft.github.io/reverse-proxy/articles/transforms.html)

That's all for today.

Hope it was helpful.

**Today's action step:** Download the [source code](https://github.com/m-jovanovic/yarp-api-gateway-sample) for the sample application implementing an API gateway with YARP, and take it for a spin. You can challenge yourself by creating multiple instances of a single service and configuring load balancing with the various load balancing algorithms.

___
