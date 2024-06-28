---
created: 2024-06-27T17:58:36 (UTC -03:00)
tags: []
source: https://www.infoworld.com/article/3715601/what-senior-developers-know.html?ref=dailydev
author: Nick Hodges
---

# What senior developers know | InfoWorld

> ## Excerpt
> Are you a good junior developer who wants to be a senior developer? Understand these four things.

---
### Are you a good junior developer who wants to be a senior developer? Understand these four things.

Contributing Writer, InfoWorld | Jun 25, 2024 2:00 am PDT

![Person jumping for joy on a mountaintop. Freedom, expansion, triumph.](https://images.idgesg.net/images/article/2024/04/shutterstock_1649812132-100963054-large.jpg?auto=webp&quality=85,70)

sivivolk/Shutterstock

You will find a lot of articles on the web about what it takes to become a senior developer. Many of these articles focus on soft skills. They often advise staying current with technology, learning to communicate well, mentoring junior developers, and other things along those lines. These recommendations are all fine and good, but, well, let’s be honest. They are also pretty much pabulum. 

I’d like to add a little depth to the discussion. The essential difference between senior developers and junior developers is time in the software development trenches, and what they’ve learned from it. With that in mind, I’ve come up with a far-from-exhaustive list of four things the senior developers know, usually through hard-earned experience.

## Senior developers realize that clarity is king

The single most important thing a developer can do is write readable code. This almost goes without saying, as it is so blazingly obvious. But I’m here to tell you that there are vast swaths of code out there that are not easily readable. Way too much code is written without a single thought or concern about the fact that some poor schmuck will need to read this code at some point in the future. And most of us fail to realize that, very often, that poor schmuck will be us. 

Not only is good code written for the purpose of being read later, but it is also written for the purpose of being debugged. All languages are different, and all debuggers work in their own way, but code can always be written in a way that makes it easy to debug. 

Perhaps surprisingly, writing code that is easy to read and writing code that is easy to debug go hand in hand. Much of debugging is understanding the state of a running application at any given moment. Declaring functions, classes, etc. clearly as the code runs enables the debugger to show you what it is up to. It also has the benefit of making the code readable.

Clarity in code is also achieved by writing code that doesn’t need comments. If a developer feels the need to comment her code, she should stop and ask why. She should very strongly consider that the reason she feels this need is because the code isn’t clear. 

I know many will disagree with me—I’ve known shops that required that _every single line of code be commented_. But the longer I am in the development business, the more strongly I believe that comments shouldn’t be necessary. If you feel the need to comment your code, rewrite it instead. Or better yet, always write code the first time in such a way that comments will be redundant.

## Senior developers know that complexity is the source of most code issues

Avoiding complexity is critical to good code. And the key to avoiding complexity in your code is simple and straightforward: Never write anything that is complex. That sounds like an oxymoron, but it isn’t.

[![ryan singer quote](https://images.idgesg.net/images/article/2024/06/ryan-singer-quote-100963277-large.jpg?auto=webp&quality=85,70)](https://images.idgesg.net/images/article/2024/06/ryan-singer-quote-100963277-orig.jpg?auto=webp&quality=85,70 "<div class='credit'>Nick Hodges</div>ryan singer quote") <small>Nick Hodges</small>

Complexity is easy to avoid. Just stop making any one thing do more than one thing. That’s kind of the definition of complexity in software: entities that do multiple things.  If every entity in your code base does just one thing, then fixing anything involves fixing just that one thing that is broken. 

Nothing—not a class, not a method, not a function, not a line of code—should ever do more than one thing. Sure, complexity will come when all of those “doing just one thing” parts interact with each other. But if something breaks, fixing it will only affect the broken thing, and not anything else.

Not writing complex things in your code is not the same as not writing complex systems. But just as a fine Swiss watch is a complex device, it consists of simple parts—gears and springs—that work together to create the complexity. Thinking of your code this way will limit the complexity of your code, not your application.

## Senior developers resist the temptation to go fast

“Slow is smooth and smooth is fast” is a mantra of the Navy SEALs. Those guys have to make life or death decisions in high-pressure situations. It seems counterintuitive, but makes perfect sense when considered. The idea is that when rushing, one is liable to make mistakes—the more one rushes, the more mistakes. Mistakes take a lot of time. Going slow—not rushing—will reduce or eliminate errors, and making no mistakes makes the overall process faster. 

Senior developers know that this principle works for code as well. Rushed code is bad code. Senior developers know that taking the time to do things right the first time means fewer mistakes, fewer lines of inscrutable code, and better outcomes down the line when the code is maintained. 

They know that nothing good comes from hurrying. Great software comes from being deliberate, careful, and thoughtful when writing code.

## Senior developers will take short-term pain for long-term gain

We’ve all done it. We’ve pounded out some new feature over the weekend, hacking it all together and making it work with utter disdain for the quality of what we are doing. There are many reasons for this, but they are all business-driven, right? A customer won’t sign a deal without it, or they’ll leave for a competitor if they don’t have it Monday morning. Or a demo needs to be completed for a presentation at a conference, and that demo turns into the actual feature.

And we _always_ regret doing this. Always. The code itself is difficult to fix when the inevitable bugs are found. The bolted-on flavor of the feature breaks three other modules in the application, causing another business crisis. It’s a merry-go-round that never stops. But we do it anyway.

A good senior developer knows to resist this as much as possible, and to limit the damage when it happens.

Sure, communication skills and mentoring junior developers are important things for a senior developer to _do_, but what they _know_ through experience is what really sets them apart. Senior developers know to keep code clear and simple, to go slow, and to take the long view.

Nick Hodges is a senior developer at Ideal Software Systems. He’s a long-time Delphi Developer who is currently interested in Astro and TypeScript. You can find him at [nickhodges.com](https://www.nickhodges.com/).

Copyright © 2024 IDG Communications, Inc.
