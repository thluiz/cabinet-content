---
created: 2024-04-02T14:13:55 (UTC -03:00)
tags: [fly,fly.io,blog,elixir,docker,cdn,hosting,servers,networking,deploy app servers,close to users,postgresql clusters,heroku competitor,heroku alternative]
source: https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/
author: 
---

# Clustering Elixir From Laptop to Cloud · The Phoenix Files

> ## Excerpt
> Documentation and guides from the team at Fly.io.

---
Author

![Mark Ericksen](https://fly.io/static/images/mark.webp)

Name

Mark Ericksen

@brainlid

[@brainlid](https://twitter.com/brainlid)

![A cool Elixir drop character wearing sunglasses at a computer. The computer is wired directly to a Fly balloon out the window.](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/assets/clustering-from-laptop-to-cloud-cover.webp)

Image by [Annie Ruygt](https://annieruygtillustration.com/)

We’re Fly.io and we transmute containers into VMs, running them on our hardware around the world. We have fast booting VM’s and GPUs; so why not [take advantage of them](https://fly.io/docs/elixir/)?

**TL;DR:** Check out the Elixir Fly.io guide [Easy Clustering from Home to Fly.io](https://fly.io/docs/elixir/advanced-guides/clustering-from-home-to-your-app-in-fly) for the steps on exactly how to cluster the Elixir application running on your laptop with one running at Fly.io. Grab the bash script that automates a chunk of it too. Here we talking about what this means and why we might want to do it.

What if you could cluster the Elixir app running on your laptop with the one you deployed on Fly.io? Would you want to? What would you do if you could? Because, you can, you know.

## [](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/#what-do-we-mean-by-quot-clustering-quot)What do we mean by “clustering”?

If you are newer to Elixir, clustering isn’t creating a web API and making calls to another server. The concept can feel foreign as not many languages have this built-in feature.

Elixir actually inherits this ability from [Erlang](https://www.erlang.org/). Erlang is a concurrent, functional programming language designed for building scalable and fault-tolerant systems. It was developed by [Ericsson](https://www.ericsson.com/en) for use in telephony applications but has since been adopted by numerous industries for applications requiring high availability and concurrency.

“[Clustering](https://fly.io/docs/elixir/the-basics/clustering/)” here refers to the practice of connecting multiple Elixir/Erlang runtime systems (nodes) over a network into a single distributed system. These clustering capabilities are a core part of the language and the OTP (Open Telecom Platform) framework, which provides a set of libraries and design principles for building robust systems.

In practical terms, this means we can have two Elixir applications running on two separate machines and connect them in a seamless way. Once connected, we can execute arbitrary functions on another machine and even send messages to processes in a way where the application code doesn’t need to know about the other machine at all!

## [](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/#why-cluster-the-app-on-my-laptop-with-one-on-the-server)Why cluster the app on my laptop with one on the server?

Clustering is already widely used for sharing [PubSub](https://hexdocs.pm/phoenix_pubsub/Phoenix.PubSub.html) messages across machines. It can be used for a _lot_ of things actually. It lets our applications assign work to another node that is better suited to the task. We don’t _have_ to assign work, perhaps we want to monitor and interact with a running application on another machine. We can do all that and more.

Let’s explore a few reasons we might want to do this:

1.  Developing AI/ML applications and leveraging more powerful hardware.
2.  Developing and testing distributed applications.
3.  It’s cool.

### [](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/#1-developing-ai-ml-applications)1\. Developing AI/ML applications

Right now, “AI” is all the rage. Got an AI startup idea? VC’s may throw money at you just for saying so. But _developing_ an AI application may be more intensive and, depending on what you have in mind to build, may require more specialized hardware than what comes packed in your laptop.

We can offload the AI/ML work to a machine with a GPU, but develop our application normally on our laptop.

More to share on that in the near future!

### [](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/#2-developing-distributed-applications)2\. Developing distributed applications

Building a globally distributed application can be challenging to model locally. With [Fly.io Regions](https://fly.io/docs/reference/regions/), we can deploy our cluster-aware application where it makes sense. Then, our local application joins the globally cluster, giving us a close-up view of how the application behaves in a truly globally distributed environment.

### [](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/#3-its-cool)3\. It’s cool

Okay, “it’s cool” isn’t a good business reason, but come on… this is cool! Sometimes we need to play with things for sake of play. When we’re playing, we’re more likely to ask ourselves a question like “Huh, I wonder if…”, which can lead us to interesting personal breakthroughs.

## [](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/#how-do-we-do-it)How do we do it?

A [Fly.io Elixir guide](https://fly.io/docs/elixir/advanced-guides/) was added detailing how to do this. In the [Easy Clustering from Home to Fly.io](https://fly.io/docs/elixir/advanced-guides/clustering-from-home-to-your-app-in-fly) docs, there’s a [bash script](https://fly.io/docs/elixir/advanced-guides/clustering-from-home-to-your-app-in-fly/#bash-script-file) that automates much of the process, making it easy to start a local Elixir application and cluster it with one running on Fly.io.

Here’s how it works:

1.  [Fly.io’s WireGuard VPN network](https://fly.io/docs/networking/private-networking/#private-network-vpn) connects the laptop to the Fly.io network
2.  Elixir application should be [setup for clustering](https://fly.io/docs/elixir/the-basics/clustering/)
3.  Run the same version of Elixir and Erlang on both ends
4.  [Configure and run the script](https://fly.io/docs/elixir/advanced-guides/clustering-from-home-to-your-app-in-fly)!

Steps 1-3, are a one-time setup. Once that’s worked out, it’s just:

-   Open the VPN connection
-   Run the script

## [](https://fly.io/phoenix-files/clustering-elixir-from-laptop-to-cloud/#conclusion)Conclusion

Connecting the Elixir application running on our laptop to an Elixir application running on the server isn’t difficult. We can cluster the same or different applications together.

There are even practical reasons to do it. We can offload AI/ML tasks to a machine with a GPU and develop our applications as per usual. We can develop and debug truly globally distributed applications right from our laptop. And, it’s freaking cool.

Check out [the documentation](https://fly.io/docs/elixir/advanced-guides/clustering-from-home-to-your-app-in-fly) to walk through the process.

What are you going to play with?

Next post ↑

[Phoenix Dev Blog – Server logs in the browser console](https://fly.io/phoenix-files/phoenix-dev-blog-server-logs-in-the-browser-console/)

Previous post ↓

[What if S3 could be a fast, globally synced, Key Value Database? That's Tigris](https://fly.io/phoenix-files/what-if-s3-could-be-a-fast-globally-synced-key-value-database-that-s-tigris/)
