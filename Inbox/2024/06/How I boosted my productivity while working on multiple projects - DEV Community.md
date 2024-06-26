---
created: 2024-06-03T11:38:31 (UTC -03:00)
tags: [productivity,tutorial,webdev,tooling,software,coding,development,engineering,inclusive,community]
source: https://dev.to/alexk/how-i-boosted-my-productivity-while-working-on-multiple-projects-3h71?ref=dailydev
author: Alex Kaul
---

# How I boosted my productivity while working on multiple projects - DEV Community

> ## Excerpt
> As a freelance web developer, app creator and open source maintainer, I have to constantly switch...

---
As a freelance web developer, app creator and open source maintainer, I have to constantly switch back and forth between multiple workflows, such as editing code, designing mockups, managing to-do lists, searching for icons and docs, executing command lines, checking emails with different accounts and so on. Each of these switches requires a constant stream of repetitive actions:

-   Launch a code or image editor and open the files of a specific project in it.
-   Open a web browser and navigate to the project in a task manager.
-   Navigate to an icon website, set image filters and perform a search.
-   Open a web mail app and switch accounts.
-   Launch Terminal and enter commands into it.
-   etc…

Everything is scattered in different places, and turns the whole process into a real mess. And when working on multiple projects, things get even worse. I thought I could greatly improve my productivity by collecting everything I needed to get my work done in one place and organizing it into projects and workflows so that they had the right context. Projects and workflows will have clear boundaries, and switching between them will no longer be a nightmare. So I came up with [Freeter](https://freeter.io/?ref=devto), an organizer app that does just that. And recently released it as a forever free and [open source project](https://github.com/FreeterApp/Freeter).

In this short post, I'll show you how I’ve increased my productivity with Freeter, using three workflows as examples. I hope this gives you some ideas on how you too can improve your productivity.

___

## [](https://dev.to/alexk/how-i-boosted-my-productivity-while-working-on-multiple-projects-3h71?ref=dailydev#workflows)Workflows

First, I analyzed my workflows and everything I often do when I’m looking for something I need while working on a project:

1.  When I’m developing an app or website, I often need to be able to access the task manager, open project files in code and image editors, search for icons and docs on specific websites, jot down quick ideas, and open the project repository in a web browser.
2.  When I check email and Twitter DMs, I need access to the webmail and the Twitter DM page. I have multiple accounts, and need to be logged in with project specific ones.
3.  When I release a new version of the app, I need to run the release command in the Terminal app, open the releases page in the git repository, open the task manager and open the "planned feature" post editor in the Freeter community.

Now it’s time to turn them into Freeter workflows.

## [](https://dev.to/alexk/how-i-boosted-my-productivity-while-working-on-multiple-projects-3h71?ref=dailydev#appwebsite-development)App/website development

To have a quick access to the things I need to develop the app/website, I set up a workflow screen using the following widgets:

-   Tasks: Webpage widget, to embed the project’s task manager right into the workflow screen.
-   Edit Code: File Opener widget, to open the project folder in the code editing program.
-   Edit Mockup: File Opener widget, to open the mockup file in the image editing program.
-   MDN: Web Query widget, to search MDN Web Docs website.
-   Node.js Docs: Web Query widget, to search Node.js Docs website.
-   Outline Icons: Web Query widget, to search a website with icons, filtered by outline icons.
-   Fill Icons: Web Query widget, to search a website with icons, filtered by fill icons
-   Notes: Note widget, to jot down quick ideas while developing a feature.
-   Open Repo: Link Opener widget, to open the project repository in a web browser.
-   Bug Reports: Link Opener widget, to open the bug reports page in a web browser.
-   Feature Requests: Link Opener widget, to open the feature requests page in a web browser.

[![App Dev Workflow Screen](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8c0ay12255rcfctnvbb1.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F8c0ay12255rcfctnvbb1.png)

This workflow allows me to immediately switch to the development context, launch everything I need to start the development process with a simple click, quickly search docs & icons, and quickly access the task list.

## [](https://dev.to/alexk/how-i-boosted-my-productivity-while-working-on-multiple-projects-3h71?ref=dailydev#messages)Messages

To check emails and Twitter DMs, I set up a workflow using two Webpage widgets:

-   To embed the Google Mail inbox page.
-   To embed the Twitter DM page.

I also set Session Scope to Project in the widget settings so that I can be logged in under different accounts in other projects.

[![Messages Workflow Screen](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpz09rm70xn2rg5bog0z1.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fpz09rm70xn2rg5bog0z1.png)

This workflow allows me to quickly get simultaneous access to Google Mail and Twitter DMs for project-specific accounts.

## [](https://dev.to/alexk/how-i-boosted-my-productivity-while-working-on-multiple-projects-3h71?ref=dailydev#new-release)New Release

To release a new version of the app, I set up a workflow with these five widgets:

-   Release: Commander widget, to execute a command line in Terminal that asks for a new version number and starts a draft build of the new version.
-   Open Releases: Link Opener widget, to open the releases page in the web browser.
-   Tasks: A copy of Tasks from the App Dev workflow. I will need it to see all the Finished tasks in the current release.
-   Planned Features: Webpage widget, to embed the Freeter community's "planned features" page into the workflow screen. With its help, I update the planned features and post about implemented features in the new release.
-   Release steps: Note widget, to not forget to do something during the release.

[![New Release Workflow Screen](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F96t0ka8kxlrcqzo4yonu.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F96t0ka8kxlrcqzo4yonu.png)

Thanks to this workflow, I can easily release a new version and post updates about new features.

## [](https://dev.to/alexk/how-i-boosted-my-productivity-while-working-on-multiple-projects-3h71?ref=dailydev#switch-between-workflows-like-a-superhero)Switch between workflows like a superhero

Now, when I switch between projects and workflows, I simply press `Ctrl+Shift+F` to bring Freeter to the front, open the workflow tab I need at the time, and get right to work.

I hope this inspires you to organize your workflows too. To get started, visit [Freeter homepage](https://freeter.io/?ref=devto).

Do your career a favor. **Join DEV.** (The website you're on right now)

It takes **_one minute_**, it's free, and is worth it for your career.

[Get started](https://dev.to/enter?state=new-user)
