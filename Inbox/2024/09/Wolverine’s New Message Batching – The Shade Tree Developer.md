---
created: 2024-09-05T17:44:48 (UTC -03:00)
tags: []
source: https://jeremydmiller.com/2024/09/03/wolverines-new-message-batching/
author: jeremydmiller
---

# Wolverine’s New Message Batching – The Shade Tree Developer

> ## Excerpt
> Hey, did you know that JasperFx Software offers both consulting services and support plans for the “Critter Stack” tools? The new feature shown in this post was done at the behest of a …

---
[![](https://jeremydmiller.com/wp-content/uploads/2024/09/a-wolverine-on-a-fallen-log-in-a-forest.png)](https://jeremydmiller.com/wp-content/uploads/2024/09/a-wolverine-on-a-fallen-log-in-a-forest.png)

_Hey, did you know that [JasperFx Software](https://jasperfx.net/) offers both consulting services and support plans for the “Critter Stack” tools? The new feature shown in this post was done at the behest of a JasperFx support customer. And of course, we’re also more than happy to help you with any kind of .NET backend development:)_

[Wolverine](https://wolverinefx.net/)‘s new 3.0.0-beta-1 release adds a long requested feature set for batching up message handling. What does that mean? Well, sometimes you might want to process a stream of incoming messages in batches rather than one at a time. This might be for performance reasons, or maybe there’s some kind of business logic that makes more sense to calculate for batches, or maybe you want a logical [“debounce”](https://medium.com/@jamischarles/what-is-debouncing-2505c0648ff1) in how your system responds to the incoming messages so you don’t update some resource on every single message received by your system.

To that end, [Wolverine 3.0](https://wolverinefx.net/guide/migration#key-changes-in-3-0) introduces a [new feature for batching up message handling](https://wolverinefx.net/guide/handlers/batching.html) within a Wolverine system. Let’s say that you have a message type in your system like this:

<table><tbody><tr><td><p>1</p></td><td><div><p><code>public</code> <code>record Item(</code><code>string</code> <code>Name);</code></p></div></td></tr></tbody></table>

And for whatever reason, we need to process these messages in batches. To do that, we first need to have a message handler for an array of `Item` like so:

<table><tbody><tr><td><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p></td><td><div><p><code>public</code> <code>static</code> <code>class</code> <code>ItemHandler</code></p><p><code>{</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>public</code> <code>static</code> <code>void</code> <code>Handle(Item[] items)</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>{</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>}</code></p><p><code>}</code></p></div></td></tr></tbody></table>

And yes, before you ask, so far Wolverine **only** understands an array of the batched message type as the input message for the batch handler.

With that in our system, now we need to tell Wolverine to group `Item` messages, and we do that with the following syntax:

<table><tbody><tr><td><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p><p>22</p><p>23</p><p>24</p><p>25</p></td><td><div><p><code>theHost = </code><code>await</code> <code>Host.CreateDefaultBuilder()</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>.UseWolverine(opts =&gt;</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>{</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>opts.BatchMessagesOf&lt;Item&gt;(batching =&gt;</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>{</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>batching.BatchSize = 500;</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>batching.LocalExecutionQueueName = </code><code>"items"</code><code>;</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>batching.TriggerTime = 1.Seconds();</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>})</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>.Sequential();</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>}).StartAsync();</code></p></div></td></tr></tbody></table>

_A particularly lazy and hopefully effective technique in OSS project documentation is to take code snippets directly out of test code, and that’s what you see above. Two birds with one stone. Sometimes that works out well._

And that’s that! Just to bring this a little more into focus, here’s an end to end test from the Wolverine codebase — which just for clarity, is using [Wolverine’s built in test automation support for end to end testing](https://wolverinefx.net/guide/testing.html):

<table><tbody><tr><td><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p><p>7</p><p>8</p><p>9</p><p>10</p><p>11</p><p>12</p><p>13</p><p>14</p><p>15</p><p>16</p><p>17</p><p>18</p><p>19</p><p>20</p><p>21</p><p>22</p><p>23</p><p>24</p><p>25</p><p>26</p><p>27</p><p>28</p><p>29</p><p>30</p><p>31</p><p>32</p><p>33</p><p>34</p><p>35</p><p>36</p></td><td><div><p><code>[Fact]</code></p><p><code>public</code> <code>async</code> <code>Task send_end_to_end_with_batch()</code></p><p><code>{</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>var</code> <code>item1 = </code><code>new</code> <code>Item(</code><code>"one"</code><code>);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>var</code> <code>item2 = </code><code>new</code> <code>Item(</code><code>"two"</code><code>);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>var</code> <code>item3 = </code><code>new</code> <code>Item(</code><code>"three"</code><code>);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>var</code> <code>item4 = </code><code>new</code> <code>Item(</code><code>"four"</code><code>);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>Func&lt;IMessageContext, Task&gt; publish = </code><code>async</code> <code>c =&gt;</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>{</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>await</code> <code>c.PublishAsync(item1);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>await</code> <code>c.PublishAsync(item2);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>await</code> <code>c.PublishAsync(item3);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>await</code> <code>c.PublishAsync(item4);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>};</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>var</code> <code>session = </code><code>await</code> <code>theHost.TrackActivity()</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>.WaitForMessageToBeReceivedAt&lt;Item[]&gt;(theHost)</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</code><code>.ExecuteAndWaitAsync(publish);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>var</code> <code>items = session.Executed.SingleMessage&lt;Item[]&gt;();</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>items.Length.ShouldBe(4);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>items.ShouldContain(item1);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>items.ShouldContain(item2);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>items.ShouldContain(item3);</code></p><p><code>&nbsp;&nbsp;&nbsp;&nbsp;</code><code>items.ShouldContain(item4);</code></p><p><code>}</code></p></div></td></tr></tbody></table>

Alright, with all that being said, here’s a few more facts about the batch messaging support:

1.  There is absolutely no need to create a specific message handler for the `Item` message, and in fact, you should not do so
2.  The message batching is able to group the message batches by tenant id _if_ your Wolverine system uses multi-tenancy
3.  If you are using a durable inbox in your system, Wolverine is not marking the incoming envelopes as handled until the messages are successfully handled inside a batch message
4.  Likewise, if a batch message fails to the point where it triggers a move to the dead letter queue, each individual message that was part of that original batch is moved to the dead letter queue separately

## Summary

Hey, that’s actually all I had to say about that! Wolverine 3.0 will hopefully go RC later this week or next, with the official release \*knock on wood\* happening before I leave for [Swetugg](https://www.swetugg.se/gbg-2024) and a visit in Copenhagen with a JasperFx client in a couple weeks.

**Published** September 3, 2024

## Post navigation
