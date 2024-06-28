---
created: 2024-06-27T17:12:45 (UTC -03:00)
tags: []
source: https://favtutor.com/articles/whatsapp-discord-and-the-secret-to-handling-millions-of-concurrent-users/?ref=dailydev
author: 
---

# WhatsApp, Discord, and the Secret to Handling Millions of Concurrent Users

> ## Excerpt
> Learn how WhatsApp and Discord use Erlang VM and Elixir to handle millions of concurrent users for seamless real-time messaging, and discover how to apply this tech to your own apps.

---
As a solo full-stack developer, one of the most popular projects our clients ask us to build is a real-time application. Oh, you know, the ones that you use every day: WhatsApp, Discord, Slack, etc. When building real-time applications, you need to consider various factors like scalability, fault tolerance, responsiveness, and distribution. And that’s a tough job, especially when you have a small (or solo) team.

But what if I told you…that you could build real-time applications that **can scale to more than 1 million users with just a few developers**? And not only that, but you could also deploy them with almost zero downtime at extremely low costs. This secret weapon I am talking about is the “**Erlang Virtual Machine**“, or BEAM (Bogdan/Björn’s Erlang Abstract Machine).

In this article, I’ll explain how Erlang and its modern counterpart, Elixir, have become the **secret weapons behind real-time applications** like WhatsApp and Discord. I am especially excited to share this because I have been using Elixir for more than 3 years now, and I can say that it’s one of the best decisions I’ve ever made in my career.  

Without further ado, let’s dive in.

## **The Origins of Erlang and Elixir**

![The Origins of Erlang and Elixir Meme](https://favtutor.com/articles/wp-content/uploads/2024/06/Erlang-meme.jpg)

**Erlang** was developed in the late 1980s by Ericsson, a Swedish telecommunications company. The goal was to create a language that could handle **massive concurrency and high availability** requirements of telecommunication systems. Erlang’s design focuses on lightweight processes, message passing, and fault tolerance, making it ideal for building systems that require continuous operation and high reliability.

Erlang’s **lightweight processes** allow for thousands or even millions of concurrent activities within the same system without significant performance degradation. Its message-passing model enables these processes to communicate efficiently without sharing memory, which reduces complexity and increases fault tolerance. The **“Let it crash”** philosophy encourages developers to write systems that can recover gracefully from failures, contributing to the robustness of Erlang-based applications.

**Elixir**, created by José Valim in 2011, builds on the solid foundation of the Erlang VM but introduces a more modern and developer-friendly syntax. Elixir retains the powerful concurrency and fault tolerance features of Erlang while adding capabilities such as metaprogramming and functional programming enhancements. Elixir’s syntax is **inspired by Ruby**, which makes it more approachable for developers familiar with that language.

Together, Erlang and Elixir provide a powerful stack for building scalable, reliable, and maintainable applications. Their features make them a perfect choice for companies like WhatsApp and Discord, which require systems capable of handling large numbers of concurrent users with minimal downtime.

## **5 core Features that make BEAM a Secret Weapon for Real-Time Applications**

The decision to use the BEAM for building real-time applications at WhatsApp and Discord was driven by several critical factors: fault tolerance, scalability, distribution, responsiveness, and live code updates.

Let’s explore how these features align with the requirements of real-time communication platforms.

### **1) Fault Tolerance**

![BEAM Fault Tolerance](https://favtutor.com/articles/wp-content/uploads/2024/06/Fault-Tolerance.jpg)

Reliability is crucial for applications like WhatsApp and Discord, which must operate continuously without significant downtime. Erlang’s **“Let it crash”** philosophy ensures that individual process failures do not bring down the entire system. Instead, processes are isolated, and supervisors can restart failed processes automatically. This design minimizes the impact of crashes and contributes to the overall stability of the application.

### **2) Scalability**

Scalability not only needs the ability to expand physical servers horizontally, but also how to communicate between those servers. But communications in distributed systems always bring several issues – race conditions, node discoveries, orchestration failure, etc. There are lots of reasons why these issues are brought up, but most of them come from the ill-managed complexity. 

BEAM, on the other hand, achieves scalability without any fancy yet dangerous tools out there – it handles inter-node communications like a charm. Let’s see a simple example of how to communicate between two nodes in Elixir:

First, we need to start two nodes:

```
# Start the first node, which doesn't have function `Hello.world/0`
$ iex --sname node1@127.0.0.1 --cookie secret -S mix

# Start the second node, which has function `Hello.world/0`
$ iex --sname node2@127.0.0.1 --cookie secret -S mix
```

**Then, we can connect the two nodes. We’ll connect \`node1\` to \`node2\`:**

```
# In node1
iex(node1@127.0.0.1)1&gt; Node.connect(:"node2@127.0.0.1")
true

# Now you can call the function `Hello.world/0` from `node2` in `node1`
iex(node1@127.0.0.1)2&gt; Node.spawn(:"node2@127.0.0.1", fn -&gt; Hello.world() end)
Hello, world!
```

This was a simple example, but you can see how easy it is to communicate between nodes in Elixir. Without the power of BEAM’s scalability, we would have to use complex distributed systems like Apache Kafka, RabbitMQ, etc. to achieve the same result.

### **3) Distribution**

Real-time “global” applications like WhatsApp and Discord require a distributed architecture to handle users from different regions. This needs a system that can distribute workloads across multiple nodes and ensure seamless communication between them. **BEAM’s scheduler** can not only distribute processes across multiple cores on a single machine but also **across multiple machines** in a network. And also, it treats every process the same, whether it’s on the same machine or a different one. This makes it easy to build distributed systems without worrying about the underlying infrastructure.

![BEAM Distribution](https://favtutor.com/articles/wp-content/uploads/2024/06/BEAM-Scheduler.jpg)

This ease of distribution also connects to the scalability feature. When you need to scale your application, you can easily add more nodes to the cluster and distribute the workload across them. No need to worry about complex load balancers or sharding strategies; BEAM takes care of it for you.

### **4) Responsiveness**

Real-time communication platforms must deliver messages and updates to users quickly and reliably. Erlang takes the execution of multiple processes into its own hands by employing dedicated schedulers that interchangeably execute many Erlang processes. A scheduler is **preemptive** — it gives a small execution window to each process and then pauses it and runs another process. 

![BEAM Responsiveness](https://favtutor.com/articles/wp-content/uploads/2024/06/Beam-Responsiveness.jpg)

Because the execution window is small, a single **long-running process can’t block** the rest of the system. Furthermore, I/O operations are internally delegated to separate threads, or a kernel-poll service of the underlying OS is used, if available. This means any process that waits for an I/O operation to finish won’t block the execution of other processes.

### **5) Live Code Updates**

Maintaining real-time applications requires the ability to deploy updates without disrupting the user experience. Usually, in a containerized cloud environment, you would use techniques like blue-green deployment, canary deployment, etc. to achieve this. But with BEAM, you can update code on a running system without restarting the application. This feature is known as “**hot code swapping**” and is crucial for real-time applications where continuous operation is essential. It allows developers to fix bugs, add features, or optimize performance without interrupting the service.  

## **Case study #1: How WhatsApp leveraged Erlang to scale real-time messaging with more than a billion daily active users**

In 2009, WhatsApp was founded by Brian Acton and Jan Koum, who wanted to create a messaging app that was **cross-platform, fast, reliable, and secure**. They chose Erlang as the foundation for their real-time messaging platform because of its fault tolerance, scalability, and distribution capabilities. The basic architecture and codebase for WhatsApp’s messaging systems were inspired by an open source project named ejabberd, an XMPP server written in Erlang. 

WhatsApp’s growth was exponential, and by 2018, it had over a **billion daily active users**. Thanks to the efficiency of Erlang and their careful refinement of both ejabberd, they managed to handle millions of messages per day with minimal downtime while keeping the team lean and their hardware and engineering costs low.

According to Anton Lavrik, a WhatsApp server engineer, one of **Erlang’s best attributes is concurrency**. He says “Other languages or platforms can try to achieve the same level of concurrency, but they often require a lot of effort and resources. Since Erlang was built to solve the problem of concurrency from the beginning, it’s much easier to achieve the same level of concurrency with Erlang”.

## **Case study #2: How Discord used Elixir to build a real-time chat application that scales to 11 million concurrent users**

Discord, founded in 2015 by Jason Citron and Stan Vishnevskiy, is a real-time chat application designed originally for gamers. Back then, they chose two main languages to build their infrastructure: Python and Elixir. The Python codebase in Elixir still remains for powering API related stuff, but most of the core features, including real-time chat, are powered by Elixir.

Discord’s popular servers, such as those dedicated to popular games like Fortnite and Minecraft, can have hundreds of thousands of users chatting simultaneously. Hence it should be very good at **broadcasting** messages and user status updates in real-time, without any delay. Elixir’s concurrency model and fault tolerance features made it an ideal choice for building Discord’s real-time chat system. The ability to handle massive concurrency and distribute workloads across multiple nodes allowed Discord to scale its chat platform while maintaining high performance and reliability.

Then in 2019, Discord reached a new milestone: they had to handle way more concurrent users than ever before. This time they had to use not only the power of BEAM, but also the power from other more so-called “performant” languages. They used Rust to build their new member list that can rapidly update the status of 11 millions of concurrent users, and the cool thing is that they adopted the precompiled Rust module into Elixir codebase, using a feature called _NIF (Native Implemented Function)_. This way, they could leverage the **performance of Rust while still using the productivity of Elixir**.  

## **Case study #3: (My personal) How I built a real-time networking application with Elixir and Phoenix**

Last year, I had a chance to build a fun project called “FoodFocus” – which broadcasts a live video of a user cooking a dish, and other users can join the live video and chat with the host. Not only that, but they can also connect their smartwatch to the application, and the application will show the synchronized recipe instructions as a timer on their watch.  

By the time I started building this project, I had to choose the most cost-effective tech stack to build it. The most important traits of the tech stack were:

– Easy to build real-time features

– Should provide ready-to-use tools so that I don’t have to bring in a lot of third-party libraries

– Should be fun to work with – a nice developer experience

After some research, I decided to use **Elixir and Phoenix** for the backend, and Flutter for the frontend. Though it was my first time building a production-ready application with Elixir, I was amazed by how easy it was to build real-time features with Phoenix.  

These are some of my favourite features and tools I found very useful while developing the project with Elixir:

-   **Phoenix Channels**: Phoenix Channels are a great way to build real-time features. They are built on top of WebSockets, and they provide a simple and easy-to-use API for building real-time applications.
-   **ETS (Erlang Term Storage)**: ETS is a built-in key-value store in Erlang that is very fast and efficient. There’s no need to bring something like Redis for caching or storing temporary data.
-   **Postgres-backed tools:** Elixir ecosystem has a lot of tools that are backed by Postgres. **Ecto** for database operations, **Oban** for background jobs, and **ElectricSQL** for running local-first queries for better UX.

Thanks to these features, I was able to deliver the project on time and within budget. If I had chosen a different tech stack, I might have spent more time configuring and integrating third-party libraries, and have less time to focus on building the actual features.

## **Conclusion** 

WhatsApp and Discord are exemplary cases of how the Erlang VM and Elixir can be leveraged to build scalable, real-time applications. However, they are not the only companies harnessing the power of these technologies. Other prominent companies like **Spotify, Pinterest, Pepsico, Financial Times, and Heroku** have also adopted Erlang-based languages to achieve high concurrency, fault tolerance, and scalability in their respective domains.

In conclusion, whether you are building a new messaging platform, a real-time gaming service, or any other hyper connected app, I strongly encourage you to consider the Erlang VM and Elixir as your secret weapons for success. Their unique features and capabilities can help you establish a strong foundation for your application and scale it to new heights without compromising on performance or reliability.
