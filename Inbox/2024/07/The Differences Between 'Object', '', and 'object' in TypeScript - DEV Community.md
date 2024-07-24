---
created: 2024-07-24T09:23:05 (UTC -03:00)
tags: [typescript,webdev,javascript,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/zacharylee/the-differences-between-object-and-object-in-typescript-f6f?context=digest
author: 
---

# The Differences Between 'Object', '', and 'object' in TypeScript - DEV Community

> ## Excerpt
> In TypeScript, when we want to define an object type, there are several concise options such as...

---
In TypeScript, when we want to define an object type, there are several concise options such as 'Object', '{}', and 'object'. What are the differences between them?

## [](https://dev.to/zacharylee/the-differences-between-object-and-object-in-typescript-f6f?context=digest#object-uppercased)Object (uppercased)

Object (uppercased) describes properties common to all JavaScipt objects. It is defined in the [_lib.es5.d.ts_](https://github.com/microsoft/TypeScript/blob/main/src/lib/es5.d.ts?utm_source=webdeveloper.beehiiv.com&utm_medium=newsletter&utm_campaign=the-differences-between-object-and-object-in-typescript#L105) file that comes with the TypeScript library.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--snngkkMR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252F502bd58d-5056-43bb-949a-b4eea66fad4a_1117x834.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--snngkkMR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252F502bd58d-5056-43bb-949a-b4eea66fad4a_1117x834.png)

As you can see, it includes some common properties like `toString()`, `valueOf()`, and so on.

Because it emphasizes only those properties that are common to JavaScript objects. So you can assign boxable objects like `string`, `boolean`, `number`, `bigint`, `symbol` to it, but not the other way around.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--BZW1fgd---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252F35260e1a-c0a0-4e17-ae27-46bf22377fc9_661x484.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--BZW1fgd---/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252F35260e1a-c0a0-4e17-ae27-46bf22377fc9_661x484.png)

## [](https://dev.to/zacharylee/the-differences-between-object-and-object-in-typescript-f6f?context=digest#)**{}**

`{}` describes an object that has no members of its own, which means TypeScript will complain if you try to access its property members:

[![](https://res.cloudinary.com/practicaldev/image/fetch/s---wbN6CCc--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252Fbfe9dc4d-c569-4754-9da2-e98199db85a6_720x1027.png)](https://res.cloudinary.com/practicaldev/image/fetch/s---wbN6CCc--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252Fbfe9dc4d-c569-4754-9da2-e98199db85a6_720x1027.png)

From the code example above, we can see that `{}` and `Object` (uppercased) have the same features. That is, it can only access those properties that are common (even if the JavaScript code logic is correct), all boxable objects can be assigned to it, etc.

This is because the `{}` type can access those common properties through the prototype chain, and it also has no own properties. So it behaves the same as the `Object` (uppercased) type. But they represent different concepts.

## [](https://dev.to/zacharylee/the-differences-between-object-and-object-in-typescript-f6f?context=digest#object-lowercased)**object (lowercased)**

object (lowercased) means any non-primitive type, which is expressed in code like this:  

```
<span>type</span> <span>PrimitiveType</span> <span>=</span>
  <span>|</span> <span>undefined</span>
  <span>|</span> <span>null</span>
  <span>|</span> <span>string</span>
  <span>|</span> <span>number</span>
  <span>|</span> <span>boolean</span>
  <span>|</span> <span>bigint</span>
  <span>|</span> <span>symbol</span><span>;</span>

<span>type</span> <span>NonPrimitiveType</span> <span>=</span> <span>object</span><span>;</span>
```

This means that all non-primitive types are not assignable to it, and vice versa.

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--jR0y3b1d--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252F755f5846-0a2c-45d2-a541-fb10367bdd30_699x610.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--jR0y3b1d--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://substackcdn.com/image/fetch/w_1456%2Cc_limit%2Cf_auto%2Cq_auto:good%2Cfl_progressive:steep/https%253A%252F%252Fsubstack-post-media.s3.amazonaws.com%252Fpublic%252Fimages%252F755f5846-0a2c-45d2-a541-fb10367bdd30_699x610.png)

## [](https://dev.to/zacharylee/the-differences-between-object-and-object-in-typescript-f6f?context=digest#snack-record)Snack: Record

In the source code of many common libraries, we may see `Record<string, any>` to represent non-primitive types. It has the same effect as object (lowercased), but it is more semantic.

___

_If you find my content helpful, please **[consider subscribing](https://webdeveloper.beehiiv.com/subscribe?utm_source=webdeveloper.beehiiv.com&utm_medium=newsletter&utm_campaign=the-differences-between-object-and-object-in-typescript)**. I send a **weekly newsletter every Sunday** with the latest web development updates. Thanks for your support!_
