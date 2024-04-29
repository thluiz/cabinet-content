---
created: 2023-12-27T10:34:54 (UTC -03:00)
tags: []
source: https://twitter.com/milan_milanovic/status/1739559959140958481?s=20
author: 
---

# Dr Milan Milanović no X: "𝗪𝗵𝗮𝘁 𝗶𝘀 𝘁𝗵𝗲 𝗺𝗼𝘀𝘁 𝗶𝗺𝗽𝗼𝗿𝘁𝗮𝗻𝘁 𝘁𝗵𝗶𝗻𝗴 𝗶𝗻 𝗰𝗼𝗺𝗽𝘂𝘁𝗲𝗿 𝘀𝗰𝗶𝗲𝗻𝗰𝗲? According to Professor Of Computer Science John Ousterhout from Stanford University, it is a 𝗽𝗿𝗼𝗯𝗹𝗲𝗺 𝗱𝗲𝗰𝗼𝗺𝗽𝗼𝘀𝗶𝘁𝗶𝗼𝗻. In his book "𝗔 𝗣𝗵𝗶𝗹𝗼𝘀𝗼𝗽𝗵𝘆 𝗼𝗳… https://t.co/c26yNrRznm" / X

> ## Excerpt
> Post

---
## Post

## Conversa

𝗪𝗵𝗮𝘁 𝗶𝘀 𝘁𝗵𝗲 𝗺𝗼𝘀𝘁 𝗶𝗺𝗽𝗼𝗿𝘁𝗮𝗻𝘁 𝘁𝗵𝗶𝗻𝗴 𝗶𝗻 𝗰𝗼𝗺𝗽𝘂𝘁𝗲𝗿 𝘀𝗰𝗶𝗲𝗻𝗰𝗲? According to Professor Of Computer Science John Ousterhout from Stanford University, it is a 𝗽𝗿𝗼𝗯𝗹𝗲𝗺 𝗱𝗲𝗰𝗼𝗺𝗽𝗼𝘀𝗶𝘁𝗶𝗼𝗻. In his book "𝗔 𝗣𝗵𝗶𝗹𝗼𝘀𝗼𝗽𝗵𝘆 𝗼𝗳 𝗦𝗼𝗳𝘁𝘄𝗮𝗿𝗲 𝗗𝗲𝘀𝗶𝗴𝗻" he advocates that good software design means fighting complexity and recommends different strategies. Here are some which I agree with: 𝟭. 𝗖𝗹𝗮𝘀𝘀𝗲𝘀 𝘀𝗵𝗼𝘂𝗹𝗱 𝗯𝗲 𝗱𝗲𝗲𝗽 𝗮𝗻𝗱 𝗶𝗻𝘁𝗲𝗿𝗳𝗮𝗰𝗲𝘀 𝘀𝗶𝗺𝗽𝗹𝗲 The famous David Parnas paper in 1970 ("On the Criteria To Be Used In Decomposing Systems into Modules") mentions that we need simple interfaces but much functionality inside a method or class. We typically have shallow methods with only one line of code inside. He considers the most significant mistake too small and too shallow of classes. For example, we can see (something he calls classisitis) that we need two to three classes in Java to read a simple file: 𝙵𝚒𝚕𝚎𝙸𝚗𝚙𝚞𝚝𝚂𝚝𝚛𝚎𝚊𝚖→𝙱𝚞𝚏𝚏𝚎𝚛𝚎𝚍𝙸𝚗𝚙𝚞𝚝𝚂𝚝𝚛𝚎𝚊𝚖. Bigger modules, generic interfaces, and classes encourage information hiding and reduce complexity. In addition, the author mentions a famous API design antipattern with overexposing internals, which adds to an architecture debt later on. 𝟮. 𝗠𝗶𝗻𝗱𝘀𝗲𝘁 (𝗦𝘁𝗿𝗮𝘁𝗲𝗴𝗶𝗰 𝘃𝘀 𝘁𝗮𝗰𝘁𝗶𝗰𝗮𝗹 𝗽𝗿𝗼𝗴𝗿𝗮𝗺𝗺𝗶𝗻𝗴) Most people use a tactical approach, where the goal is to make something work. However, the result is a bad design with a lot of tech complexity, which usually results in spaghetti code. Complexity is not a single line but many lines in a project, which we overlook as a whole. We sometimes also create a feature that we may need (YAGNI). His recommendation is to take a strategic approach where the working code is not the only goal, but the goal should be great design, which simplifies development and minimizes complexity. 𝟯. 𝗜𝗻𝘃𝗲𝘀𝘁 𝘆𝗼𝘂𝗿 𝘁𝗶𝗺𝗲 𝗶𝗻 𝗾𝘂𝗮𝗹𝗶𝘁𝘆 What we usually see with startups is pressure to build products quickly, which can result in a spaghetti code. But good code gives you more advantages. One of those is to hire great people who like to work with high-quality codebases. In addition, we need to make our development 10-20% slower, and you will get it all back in the future (this also reduces tech debt). Code and refactor in small steps. I can't entirely agree with the usage of 𝗰𝗼𝗱𝗲 𝗰𝗼𝗺𝗺𝗲𝗻𝘁𝘀, although the author mentions that we need to use them only for what is not apparent. He also disagrees with the 𝘂𝘀𝗮𝗴𝗲 𝗼𝗳 𝗲𝘅𝗰𝗲𝗽𝘁𝗶𝗼𝗻𝘀, where the point is that they are a massive source of complexity. Exceptions are usually reasonable, especially if they are appropriately monitored and logged. [#softwaredesign](https://twitter.com/hashtag/softwaredesign?src=hashtag_click) [#softwarearchitecture](https://twitter.com/hashtag/softwarearchitecture?src=hashtag_click) [#programming](https://twitter.com/hashtag/programming?src=hashtag_click) [#developers](https://twitter.com/hashtag/developers?src=hashtag_click) [#techworldwithmilan](https://twitter.com/hashtag/techworldwithmilan?src=hashtag_click)

Traduzir post

[

![Imagem](https://pbs.twimg.com/media/GCQojABXYAAUCm2?format=jpg&name=900x900)



](https://twitter.com/milan_milanovic/status/1739559959140958481/photo/1)

[

![Thiago Luiz Silva](https://pbs.twimg.com/profile_images/1353495216209190912/SA_diDxx_normal.jpg)



](https://twitter.com/artthluiz)

I am a big advocate for point 3 ![👍](https://abs-0.twimg.com/emoji/v2/svg/1f44d.svg "Valeu!") Point 1 remember me the current debate about function and method sizes ![🤣](https://abs-0.twimg.com/emoji/v2/svg/1f923.svg "Rolando de rir no chão")

I've enjoyed this book and recommended to a few good friends. Worth the time to read.

It’s all about balance and trade-off , i have seen managers and leads destroy very promising products because they were too picky about introducing technical debt.

This is a great book! One of the best I have ever read.

Quality is so so important and good test strategy is ![👍](https://abs-0.twimg.com/emoji/v2/svg/1f44d.svg "Valeu!")

Didn’t he mention you may need not using oop in general;) Or concepts interface and classes goes beyond the oop paradigm?

## 

Descubra mais

Originado de todo o X

The acronym ACID in the context of DB transactions stands for Atomicity, Consistency, Isolation, and Durability. This post is going to make each term crystal clear. ![✅](https://abs-0.twimg.com/emoji/v2/svg/2705.svg "Marca de seleção branca espessa")Atomicity (A) Atomic means something that cannot be broken down. It’s the same thing that our Physics…

Mostrar mais

![](https://pbs.twimg.com/tweet_video_thumb/GCVmmAiawAAFesC.jpg)

Data Pipelines Overview. The method to download the GIF is available at the end. Data pipelines are a fundamental component of managing and processing data efficiently within modern systems. These pipelines typically encompass 5 predominant phases: Collect, Ingest, Store,…

Mostrar mais

![](https://pbs.twimg.com/tweet_video_thumb/GCVj3iobgAApEq_.jpg)
