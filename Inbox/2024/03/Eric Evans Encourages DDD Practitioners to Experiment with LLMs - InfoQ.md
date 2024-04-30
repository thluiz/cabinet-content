---
created: 2024-03-19T19:31:40 (UTC -03:00)
tags: [Evans ddd experiment llm,Architecture & Design,Domain-Driven Design,Design,Large language models,Architecture,Methodologies,Artificial Intelligence,Domain Driven Design,]
source: https://www.infoq.com/news/2024/03/Evans-ddd-experiment-llm/?ref=dailydev
author: Thomas Betts
---

# Eric Evans Encourages DDD Practitioners to Experiment with LLMs - InfoQ

> ## Excerpt
> In his keynote presentation at Explore DDD 2024 in Denver, Colorado, Eric Evans, author of Domain-Driven Design, argued that software designers need to look for innovative ways to incorporate large la

---
In his keynote presentation at [Explore DDD](https://exploreddd.com/), [Eric Evans](https://www.infoq.com/profile/Eric-Evans/), author of Domain-Driven Design, argued that software designers need to look for innovative ways to incorporate large language models into their systems. He encouraged everyone to start learning about LLMs and conducting experiments now, and sharing the results and learnings from those experiments with the community.

Evans believes there can be good combinations of DDD and AI-oriented software. He said, "because some parts of a complex system never fit into structured parts of domain models, we throw those over to humans to handle. Maybe we'll have some hard-coded, some human-handled, and a third, LLM-supported category."

Using language familiar to DDD practitioners, he said a trained language model is a bounded context. Instead of using models trained on broad language and that are intended for general purposes, such as ChatGPT, training a language model on a ubiquitous language of a bounded context makes it far more useful for specific needs. 

With generic LLMs, someone has to write careful, artificial prompts to achieve a desired response. Instead, Evans proposed having several fine-tuned models, each intended for a different purpose. He sees this as a strong separation of concerns. He predicts future domain modelers will identify tasks and subdomains that involve interpreting natural language input, and will naturally slot that into their designs. The infrastructure isn't quite ready for that yet, but trends suggest it will come soon. 

Evans emphasized that his thoughts must be considered in the context of when he was speaking, on March 14, 2024, because the landscape is changing so rapidly. Six months ago, he did not know enough about the subject, and a year from now his comments may be irrelevant. He compared our current situation to the late 90s, when he learned multiple ways to build a website, none of which would be applicable today.

Throughout the conference, other notable voices in the DDD community responded to Evans' thoughts. [Vaughn Vernon](https://www.infoq.com/profile/Vaughn-Vernon/), author of Implementing Domain-Driven Design, was largely supportive of the idea of finding novel uses for LLMs beyond the common chatbot. In his vision for self-healing software, he sees a place for a tool like ChatGPT to respond to runtime exceptions, and be a "fix suggester" that automatically creates a pull request with suggested code to resolve the bug.

However, some people remain skeptical about all the benefits of LLMs. During a panel discussion on the intersection of DDD and LLMs, [Chris Richardson](https://www.infoq.com/profile/Chris-Richardson/), author of Microservice Patterns, expressed concerns about the high monetary and computing costs of LLMs. When Richardson wondered if any service that operated an LLM could ever turn a profit, Evans replied that fine tuning makes an inexpensive model cheaper and faster than an expensive model. Fellow panelist, [Jessica Kerr](https://www.infoq.com/profile/Jessica-Kerr/), a principal developer advocate at Honeycomb.io, said, "we need to find what's valuable, then make it economical."

During his keynote, Evans went into detail about some of the experiments he conducted as part of his personal education into the capabilities of LLMs. At first, working with game designer Reed Berkowitz, he tried using ChatGPT to make a non-player character (NPC) respond to player input. An evolution of prompt engineering led him to the realization that the responses were more consistent if broken into smaller chunks, rather than one, long prompt. This approach follows his ideas of DDD breaking down complex problems.

The need for smaller, more specialized prompts naturally led to wanting a more specialized model, which would both provide better output while also being more performant and cost effective. His goal in explaining his research methods was to show how useful it can be to experiment with the technology. Although frustrating at times, the process was immensely rewarding, and many attendees said they could relate to the sense of satisfaction when you learn how to get something new working for the first time.

The [Explore DDD conference](https://exploreddd.com/) took place in Denver, Colorado, from March 12-15, 2024. Most presentations during the conference were recorded, and will be posted to the [@ExploreDDD YouTube channel](https://www.youtube.com/@ExploreDDD) over the next several weeks, and shared on the [Explore DDD LinkedIn page](https://www.linkedin.com/company/exploreddd/), starting with the opening keynote by Eric Evans.

## About the Author

[![](https://cdn.infoq.com/statics_s2_20240319075238/images/profiles/pSqI6HrU3k9rmmVjwS34OHG0bOMYiE6a.jpg)](https://www.infoq.com/profile/Thomas-Betts/)

#### **Thomas Betts**

Thomas Betts is the Lead Editor for Architecture and Design at InfoQ, a co-host of the InfoQ Podcast, and a Laureate Software Architect at Blackbaud. For over two decades, his focus has always been on providing software solutions that delight his customers. He has worked in a variety of industries, including social good, retail, finance, health care, defense and travel. Thomas lives in Denver with his wife and son, and they love hiking and otherwise exploring beautiful Colorado.

Show more
