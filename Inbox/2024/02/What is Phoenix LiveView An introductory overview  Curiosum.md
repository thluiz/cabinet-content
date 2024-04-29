---
created: 2024-01-18T10:08:36 (UTC -03:00)
tags: []
source: https://curiosum.com/blog/phoenix-liveview-overview
author: Michał Buszkiewicz
---

# What is Phoenix LiveView? An introductory overview | Curiosum

> ## Excerpt
> Offering a unique approach to interactive and real-time web development, Phoenix LiveView emerges as an interesting technological choice.

---
Offering a unique approach to interactive and real-time web development, Phoenix LiveView emerges as an interesting technological choice. Learn more about what makes Phoenix Framework's younger sibling stand out and how it compares against other contemporary counterparts.

Table of contents

1.  [What Phoenix LiveView is](https://curiosum.com/blog/phoenix-liveview-overview#what-phoenix-liveview-is)
2.  [What Phoenix LiveView is not](https://curiosum.com/blog/phoenix-liveview-overview#what-phoenix-liveview-is%C2%A0not)
3.  [What makes LiveView unique?](https://curiosum.com/blog/phoenix-liveview-overview#what-makes-liveview-unique)
4.  [Phoenix Framework and Phoenix LiveView](https://curiosum.com/blog/phoenix-liveview-overview#phoenix-framework-and-phoenix-liveview)
5.  [Structuring Phoenix LiveView applications](https://curiosum.com/blog/phoenix-liveview-overview#structuring-phoenix-liveview-applications)
6.  [How Phoenix LiveView works](https://curiosum.com/blog/phoenix-liveview-overview#how-phoenix-liveview-works)
7.  [Phoenix LiveView compared to...](https://curiosum.com/blog/phoenix-liveview-overview#phoenix-liveview-compared-to)
8.  [When to consider using Phoenix LiveView?](https://curiosum.com/blog/phoenix-liveview-overview#when-to-consider-using-phoenix-liveview)
9.  [Conclusion](https://curiosum.com/blog/phoenix-liveview-overview#conclusion)

## What Phoenix LiveView is

Phoenix LiveView, a member of the Phoenix ecosystem - Elixir's major web framework led by Chris McCord - is a web development tool that allows programmers to write reactive applications with little to no separate frontend codebase.

Thanks to the potent Erlang VM (BEAM) backing Elixir, known to handle hundred-thousands or even millions of WebSocket connections handled by multiple processes concurrently, LiveView scales amazingly well - using WebSockets to send tiny DOM diffs of server-rendered HTML over the wire, computed thanks to server-side change tracking.

LiveView also handles such aspects of the application as templates, live form validation and error presentation, real-time file uploads, browser API event integrations, optimistic updates, latency evaluation, support to write tests, and error handling.

All of this contributes to a smooth dev experience and provides the possibility to use a thin, Elixir-centric tech stack for programming Phoenix applications in a live and reactive way. It looks like magic at times, but it's just the pure power of Elixir.

## What Phoenix LiveView is _not_

Indeed, LiveView is not an answer to all web application development problems, and here are just a few examples that come to my mind.

-   LiveView does not _replace_ Phoenix Framework itself; on the contrary, both are complementary to each other, with LiveView relying on Phoenix for the purpose of mounting live views in pages served via Phoenix.
-   LiveView is _not_ a JavaScript replacement in that it does not **_run in the browser_** - it runs on the server with a companion JS library on the frontend that does the magic. It vastly reduces the amount of JavaScript code that needs to be written in Phoenix web apps, but JavaScript interoperability and plugging in new JS code is possible and - in rare cases - necessary.
-   LiveView is _not_ a React replacement, either. While we'll discuss some parallels between LiveView Components and React later in this article, they operate on different levels and serve vastly different purposes.
-   LiveView is not a mobile application framework. It can power responsive, mobile-friendly web apps and is the basis for the upcoming [LiveView Native](https://native.live/) that bridges it to native mobile components, but - in itself - it is not a competitor to React Native, Flutter, or native iOS or Android development stacks.

## What makes LiveView unique?

According to Jose Valim, the creator of Elixir, one of LiveView's unique traits is its "integration between server and client, allowing it to drastically optimize both latency and bandwidth, leading to user experiences that are faster and smoother than any other client-server combo out there."

### A unified codebase

LiveView brings application frontend code closer to the application's business domain without too tight coupling - most code can be written in plain Elixir and Elixir-flavoured HTML (HEEx) without the need for large portions of JavaScript code. JS still plays a role, but most of the code for interfacing between client and server is already bundled and provided by the LiveView core team. With Tailwind, most styling work can also be done inside the HTML templates, so there's less context-switching involved. It only takes a Phoenix developer for a full-stack role.

Frontend and backend being closer to each other vastly reduces the time to deliver in many use cases. JavaScript coding is reduced to handling concerns such as drag-and-drop or custom application event handling, as well as interoperability with existing JS libraries you might want to use in the app for some purpose.

### Server-side state

Each live view being viewed by a specific user (or browser) is backed by a single Erlang VM process. These processes are very cheap to manage and resource-saving, so LiveView takes advantage of them and lets the state be contained in each of these processes.

Pretty recently, the mechanisms behind state management have been optimized with the addition of Streams, which - per the documentation - is a "mechanism managing large collections on the client without keeping the resources on the server."

### Beyond HTTP

Once a live view is rendered, HTTP and REST APIs go out of the equation. Updates are transported between the browser and the server on top of Phoenix Channels, a technology that leverages WebSockets. Elixir's usage of the Erlang VM and its lightweight process (actor) model let channels and live views operate in very large numbers concurrently on a single node because these processes are blazingly quick to start and very light on consumed resources.

## Phoenix Framework and Phoenix LiveView

LiveView is built on top of Phoenix, which takes the classical concept of an MVC framework and brings it to the world of Elixir programming language and functional programming.

In Phoenix projects, live views can be either served straight through the router pipeline or embedded into static Phoenix pages so they can be used where specifically needed.

Whereas in Phoenix, each HTTP request handled via plug pipelines is handled by a distinct Erlang VM process, in LiveView, each process backs a single WebSocket connection (encapsulated with Phoenix Channels) between the server and a client viewing the live view.

Each stateful process manages the view's state and handles events from the client as well as from other processes, and broadcasting updates to other processes (and then clients) is easy thanks to Phoenix's built-in features, such as Phoenix Presence and [Phoenix PubSub](https://curiosum.com/blog/elixir-phoenix-liveview-messenger-part-3). Each event handler is a plain-Elixir function that takes client data and returns the new state of a live view module.

[![Book a Phoenix LiveView consulting meeting](https://curiosum.com/images/blog-post/book-a-phoenix-liveview-consulting-meeting?size=original "Book a Phoenix LiveView consulting meeting")](https://calendly.com/d/yqy-v59-b5v/app-development-consultation)

## Structuring Phoenix LiveView applications

The basis for a LiveView app structure is provided by Phoenix, which introduces routing, HEEx (HTML + Embedded Elixir) templates, and their compile-time checking, views, and controllers, usually also backed with data coming from Ecto, the main SQL database wrapper for Elixir, encapsulating data structures in schema modules and providing changeset function mechanisms to manage validations.

### Building blocks

LiveView adds to this a number of templates for implementing other modules serving as structural blocks for the app:

-   `Phoenix.LiveView` provides the means to declare a live view's appearance and behaviour, consisting of a render function providing the live view's template and callbacks implementing the live view's event handling and state management.
-   `Phoenix.Component` represents a stateless or function component, defined in a reusable way to help structure code. Its render function returns the component's template, just like in a LiveView module.
-   `Phoenix.LiveComponent` represents a stateful component, compartmentalizing state, markup, and events. A live component has its own lifecycle and can handle its own events but is rendered as part of (and inside an Erlang process of) a LiveView.

### What are Phoenix Components?

Components and live components are the most important building blocks that help LiveView developers structure application code, with function components typically preferred over stateful ones unless explicitly needed for cases where component output depends on changing data (forms, counters, etc.). This comes across as a kind of parallel to React, a JavaScript framework that is also centered around components, which used to be strictly stateful or stateless back in the day, though the introduction of hooks has made the distinction more fuzzy. Learn more about Phoenix Components usage in [our article](https://curiosum.com/blog/phoenix-component).

## How Phoenix LiveView works

In very simple terms, LiveView renders pages, keeps on the server the state of what a particular user sees on the page (e.g., a dashboard or a list of items), pushes updates to the browser every time a state change should affect the view (e.g. when a counter is incremented, then an update of a visual element displaying its value is pushed to the browser), and receives and handles events from the browser where programmed to do so (e.g. when a form is updated).

Below is a short elaboration on the way LiveView works. We're sure the official documentation is a good source for further reading.

### Routing

As stated before, processing starts inside the Phoenix router pipeline, which routes HTTP requests to a specific Phoenix controller module (in which case, the view may delegate rendering to a live view module inside its template) or directly to a specific live view module.

### Lifecycle of live views

Upon the first render of a live view, it is mounted (using a callback setting it up based on params and current session) and then - after a stage of handling browser query params - rendered (using a template defined by the render function) as a regular static HTML response - this is very good for SEO purposes since the page is initially fully rendered on the server.

From this point on, HTTP is completely thrown away in favour of a WebSocket connection between the client and the live view's backing Erlang VM process. Inside the process, mounting happens once again, and rendering happens whenever change tracking detects the need to update the DOM, and changes are pushed via the WebSocket connection running on Phoenix Channels. The client can then push events to the live view's backing process, which can result in further state changes and re-renders.

Each lifecycle event can be hooked to using the attach\_hook function, allowing developers to provide reusable code serving purposes common throughout the codebase. For instance, Curiosum's [Permit open-source authorization library](https://curiosum.com/portfolio/permit) hooks into mount and handle\_params to check for a user's authorization to perform a given action.

### Disconnection and persistence

When the user leaves the page or the connection is lost (this distinction is handled with heartbeat mechanisms common to distributed systems), the Elixir process backing the live view is terminated, and the state is discarded. After reconnecting, a new process is always started, so restoring the state of the page after reconnecting is typically done by loading records in the mount and handle\_params lifecycle callbacks (usually using context functions), using a persistent source such as Ecto; in the case of forms, LiveView provides an automatic mechanism to restore their editing state, or in very peculiar cases using a JavaScript hook, one of the rare situations where JS code needs to be written.

## Phoenix LiveView compared to...

We'll take, for example, a number of other tools out there and outline similarities and differences between them and LiveView. Examples taken may or may not be related to Elixir and sometimes intentionally not analogous to LiveView. The choice represents a set of tools that you might want to compare to LiveView for a variety of reasons when deciding on your project's technological stack.

### Phoenix Framework

As noted before, Phoenix Framework is an MVC framework focusing on providing means to serve content via HTTP request-response cycle while also providing means for real-time communication mechanisms such as Phoenix Channels - which serves as a foundation for LiveView's WebSocket communication (though a long-polling transport is available as a fallback).

It is, therefore, notable that both "vanilla" Phoenix and LiveView can coexist, and most contemporary Phoenix apps are a mix of the Plug-based, purely HTTP-driven modules and the HTTP + WebSocket-based LiveView approach where suitable.

Phoenix Framework itself is relatively easy to grasp when making a transition from MVC frameworks in different languages such as Ruby on Rails and Django - with Elixir's functional parading meaning that an ORM is replaced with Ecto and its Repository pattern. In contrast, LiveView will be a new idea to most users of those, perhaps except for Hotwire, a quite similar concept heavily promoted in modern Rails.

To learn more about Phoenix, head to [our space dedicated to Phoenix-related resources](https://curiosum.com/blog?search=phoenix) such as articles and meetup recordings.

### Ruby on Rails, Django, and other frameworks

When putting Phoenix LiveView up against Rails or Django, apart from the obvious differences between their respective languages, it is important to note that these are MVC frameworks, and LiveView is a server-side live rendering library, which is an important difference to understand business-wise when deciding on the technological stack for your project. Ruby on Rails and Django makes more sense to be compared against Phoenix Framework specifically, with LiveView being more analogous to Rails' close ally, Hotwire.

Programming Phoenix Framework itself is a similarly smooth development experience for developers compared to the other frameworks, arguably even better so in the long term, thanks to Elixir's functional programming paradigm, including pattern matching, immutability, and other convenient traits. At the same time, Phoenix wins in terms of scalability thanks to Erlang VM's concurrency model, and LiveView can be an important decisive factor because the tech stack reduction means that less manpower and time will be needed to build features powering your new project.

### Hotwire

Hotwire has become Ruby on Rails' default frontend framework and is often cited as an example of a library analogous to LiveView in terms of purpose. Hotwire, itself a combination of Turbo and Stimulus, is framework-agnostic, which is one key difference compared to LiveView, which is specifically coupled (and depends on) Phoenix, so it cannot be used outside Elixir.

While also being an "HTML-over-the-wire" library that streams partial page updates to the client, its approach to state management is different; it's stateless in contrast to the stateful (i.e., maintaining its state on the server) Phoenix LiveView. Statelessness means that the state of components needs to be maintained in the browser client, which introduces additional communication overhead.

Additionally, when paired with Ruby on Rails, it does not get close to the ease of leveraging high concurrency levels that Elixir is known for, so you might end up asking yourself questions at the stage at which you want to scale.

### React

There are parallels between React and Phoenix LiveView in the component-based structure, and indeed, similarly to the presence of a mobile app framework in the form of React Native, there's also LiveView Native in the works, aiming to bridge the gap between backend and mobile. Both frameworks render HTML components into the page, although whereas React maintains a virtual DOM, LiveView only tracks variable locations in the template and pushes tiny diffs to the client.

React is a frontend JS framework (although it also optionally supports server-side rendering to an extent), and LiveView is Elixir-based. As a consequence of this, React will work great in teams with specialist frontend and backend developers and projects that include a REST or GraphQL API, which React (or React Native) consumes on the frontend. On a side note, Phoenix Framework is great for programming backends for React apps, too.

LiveView, on the other hand, allows maintaining a slimmer, Elixir-centric technological stack and a smaller, more unified team working on a pure Phoenix project without a hefty JavaScript codebase while brilliantly handling real-time and highly concurrent communication.

[![Check our Phoenix LiveView consulting services](https://curiosum.com/images/blog-post/check-our-phoenix-liveview-consulting-services?size=original "Check our Phoenix LiveView consulting services")](https://curiosum.com/services/elixir-phoenix-development)

## When to consider using Phoenix LiveView?

Programming modern web apps can be dauntingly complex, and Phoenix LiveView is a good choice when your intention is to build your product using a technology typified by fast time-to-deliver at an MVP stage, as well as a perspective of seamless scaling when your business needs it.

### Common use cases

Examples of applications likely to benefit from the usage of LiveView include those that are dashboard-heavy, as real-time updating of data is trivial to implement; also, index pages with interactions such as filtering, sorting, and pagination, or perhaps live updates, are a good use case for implementation with LiveView.

It also fares extremely well with forms, ranging from basic ones that need little more than validation to interactive, wizard-like forms and real-time communication features such as chats, chatbots, and support contact widgets; perhaps less so with applications whose offline capabilities are critical to their operation.

### Synergy with Phoenix Framework and Phoenix Channels

Even if the mentioned use cases are not dominant in your app, the project can be written using Phoenix Framework with LiveView kicking in where applicable. If your application is an offline-first concept, such as in the case of many Progressive Web Apps on the market, but you still would like to use Elixir and incorporate real-time updates into some parts of the app, you could add custom connection loss routines via JavaScript code or use Phoenix Channels for client-server communication along with your own JS code to ensure the desired level of interactivity when offline.

### Personnel considerations

In terms of team composition, LiveView has allowed us to build teams where a full-stack developer does not necessarily mean someone fluent in both Elixir and Phoenix and a whole slew of JavaScript frontend frameworks - the latter can be taken out of the equation, which allows a single developer with a common sense and basic styling skills to deal with most frontend work in the project. We have developed LiveView-based software and provided LiveView consulting in several projects such as [Skills Marketplace](https://curiosum.com/portfolio/skills-marketplace), [WeBill](https://curiosum.com/portfolio/webill), [Formalerte](https://curiosum.com/portfolio/formalerte), and [Kanta](https://curiosum.com/portfolio/kanta) to the continued satisfaction of their respective users and owners.

### Consciously choosing Elixir

LiveView is backed by the Elixir programming language, which is known to be loved by programmers according to yearly StackOverflow surveys. This is an important fact to consider when thinking of long-term commitment from the team.

Moreover, Curiosum's Elixir Survey 2023 points out that 9 out of 10 Elixir users rate the ecosystem's documentation and community support highly, which is an important point when considering a long-term investment in technology. The survey also indicates that the vast majority of Elixir adopters have experienced an improvement in their time-to-market metrics thanks to the adoption of the technology. Numerous [SaaS businesses](https://curiosum.com/blog/saas-using-elixir-and-phoenix), [startups](https://curiosum.com/blog/12-startups-using-elixir-language-in-production), and [top companies](https://curiosum.com/blog/adoption-of-elixir-by-top-companies) alike have adopted Elixir to their success.

## Conclusion

LiveView has been around since 2019, and over time, it has gained a reputation for being a great tool for programming reactive UIs in Elixir, Phoenix, and little or no JavaScript code, and there are plenty of production usage examples of the framework. It evolves quickly but is mature enough to be considered a safe choice for programming large-scale applications for a variety of purposes.

While some parallels can be drawn between LiveView and a number of vastly different tools such as React or Hotwire, LiveView still appears to be unique in its usage of Elixir language features, particularly its strong concurrency capabilities allowing for state management on the server.

If you think building a LiveView-powered app might be a good choice for your business project, [contact us for consultation](https://curiosum.com/contact) - our team of Elixir, Phoenix, and LiveView experts will help you make the right choice and build your software.
