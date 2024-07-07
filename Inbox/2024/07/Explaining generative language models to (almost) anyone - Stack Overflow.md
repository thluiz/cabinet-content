---
created: 2024-07-06T17:39:51 (UTC -03:00)
tags: []
source: https://stackoverflow.blog/2024/06/27/explaining-generative-language-models-to-almost-anyone/?utm_campaign=the-overflow-newsletter&utm_medium=email&utm_source=iterable
author: Cameron R. Wolfe, PhD
---

# Explaining generative language models to (almost) anyone - Stack Overflow

> ## Excerpt
> Generative AI has now become a popular topic among both researchers and the general public. Now more than ever before, it is important that researchers and engineers (i.e., those building the technology) develop an ability to communicate the nuances of their creations to others. A failure to communicate the technical aspects of AI in an understandable and accessible manner could lead to widespread public skepticism (e.g., research on nuclear energy went down a comparable path) or the enactment of overly-restrictive legislation that hinders forward progress in our field.

---
Generative AI has now become a popular topic among both researchers and the general public. Now more than ever before, it is important that researchers and engineers (i.e., those building the technology) develop an ability to communicate the nuances of their creations to others. A failure to communicate the technical aspects of AI in an understandable and accessible manner could lead to widespread public skepticism (e.g., research on nuclear energy went down a comparable path) or the enactment of overly-restrictive legislation that hinders forward progress in our field.

Here’s a simple, three-part framework that you can use to explain generative language models to (almost) anyone.

1.  Transformer architecture: the neural network architecture used by LLMs.
2.  Language model pretraining: the (initial) training process used by LLMs.
3.  The alignment process: how we teach LLMs to behave to our liking.

Although AI researchers might know these techniques well, it is important that we know how to explain them in simple terms as well! AI is no longer just a research topic, but rather a topic of public interest.

## Transformer architecture

Most recent generative language models are based upon the transformer architecture. Although the transformer was originally proposed with two modules (i.e., an encoder and a decoder), generative LLMs use a decoder-only variant of this architecture. This architecture takes as input a sequence of tokens (i.e., words or subwords) that have been embedded into a corresponding vector representation and transforms them via two repeated operations:

-   Masked self-attention: looks at other tokens in the sequence (i..e, those that precede the current token).
-   Feed-forward transformation: transforms each token representation individually.

These two operations each play a distinct and crucial role. By stacking several blocks of masked self-attention and feed-forward transformations on top of each other, we get the neural network architecture that is used by most generative LLMs today.

## Pretraining

Self-supervised learning refers to the idea of using signals that are already present in raw data to train a machine learning model. In the case of generative language models, the most commonly-used objective for self-supervised learning is next token prediction, also known as the standard language modeling objective. Interestingly, this objective—despite being quite simple to understand—is the core of all generative language models. To pretrain a generative language model, we first curate a large corpus of raw text (e.g., from books, the web, scientific publications, and much more) to use as a dataset. Starting from a randomly initialized model, we then pretrain the LLM by iteratively performing the following steps:

1\. Sample a sequence of raw text from the dataset.

2\. Pass this textual sequence through the decoder-only transformer.

3\. Train the model to accurately predict the next token at each position within the sequence.

## Alignment

After pretraining, the LLM can accurately perform next token prediction, but its output is oftentimes repetitive and uninteresting. The alignment process teaches a language model how to generate text that aligns with the desires of a human user. To align a language model, we first define a set of alignment criteria (e.g., helpful and harmless). To instill each of these alignment criteria within the model, we perform fine-tuning via supervised finetuning (SFT) and reinforcement learning from human feedback (RLHF), which together form the three-step technique for alignment proposed by InstructGPT.
