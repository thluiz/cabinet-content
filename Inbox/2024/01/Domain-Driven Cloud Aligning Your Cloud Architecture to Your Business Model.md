---
created: 2023-10-11T15:01:18 (UTC -03:00)
tags: [domain driven cloud,Architecture & Design,AWS,Azure,Amazon,PaaS,Teamwork,Business Models,Cloud,Agile,Domain Driven Design,Cloud Architecture,Architecture,Cloud Computing,]
source: https://www.infoq.com/articles/domain-driven-cloud/?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global&ref=dailydev
author: Ryan Shriver, Chris Belyea
---

# Domain-Driven Cloud: Aligning Your Cloud Architecture to Your Business Model

> ## Excerpt
> This article introduces Domain-Driven Cloud as an approach that brings technical and human benefits by aligning your cloud architecture to the bounded contexts in your business model.

---
### Key Takeaways

-   Domain-Driven Cloud (DDC) is an approach for creating your organization’s cloud architecture based on the bounded contexts of your business model. DDC extends the principles of Domain-Driven Design (DDD) beyond traditional software systems to create a unifying architecture approach across business domains, software systems and cloud infrastructure.
-   DDC creates a cloud architecture that evolves as your business changes, improves team autonomy and promotes low coupling between distributed workloads. DDC simplifies security, governance and cost management in a way that promotes transparency within your organization.
-   In practice, DDC aligns your bounded contexts with AWS Organizational Units (OU’s) and Azure Management Groups (MG’s). Bounded contexts are categorized as domain contexts based on your business model and supporting technical contexts. DDC gives you freedom to implement different AWS Account or Azure Subscription taxonomies while still aligning to your business model.
-   DDC uses inheritance to enforce policies and controls downward while reporting costs and compliance upwards. Using DDC makes it automatically transparent how your cloud costs align to your business model without implementing complex reports and error-prone tagging requirements.
-   DDC aligns with established AWS and Azure well-architected best practices. You can implement DDC in 5 basic steps whether a new migration (green field) or upgrading your existing cloud architecture (brown field).

Domain-Driven Cloud (DDC) is an approach for creating your organization’s cloud architecture based on your business model. DDC uses the bounded contexts of your business model as inputs and outputs a flexible cloud architecture to support all of the workloads in your organization and evolve as your business changes. DDC promotes team autonomy by giving teams the ability to innovate within guardrails. Operationally, DDC simplifies security, governance, integration and cost management in a way that promotes transparency for IT and business stakeholders alike.

Based on Domain-Driven Design (DDD) and the architecture principle of high cohesion and low coupling, this article introduces DDC including the technical and human benefits of aligning your cloud architecture to the bounded contexts in your business model. You will learn how DDC can be implemented in cloud platforms including Amazon Web Services (AWS) and Microsoft Azure while aligning with their well-architected frameworks. Using illustrative examples from one of our real customers, you will learn the 5 steps to implementing DDC in your organization.

## What is Domain-Driven Cloud (DDC)?

DDC extends the principles of DDD beyond traditional software systems to create a unifying architecture spanning business domains, software systems and cloud infrastructure.  

#### Related Sponsored Content

-   ##### [Maximizing Efficiency and Value: A Guide to Value Stream Management](https://www.infoq.com/vendorcontent/show.action?vcr=2c7deb1d-fb02-4e0a-ac8d-fd10eb7afe5a&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fdomain-driven-cloud%2F%3Futm_campaign%3Dinfoq_content%26utm_source%3Dinfoq%26utm_medium%3Dfeed%26utm_term%3Dglobal%26ref%3Ddailydev&itm_source=infoq&itm_medium=VCR&itm_campaign=vcr_articles_click&itm_content=embedded&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Architecting for Scale - Download Now (By O'Reilly)](https://www.infoq.com/vendorcontent/show.action?vcr=9cf809c2-ae1d-47fc-b9ff-220d63926174&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fdomain-driven-cloud%2F%3Futm_campaign%3Dinfoq_content%26utm_source%3Dinfoq%26utm_medium%3Dfeed%26utm_term%3Dglobal%26ref%3Ddailydev&itm_source=infoq&itm_medium=VCR&itm_campaign=vcr_articles_click&itm_content=embedded&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [BigQuery Editions and What You Need to Know](https://www.infoq.com/vendorcontent/show.action?vcr=39d892cc-6eed-41b6-a22c-7242a655fff8&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fdomain-driven-cloud%2F%3Futm_campaign%3Dinfoq_content%26utm_source%3Dinfoq%26utm_medium%3Dfeed%26utm_term%3Dglobal%26ref%3Ddailydev&itm_source=infoq&itm_medium=VCR&itm_campaign=vcr_articles_click&itm_content=embedded&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Why choose a purpose-built time series database?](https://www.infoq.com/vendorcontent/show.action?vcr=dd3fc2ea-5c64-4938-b51c-8bd27906f53c&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fdomain-driven-cloud%2F%3Futm_campaign%3Dinfoq_content%26utm_source%3Dinfoq%26utm_medium%3Dfeed%26utm_term%3Dglobal%26ref%3Ddailydev&itm_source=infoq&itm_medium=VCR&itm_campaign=vcr_articles_click&itm_content=embedded&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    
-   ##### [Cache and Message Broker for Microservices](https://www.infoq.com/vendorcontent/show.action?vcr=51a3ade8-9eab-40bb-89c4-a933d872cbad&primaryTopicId=2498&vcrPlace=EMBEDDED&pageType=ARTICLE_PAGE&vcrReferrer=https%3A%2F%2Fwww.infoq.com%2Farticles%2Fdomain-driven-cloud%2F%3Futm_campaign%3Dinfoq_content%26utm_source%3Dinfoq%26utm_medium%3Dfeed%26utm_term%3Dglobal%26ref%3Ddailydev&itm_source=infoq&itm_medium=VCR&itm_campaign=vcr_articles_click&itm_content=embedded&ha=bW91c2Vtb3Zl&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs&ha=c2Nyb2xs)
    

Our customers perpetually strive to align "people, process and technology" together so they can work in harmony to deliver business outcomes. However, in practice, this often falls down as the Business (Biz), IT Development (Dev) and IT Operations (Ops) all go to their separate corners to design solutions for complex problems that actually span all three.

What emerges is business process redesigns, enterprise architectures and cloud platform architecture all designed and implemented by different groups using different approaches and localized languages.  

What’s missing is a unified architecture approach using a shared language that integrates BizDevOps. This is where DDC steps in, with a specific focus on aligning the cloud architecture and software systems that run on them to the bounded contexts of your business model, identified using DDD. Figure 1 illustrates how DDC extends the principles of DDD to include cloud infrastructure architecture and in doing so creates a unified architecture that aligns BizDevOps.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure01-DDD-vs-DDC-1695044155990.jpg)

In DDC, the most important cloud services are AWS [Organizational Units](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) (OU’s) that contain Accounts and Azure [Management Groups](https://learn.microsoft.com/en-us/azure/governance/management-groups/overview) (MG’s) that contain Subscriptions. Because 100% of the cloud resources you secure, use and pay for are connected to Accounts and Subscriptions, these are the natural cost and security containers. By enabling management and security at the higher OU/MG level and anchoring these on the bounded contexts of your business model, you can now create a unifying architecture spanning Biz, Dev and Ops. You can do this while giving your teams flexibility in how they use Accounts and Subscriptions to meet specific requirements.

## Why align your Cloud Architecture with your Business Model?

The benefits for aligning your cloud architecture to your organization’s business model include:

-   **Evolves with your Business** - Businesses are not static and neither is your cloud architecture. As markets change and your business evolves, new contexts may emerge and others may consolidate or fade away. Some contexts that historically were strategic differentiators may drive less business value today. The direct alignment of your cloud management, security and costs to bounded contexts means your cloud architecture evolves with your business.
-   **Improves Team Autonomy** - While some cloud management tasks must be centralized, DDC recommends giving teams autonomy within their domain contexts for things like provisioning infrastructure and deploying applications. This enables _innovation within guardrails_ so your agile teams can go faster and be more responsive to changes as your business grows. It also ensures dependencies between workloads in different contexts are explicit with the goal of promoting a loosely-coupled architecture aligned to empowered teams.
-   **Promotes High Cohesion and Low Coupling** - Aligning your networks to bounded contexts enables you to explicitly allow or deny network connectivity between all contexts. This is extraordinarily powerful, especially for enforcing low coupling across the your cloud platform and avoiding a modern architecture that looks like a bowl of spaghetti. Within a context, teams and workloads ideally have high cohesion with respect to security, network integration and alignment on supporting a specific part of your business. You also have freedom to make availability and resiliency decisions at both the bounded context and workload levels.
-   **Increases Cost Transparency** \- By aligning your bounded contexts to OU’s and MG’s, all cloud resource usage, budgets and costs are precisely tracked at a granular level. Then they are automatically summarized at the bounded contexts without custom reports and nagging all your engineers to tag everything! With DDC you can look at your monthly cloud bill and know the exact cloud spend for each of your bounded contexts, enabling you to assess whether these costs are commensurate with each context’s business value. Cloud budgets and alarms can be delegated to context-aligned teams enabling them to monitor and optimize their spend while your organization has a clear top-down view of overall cloud costs.
-   **Domain-Aligned Security** - Security policies, controls, identity and access management all line up nicely with bounded contexts. Some policies and controls can be deployed across-the-board to all contexts to create a strong security baseline. From here, selected controls can be safely delegated to teams for self-management while still enforcing enterprise security standards.
-   **Repeatable with Code Templates** - Both AWS and Azure provide ways to provision new Accounts or Subscriptions consistently from a code-based blueprint. In DDC, we recommend defining one template for all domain contexts, then using this template (plus configurable input parameters) to provision and configure new OU’s and Accounts or MG’s and Subscriptions as needed. These management constructs are free (you only pay for the actual resources used within them), enabling you to build out your cloud architecture incrementally yet towards a defined future-state, without incurring additional cloud costs along the way.

DDC may not be the best approach in all situations. Alternatives such as organizing your cloud architecture by tenant/customer (SaaS) or legal entity are viable options, too.

Unfortunately, we often see customers default to organizing their cloud architecture by their current org structure, following Conway’s Law from the 1960’s. We think this is a mistake and that DDC is a better alternative for one simple reason: **your business model is more stable than your org structure**.

One of the core tenets of good architecture is that we don’t have more stable components depending on less stable components (aka the [Stable Dependencies Principle](https://devlead.io/DevTips/StableDependenciesPrinciple)). Organizations, especially large ones, like to reorganize often, making their org structure less stable than their business model. Basing your cloud architecture on your org structure means that every time you reorganize your cloud architecture is directly impacted, which may impact all the workloads running in your cloud environment. Why do this? Basing your cloud architecture on your organization’s business model enables it to evolve naturally as your business strategy evolves, as seen in Figure 2.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure02-Cloud-Architecture-Stability-1695044155990.jpg)

We recognize that, as Ruth Malan states, "If the architecture of the system and the architecture of the organization are at odds, the architecture of the organization wins". We also acknowledge there is work to do with how OU’s/MG’s and all the workloads within them best align to team boundaries and responsibilities. We think ideas like [Team Topologies](https://www.infoq.com/articles/book-review-team-topologies/) may help here.

We are seeing today’s organizations move away from siloed departmental projects within formal communications structures to cross-functional teams creating products and services that span organizational boundaries. These modern solutions run in the cloud, so we feel the time is right for evolving your enterprise architecture in a way that unifies Biz, Dev and Ops using a shared language and architecture approach.

## What about Well-Architected frameworks?

Both [AWS’s Well-Architected framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) and [Azure’s Well-Architected framework](https://azure.microsoft.com/en-us/solutions/cloud-enablement/well-architected/) provide a curated set of design principles and best practices for designing and operating systems in your cloud environments. DDC fully embraces these frameworks and at SingleStone we use these with our customers. While these frameworks provide specific recommendations and benefits for organizing your workloads into multiple Accounts or Subscriptions, managed with OU’s and MG’s, they leave it to you to figure out the best taxonomy for your organization.

DDC is opinionated on basing your cloud architecture on your bounded contexts, while being 100% compatible with models like AWS’s Separated AEO/IEO and design principles like "Perform operations as code" and "Automatically recover from failure". You can adopt DDC and apply these best practices, too. Tools such as AWS Landing Zone and Azure Landing Zones can accelerate the setup of your cloud architecture while also being domain-driven.

## 5 Steps for Implementing Domain-Driven Cloud

Do you think a unified architecture using a shared language across BizDevOps might benefit your organization? While a comprehensive list of all tasks is beyond the scope of this article, here are the five basic steps you can follow, with illustrations from one of our customers who recently migrated to Azure.

### Step 1: Start with Bounded Contexts

The starting point for implementing DDC is a set of bounded contexts that describes your business model. The steps to identify your bounded contexts are not covered here, but the process described in [Domain-Driven Discovery](https://www.infoq.com/articles/architecture-modernization-domain-driven-discovery/) is one approach.

Once you identify your bounded contexts, organize them into two groups:

-   **Domain contexts** are directly aligned to your business model.
-   **Technical contexts** support all domain contexts with shared infrastructure and services

To illustrate, let’s look at our customer who is a medical supply company. Their domain and technical contexts are shown in Figure 3.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure03-Customer-Bounded-Contexts-1695044155990.jpg)

Your organization’s domain contexts would be different, of course.

For technical contexts, the number will depend on factors including your organization’s industry, complexity, regulatory and security requirements. A Fortune 100 financial services firm will have more technical contexts than a new media start-up. With that said, as a starting point DDC recommends six technical contexts for supporting all your systems and data.

-   **Cloud Management** \- Context for the configuration and management of your cloud platform including OU/MG’s, Accounts/Subscriptions, cloud budgets and cloud controls.
-   **Security** \- Context for identity and access management, secrets management and other shared security services used by any workload.
-   **Network** \- Context for all centralized networking services including subnets, firewalls, traffic management and on-premise network connectivity.
-   **Compliance** \- Context for any compliance-related services and data storage that supports regulatory, audit and forensic activities.
-   **Platform Services** - Context for common development and operations services including CI/CD, package management, observability, logging, compute and storage.
-   **Analytics** \- Context for enterprise data warehouses, governance, reporting and dashboards.

You don’t have to create these all up-front, start with Cloud Management initially and build out as-needed.

### Step 2: Build a Solid Foundation

WIth your bounded contexts defined, it’s now time to build a secure cloud foundation for supporting your organization’s workloads today and in the future. In our experience, we have found it is helpful to organize your cloud capabilities into three layers based on how they support your workloads. For our medical supply customer, Figure 4 shows their contexts aligned to Application, Platform and Foundation layers of their cloud architecture.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure04-Cloud-Architecture-Layers-1695044155990.jpg)

With DDC, you align AWS Organizational Units (OU’s) or Azure Management Groups (MG’s) to bounded contexts. By align, we mean you name them after your bounded contexts. These are the highest levels of management and through the use of inheritance they give you the ability to standardize controls and settings across your entire cloud architecture.

DDC gives you flexibility in how best to organize your Accounts and Subscription taxonomy, from coarse-grained to fine-grained, as seen in Figure 5.

DDC recommends starting with one OU/MG and at least two Accounts/Subscriptions per bounded context. If your organization has higher workload isolation requirements, DDC can support this too, as seen in Figure 5.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure05-Account-Subscription-Taxonomies-1695044155990.jpg)

For our customer who had a small cloud team new to Azure, separate Subscriptions for Prod and NonProd for each context made sense as a starting point, as shown in Figure 6.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure06-Azure-MGs-and-Subscriptions-1695044155990.jpg)

Figure 7 shows what this would look like in AWS.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure07-AWS-OUs-and-Accounts-1695044155990.jpg)

For our customer, further environments like Dev, Test and Stage could be created within their respective Prod and Non-Prod Subscriptions. This provides them isolation between environments with the ability to configure environment-specific settings at the Subscription or lower levels. They also decided to build just the Prod Subscriptions for the six technical contexts to keep it simple to start. Again, if your organization wanted to create separate Accounts or Subscriptions for every workload environment, this can be done too and still aligned with DDC.

From a governance perspective, in DDC we recommend domain contexts inherit security controls and configurations from technical contexts. Creating a strong security posture in your technical contexts enables all your workloads that run in domain contexts to inherit this security by default. Domain contexts can then override selected controls and settings on a case-by-case basis balancing team autonomy and flexibility with required security guardrails.

Using DDC, your organization can grant autonomy to teams to enable innovation within guardrails. Leveraging [key concepts from team topologies](https://teamtopologies.com/key-concepts), stream-aligned teams can be self-sufficient within domain contexts when creating cloud infrastructure, deploying releases and monitoring their workloads. Platform teams, primarily working in technical contexts, can focus on designing and running highly-available services used by the stream-aligned teams. These teams work together to create the right balance between centralization and decentralization of cloud controls to meet your organization’s security and risk requirements, as shown in Figure 8.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure08-Security-and-Compliance-1695044155990.jpg)

As this figure shows, policies and controls defined at higher level OU’s/MG’s are enforced downwards while costs and compliance are reported upwards. For our medical supply customer, this means their monthly Azure bill is automatically itemized by their bounded contexts with summarized cloud costs for Orders, Distributors and Payers to name a few.

This makes it easy for their CTO to share cloud costs with their business counterparts and establish realistic budgets that can be monitored over time. Just like costs, policy compliance across all contexts can be reported upwards with evidence stored in the Compliance technical context for auditing or forensic purposes. Services such as Azure Policy and AWS Audit Manager are helpful for continually maintaining compliance across your cloud environments by organizing your policies and controls in one place for management.

### Step 3: Align Workloads to Bounded Contexts

With a solid foundation and our bounded contexts identified, the next step is to align your workloads to the bounded contexts. Identifying all the workloads that will run in your cloud environment is often done during a cloud migration discovery, aided in part by a change management database (CMDB) that contains your organization’s portfolio of applications.

When aligning workloads to bounded contexts we prefer a workshop approach that promotes discussion and collaboration. In our experience this makes DDC understandable and relatable by the teams involved in migration. Because teams must develop and support these workloads, the workshop also highlights where organizational structures may align (or not) to bounded contexts. This workshop (or a follow-up one) can also identify which applications should be independently deployable and how the team’s ownership boundaries map to bounded contexts.

For our medical supply customer, this workshop revealed the permissions required for a shared CI/CD tool in the Shared Services context was needed to deploy a new version of their Order Management system in the Orders context. This drove a discussion on working out how secrets and permissions would be managed across contexts, identifying new capabilities needed for secrets management that were prioritized during cloud migration. By creating a reusable solution that worked for all future workloads in domain contexts, the cloud team created a new capability that improved the speed of future migrations.

Figure 9 summarizes how our customer aligned their workloads to bounded contexts, which are aligned to their Azure Management Groups.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure09-Apps-aligned-to-Contexts-1695044155990.jpg)

Within the Order context, our customer used Azure Resource Groups for independently deployable applications or services that contain Azure Resources, as shown in Figure 10.

\[Click on the image to view full-size\]

![](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/domain-driven-cloud/en/resources/1Figure10-Order-Bounded-Contexts-1695044155990.jpg)

This design served as a starting point for their initial migration of applications running in a data center to Azure. Over the next few years their goal was to re-factor these applications into multiple independent micro-services. When this time came, they could incrementally do this an application at a time by creating additional Resource Groups for each service.

If our customer were using AWS, Figure 10 would look very similar but use Organizational Units, Accounts and AWS Stacks for organizing independently deployable applications or services that contained resources. One difference in cloud providers is that AWS allows nested stacks (stacks within stacks) whereas Azure Resource Groups cannot be nested.

For networking, in order for workloads running in domain contexts to access shared services in technical contexts, their networks must be connected or permissions explicitly enabled to allow access. While the Network technical context contains centralized networking services, by default each Account or Subscription aligned to a domain context will have its own private network containing subnets that are independently created, maintained and used by the workloads running inside them.

Depending on the total number of Accounts or Subscriptions, this may be desired or it may be too many separate networks to manage (each potentially has their own IP range). Alternatively, core networks can be defined in the Network Context and shared to specific domain or technical contexts thereby avoiding every context having its own private network. The details of cloud networking are beyond the scope of this article but DDC enables multiple networking options while still aligning your cloud architecture to your business model. Bottom line: you don’t have to sacrifice network security to adopt DDC.

### Step 4: Migrate Workloads

Now that we have identified where each workload will run, it was time to begin moving them into the right Account or Subscription. While this was a new migration for our customer (greenfield), for your organization this may involve re-architecting your existing cloud platform (brownfield). Migrating a portfolio of workloads to AWS or Azure and the steps for architecting your cloud platform is beyond the scope of this article, but with respect to DDC this is a checklist of the key things to keep in mind:

-   Name your AWS Organizational Units (OU’s) or Azure Management Groups (MG’s) after your bounded contexts.
-   Organize your contexts into domain and technical groupings, with:
    -   Technical contexts as the foundation and platform layers of your cloud architecture.
    -   Domain contexts as the application layer of your cloud architecture.
-   Centralize common controls in technical contexts for a strong security posture.
-   Decentralize selected controls in domain contexts to promote team autonomy, speed and agility.
-   Use inheritance within OU’s or MG’s for enforcing policies and controls downward while reporting cost and compliance upwards.
-   Decide on your Account / Subscription taxonomy within the OU’s / MG’s, balancing workload isolation with management complexity.
-   Decide how your networks will map to domain and technical contexts, balancing centralization versus decentralization.
-   Create domain contexts templates for consistency and use these when provisioning new Accounts / Subscriptions.

For brownfield deployments of DDC that are starting with an existing cloud architecture, the basic recipe is:

1.  Create new OU’s / MG’s named after your bounded contexts. For a period of time these will live side-by-side with your existing OU’s / MG’s and should have no impact on current operations.
2.  Implement policies and controls within the new OU’s / MG’s for your technical contexts, using inheritance as appropriate.
3.  Create a common code template for all domain contexts that inherits policies and controls from your technical contexts. Use parameters for anything that’s different between contexts.
4.  Based on the output of your workloads mapping workshop, for each workload either:
    -   a.  Create a new Account / Subscription using the common template, aligned with your desired account taxonomy, for holding the workload or
    -   b.  Migrate an existing Account / Subscription, including all workloads and resources within the, to the new OU / MG. When migrating, pay careful attention to controls from the originating OU / MG to ensure they are also enabled in the target OU / MG.
5.  The order you move workloads will be driven by the dependencies between your workloads, so this should be understood before beginning. The same goes for shared services that workloads depend on.
6.  Depending on the number of workloads to migrate, this may take weeks or months (but hopefully not years). Work methodically as you migrate workloads, verifying that controls, costs and compliance are working correctly for each context.
7.  Once done, decommission the old OU / MG structure and any Accounts / Subscriptions no longer in use.

### Step 5: Inspect and Adapt

Your cloud architecture is not a static artifact, the design will continue to evolve over time as your business changes and new technologies emerge. New bounded contexts will appear that require changes to your cloud platform. Ideally much of this work is codified and automated, but in all likelihood you will still have some manual steps involved as your bounded contexts evolve.

Your Account / Subscription taxonomy may change over time too, starting with fewer to simplify initial management and growing as your teams and processes mature. The responsibility boundaries of teams and how these align to bounded contexts will also mature over time. Methods like GitOps work nicely alongside DDC to keep your cloud infrastructure flexible and extensible over time and continually aligned with your business model.

## Conclusion

DDC extends the principles of DDD beyond traditional software systems to create a unifying architecture spanning business domains, software systems and cloud infrastructure (BizDevOps). DDC is based on the software architecture principle of high cohesion and low coupling that is used when designing complex distributed systems, like your AWS and Azure environments. Employing the transparency and shared language benefits of DDD when creating your organization’s cloud architecture results in a secure-yet-flexible platform that naturally evolves as your business changes over time.

_Special thanks to John Chapin, Casey Lee, Brandon Linton and Nick Tune for feedback on early drafts of this article and Abby Franks for the images. High-resolution images for this article are available [here](https://www.singlestoneconsulting.com/domain-driven-cloud)._

## About the Authors

[![](https://cdn.infoq.com/statics_s1_20231003070353/images/profiles/dbeb1926e5207479fa2962ba2c3bafe7.jpg)](https://www.infoq.com/profile/Ryan-Shriver/)

#### **Ryan Shriver**

Ryan Shriver is the chief technology officer of SingleStone, a tech consulting firm that takes a human-centered approach to problem solving. At SingleStone, he leads the design and engineering teams working on architecture modernizations with clients ranging from start-ups to Fortune 100 companies. Shriver also teaches Human-Centered Design at VCU’s School of the Arts, where he helps students apply Design Thinking and Agile practices to solve real-world problems. Shriver lives and works in Richmond, Virginia.

Show moreShow less

[![](https://cdn.infoq.com/statics_s1_20231003070353/images/profiles/bEYeHyADcwQQ4NPfL3ZBNtFYLSAKxYfI.jpg)](https://www.infoq.com/profile/Chris-Belyea/)

#### **Chris Belyea**

Chris Belyea is SingleStone’s Director of Cloud Engineering and Principal Architect. Chris helps his clients build cloud-native software efficiently and securely, designing architectures for new cloud environments and applications, and leading their implementation.

Show moreShow less
