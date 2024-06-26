---
created: 2024-04-01T15:52:16 (UTC -03:00)
tags: []
source: https://graphite.dev/blog/github-monopoly-on-code-hosting?utm_source=tldrnewsletter
author: Written by
---

# How GitHub monopolized code hosting

> ## Excerpt
> How GitHub became a version control monopoly

---
![Edit How GitHub replaced SourceForge as the dominant code hosting platform](https://graphite.dev/_next/image?url=https%3A%2F%2Fwww.datocms-assets.com%2F85246%2F1711802484-screenshot-2024-03-30-at-8-41-15-am.png&w=3840&q=75)

I’ve been writing code since high school. I have faint memories of creating an Android game with a friend using [Tortoise SVM](https://tortoisesvn.net/) to share code. At college, I learned to clone GitHub repos to access CS homework assignments. Later, during internships, I joined the ranks using GitHub to review and merge PRs. Most developers who came to professional age during the last decade probably had a similar experience to me - GitHub is synonymous with source code and code changes, whether with open source projects or on private company teams.

It’s easy to take GitHub ubiquity for granted - but how did things get this way?

I asked my teammate, David, how he came to discover GitHub. He’s who’s been coding a decade longer than I have, and lived through the professional transition. Throughout the 2000’s, he recounted using SVN at programming jobs. He would download software from SourceForge but considered the UI utilitarian and “crappy.” Eventually, he found himself navigating to GitHub more and more often to find documentation and download open-source tools like [Rails](https://rubyonrails.org/). This lead him to read up about Git and eventually use a git-to-svn translator at work. But even up until 2010, many companies were still hosting code on SVN, and it wasn’t until years later that most private organizations migrated fully to Git.

David’s anecdote only piqued my interest further. How did GitHub enter the market? What existed before, and what gap did they fill?

## The world before GitHub[](https://graphite.dev/blog/github-monopoly-on-code-hosting#the-world-before-github)

Four years before GitHub was founded, in 2004, Linus created Git. While many folks still used SVN, Git was growing rapidly in popularity as a distributed version control system. It came with compelling value. Unlike prior tools like CVS and SVN, Git users could host entire copies of source code on their computers without needing to communicate to a centralized server. Folks could update code (even offline!) and share copies with one another without the approval (or hosting costs) from a centrally managed source. And while branching code in SVN required duplicating the entire repository, creating branches in Git was fast and cheap.

As one can imagine, these improvements led to an explosion of creative development in the open-source community. Git was custom-built for distributed democratized development, and spread virally. Despite its advantages, however, Git was slower to catch on to corporate codebases, which remained reasonably well served by private centrally managed SVN servers with legacy workflows.

Skip forward four years to 2008. Open-source projects like Rails were following Linux and were starting to adopt Git. Private organizations continued using SVN and Perforce servers to centrally manage source code. Open-source software was distributed primarily on SourceForge, later Google Code, or rarely on alternatives such as personally hosted servers.

SourceForge, despite its dominance, left much to be desired. For one, the site didn’t offer Git support until [2009](https://arstechnica.com/information-technology/2009/03/sourceforge-adds-support-for-new-version-control-systems/), over a year after GitHub was created, and had climbed to over 100k users. But the differences ran deeper than the technology. Today, when we think of online code hosting, we think of a website you can visit to browse the code, issues, and contributors seamlessly. That was not the SourceForge experience. Rather, it and alternatives like Google Code focused on _software distribution to end users_, not _code collaboration_. They solved half the puzzle by aggregating the distribution of open source projects - but offered little in the form of comments, code browsing, change reviews, and other modern table stakes features.

It gets worse. Creating and managing repositories on SourceForge was painful. For one, the site required an application and human approval to create a new repo. Furthermore, [private repos were never supported](https://sourceforge.net/p/forge/site-support/7229/#8326), meaning the site was never useful for closed-source hosting. Leaving comments and issues on projects was unintuitive, and forking was an uncommon practice. If you wanted to contribute to a project, the most common way was to generate a patch and then [send it over the project’s mailing server](https://news.ycombinator.com/item?id=14381076) - rather than forking and opening a pull request back to the project. Learning about SourceForge 20 years later, I'm struck by how similar it sounds to the current Apple App Store. In that light, the gap in the market GitHub could fill is more clear.

## The incumbents: SourceForge and Google Code[](https://graphite.dev/blog/github-monopoly-on-code-hosting#the-incumbents-sourceforge-and-google-code)

Check out SourceForge’s [landing page](https://web.archive.org/web/20080103101749/https://sourceforge.net/) back in 2008: What do you notice? Proud claims about being the largest open-source focused site on the web. Download numbers, and highlighted projects. New software version releases, even an advertisement on the right column. But no mention of Git, no focus on user profiles, and no private repos. The focus is on distribution, not developer collaboration.

![source forge repo screenshot](https://www.datocms-assets.com/85246/1711665044-untitled-65.png)

Example of a repo on SourceForge. Notice how the main call-to-action is a download button, rather than a clone button.

[code.google.com](http://code.google.com/) was incrementally better than SourceForge. The site focused on making it easy to host code and documentation for free. It leveraged Google’s great search acumen for discovery and offered thoughtful documentation for developers looking to learn. But critically, it lacked social and collaborative features which would later prove so powerful for open source developers. Lastly, while it launched with SVN support, Google Code didn’t add support for Git hosting until 2011 - complete with a sassy-titled [Wired article](https://www.wired.com/2011/07/google-code-finally-adds-git-support/).

![google code's old homepage](https://www.datocms-assets.com/85246/1711665036-screenshot-2024-03-28-at-1-07-28-pm.png)

## Finally, the invention of GitHub[](https://graphite.dev/blog/github-monopoly-on-code-hosting#finally-the-invention-of-github)

One evening in 2008, Tom Preston-Werner and Chris Wanstrath met for a drink at an SF sports bar after a Ruby on Rails meetup. The Rails community was starting to use Git more and more, but there was no central website like SourceForge for hosting Git repos. Moreover, social networks were clearly taking off, but no such social network site existed for developers like them working on open-source projects. Git had made collaborative software development easier than ever, and sites like SourceForge helped distribute versioned releases - but no site served as a home for collaboration. What if anyone could host source code, discuss issues, and request maintainers to pull in forked changes - all backed with profile pages and comment feeds like Facebook?

The soon-to-be GitHub cofounders began hacking on the first version of GitHub as a weekend project. They developed the bare bones of GitHub (using Rails, of course) on the weekends until it was complete enough to work at Chris’ day job. Through daily dogfooding, they worked out kinks and fixed any blocking feature gaps.

> “GitHub the company had sort of sprung up from this side project, so we never had any big vision or dream or aspirations. We just wanted to work on something cool.”
> 
> \-[Chris Wanstrath](https://signalvnoise.com/posts/2486-bootstrapped-profitable-proud-github)

![GitHub profile of the ruby on rails creator](https://www.datocms-assets.com/85246/1711665027-untitled-66.png)

The Ruby on Rails creator was also an early user of GitHub, helping to skyrocket the site’s early popularity.

## How GitHub looked in 2008[](https://graphite.dev/blog/github-monopoly-on-code-hosting#how-github-looked-in-2008)

![early github screenshot 1](https://www.datocms-assets.com/85246/1711665020-untitled-67.png)

![early github screenshot 2](https://www.datocms-assets.com/85246/1711665014-untitled-68.png)

GitHub’s original logo included “Social code hosting” and their H1 brand promise read: “Git repository hosting, no longer a pain in the ass.” There could be no confusion - GitHub launched with two core value adds: social network features and the ability to host Git repos online. No other site competed on these terms.  

**Project and Developer News Feeds:** Keep tabs on your favorite projects and the people that work on them.

**Source Code Browser:** Easily view your code at any version and on any branch or tag.

**Public Developer Profiles:** See what other developers are working on and how many commits they've made.

> “Killer apps make or break any platform. With GitHub, I think the Git hub just scored one.” —David Heinemeier Hansson

> “What’s amazing about GitHub is how it really brings the social aspect into play. Chris and Tom are showing us all visually how git development is supposed to work. I know I personally had some bing moments once I started pulling in commits from external git repos.” —Rick Olson

> “You’ve probably heard this at least twelve times in the last week, but GitHub is totally badass. I’ve never had a reason to put my code up on a hosting service like that before, but now I do.” —Josh Susser

## GitHub’s rapid growth[](https://graphite.dev/blog/github-monopoly-on-code-hosting#githubs-rapid-growth)

After dogfooding the MVP, the GitHub cofounders launched a free beta to friends for hosting public repositories.

GitHub’s growth in the open-source community was explosive, demonstrating a great product market fit. It was quickly adopted and enforced by the Rails project, meaning anyone wanting to use Ruby on Rails had to interact with GitHub. (That’s how folks like my teammate David first learned about GitHub and Git).

-   In its first year, GitHub grew to host 46,000 public repositories.
    
-   The next year it grew to 90,000 repositories and 100,000 users.
    
-   In year three that number grew to 1 million repositories, and by 2011 GitHub surpassed SourceForge and Google Code in scale.
    

Despite the rapid early growth, the three founders remained frugal and bootstrapped. They worked to generate revenue quickly. Rather than focusing on ads as SourceForge had, or enterprise sales as Perforce did, the GitHub founders quickly began selling individual tier subscriptions for hosting private repos. The model was easy to understand, self-serve, and somewhat original in the space compared to other hosting products and social network sites. Not only did Google Code and SourceForge not host Git repos, but they also had no option for private code hosting. Outside of GitHub, the only option for private repository hosting was running a personally managed server.

Beyond quick revenue from subscriptions, the GitHub cofounders found alternative ways of making and saving money. They experimented with alternative revenue streams such as one-off ad spots, a merch store, git training services, and a jobs board. To keep business costs as low as possible, GitHub partnered with Engine Yard, and later Rackspace, offering footer advertising in return for free hosting.

In 2009, [GitHub launched a self-hosted version](https://github.blog/2009-06-01-announcing-github-fi/https://github.blog/2009-06-01-announcing-github-fi/) that enabled larger enterprises to run GitHub. Instead of using SVN servers or products like Perforce, engineers could use their newfound Git tooling for both open-source and closed-source development. A GitHub plan for organizations and teams launched in 2010, further pushing into the private market for code hosting and change collaboration, and in 2011, they rebranded Github Fi into a formal enterprise server product.

> “Our [GitHub Enterprise](https://enterprise.github.com/) product was created to help us spread GitHub to more people. So whether you're stuck behind a firewall or have full access to the web, we want GitHub to work for you.”
> 
> \-[Chris Wanstrath](https://www.forbes.com/sites/venkateshrao/2012/03/27/github-and-the-democratization-of-programming/?sh=245cb24540a5)

By innovating on traditional code-hosting business models, GitHub earned enough money to scale its team and iterate on the product experience further than any previous product. But the deep revenue focus and bootstrapped growth wasn’t just a choice—VC investment wasn't an option for the cofounders. Up until 2010, “companies making developer tools failed to attract any significant investment.

> "Even when tools companies saw exits such as the 2013 [Facebook acquisition of Parse](https://techcrunch.com/2013/04/25/facebook-parse/), the maker of tools for mobile apps, the reported $80 million was considered on the low side. For some, developer tools would always remain a venture capital (VC) dead zone."
> 
> \-[Forbes](https://www.forbes.com/sites/forbestechcouncil/2019/10/16/developer-tools-quietly-become-a-growth-story/?sh=5b0b511c6dea)

It took investment traction from Heroku, Atlassian, Stripe, Twilio, and Sendgrid to help open up the market. Only after six years did GitHub raise investment from a bold Andrewson Horowitz - 100 million dollars for what was marketed as the [largest series A ever](https://www.forecastr.co/blog/the-largest-series-a-funding-rounds-in-history-5-awesome-stories#:~:text=GitHub%20was%20founded%20in%20San,ever%20written%20at%20that%20time).

## Who didn’t adopt GitHub?[](https://graphite.dev/blog/github-monopoly-on-code-hosting#who-didnt-adopt-github)

Google never adopted GitHub internally. Throughout the 2000s, they used Perforce and later developed a custom version control system named Piper. Not only did Google have unique advanced version control tooling, but to my knowledge, they also invented web-based code reviews. [Their early code review dashboard from 2004](https://www.youtube.com/watch?v=sMql3Di4Kgc) took inspiration from Gmail and set a gold standard for corporate software development workflows. They had no immediate need for Git and GitHub.

Facebook also never adopted GitHub for internal development. Around 2010, Facebook’s Evan Pricely developed Phabricator in 2010 - a year before GitHub formally launched enterprise self-hosting. Even if GitHub’s offering had been available, it’s likely Facebook would have still opted to develop their own tooling to better integrate with tailored internal solutions and to help handle monorepo scale that even raw Git struggled to keep up with. What's more, [Facebook moved off Git to Mercurial](https://graphite.dev/blog/why-facebook-doesnt-use-git), which GitHub never developed formal support for.

Some unicorns, such as Airbnb, adopted GitHub early on. Other notable companies, such as Uber and Pinterest, self-hosted and forked Phabricator. While I'm not certain, I suspect those who chose Phabricator did so because 1) it was the best self-hostable and open-source source control service and 2) they were filled with ex-Facebookers missing their former toolchain.

GitLab launched in 2011 and counter-positioned against GitHub by focusing on being a complete dev-ops platform, rather than “social coding”. They tagged onto the booming trend in DevOps and leaned heavily on CI/CD to gain market share in some major tech companies, such as Nvidia.

Working on Graphite in 2024, I speak with hundreds, if not thousands, of high-tech engineering teams. It is exceedingly rare that I hear about companies that don’t use GitHub. StackOverflow’s 2022 survey claims that GitHub’s professional market share is only double GitLabs. But in practice, I’d anecdotally say that 95% of modern tech companies use GitHub, and only a handful of those companies self-host GitHub enterprise. The remaining few either use GitLab, Phabricator, or Gerrit or have custom-built an [in-house code hosting platform](https://survey.stackoverflow.co/2022#section-version-control-version-control-platforms).

## The future of GitHub and code hosting[](https://graphite.dev/blog/github-monopoly-on-code-hosting#the-future-of-github-and-code-hosting)

> Linus Torvalds, the original developer of the Git software, has highly praised GitHub by stating "The hosting of github \[sic\] is excellent. They've done a good job on that. I think GitHub should be commended enormously for making open source project hosting so easy." However, he also sharply criticized the implementation of GitHub’s merging interface, stating that "Git comes with a nice pull-request generation module, but GitHub instead decided to replace it with their own totally inferior version. As a result, I consider GitHub useless for these kinds of things. It's fine for hosting, but the pull requests and the online commit editing, are just pure garbage."  
> \-[Wired](https://www.wired.com/2012/05/torvalds-github/)

What's the future of code hosting? In Clay Christensen’s famous book, [The Innovator’s Dilemma](https://www.goodreads.com/en/book/show/2615) , Christianson argues that early innovative products often start as integrated solutions. I would argue that GitHub and GitLab are examples of integrated offerings, offering a one-stop shop for teams looking to manage their source code.

Christensen argues that as a market matures, solutions become specialized and modular. We've already seen this begin to happen in a few areas of “social coding.” Jira and Linear offer modular issue tracking, while Jenkins and Buildkite offer modular CI solutions. GitHub was the original Git repo hosting solution, but in time, BitBucket, AWS Code Commit offered the same solution. GitHub now offers an integrated merge queue, but more specialized offerings now exist from Mergify, Aviator, Trunk, and Graphite.

GitHub maintains a monopoly over open-source code due to strong network effects and product features like forking, forum-style comments, and moderation that lend themselves fantastically to open-source development. Closed-source repos originally adopted GitHub due to their specialization in Git hosting - but that has now become a commodity capability. GitHub’s social features are nearly useless at private companies, where discussions happen over Slack, Notion, Linear, and Zoom.

I believe the future involves a bifurcation between dev-tools for open-source and dev-tools for closed-source. Open-source collaboration needs discussions, profiles, moderation, forking, and discovery. Closed-source collaboration needs code changes to be reviewed in hours, not days. It needs trunk-based workflows, merges queues, emergency procedures, and CI/CD coordination. There’s overlap in both cases, but I believe, over time, the world will further specialize in different solutions for the different use cases.

We’ve already seen specialized solutions from Facebook and Google. By evolving their source control systems independently from GitHub’s open-source constraints, they’ve created powerful patterns like Google’s PR inbox and Facebook’s stacked diffs.

I’d like for modularization to progress to a point where every engineer can pick how they’d like to host their source code, completely independently of the tools they’d like to use to change that source code. We already see this in other facets of development, such as how developers are free to use their preferred IDE, or their preferred cloud hosting provider. **GitHub earned their monopoly fair-and-square by better specializing into code hosting and social coding - but they’re not the end of the story.** One day in the future, I hope developers have five viable options on where to host, not just one.

> 💡 Disclaimer: I’m young enough to have not been professionally coding before GitHub. Much of the information in this post is cobbled together from online sources, interviews, and learnings I’ve had working in the space. It’s my best approximation of how things came to be and where they’re heading.

##   
The full timeline[](https://graphite.dev/blog/github-monopoly-on-code-hosting#the-full-timeline)

1.  **1999**: [SourceForge](https://sourceforge.net/about) goes live, becoming the first free open source hosting provider.
    
2.  **Pre-2004:** Use of [CVS](https://cvs.nongnu.org/) and [SVN](https://subversion.apache.org/) for version control; SourceForge leads in hosting open-source projects.
    
3.  **2004:** Linus Torvalds creates [Git](https://en.wikipedia.org/wiki/Git), revolutionizing version control with a distributed system.
    
4.  **2006:** [Google Code](https://code.google.com/archive/about) launches, initially supporting SVN.
    
5.  **2008:** [GitHub](https://github.com/about) founded, offering Git repository hosting with a focus on social coding.
    
6.  **2009:** SourceForge adds Git support; GitHub introduces a self-hosted version, laying the groundwork for GitHub Enterprise.
    
7.  **2010:** Facebook develops [Phabricator](https://www.phacility.com/phabricator/), a suite of web-based tools for code review and software development.
    
8.  **2011:** [GitLab](https://about.gitlab.com/company/) founded, emphasizing a complete DevOps platform; Google Code adds Git support.
    
9.  **2012:** GitHub launches GitHub Enterprise, catering to larger organizations' needs for private hosting.
    
10.  **2016:** Google Code shuts down, highlighting GitHub's growing dominance in code hosting.
    
11.  **2018:** [GitHub introduces GitHub Actions](https://resources.github.com/devops/tools/automation/actions/https://resources.github.com/devops/tools/automation/actions/), automating workflows within the software development process.
    
12.  **2021**: Phabricator is no longer actively maintained, pushing more users to GitHub
