---
created: 2024-04-12T17:01:53 (UTC -03:00)
tags: []
source: https://blog.beeper.com/2024/04/09/how-beeper-android-works/
author: 
---

# How Beeper Android Works – Beeper Blog

> ## Excerpt
> This blog post is the second in the series of how Beeper works. The first, How Beeper Mini Works, was published back in December with the release of our incredibly popular, but short-lived iMessage…

---
![](https://blog.beeper.com/wp-content/uploads/2024/04/android-tech.png?w=1024)

This blog post is the second in the series of how Beeper works. The first, [How Beeper Mini Works](https://blog.beeper.com/p/how-beeper-mini-works), was published back in December with the release of our incredibly popular, but short-lived iMessage-on-Android app. This post will describe how we’re continuing to evolve our all-your-chats-in-one-app experience to have better performance and security.

Restating our previous post, we believe that it is critical for you to be able to trust the software that you use, especially something as important and sensitive as your chat app. We work to earn and keep your trust in three ways:

1.  Transparency – since we started Beeper 3 years ago, we’ve been taking opportunities like this to explain how Beeper works. We have a proud history of building products, like Pebble, and [stand](https://twitter.com/ericmigi) [publicly](https://twitter.com/bradtgmurray) behind our work.
2.  Open source – each major piece of software that we’ve built to interact with other chat networks is open source at [github.com/beeper](http://github.com/beeper).
3.  Privacy and security-aligned business model – we make great software and will charge for an optional premium subscription. Simple as that. No ads. Your data stays private.

## How Beeper Works Today

Beeper is our all-your-chats-in-one-app experience that we’ve been building for the last 3+ years. The architecture is heavily based on [Matrix](https://matrix.org/), a open, decentralized and secure chat protocol that we believe is the future of chat. Your Beeper username is a bonafide Matrix account, which allows you to securly chat with anyone on any Matrix server in the world. Our original clients were all derived from the open source Element suite of clients, which we’ve extended over the years to better support our chat aggregation features and our focus on personal chat.

In order to aggregate all your contacts on ally your networks into a single chat experience, we take advantage of Matrix’s ability to “bridge” other networks into the Matrix network. Whenever you connect one of your other chat accounts to your Beeper account, we’re running a piece of software that whenever you send or receive messages from that account it’s bridged into your Matrix account, which is then served to any clients you are running. While we take advantage of Matrix’s built-in message encryption to make it so only your clients can read your message history once it’s been bridged, this does mean that you have to trust us with your access tokens and keys to each of your accounts to send and receive messages on your behalf.

![](https://blog.beeper.com/wp-content/uploads/2024/04/image-4.png?w=1024)

While this approach does have a number of advantages when it comes to reliability, speed, and smoth syncing across all your Beeper clients, it’s not as privacy-preserving as we would like. To mitigate this, we’ve invested heavily in the security of our services, and have [open sourced](https://github.com/beeper/) our software to connect to other networks so they can be audited and understood by the community.

Additionally, we’ve built systems for tech savvy users to [self-host](https://github.com/beeper/bridge-manager) the bridges themselves. Users can use a server they already own, or rent an inexpensive virtual machine and host our open source bridge software on their own hardware. When using self-hosting, our servers only see your messages once they’ve already been encrypted with Matrix encryption, and we never see your account credentials or access tokens.

With our new Beeper Android app, we’re also starting to offer the ability to run your bridges locally inside your Beeper Client, but more on that later.

## The Rewrite

![](https://blog.beeper.com/wp-content/uploads/2024/04/mobile-1.png?w=1024)

Our [new Android app](https://play.google.com/store/apps/details?id=com.beeper.android) is a beautifully designed, ground up rewrite of our previous Android app. Contemplating a rewrite is always a challenging decision for engineering teams, as a huge amount of effort could be spent to produce something that’s not as good as what you started with. We resisted this decision for a while, and continued to try to incrementally improve our fork of the open source Element Android app to get it to where we wanted it to be.

However, we made the decision a few months ago based on a few key points.

-   The old Android app was heavily built around a few legacy Android libraries, such as Epoxy for the UI and Realm for the database. These libraries made our builds take longer (full rebuilds were around 15 minutes) and made the app run slower, and were so deeply integrated they were hard to remove without a rewrite. Our new Android app is built using Jetpack Compose for the UI and Room for persistence, and builds in 2-3 minutes, which means it’s faster for our team to fix issues and ship new features.
-   The old Android app was just too large, as its role as the reference Matrix client meant it had to support every feature created for Matrix. Our old Android app was roughly 240k lines of code, where the new one is only 110k (70k~ Kotlin, 40k~ Go, more on that later). Having a smaller codebase that we built ourselves allows us to move quickly to fix bugs and add new features.
-   We had a great starting point with Beeper Mini, which was a new codebase that we built in house. Beeper Mini is much simpler than our full Beeper app, but it gave us a great starting point that gave us confidence we could replace our legacy Android app by adding Matrix support to Beeper Mini.

The effort to replace our old Beeper Android app with a new one based on Beeper Mini began at the start of January 2024. We were able to start sharing an early version with our friends, family, and alpha testers by the middle of February, and launched it in beta for anyone to try out on [March 14th](https://blog.beeper.com/2024/03/14/new-beeper-android-app-open-beta-test/). The feedback has been incredibly positive, and while we’re still missing some features (inbox auto-archive coming soon!), the new app is a huge step forward from the old one.

## Matrix with Go

Something we’ve always struggled with is maintaining 3 different codebases (Android, iOS, desktop) that all implement the same Matrix client bits. Our desktop app has struggled with restoring room encryption keys immediately after login. Our old Android app was slow to send messages because it was slow to share room encryption keys with users. Our iOS app had occasionally send failures because it had issues tracking who it had already shared keys with. Each platform implemented the Matrix protocol in its own way, and had its own bugs. Fixing one didn’t mean that it was fixed everywhere.

With our Android rewrite, we decided to implement the same strategy that Element decided to use, with a common SDK for all their clients. Ours is built in Go instead of Rust.

We decided to build our own SDK instead of using matrix-rust-sdk for a few reasons.

-   At Beeper, we’re narrowly focused on personal chat (think WhatsApp), which means our use case is a little different from Element. For example, where matrix-rust-sdk requires sliding sync to handle users being in 1000s of rooms with potentially 10s of 1000s of members. For us, our users are in much fewer rooms, with much fewer members. To satisfy this, we designed something that we refer to as “Streaming Sync”, where there are no timeline limits or lazy loaded members, your phone simply receives every event in every room in real time. Such different requirements lead to different design decisions.
-   We also wanted a different architecture and scope than matrix-rust-sdk. matrix-rust-sdk is intended to be a full client that you wrap a UI around, and is intended to be a reference Matrix SDK that implements the full spec. This has a size and complexity cost, and weighs in at around 127k lines of Rust code. We wanted to have a thin-client approach, where our SDK would handle syncing data and decrypting it and little else, and handing most of the responsibilities up to the app layer. This allows us to have a much simpler SDK of only around 40k lines of Go, and to use the libraries native to the client platform to manage things like the room members or the timeline. The Go SDK handles receiving events from the Matrix API, decrypts them, and then just sends them up to the Kotlin App layer for processing and storage.
-   Our team already knows Go inside and out and maintains mautrix-go, a full Matrix client library. The vast majority of the Go code in the Android app is just mautrix-go, with only another 4k lines to adapt mautrix-go to being used as a mobile client SDK. We already knew that mautrix-go would allow us to build a great client, as Tulir has already built a command line Matrix client named [gomuks](https://github.com/tulir/gomuks) using it, which also powers [Beepy](https://beepy.sqfmi.com/). We also knew that Go would work well inside an Android app based on our experience using our [iMessage Go library](https://github.com/beeper/imessage) to power [Beeper Mini](https://blog.beeper.com/p/how-beeper-mini-works).

This approach has worked wonderfully, and work has begun on moving our Beeper Desktop and Beeper iOS codebases over to using our new Go-based Matrix SDK.

## Local Signal Bridge

The other thing that this architecture allows us to do is to include additional Go libraries to enable adding more networks than just Matrix to our Android chat app. With our new Beeper Android app, we’ve included an experimental local Signal bridge, powered by the same library that we use for our open-source [Signal bridge](https://github.com/mautrix/signal). This allows users to use their existing Cloud-based bridges through Matrix, while also connecting directly to the Signal servers for the most secure experience, using an approach very similar to [How Beeper Mini Works](https://blog.beeper.com/p/how-beeper-mini-works).

That means that with our local Signal bridge, your Signal keys never leave your device, and end-to-end encryption is completely preserved.  

![](https://blog.beeper.com/wp-content/uploads/2024/04/image-5.png?w=1024)

At a high level, when a new Signal client is connected to your Signal account that client creates a random client-specific password and passes that to the Signal server at registration time. This password, along with the user’s `ACI` (ACcount Identifer, a UUID that represents the user’s Signal account), becomes a set of credentials that the client can use to connect to the Signal servers through a websocket. Whenever a message is sent to that user’s account, the Signal servers will notify the websocket that a message was received. This message will be encrypted with the client’s Signal encryption keys, which only the client has, and are different than the client-specific password created earlier.

With Android applications, Android itself limits how often your code can run in the background in order to minimize impact on battery life. In order to process messages immediately when they’re received, we need to wake up the Android application immediately when a message is received. We need something that can stay always available to receive the notification, even when your Android app is asleep.

We use the same Beeper Push Notification Service (BPNS) we used for Beeper Mini, adapted to handle Signal credentials instead of iMessage credentials. The local Signal bridge running inside the Beeper Android app shares the `ACI UUID` and the client-specific password with BPNS, and then it can connect to the Signal websocket on behalf of the client when the Android app is asleep. BPNS will be notified whenever a message is sent to the user, and then BPNS can use Firebase Cloud Messaging to wake up the Android app to reconnect the same websocket, receive the encrypted message, decrypt it, and show a notification.

BPNS can not see the message content since it’s encrypted with the Signal message encryption keys, and thanks to technologies like [Sealed Sender](https://signal.org/blog/sealed-sender/), BPNS doesn’t even know who the message was from. Only the Android device that you own can read your Signal messages, and no Beeper servers can send or receive messages on your behalf.

Our local Signal Bridge is still experimental, as there are several challenges to solve. Only Beeper Android supports the local Signal Bridge, which means that if you’d like to use Signal on Beeper Desktop or Beeper iOS, you’ll still need to use our cloud-based connection until we add local bridge support to those clients later this year. Features such as scheduled messages will need to be rebuilt as the local Signal Bridge on your phone that can send a message and it may not be immediately available. Since this bridge is not connected in any way to your Matrix homeserver, bots or other integrations built against the Matrix HTTP API won’t be able to interact with your chats.

Even with all these challenges, we think local bridges are the best way to securly connect your Beeper clients to your accounts on end-to-end encrypted networks. We’re active working on bringing the Local Signal Bridge out of its experimental phase, enabling local bridges in all of our Beeper clients (not just Android), and supporting more networks than just Signal (especially the end-to-end encrypted ones, such as WhatsApp and iMessage).

Brad Murray  
Beeper CTO -> Engineering @ Automattic
