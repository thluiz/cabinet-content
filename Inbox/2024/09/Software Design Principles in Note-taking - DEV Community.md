---
created: 2024-09-06T14:30:35 (UTC -03:00)
tags: [learning,productivity,software,coding,development,engineering,inclusive,community]
source: https://dev.to/devsatasurion/software-design-principles-in-note-taking-3p6h?ref=dailydev
author: 
---

# Software Design Principles in Note-taking - DEV Community

> ## Excerpt
> Those of us who've been writing code for some time have likely applied these design principles in our...

---
Those of us who've been writing code for some time have likely applied these design principles in our work. These help our code become clear and readable - maintainable. It helps collaborators (and your future self) better understand your creation.

I started to entertain strategies for note-taking when I realized that it was time to stop dumping all of my notes into Notepad and start compiling and organizing them into something that sentient beings could understand. However, those that I've read were mostly geared towards capturing ideas (for book writing, graduate studies, etc.)

And so I thought, "I'm familiar with principles to write clean code... could I apply those to my notes, maybe?" I've found success in translating these into note-taking. As I share them with you, I hope you do too.

___

## [](https://dev.to/devsatasurion/software-design-principles-in-note-taking-3p6h?ref=dailydev#single-responsibility-principle)Single Responsibility Principle

> _"A class, method, or function should only have one responsibility."_

In line with it's traditional definition, each note or page should contain only one focal idea or purpose. The main goal: don't dump everything on a single document. Give each major component an island of their own.

Using application support as an example - responsibilities include supporting weekly requirements and troubleshooting of reported issues. Respectively, I've organized my notes as such:

-   **Deployment Steps** - a detailed, step-by-step guide on how to do the deployment. Screenshots and all.
-   **Troubleshooting** - the first aid kit. A compilation of valid, known, and even weird issues encountered and how they were solved. This comes in handy especially when someone else performed the resolution.
-   **Overview** - the page that ties them all together. Contains top-level information like the AWS account and region, access requirements, relevant URLs, plus it references the Deployment Steps and Troubleshooting pages.

Looks something like this:

[![SRP visual](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpe8h0r7re8y6hgjgobw6.jpg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpe8h0r7re8y6hgjgobw6.jpg)

You might think that having separate pages for each topic would be counterproductive in the long run, but it's actually the opposite. I know exactly where something is. No more guessing which piece contains what information. It also makes everything shareable and easier to build on to.

___

## [](https://dev.to/devsatasurion/software-design-principles-in-note-taking-3p6h?ref=dailydev#openclose-principle)Open-Close Principle

> _"Entities should be open to extension, but closed to modification."_

Chain your notes together, but don't weld them. Build them up as if you were making notes using pen and paper - give each idea or article, no matter how small, it's own space to grow. Then make use of links to connect the dots, like the choose-your-own-adventure books of old.

[![charlie conspiracy meme](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fszmbu5yj3aju6j6f077p.jpg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fszmbu5yj3aju6j6f077p.jpg)

It could look something like this - let's say you have an application issue that's been a pain point for quite some time. And so, you add it to your wish list of improvements. Fast forward in time, you're writing about what's shiny and new in the enhancement's you've made and the story behind it.

[![OCP visual](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyze37emtoax9ic5pwpez.jpg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyze37emtoax9ic5pwpez.jpg)

___

## [](https://dev.to/devsatasurion/software-design-principles-in-note-taking-3p6h?ref=dailydev#dont-repeat-yourself)Don't Repeat Yourself

> _"Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."_

Your mileage may vary on this one as it depends on your use case. But the idea is to link your notes whenever you can.

One possible use of this could be: you encounter an issue or error that has the same nature as one that you've encountered and/or resolved before. When you get around to documenting, instead of writing everything down again from scratch, you could just make a reference to the existing document.

___

## [](https://dev.to/devsatasurion/software-design-principles-in-note-taking-3p6h?ref=dailydev#bonus-single-source-of-truth)Bonus: Single Source of Truth

> _"There should be one definitive data repository that all users and systems refer to."_

This is more a personal preference of mine. Having a "master copy" of my notes in one place eliminates the mental load of having to keep track of what is where. Gone are the days of _Did I have that in Confluence? OneNote? Or was it in Slack Canvas?_ 😬

I like to use Obsidian. All of the notes I keep and its revisions go through there first before being shared anywhere else. I find it convenient to have the updated copy within reach, as the ones in Confluence or even GitHub can fall behind when I jump from project to project.

[![SSoT stock image](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyacc4mkqovxwdpvfl7mh.jpg)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyacc4mkqovxwdpvfl7mh.jpg)

**But!** Having a single source of truth does not mean that you should limit yourself to a single tool. Go with whichever you fancy the most, but there's no one app to rule them all; use the right tool for the right job. I use a combination of Logseq and Obsidian as they both use markdown formatting - Obsidian excels in long-form notes whereas Logseq excels in short-form (bulleted, quick notes and to-do lists).

___

Note-taking is not much different from any other skill we learn. And it's tools are the equivalent of an IDE. Pick the one that works with your style and you end up with a second brain that works for you.

###### [](https://dev.to/devsatasurion/software-design-principles-in-note-taking-3p6h?ref=dailydev#related-zettelkasten-method)_Related: [Zettelkasten Method](https://zettelkasten.de/introduction/)_
