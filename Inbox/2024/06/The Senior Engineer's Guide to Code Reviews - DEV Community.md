---
created: 2024-06-13T13:02:48 (UTC -03:00)
tags: [codereview,productivity,learning,testing,software,coding,development,engineering,inclusive,community]
source: https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev
author: 
---

# The Senior Engineer's Guide to Code Reviews - DEV Community

> ## Excerpt
> Code reviews.   You know how important they are.  They are one of the pillars of getting...

---
## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#code-reviews)Code reviews.

You know how important they are.

They are one of the pillars of getting reliable code out there.

Yet, it’s one of those things you need to **squeeze** out some time for in your super busy days.

If you’re not reviewing code, you might as well ship landmines to your users because you never know when it’ll blow up. 🤷

Obviously, you know that. You’re not here to be told “Hey! You should have code reviews! It’s a vital thing!”

## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#my-team-already-does-reviews-why-should-i-care)My team already does reviews. Why should I care?

Code reviews processes handled without care and diligence can have serious consequences.

At one of my previous orgs, code reviews were often not done thoroughly, and hence needed multiple passes. They were also done by reviewers on the opposite ends of the earth! 🌏

So addressing any comment took almost a whole day. And again, because the reviews would usually not be comprehensive the rework time on a PR would be in days for trivial things.

[![Cycle time breakdown of a team](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuvws8nxa2kjdvx6034n1.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuvws8nxa2kjdvx6034n1.png)

“You can’t improve, what you don’t measure”

_Often attributed to Peter Drucker, but I’ve not been able to actually find evidence for that._

But it’s a statement I found to be profound in my experiences.

I’ve made a case to my leadership in the past to make much needed organizational changes to enable all teams to have fewer inter-dependencies across time-zones to enable people to collaborate faster.

I understand how difficult it can be to do so, but it’s even harder to get any change in motion without a solid data-backed reason for why it’s needed.

_P.S.: That’s part of why Dhruv & I started [Middleware](https://www.middlewarehq.com/). 🚀_

## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#okay-i-hear-ya-what-are-my-options)Okay, I hear ya. What are my options?

What you ideally want are code reviews that are done thoroughly, which is to say that obvious red flags, performance or security defects, or other hard-to-read code shouldn’t go unnoticed.

But you also want all of this to happen in a reasonable amount of time.

Well reviewed code merged in a reasonable amount of time, means your team's delivery predictably, and with high reliability.

Only if there were a well researched, structured way of getting a grip on this. 🤔

…

Have you heard of… DORA metrics?

Okay, this isn’t another one of “DORA GOOD!” articles.

These are my experiences of how keeping an eye on the four-keys (as [explained](https://dora.dev/guides/dora-metrics-four-keys/) by the awesome [Nathen Harvey](https://www.linkedin.com/in/nathen/)) helped me improve the code delivery experience for myself and my team in the past.

[![DORA Metrics](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fb49lb17w68bs5fojske7.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fb49lb17w68bs5fojske7.png)

_DORA Metrics as seen on Middleware Open Source_

## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#exploring-code-reviews-with-dora)Exploring Code Reviews with DORA

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#how-long-reviews-inflate-lead-time)How Long Reviews Inflate Lead Time

Long review cycles directly impact Lead Time for Changes.

Lead time consists of basically 5 parts.

1.  Time from first commit to the PR being open
2.  Then the PR receiving its first review (could be a comment, change, approval)
3.  Time spent on making changes to the PR, till it’s finally approved
4.  Time between approval and the PR being merged
5.  Time when the PR was eventually deployed

Naturally, any of the parts taking time will inflate your team time. But there are 2 parts that are particularly egregious factors for delays here. That’s #2 and #3.

**#2. Time till the PR receives its first review (First Response Time)**

After the PR is open, a dev can’t really do much on it. The PR may be totally good to go! It may need solid changes. At this point, only a review will tell. This is also the point when a dev may not be able to pick up more tasks either because technically a review could happen at any time, and they would suffer from context switching.

**Context switching is one of the biggest productivity killers for devs.**

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#the-misleading-focus-on-time-per-review)The Misleading Focus on "Time per Review"

This talks about the third sub-part of the Lead Time metric.

**#3. Time spent on making changes to the PR (Rework Time)**

The real problem here isn’t stemming so much from how much time was spent here, but how many times back and forth happened. Let’s call that “Rework cycles”.

Because if there was only 1 rework cycle because the PR was approved, then it could still have taken a long time before approval, but it was actual implementation time, not idle time. This kind of rework could be mitigated by better training, codebase onboarding, context sharing, etc.

But… if you’re going back and forth a lot of times, then each of these cycles has some idle time associated with it, much like first response time.

During this time, the dev can’t pick up new work, because that would inevitably result in rapid context switching.

This is likely to happen when the PR is too large to review in one go, or the reviewer didn’t review thoroughly for other reasons. This is especially exacerbated when the author and reviewer are in far apart time-zones. Because each review, and rework is likely to happen during the work hours in their respective time-zones, inflating the time before the reworked changes can be checked into many many hours.

**This is a snow-ball effect**

The more PRs get blocked like this, the slower the teams deliver. And often the new work doesn’t stop coming, so that makes it even more challenging for devs to manage and estimate their work accurately.

If this keeps happening constantly, it also deals a blow to the morale of the team.

**tl;dr**

Focusing solely on reducing "time per review" can backfire.

The goal should be to optimize the review process without sacrificing thoroughness, ensuring each review adds real value.

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#subpar-reviews-and-change-failure-rates)Subpar Reviews and Change Failure Rates

Teams operate under pressure and tight deadlines all the time. And it’s unreasonable to expect that to magically change. But it’s also unrealistic to think that corners won’t be cut to ensure things don’t get shipped on time.

Since we’re talking about code reviews, one of the corners that are cut often, are:

1.  Large PRs created that contain all the code for a feature instead of well contained smaller and easier to review PRs.
2.  PRs are reviewed by just skimming over them because the reviewer may just not have the mental capacity or time to deal with it properly at the moment.

Both of those things happen from time to time. Devs are humans too. You won’t solve this by just blaming it on them or strong-arming them into reviewing “properly”.

The most important thing is for you to know that it’s happening in the first place. Because then you can do something about it. How would you know about it, you ask?

1.  Your Lead Time should be going down. Because reviews are being done faster (often than they should)
2.  Your Change Failure Rate might be going up. Of course, with subpar reviews you’re likely shipping more bugs.
    1.  But, even if your CFR isn’t going noticeably down, your team might still be shipping low performance or quality code that would bite you back later, and will likely show up as higher Lead Time down the line. But by then it’ll be too difficult to correlate with the reviews of today.

**This is a good time to mention that DORA is a great guide, but it’s not perfect.**

Don’t treat it like a definitive rule-book. Don’t measure individuals against it.

Use it holistically for your team, but also be involved to make sure it’s actually helping your team. That’s the goal after all, isn’t it?

## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#great-how-strategies-for-faster-more-effective-reviews)Great! How? 👉 Strategies for Faster, More Effective Reviews

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#heres-a-quick-prereview-checklist)Here’s a quick **pre-review checklist**

1.  Tests: Ensure all relevant tests are written and passing ✅.
    1.  This can be done by a CI bot (or Github Actions)
2.  Documentation: Update relevant docs, including inline comments and README files.
3.  Clear Commit Messages: Write descriptive commit messages that explain the 'why' behind changes.
    1.  This could also be enforced via [commit-lint](https://commitlint.js.org/)
    2.  You could also use [aicommit](https://github.com/Nutlope/aicommits) to help write good and detailed commit messages!
    3.  My team often uses GH Copilot to create commit messages that actually end up being totally satisfactory to me!

Example commit message:  

```

feat: add user authentication

- Implemented OAuth2 for secure login

- Added unit tests for authentication flows

- Updated API documentation with new endpoints

```

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#right-reviewer-right-time)Right Reviewer, Right Time

Match reviewers to their expertise and current workload to avoid overload. Complex changes go to senior devs, simpler ones to peers.

But you also need to be aware of how much context a dev has of a specific codebase.

There’s a few challenges here:

-   If your devs are highly specialized within singular specific repos, then it’ll be pretty difficult to use their skills on a separate codebase simply due to the required time to onboard and share context.
-   If your devs are too generalized over all codebases, it might be difficult for them to solve certain issues faster due to a lack of deep context of specific codebases.
-   If one of the devs on the team has a lot of context about things, it’s super easy to overburden them. You need to make time to distribute context sooner than later, so your work doesn’t get blocked at a time when it’s most critical.

You want to ensure you have a mix of both, and that could be achieved with as few as 2-3 devs that you work with.

[![Visualizing bottlenecks in a team](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fva7uc3ajyb17kiamhq0h.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fva7uc3ajyb17kiamhq0h.png)

_Understanding who gets blocked on whom for code reviews is crucial. You don’t want your team to not deliver at all because someone needed to be on leave._

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#tools-of-the-trade)Tools of the Trade

Use static analysis, code linting, and automated checks to catch simple issues before human review. This lets reviewers focus on more complex feedback.

Example Tools:

-   [ESLint](https://eslint.org/): JavaScript linting.
-   [Husky](https://typicode.github.io/husky/): For running pre-commit checks and static analysis.
-   CI/CD Pipelines: Automated testing and build processes.

**Super important tip:**

It’s easy to lose a LOT of time arguing over spaces and tabs, semicolons or not, trailing newlines.

But all that doesn’t matter.

Decide on, and agree with whatever code-style the team finalizes, and enforce them as part of the linter rules.

This stuff isn’t worth your time. 👍

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#the-art-of-feedback)The Art of Feedback

Give actionable, specific comments that focus on improvement, not nitpicking. Avoid vague statements and offer clear guidance.

Share how a file could have been restructured into multiple, along with why doing that is a good idea.

Share why making that DB call multiple times in a loop might be a bad idea because of reasons I’m sure I don’t need to explain here. 😆

If the nitpicks are largely things that could have been handled by a linter, then use one of those.

People hate reviews that mostly have only nits. But again, poor variable names, typos, etc. can’t just go to prod! 😁

Example:  

```

# Ineffective comment

"Fix this."

# Effective comment

"Consider using a map here to improve lookup efficiency. This will reduce the time complexity from O(n) to O(1)."

```

## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#streamlining-the-process-with-middleware)Streamlining the Process with Middleware

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#how-middleware-helps)How Middleware Helps

I’m able to see specifically where my teams get stuck, why, and how I can unblock them.

That’s kind of half of my job, and now I’m able to do this stuff a lot faster than before!

[![Process overview of a team](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyl0k380fegaussrrckyd.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyl0k380fegaussrrckyd.png)

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#heres-a-few-things-i-focus-on)Here’s a few things I focus on:

-   Review Metrics: Track how long reviews take and identify where delays occur.
-   Process Insights: Gain visibility into the entire review process and find areas for optimization.

I won’t get too much into that because then it’ll sound like a sales pitch! 😂

## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#beyond-technicalities-the-human-element)Beyond Technicalities: The Human Element

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#fostering-a-culture-of-constructive-feedback)Fostering a Culture of Constructive Feedback

Promote a culture where feedback is seen as a growth opportunity. Constructive, respectful communication helps improve code quality and team morale 💬.

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#balancing-speed-with-thoroughness)Balancing Speed with Thoroughness

Balance speed with thoroughness. Quick reviews shouldn't compromise scrutiny, and thorough reviews shouldn't drag on.

### [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#psychological-safety)Psychological Safety

Ensure psychological safety for both reviewers and authors. Encourage open discussions and address mistakes without blame, fostering an environment of continuous improvement 🌱.

Remember, people often go guards-up when you’re sharing feedback for improvement. Be considerate, and clear.

## [](https://dev.to/middleware/the-senior-engineers-guide-to-the-code-reviews-1p3b?ref=dailydev#conclusion)Conclusion

Effective code reviews are crucial for maintaining code quality and delivery speed. By aligning with DORA metrics, using the right tools, and fostering a constructive feedback culture, teams can streamline their review processes. Embrace these practices to make your code reviews both efficient and impactful.

_Try [Middleware](https://www.middlewarehq.com/) to gain deeper insights into your code review processes and identify areas for further improvement. 🚀_

Again these are just guidelines and how we look at code reviews. Do share how code reviews are done in your organization!

Code reviews play a vital role in overall product reliability. There are instances of bad code reviews (not lousy code!) causing negative brand impact. To sum up, better code review processes contribute to less failure rate.

Frameworks like [DORA](https://github.com/middlewarehq/middleware) are designed to be light-weight to help the engineering team be productive without too much effort from engineers or even leaders. We at Middleware are on a mission to help engineering teams ship productive code. Do check out our [Middleware open-source](https://github.com/middlewarehq/middleware), our open-source DORA metrics solution, that is locally hostable. Consider giving us a star if you like it!
