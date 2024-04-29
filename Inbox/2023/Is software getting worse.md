---
created: 2023-10-25T16:31:59 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2023/07/24/is-software-getting-worse/
author: Mark Seemann
---

# Is software getting worse?

> ## Excerpt
> A rant, with some examples.

---
_A rant, with some examples._

I've been a software user for thirty years.

My first PC was [DOS](https://en.wikipedia.org/wiki/DOS)\-based. In my first job, I used [OS/2](https://en.wikipedia.org/wiki/OS/2), in the next, [Windows 3.11](https://en.wikipedia.org/wiki/Windows_3.1x), [NT](https://en.wikipedia.org/wiki/Windows_NT), and later incarnations of Windows.

I wrote my first web sites in [Arachnophilia](https://en.wikipedia.org/wiki/Arachnophilia), and my first professional software in Visual Basic, Visual C++, and [Visual InterDev](https://en.wikipedia.org/wiki/Visual_InterDev).

I used [Terminate](https://en.wikipedia.org/wiki/Terminate_(software)) with my first modem. If I recall correctly, it had a built-in email downloader and offline reader. Later, I switched to Outlook for email. I've used [Netscape Navigator](https://en.wikipedia.org/wiki/Netscape_Navigator), [Internet Explorer](https://en.wikipedia.org/wiki/Internet_Explorer), Firefox, and Chrome to surf the web.

I've written theses, articles, reports, etc. in [Word Perfect](https://en.wikipedia.org/wiki/WordPerfect) for DOS and MS Word for Windows. I wrote my new book [Code That Fits In your Head](https://blog.ploeh.dk/2021/06/14/new-book-code-that-fits-in-your-head) in [TexStudio](https://www.texstudio.org/). Yes, it was written entirely in [LaTeX](https://en.wikipedia.org/wiki/LaTeX).

### Updates [#](https://blog.ploeh.dk/2023/07/24/is-software-getting-worse/#c3ee042288d94295bec33308840d075f)

For the first fifteen years, new software version were rare. You'd get a version of AutoCAD, Windows, or Visual C++, and you'd use it for years. After a few years, a new version would come out, and that would be a big deal.

Interim service releases were rare, too, since there was no network-based delivery mechanism. Software came on [floppy disks](https://en.wikipedia.org/wiki/Floppy_disk), and later on [CD](https://en.wikipedia.org/wiki/Compact_disc)s.

Even if a bug fix was easy to make, it was difficult for a software vendor to _distribute_ it, so most software releases were well-tested. Granted, software had bugs back then, and some of them you learned to work around.

When a new version came out, the same forces were at work. The new version had to be as solid and stable as the previous one. Again, I grant that once in a while, even in those days, this wasn't always the case. Usually, a bad release spelled the demise of a company, because release times were so long that competitors could take advantage of a bad software release.

Usually, however, software updates were _improvements_, and you looked forward to them.

### Decay [#](https://blog.ploeh.dk/2023/07/24/is-software-getting-worse/#865a6ab3b3c64982bbeb31ccf64db6b5)

I no longer look forward to updates. These days, software is delivered over the internet, and some applications update automatically.

From a security perspective it can be a good idea to stay up-to-date, and for years, I diligently did that. Lately, however, I've become more conservative. Particularly when it comes to Windows, I ignore all suggestions to update it until it literally forces the update on me.

Just like [even-numbered Star Trek movies don't suck](https://tvtropes.org/pmwiki/pmwiki.php/Main/StarTrekMovieCurse) the same pattern seems to be true for Windows: [Windows XP](https://en.wikipedia.org/wiki/Windows_XP) was good, [Windows 7](https://en.wikipedia.org/wiki/Windows_7) was good, and [Windows 10](https://en.wikipedia.org/wiki/Windows_10) wasn't bad either. I kept putting off Windows 11 for as long as possible, but now I use it, and I can't say that I'm surprised that I don't like it.

This article, however, isn't a rant about Windows in particular. This seems to be a general trend, and it's been noticeable for years.

### Examples [#](https://blog.ploeh.dk/2023/07/24/is-software-getting-worse/#f3bd0d4e750b4d9aa53ee9a6c0f59ddc)

I think that the first time I noticed a particular application degrading was [Vivino](https://en.wikipedia.org/wiki/Vivino). It started out as a local company here in Copenhagen, and I was a fairly early adopter. Initially, it was great: If you like wine, but don't know that much about it, you could photograph a bottle's label, and it'd automatically recognise the wine and register it in your 'wine library'. I found it useful that I could look up my notes about a wine I'd had a year ago to remind me what I thought of it. As time went on, however, I started to notice errors in my wine library. It might be double entries, or wines that were silently changed to another vintage, etc. Eventually it got so bad that I lost trust in the application and uninstalled it.

Another example is [Sublime Text](https://www.sublimetext.com/), which I used for writing articles for this blog. I even bought a licence for it. Version 3 was great, but version 4 was weird from the outset. One thing was that they changed how they indicated which parts of a file I'd edited after opening it, and I never understood the idea behind the visuals. Worse was that auto-closing of HTML stopped working. Since I'm that [weird dude who writes raw HTML](https://rakhim.org/honestly-undefined/19/), such a feature is quite important to me. If I write an HTML tag, I expect the editor to automatically add the closing tag, and place my cursor between the two. Sublime Text stopped doing that consistently, and eventually it became annoying enough that I though: _Why bother?_ Now I write in [Visual Studio Code](https://code.visualstudio.com/).

Microsoft is almost a chapter in itself, but to be fair, I don't consider Microsoft products _worse_ than others. There's just so much of it, and since I've always been working in the Microsoft tech stack, I use a lot of it. Thus, [selection bias](https://en.wikipedia.org/wiki/Selection_bias) clearly is at work here. Still, while I don't think Microsoft is worse than the competition, it seems to be part of the trend.

For years, my login screen was stuck on the same mountain lake, even though I tried every remedy suggested on the internet. Eventually, however, a new version of Windows fixed the issue. So, granted, sometimes new versions improve things.

Now, however, I have another problem with [Windows Spotlight](https://en.wikipedia.org/wiki/Windows_spotlight). It shows nice pictures, and there used to be an option to see where the picture was taken. Since I repaved my machine, this option is gone. Again, I've scoured the internet for resolutions to this problem, but neither rebooting, regedit changes, etc. has so far solved the problem.

That sounds like small problems, so let's consider something more serious. Half a year ago, Outlook used to be able to detect whether I was writing an email in English or Danish. It could even handle the hybrid scenario where parts of an email was in English, and parts in Danish. Since I repaved my machine, this feature no longer works. Outlook doesn't recognise Danish when I write it. One thing are the red squiggly lines under most words, but that's not even the worst. The worst part of this is that even though I'm writing in Danish, outlook thinks I'm writing in English, so it silently auto-corrects Danish words to whatever looks adjacent in English.

![Screen shot of Outlook language bug.](https://blog.ploeh.dk/content/binary/outlook-language-bug.png)

This became so annoying that I contacted Microsoft support about it, but while they had me try a number of things, nothing worked. They eventually had to give up and suggested that I reinstalled my machine - which, at that point, I'd done two weeks before.

This used to work, but now it doesn't.

### It's not all bad [#](https://blog.ploeh.dk/2023/07/24/is-software-getting-worse/#b314b021b97f455184e44c52ab584afa)

I could go on with other examples, but I think that this suffices. After all, I don't think it makes for a compelling read.

Of course, not everything is bad. While it looks as though I'm particularly harping on Microsoft, I rarely detect problems with [Visual Studio](https://visualstudio.microsoft.com/) or Code, and I usually install updates as soon as they are available. The same is true for much other software I use. [Paint.NET](https://www.getpaint.net/) is awesome, [MusicBee](https://www.getmusicbee.com/) is solid, and even the [Sonos](https://www.sonos.com/) Windows app, while horrific, is at least consistently so.

### Conclusion [#](https://blog.ploeh.dk/2023/07/24/is-software-getting-worse/#708ec0a2be1d42f083c1c2d37c24221d)

It seems to me that some software is actually getting worse, and that this is a more recent trend.

The point isn't that some software is bad. This has always been the case. What seems new to me is that software that _used to be good_ deteriorates. While this wasn't entirely unheard of in the nineties (I'm looking at you, WordPerfect), this is becoming much more noticeable.

Perhaps it's just [frequency illusion](https://en.wikipedia.org/wiki/Frequency_illusion), or perhaps it's because I use software much more than I did in the nineties. Still, I can't shake the feeling that some software is deteriorating.

Why does this happen? I don't know, but my own bias suggests that it's because there's less focus on regression testing. Many of the problems I see look like regression bugs to me. A good engineering team could have caught them with automated regression tests, but these days, it seems as though many teams rely on releasing often and then letting users do the testing.

The problem with that approach, however, is that if you don't have good automated tests, fixing one regression may resurrect another.
