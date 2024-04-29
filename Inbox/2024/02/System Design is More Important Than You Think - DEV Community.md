This past month, I've been going all in on system design. After face-planting badly in one of my interviews (Temper your imposter syndrome by reading my previous [blog post](https://dev.to/dezhango/the-problem-with-system-design-interview-prep-55me) about my hilariously bad performance), I took the failure personally and decided to dedicate some serious time towards improving my system design knowledge and skills. If there's one thing I learned on this journey, it's that system design isn't just an interview prep thing, or something that only senior engineers should worry about. It's a vital skill that every software engineer should try to regularly practice and develop.

Story time: I launched [System Design Daily](https://systemdesigndaily.com/), an interactive study tool for myself mainly, but also other engineers who want a more engaging way to learn these concepts. I've recently added some gamification features that I'm pretty excited about, but that's a story for another article.

[![system-design-daily-progression](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftcna95gszncvltrs27j8.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ftcna95gszncvltrs27j8.png)

> _Hard-stuck bronze in both Valorant and system design interviews_

After launching the site, I got a whopping 3 users: me, my test account, and my partner who I definitely did not force into using it.

[![system-design-daily-users](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnok8tpii493wwf9e76rx.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fnok8tpii493wwf9e76rx.png)

> _Me checking my user numbers at launch_

It was time for some #digitalmarketing and #buildinginpublic. The only problem was I had about 14 followers on Twitter and about 4 posts in the span of 9 years. I'm not a big social media guy.

At first I went the Herbalife route and hit up all my software engineer friends and family, begging for some feedback on the site. This kind of worked, though one issue was that all my close friends and family were job-havers, and weren't actively interviewing. So this tool wasn't really of much use to them. (Hopefully by the end of this article I'll change their minds).

I decided to change up my strategy and talk to strangers on the internet instead. Instead of asking them to go use my site, I asked for stories and experiences. I began by hitting up my LinkedIn network, specifically looking for #opentowork folks like myself. I then expanded to Reddit and random Discord communities. And in speaking to people and reading people's comments and replies, I began to realize a few things about system design.

## [](https://dev.to/dezhango/system-design-is-more-important-than-you-think-g0e#the-job-market%C2%A0)The Job Market 💩🔥

Speaking to fellow unemployed brothers and sisters yielded two key realizations about the state of tech hiring in 2024:

1.  The job market is super competitive and terrible
2.  If you're a new grad - point 1, but like put it in big red text with flames and a skeleton for extra emphasis.

It's freaking brutal out there. Engineers are getting laid off left and right. Tons of people with lots of experience are flooding the market. Many of them are absolute Leetcode gods - I've seen some profiles with more Leetcode problems solved in a single day than I do in like a whole year.

What does that mean? It means that everybody is good at the coding interviews now. There are half a billion Youtube channels explaining Three Sum and Fast and Slow Pointers. Literally every question ever asked or ever will be asked is on Leetcode. As a result, the interview landscape is starting to change now that companies are starting to get wise to Leetcode grinders memorizing solutions, and, in some cases, system design and behavioral interviews are supplanting Leetcode-style interviews entirely. The bottom line is: if you are an engineer trying to compete, **Leetcode is no longer enough.**

For industry hires especially, you can't really afford not to be good at system design. At your level, system design and behavioral interviews are going to be the determining factor for whether or not you even get the job. System design in particular will be used to determine your seniority level. So it also serves you well to know this stuff if you want to make the big money and impress blind.com.

[![system-design-studier](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhi3rwpe0osn1qzzondxl.jpeg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhi3rwpe0osn1qzzondxl.jpeg)

> _The Average System Design Studier, circa 2024_

For you new grads out there, you might be thinking - well I'm off the hook right? Wrong! (kind of). While it's true that you won't normally be asked to showcase system design knowledge in your interviews, it's an extremely important long term skill to develop. That's because knowing system design will accelerate your career. I'll get into this in more detail, but long story short, you'll build up a design "sense" and systems knowledge that'll empower you to reach for more responsibility in your job. The end result is a fast track towards those senior / principal engineering roles with the giant paychecks.

And another thing for the new grads out there - While the system design reaper won't be knocking at your door for now, it's evil little brother **low level object-oriented design** most likely will. Sure, it might not operate at the same granularity, but it exercises the same overall skillset: you'll need to gather requirements, organize and describe your classes, and explain how they interact with each other.

## [](https://dev.to/dezhango/system-design-is-more-important-than-you-think-g0e#career-growth)Career Growth

So you might be thinking to yourself, "well, I have a job. Let those unemployed fellas worry about system design"

First of all, I admire your confidence in believing you'll have permanent job security - especially with the recent mass layoffs in tech and the rise of AI.

But secondly, I believe that system design can be useful even for you employed folks out there.

There's a reason why system design is used to hire for senior engineers. System design is a gauge of experience. The people who are best at system design have seen a lot of things and have built a lot of systems. They know what can go wrong because they've been there, and can design accordingly to mitigate potential problems.

But just how do you get to work on those systems? Sure, maybe your manager will trust you enough to just hand you large scale projects unprompted. But 99% of the time, you'll need to ask for more responsibility and ownership. And in order to do so with confidence, you'll need to prove that you understand things like architectural patterns, data storage technologies, and scaling strategies. In short, system design will help you go from junior to senior.

Ok, so what if you're already a senior or principal engineer? How does this help you? First of all, teach me pls. But second of all, technology is constantly changing, and it's easy to lose these skills if you don't keep them sharp.

When I worked at Amazon, one of the biggest things I was cautioned about was how easy it was to get isolated from the rest of the wider software world. When you always use the same tools and processes, you start to rely on their ability to do the right thing, and eventually forget why they're built the way they are in the first place. You also shut yourself off from new ways of approaching problems that you might not have thought of. Reviewing system design topics will also help you stay on top of the latest paradigms and technologies and will make you more adaptable.

## [](https://dev.to/dezhango/system-design-is-more-important-than-you-think-g0e#system-design-aint-leetcode)System Design Ain't Leetcode!

That brings me to my next point. System design is not like Leetcode. What do I mean by that? Well, there's a popular mentality among software engineers that "interview prep stuff" (which usually consists solely of Leetcode), is a thing that only sees the light of day when it's time to find a new job. Under normal circumstances, "House Robber II", "Merge K Sorted Lists", and the rest of the Blind 75 gang get buried under a foot of cement in the basement of your memory.

![john-wick-opening-guncase](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7o4vf5ydg0onm2kk1294.gif)

> _Engineers re-starting their Leetcode premium subscription be like_

To be honest, this is a perfectly fine way to approach Leetcode and DSA/coding interview prep in general. That's because most of the problems you'll be solving aren't things you'll encounter in your actual job. And what's more, the "whiteboard interview environment" is a completely unnatural setting contrived by large companies trying to filter out applicants. In no real world context will you ever have to produce an efficient implementation of an often obscure algorithm entirely from memory in under 40 minutes. So, like a pull-based CDN, you pretty much only retrieve this knowledge and skillset from object storage when needed.

This isn't a new idea at all. "Leetcode interviews" have been getting (rightfully) roasted by tech influencers for some time now. But I'll argue that while this "cold storage" mentality makes sense for Leetcode, it does not and should not apply to system design.

As I've mentioned before, system design actually has real world applications. You won't always have a ton of opportunities to design high level system architectures in your job. Your day to day will inevitably get crammed with non-design related work, so you need to exercise your design muscles with a healthy regimen of study and practice.

Furthermore, system components and design patterns are constantly evolving alongside the tech landscape as a whole. That means you need to keep up with them if you want to stay on top. Understanding system design gives you a framework to do that effectively.

## [](https://dev.to/dezhango/system-design-is-more-important-than-you-think-g0e#the-takeaway-system-design-is-dope)The Takeaway: System Design Is Dope

At the end of the day, knowing system design will make you a better engineer, plain and simple. Though it might just sound like I'm just saying that because I'm working on a system design study tool, I really believe that understanding how scalable distributed software systems work will make you better at building scalable distributed software systems.

I've also just found it fun and cool. Of course, the prescribed methods and resources for system design can feel like the opposite. But if you can get past the often dry and bland presentation format of the material, you might, like me, become captivated by how companies with insane scale deal with the unique, often unexpected problems that come with it. For all the gamers out there, it's not unlike playing one of those optimization or management simulator games (think Factorio, Two Point Hospital, etc.).

Anyways, I just think System Design is great and underrated and wanted to talk about this. That's about it. See ya.