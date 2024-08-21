---
created: 2024-08-21T14:43:13 (UTC -03:00)
tags: []
source: https://lirantal.hashnode.dev/zero-dependency-javascript-is-the-future
author: Liran Tal
---

# Zero Dependency JavaScript is the Future?

> ## Excerpt
> The JavaScript ecosystem is well known for its use of small packages (left-pad anyone?) and being a huge repository spanning more than 3 million open-source packages but what if we could build our JavaScript applications without any dependencies? Are...

---
The JavaScript ecosystem is well known for its use of small packages (`left-pad` anyone?) and being a huge repository spanning more than 3 million open-source packages but what if we could build our JavaScript applications without any dependencies? Are we seeing a trend towards **zero dependency JavaScript**? what does it mean for the future of the JavaScript development, its impact on security, and the overall developer experience?

## The AXObject Query controversy: backwards compatibility vs modern JavaScript

On June 22nd, a code change through a merged [pull request](https://github.com/A11yance/axobject-query/pull/354) from `ljharb` landed on the `axobject-query` GitHub repository. The goal with this update was to restore backwards compatibility that was unknowingly broken by a previous code change that switched dependencies to the `dequal` package.

The package maintainer for AXObject agreed handing over the project’s maintenance with expectations for `ljhrab` making the necessary changes to restore the compatibility with older versions of Node.js, specifically Node.js 0.4. Yet, this changed introduced a new controversy in the JavaScript ecosystem.

![no-one needs Node 4 support](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852916281/ee29b052-c0cd-4d11-a7a2-3b7324f1390b.png?auto=compress,format&format=webp)

Specifically, this pull request replaced some packages like the Jest testing framework with tape and other dependencies such as `object.values`, and `deep-qual-json` (that replaced `dequal`). While changing dependencies and switching versions and toolchains in and out is nothing special in the JavaScript ecosystem, this change was different.

From Jordan’s (`ljharb`) perspective, the goal of this pull request is to backport the `axobject-query` package to be compatible with older and unsupported versions of Node.js that are now effectively out of maintenance and deemed as EOL (End of Life) by the Node.js project. However, at what cost?

The new changes introduced by the pull request, which aimed to support Node.js 0.4 resulted in the [growth of the third-party dependency tree](https://npmgraph.js.org/?q=deep-equal-json) of the `axobject-query` package. Specifically, introducing the `deep-equal-json` adds 16 new nested dependencies:

![deep-equal-json dependency tree](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852916979/eee05493-dcca-4e42-aa4c-f6084938d70a.png?auto=compress,format&format=webp)

Finally, Jordan only merged the Node.js 0.4 backward compatibility changes to the v3 branch of the `axobject-query` package, leaving the main branch with `dequal`.

## What is the problem with a large dependency tree?

Generally speaking, npm packages help us build a maintainable and modular application by breaking down the code into smaller, reusable, and testable units. The open and open-source JavaScript ecosystem greatly benefits from this modular approach and advocates for it.

However, some would argue the more dependencies we introduce into our projects, the more potential burden we add to our applications. This burden can manifest in various ways:

-   **Speed**: The more dependencies we have, the longer it takes for the `npm` package manager to install them. It amounts to many HTTP requests, bandwidth, and can result in a slow down the development process and CI/CD pipelines, as well as end-users consumers that rely on a package. Especially for command-line applications that rely on the `npx <command>` pattern.
    
-   **Storage**: The more dependencies we have, the more disk space we need to store them. `npkill` is a day-to-day use CLI that really portrays the issue of a growing `node_modules/` folder, often times to gigabytes of size.
    
-   **Maintenance**: The more dependencies we have, the more maintenance we need to do. We need to keep track of the dependencies, their versions, and their potential vulnerabilities. We need to update them regularly to benefit from the latest features, bug fixes, and security patches. We need to test them to ensure they work as expected and don’t introduce regressions. We need to fix them if they break our applications. We need to remove them if they are no longer needed. We need to document them to help other developers understand how they work and how they can be used. We need to contribute to them if we find bugs or want to add new features. We need to review them to ensure they meet our quality standards and don’t introduce any potential vulnerabilities. So bottom line, a lot of work :-).
    
-   **Security**: A bigger dependency tree would often mean bigger potential for security vulnerabilities we introduce into our applications due to more code written by others who expand the application surface in potentially insecure code. A growth in dependency tree could also potentially translate to more individual maintainers who publish them, which means more reliance, trust, and hope that they all follow good security practices (enabling 2FA, not leaking or reusing secrets, etc).
    

## Zero dependency vs mere 3 new dependency addition

A follow-up pull request to a different repository of Jordan’s demonstrate the different between a zero dependency surface of an npm package (its size, among other traits), and the addition of new dependencies.

The `traverse` package transform objects by visiting every node on a recursive walk. In version 0.6.8, it is a small package with zero dependencies. However, in version 0.6.9, it [introduces 3 new dependencies](https://github.com/ljharb/js-traverse/compare/v0.6.8...v0.6.9):

![traverse-package-diff](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852917588/bda32e69-19c3-4aa1-b9d5-1e7645ff8e3a.png?auto=compress,format&format=webp)

Those mere 3 dependencies transformed the `traverse` npm package from a zero dependency to the following dependency tree (on the right):

![traverse-npm-package-dependency-change](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852918238/962ce0f6-20bb-47a2-873e-0415b47256b4.png?auto=compress,format&format=webp)

This change introduces real-world implications on the size of the package, the number of dependencies, and the fetch time for this package:

-   The package size increased to 65.7KB (from 4KB)
-   The download time over a slow connection increased to 362ms (from 30ms)

These are significant orders of magnitude changes that can impact the developer experience, the end-user experience, and the overall performance of the application.

![new-traverse-npm-package-size](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852918980/175287da-5a1d-4919-a389-101edc737fc9.png?auto=compress,format&format=webp)

## The rise of zero dependency JavaScript

Concerned with the new traverse change, [Puru Vijay](https://github.com/puruvj) of the Svelte team, introduced [neotraverse](https://www.npmjs.com/package/neotraverse) (with a matching version structure too, i.e: 0.6.9) to provide a zero dependency alternative to the `traverse` package. The `neotraverse` package is a drop-in replacement for the `traverse` package and provides the same functionality without any dependencies.

It’s a drop-in replacement, ESM-first, no polyfills and while still maintaining the original traverse API, is aimed at being fast (and of course dependencies free).

Whether `neotraverse` get picked up by developers, or serve as a social experiment, it none-the-less an important voice of developers and maintainers in the JavaScript ecosystem who want to move towards modern JavaScript capabilities (ESM-only), and respect low or zero dependency future.

SvelteKit maintainer, [Ben McCann](https://x.com/BenjaminMcCann) made a similar zero dependency change with [svelte-preprocess](https://github.com/sveltejs/svelte-preprocess) which gets more than 400,000 weekly downloads, making its impact on downstream users and upstream consumers significant for DX:

![svelte-preprocess npm package](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852919664/888bf8ff-5fd3-4fe6-b29a-f997a0d92e5f.png?auto=compress,format&format=webp)

## Who is using Node.js 0.4 ?

The Node.js 0.4 version was released on 2011-02-10 and have long been considered EOL (End of Life) by the Node.js project.

To the naked eye, and intuitively, many developers will shake off any support needed or real-world cases of Node.js 0.4 running in production, but what about the numbers?

[Node.js runtime download metrics](https://nodejs.org/metrics/summaries/version.png) show that Node.js 0.4 still receives 120,000 daily downloads. Are these legacy servers? build and CI? regardless, these numbers are not zero and potentially make the case for supporting older versions of Node.js. Whether that is practical for every npm package, or are there better ways of doing so, is a different question.

![nodejs-runtime-daily-downloads-by-version](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852920401/7b86f91d-c122-4c81-a614-14f4d01a205b.png?auto=compress,format&format=webp)

## More efforts towards a zero dependency JavaScript world

One random example is Biome’s maintainer [Superchupu](https://x.com/superchupu), who [proposed to replace](https://github.com/egoist/tsup/pull/1158) the popular `globby` package with `fdir` and `picomatch`, both having 0 dependencies and are faster than `globby`:

![replacing globby with 0 dependency alternative](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852920953/5956e00a-d6d6-4f1e-95cf-50456e84ab21.png?auto=compress,format&format=webp)

Next example comes from [Pascal Schilp](https://x.com/passle_) who shared a post about using codemods to replace third-party dependencies that are associative with the `left-pad` incident - they’re mostly providing fundamental code that is concise and was used in the past due to missed features in the JavaScript language but no longer the case:

![codemods to replace third-party dependencies](https://cdn.hashnode.com/res/hashnode/imageupload/v1721852921414/bc4bc224-7411-4d71-bb67-b5c7bdaf3ffd.png?auto=compress,format&format=webp)

And finally, the [e18e](https://e18e.dev/) project is an initiative in the JavaScript ecosystem with aim to clean up the dependency tree of popular npm packages with stale, out of date, and a focus on speed and performance improvements.

## Conclusion

The rise of zero dependency packages like `neotraverse` and the controversy around the `axobject-query` package demonstrate the different perspectives and trade-offs that developers and maintainers need to consider when building and maintaining JavaScript applications.

We’re yet to see if zero dependency JavaScript is a moment-in-time trend or a long-term shift in the JavaScript ecosystem. Perhaps the new ESM vs CJS module support will also fuel the zero dependency trend as maintainers will not be keen to take on the burden of dual module system support, and will opt for the modern ESM-only approach, for which they may not find ESM-only modules to be compatible with.

One last note on Node.js runtime versions: The Node.js project has a [long-term support (LTS) schedule](https://nodejs.org/en/about/releases/) that provides a stable and secure platform for developers to build and deploy their applications on. You should aim to always use the latest LTS version of Node.js to benefit from the latest features, bug fixes, and security patches.
