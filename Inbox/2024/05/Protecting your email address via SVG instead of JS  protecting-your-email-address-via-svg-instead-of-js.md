---
created: 2024-05-14T10:24:44 (UTC -03:00)
tags: []
source: https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter
author: 
---

# Protecting your email address via SVG instead of JS | protecting-your-email-address-via-svg-instead-of-js

> ## Excerpt
> Conventionally, email addresses published on webpages may be protected from email-harvesting spambots via various HTML, CSS & JS-based techniques. This alternative technique is SVG-based.

---
## Protecting your email address via SVG instead of JS

_For a live demo of this accessible, no-javascript technique, see:_

-   [_SVG-based Email Protection_](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/svg-email-protection.html)

___

Email addresses published on webpages usually need to be protected from email-harvesting spambots.

Conventionally, email protection techniques utilise a combination of **HTML**, **CSS** and **JS** - though each approach is subject to its own pros and cons.

In general, approaches involving **JS** tend to be more sophisticated than alternatives based on **HTML** and / or **CSS**.

But the downside is that **JS** then becomes an unavoidable dependency of that page.

There is always benefit in considering the school of thought which advocates that while JS may _enhance_ pages, ideally we should want all the essential functionality on those pages to work even while JavaScript is turned off.

___

The technique detailed on this page utilises an an approach entirely different from conventional email protection techniques, based on **CSS**, **JS**, **CSS + JS** etc.

Because _this_ technique is based on **SVG**.

**N.B.** _This technique - and any other email-protection technique utilising front-end-technologies - **won’t** protect your published email address from being harvested by the most determined and sophisticated spambots. But, as with many JS-based email-protection techniques, it **will** protect you nevertheless from a great many unsophisticated harvesters and keep your email successfully hidden from any simple or amateurish scripts trawling the web, seeking to copy any unprotected email addresses they find._

___

## Three advantages of an SVG-based approach to protecting email addresses[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#three-advantages-of-an-svg-based-approach-to-protecting-email-addresses)

### 1\. Works with JavaScript turned off[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#1-works-with-javascript-turned-off)

The main advantage of this **SVG-based approach** to protecting emails, is that it involves **no JavaScript**.

As such, even when a human visitor has their JavaScript turned off, the email address displayed on the page remains usable, accessible and protected, while remaining secure and hidden from spambots.

### 2\. Permits a standard `mailto:` link[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#2-permits-a-standard-mailto-link)

Unlike other _no-JavaScript-required_ approaches (e.g. obfuscating email addresses by inserting non-visible HTML Comments or inserting visible elements and subsequently hiding them via CSS), this **SVG-based approach** allows for standard `mailto:` links. The twist is: the `mailto:` link exists inside the external **SVG document**, _not_ inside the referring **HTML document**.

### 3\. Conceals content like an image; Copyable like text[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#3-conceals-content-like-an-image-copyable-like-text)

A third advantage is that embedded **SVGs** are _image-like_ but _not_ images.

As replaced elements embedded within a hypertext document, **SVGs** may conceal an email address from spambots nearly as effectively as an image might.

But, strictly, **SVGs** are _graphics documents_ rather than actual images.

Consequently, _unlike with an image_, a human visitor may still copy the email address by **right-clicking** on the `<text>` element in the embedded **SVG**.

This would not be possible with a conventional image.

___

## Implementing the Code[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#implementing-the-code)

In the example below there are two files.

The SVG graphics document is embedded in the HTML hypertext document via:

```
&lt;object data="svg-email-protection.svg" type="image/svg+xml" /&gt;&lt;/object&gt;
```

Note that the same SVG graphics document may be embedded in hypertext once - or multiple times.

### HTML File[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#html-file)

```
<span>&lt;!DOCTYPE html&gt;</span>
<span>&lt;html</span> <span>lang=</span><span>"en-GB"</span><span>&gt;</span>
<span>&lt;head&gt;</span>
<span>&lt;meta</span> <span>charset=</span><span>"utf-8"</span><span>&gt;</span>
<span>&lt;title&gt;</span>SVG Email Protection<span>&lt;/title&gt;</span>
    
<span>&lt;style&gt;</span>
    
<span>.svg-email-protection</span> <span>{</span>
  <span>width</span><span>:</span> <span>180px</span><span>;</span>
  <span>height</span><span>:</span> <span>24px</span><span>;</span>
  <span>vertical-align</span><span>:</span> <span>middle</span><span>;</span>
<span>}</span>
    
<span>&lt;/style&gt;</span>
<span>&lt;/head&gt;</span>

<span>&lt;body&gt;</span>

<span>&lt;p&gt;</span>This is my email: <span>&lt;object</span> <span>class=</span><span>"svg-email-protection"</span> <span>data=</span><span>"svg-email-protection.svg"</span> <span>type=</span><span>"image/svg+xml"</span><span>&gt;&lt;/object&gt;&lt;/p&gt;</span>

<span>&lt;/body&gt;</span>
<span>&lt;/html&gt;</span>
```

### SVG File[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#svg-file)

```svg
<svg xmlns="http://www.w3.org/2000/svg" lang="en-GB" aria-labelledby="title" viewBox="0 0 200 24"> <title id="title">Email Us!</title> <defs> <style type="text/css"><![CDATA[ rect { width: 200px; height: 24px; fill: rgb(255, 255, 255); } a:focus rect, rect:hover { rx: 4px; ry: 4px; fill: rgb(0, 0, 255); } text { font-size: 16px; fill: rgb(0, 0, 255); pointer-events: none; } a:focus text, rect:hover + text { fill: rgb(255, 255, 255); font-weight: 900; text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.2); text-decoration: underline 1px solid rgb(255, 255, 255); text-underline-offset: 5px; } ]]></style> </defs> <a href="mailto:myemail@mydomain.tld" aria-label="Email Us!"> <rect /> <text x="50%" y="50%" text-anchor="middle" dominant-baseline="middle">myemail@mydomain.tld</text> </a> </svg>
```

___

## Accessibility[](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/?utm_source=tldrnewsletter#accessibility)

As ever, it’s important to ensure that this setup remains as **_accessible_** as possible.

On this basis, note the following in the SVG graphics document:

-   the entire SVG document is `aria-labelledby` the SVG document `<title>`, indicating a call-to-action
-   the anchor element (`<a>`) inside the SVG has an `aria-label` which has the same call-to-action
-   the SVG is styled such that when the _tab-focus_ falls on the anchor element (`<a>`), the child-elements, `<rect />` and the `<text />`, are both highlighted

___

_To see a live demo of this accessible, no-javascript technique, go to:_

-   [_SVG-based Email Protection_](https://rouninmedia.github.io/protecting-your-email-address-via-svg-instead-of-js/svg-email-protection.html)
