---
created: 2024-07-26T15:41:36 (UTC -03:00)
tags: []
source: https://thenewstack.io/whats-new-for-javascript-developers-in-ecmascript-2024/
author: Mary Branscombe
---

# What’s New for JavaScript Developers in ECMAScript 2024 - The New Stack

> ## Excerpt
> Our analysis of the small but helpful features in the latest annual JavaScript release, including easier WebAssembly integration.

---
The ECMAScript standard for JavaScript continues to add new language features in a deliberate way. This year there’s a mix of APIs that standardize common patterns that developers have been writing by hand or importing from third-party libraries — including some aimed specifically at library authors — plus improvements in string handling, regular expressions, multithreading and WebAssembly interoperability.

Meanwhile, the TC39 committee that assesses proposals is also making progress on some of the much larger proposals, like the long-awaited [Temporal](https://thenewstack.io/javascript-forecast-whats-ahead-for-ecmascript-2022/) and [Decorators](https://thenewstack.io/the-new-javascript-features-coming-in-ecmascript-2023/) that may be ready for ECMAScript 2025, Ecma vice president [Daniel Ehrenberg](https://www.linkedin.com/in/danielehrenberg/) told The New Stack.

“Looking at what we’ve done over the past year, ECMAScript 2024 is a little similar to ECMAScript 2023 in that it sees smaller features; but meanwhile, we’re building very strongly towards these big features.” Many of those only need “the last finishing touches.”

> “You need to access the WebAssembly heap reasonably efficiently and frequently from the JavaScript side, because in real applications you will not have communication between the two.”  
> **– Daniel Ehrenberg, Ecma vice president**

In fact, since the completed feature proposals for ECMAScript 2024 were signed off in March of this year, ready for approval of the standard in July, at least one important proposal — [Set Methods](https://github.com/tc39/proposal-set-methods) — has already reached stage four, ready for 2025.

## Making Developers Happier With Promises

Although [promises](https://thenewstack.io/what-are-promises-in-javascript/) are a powerful JavaScript feature introduced in [ECMAScript 2015](https://thenewstack.io/ecmascript-6-biggest-update-javascript-yet-start-rolling-annual-improvements/), the pattern the promise constructor uses isn’t common elsewhere in JavaScript, and turned out not to be the way developers want to write code, Ehrenberg explained. “It takes some mental bandwidth to use these weird idioms.”

The hope was that over time on the web platform, enough APIs would natively return promises instead of callbacks that developers wouldn’t often need to use the promise constructor. However, existing APIs haven’t changed to be more ergonomic.

> “It comes up at least once in every project. Almost every project was writing this same little helper so it being in the language is one of those really nice developer happiness APIs.”  
> **– Ashley Claymore, Bloomberg software engineer**

Instead, developers are left with a cumbersome workaround that many libraries, frameworks and other tools — from React to TypeScript — have implemented different versions of: it’s in jQuery as the deferred function. “People have this boilerplate pattern that they have to write over and over again, where they call the promise constructor, they get the resolve and reject callbacks, they write those to a variable, and then they inevitably do something else \[with them\]. It’s just an annoying piece of code to write,” said Ehrenberg.

Libraries that implemented promises before ECMAScript 2015 typically covered this, but the feature didn’t make it into the language; Chrome briefly supported and then removed a similar option. But developers still need this often enough that the [Promise.withResolvers](https://github.com/tc39/proposal-promise-with-resolvers) proposal to add a static method made it through the entire TC39 process in the twelve months between ECMAScript 2023 being finalized and the cutoff date for this year’s update to the language — an achievement so unusual that TC-39 co-chair [Rob Palmer](https://www.linkedin.com/in/robpalmer2) referred to it as a speedrun.

“Previously, when you created a promise, the ways that you resolve it and you give it its final state were APIs only accessible inside the function that you built the promise with,” Ehrenberg continued. “Promise.withResolvers gives you a way to create a promise and it gives you direct access to those resolution functions.”

Other functions in your code might depend on whether a promise is resolved or rejected, or you might want to pass the function to something else that can resolve the promise for you, reflecting the complex ways promises are used for orchestration in modern JavaScript, [Ashley Claymore](https://www.linkedin.com/in/ashleycutmore/) (a Bloomberg software engineer who has worked on multiple TC39 proposals) suggested.

“The classical way of creating a promise works well when it’s a small task that’s asynchronous; taking something that was purely callback based or something that was promise-like, and then wrapping it up so it was actually a promise,” Claymore said. “In any code where I start doing lots of requests and need to line them up with IDs from elsewhere, so I’m putting promises or resolve functions inside a map because I’m orchestrating lots of async things that aren’t promise based, you’re always having to do this. I need to pull these things out because I’m sending them to different places.”

“It comes up at least once in every project. Almost every project was writing this same little helper so it being in the language is one of those really nice developer happiness APIs.”

TRENDING STORIES

1.  [What’s New for JavaScript Developers in ECMAScript 2024](https://thenewstack.io/whats-new-for-javascript-developers-in-ecmascript-2024/)
2.  [Top 10 JavaScript Libraries To Use in 2024](https://thenewstack.io/top-10-javascript-libraries-to-use-in-2024/)
3.  [Pivoting From React to Native DOM APIs: A Real World Example](https://thenewstack.io/pivoting-from-react-to-native-dom-apis-a-real-world-example/)
4.  [How To Master JavaScript Performance Optimization](https://thenewstack.io/how-to-master-javascript-performance-optimization/)
5.  [Convert a Google Spreadsheet to JSON-Formatted Text](https://thenewstack.io/how-to-convert-google-spreadsheet-to-json-formatted-text/)

Other improvements to promises are much further down the line; Claymore is involved in [a proposal to simplify combining multiple promises](https://github.com/tc39/proposal-await-dictionary) without using an array — which involves keeping track of which order all the promises are in. “That works fine for like one, two or three things: after that, it can start getting harder to follow the code,” he said. “What was the fifth thing? You’re counting lines of code to make sure you’ve got the right thing.”

Having an Await dictionary of Promises would let developers name promises: particularly helpful when they cover different areas — like gathering user information, database settings and network details that likely return at very different times. This wouldn’t be a difficult feature to develop: the delay is deciding whether it’s useful enough to be in the language because the TC39 committee wants to avoid adding too many niche features that could confuse new developers.

## Keeping Compatibility and Categorizing Data

That’s not a concern for the second major feature in ECMAScript 2024, a new way to categorize objects into categories using [Array grouping](https://github.com/tc39/proposal-array-grouping): something common in other languages (including SQL) that developers frequently import the [Lodash](https://lodash.com/) userland library for.

You can pass in different items and classify them by some property, like color. “The result is a key value dictionary that is indexed by ‘here’s all your green things, here are your orange things’ and that dictionary can be expressed either as an object or a map”, Palmer explained. Use a map if you want to group keys that aren’t only strings and symbols; to extract multiple data values at the same time (known as destructuring), you need an object.

> “As a standards committee we shouldn’t be asking them to incur the cost of risking outages when we already know that something is highly risky.”  
> **– Ehrenberg**

That’s useful for everything from bucketing performance data about your web site to grouping a list of settled promises by status, a common use with Promise.allSettled, Claymore said. “You give it an array of promises, it will wait for all of them to settle, then you get back an array of objects that says, ‘did this reject or did it resolve?’ They’re in the same order as you started, but it’s quite common to have one bit of code I want to give all the promises that were successful and resolved, and another bit of code I want to give rejected \[promises\].” For that you can pass the result of Promise.allSettled to groupBy to group by promise status, which groups all the resolved promises and all the rejected promises separately.

Building the new grouping functionality also delivered a useful lesson about web compatibility.

The utilities in Lodash are functionality that developers could write in 5-10 lines of code, Palmer noted. “But when you look at the frequency at which they’re used, they’re so widely used by so many programs that at some point it’s worth taking the top usage ones and then putting them in the platform, so people don’t have to write their own.” A number of them have now ended up as native constructs.

“This functionality being in the language is a really nice convenience for projects that are trying not to have a large set of dependencies while still having access to these really common things,” Claymore agreed. “They’re not the hardest things to write by hand, but it’s no fun rewriting them by hand and they can subtly get them wrong.”

Unusually, the new Map.groupBy and Object.groupBy methods are static methods rather than array methods, the way Lodash functionality has previously been added to JavaScript. That’s because two previous attempts to add this functionality as array methods both clashed (in different ways) with existing code on websites already using the same two names the proposal came up with, including the popular [Sugar](https://sugarjs.com/) library.

This problem could recur any time TC39 proposals try to add new prototype methods to arrays or instance methods, Palmer warned. “Whenever you try and think of any reasonable English language verb you might want to add, it seems it triggers web compatibility problems somewhere on the internet.”

Ideally, good coding standards would avoid that, but part of the reason it takes time to add new features to JavaScript is the need to test for exactly these kinds of issues and work around them when they crop up.

“We can say that the best practice for the web is that users should not be polluting the global prototypes: people should not be adding properties to array or prototype \[in their code\], because it can lead to web compatibility issues. But it doesn’t matter how much we say that; these sites are already up there, and we have a responsibility to not break them.”

Shipping then withdrawing implementations in browsers makes it more expensive to add new features, Ehrenberg added. “As a standards committee, we shouldn’t be asking them to incur the cost of risking outages when we already know that something is highly risky.” That means similar proposals might use static methods more in the future to avoid the issue.

## Updating JavaScript for Better Unicode Handling

JavaScript already has a /u flag for regexp that needs to handle Unicode (introduced in ECMAScript 2015), but that turned out to have some oddities and missing features. The new [/v flag](https://github.com/tc39/proposal-regexp-v-flag) fixes some of those (like getting different results if you use an upper or lowercase character when matching, even if you specify that you don’t care about the case) and forces developers to escape special characters. It also allows you to do [more complex pattern matching and string manipulation](https://v8.dev/features/regexp-v-flag) using a new unicodeSets mode, which lets you name Unicode sets so that you can refer to the ASCII character set or the emoji character set.

The new options will simplify internationalization and make it easier to support features for diversity. The /u flag already lets you refer to emoji, but only if they were only a single character — excluding emoji that combine multiple characters to get a new emoji or to specify the gender or skin tone of an emoji representing a person, and even some country flags.

It also simplifies operations like sanitizing or verifying inputs, by adding more set operations including intersections and nesting, making complex regular expressions more readable. “It adds subtraction so you could say, for example, ‘all the ASCII characters’, but then subtract the digits zero to nine, and that would match a narrower range than all of the ASCII characters,” Palmer explained. You could remove invisible characters or convert numbers expressed as words into digits.

> “It’s easy to make assumptions about Unicode, and it’s such a big topic; the number of people in the world \[who\] understand these things well enough to not make mistakes is very small.”  
> **– Claymore**

You can also match against various properties of strings, such as what script they’re written in, so that you can find characters like π and treat them differently from p in another language.

You can’t use the /u flag and the /v flag together and you will probably always want to use /v. Palmer described the choice as “the /v flag enables all the good parts of the/ u flag with new features and improvements, but some of them are backwards incompatible with the /u flag.”

ECMAScript 2025 will add another useful improvement for regexp: being able to [use the same names in different branches of a regexp](https://github.com/tc39/proposal-duplicate-named-capturing-groups). Currently, if you’re writing a regular expression to match something that can be expressed in multiple ways, like the year in a date that might be /2024 or just /24, you can’t use ‘year’ in both branches of the regular expression, even though only one branch can ever match, so you have to say ‘year1’ and ‘year2’ or ‘shortyear’ and ‘longyear’.

“Now we say that’s no longer an error, and you can have multiple parts of the regular expression given the same name, as long as they are on different branches and as long as only one of them can ever be matched,” Claymore explained.

Another new feature in ECMAScript 2024 improves Unicode handling by ensuring that code is using [well-formed Unicode strings](https://github.com/tc39/proposal-is-usv-string).

Strings in JavaScript are technically UTF-16 encoded: in practice, JavaScript (like a number of other languages) [doesn’t enforce that those encodings are valid UTF-16](https://www.wingolog.org/archives/2023/10/19/requiem-for-a-stringref) even though that’s important for the URI API and the WebAssembly Component Model, for example. “There are various APIs in the web platform that need well-formed strings and they might throw an error or silently replace the string if they get bad data,” Palmer explained.

Because it’s possible for valid JavaScript code to use strings that are invalid UTF sequences, developers need ways to check for that. The new isWellFormed method checks that a JavaScript string is correctly encoded; if not, the new .toWellFormed method fixes the string by replacing anything that isn’t correctly encoded with the 0xFFFD [replacement character](https://unicodeplus.com/U+FFFD) �.

While experienced Unicode developers could already write checks for this, “It’s very easy to get wrong,” Claymore noted. “It’s easy to make assumptions about Unicode, and it’s such a big topic; the number of people in the world that actually properly understand these things well enough to not make mistakes is very small. This encourages people to fall into the pit of success rather than try and do these things by hand and make mistakes because of not knowing all the edge cases.”

Having it in the language itself might even prove more efficient, Palmer suggested. “Potentially, one of the advantages of delegating this to the JavaScript engine is that it might be able to find faster ways to do this check. You could imagine, for example, it might just cache a single bit of information with every string to say ’I’ve already checked this, this string is not good’ so that every time you pass it somewhere that needs a good string, it doesn’t need to walk every character to check it again, but just look at that one bit.”

## Adding Locks With Async Code

> “On the main thread, where you’re providing interactivity to the user, it’s one of the rules of the web: thou shalt not halt the main thread!”  
> **– Rob Palmer, TC-39 co-chair**

JavaScript is technically a single-threaded language that supports multithreading and asynchronous code. That’s because as well as having web workers and service workers that are isolated from the main thread that provides the user interface, it’s the nature of the web that quite often you’re waiting for something from the network or the operating system, so the main thread can run other code.

The tools in JavaScript for managing this continue to get more powerful with a new option in ECMAScript 2024, [Atomics.waitAsync](https://github.com/tc39/proposal-atomics-wait-async).

“If you want to do multithreading in JavaScript, we have web workers, you can spin up another thread, and the model that was originally based on is message passing, which is nice and safe,” Palmer explained. “But for people \[who\] want to go faster, where that becomes a bottleneck, shared memory is the more raw, lower-level way of having these threads cooperate on shared data, and SharedArrayBuffer was introduced long ago to permit this memory sharing. And when you’ve got a multithreaded system with shared memory, you need locks to make sure that you got orderly operations.”

“When you wait, you lock up the thread, it can’t make any progress. And that’s fine on a worker thread. On the main thread, where you’re providing interactivity to the user, it’s one of the rules of the web: thou shalt not halt the main thread!”

Async avoids blocking the main thread because it can move on to any other tasks that are ready to go, like loading data that has come in from the network, and the Atomics.wait API offers event-based waiting when you’re not on the main thread. But sometimes you do want the main thread to wait for something.

“Even if you’re not on the main thread, you shouldn’t be blocking most of the time,” Ehrenberg warned, while noting that “it was important for game engines to be allowed to block when they’re not on the main thread, to be able to recreate what they could do in C++ code bases.”

Developers who need this have created workarounds for this, again using message passing, but this had some overheads and slowed things down. Atomics.waitAsync can be used on the main thread and provides a first-class way of waiting on a lock. “The key thing is that it doesn’t stall the main thread,” Palmer said.

“If you call it and the lock is not ready, it will instead give you a backup promise so \[that\] you can use regular async.await and treat it just like any other promise. This solves how to have high-performance access operations on locks.”

Another proposal still in development takes a slightly different approach to waiting on the main thread and would be useful for making multithreaded applications written in Emscripten more efficient. [Atomics.pause](https://github.com/tc39/proposal-atomics-microwait) promises ‘microwaits’ that can be called from the main thread or worker threads. “It does block, but it’s limited in how long it can block for,” Claymore told us.

Most JavaScript developers won’t use either of these options directly, Palmer pointed out: “It’s very hard to write threaded code.” But mutex libraries would likely rely on it, as might tools for compiling to WebAssembly.

“We can all benefit from this, even if we’re not using it directly.”

## Easier WebAssembly Integration

Another proposal for adding a JavaScript API for features previously handled in DOM APIs or available to WebAssembly bytecode, but not JavaScript, is [resizable array buffers](https://github.com/tc39/proposal-resizablearraybuffer).

WebAssembly and JavaScript programs need to share memory. “You need to access the WebAssembly heap reasonably efficiently and frequently from the JavaScript side, because in real applications you will not have communication between the two sides; they’re largely sharing their information via the heap,” Ehrenberg explained.

> “If you’ve got a WebAssembly toolchain like Emscripten, it means it can do this without creating wrapper objects.”  
> **– Palmer**

WebAssembly memory can grow if required: but if it does and you want to access it from JavaScript, that means detaching the ArrayBuffer you’re using for handling binary data in memory and building a new TypedArray over the underlying ArrayBuffer next time you need to access the heap. That’s extra work that fragments the memory space on 32-bit systems.

Now you can create a new type of array buffer that’s resizable, so it can grow or shrink without needing to be detached.

“If you’ve got a WebAssembly toolchain like [Emscripten](https://emscripten.org/), it means it can do this without creating wrapper objects, which just add inefficiencies,” Palmer added. Again, while few JavaScript developers will use this directly, libraries will likely use resizable arrays for efficiency and no longer need to work around a missing part of the language to do it — making them smaller and faster to load by reducing the amount of code that needs to be downloaded.

Developers have to explicitly choose this, because array buffers and typed arrays are the most common way hackers try to attack browsers, and making the existing ArrayBuffer resizable would mean changing a lot of code that’s been extensively tested and hardened, possibly creating bugs and vulnerabilities.

“That detach operation was initially created to enable transfer to and from workers,” Ehrenberg explained. “When you send an ArrayBuffer via a POST message to transfer it to another worker, the ownership transfers and all the typed arrays that are closing over that ArrayBuffer become detached.” A companion proposal lets developers [transfer the ownership of array buffers](https://github.com/tc39/proposal-arraybuffer-transfer) so they can’t be tampered with.

“If you pass it into a function, the receiver can use this transfer function to acquire ownership, so anyone who used to have a handle to it no longer has it,” Palmer explained. “That’s good for integrity.”

As well as being part of ECMAScript 2024, resizable array buffers have been integrated into the WebAssembly JS API; Ehrenberg called this out as an example of collaboration between different communities in the web ecosystem that’s working well.

“These are efforts that span multiple different standards committees and require explicit cooperation among them. That can be complicated because you have to bring a lot of people in the loop, but ultimately, I think it leads to \[a\] good design process. You get a lot of vetting.”
