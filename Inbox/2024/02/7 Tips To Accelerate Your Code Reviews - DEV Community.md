---
created: 2024-04-18T19:28:08 (UTC -03:00)
tags: [softwareengineering,codereview,productivity,careerdevelopment,software,coding,development,engineering,inclusive,community]
source: https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p
author: 
---

# 7 Tips To Accelerate Your Code Reviews - DEV Community

> ## Excerpt
> One complaint I often hear from software developers is about how long it takes to merge Pull Requests...

---
One complaint I often hear from software developers is about how long it takes to merge Pull Requests (PR). When I dig deeper, I often find it is all about code reviews. Indeed, code reviews can be a bottleneck, as they require someone to shift the focus from their work to review the PR. The key to faster code reviews is making reviewing PRs as easy as possible. There are several ways to achieve this.

## [](https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p#send-clean-prs)Send clean PRs

You shouldn’t expect to quickly merge a PR with red signals. If your code doesn’t compile, tests fail, or if there are linter warnings you might only hope that someone will notice and ask for a fix. Often this won’t happen. Instead, people will quietly ignore your PR thinking it was sent by mistake.

I posted a [post dedicated to this topic](https://dev.to/moozzyk/write-clean-diffs-to-accelerate-your-dev-career-2eic) recently

## [](https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p#write-a-good-commit-message)Write a good commit message

The struggle to find someone to review a PR is often a common reason why code reviews take so long. Good commit messages can help with that. A good commit message helps potential reviewers quickly understand if they are the right person to review the change and gives them an idea of how much time and effort the review will need.

Consider these two PR titles as an example: “Fixing a bug” versus “Fixing memory leak caused by retain cycle in X”. Which one do you think will catch more attention? The second title is more likely to attract reviewers because of its specificity. It tells them what the problem is and where in just a few words.

If you would like to learn more, I wrote a detailed post on this very subject [here on dev.to](https://dev.to/moozzyk/write-good-commit-messages-to-accelerate-your-dev-career-2mjf)

## [](https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p#ensure-good-test-coverage)Ensure good test coverage

“Can you add tests?” might be the first, and the only comment you get on your PR if you haven’t provided sufficient test coverage. It’s easy to understand why people hesitate to review PRs without good test coverage. Adding tests often uncovers issues or gaps in the code. Fixing them might lead to significant changes, that will trigger a complete re-review. Good test coverage not only saves time for the reviewer, who won’t need to go through your changes twice but also for you, as it reduces the number of necessary revisions.

## [](https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p#keep-your-prs-small)Keep your PRs small

Conducting a thorough review of a huge PR is time-consuming and requires significant effort. Not many team members can commit to such a task. However, most large PRs can be broken down into smaller, logically complete PRs. By doing this, you can distribute the effort to review your changes among more team members, shortening the time needed to merge them.

For a more in-depth discussion on this topic, check out my [post on LinkedIn](https://www.linkedin.com/posts/pawel-kadluczka_accelerate-your-software-engineering-career-activity-7127894071074816000-GO3A)

## [](https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p#address-comments-quickly)Address comments quickly

Respond to comments on your PR promptly, to keep the reviewer engaged while the details are still fresh in their mind. Delaying your response can lead to deprioritizing your PR as the reviewer will have to spend time and effort to recall necessary context. Addressing comments doesn’t always mean you need to send a new revision. It is often about providing clarifications or confirming assumptions. However, if the reviewer identifies an issue that requires a fix, you should promptly update your PR.

## [](https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p#ask-for-reviews)Ask for reviews

If you did everything possible to make your PR easy to review but it is still not getting traction, you may need to ask for a review directly. If anyone on the team can review your change, consider asking in your team’s chat room. If you would like a specific person to review your PR, contact them individually. However, be cautious not to overdo this. On most teams reviewing code is an expectation, and pinging people for reviews immediately after publishing a PR can be very distracting. In our team, we have a 24-hour rule for non-urgent PRs: we only ask for a review if a PR hasn’t been picked up within a day.

## [](https://dev.to/moozzyk/7-tips-to-accelerate-your-code-reviews-4d1p#review-your-team-members-prs)Review your team members’ PRs

If you don’t review your team members’ PRs, don’t expect them to prioritize yours. PRs that are constantly at the end of the queue will inevitably take longer to be reviewed, approved, and merged. That’s why it is important to dedicate a few minutes each day to code reviews. My rule is to review at least as many PRs as I publish. It works wonders - most of the time I can merge my PRs on the same day I submit them. Moreover, if my PRs aren’t getting enough attention, I don’t feel uncomfortable asking to review them.

And when you get your PR reviewed, [remember to promptly merge it](https://dev.to/moozzyk/why-you-should-always-merge-your-diffs-promptly-53lg).

What are your tips to speed up code reviews? I’d love to hear them.
