---
created: 2024-08-09T19:04:51 (UTC -03:00)
tags: []
source: https://blog.isquaredsoftware.com/2020/09/coding-career-advice-daily-work-journal/?ref=dailydev
author: Collector of interesting links, answerer of questions
---

# Coding Career Advice: Keeping a Daily Work Journal · Mark's Dev Blog

> ## Excerpt
> First in a series of tips on things I've found useful in my career as a dev. Starting topic: the value of daily notes on work progress

---
This is a post in the [**Coding Career Advice**](https://blog.isquaredsoftware.com/series/coding-career-advice) series.

___

_First in a series of tips on things I've found useful in my career as a dev. Starting topic: the value of daily notes on work progress_

## Introduction

As I've progressed through my career as a developer, I've learned a bunch of useful things based on my experiences. I've frequently shared some of these thoughts with other developers on my team and various folks I've chatted with online, so it's worth trying to write these down so I can share them more widely.

None of these thoughts are particularly new, and in fact I'll try to link to other resources that say similar things. But, writing them down means I've got somewhere to point people to in the future, and maybe they'll be [new to someone who hasn't seen these thoughts before](https://xkcd.com/1053/).

> **Note**: For a much more extensive set of career advice on many topics, I strongly recommend Shawn Swyx Wang's book [**The Coding Career Handbook: Guides, Principles, Strategies, and Tactics from Code Newbie to Senior Dev**](https://www.learninpublic.org/). There's a _ton_ of valuable info in there, and it's totally worth the purchase.

## Tip #1: Keeping a Daily Work Journal

At the start of 2013, I started keeping a daily work journal. **At the end of every workday, I take 10-15 minutes to write down a few paragraphs covering what I did that day**, such as:

-   Tasks I worked on and what specific things I did for those
-   Conversations with other developers on my team and what topics we covered, or key takeways from discussions outside the team
-   Problems I ran into, and (hopefully) how I solved them
-   Where I left off at the end of the day
-   What tasks I need to prioritize the next day
-   Useful code snippets

I don't hit all of those points every day, but they're all common things I cover.

### Why Keep a Work Journal?

Writing journal entries like this has a lot of potential benefits:

-   When I pick up work after a long weekend, it reminds me what I'm even supposed to be working on now
-   I sometimes run into a bug that I _know_ I solved months or even years ago. There's been several times I dug back into my notes and found the solution I'd written down the last time.
-   It's often interesting to look back at how I approached problems years ago, and see how I've grown since then
-   But most of all, **it's _extremely_ valuable come performance review season. I can go back through every single workday of the past year, see exactly what tasks I worked on, and put together an extensive list of accomplishments for my self-evaluation writeup.**

### Journaling Tools

I strongly prefer to write notes in Markdown format, with actual syntax highlighting in the Markdown text and the rendered output.

I currently use [the "legacy" Boostnote desktop app (v0.16)](https://github.com/BoostIO/boost-releases/releases/tag/v0.16.1), although I may end up switching to the "modern" Boostnote app in the near future once it looks mature enough. Boostnote does a pretty good job handling Markdown and syntax highlighting. It also has decent search capabilities.

I've also been writing a lot of Markdown documentation in VS Code lately, and there's some notebook plugins for VS Code that look like they might be viable options as well.

You should feel free to use whatever works best for you, electronic or analog, although I would suggest something electronic because it's probably easier to search for text keywords.

### Journal Organization

I have a separate folder for each month, typically named like `2020-09 (September)`, and create a separate Markdown note for each day, named like `2020-09-21 (Monday)`. Legacy Boostnote sadly doesn't have nested folders, so all folders for all years are flattened out, but that makes it pretty easy to jump straight to a period of time that I'm interested in. (Looks like new Boostnote does support nested folders, so that may be a reason for me to switch.)

I also have a separate parent category for other notes organized by topics.

### Example Journal Entry

Here's a very slightly anonymized version of a real journal entry I wrote recently:

> ### 2020-09-03 (Thursday)
> 
> Talked to $DEV\_1 and split the remaining "UI polish" tasks up.
> 
> Fixed an issue with input cells not focusing in FF. Turns out FF doesn't trigger click events on disabled inputs. Had to apply `input[disabled] { pointer-events: none; }`, per Stack Overflow.
> 
> I tried to get sticky table columns to work, and bounced off that. Too tough for now, so we'll put that for later.
> 
> Had a good talk with $UX\_DESIGNER. He's been busy the last few weeks with other projects, so we caught up on the state of our work. He's going to see if he can get a couple other UX folks to look at what we've got. He'll also think about the best way to display things like the couple dozen columns for $BUSINESS\_FEATURE.
> 
> I tossed out the idea of showing $FEATURE instruction help as a separate new popup window. He agreed that was viable.
> 
> Found `react-new-window`, pulled that in. Was able to copy and paste field definitions from Excel and convert to HTML via [Tableizer](https://tableizer.journalistopia.com/). Threw together help pages for the three $FEATURE tabs so far.
> 
> Reviewed $DEV\_2's PR that had removal of dead $FEATURE code, backend models converted to TS, a unit test for deleting $ITEM, and a bunch of other cleanup. Looks pretty good overall.
> 
> I'll try to knock off the remaining UI polish tasks next.

## Further Information

-   [Keep journals to become a better developer](https://dbader.org/blog/keep-journals-to-become-a-better-developer)
-   [Getting started with a code journal](https://dev.to/jacquibo/why-you-should-keep-a-code-journal-code-journaling-pt-1-of-4-k35)
-   [Tools for Brainstorming and Tracking Accomplishments](https://www.livecareer.com/resources/jobs/search/tracking-accomplishments-tools)
-   [The More Senior Your Job Title, the More You Need to Keep a Journal](https://hbr.org/2017/07/the-more-senior-your-job-title-the-more-you-need-to-keep-a-journal)
    -   [HN discussion thread](https://news.ycombinator.com/item?id=23768624)

___

This is a post in the [**Coding Career Advice**](https://blog.isquaredsoftware.com/series/coding-career-advice) series. Other posts in this series:

-   Jan 27, 2021 - [**Coding Career Advice: Using Git for Version Control Effectively**](https://blog.isquaredsoftware.com/2021/01/coding-career-git-usage/)
-   Nov 23, 2020 - [**Coding Career Advice: Searching and Evaluating Online Information Efficiently**](https://blog.isquaredsoftware.com/2020/11/coding-career-searching-information/)
-   Sep 21, 2020 - [**Coding Career Advice: Evaluating Software Libraries and Tools**](https://blog.isquaredsoftware.com/2020/09/coding-career-advice-evaluating-libraries-tools/)
-   Sep 21, 2020 - **Coding Career Advice: Keeping a Daily Work Journal**
