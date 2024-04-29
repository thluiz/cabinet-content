---
created: 2024-03-05T12:38:13 (UTC -03:00)
tags: []
source: https://stackoverflow.blog/2023/12/29/the-hardest-part-of-building-software-is-not-coding-its-requirements/?utm_campaign=the-overflow-newsletter&utm_medium=email&utm_source=iterable
author: Jared Toporek
---

# The hardest part of building software is not coding, it's requirements - Stack Overflow

> ## Excerpt
> [Ed. note: While we take some time to rest up over the holidays and prepare for next year, we are re-publishing our top ten posts for the year. Please enjoy our favorite work this year and we’ll see you in 2024.]

---
_\[Ed. note: While we take some time to rest up over the holidays and prepare for next year, we are re-publishing our top ten posts for the year. Please enjoy our favorite work this year and we’ll see you in 2024.\]_

With all the articles about just how amazing all the developments in AI have been, there’s plenty of hand wringing around the possibility that we, as software developers, could soon be out of a job, replaced by artificial intelligence. They imagine all the business execs and product researchers will bypass most or all of their software developers and asking AI directly to build exactly what they think they want or need. As someone who’s spent 15 years creating software from the specs these folks create, I find it hard to take all the worrying seriously.

Coding can be a challenge, but I’ve never had spent more than two weeks trying to figure out what is wrong with the code. Once you get the hang of the syntax, logic, and techniques, it’s a pretty straightforward process—most of the time. The real problems are usually centered around what the software is supposed to do. The hardest part about creating software is not writing code—it’s creating the requirements, and those software requirements are still defined by humans.

This article will talk about the relationship between requirements and software, as well as what an AI needs to produce good results.

## It’s not a bug, it’s feature…no wait, it’s a bug

Early in my software career, I was placed on a project midstream in order to help increase the velocity of the team. The main purpose of the software was to configure custom products on ecommerce sites.

I was tasked with generating dynamic terms and conditions. There was conditional verbiage that depended on the type of product being purchased, as well as which US state the customer was located in due to legal requirements.

At some point, I thought I found a potential defect. A user would pick one product type, which would generate the appropriate terms and conditions, but further along the workflow it would allow the user to pick a different product type and predefined terms and conditions. It would violate one of the features explicitly agreed on in the business requirement that had the client’s signature.

I naively asked the client, “Should I remove the input that allowed a user to override the right terms and conditions?” The response I got has been seared inside my brain ever since. His exact words were spoken with complete and total confidence;

_“That will never happen”_

This was a senior executive who had been at the company for years, knew the company’s business processes, and was chosen to oversee the software for a reason. The ability to override the default terms and conditions was explicitly requested by the same person. Who the heck was I to question anyone, much less a senior executive of a company that was paying us money to build this product? I shrugged it off and promptly forgot about it.

Months later, just a few weeks before the software was to go live, a tester on the client side had found a defect, and it was assigned to me. When I saw the details of the defect, I laughed out loud.

That concern I had about overriding default terms and conditions, the thing I was told would never happen? Guess what was happening? Guess who was blamed for it, and who was asked to fix it?

The fix was relatively easy, and the consequences of the bug were low, but this experience has been a recurring theme in my career building software. I’ve talked to enough fellow software engineers to know I’m not alone. The problems have become bigger, harder to fix, and more costly, but the source of the problem is usually the same: the requirements were unclear, inconsistent, or wrong.

![](https://cdn.stackoverflow.co/images/jo7n4k8s/production/a8b0ba6fe14b1bdc9aa4e8556eacced5f30f18c7-592x217.png?auto=format)

## AI right now: Chess versus self-driving cars

The concept of artificial intelligence has been around for quite some time, although the high profile advances have raised concerns in the media [as well as Congress](https://www.npr.org/2023/05/15/1175776384/congress-wants-regulate-ai-artificial-intelligence-lot-of-catching-up-to-do). Artificial intelligence has already been very successful in certain areas. The first one that comes to mind is chess.

AI has been applied to chess as far back as the 1980s. It is widely accepted that AI has exceeded human’s ability to win at chess. It’s also not surprising, as the parameters of chess are FINITE ([but the game has not yet been solved](https://chess.stackexchange.com/questions/13522/is-chess-a-solved-game)).

Chess always starts with 32 pieces on 64 squares, has well documented officially agreed upon rules, and most importantly has a clearly defined objective. In each turn, there are a finite number of possible moves. Playing chess is just following a rules engine. AI systems can calculate the repercussions of every move to select the move most likely outcome to capture an opponent's piece or gain position, and ultimately win.

There has been another front where AI has been very active - self driving cars. Manufacturers have been promising self-driving cars for quite some time. Some have the capacity to self-drive, but there are caveats. In many situations the car requires active supervision; the driver may need to keep their hands on the wheel, the self-driving feature is not autonomous.

Like chess-playing AI programs, self-driving cars largely use [rules-based engines](https://www.oreilly.com/radar/podcast/the-technology-behind-self-driving-vehicles/) to make decisions. Unlike the chess programs, the rules on how to navigate every possible situation are not clearly defined. There are thousands of little judgments drivers make in a given trip avoiding pedestrians, navigating around double-parked cars, and turning in busy intersections. Getting those judgments right means the difference between arriving at the mall safely or arriving at the hospital.

In technology, the standard is [five or even six 9s for availability](https://www.skmurphy.com/blog/2009/09/01/achieving-six-nines-when-you-launch/)—a website or service is available 99.999% (or 99.9999%) of the time. The cost to achieve the first 99% isn’t that high. It means that your website or service can be down for more than three days—87.6 hours—a year. However for each 9 you add at the end, the cost to get there grows exponentially. By the time you reach 99.9999%, you can only allow for 31.5 seconds of downtime a year. It requires significantly more planning and effort and of course is more expensive. Getting the first 99% may not be easy, but proportionally it’s a lot easier and cheaper than that last tiny fraction.

365 X 24 X 60 minutes = 525,600 minutes a year

99% availability -> down for 5256 minutes, 87.6 hours 99.9% availability -> down 526 minutes, 8.76 hours 99.99% -> 52 minutes, less than 1 hour 99.999% -> 5.2 minutes 99.9999% -> 0.52 minutes, roughly 31.5 seconds

No matter how close AI gets to being good enough, there’s always the risk of accidents and fatalities. Those risks and consequences happen every day with humans behind the wheel. I don’t know what rate of accidents and fatalities will be acceptable by governments, but you have to think it needs to be at least as good as human beings.

The reason it’s so difficult to get that acceptable level of safety is because driving a car entails significantly more variables than chess, and those variables are NOT FINITE. The first 95% or 99% might be predictable and easy to account for. However, there are so many edge cases after that first 99%, and each one may share some traits but each one is unique; other vehicles on the road driven by other human beings, road closures, construction, accidents, weather events, How many times have you driven after a road has been paved over but the paint for the dividing lines on the road has not been applied. It’s significantly harder to get your AI model to be able to account for and recognize those anomalies and edge cases, and more importantly how to respond appropriately without getting into an accident. Each edge case may share some traits, but rarely are they identical, which makes it harder for AI identify the appropriate way to respond.

## AI can’t create software, only code

Creating and maintaining software has a lot more in common with driving than playing chess. There are far more variables involved and the rules are based on judgment calls. You may have a desired outcome when you are building software, but it’s unlikely that it's as singular as chess. Software is rarely done; features get added and bugs are fixed; it’s an ongoing exercise. Unlike software, once a chess game is won or lost it's over.

In software development, we do have a tool to get our software designs closer to the tightly-controlled rules engine of chess: [technical specifications](https://stackoverflow.blog/2020/04/06/a-practical-guide-to-writing-technical-specs/). At their best, specs walk through expected user behaviors and program flows. Here’s how a user buys an e-sandwich: click this button, create this data structure, run this service. However, that’s rarely what we get. Too often, we’re handed wishlists as feature specs, back-of-the-napkin wireframes, and unclear requirements documents and told to make our best judgments.

Worse yet, requirements change or are ignored. Recently I was asked to help a team build something that could help people get information on health issues related to COVID 19. The application was going to be for an area of the globe that did not have reliable WIFI. The team wanted me to help build an application that could do surveys via SMS—phone text messages. Initially I was excited to be involved.

Once I started hearing the team describe what they thought they wanted, I realized this was going to be a problem. It’s one thing for a retail company to ask you on a scale of 1-10 how likely you are to shop in their store again. It’s very different to ask multistep surveys with multiple choice questions about the symptoms you’re experiencing with a possible COVID infection. I never said no, but I did bring up all the possible points of failure in this process and wanted the team to clearly define how we would handle incoming answers for all questions. Would it be comma separated numbers mapped to each answer? What happens if a submitted answer does not map to any of the options given?

After all these questions, the team came to the same conclusion. We decided it would be best not to go through with it. Believe it or not, I'd say this was actually a successful outcome. It would have been more wasteful to have gone ahead without a clear resolution for all of the potential errors when invalid user data was submitted.

Is the idea behind using AI to create software to just let those same stakeholders talk directly to a computer to create a SMS based survey? Is AI going to ask probing questions about how to handle all the possible issues of collecting survey data via SMS? Is it going to account for all the things that we as human beings might do incorrectly along the way and how to handle those missteps?

In order to produce a functional piece of software from AI, you need to know what you want and be able to clearly and precisely define it. There are times when I'm writing software just for myself where I don't realize some of the difficulties and challenges until I actually start writing code.

Over the past decade, the software industry has transitioned from the waterfall methodology to [agile](https://stackoverflow.blog/2023/06/13/the-meeting-that-changed-how-we-build-software-ep-579/). Waterfall defines exactly what you want before any code is written, while agile allows enough flexibility so you can make adjustments along the way.

So many software projects using waterfall have failed because the stakeholders thought they knew what they wanted and thought they could accurately describe it and document it, only to be very disappointed when the final product was delivered. Agile software development is supposed to be an antidote to this process.

AI might be best suited to rewrite the software we already have but need to rewrite it to use newer hardware or a more modern programming language. There are still a lot of institutions with software written in [COBOL](https://stackoverflow.blog/2020/04/20/brush-up-your-cobol-why-is-a-60-year-old-language-suddenly-in-demand/), but there are fewer programmers learning how to use it. If you know exactly what you want, maybe you could get AI to produce software faster and cheaper than a team of human programmers. I believe AI could create the software that has already been created faster than human programmers but that's because someone figured out what that software should do along the way.

AI might actually do pretty well building software using the waterfall process, which is also affectionately known as death march. You know who is terrible at waterfall? We are, human beings. And it's not because of the part where the signed documents are handed over to a team of programmers so they can write the code. It's everything before that. Artificial intelligence can do some extraordinary things, but it can’t read your mind or tell you what you should want.
