---
created: 2024-01-04T09:02:09 (UTC -03:00)
tags: []
source: https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/
author: 
---

# Web Components Will Outlive Your JavaScript Framework | jakelazaroff.com

> ## Excerpt
> If we're building things that we want to work in five or ten or even 20 years, we need to avoid dependencies and use the web with no layers in between.

---
If you’re anything like me, when you’re starting a project, there’s a paralyzing period of indecision while you try to figure out how to build it. In the JavaScript world, that usually boils down to picking a framework. Do you go with Ol’ Reliable, a.k.a. React? Something slimmer and trendier, like Svelte or Solid? How about kicking it old school with a server-side framework and HTMX?

When I was writing my [CRDT blog post series](https://jakelazaroff.com/words/an-interactive-intro-to-crdts/), I knew I wanted to include interactive demos to illustrate the concepts. Here’s an example: a toy collaborative pixel art editor.

Even though I’ve written before — and still believe — that [React is a good default option](https://jakelazaroff.com/words/no-one-ever-got-fired-for-choosing-react/), the constraints of a project should determine the technology decisions. In this case, I chose to use vanilla JS web components. I want to talk about why.

There was one guiding principle for this project: although they happened to be built with HTML, CSS and JS, these examples were **content, not code**. In other words, they’d be handled more or less the same as any image or video I would include in my blog posts. They should be portable to any place in which I can render HTML.

As of 2023, this blog is built with Astro. Before that, it was built with [my own static site generator](https://jake.museum/jakelazaroff-v5/). Before that, [Hugo](https://jake.museum/jakenyc-v3/); before that, [a custom CMS written in PHP](https://jake.museum/jakelazaroff-blog/); before that, [Tumblr](https://jake.museum/hexnut-v5/), [Movable Type](https://jake.museum/hexnut-v4/) and [WordPress](https://jake.museum/mlingojones/) — and I’m sure I’m missing some in between. I really like Astro, but it’s reasonable to assume that this website won’t run on it forever.

One thing that has made these migrations easier in recent years is keeping all my content in plain text files written in Markdown. Rather than dealing with the invariably convoluted process of moving my content between systems — exporting it from one, importing it into another, fixing any incompatibilities, maybe removing some things that I can’t find a way to port over — I drop my Markdown files into the new website and it mostly Just Works.

Most website generators have a way to include more complex markup within your content, and Astro is no different. The MDX integration allows you to render Astro components within your Markdown files. Those components have access to all the niceties of the Astro build system: you can write HTML, CSS and JS within one file, and Astro will automagically extract and optimize everything for you. It will scope CSS selectors and compile TypeScript and let you conditionally render markup and do all sorts of other fancy stuff.

The drawback, of course, is that it all only works inside Astro. In order to switch to a different site generator, I’d have to rewrite those components. I might need to split up the HTML, CSS and JS, or configure a new build system, or find a new way to scope styles. So Astro-specific features were off limits — no matter how convenient.

But Markdown has a secret weapon: [you can write HTML inside of it](https://daringfireball.net/projects/markdown/syntax#html)! That means any fancy interactive diagrams I wanted to add would be just as portable as my the rest of my Markdown as long as I could express them as plain HTML tags.

[Web components](https://developer.mozilla.org/en-US/docs/Web/API/Web_Components) hit that nail square on the head. They’re a set of W3C standards for building reusable HTML elements. You use them by writing a class for a custom element, registering a tag name and using it in your markup. Here’s how I embedded that pixel art editor before:

```html
<pixelart-demo></pixelart-demo>
```

That’s the honest-to-goodness HTML I have in the Markdown for this post. That’s it! There’s no special setup; I don’t have to remember to put specific elements on the page before calling a function or load a bunch of extra resources.<sup><a href="https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/#user-content-fn-cheating" id="user-content-fnref-cheating" data-footnote-ref="" aria-describedby="footnote-label" data-astro-cid-bi7aps5f="" role="button">1</a></sup> Of course, I do need to keep the JS files around and link to them with a `<script>` tag. But that goes for any media: there needs to be **some** way to reference it from within textual content. With web components, once the script is loaded, the tag name gets registered and works anywhere on the page — even if the markup is present before the JavaScript runs.

Web components encapsulate all their HTML, CSS and JS within a single file, with no build system necessary. Having all the code for a component in one place significantly reduces my mental overhead, and I continue to be a huge fan of single-file components for their developer experience. While web components aren’t **quite** as nice to write as their Astro or Svelte counterparts, they’re still super convenient.<sup><a href="https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/#user-content-fn-frameworks" id="user-content-fnref-frameworks" data-footnote-ref="" aria-describedby="footnote-label" data-astro-cid-bi7aps5f="" role="button">2</a></sup>

In case you’re not familiar with web components, here’s the code for that `<pixelart-demo>` component above:<sup><a href="https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/#user-content-fn-abridged" id="user-content-fnref-abridged" data-footnote-ref="" aria-describedby="footnote-label" data-astro-cid-bi7aps5f="" role="button">3</a></sup>

```js
import PixelEditor from "./PixelEditor.js"; class PixelArtDemo extends HTMLElement { constructor() { super(); this.shadow = this.attachShadow({ mode: "closed" }); this.render(); const resolution = Number(this.getAttribute("resolution")) || 100; const size = { w: resolution, h: resolution }; const alice = new PixelEditor(this.shadow.querySelector("#alice"), size); const bob = new PixelEditor(this.shadow.querySelector("#bob"), size); alice.debug = bob.debug = this.hasAttribute("debug"); } render() { this.shadow.innerHTML = ` <div class="wrapper"> <canvas class="canvas" id="alice"></canvas> <canvas class="canvas" id="bob"></canvas> <input class="color" type="color" value="#000000" /> </div> <style> .wrapper { display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: 1fr auto; gap: 1rem; margin: 2rem 0 3rem; } .canvas { grid-row: 1; width: 100%; aspect-ratio: 1 / 1; border: 0.25rem solid #eeeeee; border-radius: 0.25rem; cursor: crosshair; } .color { grid-column: 1 / span 2; } </style> `; } } customElements.define("pixelart-demo", PixelArtDemo);
```

Everything is nicely contained within this one file. There is that one import at the top, but it’s an [ES module import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) — it doesn’t rely on any sort of build system. As long as I keep all the files together, the browser will sort everything out.

Another nice thing about Web components is shadow DOM, which isolates the component from the surrounding page. I think shadow DOM is often awkward when you want to share styles between your components and the rest of your app, but it’s perfect when you do truly want everything to be isolated. Just like images and videos, these components will look and act the same no matter where they’re used.

Sorry — they’re not **just** like images and videos. Web components can expose attributes that allow you to configure them from the outside. You can think of them as native props. Voilà:

Two input ranges with different accent colors. In this case, I’m just setting a CSS variable, which is one of the few things allowed into the shadow DOM:

```html
<range-slider style="--accent: #0085F2"></range-slider>
```

Here’s a more complex example:

And here’s the markup. It uses attributes to alter the component’s behavior, setting the resolution to 20 and showing debug information on every pixel:

```html
<pixelart-demo debug resolution="20"></pixelart-demo>
```

If you were wondering what those calls to `getAttribute` and `hasAttribute` were doing in the web component class, now you know. This was particularly useful when reusing the same component for different stages of a tutorial, allowing me to enable certain features as the tutorial progressed.

The other part of the equation was using vanilla JS. There are frameworks that compile to web components — most notably Lit (although I’d call it more of a library) but also Stencil, Svelte, and probably others. I’m sure they’re all wonderful tools that would have made my life easier in a lot of ways. But frameworks are dependencies, and dependencies have a bunch of tradeoffs. In this case, the tradeoff I’m most worried about is maintenance.<sup><a href="https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/#user-content-fn-vendoring" id="user-content-fnref-vendoring" data-footnote-ref="" aria-describedby="footnote-label" data-astro-cid-bi7aps5f="" role="button">4</a></sup>

That goes for TypeScript, too. By my count, the last 15 versions of TypeScript have had breaking changes — many of them new features that I was happy to have, even though I had to change my code to accommodate them. But as much as I love TypeScript, it’s not a native substrate of the web. It’s still a dependency.

There’s a cost to using dependencies. New versions are released, APIs change, and it takes time and effort to make sure your own code remains compatible with them. And the cost accumulates over time. It would be one thing if I planned to continually work on this code; it’s usually simple enough to migrate from one version of a depenency to the next. But I’m not planning to ever really touch this code again unless I absolutely need to. And if I **do** ever need to touch this code, I **really** don’t want to go through multiple years’ worth of updates all at once.

I [learned that lesson the hard way](https://jakelazaroff.com/words/preserving-the-web/) when I built my online museum, wiping the cobwebs off of code saved on laptops that hadn’t been turned on in a full decade. The more dependencies a website had, the more difficult it was to restore.

I’ve been building on the web for almost 20 years. That’s long enough to witness the birth, rise and fall of jQuery. Node.js was created, forked into io.js and merged back into Node. Backbone burst onto the scene and was quickly replaced with AngularJS, which was replaced with React, which has been around for only half that time and has **still** gone through like five different ways to write components.

But as the ecosystem around it swirled, the web platform itself remained remarkably stable — largely because the stewards of the standards painstakingly ensured that no new change would break existing websites.<sup><a href="https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/#user-content-fn-smooshgate" id="user-content-fnref-smooshgate" data-footnote-ref="" aria-describedby="footnote-label" data-astro-cid-bi7aps5f="" role="button">5</a></sup> The [original Space Jam website](https://www.spacejam.com/1996/) from 1996 is famously still up, and renders perfectly in modern browsers. So does the [first version of the website you’re reading now](https://jake.museum/jakelazaroff-v1/), made when I was a freshman in college 15 years ago. Hell, the [first website ever created](http://info.cern.ch/hypertext/WWW/TheProject.html) — built closer to the formation of the Beatles <sup><a href="https://jakelazaroff.com/words/web-components-will-outlive-your-javascript-framework/#user-content-fn-moonlanding" id="user-content-fnref-moonlanding" data-footnote-ref="" aria-describedby="footnote-label" data-astro-cid-bi7aps5f="" role="button">6</a></sup> than to today! — still works, in all its barebones hypertext glory.

If we want that sort of longevity, we need to avoid dependencies that we don’t control and stick to standards that we know won’t break. If we want our work to be accessible in five or ten or even 20 years, we need to use the web with no layers in between. For all its warts, the web has become the most resilient, portable, future-proof computing platform we’ve ever created — at least, if we build with that in mind.
