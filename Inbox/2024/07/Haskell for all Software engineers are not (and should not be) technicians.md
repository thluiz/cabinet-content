---
created: 2024-07-24T19:35:21 (UTC -03:00)
tags: []
source: https://www.haskellforall.com/2024/07/software-engineers-are-not-and-should.html?utm_source=tldrnewsletter
author: 
---

# Haskell for all: Software engineers are not (and should not be) technicians

> ## Excerpt
> Software engineers are not (and should not be) technicians          I don’t actually think predictability is a goo...

---
I don’t actually think predictability is a good thing in software engineering. This will probably come as a surprise to some people (especially managers), but I’ll explain what I mean.

In my view, a great software engineer is one who automates repetitive/manual labor. You would think that this is a pretty low bar to clear, right? Isn’t automation of repetitive tasks … like … programming 101? Wouldn’t most software engineers be great engineers according to my criterion?

No.

I would argue that **most large software engineering organizations incentivize anti-automation** and it’s primarily because of their penchant for predictability, especially predictable estimates and predictable work. The reason this happens is that **predictable work is work that could have been automated but was not automated**.

#### Example

I’ll give a concrete example of predictable work from my last job. Early on we had a dedicated developer for maintaining our web API. Every time some other team added a new gRPC API endpoint to an internal service this developer was tasked with exposing that same information via an HTTP API. This was a fairly routine job but it still required time and thought on their part.

Initially managers liked the fact that this developer could estimate reliably (because the work was well-understood) and this developer liked the fact that they didn’t have to leave their comfort zone. **But it wasn’t great for the business!** This person frequently became a bottleneck for releasing new features because they had inserted their own manual labor as a necessary step in the development pipeline. They made the case that management should hire _more_ such developers like themselves to handle increased demand for their work.

Our team pushed back on this because we recognized that this developer’s work was so predictable that it could be completely automated. We made the case to management that rather than hiring another person to do the same work we should be automating more and it’s a good thing we did; that developer soon left the company and instead of hiring to replace them we automated away their job instead. We wrote some code to automatically generate an HTTP API from the corresponding gRPC API[<sup>1</sup>](https://www.haskellforall.com/2024/07/software-engineers-are-not-and-should.html?utm_source=tldrnewsletter#fn1) and that generated much more value for the business than hiring a new developer.

#### Technicians vs Engineers

I like to use the term “technician” to describe a developer who (A) does work that is well-understood and (B) doesn’t need to leave their comfort zone very often. Obviously there is not a bright line dividing engineers from technicians, but generally speaking the more predictable and routine a developer’s job the more they tend to slide into becoming a technician. In the above example, I viewed the developer maintaining the web API as more of a technician than an engineer.

In contrast, the more someone leans into being an engineer the more unpredictable their work gets (along with their estimates). If you’re consistently automating things then all of the predictable work slowly dries up and all that’s left is unpredictable work. The nature of a software engineer’s job is that they are tackling increasingly challenging and ambitious tasks as they progressively automate more.

I believe that most tech companies should not bias towards predictability and should avoid hiring/cultivating technicians. The reason that tech companies command outsized valuations is because of automation. Leaning into predictability and well-understood work inadvertently incentivizes manual labor instead of automation. This isn’t obvious to a lot of tech companies because they assume any work involving code is necessarily automation but that’s not always the case[<sup>2</sup>](https://www.haskellforall.com/2024/07/software-engineers-are-not-and-should.html?utm_source=tldrnewsletter#fn2). Tech companies that fail to recognize this end up over-hiring and wondering why less work is getting done with more people.

Or to put it another way: I actually view it as a red flag if an engineer or team gets into a predictable “flow” because it means that there is a promising opportunity for automation they’re ignoring.

___

1.  Nowadays there are off-the-shelf tools to do this like [`grpc-gateway`](https://github.com/grpc-ecosystem/grpc-gateway) but this wasn’t available to us at the time.[↩︎](https://www.haskellforall.com/2024/07/software-engineers-are-not-and-should.html?utm_source=tldrnewsletter#fnref1)
    
2.  … or even usually the case; I’m personally very cynical about the engineering effectiveness of most tech companies.[↩︎](https://www.haskellforall.com/2024/07/software-engineers-are-not-and-should.html?utm_source=tldrnewsletter#fnref2)
