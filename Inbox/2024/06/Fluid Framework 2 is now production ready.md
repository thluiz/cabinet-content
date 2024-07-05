---
created: 2024-06-28T17:10:28 (UTC -03:00)
tags: []
source: https://devblogs.microsoft.com/microsoft365dev/fluid-framework-2-is-now-production-ready/?ref=dailydev
author: Kashif Qureshi
---

# Fluid Framework 2 is now production ready

> ## Excerpt
> We are excited to announce that Fluid Framework 2 is now generally available and ready for production applications.

---
June 26th, 2024

We are excited to announce that Fluid Framework 2 is now generally available and ready for production applications! It is the result of the insights we gained from enabling real-time collaboration in Microsoft and partner applications. 

Since we first introduced Fluid Framework, it has been used extensively both within Microsoft and by external customers to build real-time collaborative experiences. With FF 2, we’re making it even easier, more flexible and intuitive to build real-time collaborative applications. 

[**Microsoft Loop**](https://aka.ms/fluid/build24/loop) – Microsoft Loop is powered by Fluid Framework and SharePoint Embedded, which is now also available for everyone to use.

[**Edge Workspaces**](https://aka.ms/fluid/build24/edge) – Workspaces in Microsoft Edge provide an incredible way for users to separate their browsing tasks into dedicated windows. Each workspace has its own set of tabs and favorites, all created and curated by collaborators in real-time with features like live presence powered by Fluid Framework.

**[Teams Live Share](https://aka.ms/fluid/build24/teams)** – Live Share is an SDK that uses Fluid Framework to transform Teams apps into collaborative multi-user experiences without writing any dedicated back-end code.

[![Screenshots shows an example of multiple users drawing on a canvas during a meeting.](https://devblogs.microsoft.com/microsoft365dev/wp-content/uploads/sites/73/2024/06/screenshots-shows-an-example-of-multiple-users-dra.png)](https://devblogs.microsoft.com/microsoft365dev/wp-content/uploads/sites/73/2024/06/screenshots-shows-an-example-of-multiple-users-dra.png)

Additionally, our industry partners like [A2MAC1](https://aka.ms/fluid/partner/a2mac1), Autodesk and [Hexagon](https://aka.ms/fluid/partner/hexagon) have been building incredible real-time collaboration experiences for their audiences.

FF 2 contains the following major updates:

1.  **SharedTree Distributed Data Structure (DDS)** – A new, powerful and flexible DDS designed to keep hierarchical data synchronized between clients.
2.  **SharePoint Embedded Support** – A new relay and storage solution that allows you to keep the Fluid collaborative data within SharePoint for a Microsoft 365 tenant.
3.  **Fluid DevTools** – A browser extension that helps developers when writing and debugging Fluid applications.
4.  **Typed Telemetry and App Insights support** – A flexible package that provides Typed telemetry events that you can funnel to your analytics tool of choice or use the included Azure App Insights connector for data visualization and analysis.

The [new SharedTree](https://fluidframework.com/docs/data-structures/tree/) Distributed Data Structure (DDS) provides an intuitive programming interface for working with data and supports a broad range of data types including primitives, objects, arrays, and maps. In addition to including standard data manipulation operations (add, remove, update, move) for these data types, it also includes features like:

**Undo/Redo** – The ability to listen for changes and track revertible objects on your undo/redo stacks. Revertibles allow you to undo and redo changes even if other changes have been made in remote clients.

**Transactions** – You can group multiple changes such that they are applied atomically, and if they fail, they fail atomically. As a result of grouping changes in a transaction, you also get a single revertible object making it easier to undo and redo.

**Events** – We have updated the Events to make it easier to create granular event listeners for single nodes and better support the undo/redo feature.

**Schema improvements** – Schemas are even more powerful now with the added support for recursive types, which allows you to define types that reference nodes of the same type in their subtree.

Find out more about SharedTree and how to use Fluid Framework to build AI-driven collaborative apps with this session video from Microsoft Build 2024:

<iframe src="//www.youtube.com/embed/fjRfTdIYzWg?si=Dhe827O3wRlIcqL4" width="560" height="314" allowfullscreen="allowfullscreen" title="Fluid Framework 2 is now production ready"></iframe>

Fluid Framework 2 offers support for a new relay service that is part of [SharePoint Embedded](https://aka.ms/fluid/spe) and allows developers to keep collaborative data within a Microsoft 365 tenant. Enterprise developers can build collaborative line of business apps while benefiting from all features of Microsoft 365 storage including security and compliance. ISVs can also leverage this option in their collaborative apps, with the data being managed within the end customer’s tenant.

<iframe src="//www.youtube.com/embed/t1ZPKR7iEJI?si=7UZlauHBRiharwh1" width="560" height="314" allowfullscreen="allowfullscreen" title="Fluid Framework 2 is now production ready"></iframe>

Fluid Developer Tools is a browser extension that improves the developer experience when writing and debugging Fluid applications. As Fluid Framework abstracts away complexities of dealing with real-time collaborative applications, it is crucial that developers have access to the underlying state.

[![A screenshot of a computer Description automatically generated](https://devblogs.microsoft.com/microsoft365dev/wp-content/uploads/sites/73/2024/06/a-screenshot-of-a-computer-description-automatica-12.png)](https://devblogs.microsoft.com/microsoft365dev/wp-content/uploads/sites/73/2024/06/a-screenshot-of-a-computer-description-automatica-12.png)

Fluid Developer Tools provide the following features:

-   Container state and data visualization
-   Ability to modify container state for testing offline and reconnect scenarios
-   Visibility into audience membership, permissions, and join/leave logs
-   Framework event logs
-   Telemetry graphs to view performance

You can learn more about Fluid DevTools [here](https://aka.ms/fluid/devtool/docs).[](https://devblogs.microsoft.com/microsoft365dev/wp-content/uploads/sites/73/2023/06/word-image-14258-2.png)

## Typed telemetry and app insights

Before deploying your application at scale, it is critical to have the holistic telemetry in place to monitor its usage and look for issues and optimizations. To make this easier, we are providing a fluid-telemetry package that comes with Typed telemetry events that you can funnel to your any analytics tool of your choice. If you decide to use Azure App Insights to view this data, we also provide helper packages and dashboard queries to get you started quickly. [Learn more about the telemetry packages here.](https://aka.ms/fluid/telemetry)

## Start building today

Fluid Framework 2 is now ready to power your production app experiences. We’re looking forward to seeing what you build with it!

Visit the following resources to learn more:

-   [Get Started](https://aka.ms/fluid/start) with Fluid Framework 2
-   [Sample app code](https://aka.ms/fluid/samples) using SharedTree DDS and SharePoint Embedded
-   For questions or feedback, [reach out via Github](https://aka.ms/fluid/discuss)
-   [Connect directly with the Fluid team](https://aka.ms/fluid/connect), we would love to hear what you are building!
-   Follow [@FluidFramework on X (Twitter)](http://twitter.com/fluidframework) to stay updated
