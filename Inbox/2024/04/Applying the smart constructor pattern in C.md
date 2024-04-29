---
created: 2023-10-06T09:54:45 (UTC -03:00)
tags: []
source: https://bartwullems.blogspot.com/2023/10/applying-smart-constructor-pattern-in-c.html?ref=dailydev
author: Share
---

# Applying the smart constructor pattern in C#

> ## Excerpt
> In Domain-Driven Design (DDD), domain invariants  are fundamental principles or rules that must always hold true within a specific domain. T...

---
### Applying the smart constructor pattern in C#

In Domain-Driven Design (DDD), domain _invariants_ are fundamental principles or rules that must always hold true within a specific domain. These invariants define constraints or conditions that govern the behavior and state of the entities, value objects, and aggregates within the domain.

They emphasize the Always-valid rule:

> Your domain model should always be in a valid state.

By using invariants, your domain model guarantees that it cannot represent illegal states. These invariants **_define_** the domain class: that class is what it is because of them. Therefore, you can’t possibly violate these invariants. If you do so, the domain class would simply cease being the thing you expect it to be; it’d become something else.

As Greg Young explains it:

> A unicorn without a horn is a horse, not a unicorn.

There are multiple ways to protect those invariants. Using value objects and taking advantage of the type system are a great help here. However not all invariants can be enforced by the type system.

In that situation we can use Smart Constructors which is a function to create values only if they pass a certain criteria.

Let’s look at an example…

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="C#" data-tagsearch-path="ProductItem.cs"><tbody><tr><td id="file-productitem-cs-L1" data-line-number="1"></td><td id="file-productitem-cs-LC1"><span>public</span> <span>record</span> <span>ProductItem</span></td></tr><tr><td id="file-productitem-cs-L2" data-line-number="2"></td><td id="file-productitem-cs-LC2"><span>{</span></td></tr><tr><td id="file-productitem-cs-L3" data-line-number="3"></td><td id="file-productitem-cs-LC3"><span>public</span> <span>Guid</span> <span>ProductId</span> <span>{</span> <span>get</span><span>;</span> <span>}</span></td></tr><tr><td id="file-productitem-cs-L4" data-line-number="4"></td><td id="file-productitem-cs-LC4"></td></tr><tr><td id="file-productitem-cs-L5" data-line-number="5"></td><td id="file-productitem-cs-LC5"><span>public</span> <span>int</span> <span>Quantity</span> <span>{</span> <span>get</span><span>;</span> <span>}</span></td></tr><tr><td id="file-productitem-cs-L6" data-line-number="6"></td><td id="file-productitem-cs-LC6"></td></tr><tr><td id="file-productitem-cs-L7" data-line-number="7"></td><td id="file-productitem-cs-LC7"><span>private</span> <span>ProductItem</span><span>(</span><span>Guid</span> <span>productId</span><span>,</span> <span>int</span> <span>quantity</span><span>)</span></td></tr><tr><td id="file-productitem-cs-L8" data-line-number="8"></td><td id="file-productitem-cs-LC8"><span>{</span></td></tr><tr><td id="file-productitem-cs-L9" data-line-number="9"></td><td id="file-productitem-cs-LC9"><span>ProductId</span> <span>=</span> <span>productId</span><span>;</span></td></tr><tr><td id="file-productitem-cs-L10" data-line-number="10"></td><td id="file-productitem-cs-LC10"><span>Quantity</span> <span>=</span> <span>quantity</span><span>;</span></td></tr><tr><td id="file-productitem-cs-L11" data-line-number="11"></td><td id="file-productitem-cs-LC11"><span>}</span></td></tr><tr><td id="file-productitem-cs-L12" data-line-number="12"></td><td id="file-productitem-cs-LC12"><span>}</span></td></tr></tbody></table>

We have a small `ProductItem` value object (notice that I’m using C# record types here). As we don’t want to create the value object directly, we made the constructor private.

Now we add a static method that does the necessary validation before creating the value object:

<table data-hpc="" data-tab-size="8" data-paste-markdown-skip="" data-tagsearch-lang="C#" data-tagsearch-path="ProductItem.cs"><tbody><tr><td id="file-productitem-cs-L1" data-line-number="1"></td><td id="file-productitem-cs-LC1"><span>public</span> <span>record</span> <span>ProductItem</span></td></tr><tr><td id="file-productitem-cs-L2" data-line-number="2"></td><td id="file-productitem-cs-LC2"><span>{</span></td></tr><tr><td id="file-productitem-cs-L3" data-line-number="3"></td><td id="file-productitem-cs-LC3"><span>public</span> <span>Guid</span> <span>ProductId</span> <span>{</span> <span>get</span><span>;</span> <span>}</span></td></tr><tr><td id="file-productitem-cs-L4" data-line-number="4"></td><td id="file-productitem-cs-LC4"></td></tr><tr><td id="file-productitem-cs-L5" data-line-number="5"></td><td id="file-productitem-cs-LC5"><span>public</span> <span>int</span> <span>Quantity</span> <span>{</span> <span>get</span><span>;</span> <span>}</span></td></tr><tr><td id="file-productitem-cs-L6" data-line-number="6"></td><td id="file-productitem-cs-LC6"></td></tr><tr><td id="file-productitem-cs-L7" data-line-number="7"></td><td id="file-productitem-cs-LC7"><span>private</span> <span>ProductItem</span><span>(</span><span>Guid</span> <span>productId</span><span>,</span> <span>int</span> <span>quantity</span><span>)</span></td></tr><tr><td id="file-productitem-cs-L8" data-line-number="8"></td><td id="file-productitem-cs-LC8"><span>{</span></td></tr><tr><td id="file-productitem-cs-L9" data-line-number="9"></td><td id="file-productitem-cs-LC9"><span>ProductId</span> <span>=</span> <span>productId</span><span>;</span></td></tr><tr><td id="file-productitem-cs-L10" data-line-number="10"></td><td id="file-productitem-cs-LC10"><span>Quantity</span> <span>=</span> <span>quantity</span><span>;</span></td></tr><tr><td id="file-productitem-cs-L11" data-line-number="11"></td><td id="file-productitem-cs-LC11"><span>}</span></td></tr><tr><td id="file-productitem-cs-L12" data-line-number="12"></td><td id="file-productitem-cs-LC12"></td></tr><tr><td id="file-productitem-cs-L13" data-line-number="13"></td><td id="file-productitem-cs-LC13"><span>public</span> <span><span>static</span></span> ProductItem <span>From</span><span>(</span><span>Guid<span>?</span></span> <span>productId</span><span>,</span> <span><span>int</span><span>?</span></span> <span>quantity</span><span>)</span></td></tr><tr><td id="file-productitem-cs-L14" data-line-number="14"></td><td id="file-productitem-cs-LC14"><span>{</span></td></tr><tr><td id="file-productitem-cs-L15" data-line-number="15"></td><td id="file-productitem-cs-LC15"><span>if</span> <span>(</span><span>!</span>productId<span>.</span>HasValue<span>)</span></td></tr><tr><td id="file-productitem-cs-L16" data-line-number="16"></td><td id="file-productitem-cs-LC16"><span>throw</span> <span>new</span> ArgumentNullException<span>(</span>nameof<span>(</span>productId<span>)</span><span>)</span><span>;</span></td></tr><tr><td id="file-productitem-cs-L17" data-line-number="17"></td><td id="file-productitem-cs-LC17"></td></tr><tr><td id="file-productitem-cs-L18" data-line-number="18"></td><td id="file-productitem-cs-LC18"><span>return</span> <span>quantity</span> <span>switch</span></td></tr><tr><td id="file-productitem-cs-L19" data-line-number="19"></td><td id="file-productitem-cs-LC19"><span>{</span></td></tr><tr><td id="file-productitem-cs-L20" data-line-number="20"></td><td id="file-productitem-cs-LC20"><span>null</span> <span>=&gt;</span> <span>throw</span> <span>new</span> ArgumentNullException<span>(</span>nameof<span>(</span>quantity<span>)</span><span>)</span><span>,</span></td></tr><tr><td id="file-productitem-cs-L21" data-line-number="21"></td><td id="file-productitem-cs-LC21">&lt;= <span>0</span> <span>=&gt;</span> <span>throw</span> <span>new</span> ArgumentOutOfRangeException<span>(</span>nameof<span>(</span>quantity<span>)</span><span>,</span> <span><span>"</span>Quantity has to be a positive number<span>"</span></span><span>)</span><span>,</span></td></tr><tr><td id="file-productitem-cs-L22" data-line-number="22"></td><td id="file-productitem-cs-LC22">_ <span>=&gt;</span> <span>new</span> ProductItem<span>(</span>productId<span>.</span>Value<span>,</span> quantity<span>.</span>Value<span>)</span></td></tr><tr><td id="file-productitem-cs-L23" data-line-number="23"></td><td id="file-productitem-cs-LC23"><span>}</span><span>;</span></td></tr><tr><td id="file-productitem-cs-L24" data-line-number="24"></td><td id="file-productitem-cs-LC24"><span>}</span></td></tr><tr><td id="file-productitem-cs-L25" data-line-number="25"></td><td id="file-productitem-cs-LC25"><span>}</span></td></tr></tbody></table>

**Remark:** The example above could probably be handled using a normal constructor but you get the point.

## Learn more

[Always-Valid Domain Model · Enterprise Craftsmanship](https://enterprisecraftsmanship.com/posts/always-valid-domain-model/)

[Smart constructors - HaskellWiki](https://wiki.haskell.org/Smart_constructors)

[reason-design-patterns/patterns/smart-constructors.md at master · leostera/reason-design-patterns (github.com)](https://github.com/leostera/reason-design-patterns/blob/master/patterns/smart-constructors.md)
