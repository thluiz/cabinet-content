---
created: 2024-07-01T10:01:34 (UTC -03:00)
tags: []
source: https://brandur.org/fragments/dark-mode-notes?utm_source=tldrnewsletter
author: 
---

# Notes on implementing dark mode — brandur.org

> ## Excerpt
> <em>Not</em> a dark mode tutorial, but a few notes on some specific refinements of a good dark mode implementation like tri-state instead of bi-state toggle, avoiding page flicker, and responding to theme changes from other tabs or the OS.

---
As you can see from the pretty new toggle at the top, I recently added dark mode to this site. I thought this was something that’d never happen because over a decade I’d built up an inescapable legacy of fundamentally unmaintainable CSS, but for over a year I’ve been slowly making headway refactoring everything to Tailwind, and with that finally done, the possibility of dark mode was unlocked.

The internet is awash with tutorials on how to implement dark mode, so I won’t cover the basics, but while those tutorials will get you to a rudimentary dark mode implementation, I found that every one I read lacked the refinements necessary to get to a _great_ dark mode implementation, with many of the fine details easy to get wrong. Here I’ll cover the big ones that are commonly missing.

Some select code snippets are included below, but a more complete version should be found in this site’s repository. [HTML including Tailwind styling](https://github.com/brandur/sorg/blob/master/views/_dark_mode_toggle.tmpl.html), [JavaScript](https://github.com/brandur/sorg/blob/master/views/_dark_mode_js.tmpl.html), and [a little custom non-Tailwind CSS](https://github.com/brandur/sorg/blob/ed0bc11a82d1159d73e1f0068f8eb37561903f23/content/stylesheets-modular/tailwind_custom.css#L41-L61).

___

## [Frontend basics for backend people](https://brandur.org/fragments/dark-mode-notes?utm_source=tldrnewsletter#frontend-basics)

First, a couple core concepts that’ll be referenced below.

If you’re not a frontend programmer, then you should be aware of the existence of the [`prefers-color-scheme` CSS media selector](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme) which lets a web page react to an OS-level light/dark mode setting:

```
<span><span>@</span><span>media</span><span> (</span><span>prefers-color-scheme</span><span>:</span><span> dark</span><span>)</span><span> {</span></span>
<span><span>    // dark mode styling here</span></span>
<span><span>}</span></span>
<span></span>
```

We’ll also be making use of [local storage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API). Although both are superficially key/value stores, local storage differs from cookies in that it’s intended for use from a client’s browser itself compared to cookies which are server-side constructs.

The only way to implement a _permanently_ lived light/dark mode setting that’s persistent forever and across computers is to use a cookie to reference a server-side account where its stored in a database. That’s not possible for this site because it doesn’t have a server-side implementation or database, but local storage the next best thing. It also has the added benefits of having no default expiration date (so unless a user manually clears data, a light/dark mode setting is sticky for a long time), and sends less personal information to the server, which is broadly a good thing.

___

## [Tri-state](https://brandur.org/fragments/dark-mode-notes?utm_source=tldrnewsletter#tri-state)

By far the most common mistake in dark mode implementations is to make it a bi-state instead of tri-state setting. At first glance it might seem like the only two relevant states are light or dark, but there are actually three:

-   User has explicitly enabled dark mode.
-   User has explicitly enabled light mode.
-   User has enabled neither dark mode nor light mode. Fall back to the preference expressed by their OS in `prefers-color-scheme`.

This is implemented by way of a three state radio button that’s heavily styled to look like the toggle you see above (Tailwind classes have been removed for clarity, but see [the toggle’s template](https://github.com/brandur/sorg/blob/master/views/_dark_mode_toggle.tmpl.html) for gritty details):

```
<span><span>&lt;</span><span>input</span><span> value</span><span>=</span><span>"</span><span>light</span><span>"</span><span> name</span><span>=</span><span>"</span><span>theme_toggle_state</span><span>"</span><span> type</span><span>=</span><span>"</span><span>radio</span><span>"</span><span> /&gt;</span></span>
<span><span>&lt;</span><span>input</span><span> value</span><span>=</span><span>"</span><span>auto</span><span>"</span><span> name</span><span>=</span><span>"</span><span>theme_toggle_state</span><span>"</span><span> type</span><span>=</span><span>"</span><span>radio</span><span>"</span><span> /&gt;</span></span>
<span><span>&lt;</span><span>input</span><span> value</span><span>=</span><span>"</span><span>dark</span><span>"</span><span> name</span><span>=</span><span>"</span><span>theme_toggle_state</span><span>"</span><span> type</span><span>=</span><span>"</span><span>radio</span><span>"</span><span> /&gt;</span></span>
<span></span>
```

On input change, store the selected value to local storage and add the CSS class `dark` to the page’s HTML element so that Tailwind knows to style itself with the appropriate theme:

```
<span><span>//</span><span> Runs on initial page load. Add change listeners to light/dark</span></span>
<span><span>//</span><span> toggles that set a local storage key and trigger a theme change.</span></span>
<span><span>window</span><span>.</span><span>addEventListener</span><span>(</span><span>'</span><span>DOMContentLoaded</span><span>'</span><span>,</span><span> ()</span><span> =&gt;</span><span> {</span></span>
<span><span>    document</span><span>.</span><span>querySelectorAll</span><span>(</span><span>'</span><span>.theme_toggle input</span><span>'</span><span>)</span><span>.</span><span>forEach</span><span>(</span><span>(</span><span>toggle</span><span>)</span><span> =&gt;</span><span> {</span></span>
<span><span>        toggle</span><span>.</span><span>addEventListener</span><span>(</span><span>'</span><span>change</span><span>'</span><span>,</span><span> (</span><span>e</span><span>)</span><span> =&gt;</span><span> {</span></span>
<span><span>            if</span><span> (</span><span>e</span><span>.</span><span>target</span><span>.</span><span>checked</span><span>) </span><span>{</span></span>
<span><span>                if</span><span> (</span><span>e</span><span>.</span><span>target</span><span>.</span><span>value</span><span> ==</span><span> THEME_VALUE_AUTO</span><span>) </span><span>{</span></span>
<span><span>                    localStorage</span><span>.</span><span>removeItem</span><span>(</span><span>LOCAL_STORAGE_KEY_THEME</span><span>)</span></span>
<span><span>                }</span><span> else</span><span> {</span></span>
<span><span>                    localStorage</span><span>.</span><span>setItem</span><span>(</span><span>LOCAL_STORAGE_KEY_THEME</span><span>,</span><span> e</span><span>.</span><span>target</span><span>.</span><span>value</span><span>)</span></span>
<span><span>                }</span></span>
<span><span>            }</span></span>
<span></span>
<span><span>            setThemeFromLocalStorageOrMediaPreference</span><span>()</span></span>
<span><span>        },</span><span> false</span><span>)</span></span>
<span><span>    }</span><span>)</span></span>
<span><span>}</span><span>)</span></span>
<span></span>
```

Use of `auto` (no explicit light/dark preference) styles the page based on `prefers-color-scheme`:

```
<span><span>//</span><span> Sets light or dark mode based on a preference from local</span></span>
<span><span>//</span><span> storage, or if none is set there, sets based on preference</span></span>
<span><span>//</span><span> from the `prefers-color-scheme` CSS media selector.</span></span>
<span><span>function</span><span> setThemeFromLocalStorageOrMediaPreference</span><span>()</span><span> {</span></span>
<span><span>    const</span><span> theme</span><span> =</span><span> localStorage</span><span>.</span><span>getItem</span><span>(</span><span>LOCAL_STORAGE_KEY_THEME</span><span>) </span><span>||</span><span> THEME_VALUE_AUTO</span></span>
<span></span>
<span><span>    switch</span><span> (</span><span>theme</span><span>) </span><span>{</span></span>
<span><span>        case</span><span> THEME_VALUE_AUTO</span><span>:</span></span>
<span><span>            if</span><span> (</span><span>window</span><span>.</span><span>matchMedia</span><span>(</span><span>'</span><span>(prefers-color-scheme: dark)</span><span>'</span><span>)</span><span>.</span><span>matches</span><span>) </span><span>{</span></span>
<span><span>                setDocumentClasses</span><span>(</span><span>THEME_CLASS_DARK</span><span>)</span></span>
<span><span>            }</span><span> else</span><span> if</span><span> (</span><span>window</span><span>.</span><span>matchMedia</span><span>(</span><span>'</span><span>(prefers-color-scheme: light)</span><span>'</span><span>)</span><span>.</span><span>matches</span><span>) </span><span>{</span></span>
<span><span>                setDocumentClasses</span><span>(</span><span>THEME_CLASS_LIGHT</span><span>)</span></span>
<span><span>            }</span></span>
<span><span>            break</span></span>
<span></span>
<span><span>        case</span><span> THEME_VALUE_DARK</span><span>:</span></span>
<span><span>            setDocumentClasses</span><span>(</span><span>THEME_CLASS_DARK</span><span>,</span><span> THEME_CLASS_DARK_OVERRIDE</span><span>)</span></span>
<span><span>            break</span></span>
<span></span>
<span><span>        case</span><span> THEME_VALUE_LIGHT</span><span>:</span></span>
<span><span>            setDocumentClasses</span><span>(</span><span>THEME_CLASS_LIGHT</span><span>,</span><span> THEME_CLASS_LIGHT_OVERRIDE</span><span>)</span></span>
<span><span>            break</span></span>
<span><span>    }</span></span>
<span></span>
<span><span>    document</span><span>.</span><span>querySelectorAll</span><span>(</span><span>`</span><span>.theme_toggle input[value='</span><span>${</span><span>theme</span><span>}</span><span>']</span><span>`</span><span>)</span><span>.</span><span>forEach</span><span>(</span><span>function</span><span>(</span><span>toggle</span><span>)</span><span> {</span></span>
<span><span>        toggle</span><span>.</span><span>checked</span><span> =</span><span> true</span><span>;</span></span>
<span><span>    }</span><span>)</span></span>
<span><span>}</span></span>
<span></span>
```

## [Avoiding theme flicker on load](https://brandur.org/fragments/dark-mode-notes?utm_source=tldrnewsletter#flicker)

The next most common mistake is page flicker on load. The flicker is caused by the page initially styling itself with its default theme (usually light mode), then noticing that a different theme should be set and reskinning itself, but not before there’s an observable flash.

A lot of sites have so much crap happening when they’re loading that a flicker is lost amongst a sea of jarring effects (e.g. UI elements jumping around as sizes are determined suboptimally late), but a tasteful dark mode implementation takes care to avoid it.

The key to doing so is to make sure that the theme is checked initially by JavaScript that’s run inline with the page’s body. Putting it in an external file `<script src="...">` or in a listener like `DOMContentLoaded` is _too late_, and will cause a flicker.

Common convention is to use a `<script>` tag right before body close:

```
<span><span>&lt;</span><span>body</span><span>&gt;</span></span>
<span><span>    &lt;</span><span>script</span><span>&gt;</span></span>
<span><span>        ...</span></span>
<span></span>
<span><span>        //</span><span> script must run inline with the page being loaded</span></span>
<span><span>        setThemeFromLocalStorageOrMediaPreference</span><span>()</span></span>
<span><span>    &lt;/</span><span>script</span><span>&gt;</span></span>
<span></span>
<span><span>    ...</span></span>
<span><span>&lt;/</span><span>body</span><span>&gt;</span></span>
<span></span>
```

(I originally had the `<script>` tag right before `</body>` close instead of at the top on `<body>` open because that’s a more conventional place to put JavaScript, but found that even that was enough to produce a noticeable flicker when loading over a slower connection.)

## [Theme changes in other tabs](https://brandur.org/fragments/dark-mode-notes?utm_source=tldrnewsletter#theme-other-tabs)

If a user has multiple tabs open to your site and changes the theme in one of them, it should take effect immediately in all others.

Luckily, our use of local storage makes this trivially easy. JavaScript provides the `storage` event for when a local storage key changes. Hook into that, and this problem is solved with five lines of code:

```
<span><span>//</span><span> Listen for local storage changes on our theme key. This lets</span></span>
<span><span>//</span><span> one tab to be notified if the theme is changed in another,</span></span>
<span><span>//</span><span> and update itself accordingly.</span></span>
<span><span>window</span><span>.</span><span>addEventListener</span><span>(</span><span>"</span><span>storage</span><span>"</span><span>,</span><span> (</span><span>e</span><span>)</span><span> =&gt;</span><span> {</span></span>
<span><span>    if</span><span> (</span><span>e</span><span>.</span><span>key</span><span> ==</span><span> LOCAL_STORAGE_KEY_THEME</span><span>) </span><span>{</span></span>
<span><span>        setThemeFromLocalStorageOrMediaPreference</span><span>()</span></span>
<span><span>    }</span></span>
<span><span>}</span><span>)</span></span>
<span></span>
```

A changed theme takes effect instantly. A user clicks back to another tab and the new theme is there. No page reload required.

Take care that along with the page’s theme, `setThemeFromLocalStorageOrMediaPreference()` also sets any light/dark toggles to the right place.

## [Theme changes from the OS](https://brandur.org/fragments/dark-mode-notes?utm_source=tldrnewsletter#theme-os)

The page should respond to OS-level changes in theme, which is easy via the [`matchMedia()` API](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia):

```
<span><span>//</span><span> Watch for OS-level changes in preference for light/dark mode.</span></span>
<span><span>//</span><span> This will trigger for example if a user explicitly changes</span></span>
<span><span>//</span><span> their OS-level light/dark configuration, or on sunrise/sunset</span></span>
<span><span>//</span><span> if they have it set to automatic.</span></span>
<span><span>window</span><span>.</span><span>matchMedia</span><span>(</span><span>'</span><span>(prefers-color-scheme: dark)</span><span>'</span><span>)</span><span>.</span><span>addEventListener</span><span>(</span><span>'</span><span>change</span><span>'</span><span>,</span><span> e</span><span> =&gt;</span><span> {</span></span>
<span><span>    setThemeFromLocalStorageOrMediaPreference</span><span>()</span></span>
<span><span>}</span><span>)</span></span>
<span></span>
```

If you’re on macOS and have appearance set to “Auto” for light/dark mode to change at sunrise and sunset, this code makes sure that a site restyles itself automatically at that time. It also responds if a user manually sets their OS-level appearance. This is another small detail that most people won’t even notice (manually reloading the page will also set the right theme), but a good one nonetheless, and takes mere seconds to get right.

Side note: As a frequent critic of JavaScript, I have to acknowledge just how good its browser APIs are at helping developers _get things done_. Local storage and media matchers are powerful, complicated features, and yet we can plug into them knowing practically nothing about the elaborate effort that went into their internal implementations, and with only a handful of lines of code. Excellent work.

## [Syntax highlighting with Shiki](https://brandur.org/fragments/dark-mode-notes?utm_source=tldrnewsletter#shiki)

A site’s syntax highlighting for code blocks likely involves elaborate styling, and while some themes might look okay in either light or dark, it’s even better if the code theme changes along with the rest of the site.

I’d gotten tired of Prism’s various quirks, and recently made the move [over to Shiki](https://brandur.org/fragments/shiki). One of its many benefits is easy support for [dual light/dark themes](https://shiki.matsu.io/guide/dual-themes) with minimal configuration:

```
<span><span>//</span><span> Shiki will add its own `&lt;pre&gt;&lt;code&gt;`, so go to parent `&lt;pre&gt;`</span></span>
<span><span>//</span><span> and replace the entirety of its HTML.</span></span>
<span><span>codeBlock</span><span>.</span><span>parentElement</span><span>.</span><span>outerHTML</span><span> =</span><span> </span></span>
<span><span>    await</span><span> codeToHtml</span><span>(</span><span>code</span><span>,</span><span> {</span></span>
<span><span>        lang</span><span>:</span><span> language</span><span>,</span></span>
<span><span>        themes</span><span>:</span><span> {</span></span>
<span><span>            dark</span><span>:</span><span> '</span><span>nord</span><span>'</span><span>,</span></span>
<span><span>            light</span><span>:</span><span> '</span><span>rose-pine</span><span>'</span></span>
<span><span>        }</span></span>
<span><span>    }</span><span>)</span></span>
<span></span>
```

```
<span><span>html</span><span>.</span><span>dark</span><span> .</span><span>shiki</span><span>,</span></span>
<span><span>html</span><span>.</span><span>dark</span><span> .</span><span>shiki</span><span> span</span><span> {</span></span>
<span><span>    background-color</span><span>:</span><span> var</span><span>(</span><span>--shiki-dark-bg</span><span>)</span><span> !important</span><span>;</span></span>
<span><span>    color</span><span>:</span><span> var</span><span>(</span><span>--shiki-dark</span><span>)</span><span> !important</span><span>;</span></span>
<span><span>    font-style</span><span>:</span><span> var</span><span>(</span><span>--shiki-dark-font-style</span><span>)</span><span> !important</span><span>;</span></span>
<span><span>    font-weight</span><span>:</span><span> var</span><span>(</span><span>--shiki-dark-font-weight</span><span>)</span><span> !important</span><span>;</span></span>
<span><span>    text-decoration</span><span>:</span><span> var</span><span>(</span><span>--shiki-dark-text-decoration</span><span>)</span><span> !important</span><span>;</span></span>
<span><span>}</span></span>
<span></span>
```
