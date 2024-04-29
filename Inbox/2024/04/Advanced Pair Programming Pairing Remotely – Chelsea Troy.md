---
created: 2024-03-06T08:05:43 (UTC -03:00)
tags: []
source: https://chelseatroy.com/2017/04/01/advanced-pair-programming-pairing-remotely/
author: Chelsea
---

# Advanced Pair Programming: Pairing Remotely – Chelsea Troy

> ## Excerpt
> Maybe you work on a distributed team and you want to introduce more pairing into your development cycles. Or maybe you’re used to solving problems with pair programming and you have to be out…

---
Reading Time: 8 minutes

Maybe you work on a distributed team and you want to introduce more pairing into your development cycles. Or maybe you’re used to solving problems with pair programming and you have to be out of town for a few days. Maybe you’re a mentee, and you want to pair with a mentor in another city on your application. In any case, it’s valuable to know how to get the most out of remote pair programming. Here we’ll talk about some of the lessons I have learned pairing remotely with colleagues and clients on entrerprise applications, as well as with some mentors and mentees.  

[![remote](https://i0.wp.com/chelseatroy.com/wp-content/uploads/2017/04/screen-shot-2017-04-04-at-1-04-39-pm.png?resize=656%2C585&ssl=1)](https://www.theodysseyonline.com/life-with-ldr)

This is part one in a three part series about advanced pair programming.

-   **Pairing Remotely**
-   [Enabling Your Pair](https://chelseatroy.com/2017/04/04/advanced-pair-programming-enabling-your-pair/)
-   [Learning from Your Pair](https://chelseatroy.com/2017/04/06/advanced-pair-programming-learning-from-your-pair/)

## **Your Setup**

You can remote pair from any computer with internet access, sound, and a microphone. But the longer and the more often you’ll be doing it, the more important it is to have a setup that maximizes your ability to communicate with your pair.

**Hardware:**

I like to have two screens: my code/work screen, which I screenshare with my pair, and then a monitor on which I can videoconference with my pair. I put the monitor to my side and angle it toward me. This allows me to look to my side and see (if my pair is set up the same way) a side view of my pair, as if we were sitting beside one another. It mimics the configuration in which I would pair with someone while co-located.

I also try to find a good headset with microphone so I can hear and speak to my pair with ease.

**Software:** 

I’ll share my screen with [Screenhero](https://screenhero.com/) if I can. This screen-sharing software allows your pair to click and type on your screen, so it’s ideal for collaborating when you want both participants to play the driving role. If you or your pair is working behind a VPN, though, you may not both be able to access Screenhero. In this case, either the company with the VPN may have an approved solution, or you’re relegated to sharing your screen in a way that _does not_ allow both parties to type. You can do this with Zoom, Google Hangouts, Webex,  or appear.in.

For the video portion, I usually use [appear.in](http://appear.in/). Neither of you has to register: you make up a URL (say appear.in/mypairingsession), and when your pair visits it, you’ll both be able to see video of one another. Any videoconferencing app can of course do the same thing if you cannot use appear.in.

I’m usually writing code in a JetBrains IDE, so I’ll also turn on [Presentation Assistant](https://plugins.jetbrains.com/plugin/7345-presentation-assistant); it’s a plugin that shows your keyboard shortcuts on the screen as you use them, so my pair can see exactly how I’m navigating around the editor. If they’re new to key bindings, I’ll also say them _aloud_ as I use them, just as I would in co-located pairing.

## **Communication**

Pair programming generally includes two roles—one for each of the people in the pair. One person will play the role of _driver_—typing on the keyboard. The other person plays a _navigator_ role; this person thinks a few steps ahead of what the driver is typing and tells the driver where to go and what to implement. We keep these roles separate so both pairs have context at all times_._ The navigator calls the shots, but the shots have to go _through_ the driver to end up in the code—so the navigator can’t go faster than the driver’s capacity to understand what the navigator is suggesting. This forces the navigator to explain things clearly, offer good reasons for their suggestions, and _slow down—_which encourages critical thinking and better solutions.

If the same person is both typing and navigating, then that person now has _all_ the immediate context about what’s going on, and the other person in the pair might lose context. So the two roles are an important safety check for communication and shared context, and they go from critical to indispensable when you’re pairing remotely. If at all possible, especially when remote, _one person navigates, and the other person types._ Nobody gets to do both at once. It’s not fun for the observer, and it’s not safe for preserving shared context. It’s hard enough to observe in person, but it’s even harder to stay engaged while staring at a screen and listening to headphones.

I get that you might not have the perfect setup for this. Maybe there’s a VPN issue or an accessibility issue that prevents one person from driving, so the other person has to do all of it. In this case, if the navigator needs support or alternative ideas while navigating, the driver can’t just have the navigator take the keyboard and suggest alternative solutions as would be ideal. Instead in this case, the driver must become a _driver-navigator_. The driver-navigator accepts responsibility for checking in with the observer every few seconds to make sure they understand what’s going on. This means pausing, asking questions, and closely monitoring the responses. “Mm-hmm” or “yep” often really means “I don’t quite follow, but you have failed to make me feel comfortable stopping you or asking questions, so I’m just going to pretend I understand.” Note that the failure here is not on the observer’s part; it’s on the driver-navigator.

Also, if you find yourself acting as a driver-navigator, you should orate your thoughts and actions to a degree that feels ridiculous. If you have never seen this modeled, turn on the Food Network on TV. Take notes on how the TV chef explains every single thing they do. Notice how they don’t stop talking even for the simple, little things, like shaking paprika onto the dish or sticking a pan in the oven.

It’s the same for programming: _talk through every variable and every loop_. Orate your reasoning for naming your method this or that. Conversely, when the decisions get more complex, make sure you’re speaking your thoughts out loud. This one is my Achilles heel: as the task approaches the limit of my current understanding, I fall silent to think. You can pause to think, but then _explain what you thought_. It’s tough for your pair to watch you type in silence, but when you’re driving it can be easy to go silent by accident. You preserve context when you _talk_. This skill is difficult to learn and challenging to maintain, but it’s exceedingly valuable for pair programming.

## Connecting with your Pair

When you connect with your pair and establish a closer relationship, your programming and your pairing both improve. First, this happens because you want to have reached a comfort level with your pair where you can state what you need from them—like turning up the mic, talking through their typing more, or taking over the keyboard for a while. Second, when you and your pair are friends, it’s easier to challenge each other’s architecture decisions without putting anyone on the defensive.

I like to do this through music, especially during recreational pairing or remote pairing in a locally distracting environment. I’ll listen to music with my pair on one of the myriad [group listening platforms](http://evolver.fm/2012/05/09/top-5-turntable-fm-clones-want-to-throw-your-next-listening-party/). Recreational pairing calls for contemporary/radio music; for a locally distracting environment I’ll use something like traditional Japanese music, classical music, or (if it’s really disruptive around me) white noise.

There are other ways to connect with your pair, too. You can converse on Slack or appear.in about topics other than pairing, or you can share photos.

## To Summarize

If your location or setup separate you from other programmers with whom you would like to pair, it’s absolutely possible to pair remotely. The longer and the more often you’ll be pairing remotely, the more time and money you’ll want to sink into creating a setup that mimics in-person pairing as much as possible. When pairing remotely, it’s helpful to deliberately invest energy in establishing and maintaining a collegial relationship with your pair/s. And when pairing remotely, it’s even easier to lose track of each other’s thoughts than it is in person. You can create a safety check for this by having one person drive while the other person navigates. If this is not possible, the driver-navigator should take extra care to engage their pair and make their pair feel comfortable slowing down the action or asking questions.

##  For more on remote pair programming, check out:

[Remote Pair Programming: Best Practices](https://chelseatroy.com/2016/01/30/remote-pair-programming-best-practices/)
