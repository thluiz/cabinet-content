---
created: 2024-07-11T12:01:08 (UTC -03:00)
tags: []
source: https://fredhohman.com/card-shuffling/
author: By: Fred Hohman
---

# The Math of Card Shuffling

> ## Excerpt
> Riffling from factory order to complete randomness.

---
## Riffling from factory order to complete randomness.

You’ve probably seen a few ways to shuffle a deck of cards. Sometimes the deck is split in half and the halves are switched. Sometimes the deck is _[smooshed](https://big.assets.huffingtonpost.com/smooshing.gif)_ until it’s all mixed up. But most of the time, a deck of cards is shuffled using a _riffle_.

![](https://fredhohman.com/card-shuffling/static/images/riffle.gif)

Here’s a question: _how many times do you have to riffle a deck of cards before it is completely shuffled?_ It’s a tricky one, but math has us covered: [you need seven riffles](https://en.wikipedia.org/wiki/Shuffling#Riffle).

We can calculate the number of orderings of a deck of cards using the notion of a [permutation](https://en.wikipedia.org/wiki/Permutation). To find all arrangements of 52 cards in a deck, we compute **52!**, which happens to be a [really big number](http://www.wolframalpha.com/input/?i=52!).

Riffle seven times and you’ll have a sufficiently random ordering of cards, an ordering that has likely [never existed before](http://www.murderousmaths.co.uk/cardperms.htm). In other words, it’s unlikely you’ll ever shuffle two decks the same.

The card shuffling result appears in a [Numberphile video from 2015](https://www.youtube.com/watch?v=AxJubaijQbI), along with a number of other card shuffling facts. Here’s another problem posed in that video: what if instead of a standard riffle using a deck roughly split in half, you were to only riffle _1 card at a time_?

[This paper](https://projecteuclid.org/download/pdf_1/euclid.aoap/1177005705) shows a number of other interesting card shuffling results in all their gory details.

That is, using a standard deck of 52 cards, in one hand hold 51 cards and in the other hold the remaining single card. Now riffle these together. This is equivalent to taking one card and placing it at random inside of the deck.

**So here’s the question:**  
_How many single riffles do you have to do in order to have a completely shuffled deck?_

## Theorem

You could simulate this in a short program, which we will do towards the end, but first we can solve for the number of riffles explicitly.

Consider an ordered deck of cards. [Without loss of generality](https://en.wikipedia.org/wiki/Without_loss_of_generality), let’s say the suits are in the following order: ♠, ♣, ♥, ♦. So our ordered deck looks like this.

The bottom suit is ♦, which means the bottom card of our deck is the King of Diamonds (K♦). Now perform the following iteration:

> Place the top card of the deck randomly inside the deck

This means taking the A♠ and placing it randomly somewhere in the deck. The top card then becomes 2♠.

If this procedure is repeated, eventually the top card will be placed at the very bottom of the deck, shifting the K♦ to the penultimate position. Since every riffled card has a $\frac{1}{52}$ chance of moving to any new position in the deck, that means, on average, after about 52 top card riffles, the top card will become the new bottom card.

**Note:** notice the K♦ can only rise in the deck. There are two cases:

1.  The top card is placed above the K♦, therefore its position does not change.
2.  The top card is placed underneath the K♦, therefore it rises one position closer to the top.

Once the K♦ moves up one position, upon subsequent riffles there are now two spots for the new top card to be placed underneath it. That means there is now a $\frac{1}{52}+\frac{1}{52}=\frac{2}{52}$ chance of a riffled card going underneath the K♦.

Continuing this procedure, the original bottom card, the K♦, will eventually rise to the top of the deck and be riffled. Once this happens, the deck is randomly shuffled: the order we’re left with is equally as likely as any other order.

So, how many single card riffles does this take? Recall each time a card is placed underneath the K♦, our chances of placing another card increases by $\frac{1}{52}$ . We can calculate the number of riffles this would take.

$\sum_{i=1}^{52} \frac{52}{i} = \frac{52}{1} + \frac{52}{2} + \frac{52}{3} + ... + \frac{52}{52} \approx 236$

On average, 236 single card riffles will randomly shuffle a deck of cards.

## Let’s Riffle

Equations are great, but let’s visualize this! Below is the same ordered deck of cards from before, except the K♦ has been highlighted red so we can follow its journey to the top of the deck.

Click the **Riffle** button to move the top card somewhere else in the deck randomly.

Did you see where it went? Click again.

Click a bunch more really fast.

Now I could tell you to keep clicking until the highlighted K♦ rises to the top, but as we have already shown, that would take about 236 clicks. Instead, click the **Riffle (x10)** button to riffle 10 times. Keep riffling until the K♦ moves to the top.

You have riffled **0** times.  

Here is a chart of the K♦’s position in the deck for each riffle. Notice how it takes many riffles to move the K♦ up just a few positions, but once the K♦ starts rising towards the top of the deck, it accelerates.

Once the K♦ is the top card, click the **Clear** button to try again.

On average, the K♦ will reach the top position somewhere around 236 riffles. Since this is the _average_ result, there is a chance your first shuffled deck of cards took less riffles (or many more!). To try again, click the **Clear** button and get riffling.

### Acknowledgements

-   This article was created using [Idyll](https://idyll-lang.org/).
-   Shoutout to [@mathisonian](https://twitter.com/mathisonian) for help and feedback.
-   The source code is available on [Github](https://github.com/fredhohman/card-shuffling/).
