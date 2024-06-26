---
created: 2024-06-03T09:11:01 (UTC -03:00)
tags: [productivity,webdev,vscode,tooling,software,coding,development,engineering,inclusive,community]
source: https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev
author: Nick Taylor
---

# Tools that keep me productive - DEV Community

> ## Excerpt
> This page details mostly all I use as a developer. I use a Mac, so a bunch of tools are...

---
This page details mostly all I use as a developer. I use a Mac, so a bunch of tools are macOS-specific, but there are some OS-agnostic ones in the list.

One thing to mention before we get started is that these are the tools that make me productive. Maybe they won't make you productive like the way they do for me. I always say, _use the tools that make you the most productive_.

Some of these tools are free but some are paid. I personally think the paid ones are worth it, but I leave that up to you and your wallet.

_Note: I've put some referral links in here. Just want to be upfront about that is all._

## [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#editor)Editor

It all starts with the editor. [Visual Studio Code](https://code.visualstudio.com/) (VS Code) is my go-to editor. I was using the [Insider’s Edition](https://code.visualstudio.com/insiders/) for the longest time, but some extensions would try to log in and redirect to VS Code regular edition, so I decided to go back to it. That said, VS Code Insider's is very stable.

I was a big fan of the Dank Mono for the longest time, but GitHub released a bunch of monospaced fonts this year and I've been loving [Monaspace Krypton](https://monaspace.githubnext.com/).

For the theme, it varies. I've been on the light modern default theme recently as I find it's better for [my live streaming](https://nickyt.live/), but I'm also a fan of the [Houston](https://marketplace.visualstudio.com/items?itemName=astro-build.houston) and [Fortnite](https://marketplace.visualstudio.com/items?itemName=sdras.fortnite-vscode-theme) themes.

[![Me when I tell them I use a dark theme in my editor.](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F52lbfjxugvqsj017c5e5.png)](https://x.com/nickytonline/status/1787621116636221727)

Although I have [iTerm](https://iterm2.com/) installed, a great terminal for macOS, I honestly live in the VS Code terminal 99.999% of the time.

### [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#editor-settings)Editor Settings

If you're interested in my editor settings, [here's my current settings](https://gist.github.com/nickytonline/e6ceb17a1fb7b6438c3f09ff800748da).

One of the more fun ones is you can change the title bar, so I've added some emojis to mine.  

```
<span>  </span><span>"window.title"</span><span>:</span><span> </span><span>"🦙⚡🫡 – ${activeEditorShort}${separator}${rootName} – 🫡⚡🦙"</span><span>,</span><span>
</span>
```

![An alpaca floating through a rainbow](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fit745h3v0bkp60tt3z41.gif)

Another setting that I find super handy is `terminal.integrated.autoReplies`. I never want to source my .env file and this handles it perfectly.  

```
<span>  </span><span>"terminal.integrated.autoReplies"</span><span>:</span><span> </span><span>{</span><span>
    </span><span>"dotenv: found '.env' file. Source it? ([Y]es/[n]o/[a]lways/n[e]ver)"</span><span>:</span><span> </span><span>"e</span><span>\r</span><span>"</span><span>
  </span><span>}</span><span>,</span><span>
</span>
```

### [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#maybe-zed-soon)Maybe Zed Soon?

I do want to give a shout out to the [Zed](https://zed.dev/) editor. I use it occasionally and it’s super fast, but it hasn’t become my main editor yet. I think once the extension ecosystem blows up a little more is when I move to this. Maybe in the next year. We’ll see. 😎

## [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#browser-extensions)Browser Extensions

I don't use all of these everyday, but these are my go-to browser extensions.

-   [Refined GitHub](https://chrome.google.com/webstore/detail/refined-github/hlepfoohegkhhmjieoechaddaejaokhf) - GitHub on steroids
-   [VisBug](https://chrome.google.com/webstore/detail/visbug/cdockenadnadldjbbgcallicgledbeoc?hl=en) - A fantastic tool for frontend (this one’s new for me) (only for Chromium-based browsers)
-   [React DevTools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) - Because React
-   [Preact DevTools](https://preactjs.github.io/preact-devtools/) - Because Preact
-   [Axe](https://chrome.google.com/webstore/detail/axe-web-accessibility-tes/lhdoppojpmngadmnindnejefpokejbdd) - For web accessibility testing
-   [WAVE](https://wave.webaim.org/extension/) - For web accessibility testing
-   [HTTPS Everywhere](https://www.eff.org/https-everywhere)
-   [uBlock](https://ublock.org/)
-   [LanguageTool](https://languagetool.org/) - A grammar and spell checking tool
-   [Pocket](https://getpocket.com/) - For bookmarking stuff to read
-   [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc) - A prettified view of JSON payloads
-   [Tweak New Twitter](https://github.com/insin/tweak-new-twitter/) - Gets rid of a lot of noise in the Twitter UI
-   [a11y Twitter](https://github.com/nickytonline/a11y-twitter) - Small changes to how you use Twitter to promote Tweeting in an accessible manner.

## [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#desktop-apps)Desktop Apps

These are most of the desktop apps that I use every day. Let's get started with some general ones.

### [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#general-tools-for-common-things-i-do-everyday)General Tools for Common Things I do Everyday

[Arc Browser](https://arc.net/gift/93e342bc) is a Chromium-based browser that, in my opinion, has nailed a tonne of the user experience (UX) issues I've encountered with any other browsers. Vertical tabs, command palette, and auto-picture in picture video to name a few.

I used [Vanilla](https://matthewpalmer.net/vanilla/) for the longest time for my top menu bar icons, but once I got a MacBook Pro with the notch, it just didn't work well. I've since moved on to [Bartender](https://www.macbartender.com/) for managing my menu bar.

The emoji picker on macOS isn't that great, but [Rocket](https://matthewpalmer.net/rocket/) makes it so easy to add emojis. I can't tell you how many times a day I use this.

[Raycast](https://raycast.com/) is my go-to replacement for macOS' spotlight. It's like Spotlight on steroids. I previously used [Alfred](https://www.alfredapp.com/), another outstanding Spotlight alternative, but for some reason Raycast grew on me. I also use it for window management.

For those evenings where I'm in front of the computer, [f.lux](https://justgetflux.com/) is a must. Like some wise person said, "Be kind to your eyeballs". macOS's [Nightshift](https://support.apple.com/en-ca/102191) kind of works, but f.lux destroys it.

For managing meetings, [Dato](https://sindresorhus.com/dato) is a better date app for macOS. It's great for having multiple time zones in the address bar. I have my local time as well as UTC. I also use it for upcoming meetings and events. Previously I was using [Meeter](https://trymeeter.com/) which is great for this, but it's one less app I need now.

I take screenshots or short video recordings almost daily, and [Cleanshot X](https://cleanshot.com/) is so great for this.

### [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#tools-for-git)Tools for Git

I do most of my "git"ing on the command line, but sometimes I need a graphical user interface (GUI) to really understand what's going on. When I need that, I reach for [Fork](https://git-fork.com/).

![Cassidy demonstrating squash, rebase and merge](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fmwz6yp06cesc6k6c3aw1.gif)

Shoutout to Cassidy ([@cassidoo](https://dev.to/cassidoo)) for the awesome GIF!

If you're using Git, which I imagine most of you are, [signing your commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits) is super important. [GPG Suite](https://gpgtools.org/) makes this easy to set up.

### [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#frontend-tooling)Frontend Tooling

I do a lot of work building user interfaces (UIs) and these are some indispensable tools for that kind of work.

[xScope](https://xscope.app/) is a fantastic tool set for frontend development. Rulers, guides etc.

[Figma](https://www.figma.com/) is where I live when I need to coordinate with our designer, look at designs, or pull some assets.

I had heard about [Polypane](https://polypane.app/) before and I think I may have tried it a few years ago, but nowadays, It's a must for frontend. It helps you build out responsive, accessible apps with all kinds of goodies. Curious about it? I hung out with the creator of Polypane, Kilian Valkhof ([@kilianvalkhof](https://dev.to/kilianvalkhof)), on a live stream earlier this year.

<iframe width="710" height="399" src="https://www.youtube.com/embed/fsIhghVlHJE" allowfullscreen="" loading="lazy"></iframe>

For color contrast issues, TPGi's [Color Contrast Analyzer](https://www.tpgi.com/color-contrast-checker/) is top tier. I can't recommend it enough. Thanks to Todd Libby ([@colabottles](https://dev.to/colabottles)) for recommending this to me last year.

### [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#other-desktop-apps-i-use)Other Desktop Apps I Use

-   [Cloudflare Warp](https://blog.cloudflare.com/1111-warp-better-vpn/) - Faster Internet and some VPN goodness
-   [Plash](https://apps.apple.com/us/app/plash/id1494023538) - An interactive desktop background (one or more web pages) for your Mac
-   [CleanMyMac X](https://macpaw.com/cleanmymac) - A suite of utilities for keeping your Mac in tip-top shape.
-   [Starship](https://starship.rs/) - A cross shell prompt

## [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#live-streaming-software)Live Streaming Software

I'm sure there are streamers with bigger audiences that have a better setup, but this is how I roll.

[Restream.io](https://restream.io/join/zZ8Wr) is what I use to stream to multiple platforms, currently Twitch, YouTube, X/Twitter, and LinkedIn.

[OBS](https://obsproject.com/download) is used by many including myself. It's a great piece of open source software. I use it for streaming instead of Restream Studio or similar tools like Streamyard because I have custom overlays and some other customizations.

## ![GitHub logo](https://res.cloudinary.com/practicaldev/image/fetch/s--A9-wwsHG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev.to/assets/github-logo-5a155e1f9a670af7944dd5e12375bc76ed542ea80224905ecaf878b9157cdefc.svg) [obsproject](https://github.com/obsproject) / [obs-studio](https://github.com/obsproject/obs-studio)

### OBS Studio - Free and open source software for live streaming and screen recording

## OBS Studio <[https://obsproject.com](https://obsproject.com/)\>

[![OBS Studio Build Status - GitHub Actions](https://github.com/obsproject/obs-studio/actions/workflows/push.yaml/badge.svg?branch=master)](https://github.com/obsproject/obs-studio/actions/workflows/push.yaml?query=branch%3Amaster)

[![OBS Studio Translation Project Progress](https://camo.githubusercontent.com/892809a440bb5ca72d1e4c04f9d2323e0fd5f8993d81d4c6fc9f1cff0bd1abb1/68747470733a2f2f6261646765732e63726f7764696e2e6e65742f6f62732d73747564696f2f6c6f63616c697a65642e737667)](https://crowdin.com/project/obs-studio)

[![OBS Studio Discord Server](https://camo.githubusercontent.com/75b315b8503f25b517cf3de607f99b45fc5ddb03fae56dfd11b204ff23ebc843/68747470733a2f2f696d672e736869656c64732e696f2f646973636f72642f3334383937333030363538313932333834302e7376673f6c6162656c3d266c6f676f3d646973636f7264266c6f676f436f6c6f723d66666666666626636f6c6f723d373338394438266c6162656c436f6c6f723d364137454332)](https://obsproject.com/discord)

## What is OBS Studio?

OBS Studio is software designed for capturing, compositing, encoding, recording, and streaming video content, efficiently.

It's distributed under the GNU General Public License v2 (or any later version) - see the accompanying COPYING file for more details.

## Quick Links

-   Website: [https://obsproject.com](https://obsproject.com/)
-   Help/Documentation/Guides: [https://github.com/obsproject/obs-studio/wiki](https://github.com/obsproject/obs-studio/wiki)
-   Forums: [https://obsproject.com/forum/](https://obsproject.com/forum/)
-   Build Instructions: [https://github.com/obsproject/obs-studio/wiki/Install-Instructions](https://github.com/obsproject/obs-studio/wiki/Install-Instructions)
-   Developer/API Documentation: [https://obsproject.com/docs](https://obsproject.com/docs)
-   Donating/backing/sponsoring: [https://obsproject.com/contribute](https://obsproject.com/contribute)
-   Bug Tracker: [https://github.com/obsproject/obs-studio/issues](https://github.com/obsproject/obs-studio/issues)

## Contributing

-   If you would like to help fund or sponsor the project, you can do so via [Patreon](https://www.patreon.com/obsproject), [OpenCollective](https://opencollective.com/obsproject), or [PayPal](https://www.paypal.me/obsproject). See our [contribute page](https://obsproject.com/contribute) for more information.
-   If you wish to contribute code to the project, please make sure to read the coding and commit guidelines: [https://github.com/obsproject/obs-studio/blob/master/CONTRIBUTING.rst](https://github.com/obsproject/obs-studio/blob/master/CONTRIBUTING.rst)
-   Developer/API documentation can be found here: [https://obsproject.com/docs](https://obsproject.com/docs)
-   If you wish to contribute translations, do not submit pull requests. Instead, please use Crowdin. For more information read this page: [https://obsproject.com/wiki/How-To-Contribute-Translations-For-OBS](https://obsproject.com/wiki/How-To-Contribute-Translations-For-OBS)
-   Other ways to contribute are…

[Krisp](https://krisp.ai/) is outstanding for filtering out unwanted noise on calls and streams. Say goodbye to fire engines in the background while you stream. 🤣

I use [Loopback](https://rogueamoeba.com/loopback/) for virtual audio sources. This is super helpful because I create an audio source, which is my microphone and the guest's (guests') audio, and treat it as one input source. I use this audio source as the audio source for live captioning.

I don't have a fancy camera for streaming. I used to use my Logitech webcam, which was fine, but when I finally got a decent iPhone, I was like the camera on this is amazing! So I decided to use that for live streaming. [Camo](https://reincubate.com/camo/) makes it possible to do that, and it has plenty of niceties like zooming, watermarks, filters, etc.

### [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#tools-for-live-streaming-guests)Tools for Live Streaming Guests

For the longest time, I couldn't figure out how people brought guests on to livestreams. In my early days of streaming, I used to bring in the full Discord screen and share that on my live stream. Although that worked, it was not ideal. I also tried Zoom similarly, and then I also started cropping parts of Zoom on my screen, but again, not ideal.

Eventually, I discovered [vdo.ninja](https://vdo.ninja/). The TLDR is, it uses peer-to-peer technology to bring remote cameras into OBS or other studio software.

## ![GitHub logo](https://res.cloudinary.com/practicaldev/image/fetch/s--A9-wwsHG--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev.to/assets/github-logo-5a155e1f9a670af7944dd5e12375bc76ed542ea80224905ecaf878b9157cdefc.svg) [steveseguin](https://github.com/steveseguin) / [vdo.ninja](https://github.com/steveseguin/vdo.ninja)

### VDO.Ninja is a powerful tool that lets you bring remote video feeds into OBS or other studio software via WebRTC.

### ⚠ Notice! We've rebranded from OBS.Ninja to VDO.Ninja - all else is staying the same ✨

[![](https://res.cloudinary.com/practicaldev/image/fetch/s--OAmC-ySO--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://user-images.githubusercontent.com/2575698/124821455-bbfec580-df3c-11eb-9641-3d036cdd6c41.png)](https://user-images.githubusercontent.com/2575698/124821455-bbfec580-df3c-11eb-9641-3d036cdd6c41.png)

## What is **VDO NINJA**

VDO.Ninja uses peer-to-peer technology to bring remote cameras into OBS or other studio software.

In most cases, all video data is transferred directly from peer to peer, without needing to go through any video server. This results in high-quality video with super low latency. In a small number of cases, video data may go through an encrypted TURN server, which is used to facilitate peer connections when otherwise not possible.

VDO.Ninja is designed to allow content creators to produce real-time live shows using remote media streams. It can also turn smartphones into wireless webcams, with additional Virtualcam software.

VDO.Ninja is freely available to use as a managed service over at [https://vdo.ninja](https://vdo.ninja/). There's also native app versions available on the App and Play stores, however these native apps are quite…

It's a fantastic project and I highly recommend it. If your guest has a Twitch account, another similar piece of software is Twitch's [Stream Together](https://help.twitch.tv/s/article/stream-together-host-guide?language=en_US). I use this as well, depending on the guest.

## [](https://dev.to/nickytonline/tools-that-keep-me-productive-1no5?ref=dailydev#command-line-interface-cli-tools)Command Line Interface (CLI) Tools

I don't have many CLI tools, but here are some of my go-to ones:

-   [Homebrew](https://brew.sh/) - The Missing Package Manager for macOS (or Linux)
-   [GitHub CLI](https://github.com/cli/cli) - GitHub on the command line. Great for creating PRs, etc.

-   [nvm](https://github.com/nvm-sh/nvm) - Node version manager
-   [cloudflared](https://github.com/cloudflare/cloudflared) - Exposes local servers to the public internet over secure tunnels

If you're curious about the reset of my setup like hardware and office setup or what I bring when I'm on the go, feel free to check out [my uses page](https://nickyt.co/uses).

Until the next one!
