---
created: 2024-08-21T14:38:00 (UTC -03:00)
tags: [webdev,javascript,python,discuss,software,coding,development,engineering,inclusive,community]
source: https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest
author: 
---

# Screw it! I've Developed My Own Static Site Generator! - DEV Community

> ## Excerpt
> Web development nowadays has become so complicated thanks to the creators of thousands of new ways to...

---
Web development nowadays has become so complicated thanks to the creators of thousands of new ways to do the same thing. In the early days of web development he had PHP and jQuery which did pretty much everything we needed. But well things have changed now.

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#long-story-short)Long Story Short

So, I was looking for a way to build my personal website. Which would have had some blogs and my project showcase that's it, no big deal right? Well, though the same thing as well. So, my initial thought was to use these as my tech stack

-   React
-   Firebase/Supabase
-   Tailwind CSS
-   Cloudflare Pages for deploying

Well, this could be the happy ending but... 🙂

As I've already stated I will need a blog section and ironically blogs and react doesn't go well together. Since React is basically for building WebApps not content driven websites. Now those who don't know why here's a summary from chatGPT

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#why-react-is-not-suitable-for-content-driven-sites)Why React is Not Suitable for Content Driven Sites

**ChatGPT Said,**

> React is not ideal for content-driven sites primarily because it relies on client-side rendering, which can negatively impact SEO and initial page load times. Content-driven sites benefit from server-side rendering (SSR) or static site generation (SSG), which React doesn't handle out of the box. Tools like Next.js or Gatsby, which extend React, are better suited for these needs.

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#next-target-nextjs)Next Target: NextJs

Well it's obvious that I need SSR for blog site since I wanted a good indexing by search engines and a professional social media link previews. NextJs could give me both of those, but there's still a problem, and it's kind of a personal one.

See, I always loved using cloudflare pages and wanted to stick with that, besides I wanted cloudflare's free email routing to have a custom email address attached to my domain thus reducing the cost.

### [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#nextjs-at-cloudflare)NextJS at Cloudflare

I tried deploying nextJS site to cloudflare pages via their official documentation. Well, things didn't go well. I wasn't able to deploy there I tried hours finding solution and nothing worked. Let's just say nextJS and cloudflare didn't go well together for me. So if anyone from Vercel or Cloudflare reading this correct me if I'm missing something.

Well, at this point I was hopeless and the last option I had was **SSG**.

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#next-solution-ssg)Next Solution: SSG

Now SSG is good, and I understand the importance here. The problem is I've never worked with SSG before and there are multiple routes to through. There are things like Hugo, Gatsby, Astro blah blah. And probably more. Now I wasn't familiar with any of those and at this point I was so frustrated that I wasn't willing invest a bit on learning a new tool for a simple blog app. So I was like f\*$k it I'll do my own thing.

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#creating-my-own-static-site-generator)Creating My Own Static Site Generator.

Few points why I decided to develop my own static site generator

1.  I was frustrated (of course lol)
2.  Since I am making my own tool for my own thing, I will have the full control over how the pages will be generated. How will they look like.
3.  I like to reinvent.
4.  I had free times to spend.

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#the-plan)The Plan

The plan was going old fashioned way of creating website. Separate articles will have their own html pages.

Here's the full outline:

1.  I'll be writing on palin Markdown files
2.  Use python to parse the markdown into plain HTML
3.  I will already have a template where different sections will be dynamically injected.
4.  Also I'll have a config file corresponding to the article. So the file hierarchy will kinda look like this

```
articles/
├── art-1
│&nbsp;&nbsp; ├── art.md
│&nbsp;&nbsp; └── config.json
├── art-2
│&nbsp;&nbsp; ├── art.md
│&nbsp;&nbsp; └── config.json
├── art-3
│&nbsp;&nbsp; ├── art.md
│&nbsp;&nbsp; └── config.json
└── art-4
    ├── art.md
    └── config.json

```

Hence, each post will have it's own folder and the folder will have the `config.json` and `art.md` , The python script will take the `template.html` and insert dynamic contents to that HTML template, for instance the post title, slug, thumbnails from the config file and main article from the parsed markdown file. Most importantly it will dynamically generate meta tags for SEO and social medias.Afterwards, it will write the changes to a file called `art/<post-slug>.html` so that the post link will be `example.com/art/slug`.

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#how-was-it-integrated)How was it integrated?

Well so I've developed a CLI interface for interacting with the generator. I've named it **fit** you know as in **F it**. It has the following commands or options:  

```
<span>$ </span>./fit <span>--help</span>
fit: also known has f<span>**</span>k it build system
A build system <span>for </span>my personal site developed by Shazin

USAGE
     fit &lt;action&gt; &lt;argument&gt;
COMMANDS
    init                Creates a new post template at articles/art-[n]
    build art-&lt;n&gt;       Builds the specified article
    <span>sync                </span>Syncs the global articles index to homepage
    uploader            Launches the GTK GUI image uploader
    upload &lt;file_path&gt;  Uploads the specified file to firebase
    deploy              Deploys <span>local </span>changes to remote repository
    <span>help</span>, <span>-h</span>, <span>--help</span>    Displays this <span>help </span>menu

```

### [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#deployment-mechanism)Deployment mechanism

So, like I said I wanted to use Cloudflare pages for deploying. Basically what I've done is I've created a branch called `prod` and whenever the `./fit deploy` command is run it will basically copy all the necessary files to the prod branch and push the changes to github. Then, cloudflare will automatically build and redeploy the changes.

### [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#handing-images)Handing images

In order to handle images or any static files I have used firebase storage, the `./fit uploader` will pop open a GTK based GUI uploader from which I can upload an image and it will give me the public url which I can than copy, Here's how it looks:

**Upload Interface**

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--9X3c2x2M--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/c988a419-4684-4f6f-ac0e-d06f3322da49.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--9X3c2x2M--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/c988a419-4684-4f6f-ac0e-d06f3322da49.png)

**Post Upload Interface**

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--MBlIOKfR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/83bb9517-00bc-4a37-a8b6-7b0659aa1227.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--MBlIOKfR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/83bb9517-00bc-4a37-a8b6-7b0659aa1227.png)

#### [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#cli-interface)CLI Interface

There's also a CLI Interface which can be used by `./fit upload <filepath` this was an option for cases where I don't have access to a GUI interface. This allows me to publish or edit articles from my phone as well using terminal emulator like Termux

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--OY57ZkN3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/5a571151-32f6-4415-95fd-aa6b151832e3.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--OY57ZkN3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/5a571151-32f6-4415-95fd-aa6b151832e3.png)

### [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#dynamic-ambient-bcakground)Dynamic Ambient Bcakground

So, I thought when I am the one handling all the building and generation myself I can definitely do some cool stuffs with it, so I've added a dynamic colored ambient background to each post. The idea was to pick an average color from the thumbnail image and then darken it and use it as the background. I've also picked a primary color for the links and buttons from the thumbnail image as well and honestly to me, it looks really cool, here's an screenshot

[![A post page sample](https://res.cloudinary.com/practicaldev/image/fetch/s--6NlT8l8v--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/d5d12272-43c6-40dd-8b75-aa58217c62e1.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--6NlT8l8v--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://storage.googleapis.com/shazin-me-data.appspot.com/d5d12272-43c6-40dd-8b75-aa58217c62e1.png)

### [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#comments-and-discussion)Comments and Discussion

Since I was working with basically no database or no backend service at all, I had to choose an external service for this and what else does this better than [Disqus](https://disqus.com/) .

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#was-it-worth-it)Was it Worth It?

Well, to be honest like I said I was spending some free times, so yeah it was definitely worth it, and it didn't take me long time to be honest I've spent 2-3 days for this full project and really had fun building something creative.

## [](https://dev.to/shazin/screw-it-ive-developed-my-own-static-site-generator-27cd?context=digest#wrapping-up)Wrapping Up

So, I've had really fun experience with this project and will hopefully do more improvements and add more functionalities to it. Right now it's so basic and simple which was what I wanted. If you like this project or want me to open source it please let me know. Oh and here's the link of the site I was screaming about [shazin.me](https://shazin.me/) Thanks for reading.
