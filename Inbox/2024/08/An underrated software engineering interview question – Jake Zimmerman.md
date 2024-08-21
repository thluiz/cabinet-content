---
created: 2024-08-21T14:05:52 (UTC -03:00)
tags: []
source: https://blog.jez.io/bugsquash/?utm_source=tldrnewsletter
author: Jake Zimmerman
---

# An underrated software engineering interview question – Jake Zimmerman

> ## Excerpt
> I love bug squash interviews.

---
The highest signal-to-noise software engineering interview I’ve seen goes like this:

> Here’s a repo you’ve never seen before. Here’s how to build and run the tests in this repo. There’s a bug: what we’re observing is X, but we want to see Y instead. Find the bug and maybe even write some code to fix it.

I’ve only ever heard it called a “bug squash” interview, but if it goes by other names I’d love to learn them (if only to hear about more companies that use this interview style).

There’s a lot of reasons why I like this interview:

1.  It reflects everyday software development. Have I ever been paid to write a function that checks if a word is a palindrome? No. Have I had to dive into foreign code (or maybe just: “code foreign to me”) to unbreak something? Literally every day.
    
2.  It’s fun. It’s fun in the same way an escape room is fun. It’s fun because of the dopamine you get when the test suite blinks green. It’s fun because fixing self-contained, reproducible bugs like this is what so many of us enjoy the most about software engineering in the first place.
    
3.  It’s easy for the candidate to self-assess their own progress.I’ve sometimes heard this reduced to “it’s fair.” At the very least, maybe this is a necessary condition for a fair interview, though I’m unclear whether it’s sufficient. When the candidate isn’t doing well on it, they probably already knew it without needing to be told as much by the recruiter. This is a much better candidate experience than the whiplash of thinking you solved a question perfectly only to realize that the interviewer was looking for something else entirely.
    
4.  It’s especially useful in certain kinds of high-growth companies. If the median tenure of software engineers at a company is low,2 years? 1 year? God forbid… 6 months? there’s going to be a huge mass of code that the majority of engineers are seeing for the first time. In these companies, foreign code is almost the norm, not the exception!
    
5.  It gives experienced candidates a moment in the spotlight. I’ve seen candidates drop into `strace` to debug a Ruby program. I’ve seen candidates fire up `mitmproxy` to debug a Python web server. I’ve seen candidates wield their text editor like it was an extension of their fingertips. When someone is in complete command of their tools, this interview showcases that better than any other interview.
    
6.  Cheating effectively is indistinguishable from debugging skill. Even with knowledge of the exact bug ahead of time, if you just open the file with the bug, barf out the code to fix it, and run the tests, you’re going to fail the interview.
    
    Passing involves convincing the interviewer that the technique used to find the bug is repeatable and somewhat “scientific.” Is the candidate making hypotheses about how the codebase works and testing them? Does the candidate have good tools or techniques for finding code and navigating a codebase? Is the candidate building up a mental model of the codebase as a whole, or just blindly tracing code line by line, hoping the bug eventually falls under their gaze?
    
    Even if you only did these things because someone slipped you a prepared, hour-long script to recite, congrats: you just cheated your way into learning how to debug. That’s a useful skill!
    
7.  It recognizes how much time in software development is spent reading and understanding existing code vs writing net new code. (Hint: even in the most product-heavy, greenfield teams, it’s not zero!)
    
    As a result, it doesn’t require “practicing how to interview” the same way that leetcode questions do. Candidates who are good at this interview are good at it because it’s a skill they’ve practiced day in and day out their entire career.
    

It’s my favorite interview to give by far.

Here’s the problems with it:

1.  Writing these interview questions takes time. The bug needs to fit some sort of sweet spot between “too hard to make meaningful progress on in an hour” and “so trivial it takes 5 minutes.” It’s not going to be obvious whether a bug lies in that sweet spot without calibration testing.
    
2.  You’re probably going to want more than one bug per project. Some candidates _will_ find the bug in 10-15 minutes naturally, but you’re going to want to keep using the remaining ~40 minutes digging in to expose their full ability: you want to _know_ when you’ve found a strong passing candidate vs an average passing candidate. Finding more of these already-rare “sweet spot” bugs is more work.
    
3.  You’ll need at least one question _per language_ that you want to allow people to interview in. You’ll probably start with a few like JavaScript, Python, Java, and C++, but you probably want to extend that to C#, Objective-C, Ruby, Go, Rust, etc. If you ask candidates to choose and they’re forced to choose a language whose debugging tools they’re unfamiliar with, you’ll get less signal. “Was this performance poor because they’re bad at debugging, or just unfamiliar with this language’s tools?”
    
4.  Similarly, unless the language as a whole makes it prohibitively difficult to develop on certain operating systems, you’re going to want to make sure that the project in question has multi-platform support (basically: you’ll want projects which build and pass tests on Windows absent WSL).
    
5.  The codebase has to be relatively easy to build and test. Candidates are going to show up to interview with the personal laptop that they last used 10 years ago in college and never needed to replace because work always gave them a new one every 2 years. It might take you 10 seconds to build and test that project, but on their 2013 MacBook Air with Zoom running in the background, their dev loop is easily 10-20x longer.
    
    For companies that do on-site interview loops (rare post-pandemic), it’s possible to maintain a pool of interview loaner laptops to bail out candidates like this, but this also means needing to pre-install most of the tools (IDEs, terminal emulators, text editors, etc.) that candidates are likely to want, and replicating that across macOS, Windows, and Linux.
    
6.  It’s requires upkeep, which is rare for most interviews. The snapshot of the project at a point in time will bitrot as new language, dependency, and operating system versions are released. In practice it’s not usually _too_ bad, because if the question bitrots, chances are the same problem is affecting the repo the snapshot was taken from.
    
7.  It depends on the candidate being able to install the dependencies of the project on their laptop (including any system or language-level dependencies). In practice, this also isn’t too bad, as you can provide candidates a mostly-empty “before your interview” sandbox repo which mimics the build and test setup for the target repo so they can arrive to the interview with a working development environment.
    

I’d love to see this kind of interview used more widely.

It’s not the end all, be all of interviews. A candidate that passes the bug squash interview is not instantly hired.

Rather, the bug squash interview is part of a balanced ~breakfast~ interview loop—it shines alongside classic interview types like one or two leetcode questions, an “experience and goals” interview, maybe a software architecture interview. In that setting: the bug squash interview tests a skill rarely covered by existing interview loops, yet is extremely common in everyday software engineering.

And honestly: it’s plain fun.
