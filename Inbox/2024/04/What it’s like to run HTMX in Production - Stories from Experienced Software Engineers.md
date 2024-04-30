---
created: 2024-04-10T22:13:53 (UTC -03:00)
tags: []
source: https://hamy.xyz/labs/2024-04_htmx-in-production?utm_source=tldrnewsletter
author: 
---

# What it’s like to run HTMX in Production - Stories from Experienced Software Engineers

> ## Excerpt
> HTMX is changing how modern web apps get built. It provides most of the capabilities of a Single-Page Application with the simplicity of a Multi-Page Application. This means you don't really need reach for React, Vue, Angular, Svelte, etc to get a modern app - and in most cases it's simpler not to.

---
HTMX is changing how modern web apps get built. It provides most of [the capabilities of a Single-Page Application with the simplicity of a Multi-Page Application](https://hamy.xyz/labs/2024-02_htmx-for-side-projects). This means you don't really need reach for React, Vue, Angular, Svelte, etc to get a modern app - and in most cases it's simpler not to.

But all technologies make big promises and big splashes early in their hype cycle while few survive the test of running in production. In this post we're going to survey the experiences of real developers running HTMX in production to see if it works well or is just hype.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Ec_ovkHHuZ8?si=arnrg2G85rGeMLLl" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen=""></iframe>

## Overview

Developer responses were gathered from [r/ExperiencedDevs "To those that use HTMX in production, how is it?"](https://www.reddit.com/r/ExperiencedDevs/comments/1bomixd/to_those_that_use_htmx_in_production_how_is_it/)

Themes in this post were created by:

-   Sifting responses
-   Organizing by theme / idea
-   Condensing into learnings / takeaways

In the rest of this post we'll explore these themes (the good and bad), what we can learn from them, and how this might affect your HTMX tech stack decisions in the future.

![MPA vs HTMX vs SPA](https://storage.googleapis.com/iamhamy-static/labs/posts/2024/2024-04_htmx-in-production/2024-04_mpa-vs-htmx-vs-spa.png)

_MPA vs HTMX vs SPA from [HTMX for Side Projects](https://hamy.xyz/labs/2024-02_htmx-for-side-projects)_

## Good: HTMX Simplifies Frontend Code

The majority of responses praise HTMX for vastly simplifying frontend code. This allows for:

-   Faster changes
-   Easier to onboard new developers

**Frontend Simplification: Reduce layers of abstraction**

> From what I can see, HTMX strips back a lot of the layers of nonsense that was going on before. We do have some javascript, but we use it mostly for the original intent of “little bits of interactivity” that doesn’t need to touch the server, like flipping on dark mode. (lightmatter501)

I feel this strongly. A lot of the work in fullstack CRUD apps is simply piping data from DB -> BE -> FE.

-   SPA: DB -> BELogic -> JSON -> FELogic -> HTML
-   MPA/HTMX: DB -> BELogic -> HTML

The MPA route is simpler - it has 3 steps instead of 5. [HTMX gets you most of the SPA experience with the simplicity of an MPA](https://hamy.xyz/labs/2024-02_htmx-for-side-projects).

**Frontend Simplification: No JS needed**

> Most of our apps have zero JavaScript outside of the HTMX page now. In all of our apps that I've done so far, there's no need to have any JavaScript even jQuery on our pages if we can just "post back" and get the out of band updates that we need. (leathakkor)

When you really think about [what makes SPAs feel modern - it's mostly that they feel fast and that's largely due to targeted, partial page rerenders](https://hamy.xyz/labs/2024-01_htmx-modern-web-apps-faster-cheaper).

SPAs do this in all sorts of ways but generally via a heavyweight state:dom mapping. When the state changes they figure out what parts of the dom are affected and just rerender those.

HTMX allows the same thing but without the state portion so you basically get the same functionality but in a much lighter weight form. This means you don't need to worry about that intermediate clientside state / logic step and can build with much less JS.

**Faster Changes**

> I've been using it recently for some internal facing apps and I'm really happy with it. No concern for scaling due to the nature of the apps, but it's super simple to get up and running quickly. (Acceptable\_Durian868)

Because the frontend is simplified, making changes is quite fast. You can essentially just build an MPA and sprinkle in dynamic renders with HTMX where useful. Simple!

**Easier to Onboard new developers**

> I'll answer how it scales for a small team in my opinion where I work:

> I use it in my production apps at work, I will say that one of the best parts of it is that when a feature request, bug fix or enhancement comes in, Pretty much anyone can pick it up.

> I've built angular apps and react apps and unfortunately even making text changes in those systems can be too difficult for a junior developer to make the change if they've never worked in that environment before. And forget about complicated logic changes. (leathakkor)

FE code has gotten so complex that many orgs now have specialized positions - FE and BE. In practice this means that it's very hard for any one developer to get a fullstack change out.

Short-term this means that all changes incur extra cost due to context switching and collaboration overhead between FE / BE dev

Long-term this means a general architectural bias against doing things fullstack. Both sides will go to great lengths to not have to do things fullstack which often leads to suboptimal architectures throughout. This is how we got heavyweight client state management (Redux), interesting RPC protocols (GraphQL), and the push towards JS-only stacks (NextJS, Nuxt, SvelteKit, etc) - all methods to try and lower the pain of FE <> BE communication.

For most orgs they just want to build so choosing technologies that are so simple anyone can make a change likely leads to velocity and business benefits over time.

HTMX allows devs to be fullstack:

-   BE people to do FE
-   FE people to do BE

## Good: HTMX is often MORE performant than SPAs

> lets us ship 90% of our page from CDNs and then the HTMX talks to servers, unlike how JS server side works. We have a custom CDN server that is designed on top of DPDK (hackathon project), so we like serving as many chunks of static content out of that as we can, since it’s well over 10x higher throughput and ~50x lower latency (lightmatter501)

One of the big complaints against HTMX is that it won't be performant - you're using a network call for every piece of page interactivity. That is a fair point - network calls are much more expensive than local calls - but in practice this isn't really a problem.

Now if you naively use HTMX for EVERYTHING then yes it can be bad (as we'll talk ab later). But if you use it reasonably then it's mostly the same or better than SPAs.

-   **Server Interactivity** - HTMX is great. This is anything that needs data from the server.
    -   **Example:** Submitting data, sorting a data table, refreshing data
-   **Client Interactivity** - Use something else. This is anything that does not need data from the server.
    -   **Example:** light vs dark mode, toggling a pop up

_More on Server vs Client: [HTMX vs AlpineJS - Which should you use for your web app?](https://hamy.xyz/labs/2024-01_htmx-vs-alpine)_

For server interactivity HTMX does less work than clientside frameworks. Clientside frameworks need to do the same network request plus their own internal processing to render. HTMX simply does the request and plops it on the page.

The only case where HTMX does more work is for client-only changes in which case maybe don't use HTMX for that - just use JS or low-js like Alpine.

Now I'll be honest and say that this poster's stack seems to be a bit overengineered. You don't need a custom CDN to do this stuff. If you use Hypermedia / URL as the source of application state then regular caching works super well. But in most cases you won't even need it cause it'll be way faster than your SPA anyways.

## Good: Easy to add to existing projects

> I've just introduced it where I work. All our most recent projects were exclusively using static templating with Golang so HTMX was a much easier sell to add a more SPA-like user experience than having another protracted argument with the architects about the pros and cons of JS frameworks. The best thing from our perspective is it is opt-in, so we didn't need to change anything about our processes and could just adopt it as and when.

One of the hardest parts about adopting new technologies is that many are "contagious". This means that it requires lots of changes in many parts of your code to use them (i.e. async code is contagious because to get async you need async in all callers).

Going from MPA to SPA for example would require you to setup all the build steps for that particular SPA, figure out how to serve it, and then get it to talk to your existing code. Often it's easier to simply build a whole new SPA app which is why so many stacks are dual monoliths (one FE, one BE).

HTMX (and low-js in general) takes a different approach - it's just browser-based JS libraries so you can simply DL it onto a page and start using it.

This allows for gradual adoption so you can try it out with minimal costs / implications on the broader org.

(technologies with these attributes are typically easier to compose and thus more maintainable over time - library vs framework arg and all that).

## Bad: HTMX lacks a paved road

> Got an additionnal question - I do think HTMX can replace a lot of the React cases. Where I still see myself using React is for componenent librairies (Shad, MartineUI, Antd...) - I found those to be a major productivity gain in smallish team (no design system, etc). (vanakenm)

HTMX apps are very similar to MPA apps so there's a lot of tech that just works out of the box. The problem generally is that SPAs have held majority mindshare for the past decade and thus general knowledge of these techniques and modern, mainstream convenience libraries are not as prevalent as React, Vue, Angular, etc.

The good thing is all your favorite React, Vue, Angular, Svelte libs mandatorily render down to vanilla HTML / CSS cause that's what the browser understands. This means there's nothing preventing these things from coming to low-js land. But it will take some work - unwinding these things from their heavy build states.

If you want ready-to-use, heavily-customized styles then it might be more work to try and move to HTMX.

Personally I've found success with:

-   TailwindCSS for base styles, DaisyUI for base components, all bundled in my ASP.NET build step ([Building ASP.NET apps with Tailwind CSS](https://hamy.xyz/labs/2024-01_tailwind-dotnet))
-   Build my own styled components ([coarse Interactive Islands](https://hamy.xyz/labs/2023-12-fsharp-htmx))

More generally it'll take some time to get back to large MPA / HTMX mindshare in terms of convenience libraries and guides but given the sustained popularity of low-js I think it will happen sooner rather than later.

## Bad: HTMX is not a silver bullet

> The kiosks also have to operate with less than favourable network conditions. This is, like, exactly what HTMX is not designed for, but the team in question have bitten the "JavaScript bad" meme. I'm watching the project gradually disintegrate. (yojimbo\_beta)

HTMX is very good at making modern web apps simply. But it is not a silver bullet.

If you are building something that needs to work offline (aka not a web app) or that needs very low latency client-only changes (like a video game) then HTMX is probably not the best choice.

## Open Questions: Does HTMX scale for large teams / codebases?

Overall HTMX seems to make it very simple to build modern web apps. We've seen lots of success stories from small teams in small-medium codebases.

The question is whether this scales to large enterprise teams and codebases. The challenges there are often different:

-   Maintainability - testing, type checking, convenience
-   Human scale - preventing human errors, easy onboarding

_Related: [Why Type-safe Programming Languages are better than Dynamic and Lead to Faster, Safer Software at Scale](https://hamy.xyz/labs/2024-03_typesafe-vs-dynamic-programming-languages)_

My main concern with HTMX is that there's still a bit of raw string logic. We're building frontends connected to backends based on raw string url paths with logic embedded in markup via raw strings.

The load-bearing raw string url exists in all apps but the raw logic is usually constrained - this signals to me that there's a risk this stuff gets brittle when we have dozens of devs changing it each week without tests, type checks, or convenience libs to hide it.

Totally a skill issue but at scale our code approaches the lowest common skill - so smth you need to worry about.

I don't have an answer to this yet and we'll likely need to wait awhile for enterprises to onboard and run it in prod for a good 6-12 months before we hear back.

## Next

Overall I'm a huge fan of the low-js movement. I've built most of my apps in 2024 with HTMX and it's been great - simpler, faster modern web development. I plan to keep building with this architectural philosophy til a better one comes around - see: [The HAM Stack](https://hamy.xyz/labs/2024-02_hamstack).

**Q: What's your experience building and running HTMX apps in production? What are the biggest pros / cons you've seen?**

If you liked this post you might also like:

-   [Why you should choose HTMX for your next web-based side project - and ditch the crufty MPA and complex SPA](https://hamy.xyz/labs/2024-02_htmx-for-side-projects)
-   [HTMX vs AlpineJS - Which should you use for your web app?](https://hamy.xyz/labs/2024-01_htmx-vs-alpine)
-   [The HAM Stack - A Simple Scalable Tech Stack for building modern web apps fast and cheap](https://hamy.xyz/labs/2024-02_hamstack)
