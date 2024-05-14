---
created: 2024-05-13T19:21:56 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: Kent Beck
---

# 

> ## Excerpt
> Bad news: measuring engineering productivity is doomed Good news: there are effective ways to address the needs of all involved—engineers, managers, executives, & customers The desire to measure engineering productivity expresses a genuine human need. The problem is that trying to meet that need through metrics or even surveys inevitably poisons the data on which productivity measurement relies. Let’s dive into the structure of this dilemma before talking about the actual needs & how they can actually be addressed.

---
## Productivity Measurement as a Tradeoff

[![](https://substackcdn.com/image/fetch/w_80,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F000da410-0ed6-4a25-80b1-6a46e964ae0b_242x242.jpeg)](https://substack.com/profile/24333739-kent-beck)

Bad news: measuring engineering productivity is doomed

Good news: there are effective ways to address the needs of all involved—engineers, managers, executives, & customers

The desire to measure engineering productivity expresses a genuine human need. The problem is that trying to meet that need through metrics or even surveys inevitably poisons the data on which productivity measurement relies. Let’s dive into the structure of this dilemma before talking about the actual needs & how they can actually be addressed.

## Tradeoffs

Paying subscribers to Tidy First? receive weekly Thinkies, habits of creative thought I’ve collected. Lately I’ve been particularly interested in thinking in terms of tradeoffs. Tradeoffs have a surprisingly subtle structure, leading me to write a booklet diving into the details (also available to paying subscribers).

Measuring engineering productivity is an excellent example of a tradeoff. It has all the anatomical elements of a tradeoff:

-   A goal to be improved upon—reducing wasted effort.
    
-   A continuum of possibilities for addressing the goal—measurement from none to surveys to increasingly detailed metrics.
    
-   A response curve—how much waste you expect to eliminate for each of interventions.
    
-   A counter response curve—waste created because you went too far
    

Graphically, it can be rendered like this:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F50c42b05-08d3-47aa-833e-f66b49b3d243_1758x1420.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F50c42b05-08d3-47aa-833e-f66b49b3d243_1758x1420.jpeg)

Seems sensible—diminishing marginal returns & increasing overhead for greater measurement precision. Sensible but wildly wrong.

## Better Picture

The response curves:

-   Are the wrong shape.
    
-   Are out of proportion.
    
-   The counter-response curve in particular misses gigantic effects.
    

The potential gains from productivity improvement aren’t infinite. You have an organization now. You have customers now. You’re doing something. It works. Some.

If you kept improving for years, yes you might make dramatic improvements, but that will come through all kinds of interventions, not just measuring productivity. Measuring & responding may net a win but then the bottleneck will move elsewhere.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ce354d0-1734-4361-a0e7-bfe1b51152bc_1692x1367.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ce354d0-1734-4361-a0e7-bfe1b51152bc_1692x1367.jpeg)

Overhead & analysis remain. Who knows their magnitude compared to the potential efficiency gains. I’ll just make them similar. Draw your own curves for your own tradeoff.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F568518aa-c9f5-4440-8acc-2d008c587d46_1750x1020.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F568518aa-c9f5-4440-8acc-2d008c587d46_1750x1020.png)

As soon as metrics have consequences, gaming begins. Goodhart’s Law kicks in[1](https://tidyfirst.substack.com/p/productivity-measurement-as-a-tradeoff#footnote-1-140533956). The cost rises as people spend effort gaming the system instead of doing useful work. Also, the distortion of the measurement reduces any value to be derived.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06e0b80b-13da-49c8-8cb9-219b30c29053_1732x1006.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06e0b80b-13da-49c8-8cb9-219b30c29053_1732x1006.png)

Note that it’s hard to catch gaming, exactly because it’s intended to be invisible to operation of the metrics. How much effort do you want to expand catching cheaters instead of setting up a system where cheating doesn’t make sense?

Precise measurement rises further. As departments’ incentives diverge, the incentive to collaborate diminishes. Why would I make you look good if that’s going to make me look worse by comparison? Work that would have been done together for mutual benefit ceases.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4479a29c-7600-4fbf-b537-6112ef01c1b9_1670x1028.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4479a29c-7600-4fbf-b537-6112ef01c1b9_1670x1028.png)

Again, these costs are completely off the books, unaccounted for. Engineering just seems slow. I know! Moar mezzures!

## Alternative

The most surprising lesson for me about the whole “measuring engineering productivity” conversation has been trying to get a straight answer to the question of, “Why?” As an engineer I get why I would want to measure my own efforts—I want to improve & I can improve faster if I know what I’m currently doing. I have no incentive to game the system. I can balance how much I want to invest in doing my work versus analyzing how I do my work.

That’s not who McKinsey is talking to. The case that keeps getting quietly mentioned is a board meeting. Sales & Engineering have both asked for more budget. Sales says, “We get $1m of sales per person, each of whom costs $500K. Give us $20m & we’ll deliver $40m in additional revenue.” Engineering says, “Um, yeah, well, uh, we need more headcount mumble mumble technical debt mumble scaling mumble keeping the lights on.” Uncomfortable!

Another way this scenario is introduced is, “The CFO wants to know if they are getting their money’s worth from Engineering.” If I was the VP Engineering in that room, I’d want to be able to answer that question. The CFO has a genuine need. The VP Eng has a genuine need. “Measuring” “productivity” may seem to meet those needs, but at enormous cost.

What’s the alternative? Listening & thinking. The best executives I’ve worked with used data, yes, but they always had direct connection to the work. They had a diverse group of individual contributors they talked to on a regular basis. Trusted relationships let them identify misaligned & perverse incentives. It may seem like scaling requires interacting with abstractions like charts & graphs, but relying solely on abstractions puts you at the mercy of whoever creates those abstractions. They have their own agenda & it might not match yours.

### Subscribe to Software Design: Tidy First?

By Kent Beck · Thousands of paid subscribers

Software design is an exercise in human relationships. So are all the other techniques we use to develop software. How can we geeks get better at technique as one way of getting better at relationships?
