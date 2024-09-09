---
created: 2024-09-05T21:18:30 (UTC -03:00)
tags: []
source: https://avdi.codes/notes-on-a-successful-second-system/?utm_source=feedly&utm_medium=rss&utm_campaign=notes-on-a-successful-second-system
author: Avdi Grimm
---

# Notes on a Successful Second System – avdi.codes

> ## Excerpt
> "We just need to replace the whole system and then everything will be better." Famous last words? Usually! But not always...

---
Let’s talk about bad ideas.

Famous last words: _We just need to replace this whole system and then everything will be better._

Famous last words: _This is clearly a technology problem._

Famous last words: _All we need to do is add more features, and business will improve._

These are signs of a bad idea. I’ve spent my whole career learning that these statements are red flags. These are things that people say six months before the mass email that begins “What an [incredible journey](http://ourincrediblejourney.tumblr.com/) it’s been…”

In _The Mythical Man-Month_, Fred Brooks documented what he called the “[Second System Effect](https://en.wikipedia.org/wiki/Second-system_effect)“, where a team optimistically launches a replacement system that turns out to be buggy, bloated, and slow. The famous last words above are often the harbingers of Second System Effect.

But in the Spring of 2016, I decided I had a technology problem. I decided the best way to address it was to replace the whole system, with a new one that added a lot more features. And that’s exactly what I did.

And it _worked out_. And it felt great. And I’ve thought a lot about why that was. How did that fail to be a bad idea?

Let’s back up a little bit.

## The first four years of RubyTapas

In 2016, my screencast series [RubyTapas](http://www.rubytapas.com/) (which has since become [Graceful.Dev](https://graceful.dev/)) had (very intentionally) gone from a side project to my main source of income. Four years before, I had launched the show using a third-party “digital product store” service, and it had been hosted there ever since.

The upside of this arrangement was that enabled me to focus exclusively on creating content, and not think about payment processing, subscription management, delivery, or anything else technical. The downside was that it meant I was limited to the features the store service chose to develop, at whatever pace they chose to deliver them at.

Unfortunately, this pace turned out to be that of Flash, the Zootopia sloth.

![](https://avdi.codes/wp-content/uploads/2024/08/tenor-3833354094.gif)

Years went by. I made do. Customers complained of the ugly site, poor user experience and lack of basic features like search, categories, or streaming video. But they put up with it.

But then subscriptions started to slowly flag. Not as a direct result of the lack of features, and not because of the content. More because of the inevitable, slow migration of developers towards different technologies and other sources of training. And I wasn’t doing a good enough job of drawing in new developers to replace the subscriptions I’d lost.

## It’s hard to sell what you don’t like

I knew what I needed to do. I had lots of concrete, practical plans for picking up on the marketing efforts that I had totally let slide.

There was just one problem: **I hated what I was marketing**. Not the content, but the medium.

Yes, there were also a lot of _technical_ ways that the platform made marketing harder: it forced me to have separate marketing and subscriber sites. It gave me no way to show “teasers” of the content. It limited the information I could discover about how people used the site. It didn’t even let me issue coupons for sales.

But all that pales in comparison with the ugly fact that every time I set out to start some new marketing initiative, I’d slow down and stop because I was **embarrassed of the thing I was selling.** I knew that if _I_ was a new subscriber to RubyTapas, as soon as I saw the subscriber-side site I’d be saying: “seriously?!”.

This sense of shame completely took the wind out of my sails when it came to selling myself and my product.

## A sudden course-change

Of course, I’d made stabs at changing the situation over the years. I had been slowly—very slowly—trying to build out the part of the site that I controlled into something more featureful. I had big plans for the experience I wanted to provide.

But I didn’t have the time to create content on a weekly basis _and_ build a full-featured subscription website. Or the extra cash to pay someone else to do it. I’d work for an hour here, and an hour there. Then there’d be a lull when other things would get in the way. And then I’d spend a couple of days just trying to remember where I’d left off.

Then, in May 2016, I changed course. I threw out all of my plans of building a new site by hand, from scratch. In fact, I threw out the idea of “building” anything at all.

Instead, I created a new version of the site on off-the-shelf software: self-hosted WordPress, a store-bought premium theme, and a handful of carefully selected plugins.

It took me a few days to have the initial proof of concept up and running. A couple more weeks before I was ready to announce it and start accepting new subscriptions. A little over two months after I first considered going down this route, I was migrating users over from the old system.

Users, new and old, were thrilled with new site. And I breathed a giant sigh of relief. Because for the first time in long time, **I was selling something I can be proud of**. Which meant I could do all that marketing stuff I’d been avoiding.

Which brings me back to the beginning of this article, and bad ideas.

## Why did this second system succeed?

I’ve learned the very hard lesson over the years, never to put my faith in technical solutions to business problems. Or in “the new system” that will fix all the problems of the old system. And yet, in a way that’s exactly what I’d done… and it worked out beautifully.

I thought a lot about what made this situation different. Here’s what I came up with:

### 1\. Pick a _boring_ technical solution

I could have decided that the direction I needed to go was to set aside three months to do nothing but write an app from scratch, using all of my web development experience. I can virtually guarantee you that the result of that decision would have been disaster, as more and more unconsidered edge cases turned up.

Or, I could have found a new, _better_ third-party subscription hosting service. And tied myself to a whole new set of velvet handcuffs.

Instead, I went with a known quantity. A boring quantity. WordPress, a commodity workhorse which is as often as not used as the butt of jokes by web developers. And by going with something known, and unexciting, and old, I was able to leverage the full power of an incredibly rich ecosystem: everything from themes and plugins, to specialized hosting services, backup solutions, security, and maintenance-as-a-service providers.

A big part of the risk in looking for a technical solution is the fact that 80% of the problem is usually social, not technical. But I think another factor may be that “technical solution” is often treated as synonymous with “_shiny new_ technical solution”.

No, choosing boring tech won’t help you if you’re solving the wrong problem. But if you know exactly what you need, established tools can offer powerful network effects that go above and beyond their simple technical merits.

### 2\. Admit when your problem is not a beautiful snowflake

Programmers love solving problems. The corollary is that we also love _making_ problems.

When we’re faced with something which is obviously an old and oft-solved problem, our usual tactic for making new problems is to proclaim the existing solutions “too complicated”.

_I just need to sell subscriptions, and only show content to subscribers. Simple! I don’t need one of these bloated, over-complicated pre-built systems._

_Well, and let them upgrade and downgrade to different plans._

_With proration, so they don’t get overcharged when they upgrade._

_And dunning, so they don’t just get cut off when their credit card expires._

_And I need to enable downloads for some users but not for others._

_And I need to provide a way for users in countries with strict tax laws to put their VAT IDs on their receipts._

_Which means I need proper receipts._

_And I need categories. And tags. And series._

_And an easy way to make some episodes into freebies._

_And the episodes should show a nicely formatted social card when posted to Twitter or Facebook._

_And… and… and…_

Some people will gamely power through all of these feature adds and gotchas, one by one. And then one day, they’ll find themselves scaling up, and wanting to hire someone to do copy-editing on 400 archive posts. That exist in the form of specialized files in a Github repo. And then they have to teach this new hire to work within the constraints of a home-grown, cobbled-together system. And how to commit and re-deploy the site after each page is done.

Guess how long it takes me to find someone who can immediately jump into copy-editing articles on a WordPress site, with zero advance training? Or, for that matter, to find someone to do any number of other WordPress maintenance or marketing tasks? The answer is usually measured in hours.

For a lot of developers—including younger iterations of myself—“simplicity” is defined in the implicit context of being the sole person working on a project. Forever.

In evaluating solutions for my technical problem, I acknowledged the fact that my problem was a solved one. _Very_ solved: Check out this [check out Chris Lema’s most recent lineup of WordPress membership plugins](https://chrislema.com/comparing-wordpress-membership-plugins/). Note the nitpicky details that go into rising to the top of such a list.

I think that acknowledging this truth about the mundanity of my problem saved me from going down a very hard (and unnecessary) road.

### 3\. A prototype is not a proof of concept.

This change in technology was a big and risky decision for my company. When I made the choice to go full-speed ahead on it, I made it based on a working proof of concept: a beta site that could accept payments, show videos, and differentiate access based on subscription levels. It had a production-ready UI _including_ a full-featured admin UI. And I had proved that I could import posts via HTTP APIs in an automated fashion.

In other words, it was a real application, _not a prototype_.

Too often I’ve seen just how wide the distance grows between prototype and production, padded out with assumptions and hand-waves. Not every problem will be amenable to putting together a proof-of-concept in a matter of days, like I did. But this experience has solidified my sense that, when it comes to big, make-or-break business decisions, a prototype is not enough.

### 4\. Exist in an ecosystem

One of the undersea rocks that systems built on a new-to-us technology often founder upon is lack of support. The initial roll-out goes well. But then an issue crops up that nobody knows how to solve.

This was a real danger for me. Instead of building on a stack within which I have a lot of competency (Ruby, Rails, Heroku, etc.), I was introducing a stack with which I had relatively little in-depth experience.

In years prior, I might have barreled ahead regardless. This time around, I took careful stock, not just of my own competency, but of the resources I could draw on. It was true I had no mastery of WordPress development. But:

-   I knew an expert I could talk to when I got out of my depth. Someone who could either help directly, or (more often) point me in the direction of the right people to talk to.
-   Via my WordPress guru, I had a shortlist of reliable resources: sites, forums, people, and books.
-   I knew of a well-recommended [agency](https://avdi.codes/notes-on-a-successful-second-system/wpsitecare.com) that specializes in WordPress maintenance: updates, security, and troubleshooting, whose job it is to be on-call if something goes wrong.
-   By the time I had my proof-of-concept in place, I had _already_ been in correspondence with support for the membership plugin I had decided to use, and confirmed that they were responsive, competent, and professional.

If these elements had not been in place, I almost certainly wouldn’t have gone ahead with the switch.

### 5\. Avoid featuritis

“We’ll solve our business problems with new features!”: Changes that start with this statement rarely end well.

In my case, when it came to the question of whether new features would actually address user needs, I wasn’t stabbing in the dark. I had data. I had survey results, and years of user feedback.

But I also knew that I wasn’t switching out my tech stack for any specific feature. It was more about drastically shortening the time needed to implement _any_ feature, or at least to do an acceptable first cut. And from my years of using it as a blogging platform, I knew that’s an area WordPress excels at, due to its extensive plugin ecosystem.

And finally, I was under no illusions that features alone would make a big difference in my business. I knew that ultimately, I wasn’t addressing a feature gap—I was addressing a _morale_ gap. I needed to be working on a system that I could get excited about. I needed to feel like I could quickly give users what they asked for. And I needed to enjoy the experience of working on the admin side, instead of avoiding it.

## Some questions to ask yourself

I know this was long, and very focused on a very specific situation. My hope in writing all this down is that something here might help you, the next time you are evaluating a big technical shift. Some questions you might ask yourself or your team:

-   Is there a boring, established way to do this? Is there a reason we’re not using it, other than “cool factor”? Have we latched onto some precious “must-have” feature or architectural style that is furnishing a convenient excuse not to go down a well-traveled road?
-   Are we making decisions about building our own stuff based on vague appeals to “flexibility”, or to “simplicity”?
-   Do we have the slack time to do a real proof-of-concept before moving forward? Or only time for a prototype? If the latter, is there a smaller chunk we could bite off instead?
-   Do we have support lined up for when problems with the new tech exceed our in-house competency?
-   Are we hiding a deeper reason for a change—such as developer morale—behind the excuse of “features”? And if we are honest about the real reasons, how will we measure success?

## Epilogue: Eight Years Later

Everything I wrote above was adapted from a [SIGAVDI](https://avdi.codes/sigavdi) letter I wrote back in 2016. (And if you want to get these thoughts with a latency better than 8 years, put your email into that subscribe box down below!)

In the time since then, I have migrated RubyTapas to [Graceful.Dev](https://graceful.dev/), which is effectively a _third_ system. This time, it was a WordPress-to-WordPress migration. The third iteration used new plugins, new hosting, and leverages the competency I’ve gained since 2016 in developing on top of WordPress. But while tedious, the extreme openness and well-documented nature of WordPress made the various steps of the migration pretty straightforward.

Everything I wrote about WordPress for hosting content-centric businesses is more true than ever. If you want to be able to quickly scale _yourself_ as creator, WordPress is still by far the best platform to be on. As a development foundation it’s as flexible as you will ever need it to be. But most of the customizations and add-ons you will need are already accommodated by plugins, page-builders, hosting plans, and compatible services. Most importantly, you will never, ever struggle to hire people who can hit the ground running in helping you with either content or customization.

As a Ruby developer, one thing that I’m noticing is that for more and more shops, Ruby on Rails is turning into the “boring old technology” that enables them to Just Get Stuff Done. I’m seeing more and more posts by seasoned JavaScript developers saying that they kicked off a new app with Rails, and the number of decisions made for them and things that Just Worked out of the box enabled them to skip straight to the problem domain.

Sometimes, the choice to migrate to a known-quantity technology that does 80% of the work for you is a matter of acknowledging that the state of the art has caught up with you. [Jessitron](https://jessitron.com/) calls this “rebasing on the world”: noting where commodities can now replace your former bespoke implementations. As a senior developer, part of your job is keeping an eye on when technologies have moved to the right on the [Wardley Map](https://learnwardleymapping.com/), and adjusting your Second Systems accordingly.

Have you had a successful second-system migration? What considerations and preparation went into it?
