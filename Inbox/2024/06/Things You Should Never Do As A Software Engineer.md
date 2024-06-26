---
created: 2024-06-03T09:09:54 (UTC -03:00)
tags: []
source: https://favtutor.com/articles/donts-for-software-engineer/?ref=dailydev
author: 
---

# Things You Should Never Do As A Software Engineer

> ## Excerpt
> Here are some of the things that software developers should avoid when coding to improve their work performance and mental health.

---
I’ve been working as a software engineer for more than 5 years. That doesn’t make me an expert, but I am pretty sure I’ve made enough mistakes to share with you. Here are the 10 things you should never do as a software engineer.

### **1) Being Perfectionist**

Nothing is perfect, and I am sure there’s no such thing as “perfect code” also.

Software development is an iterative process. You write code, test it, get feedback, refactor it, and repeat. Something that works today might not work tomorrow. Hence the software should be flexible and easy to change (that’s why it’s called soft-ware).

I am not saying you should abandon the pursuit of perfection. I am just saying you should be aware of writing code that is too rigid and too complex.

Being professional is different from being a perfectionist – it’s more like being optimally good for most of the time.

### **2) “Please, give me some time to refactor!”**

Code refactoring is the process of restructuring existing computer code without changing its external behaviour. The code without refactoring will start to “stink”, which means it will be difficult to be understood or utilized by other developers.

![](https://favtutor.com/articles/wp-content/uploads/2024/05/gc2m1tdq22w81.jpg)

We developers all know we should always refactor code after implementing features. The problem is that non-technical members don’t care about such details. They’ll often ask you “_We’re a fast-growing company, so I want you to focus on adding features first.”. But soon the code becomes unmaintainable and you’ll be begging “Please, give us some time to refactor!_“.

Do not beg for extra hours or days to do refactoring. Make the refactoring part of your feature implementation.

### **3) Misunderstanding what “legacy code” means**

The web development ecosystem is famous for being fast-changing. I remember one of my previous web projects which was using Next.js version 10 or something. By the time I started working on it, Next.js version 11 was released, packed with new features and improvements. Suddenly my version 10 project felt like a legacy project.

![](https://favtutor.com/articles/wp-content/uploads/2024/05/vbch95wwao121.jpg)

Many people misunderstand what “legacy code” means “old code”, but it’s not. According to Michael Feathers, author of “Working Effectively with Legacy Code”, legacy code is code without tests. If you can’t test your code, you can’t refactor it. If you can’t refactor it, you can’t maintain it.

The “old” Next.js project actually had decent test coverage, and everything was working fine. It was “well-maintained code”, not “legacy code”. Please, do not waste your time chasing the tools, just because they are new and shiny. Remember that Github is still running on Ruby on Rails, which is 17 years old.

### **4) “Functional programming is the best!”**

Yeah, functional programming is a new thing – all the cool kids are doing it. But that doesn’t mean you should use it everywhere.

For example, if you’re working on a Flutter project, it might not be a good idea to use functional programming in the UI layer. Oftentimes, using too much “purely functional” code in the UI layer can cause performance issues, due to needless re-renders. Flutter is designed to be used in object-oriented programming style, so you should use it that way.

Now that doesn’t mean you should avoid functional programming – or any other cool thing – at all. From the same example, you can use functional programming in the business logic layer, where it’s more suitable. Just be aware of the context you are working in, and choose the right tool for the job.

### **5) Following the “best practices” blindly**

Clean architecture, SOLID principles, DRY, KISS, YAGNI, TDD, BDD, CI/CD, etc. There are so many best practices in software development. Those are invented with good intentions, but you shouldn’t follow them blindly.

For example, TDD (Test-Driven Development) is a great practice to ensure your code is working as expected. But if you are working with REPL-friendly languages like Clojure or Python, you might not need to do TDD for everything.

The sole purpose of TDD is to get feedback as soon as possible. If you can get the feedback without writing tests, then you don’t need to do TDD (although, you should write a test!).

### **6) Struggling alone**

I’ve seen many junior developers who are eager to show their so-called “problem-solving skills”. They often struggle to solve a problem that has already been solved by others. Please don’t be that guy. Don’t reinvent the wheel.

The greatest minds in the world are the ones who stand on the shoulders of giants.

When you start working in a team, you’ll realize that you can learn a lot from your teammates who have more experience than you. They are your “giants”. Hop onto their shoulders, and don’t waste your time jumping off to the ground again. Your goal is now to aim for the taller giants.

### **7) Falling into the “flow”, without self-awareness**

Have you ever experienced the “flow”? It’s a state of mind where you are fully immersed in a task, feeling energized and focused. As a programmer, falling into the “flow” feels like the code is writing itself, and you are just a medium. You are in the zone.

![](https://favtutor.com/articles/wp-content/uploads/2024/05/bf2a43e0d76918d1f38537acc9b59d6c245a6eb1.jpeg)

But be careful – you might end up writing “too much” code. I often find myself over-engineering stuff when I am in the “flow”. Not only me, but Robert C. Martin – the author of “Clean Code” – also has experienced counter-productivity while in flow.

To intentionally break the flow, I recommend using the “Pomodoro technique”. It’s a time management method where you work for 25 minutes and take a 5-minute break. It helps you to stay focused and avoid burnout.

### **8) Not moving your body**

Working as a software engineer is not easy. You’ll be sitting in front of a computer for hours, typing on the keyboard, and staring at the screen. It’s easy to forget about your health when you are in the “flow”. But remember, your health is the most important thing. Your brain is no use if your body is not working properly.

Try to move your body every 30 minutes (or 25 minutes if you are using the Pomodoro technique). Stand up, stretch your body, walk around, and drink some water. It will help you to stay focused and avoid burnout.

### **9) Forgetting how fun it is to be a programmer**

When I first started coding, I was so excited about it. I was building things, solving problems, and learning new things every day.

But as time went by, I started to forget how fun it is to code. I was too focused on writing “clean code”, following “best practices”, and solving “hard problems”. More often, I couldn’t really write my own code, because I was too busy following others’ and company’s code. Where did my creativity go?

You should always remember how fun it is to be a programmer. I know it’s hard, but try to find time to work on your own projects, learn new things, and build something cool. In your workspace, try communicating with your teammates about exciting new technologies, even if you don’t use them in your project. It will help you to stay motivated and inspired.

### **10) Being a “coder”, not a software engineer**

There’s a difference between being a “coder” and a “software engineer”. A coder is someone who writes code, while a software engineer is someone who solves problems using code. I’ve got 2 strong reasons why you shouldn’t be a coder:

-   So-called “coders” will be replaced by AI in the future (actually, they are already being replaced). I know it is somewhat controversial to say, but it’s sadly true.
-   People don’t care about your code, they care about how well you solve their problems.

![](https://favtutor.com/articles/wp-content/uploads/2024/05/zku9xsa0ob6a1.jpg)

Be a “problem solver” who uses code as a tool. Understand the problem, find the best solution, and implement it using code. That’s what a software engineer does.

## **Conclusion**

These are the 10 things I personally think you should never do as a software engineer. Those are based on my own personal experience, so I don’t expect everyone to agree with me. But I hope you find them helpful.

If you have any other things to add, please let me know in the comments. Thank you for reading!
