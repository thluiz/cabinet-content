---
created: 2024-01-03T15:31:55 (UTC -03:00)
tags: [javascript,interview,software,coding,development,engineering,inclusive,community]
source: https://dev.to/polakshahar/interview-can-you-stop-foreach-in-javascript-57h0
author: Shahar Polak
---

# Interview: Can You Stop “forEach” in JavaScript? - DEV Community

> ## Excerpt
> The Saga of JavaScript's Most Persistent Loop  Interviewer: "Can you stop a relentless forEach loop...

---
[![Cover image for Interview: Can You Stop “forEach” in JavaScript?](https://res.cloudinary.com/practicaldev/image/fetch/s--OKmp7IGB--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tsk1q929l6y131mrh5o0.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--OKmp7IGB--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tsk1q929l6y131mrh5o0.png)

_The Saga of JavaScript's Most Persistent Loop_

**Interviewer:** "Can you stop a relentless `forEach` loop in JavaScript?"

That question felt like being asked to stop the Earth from spinning. There I was, in an interview room that suddenly felt as hot as a server room with broken AC.

My answer? A hesitant, "No, that's against the laws of JavaScript nature."

Cue the sound of my job prospects plummeting.

Driven by a mix of desperation and a fleeting hope of redemption, I quizzed the interviewer, "Is there some kind of secret `forEach` kill-switch I'm not aware of?"

Before he could unveil the mysteries of JavaScript, I jumped in with what I knew.

**Let's Spin a New Tale with Code**

Here's a fun little array, just hanging out, minding its own business:  

```
<span>const</span> <span>friendlyArray</span> <span>=</span> <span>[</span><span>5</span><span>,</span> <span>4</span><span>,</span> <span>3</span><span>,</span> <span>2</span><span>,</span> <span>1</span><span>,</span> <span>0</span><span>,</span> <span>-</span><span>1</span><span>,</span> <span>-</span><span>2</span><span>,</span> <span>-</span><span>3</span><span>];</span>

<span>friendlyArray</span><span>.</span><span>forEach</span><span>((</span><span>number</span><span>)</span> <span>=&gt;</span> <span>{</span>
  <span>if </span><span>(</span><span>number</span> <span>&lt;=</span> <span>0</span><span>)</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Stopping? Nope! Number:</span><span>"</span><span>,</span> <span>number</span><span>);</span>
    <span>return</span><span>;</span> <span>// You might think this would stop it, but nope!</span>
  <span>}</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Number:</span><span>"</span><span>,</span> <span>number</span><span>);</span>
<span>});</span>
```

What does this do? It prints every number, and the `return` is as effective as a screen door on a submarine.

The interviewer, still holding onto the fantasy of stopping `forEach`, wasn't buying it.

**Crafting a `forEach` Alternative**

Time to roll up the sleeves and brew a new `forEach` concoction.  

```
<span>Array</span><span>.</span><span>prototype</span><span>.</span><span>forEachButWithStyle</span> <span>=</span> <span>function </span><span>(</span><span>callback</span><span>)</span> <span>{</span>
  <span>if </span><span>(</span><span>typeof</span> <span>callback</span> <span>!==</span> <span>'</span><span>function</span><span>'</span><span>)</span> <span>{</span>
    <span>throw</span> <span>new</span> <span>Error</span><span>(</span><span>`Hey, </span><span>${</span><span>callback</span><span>}</span><span> is not a magical function!`</span><span>);</span>
  <span>}</span>

  <span>for </span><span>(</span><span>let</span> <span>i</span> <span>=</span> <span>0</span><span>;</span> <span>i</span> <span>&lt;</span> <span>this</span><span>.</span><span>length</span><span>;</span> <span>i</span><span>++</span><span>)</span> <span>{</span>
    <span>callback</span><span>(</span><span>this</span><span>[</span><span>i</span><span>],</span> <span>i</span><span>,</span> <span>this</span><span>);</span>
    <span>// Still no stopping - it's like a party that never ends!</span>
  <span>}</span>
<span>};</span>
```

Guess what? Even in this custom-built, shiny new `forEachButWithStyle`, there's no emergency exit. It's like a merry-go-round that's lost its off switch.

**The Plot Twist: Stopping `forEach` with a `throw Error`**

Just when you thought `forEach` was the marathon runner of JavaScript, never stopping for a break, I discovered a sneaky little trick. Picture this: a `forEach` loop, prancing around like it owns the place, and then suddenly, BAM! We throw an error, and it's like hitting the emergency stop button on a runaway carousel.

Here's the trick in action:  

```
<span>const</span> <span>array</span> <span>=</span> <span>[</span><span>1</span><span>,</span> <span>2</span><span>,</span> <span>3</span><span>,</span> <span>4</span><span>,</span> <span>5</span><span>];</span>

<span>try</span> <span>{</span>
  <span>array</span><span>.</span><span>forEach</span><span>((</span><span>number</span><span>)</span> <span>=&gt;</span> <span>{</span>
    <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Number:</span><span>"</span><span>,</span> <span>number</span><span>);</span>
    <span>if </span><span>(</span><span>number</span> <span>===</span> <span>3</span><span>)</span> <span>{</span>
      <span>throw</span> <span>new</span> <span>Error</span><span>(</span><span>"</span><span>Oops! Stopping the loop.</span><span>"</span><span>);</span>
    <span>}</span>
  <span>});</span>
<span>}</span> <span>catch </span><span>(</span><span>error</span><span>)</span> <span>{</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>"</span><span>Caught an error:</span><span>"</span><span>,</span> <span>error</span><span>.</span><span>message</span><span>);</span>
<span>}</span>
```

In this daring feat, when we hit the number 3, we throw an error. It's like setting off a flare in the middle of our `forEach` party. The loop screeches to a halt, and control is passed to the `catch` block. It's not the most graceful exit, more like jumping off a moving merry-go-round, but hey, it stops the loop!

**But Wait, There's a Catch!**

While this method does stop the loop, it's like using a sledgehammer to crack a nut. Throwing an error just to stop a loop is like cancelling the entire circus because you don't like clowns. It's a bit dramatic and can lead to other complications in your code.

So, there you have it. The `forEach` loop can indeed be stopped, but it's a bit like stopping a train with a boulder on the tracks. Effective, but not exactly recommended for everyday use.

Remember, with great power comes great responsibility. Use the `throw Error` method wisely, and maybe keep it as your secret JavaScript party trick.

**The Conclusion**

So, is it possible to stop a forEach loop in JavaScript? Well, technically, yes, but it's a bit like stopping a carousel with a roadblock. You can abruptly halt it with a throw Error, but it's more of a last-resort trick than a graceful solution.

It's like being at a party you can't leave, and the only way out is setting off the fire alarm – effective, but you'll definitely raise some eyebrows.

Remember, the next time someone asks you about stopping forEach, you can now give them a sly smile and share this whimsically mischievous tale. While forEach might seem like the Energizer Bunny of JavaScript, we've got a little trick up our sleeves to bring it to a standstill – just use it wisely!
