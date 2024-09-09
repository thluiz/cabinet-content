---
created: 2024-08-30T21:45:09 (UTC -03:00)
tags: [webdev,javascript,react,fullstack,software,coding,development,engineering,inclusive,community]
source: https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi
author: 
---

# Hidden cost of frontend frameworks - DEV Community

> ## Excerpt
> We all want our sites to look attractive and feel fast and responsive across a multitude of devices...

---
We all want our sites to look attractive and feel fast and responsive across a multitude of devices and screen sizes. There are common tools developed in the frontend eco system to help build such interfaces.

The most common and well known is [React](https://react.dev/), with many others sharing this space, such as [Svelte](https://svelte.dev/), [SolidJS](https://www.solidjs.com/), [Angular](https://angular.dev/), [Vue](https://vuejs.org/), [Qwik](https://qwik.dev/) and more. All are impressive feats of engineering and come with bold statements.

React:

> The library for web and native user interfaces

Solid:

> Simple and performant reactivity for building user interfaces.

VueJs:

> An approachable, performant and versatile framework for building web user interfaces.

Regardless, they all have something in common…

## [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#javascript)Javascript

If you're going to write your whole project with one of these frameworks, for better or for worse you're going to be writing all your logic in Javascript (or Typescript). After all, Javascript is the language of the web, right? It's right there in the browser, it seems only natural to write your web app in the same language.

Only, if you're going to write a fullstack app and have server rendered or statically rendered pages, you'll most likely use a meta framework like [NextJs](https://nextjs.org/), [Remix](https://remix.run/), [SvelteKit](https://kit.svelte.dev/), [SolidStart](https://start.solidjs.com/).

This means you're going to need a server, and it's going to run Javascript too. You don't really have a choice here, the framework mandates your architecture in this instance.

## [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#convenience-over-simplicity)Convenience over simplicity

But this isn't a problem right? I mean, it is so easy and _simple_ to get started. Let's take a look at a couple of examples to get up and running.

NextJs:  

```
npx create-next-app@latest
cd my-project
npm run dev
```

SolidStart:  

```
npm init solid@latest
cd my-project
npm install
npm run dev
```

In both these examples it bootstraps a project with some sensible defaults and gets you up and running in minutes with a dev server with hot reloading out of the box. This is awesome and feels like a great developer experience, right?

Let's say you want to build a new web app for a project you are working on. You can get your server side rendered interactive app in NextJs up and running in minutes and start building. It might feel great, but does your app really need to be so engineered for what it needs to do?

Do you really need a build step to transpile your modules and JSX to a JS bundle that can be ran on the server to generate the initial HTML, stream to the client with a bunch of JS to hydrate the dom nodes and have client side state keeping track of a virtual DOM and re-rendering at every change?

It might be convenient to get up and running quickly, but let's not forget that convenience does not equal simplicity. Under the hood there are abstractions upon abstractions to make the 'magic' happen and this can come to bite you as time passes and the project scales.

## [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#keeping-up-with-dependencies)Keeping up with dependencies

In the above examples we setup a basic project to get up and running in NextJs and SolidStart. There's something interesting here though that I'd like to highlight. After installing NextJs (below multiple deprecated package warnings) there is this line helpfully outputted by npm:  

```
added 361 packages, and audited 362 packages in 10s
```

And for SolidStart:  

```
added 569 packages, and audited 572 packages in 18s
```

These are the number of packages that were required and downloaded by NextJs and SolidStart respectively when we initialised the projects.

And this is just for the frameworks, it doesn't include any package you might need to reach for in your project. Want to have a code formatter to force consistent formatting? Download Prettier. Want to enforce some code rules? Download ESLint. Want to have types? Download Typescript. Want to run tests? Download Jest (or maybe Vitest) and this goes on and on.

Though this might seem trivial, I see this as a sign that we might be using Javascript not as it was intended and we end up having to reach for packages to perform relatively common operations. This results in dependencies with dependencies with dependencies (insert node\_modules meme here).

Requiring so many dependencies inherently makes your project [warm-blooded](https://blog.jim-nielsen.com/2024/cold-blooded-software/). If you don't touch this project for 6 months, you might see it will be out of date when you come back to it.

No worries! You don't need to keep things up to date, right? Well, it depends. Let's say you are security conscious and use a tool like [Dependabot](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide) to scan your dependencies for vulnerabilities. You might find you have a few per week, especially if your project is into the thousands of dependencies, which is not too unusual for middle to large projects.

Let's say you're not bothered about updating at all. The older the project gets, the more difficult it will become to avoid unleashing the dependency waterfall. Eventually, you'll have to open the floodgates when it's time to update Node, want to use a newly released feature, or add a new package which is not compatible with another outdated dependency.

On the flipside, I've seen projects that want to keep up to date with all dependencies, which can result with multiple package updates daily, and sometimes introducing a range of interesting bugs and quirks that made it past tests and into production.

Extrapolate this out to multiple projects and all of a sudden it can feel like a constant background pressure and be a weekly chore to test and update projects to keep them relevant and from gathering dust.

## [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#rapidly-evolving-ecosystem)Rapidly evolving ecosystem

You've probably seen the memes 'Yet another JS framework'. In a more positive way, we could say that there are constant innovations, evolutions and changes happening in the ecosystem. It truly is impressive the amount of work and passion that goes into such projects. However, this can have a negative impact too when working on web apps in production.

If you've been in the space for a while, you might have made your first React project with `create-react-app` to get you up and running. Too bad this is deprecated now, maybe you should move to Vite. Maybe you made your first static React project with Gatsby. Well that's kind of deprecated now, you should probably migrate to NextJs or Remix. Using NextJs already? You should probably migrate to the app router. Maybe you started writing your React components as classes with logical life cycle hooks. Well that's the old way, you should be writing functional components now, and hope you pass the right dependencies to that `useEffect` hook. But wait! Hold on, React Server Components are here and there's a smart compiler on the way.

This constant shifting and trying to stay ahead of the curve increases technical debt and adds to the burden of keeping projects up to date. Think about all the concepts you need to be aware of today when using React, vs someone coming in fresh in 2016. To React's credit, it has remained backwards compatible, but we cannot ignore the cognitive overhead that is required to stay up to date with today's best practices.

## [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#the-developer-experience-fallacy)The developer experience fallacy

This brings me to the developer experience. Surely everything we've looked at so far can be seen as a trade off to the great developer experience these frameworks have to offer?

While I do believe the developer experience is great in the moment, what I feel is often overlooked is the developer experience _over time_. Sure it can feel great to spin up a new dev server in seconds with almost instant hot reloading on changes - but if you track every time you need to refactor, upgrade, or maintain a project over the years, all of a sudden the experience is not as great, and this shouldn't be overlooked.

## [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#so-what-are-the-takeaways)So what are the takeaways?

### [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#use-the-right-tool-for-the-job)Use the right tool for the job

Don't just accept that a web framework is the default and best option when building a web app. Look at your use case. Does it need to be a Single Page Application? Do you need such a level of client side interaction? Could you achieve something similar using something like [HTMX](https://htmx.org/) and any language of your choice to generate the pages?

### [](https://dev.to/manonbox/hidden-cost-of-frontend-frameworks-5pi#keep-things-simple)Keep things simple

Tech is cool, but sometimes we just need something that is _good enough_ and does what it needs to do in a simple way. This can help us keep sane and avoid feelings of burnout. Avoid premature abstractions, try sticking to web standards and don't scale to the moon.

As developers we can worry too much, and fixate on the perfect and best way to do things. Lets not forget to have fun, explore and learn. If it doesn't work out, that means you learned something and you can always change it later.
