---
created: 2024-04-30T06:41:03 (UTC -03:00)
tags: []
source: https://benfoster.io/blog/customize-authorization-response-aspnet-core/
author: 
---

# Customizing Authorization Responses in .NET 5.0 - Ben Foster

> ## Excerpt
> In this post we explore the new IAuthorizationMiddlewareResultHandler interface in .NET 5 that enables you to customize the HTTP response when authorization fails.

---
One feature [frequently requested](https://github.com/dotnet/aspnetcore/issues/4670) in the ASP.NET Core Authorization Framework was the ability to customize the HTTP response when authorization fails.

Previously the only way to this was to either invoke the authorization service (`IAuthorizationService`) directly in your controllers (or via a filter), similar to the approach for [resource-based authorization](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/resourcebased?view=aspnetcore-5.0) or [implement your own authorization filter](https://ignas.me/tech/custom-unauthorized-response-body/).

As of .NET 5.0 you can now customize the HTTP response, by implementing the `IAuthorizationMiddlewareResultHandler` interface; middleware that will be automatically invoked by the authorization framework when authorization fails.

It turns out this _is_ [documented](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/customizingauthorizationmiddlewareresponse?view=aspnetcore-5.0) on the Microsoft Docs site but it took me a while to find based on my particular use case.

## The Problem

I’ve been working on porting an older ASP.NET Web API application to .NET Core 5.0. This API had a hierarchical URI structure such that majority of the endpoints would sit under a “site” resource, for example:

-   `/sites`
-   `/sites/{siteId}`
-   `/sites/{siteId}/blog`

To validate that the user had access to a specified site, the application previously used a custom action filter to extract the `siteId` route parameter and validate it against the user’s claims. Moving to .NET 5.0 I wanted to leverage the authorization framework to achieve such resource-based authorization but equally did not want to duplicate this logic in every controller.

My solution was to implement an authorization handler that did a similar thing, grabbing the `siteId` parameter and validating the user’s access:

```c
public class SiteAccessAuthorizationHandler : AuthorizationHandler<SiteAccessRequirement> { private const string SiteIdRouteParameter = "siteId"; private readonly ILogger<SiteAccessAuthorizationHandler> _logger; public SiteAccessAuthorizationHandler(ILogger<SiteAccessAuthorizationHandler> logger) { _logger = logger.NotNull(nameof(logger)); } protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, SiteAccessRequirement requirement) { context.NotNull(nameof(context)); requirement.NotNull(nameof(requirement)); if (context.Resource is HttpContext httpContext && httpContext.GetRouteData().Values.TryGetValue(SiteIdRouteParameter, out object? routeValue) && routeValue is string siteId) { string qualifiedId = $"sites/{siteId}"; AccountPrincipal account = context.User.ToAccount(); _logger.LogDebug("Validating access to Site {SiteId} from User {UserId}.", qualifiedId, account.GetAuthIdentifier()); if (account.CanAccessSite(qualifiedId)) { context.Succeed(requirement); } else { _logger.LogWarning("Site validation failed. User {UserId} is not permitted to access {SiteId}.", account.GetAuthIdentifier(), qualifiedId); } } return Task.CompletedTask; } }
```

This is then registered as part of an Authorization Policy:

```c
services.AddAuthorization(options => { options.FallbackPolicy = Policies.FallbackPolicy; options.AddPolicy("SiteAccess", Policies.SiteAccessPolicy); }) public static AuthorizationPolicy SiteAccessPolicy => ConfigureDefaults(new AuthorizationPolicyBuilder()) .AddRequirements(new SiteAccessRequirement()) .Build(); private static AuthorizationPolicyBuilder ConfigureDefaults(AuthorizationPolicyBuilder builder) => builder.AddAuthenticationSchemes(JwtBearerDefaults.AuthenticationScheme) .RequireAuthenticatedUser() .RequireClaim(JwtClaimTypes.ClientId);
```

And applied to controllers and/or actions:

```c
[Authorize(Policy = "SiteAccess")] [HttpGet("{siteId}", Name = RouteNames.SiteRoute)] public async Task<IActionResult> GetSiteAsync(string siteId, CancellationToken cancellationToken) { var site = await _session.LoadAsync<CMS.Domain.Site>($"sites/{siteId}", cancellationToken); return site is null ? NotFound() : Ok(Enrich(_mapper.Map<Site>(site), true)); }
```

When I try and access a site that is not mapped to the current user I’ll receive a `HTTP 403 - Forbidden` response.

Although this achieves the goal of protecting site resources, it has the downside of leaking information about the existence of sites that the user does not have access to. It would therefore be preferable to return a `HTTP 404 - Not Found` response. Semantically this also makes sense given that the site does not exist in the _user’s_ collection of site resources.

In case you’re wondering why I don’t just make the user filter part of my query, it’s because users/accounts are separate from the content domain and due to the design of the data model and the fact that I’m using a key-value store, the responsibility of validating access shifted to the application layer.

## The Solution

To achieve the above we can make use of the new `IAuthorizationMiddlewareResultHandler` and create a handler that transforms the HTTP response whenever authorization fails due to my site access requirement not being fulfilled:

```c
public class AuthorizationResultTransformer : IAuthorizationMiddlewareResultHandler { private readonly IAuthorizationMiddlewareResultHandler _handler; public AuthorizationResultTransformer() { _handler = new AuthorizationMiddlewareResultHandler(); } public async Task HandleAsync( RequestDelegate requestDelegate, HttpContext httpContext, AuthorizationPolicy authorizationPolicy, PolicyAuthorizationResult policyAuthorizationResult) { if (policyAuthorizationResult.Forbidden && policyAuthorizationResult.AuthorizationFailure != null) { if (policyAuthorizationResult.AuthorizationFailure.FailedRequirements.Any(requirement => requirement is SiteAccessRequirement)) { httpContext.Response.StatusCode = (int)HttpStatusCode.NotFound; return; } // Other transformations here } await _handler.HandleAsync(requestDelegate, httpContext, authorizationPolicy, policyAuthorizationResult); } }
```

In the above code, I check if authorization failed (result is Forbidden) and the failed requirements, changing the HTTP status code accordingly; otherwise we fall back to the default behaviour by invoking the built-in `AuthorizationMiddlewareResultHandler`.

To wire up the custom handler, it is registered in startup:

```c
services.AddAuthorization(options => { options.FallbackPolicy = Policies.FallbackPolicy; options.AddPolicy("SiteAccess", Policies.SiteAccessPolicy); }) .AddSingleton<IAuthorizationMiddlewareResultHandler, AuthorizationResultTransformer>();
```
