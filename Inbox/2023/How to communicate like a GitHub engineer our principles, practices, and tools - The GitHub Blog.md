---
created: 2023-10-09T09:59:22 (UTC -03:00)
tags: []
source: https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev
author: Ben Balter, Allison Matlack
---

# How to communicate like a GitHub engineer: our principles, practices, and tools - The GitHub Blog

> ## Excerpt
> Learn more about how we use GitHub to build GitHub, how we turned our guiding communications principles into prescriptive practices to manage our internal communications signal-to-noise ratio, and how you can contribute to the ongoing conversation.

---
As a company that’s been remote-first since day one, GitHub Engineering has learned a lot about how to communicate effectively across time zones, teams, and tools. We’ve distilled our experience into a set of guidelines that we call “How we communicate,” and [we’re sharing them with you today](https://github.com/github/how-engineering-communicates). We hope that by sharing our communication practices publicly, we can help other organizations that are embracing remote work or want to improve their collaboration culture.

Read on to learn more about how we use GitHub to build GitHub, how we turned our guiding communications principles into prescriptive practices to manage our internal communications signal-to-noise ratio, and how you can contribute to the ongoing conversation.

## Using GitHub to build GitHub[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#using-github-to-build-github)

Unlike many companies that made the transition to remote work during the pandemic, GitHub has been majority remote since its founding 15 years ago. GitHub’s remote-first communication style originally drew inspiration from the open source community. Open source development rarely requires the global community of collaborators and contributors to be in a certain place, at a certain time, in order to participate in the ongoing conversation. This is the same approach GitHub Engineering takes to our own internal communication. We believe that asynchronous communication is the best way to work globally and at scale, and as a result, we’ve built our culture around it.

We’ve [always](https://zachholman.com/talk/how-github-uses-github-to-build-github/) used GitHub to build GitHub. GitHub is not only the place where we host and review code, but also where we plan, discuss, and document our work. We use issues, pull requests, projects, and discussions to track work, collaborate on features, and share information across teams. Many of these communications patterns grew organically, as developers adopted practices from the open source community for our own internal collaboration needs. We believe open practices are the best way to work with a global and diverse team, and to make decisions that are informed, inclusive, and scalable.

While asynchronous collaboration is deeply embedded in GitHub’s DNA, we have also long had a culture of each team enjoying a great deal of autonomy in deciding how they communicate day to day. This freedom has allowed teams to experiment and uncover novel practices, but it has also meant that working across teams previously required first negotiating a meta-conversation around how to communicate before any substantive work could occur–much like a new open source project negotiating with its newfound community. Having an open set of shared expectations within the engineering organization allows us to be more effective, mindful, and inclusive about how and where we communicate, leading us to make more well-informed decisions in a way that takes into account different needs, preferences, and time zones.

## “How we communicate”[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#how-we-communicate)

To define this set of shared expectations, the GitHub Engineering Operations and Culture team collaborated with more than 100 people across the engineering organization in the first half of 2023 to create guidance on “How we communicate.” This document was intended to encourage consistency over preference by outlining a common core of shared internal communication practices for all of GitHub Engineering in the form of opinionated guidance. Teams are still encouraged to adapt the practices for their unique circumstances, while maintaining a common “API” to interface with other teams.

Today, we are [publishing our “How we communicate” guidance](https://github.com/github/how-engineering-communicates) under a [CC-BY-4.0 license](https://choosealicense.com/licenses/cc-by-4.0/), in the hopes that you’ll find it useful, especially if you’re evolving your own remote-first or remote-friendly culture; we welcome you to fork, modify, and use the documentation with attribution. We expect our guidance (lightly edited for the community, primarily to remove internal URLs and references) will evolve over time along with our organization, and, of course, pull requests are always welcome.

## From guiding principles to prescriptive practices[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#from-guiding-principles-to-prescriptive-practices)

To begin with our “How we communicate” guidance, we established eight guiding principles:

-   Be asynchronous first.
-   Write things down.
-   Make work visible and overcommunicate.
-   Prefer GitHub tools and workflows.
-   Embrace collaboration.
-   Foster a culture that values documentation maintenance.
-   Communicate openly, honestly, and authentically.
-   Remember, practicality beats purity.

From there, we began to define the specific practices that would help us live up to these principles. We started with the most common forms of communication, such as chat, discussions, issues, project boards, and pull requests, and went on to collaboratively author suggestions on how to manage notifications, run effective meetings, and schedule more inclusively.

## Managing the signal-to-noise ratio[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#managing-the-signal-to-noise-ratio)

With well over 1,500 engineers across a number of functions, we faced a challenge not unique to any organization: how to keep everyone informed and engaged without overwhelming them with notifications. We wanted to create a system that allowed everyone to opt-in, rather than opt-out, and to get the information they needed in a digestible and skimmable way. As it was, either you got everything (which we jokingly referred to as the “fire hose” of notifications), or you opted out entirely (and ignored everything). Either way, Hubbers were likely to miss important information. We set out to create a system that minimized notification fatigue, while allowing people to subscribe to the topics they cared about.

We rely on GitHub Discussions heavily to share information within and across teams. It’s a natural choice, since engineers are already working on GitHub.com, and with things like comments, upvotes, and emoji reactions, discussions are a great way to start an asynchronous conversation on just about any topic.

### Opt-in[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#opt-in)

To start, we encouraged teams to begin posting their discussions to the most logical repository, instead of directly to the main `github/engineering` repository. (For example, if a post was about GitHub Copilot, it should go in the `github/copilot` repository; if it was about GitHub Actions, it should go in the `github/actions` repository.) That way, those interested could subscribe to the repositories they cared about, and get email or web notifications when new discussions were posted. And the volume of notifications coming through `github/engineering` to the whole organization would be reduced.

### Amplify widely[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#amplify-widely)

But some posts are rightfully intended for all of GitHub Engineering. Things like staff ships (early access to new features for staff), required actions, promotions, and updates to Engineering priorities are written with a broad audience in mind. To ensure we were still surfacing the most important information to the organization, we established a small set of “magic labels” that if applied to a post, would add it to a daily content roundup, automatically amplify the message in various places for all of GitHub Engineering to see.

For a peek at our taxonomy, here’s an excerpt from our GitHub Actions workflow that makes it easy for everyone to add the set of “magic labels” to their repositories:

```
label:
          - name: eng-action-required
            description: Upcoming process/workflow changes/activities requiring Engineering Hubbers to take action
          - name: eng-availability
            description: Discussions about availability, incident response, et al
          - name: eng-celebrations
            description: Celebrating Hubber promotions and other amazingness
          - name: eng-feedback-request
            description: Posts requesting feedback from the Engineering organization
          - name: eng-org-change
            description: Announcements related to organizational changes
          - name: eng-priorities
            description: Discussions related to Engineering priorities
          - name: eng-roundup
            description: Newsletters, weekly digests, and other content and team roundups
          - name: eng-show-and-tell
            description: Share what you've learned or show off something you've made
          - name: eng-staff-ship
            description: Announcements for features made available to Hubbers for feedback and early access
          - name: eng-strategy
            description: Discussions related to strategy and vision
```

### Automate all the things![](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#automate-all-the-things)

We used GitHub Actions to schedule a workflow to automatically create daily and weekly roundups of activity across the organization based on those “magic labels,” posting the digests as discussion posts in the `github/engineering` repository.

![Screenshot of the GitHub Actions workflow in the eng-ops-automations repository that creates roundups of activity based on labels and posts them as discussions in the github/engineering repository.](https://github.blog/wp-content/uploads/2023/10/eng-ops-automation.png?w=1024&resize=1024%2C527)

Like any other discussion post, these content roundups trigger web and email notifications from GitHub.com, and they’re also amplified in Slack channels. However, rather than receiving multiple notifications a day, these roundups reduce the daily notification to one (and also make it much easier to catch up on everything that happens while you’re out of the office!). To support the needs of those who prefer receiving notifications for every discussion post individually, rather than waiting for a daily roundup (aka to instead “drink from the fire hose”), we created an `#engineering-discussions-firehose` Slack channel, which streams every labeled post as it is posted.

### Experiment with AI[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#experiment-with-ai)

With notifications reduced in our main `github/engineering` repository and discussions being posted in more logical repositories, enabling people to subscribe to more frequent notifications for specific topics, the last remaining step was to increase quick skimmability to allow for greater situational awareness without anyone having to spend all day reading teams’ discussion posts.

As part of our writing style, most of us include `TL;DR`s at the top of posts (internet slang for “too long, didn’t read,” a short summary of longer writing), but not every post author includes one. For posts that don’t have a human-authored TL;DR, we use Azure’s OpenAI service to draft a brief summary for us. That way, readers can quickly skim the daily digest (or fire hose) and decide if they want to click through to read more.

Here’s an excerpt of the prompt we use to summarize discussion posts:

```
// OpenAI
export const encodingModel = "gpt-3.5-turbo";
export const openaiModel = "gpt-35-turbo";
export const openaiPrompt = `
  The following is an internal discussion post from the engineering department at GitHub formatted in GitHub flavored Markdown. Please write a short summary appropriate for inclusion in a digest of internal discussion posts with the following requirements:

  - The summary should be no more than 3 sentences
  - The summary should focus on the most important and impactful information from the post, including key points and any calls to action
  - The summary should be detailed, thorough, to-the-point, and written for a technical audience, while maintaining clarity and conciseness
  - The communications style should be professional, but informal
  - The summary should use emoji where appropriate, but use emoji sparingly
  - The summary should be formatted in GitHub Flavored Markdown with no line breaks
  - DO NOT use the phrases "the engineering department" or "at GitHub"; instead, whenever possible, name the specific team in reference, or else use "we" to refer to the team or engineering department. For example, use, "We recently shipped a feature", and NOT, "The engineering department at GitHub recently shipped a feature".
  - Employees at GitHub are referred to as "Hubbers"
  - GitHub is ALWAYS capitalized as "GitHub", never "Github"
  - Teams are referred to as "the Actions team" or "the Copilot team", never just "actions team" or "copilot team"
`;

export const estimatedPromptTokens = 300;
export const completionTokens = 300;
```

Ironically, we relied heavily on GitHub Copilot to build the GitHub Actions workflow (it’s been a while since these Hubbers have written “production-worthy” code), meaning robots helped humans to teach robots how to summarize the work of humans, which other robots then published out to other humans. 🤖 Summarization is a core workflow for AI, and so far, while it’s not always perfect, it’s been working well. If you’re interested in the prompt we’re using (or want to help us improve it!), you can find it [here](https://github.com/github/how-engineering-communicates/blob/main/prompt.md).

## Let’s build from here[](https://github.blog/2023-10-04-how-to-communicate-like-a-github-engineer-our-principles-practices-and-tools/?ref=dailydev#lets-build-from-here)

We’re excited to share our “How we communicate” guidance with you, and we hope that it will inspire you to adopt or improve some of the practices we’ve found useful. Here are some suggestions to get you started:

-   **Principles**: Establish a set of guiding principles for your organization’s internal communications ([fork and clone our guidelines](https://github.com/github/how-engineering-communicates/fork) for a head start!). What core values do you want to promote, and how can you ensure everyone is aligned around those values so there’s a common “API” across teams?
-   **Practices**: Use those principles to develop practices. What specific practices can you adopt to help you live up to your principles, and how can you ensure those practices are adopted across the organization?
-   **Experimentations**: Experiment with automation and emerging technologies to improve your practices. How can you use AI and other tools (like GitHub Actions) to automate your workflows and improve the signal-to-noise ratio?

We recognize communication is an ongoing and evolving process, and different teams and cultures may have different needs and preferences. We welcome your feedback, suggestions, and contributions to our public repository: [https://github.com/github/how-engineering-communicates](https://github.com/github/how-engineering-communicates)

Happy communicating! 🎉

## Explore more from GitHub

![Company](https://github.blog/wp-content/uploads/2022/05/company.svg)

### Company

The latest on GitHub, from GitHub.

[Learn more](https://github.blog/category/company/)

![GitHub Universe 2023](https://github.blog/wp-content/uploads/2023/08/Icon-Circle.svg)

### GitHub Universe 2023

Get free virtual tickets to the global developer event for AI, security, and DevEx.

[Get free tickets](https://githubuniverse.com/)

![GitHub Copilot](https://github.blog/wp-content/uploads/2022/05/Copilot_Blog_Icon-1.svg)

### GitHub Copilot

Don't fly solo. Try 30 days for free.

[Learn more](https://github.com/features/copilot?utm_source=blog&utm_medium=bottomnav&utm_campaign=cta&utm_content=copilot)

![Work at GitHub!](https://github.blog/wp-content/uploads/2022/05/careers.svg)

### Work at GitHub!

Check out our current job openings.

[Learn more](https://github.com/about/careers)
