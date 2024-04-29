---
created: 2024-04-18T19:27:52 (UTC -03:00)
tags: [softwareengineering,careerdevelopment,programming,productivity,software,coding,development,engineering,inclusive,community]
source: https://dev.to/moozzyk/future-proof-code-59ib
author: 
---

# Future proof code - DEV Community

> ## Excerpt
> “Because in the future we might need…” triggers me a lot.   When I hear “In the future we might...

---
[![Cover image for Future proof code](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7j43vomnbx3dq3ru5ug7.png)](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7j43vomnbx3dq3ru5ug7.png)

[![Pawel Kadluczka](https://media.dev.to/cdn-cgi/image/width=50,height=50,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Fuser%2Fprofile_image%2F1213345%2Fe266f9df-cfe4-4b46-8830-70194cb56354.JPG)](https://dev.to/moozzyk)

“Because in the future we might need…” triggers me a lot.

When I hear “In the future we might need…” I am hearing: “I am adding this complex code for no particular reason, and we will have to maintain it for a long time only to spend weeks to remove it”.

It is like saying “In the future I might want to build a pool so let me dig a giant hole somewhere in my backyard right now.”

Digging this hole will create a bunch of problems. It’s a safety hazard, so you’ll have to fence it. During a storm, a lot of water will pool there, and you’ll need to pump it out. Your guests will ask about your hole or comment on it each time they visit you. Your or your friends’ kids won’t be able to play in the backyard, and if they try, you’ll be the one to get the ball from the hole. Eventually, you may decide to sell the house or come to the conclusion that a pool in the backyard was not that splendid of an idea, and your giant hole is now a giant problem. If you do happen to move forward with the pool project years later, your pool hole will likely not be where you want the pool now. It will probably also be either too small, too big, too deep, or too shallow.

What do you do if you consider building a pool next to your house sometime in the future? You don’t make decisions that will make it impossible to build it when the time comes: you don’t sell this land, you don’t build a bunker or mother-in-law unit there. You just keep the land maintained. You can even build some lightweight structures like a playground or a shed. If you give up on the pool idea, decide to sell the house, or use the land differently, you can easily do so at no additional cost. If you decide to build the pool and are ready for the project end-to-end, only then do you dig the hole. This time, you will know exactly what hole to dig and where.

Software is not much different. Integrating frameworks and adding unnecessary abstractions or superfluous extensibility because we might need them in the future is like digging a pool hole for a future pool. It makes code hard to understand and modify and creates a huge maintenance burden. It is a source of bugs, on-call toil, and performance issues. Removing gets harder with each incoming change. And when you finally try to use this code (if it ever happens) you will find it impossible. The assumptions you made will have changed. The abstractions won’t be quite right, and the extensibility points won’t be usable.

The best way to make the code ready for the future is to keep it as simple as possible with good test coverage. Then, when you’re ready to implement your feature, you can change it safely and easily.
