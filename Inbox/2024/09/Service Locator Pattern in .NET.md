---
created: 2024-09-09T09:53:56 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: Muhammad Waseem
---

# 

> ## Excerpt
> Read Time : 3 Mins

---
## EP 64 : Service Locator Pattern in .NET

### Read Time : 3 Mins

**[Sponsor this newsletter](https://mwaseemzakir.substack.com/p/waseems-net-newsletter-sponsorship)**

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc0f4a31-5413-4845-a93f-1adb30b7e4f3_1200x600.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc0f4a31-5413-4845-a93f-1adb30b7e4f3_1200x600.png)

-   What is the service locator pattern?
    
-   When should we use it?
    
-   How to use it in .NET
    
-   Is using it a bad practice?
    
-   Disadvantages
    

### What is the service locator pattern?

We can inject any interface into the constructor and use it in dependency injection. The default dependency injection container in .NET will handle the instantiation and disposal of objects.

If, for some reason, we are unable to inject those interfaces and we need to resolve those dependencies, we can utilize the service locator pattern as a solution.

In this pattern, we inject our service provider into the class and we can retrieve the N number of services using that single provider.

### When should we use this pattern?

There are situations where this pattern could be helpful, but it should be used wisely in the following cases:

1\. When dealing with services scope mismatch issues

2\. When working with an old code base that requires extensive changes for update

3\. Sometimes when creating generic classes or maybe when types are not clear until runtime

### How to use it in .NET?

We need to inject IServiceProvider in our desired class and then retrieve the required service by calling `GetRequiredService` method

```
public class UsersService
{
    private readonly IServiceProvider _serviceProvider;

    public UsersService(IServiceProvider ServiceProvider) 
    {
        _serviceProvider = ServiceProvider;
    }
}
```

Then we can use it at the controller level or method level :

```
public class UsersService
{
    private readonly IServiceProvider _serviceProvider;
    
    private readonly IUserRolesRepository _userRolesRepository;

    public UsersService(IServiceProvider ServiceProvider) 
    {
        _serviceProvider = ServiceProvider;

       _userRolesRepository =_serviceProvider
             .GetRequiredService&lt;IUserRolesRepository&gt;();
    }

    public void Method1()
    {
        var userRepository = _serviceProvider
                .GetRequiredService&lt;IUserRepository&gt;();
    }
}
```

This is a very simple example of how we can use `IServiceProvider,` let’s move one step next.

### Using a global implementation for a service provider

Instead of putting IServiceProvider everywhere, we can use a more universal approach by using a single class responsible for the services like this :

```
public class CachedServiceProvider 
{
protected IServiceProvider ServiceProvider { get; }

protected ConcurrentDictionary&lt;Type, Lazy&lt;object&gt;&gt; Services { get; }

public CachedServiceProvider(IServiceProvider serviceProvider)
{
   ServiceProvider = serviceProvider;

   Services = new ConcurrentDictionary&lt;Type, Lazy&lt;object&gt;&gt;();

   Services
             .TryAdd(typeof(IServiceProvider), new Lazy&lt;object&gt;(() =&gt;                ServiceProvider));
}

public virtual object GetService(Type serviceType)
{
return Services.GetOrAdd(
  serviceType,
_ =&gt; new Lazy&lt;object&gt;(() =&gt;        ServiceProvider.GetService(serviceType)!)
).Value;
}
}
```

Using this approach our single class is responsible for providing services, and then we are using a dictionary to cache the services so the same service is not created again and again.

By using a concurrent dictionary we are making sure that it can accessed by multiple threads concurrently.

### Is it bad practice to use this pattern?

Going with direct dependency injection should always be the first option because that is much cleaner, manageable, and easy to unit test.

But if that is not possible then we can opt for this pattern, like every pattern this also has some cons. But we can not say that using this is bad because it depends on the situation and use case.

### Disadvantages of this pattern?

These are the disadvantages of using this pattern:

1.  The biggest issue is testing, mocking and testing can become more complex because the service locator is often a singleton or a global instance.
    
2.  It can make dependencies less obvious, as they are not directly visible in the code where services are used.
    
3.  Over-reliance on the service locator can lead to poor design choices, where everything is fetched from the locator, leading to an anemic domain model
    

### **Whenever you’re ready, there are 2 ways I can help you:**

1.  If you want to boost your .NET skills and learn more about .NET you can do that by subscribing to my **[YouTube Channel](https://www.youtube.com/channel/UCgKGlfkNAmIBprbI35C6SpQ?sub_confirmation=1)**
    
2.  **[Promote yourself to 10400+ subscribers](https://mwaseemzakir.substack.com/p/waseems-net-newsletter-sponsorship)** by sponsoring this newsletter
    

> ### _“To praise the sun is to praise your own eyes.” - Rumi_

### Subscribe to .NET Weekly Newsletter

By Muhammad Waseem · Launched 2 years ago

Boost your .NET skills to next level with a quick read under 5 minutes every Saturday

[![](https://substackcdn.com/image/fetch/w_80,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffb032b58-1e10-4f7a-a235-d8173e106943_96x96.jpeg)](https://substack.com/profile/17128357-gianluca-gagliano?utm_source=post-reactions-face)
