---
created: 2024-08-21T18:56:25 (UTC -03:00)
tags: []
source: https://www.justinmath.com/how-to-get-from-high-school-math-to-cutting-edge-ml-ai/?utm_source=tldrnewsletter
author: 
---

# How to get from high school math to cutting-edge ML/AI: a detailed 4-stage roadmap with links to the best learning resources that I’m aware of. - Justin Skycak

> ## Excerpt
> 1) Foundational math. 2) Classical machine learning. 3) Deep learning. 4) Cutting-edge machine learning.

---
_1) Foundational math. 2) Classical machine learning. 3) Deep learning. 4) Cutting-edge machine learning._

I recently talked to a number of people who work in software and want to get to the point where they can read serious ML/AI papers like [_Denoising Diffusion Probabilistic Models_](https://arxiv.org/pdf/2006.11239).

But even though they did well in high school math, even AP Calculus, maybe even learned some undergraduate math…

the math in these cutting-edge ML papers still looks like hieroglyphics.

**So, how do you get from high school math to cutting-edge ML?**

Here’s a 4-stage roadmap.

I’ll start by briefly describing all the stages – and then I’ll go back to each stage for a deep dive where I fully explain the rationale and point you to resources that you can use to guide your learning.

-   _Stage 1: Foundational Math._ All the high school and university-level math that underpins machine learning. All of algebra, a lot of single-variable calculus / linear algebra / probability / statistics, and a bit of multivariable calculus.
-   _Stage 2: Classical Machine Learning._ Coding up streamlined versions of basic regression and classification models, all the way from linear regression to small multi-layer neural networks.
-   _Stage 3: Deep Learning._ Multi-layer neural networks with many parameters, where the architecture of the network is tailored to the specific kind of task you’re trying to get the model to perform.
-   _Stage 4: Cutting-Edge Machine Learning._ Transformers, LLMs, diffusion models, and all the crazy stuff that’s coming out now, that captured your interest to begin with.

  

Note that I’ve spent the past 5+ years working on resources to support learners in stages 1-2, and there is a general lack of serious yet friendly resources in those stages, so I’m going to be including my own resources there (along with some others).

But in stages 3-4, all the resources I’ll reference are things I’m not affiliated with in any way.

Alright, let’s dive in!

## Stage 1: Foundational Math

There’s a lot of math underpinning ML: all of high school algebra, a lot of single-variable calculus / linear algebra / probability / statistics, and a bit of multivariable calculus.

Last year, I spent some time mapping all this math out at a topic-by-topic level. In many of these math courses, some topics are absolutely essential to know for ML, while other topics are unnecessary.

For instance, in multivariable calculus:

-   You absolutely must know how to compute gradients (they show up all the time in the context of gradient descent when training ML models), and you need to be solid on the multivariable chain rule, which underpins the backpropagation algorithm for training neural networks.
-   But on the other hand, many other multivariable calculus topics like divergence, curl, spherical coordinates, and Stokes' theorem don’t really show up anywhere in ML.

  

Once I mapped out the list of required topics, we put together a [_Mathematics for Machine Learning_](https://mathacademy.com/courses/mathematics-for-machine-learning) course whose table of contents can be viewed [here](https://mathacademy.com/courses/mathematics-for-machine-learning).

(To see the list of all 242 individual topics, click on “Content” and then click to expand the Unit boxes.)

If you’re working on Math Academy’s maximum-efficiency learning system, you can learn all this content in about 70 hours if you know your math up through single-variable calculus.

(And if you’re missing background knowledge, that’s totally fine – the diagnostic assessment will automatically figure out the prerequisite material that you need to learn in algebra, calculus, etc. and add it to your learning plan.

Even if you’ve completely forgotten all the math you learned and need to rebuild your foundations from the ground up, starting with fractions, [we’ve got you covered](https://mathacademy.com/adult-students).)

It’s also possible to learn these topics through free online resources. For instance,

-   Khan Academy covers arithmetic up through high school math,
-   OpenStax covers high school and a bit of early university math,
-   MIT OpenCourseWare covers plenty of university math, and
-   for any given math topic, there are plenty of notes online and usually plenty of videos on YouTube.

  

However, to make it all the way through this Stage 1 using free resources, you’ll have to piece together a hodgepodge of scattered educational content, and there will be a lot more unproductive friction that will not only slow you down but also make you more likely to get overwhelmed and give up.

(Personally, I self-studied a bunch of math subjects through MIT OpenCourseWare and a variety of textbooks when I was in high school. These were good resources and I came a long way with them, but for the amount of effort that I put into learning, I could have gone a lot further if my time were used more efficiently. For more info, see my post [_Why Not Just Learn from a Textbook?_](https://justinmath.com/why-not-just-learn-from-a-textbook).)

For most people, this stage of foundational math learning is a make-or-break moment and the hardest part of their machine learning journey.

Learning the foundational math for machine learning is like learning how to read: achieving literacy opens up a world of further learning experiences, but without literacy, that world is completely closed off to you.

## Stage 2: Classical Machine Learning

Once you have your math foundations in place, you’re ready to start coding up streamlined versions of basic ML models from linear regression to small multi-layer neural networks.

But don’t try to jump into the fancy cutting-edge ML models just yet – if you do, you’re going to be confused by a lot of patterns that come from classical machine learning.

Let me give a concrete example of what I mean by this. Let’s go back to that paper [_Denoising Diffusion Probabilistic Models_](https://arxiv.org/pdf/2006.11239).

If you look at the bottom of page 3, you’ll see that equation (8) looks something like the following, but with a lot more subscripts and function arguments (which I’ve left out for simplicity):

L\=E\[12σ‖μ¯−μ‖2\]+C

  
Even if you know

-   what the E means (“expectation” from probability/statistics),
-   what the σ means (“standard deviation” also from probability/statistics),
-   what the ‖ means ("vector norm" from linear algebra and multivariable calculus),
-   what the +C means (arbitrary "constant of integration" that vanishes when we take the derivative, covered in calculus),
-   and so on ...

  

the way these symbols are arranged here might look really confusing.

But if you know your classical machine learning, the equation immediately hits you as resembling a “loss function” that measures the average squared difference between two quantities.

(It’s no coincidence that the authors used the letter L on the left-hand side of the equation – L for “loss.”)

In classical ML, in that average squared difference between two quantities, one of those quantities comes from your model, and the other comes from a data set, so the loss function in a sense measures how “wrong” your model is in its predictions across the data set.

(Loosely speaking, the goal of “training” a ML model is to minimize the loss function, that is, to adjust the parameters of the model to minimize how “wrong” the model is about the data.)

**My point here is that if you know about these modeling patterns from classical ML, then you’ll immediately have intuition about the equations you see in cutting-edge ML papers.**

It’s like how, if a musician has practiced playing their scales and identifying musical keys, they can often jump into any song – even one they’ve never heard it before – and improvise in a way that sounds good, because their understanding of underlying musical patterns has given them a level of “feeling” or “intuition” about the new musical structure in which they’re operating.

Similarly, an experienced software developer who knows all sorts of design patterns might be able to get a sense of how a piece of code operates just by noticing patterns in the high-level structure – even though they haven’t actually gone through line by line to understand the precise operations that are happening in the code.

If you know your classical machine learning, the same thing will happen to you when reading modern ML papers.

Just glancing at equations and diagrams, you’ll find meaningful features jumping out at you in intuitive ways.

Reading the paper will feel like looking at a “picture” where you get a high-level sense of what’s going on and then zoom in on the details afterwards.

But if you don’t know your classical ML, then good luck reading modern ML papers, because the authors are not going to spell all this out for you!

**So, once you have your math foundations in place, how can you get up to speed classical machine learning?**

One problem with many ML courses is that they don’t have students implement the key models from scratch.

Now, I’m not saying you have to implement a full-fledged ML library with all the bells and whistles of scikit-learn, pytorch, tensorflow, keras, etc., …

but simply coding up a streamlined version of each basic ML model along the way from linear regression to small multi-layer neural networks will do wonders for your understanding.

This was the premise of a series of quantitative coding classes that I developed and taught from 2020-23.

-   First, students implemented basic ML algorithms from scratch, coding up streamlined versions of polynomial and logistic regression, k-nearest neighbors, k-means clustering, and parameter fitting via gradient descent.
-   Then, they implemented more advanced ML algorithms such as decision trees and neural networks (still streamlined versions), and they reproduced academic research papers in artificial intelligence leading up to Blondie24, an AI computer program that taught itself to play checkers.

  

I wrote a textbook for those courses, [_Introduction to Algorithms and Machine Learning_](https://justinmath.com/books/#introduction-to-algorithms-and-machine-learning), which includes all the instructional material and exercises that I used during the courses. It’s freely available [here](https://justinmath.com/books/#introduction-to-algorithms-and-machine-learning).

The textbook is self-contained, and once I finished it, my classes were almost entirely self-service. Students read the assigned chapter on their own and completed the exercises. In class, instead of giving traditional lessons, I answered questions and helped students debug their code.

(My students came in without much coding experience, so the book also has you implement canonical data structures and algorithms including sorting, searching, graph traversals, etc. –

you can skip this stuff if you already know it, but if you don’t know it, it would definitely worth using these exercises to develop your algorithmic coding chops in general.

Even if you’ve been working in software for years – if you don’t have experience writing this sort of algorithm-heavy code, it would be a good idea to use this as an opportunity to get some practice.

It’s like how even beyond competitive weightlifting, most serious athletes still lift weights to develop supporting musculature and improve their overall athleticism, since it indirectly carries over to increase their sport-specific performance.)

**IMPORTANT: it’s crucial to point out that the intended learning outcome of _Introduction to Algorithms and Machine Learning_ is pretty general quantitative coding with a focus on ML/AI.**

**While the book provides you with solid foundations, you would still want to follow up with a proper ML course afterwards that goes through the whole zoo of ML algorithms.**

**For this, I would recommend working through Andrew Ng’s acclaimed [Machine Learning Specialization on Coursera](https://coursera.org/specializations/machine-learning-introduction).**

This is a sequence of 3 courses:

-   Course 1: _Supervised Machine Learning: Regression and Classification_
-   Course 2: _Advanced Learning Algorithms_
-   Course 3: _Unsupervised Learning, Recommenders, Reinforcement Learning_

  

But make sure to do the quizzes and assignments! It doesn’t count if you just watch the videos. Passively “following along” is not the same as, or even remotely close to, actually learning.

## Stage 3: Deep Learning

So much of modern machine learning is built upon deep learning, that is, multi-layer neural networks with many parameters, where the architecture of the network is tailored to the specific kind of task you’re trying to get the model to perform.

If you asked me several years ago how to spin up on deep learning, I wouldn’t know what to tell you. While neural networks have been around for a while in the academic literature, deep learning didn’t really take off in popularity until around 2010, which means there wasn’t much demand for educational material until recently.

When I first got interested in deep learning, around 2013-14, the only way to spin up on it was to trudge through miscellaneous research papers, YouTube videos, and blog posts. It was extremely frustrating and inefficient.

Later in the 2010s, a number of deep learning textbooks came out – and while they definitely improved the state of deep learning education, they were seriously lacking concrete computational examples and exercises.

It was kind of like reading a long, thorough lecture, which was better than a hodgepodge of papers, but it was still pretty hard to learn from compared to, say, a well-scaffolded calculus textbook full of worked examples and hands-on exercises.

**More recently, though, one deep learning textbook has caught my eye as a superb educational resource: [_Understanding Deep Learning_](https://udlbook.github.io/udlbook/) by Simon J. D. Prince.**

It’s freely available [here](https://udlbook.github.io/udlbook/), and I would highly recommend to check out [this highlights reel](https://x.com/SimonPrinceAI/status/1686475960973963265) demonstrating what makes the book so awesome.

I’ve read through a number of chapters myself. It’s a serious yet friendly textbook – remarkably detailed and full of visualizations, quick concrete algebraic/numerical examples and exercises, historical notes/references, and references to current work in the field.

Overall, an amazing resource for anyone who has foundational math chops and knowledge of classical ML, and has paid attention to deep learning making headlines through the last decade, but hasn’t kept up with all the technical details.

By the way, it’s not just me who loves this book. It [blew up on HackerNews](https://news.ycombinator.com/item?id=38424939) last year and has 4.8/5 stars across [97 reviews on Amazon](https://amazon.com/dp/0262048647#customerReviews) – and if you read those Amazon reviews, it’s obvious that this book has made a tremendous positive impact on many people’s lives.

**Afterwards, I would also recommend to work through Jeremy Howard’s [_Practical Deep Learning for Coders (Part 1)_](https://course.fast.ai/) on Fast.AI.**

This course has a wealth of great hands-on, guided projects that go beyond textbook exercises.

Again, make sure to do the projects! It doesn’t count if you just watch the videos.

(To be clear: I would still recommend to work through the exercises in the _Understanding Deep Learning_ book so that you come into the Fast.AI projects with plenty of background knowledge.

Projects are great for pulling a bunch of knowledge together and solidifying your knowledge, but when you’re initially learning something for the very first time, it’s more efficient to do so in a lower-complexity, higher-scaffolding setting.

Otherwise, without a serious level of background knowledge, it’s easy to get overwhelmed by a project – and when you’re overwhelmed and spinning your wheels being confused without making much progress, that’s very inefficient for your learning.

By the way, this is an instance of the “[expertise reversal effect](https://en.wikipedia.org/wiki/Expertise_reversal_effect)” in the science of learning.)

## Stage 4: Cutting-Edge Machine Learning

Finally, we reach the end destination: transformers, LLMs, diffusion models, and all the crazy stuff that’s coming out now, that captured your interest to begin with.

Guess what? You know how I said that when I first got interested in deep learning, around 2013-14, the only way to spin up on it was to trudge through miscellaneous research papers, YouTube videos, and blog posts – and it was extremely frustrating and inefficient?

Well, that’s just kind of how it goes when you reach the cutting edge of a field – and when you get to this “Stage 4,” that’s what you can expect (at least, for now, until the field matures further).

That said, even when you’re operating at the edge of human knowledge, there is still a way to optimize your approach.

I’ll quote my colleague Alex Smith who who recently [posted](https://x.com/ninja_maths/status/1820583797491925386) about his own experience getting up to speed on his dissertation topic while doing a PhD in mathematics:

-   _"My biggest mistake when starting my doctoral research was taking a top-down approach. I focused my efforts on a handful of research papers on the frontier of my chosen field, even writing code to solve problems in these papers from day one. However, I soon realized I lacked many foundational prerequisites, making the first year exceptionally tough.
    
    What I should have done was spend 3-6 months dissecting the hell out of all the key research papers and books written on the subject, starting from the very basics (from my knowledge frontier) and working my way up (the bottom-up approach)."
    
    _

  

I know “cutting-edge” might sound like a single line where you cross from reading established textbooks to reading the latest arXiv preprint that blew up yesterday…

but in reality, the cutting edge is more of a “zone” where the amount of guidance gradually fades. Just like how, on a physical knife, you typically see the blade start to narrow a couple millimeters before the true edge.

**At the start of the cutting edge, there may not be any textbooks – but there are typically miscellaneous guided resources like videos, blog posts, and foundational research papers and literature reviews that are easier to follow than the latest research papers all the way at the end of the cutting edge.**

For a concrete example: if you want to work on cutting-edge LLM research, don’t start by immediately trying to build on the latest paper that’s come out.

Instead, start out with a literature search and collect the important papers, videos, blog posts, etc., that will bring you from the beginning of the edge of the field to the very end.

For instance, Andrej Karpathy’s [_Neural Networks: Zero to Hero_](https://youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ&feature=shared) series on YouTube would stand firmly in that pool of resources.

So would Jeremy Howard’s [_From Deep Learning Foundations to Stable Diffusion_](https://course.fast.ai/Lessons/part2.html) on Fast.AI.

## Responses to Follow-Up Questions

I received some great follow-up questions about this post after it gained some traction on X/Twitter (August 2024). Feel free to [contact](https://justinmath.com/contact) me if you have any additional questions that aren’t addressed here.

  
**Don’t you need software engineering skills as well?**

Yes. This roadmap is directed at people who work in software and want to get into serious ML/AI, so I’m assuming that they’re coming in with SWE skills.

But I’m sure there are also lots of people reading who don’t have SWE experience – including a “dual space” of people who have plenty of background in math but none in SWE (I’m sure plenty of math majors fall into this category).

Which brings us to the next question:

  
**Can you also provide a roadmap for learning the fundamentals of CS and coding specifically for doing cutting-edge ML/AI research?**

I’d say that if you don’t know how to do this already, the first order of business would be to implement the canonical data structures and algorithms covered in _Introduction to Algorithms and Machine Learning_ (sorting, searching, graph traversals, etc., basically the stuff you’d see in a typical Data Structures / Algorithms course). If you work through that textbook in full, you’ll naturally cover all that stuff in addition to the core foundations of classical machine learning.

After that, I think it’s easier to pickup the rest of CS/coding as needed along the way since – at least, in my experience – it’s less hierarchical than math.

Don’t get me wrong, there are many CS/coding skills that you’ll need to learn if you don’t know already (off the top of my head: version control, interacting with databases and GPUs, various ML libraries/frameworks), but I these sorts of things can be picked up on the fly because, unlike in a tall hierarchy like math, it’s much easier to identify what prerequisites you’re missing whenever there’s something you don’t know how to do.

Like, let’s say you get to a stage where you need to run a ML model on a GPU, and you don’t know how to hook it up to the GPU. That’s something you’ll need to learn, but it’s not like there’s a mountain of prerequisite knowledge leading up to that task. You don’t have to know much of the underlying theory behind how GPUs work. There are frameworks like PyTorch where you can define a model in terms of tensor operations and the framework handles the process of getting it running efficiently on a GPU. You just have to point the framework to the GPU, which is pretty much just a configuration thing.

Now, I should mention that the way I’m interpreting “ML/AI research” is that you’re focusing on building models that are new in a theoretical sense, as opposed to deploying / scaling up existing models. If you’re actually wanting to get into ML/AI deployment, or maxing out ML/AI compute power, then there’s going to be more CS/coding/software prereqs.

For instance, if you want to further improve the efficiency with which a model utilizes a GPU – like, making improvements to PyTorch itself or something like that – then obviously you’re going to have to know a lot more about low-level GPU programming. But I don’t think that stuff would really fall under the realm of “ML/AI research” proper; it would be more properly categorized as “developing supporting software for ML/AI” or something like that.

  
**So, you don’t need to know graduate-level math for deep learning?**

That’s right, the necessary math would be classified as undergraduate level. Now, I should point out that there are some theoretical ML researchers who are trying to build unifying theories of deep learning, and they sometimes use more advanced/abstract math in their theoretical frameworks that what I’ve talked about here. But that’s a separate thing.

  
**In general, why are there so many arguments about how much math you need to learn for ML?**

Yeah, the question of “how much math do I need for ML/AI” can be a bit polarizing: some people say “no math needed” while others say “you need everything including axiomatic set theory,” but really the most generally correct answer is somewhere in the middle.

And for any sort of argument where a nuanced middle ground has to be established, it seems necessary to explicitly lay out what the specific middle ground is – what should be included, what shouldn’t be included, and why. So that’s what I’ve tried to do here.

  
**Do you think in some way the sheer number of things to study from the get go might be somewhat of a limiting factor, or discourage people? How should one balance just deep studying while also satisfying curiosity and keeping a pulse on the cutting-edge?**

Yeah, I completely agree that the sheer mountain of work ahead of you can be discouraging, so it is definitely worth doing things to keep your interest alive: reading some articles about the cutting edge, toying around with new machine learning models/libraries, etc.

At a fundamental level, playing around not as efficient for learning as actually building up your hard skills through deliberate practice. So you can’t spend all your time tinkering and expect to reach a high level of skill.

However, if you burn yourself out and lose motivation to engage in deliberate practice, and you just stop… then that’s even worse.

I think that ideally, you’d want to play around just enough to keep yourself motivated to keep building up your hard skills through deliberate practice, and you’d spend the rest of the time actually doing the latter.

To zoom out and drive this point home, I want to point out the common misconception that ability is something to be “unlocked” by curiosity (which seems easy), not something “built” by deliberate practice (which seems hard).

This misconception sounds so ridiculous when you imagine it coming from an athletic trainer: “You want to get really good at basketball? Forget about practice drills – you were born to ball; all you need to do to unlock your inner baller is come in with the right attitude and play some pick-up ball at the park.”

Now, I’m not against curiosity/interest. That’s not what I’m trying to say at all. But curiosity/interest does not itself build ability. Curiosity/interest motivates people to engage in deliberate practice, which is what builds ability.

I’m not saying curiosity/interest doesn’t help, I’m just saying it’s not what moves the needle directly. Deliberate practice is what moves the needle directly. Curiosity/interest “greases the wheels,” so to speak, but it’s not what actually moves the wheels.
