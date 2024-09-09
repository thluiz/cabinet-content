---
created: 2024-09-05T21:19:34 (UTC -03:00)
tags: []
source: https://machine-learning-made-simple.medium.com/why-mean-squared-error-hates-outliers-b601e3b116f0
author: Devansh
---

# Why Mean Squared Error Hates Outliers | by Devansh | Sep, 2024 | Medium

> ## Excerpt
> Too many ML folk blindly use MSE (Mean Squared Error) as the default error function without thinking about the consequences. Small changes in your error functions can have HUGE impacts on your AI…

---
## Why you should not blindly use error functions

[

![Devansh](https://miro.medium.com/v2/resize:fill:88:88/1*xiFRgHfgfMR7S111UB2hMw.jpeg)



](https://machine-learning-made-simple.medium.com/?source=post_page-----b601e3b116f0--------------------------------)

Too many ML folk blindly use MSE (Mean Squared Error) as the default error function without thinking about the consequences. Small changes in your error functions can have HUGE impacts on your AI training behavior.

To illustrate, we’ll discuss two well-known error functions- MSE and MAE (absolute error).

Mathematically, these are different because-

\-)MSE squares the errors (differences between predicted and actual values), which means that larger errors are heavily penalized.

\-)MAE simply takes the absolute value of the errors. All errors are treated proportionally to their size.

![](https://miro.medium.com/v2/resize:fit:875/0*t2DOPtepRFgD_kgB)

This leads to some pretty cool outcomes. For one, “MSE really hates outliers, but ignores small mistakes”. Let’s talk about why-

When your error is small (<1), then squaring it reduces the value (0.5 \*0.5 =0.25). When the error is large, squaring it increases the error (2\*2=4). Functions like MAE (or AE) will flip this dynamic.

This can be a big difference maker in your AI training. Here’s an example of when MAE becomes better (look at image two). In it, while both LS (least squares- another way calling MSE) and AE (Angular Embeddings, which are non-convex like MAE) can reconstruct the ground truth with no issues in normal conditions, AE absolutely dog-walks LS when we add in more outliers-

![](https://miro.medium.com/v2/resize:fit:804/0*poP8T0PD0cCN8Pp8)

> “AE with a quadratic criterion in the complex domain is remarkably robust to outliers, unlike its real domain counterpart LS. (a) A groundtruth surface with a height range of 1 and a maximal gradient magnitude of 0.16. (b) Gaussian noise with ¼ 0:05 is added to the pairwise height differences between pixels within a city-block distance of 2. Integration of these differences by LS and AE shows comparable reconstructions, both with a standard error of 0.0085. © Uniformly distributed outliers of values 3 are added to 10 percent of these noisy height differences. The standard error becomes 0.1666 for LS and 0.0184 for AE. The surface reconstructed by LS is barely recognizable, while that by AE remains a close approximation to the ground-truth surface”

These are just some of the many design choices you should consider when designing your AI.

If you’ve developed an interest in Angular Embeddings, check out our guide to improving AI Embedding with Geometry here-

I put a lot of work into writing this newsletter. To do so, I rely on you for support. If a few more people choose to become paid subscribers, the Chocolate Milk Cult can continue to provide high-quality and accessible education and opportunities to anyone who needs it. If you think this mission is worth contributing to, please consider a premium subscription. You can do so for less than the cost of a Netflix Subscription [(pay what you want here)](https://artificialintelligencemadesimple.substack.com/p/help-me-take-ai-made-simple-to-the).

![](https://miro.medium.com/v2/resize:fit:868/1*7tzzcjH6pmoYkQ-uGAnwfQ.png)

**_Many companies have a learning budget, and you can expense your subscription through that budget._** [**_You can use the following for an email template_**](https://docs.google.com/document/d/1xy6CNE8S7ZIM1LPKc5qdjwLJcqj6lwxzv3HFz3gEU14/edit?usp=sharing)**_._**

I regularly share mini-updates on what I read on the Microblogging sites [X(https://twitter.com/Machine01776819](https://substack.com/redirect/ff622832-9505-453b-ae09-978237ceceb0?j=eyJ1IjoiNHRuYncifQ.nS0IHgRyrTV_q5g5rpAjxOmBlt9SxaItll-b0wAdi-o)), [Threads(https://www.threads.net/@iseethings404](https://www.threads.net/@iseethings404)), and [TikTok(https://www.tiktok.com/@devansh\_ai\_made\_simple](https://www.tiktok.com/@chocolatemilkcultleader?_t=8lBL3GzTyXK&_r=1))- so follow me there if you’re interested in keeping up with my learnings.

## Reach out to me

Use the links below to check out my other content, learn more about tutoring, reach out to me about projects, or just to say hi.

[Small Snippets about Tech, AI and Machine Learning over here](https://www.instagram.com/yourgodandsavior/)

[AI Newsletter- https://artificialintelligencemadesimple.substack.com/](https://artificialintelligencemadesimple.substack.com/)

[My grandma’s favorite Tech Newsletter- https://codinginterviewsmadesimple.substack.com/](https://codinginterviewsmadesimple.substack.com/)

Check out my other articles on Medium. : [https://rb.gy/zn1aiu](https://machine-learning-made-simple.medium.com/)

My YouTube: [https://rb.gy/88iwdd](https://rb.gy/88iwdd)

Reach out to me on LinkedIn. Let’s connect: [https://rb.gy/m5ok2y](https://www.linkedin.com/in/devansh-devansh-516004168/)

My Instagram: [https://rb.gy/gmvuy9](https://rb.gy/gmvuy9)

My Twitter: [https://twitter.com/Machine01776819](https://twitter.com/Machine01776819)
