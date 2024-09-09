---
created: 2024-09-06T14:34:45 (UTC -03:00)
tags: []
source: https://medium.com/@akineralkan/clean-code-notes-chapter-1-clean-code-2ccf23240bd2
author: ✨ Akiner Alkan
---

# Clean Code Notes - Chapter 1: Clean Code | by ✨ Akiner Alkan | Jul, 2024 | Medium

> ## Excerpt
> The insights from Clean Code book are not only timeless but also crucial for anyone serious about software development.

---
[

![✨ Akiner Alkan](https://miro.medium.com/v2/resize:fill:88:88/1*xautWgGl0fHND4-wRQhykg@2x.jpeg)



](https://medium.com/@akineralkan?source=post_page-----2ccf23240bd2--------------------------------)

![](https://miro.medium.com/v2/resize:fit:700/1*WUvGkqywkwAsyxKtpmwrNg.jpeg)

Created with Dall-E

## Introduction

I’ve always been keen on improving my skills and understanding the best practices that lead to maintainable, efficient, and clean software. Recently, I revisited one of the most influential books, “Clean Code” by Robert C. Martin. Reading influential books like this helps deepen our understanding of fundamental concepts.

The insights from this book are not only timeless but also crucial for anyone serious about software development. It’s always amazing how certain principles remain relevant regardless of how technology evolves.

In this series of notes, I’ll be summarizing and reflecting on the key takeaways from each chapter.

My aim is to remind myself of the fundamental principles and share this knowledge with fellow developers, as we all benefit from a collective commitment to writing clean code.

## There Will Be Code

![](https://miro.medium.com/v2/resize:fit:700/1*TBfRsCkp0hUCfQG1v1eRTQ.jpeg)

Created with Dall-E

The chapter begins by emphasizing that no matter what new technologies emerge, coding will always be a fundamental part of software development. This highlights the enduring importance of coding in our profession.

> Writing good code is essential because code is the medium through which we solve problems and create software solutions. Good code is the backbone of effective problem-solving in software.

## Bad Code

Bad code refers to code that is difficult to understand, maintain, and modify. It may work initially but leads to long-term issues.

It’s important to recognize that short-term gains from bad code often lead to long-term pain.

Bad code can result from various reasons, such as tight deadlines, lack of understanding, or poor coding practices. Understanding the causes of bad code can help us avoid making similar mistakes.

Bad code often leads to increased bugs, slower development, and higher costs. The consequences of bad code can significantly impact a project’s success.

![](https://miro.medium.com/v2/resize:fit:700/1*FEett2Xx-KGEBCov8Wc8gg.png)

Created with Dall-E

## The Total Cost of Owning a Mess

When code is messy or poorly written, it incurs a high cost over time. This cost isn’t just financial but also includes the time developers spend trying to understand and fix the code. Investing in clean code from the start can save significant time and resources in the long run.

The longer bad code remains in the system, the more it impacts the productivity and morale of the development team.

Good code, on the other hand, minimizes these costs by being easier to understand, maintain, and extend. Remember that, good code is an investment in the future productivity of a team.

## The Grand Redesign in the Sky

This refers to the often tempting but usually impractical idea of scrapping an entire codebase and starting from scratch. The idea of starting fresh can be alluring but is rarely practical.

While it might seem like a fresh start would solve all the problems of bad code, it’s generally more effective to gradually improve the existing codebase through refactoring and cleaning up code incrementally. Incremental improvements are usually more sustainable and less disruptive.

## Attitude

A key takeaway from this chapter is that the attitude of developers towards their code is crucial. A positive attitude towards writing clean code can significantly influence its quality.

Writing clean code requires a commitment to craftsmanship and a mindset that values quality. Embracing craftsmanship in coding leads to better, more maintainable code.

Developers must take pride in their work and strive to write code that is not only functional but also elegant and clean. Taking pride in our work reflects our commitment to quality.

One another passage which resonates in me is the following:

> Most managers want the truth, even when they don’t act like it. Most managers want good code, even when they are obsessing about the schedule. They may defend the schedule and requirements with passion; but that’s their job. It’s your job to defend the code with equal passion.

**_Defending the quality of our code is as important as meeting deadlines._**

## The Primal Conundrum

The primal conundrum highlights the tension between writing code quickly to meet deadlines and taking the time to write clean, quality code. Balancing speed and quality is one of the biggest challenges in software development.

While the pressure to deliver can lead to shortcuts and bad code, in the long run, investing in clean code pays off by reducing maintenance time and preventing future problems. Investing time in clean code now saves time and headaches in the future.

## The Art of Clean Code

Writing clean code is considered an art. It involves continuously learning, practicing, and improving one’s skills. Treating code writing as an art elevates our approach to it.

Clean code is simple, direct, and expresses its intent clearly. Simplicity and clarity are pillars of good code.

It’s not just about making the code work, but making it understandable and maintainable for others who may work on it in the future. Good code should be a joy to work with, not a puzzle to solve.

## What Is Clean Code?

The book provides various perspectives on what constitutes clean code. Different perspectives can help us form a well-rounded understanding.

From these perspectives, the impression should be like the following: Clean code is all about writing code that is easy to read, maintain, and extend. It emphasizes simplicity, clarity, and efficiency.

By following principles like single responsibility, minimizing dependencies, and avoiding duplication, developers can create code that is robust, flexible, and easy to understand. Adhering to these principles helps produce superior code.

Practices such as using meaningful names, keeping functions small, and writing automated tests further contribute to the cleanliness and quality of the code. Practical habits like these reinforce the principles of clean code.

Clean code ultimately leads to better software that is easier to work with, reduces bugs, and increases productivity. Clean code is beneficial for the entire development lifecycle.

## Schools of Thought

This section highlights the idea that thinking styles differ between methodologies. Acknowledging diverse approaches helps us appreciate the breadth of software development practices.

This scenario parallels martial arts, where various schools (Karate, Judo, etc.) each have dedicated adherents, despite no single school being universally “right.”

Mastery requires commitment to one discipline, after which studying other disciplines can enhance understanding. Deep knowledge in one area can enrich our overall skill set.

Similarly, software development has numerous methodologies and styles, none inherently right or wrong. Flexibility in approach can lead to better problem-solving.

However, without an organizing principle, projects become chaotic. An organizing principle helps maintain order and direction.

## We Are Authors

This section highlights the idea that software developers are also authors. Viewing ourselves as authors underscores the importance of clear communication in code.

Just like authors of written prose, developers must think about their audience. Considering our audience improves the clarity and quality of our code.

The audience, in this case, is other developers who will read and maintain the code. Our primary audience is our fellow developers.

Writing code is a form of communication, and it’s important to write it in a way that is clear, concise, and easy to understand. Clear communication in code is essential for effective collaboration.

This mindset encourages developers to take pride in their work and strive to write code that others can easily follow. Pride in our work leads to higher standards and better code.

## The Boy Scout Rule

The Boy Scout Rule is a simple yet powerful principle: “Leave the campground cleaner than you found it.”

Applied to software development, it means that developers should always strive to leave the codebase in a better state than when they found it.

This can be achieved by making small, incremental improvements to the code whenever possible. Always remember to make small improvements and add them up over time.

By consistently applying this rule, a codebase can be gradually improved over time, leading to cleaner and more maintainable code. Consistent effort in improving code pays off in the long run.

## Summary

Chapter 1 of “Clean Code” sets the stage for understanding the importance of writing clean, maintainable, and readable code. Setting a strong foundation is crucial for good coding practices.

It discusses the pitfalls of bad code, the high cost of maintaining a messy codebase, and the importance of a developer’s attitude towards their work. Awareness of these aspects helps us avoid common pitfalls.

Clean code is an art that requires ongoing effort and a commitment to quality, but it ultimately leads to more efficient and sustainable software development. The effort invested in clean code is well worth it.

> We all need good colleagues who understand the importance of clean code. Having like-minded colleagues enhances team synergy and productivity.

_If you find this article interesting, kindly consider liking and sharing it with others, allowing more people to come across it._

_If you’re curious about technology and software development, I invite you to explore my other articles. You’ll find a range of topics that delve into the world of coding, app creation, and the latest tech trends. Whether you’re a seasoned developer or just starting, there’s something_ [_here_](https://medium.com/@akineralkan) _for everyone._

## References

Robert C. Martin. 2008. Clean Code: A Handbook of Agile Software Craftsmanship (1st. ed.). Prentice Hall PTR, USA.
