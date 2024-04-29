---
created: 2024-03-05T14:01:06 (UTC -03:00)
tags: []
source: https://martinfowler.com/articles/patterns-legacy-displacement/event-interception.html
author: by Ian Cartwright, Rob Horn, and James Lewis
---

# Event Interception

> ## Excerpt
> Intercept any updates to system state and route some of them to a new
        component

---
As we look to displace a legacy system a part at a time, we look to identify, extract and replace application capabilities. To do this we will be introducing cases where both the legacy system and its replacement need to interact - be that to handle state changes, processing of commands, queries or user interactions. Often the legacy system is difficult or costly to change (addressing this challenge may be the primary reason for the displacement programme in the first place), and so we need a mechanism that can allow new capabilities provided by the new systems to be integrated, whilst minimising impacts to the legacy system.

With Event Interception, we identify existing integration points between legacy components and if possible take advantage of them as seams we can use to introduce new capabilities.

## How It Works

![legacy systems may have numerous targets for Event Interception](https://martinfowler.com/articles/patterns-legacy-displacement/legacy-endpoints.png)

Legacy systems may and usually do have numerous targets for Event Interception including message consumers, http API's, SQL connections, batch jobs and others.

We take advantage of existing integration points between legacy components, the "technical seams" above, to enable us to break apart, extract, or integrate with new application components. Event Interception is a technique that uses these technical seams, and does this by enabling events passing from one component to another to be intercepted and routed to new components/services. As we do this we are hoping to:

-   Minimise changes to legacy components - either those creating or consuming the events
-   Allow events to be routed to a new system - with or without duplication

### Messaging Infrastructure

Where messaging infrastructure is already in play, then you are in luck - one of its key advantages is that it decouples the producer from the consumer of a message.

![Messaging decouples producer from consumer](https://martinfowler.com/articles/patterns-legacy-displacement/event-interception-messaging-1.png)

Patterns like [Wire Tap](http://www.enterpriseintegrationpatterns.com/WireTap.html), [Message Router](http://www.enterpriseintegrationpatterns.com/patterns/messaging/MessageRouter.html), [Content-Based Router](http://www.enterpriseintegrationpatterns.com/ContentBasedRouter.html) etc can be used to intercept and/or create events that can be routed and handled by new systems or existing legacy systems for processing. Messaging infrastructure will typically allow message headers to be used to filter messages, making certain messages available to consumers. For example JMS implementations use Message Selectors to allow message Providers to filter messages. In this way simple interception and routing can be achieved.

![Message Routing with Message Selectors](https://martinfowler.com/articles/patterns-legacy-displacement/event-interception-messaging-3.png)

In the above figure the Legacy Consumer will need to have its producer configured to include the message selector (to effectively add a filter)

More complex routing may require the body of a message to be inspected - in this case (Content Based Routing) a message will need to be consumed by a routing component, the message payload inspected and the message re-enqueued onto new destination queues as required based on the message contents.

![Content Based Message Routing](https://martinfowler.com/articles/patterns-legacy-displacement/event-interception-messaging-2.png)

Similarly the Legacy Consumer will need to be reconfigured to consume from the new destination

**Reverse Proxy - Routing based on URLs, Query Params or Form data**  
A reverse proxy can be another very effective way of creating and/or intercepting events - letting you redirect the http(s) requests to different resources. Simlilar to messaging infrastructure - a reverse proxy will typically be able to route requests after simple inspection of host and port parts, paths, query parameters and headers.

![Reverse                                                                         Proxy                                                                         Routing                                                                         Requests](https://martinfowler.com/articles/patterns-legacy-displacement/event-interception-reverse-proxy.png)

Routing requests depending on the contents of form data for example might require a custom implementation but could enable a lot of flexibility.

### API Gateway

An API Gateway provides a level of indirection between a service's published endpoint, and its implementation. At a course-grained level an API Gateway gives us the opportunity to apply the [Branch by Abstraction](https://martinfowler.com/bliki/BranchByAbstraction.html) pattern, allowing the gateway to route all requests to an implementation of our choosing. At a finer granularity it may also be possible to route service requests as needed depending on the request or the content of a payload.

### Within a web app - Progressive Enhancement

The idea here is to make a small change to a legacy web application which will then decouple the modernisation effort's release cycle from further changes to the legacy application. To do this you can add a script element to a template (or every page in scope). That script can be developed over time (and released independently), and use progressive enhancement to intercept user actions and change behaviour as needed. For example you could use this approach to attach or overwrite an event handler to change a URL that a form submits to. Before progressive enhancement is added:

![Before using Progressive Enhancement to intercept events](https://martinfowler.com/articles/patterns-legacy-displacement/event-interception-progressive-enhancement-before.png)

And after:

![Using Progressive Enhancement to intercept events](https://martinfowler.com/articles/patterns-legacy-displacement/event-interception-progressive-enhancement-after.png)

An example where we have seen this used successfully was when replacing a legacy Product Images solution within a web storefront. We built a new system that enabled users to submit multiple images of products, and for specialist users to quality assure them. We were able to make a simple change to the legacy storefront's global template to add a script tag, the Javascript content of which could be released independently. The script used progressive enhancement within the storefront to check if the new system had images for a product, and if it did, create a carousel for those images. Displaying an advanced carousel with multple detailed images enabled an increase in sale price to be achieved.

### Database layer - Triggers

By the time changes have made their way to the legacy database, then you could argue that it is too late for event interception. That said, "Pre-commit" triggers can be used to intercept a database write event and take different actions. For example a row could be inserted into a separate Events table to be read/processed by a new component - whilst proceeding with the write as before (or aborting it). Note that significant care should be taken if you change the existing write behaviour as you may be breaking a vital implicit contract.

#### Case Study: Incremental domain extraction

One of our teams was working for a client whose legacy system had stability issues and had become difficult to maintain and slow to update.

The organisation was looking to remedy this, and it had been decided that the most appropriate way forward for them was to displace the legacy system with capabilities realised by a Service Based Architecture.

The strategy that the team adopted was to use the [Strangler Fig](https://martinfowler.com/bliki/StranglerFigApplication.html) pattern and extract domains, one at a time, until there was little to none of the original application left. Other considerations that were in play included:

-   The need to continue to use the legacy system without interruption
-   The need to continue to allow maintenance and enhancement to the legacy system (though minimising changes to domains being extracted was allowed)
-   Changes to the legacy application were to be minimised - there was an acute shortage of retained knowledge of the legacy system

#### Legacy state

The diagram below shows the architecture of the legacy architecture. The monolithic system's architecture was primarily [Presentation-Domain-Data Layers](https://martinfowler.com//bliki/PresentationDomainDataLayering.html).

![](https://martinfowler.com/articles/patterns-legacy-displacement/legacy-state.png)

#### Stage 1 - Dark launch service(s) for a single domain

Firstly the team created a set of services for a single business domain along with the capability for the data exposed by these services to stay in sync with the legacy system.

The services used [Dark Launching](https://martinfowler.com//bliki/DarkLaunching.html) - i.e. not used by any consumers, instead the services allowed the team to validate that data migration and synchronisation achieved 100% parity with the legacy datastore. Where there were issues with reconciliation checks, the team could reason about, and fix them ensuring consistency was achieved - without business impact.

The migration of historical data was achieved through a "single shot" data migration process. Whilst not strictly [Event Interception](https://martinfowler.com/articles/patterns-legacy-displacement/event-interception.html), the ongoing synchronisation was achieved using a Change Data Capture (CDC) process.

![](https://martinfowler.com/articles/patterns-legacy-displacement/extraction-stage1.png)

#### Stage 2 - Intercept all reads and redirect to the new service(s)

For stage 2 the team updated the legacy Persistence Layer to intercept and redirect all the read operations (for this domain) to retrieve the data from the new domain service(s). Write operations still utilised the legacy data store. This is and example of [Branch by Abstraction](https://martinfowler.com/bliki/BranchByAbstraction.html) - the interface of the Persistence Layer remains unchanged and a new underlying implementation put in place.

![](https://martinfowler.com/articles/patterns-legacy-displacement/extraction-stage2.png)

#### Stage 3 - Intercept all writes and redirect to the new service(s)

At stage 3 a number of changes occurred. Write operations (for the domain) were intercepted and redirected to create/update/remove data within the new domain service(s).

This change made the new domain service the System of Record for this data, as the legacy data store was no longer updated. Any downstream usage of that data, such as reports, also had to be migrated to become part of or use the new domain service.

![](https://martinfowler.com/articles/patterns-legacy-displacement/extraction-stage3.png)

#### Stage 4 - Migrate domain business rules / logic to the new service(s)

At stage 4 business logic was migrated into the new domain services (transforming them from anemic "data services" into true business services). The front end remained unchanged, and was now using a legacy facade which redirected implementation to the new domain service(s).

![](https://martinfowler.com/articles/patterns-legacy-displacement/extraction-stage4.png)

## When to Use It

If you are using the Strangler Fig pattern then you will also be using some form of Event Interception. If you are creative then you will find there are a lot of places where seams can be created using this pattern.

### Some alternative approaches / considerations

#### The human event interceptor

Assuming that an event originates from a user action, there may be an opportunity to intercept that event at source by getting the users to do something else! Re-designing the business process to fully take advantage of the modernisation efforts may mean that legacy events need no longer apply. Where they do, it may be possible to trade off a lower quality user experience and a potential reduction in operational efficiency for a period of time, against not having to create a technical seam at all. So called "Swivel chair integration", asking users to perform certain actions on a new system and/or others on the old, may provide the seam you need to make rapid progress on displacing a legacy part.

#### Database Triggers - Post-commit

"Post-commit" triggers don't intercept the write event, but can also be useful for creating events to notify other systems that a certain state change has occurred.

#### Change Data Capture (CDC)

CDC encompasses technologies that allow you to create an event stream from entries appended to a database's transaction log. For example our teams have had good experiences using Debezium to create a Kafka event stream that can be consumed by new applications. Again - not strictly interception per-ce, the event stream can be consumed by a new system being operated in parallel.

#### Routing events within Legacy

One of the aims of Event Interception is to minimise or avoid the need to update the legacy system in order to enable an integration with new systems, but an obvious alternative is when it is possible to just modify the caller to call the new system directly rather than introducing an interceptor. Some of the trade-offs to take into consideration:

-   Separation of concerns and required complexity.  
    It will be necessary for the caller to take on all of the functionality, and operability concerns that would otherwise be taken on by an interceptor. For example understanding of the contract of the new systems interface, message structures, error handling, logging, monitoring and alerting etc.
-   Avoiding additional complexity.  
    Modifying legacy will avoid the need for another piece of [Transitional Architecture](https://martinfowler.com/articles/patterns-legacy-displacement/transitional-architecture.html) to be added in the mix. This means Less to go wrong, less to operate, and reduced congnitive load.
-   Where an integration has multiple callers to a single interface, and the provider of that interface is being replaced it may be worth considering using the [Legacy Mimic](https://martinfowler.com/articles/patterns-legacy-displacement/legacy-mimic.html) pattern instead. The new component will implement the legacy interface - and as such take on the responsibility of an event interceptor without adding additional [Transitional Architecture](https://martinfowler.com/articles/patterns-legacy-displacement/transitional-architecture.html)

## Further Reading

This pattern and [Legacy Mimic](https://martinfowler.com/articles/patterns-legacy-displacement/legacy-mimic.html) are examples of how one can go about realising the much more generalised [Strangler Fig](https://martinfowler.com/bliki/StranglerFigApplication.html) pattern from Martin's initial post.

This pattern was originally published on this site by Martin Fowler in 2004 as a bliki entry, this text supersedes that description.
