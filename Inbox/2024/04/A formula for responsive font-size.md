---
created: 2024-03-13T11:21:57 (UTC -03:00)
tags: [programming,web]
source: https://jameshfisher.com/2024/03/12/a-formula-for-responsive-font-size/?utm_source=tldrnewsletter
author: 
---

# A formula for responsive font-size

> ## Excerpt
> This CSS is now part of most websites I make:

---
This CSS is now part of most websites I make:

```css
:root { font-size: calc(1rem + 0.25vw); }
```

This rule is an alternative to `@media` rules like this from nytimes.com:

```css
p { font-size: 1.125rem; } @media (min-width: 740px) { p { font-size: 1.25rem; } }
```

This pattern is the norm: a small font size, a large font size, and a breakpoint. Here’s a sample of common sites:

|  | Small font-size | Large font-size | Breakpoint |
| --- | --- | --- | --- |
| **Medium.com** | `1.125rem` | `1.25rem` | `728px` |
| **Substack.com** | `1.125rem` | `1.25rem` | `768px` |
| **Nytimes.com** | `1.125rem` | `1.25rem` | `740px` |

But breakpoints are mathematically ugly! Instead of defining `font-size` piecewise, can’t we use one linear function? Here’s the line I believe they’re trying to approximate:

![](https://jameshfisher.com/assets/2024-03-12/chart.png)

With modern CSS, we can just write that function! It’s `calc(1.0625rem + 0.2604vw)`. I round this to `1rem + 0.25vw`.

Sharp-eyed readers might wonder: doesn’t my CSS have circular reasoning? If `rem` is defined as “Font size of the root element”, how can we use `1rem` in the definition of `font-size` on the root element?! It turns out [it’s a special case](https://www.w3.org/TR/css-values-3/#rem):

> When specified in the `font-size` property of the root element, or in a document with no root element, `1rem` is equal to the initial value of the `font-size` property.

### More by Jim

Tagged [#programming](https://jameshfisher.com/tag/programming), [#web](https://jameshfisher.com/tag/web). All content copyright James Fisher 2024. This post is not associated with my employer. [Found an error? Edit this page.](https://github.com/jameshfisher/jameshfisher.com/edit/master/_posts/2024-03-12-a-formula-for-responsive-font-size.md)
