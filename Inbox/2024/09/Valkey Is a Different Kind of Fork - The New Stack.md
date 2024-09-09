---
created: 2024-09-03T14:56:20 (UTC -03:00)
tags: []
source: https://thenewstack.io/valkey-is-a-different-kind-of-fork/?utm_source=tldrnewsletter
author: Steven J. Vaughan-Nichols
---

# Valkey Is a Different Kind of Fork - The New Stack

> ## Excerpt
> A fork of Redis, Valkey starts to gain its own momentum.

---
HONG KONG —At KubeCon + CloudNativeCon + Open Source Summit China 2024 this week, it was made crystal clear that the Valkey open source data store has a bright future ahead.

[Dirk Hohnde](https://www.linkedin.com/in/dirkhohndel/)l, a Linux kernel developer and long-time open source leader, wanted his audience at [KubeCon + CloudNativeCon + Open Source Summit China 2024 Summit China](https://events.linuxfoundation.org/kubecon-cloudnativecon-open-source-summit-ai-dev-china/) to know he’s not a [Valkey](https://valkey.io/) developer. He’s a Valkey user and fan.

He opened his speech by [recalling](https://thenewstack.io/linux-foundation-forks-the-open-source-redis-as-valkey/) how the open source, high-performance key/value datastore [Valkey had been forked from Redis](https://thenewstack.io/valkey-will-not-just-be-a-redis-retread/).

Hohndel said, “Five months ago, [Redis](https://redis.io/) decided to change the license after saying that Redis would forever be open source. A lot of people who use Redis under the terms of their [open source licenses](https://thenewstack.io/how-do-open-source-licenses-work-the-ultimate-guide/ "open source licenses") were very unhappy. The timing that Redis did this was super interesting because they did it during [KubeCon Europe](https://events.linuxfoundation.org/kubecon-cloudnativecon-europe/), where all the annoyed companies and many of the most annoyed people were meeting. So, they got together and decided to support a fork.”

In the meantime, “the maintainers with [Madelyn Olson](https://www.linkedin.com/in/madelyn-olson-6a5053b6/), who is an [Amazon Web Services](https://aws.amazon.com/?utm_content=inline+mention) principal engineer and [former longtime Redis maintainer](https://www.linkedin.com/feed/update/urn:li:activity:7176350563071139840/), were actually ahead of this and had already created a new version. So a couple of weeks later, the first release of Valkey 7.2 \[a fork of Redis 7.2\] came out.”

Hohndel emphasized that “forks are good. Forks are one of the key things that open source licenses are for. So, if the maintainer starts doing things you don’t like, you can fork the code under the same license and do better. So, if you think Linus Torvalds is not a good maintainer for the Linux kernel, you can fork it. Not sure how well this will go, but more power to you.”

In this case, though, Redis had done a “bait-and-switch” with the Redis code, Hohndale argued. This was because they had made an all-too-common business failure: They hadn’t realized that “open source is not a business model.”

TRENDING STORIES

1.  [Valkey Is a Different Kind of Fork](https://thenewstack.io/valkey-is-a-different-kind-of-fork/)
2.  [Get Started With AWS Bedrock for GenAI Apps](https://thenewstack.io/get-started-with-aws-bedrock-for-genai-apps/) 
3.  [Database as a Service: The Hidden Cost of Convenience](https://thenewstack.io/the-hidden-cost-of-dbaass-convenience/)
4.  [Will Your Cassandra Database Project Succeed?](https://thenewstack.io/will-your-cassandra-database-project-succeed/) 
5.  [Valkey Will Not Just Be a Redis Retread](https://thenewstack.io/valkey-will-not-just-be-a-redis-retread/)

Redis’ [move](https://thenewstack.io/valkey-a-redis-fork-with-a-future/) did not just cause a fork, though. Many open source forks have occurred, and more often than not, they fail rather than succeed. As  Torvalds himself has said, “I think [forks are good things](https://www.scientificamerican.com/article/linux-and-android-together-at-last-2012-03/). It means somebody sees a need and a technical reason to do something different from the standard kernel. But most forks are failures.”

## Reasons to Succeed

Redis is different. While the licensing change is what prompted the fork, Hohndel sees leadership and technical reasons why the Valkey fork is likely to succeed.

First, two-thirds of the formerly top Redis maintainers and developers have switched to Valkey. In addition, AWS, [Google Cloud](https://cloud.google.com/?utm_content=inline+mention), and [Oracle](https://developer.oracle.com/?utm_content=inline+mention), under the [Linux Foundation](https://training.linuxfoundation.org/training/course-catalog/?utm_content=inline+mention)‘s auspices, all support Valkey. When both the technical and money people agree, good things can happen.

The other reason is that Valkey already looks like it will be the better technical choice.

## Valkey 8.0

That’s because the recently announced [Valkey 8.0](https://valkey.io/blog/valkey-8-0-0-rc1/), which builds upon the last open source version of Redis, 7.2.4, introduces serious speed improvements and new features that Redis users have wanted for some time.

As Olson said at Open Source Summit North America earlier this year, “Redis really didn’t want to break anything.” Valkey wants to move a bit faster.

How much faster? A lot. Valkey 8.0 overhauls Redis’s single-threaded event loop threading model with a more sophisticated multithreaded approach to I/O operations.

Hohndel reported that on his small Valkey-powered aircraft tracking system, “I see roughly a threefold improvement in performance, and I stream a lot of data, 60 million data points a day.”

He continued, “Obviously, benchmarks are never the same as running your actual application, but one of the things I love about open source is you can simply try and see for yourself.”

Hohndel added that with Valkey 8, “he also saw something like a 20% reduction in the size of separate cache tables. When you’re talking about terabytes and more on Amazon Web Services, that’s a real savings in size and cash.”

Valkey 8.0 introduces several features aimed at improving the developer experience. These include improved debugging tools; enhanced documentation, including migration guides for Redis users; and new command-line utilities for easier management and monitoring.

Simultaneously, the Valkey team has put significant effort into ensuring a smooth transition for existing [Redis users](https://thenewstack.io/redis-is-not-just-a-cache/). Valkey 8.0 maintains full API and CLI compatibility with Redis and compatibility with the Redis Serialization Protocol (RESP).

This makes migrating from Redis to Valkey easy, said Hohndel. You can do a rollover or simply backup and restore your data from Redis to Valkey without any fuss or muss. The last is what Hohndel did. He saw on his admittedly small, simple system, ten-seconds of downtime. Companies are also now offering Redis to Valkey services.

Valkey also already has support from major cloud providers such as AWS, Google Cloud, and Oracle Cloud. In addition, many Linux distros, including [AlmaLinux](https://thenewstack.io/almalinux-your-enterprise-linux-ticket-to-freedom/), Fedora, and Alpine are now supporting Valkey.

Hohndel thinks this combination of broad support and greatly improved performance will make Valkey a winner to users and developers alike. Based on what I’ve seen and heard, I agree with him. If Valkey lives up to its promise, [Redis](https://redis.com/?utm_content=inline+mention) has serious competition.

___

**Correction:** A previous version of this article misidentified the version numbers of the initial fork of Redis and the resulting Valkey project. The first release of Valkey 7.2 was forked from Redis 7.2. Also, Valkey 8, not Redis 8, saw a 20% reduction in the size of separate cache tables, according to Hohndel.
