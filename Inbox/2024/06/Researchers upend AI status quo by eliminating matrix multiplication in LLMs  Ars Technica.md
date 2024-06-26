---
created: 2024-06-26T18:26:21 (UTC -03:00)
tags: []
source: https://arstechnica.com/information-technology/2024/06/researchers-upend-ai-status-quo-by-eliminating-matrix-multiplication-in-llms/?utm_source=tldrnewsletter
author: Benj Edwards
    -    6/25/2024, 7:27 PM
---

# Researchers upend AI status quo by eliminating matrix multiplication in LLMs | Ars Technica

> ## Excerpt
> Running AI models without floating point matrix math could mean far less power consumption.

---
The researchers' approach involves two main innovations: first, they created a custom LLM and constrained it to use only [ternary values](https://en.wikipedia.org/wiki/Three-valued_logic) (-1, 0, 1) instead of traditional floating-point numbers, which allows for simpler computations. Second, the researchers redesigned the computationally expensive [self-attention mechanism](https://sebastianraschka.com/blog/2023/self-attention-from-scratch.html) in traditional language models with a simpler, more efficient unit (that they called a MatMul-free Linear Gated Recurrent Unit—or MLGRU) that processes words sequentially using basic arithmetic operations instead of matrix multiplications.

Third, they adapted a [Gated Linear Unit](https://paperswithcode.com/method/glu) (GLU)—a gating mechanism to control information flow in neural networks—to use ternary weights for channel mixing. Channel mixing refers to the process of combining and transforming different aspects or features of the data the AI is working with, similar to how a DJ might mix different audio channels to create a cohesive song.

These changes, combined with a custom hardware implementation to accelerate ternary operations through the aforementioned FPGA chip, allowed the researchers to achieve what they claim is performance comparable to state-of-the-art models while reducing energy use. Although they ran comparisons on GPUs to benchmark against traditional models, the MatMul-free models are designed to operate efficiently on hardware that is optimized for simpler arithmetic operations, such as FPGAs. This suggests that these models could potentially be run on various types of hardware, including those with more limited computational resources than GPUs.

[![This chart taken from the paper shows relative performance of the MatMul-free LLM compared to a conventional (Transformer++) LLM on benchmarks.](https://cdn.arstechnica.net/wp-content/uploads/2024/06/matmul-free-chart-640x603.jpg)](https://cdn.arstechnica.net/wp-content/uploads/2024/06/matmul-free-chart.jpg)

[Enlarge](https://cdn.arstechnica.net/wp-content/uploads/2024/06/matmul-free-chart.jpg) / This chart taken from the paper shows relative performance of the MatMul-free LLM compared to a conventional (Transformer++) LLM on benchmarks.

To evaluate their approach, the researchers compared their MatMul-free LM against a reproduced [Llama-2-style](https://arstechnica.com/information-technology/2023/07/meta-launches-llama-2-an-open-source-ai-model-that-allows-commercial-applications/) model (which they call "Transformer++") across three model sizes: 370M, 1.3B, and 2.7B parameters. All models were pre-trained on the [SlimPajama dataset](https://www.cerebras.net/blog/slimpajama-a-627b-token-cleaned-and-deduplicated-version-of-redpajama), with the larger models trained on 100 billion tokens each. Researchers claim the MatMul-free LM achieved competitive performance against the Llama 2 baseline on several benchmark tasks, including answering questions, commonsense reasoning, and physical understanding.

In addition to power reductions, the researchers' MatMul-free LM significantly reduced memory usage. Their optimized GPU implementation decreased memory consumption by up to 61 percent during training compared to an unoptimized baseline.

To be clear, a 2.7B parameter Llama-2 model is a long way from the current best LLMs on the market, such as [GPT-4](https://arstechnica.com/information-technology/2023/03/openai-announces-gpt-4-its-next-generation-ai-language-model/), which is estimated to have [over 1 trillion parameters](https://the-decoder.com/gpt-4-architecture-datasets-costs-and-more-leaked/) in aggregate. GPT-3 shipped with 175 billion parameters in 2020. Parameter count generally means more complexity (and, roughly, capability) baked into the model, but at the same time, researchers have been [finding ways](https://arstechnica.com/information-technology/2024/04/microsofts-phi-3-shows-the-surprising-power-of-small-locally-run-ai-language-models/) to achieve higher-level LLM performance with fewer parameters.

So, we're not talking ChatGPT-level processing capability here yet, but the UC Santa Cruz technique does not necessarily preclude that level of performance, given more resources.

## Extrapolating into the future

The researchers say that scaling laws observed in their experiments suggest that the MatMul-free LM may also outperform traditional LLMs at very large scales. The researchers project that their approach could theoretically intersect with and surpass the performance of standard LLMs at scales around 10²³ FLOPS, which is roughly equivalent to the training compute required for models like Meta's [Llama-3 8B](https://arstechnica.com/information-technology/2024/04/meta-releases-chatgpt-like-ai-site-and-open-weights-llama-3-model/) or Llama-2 70B.

However, the authors note that their work has limitations. The MatMul-free LM has not been tested on extremely large-scale models (e.g., 100 billion-plus parameters) due to computational constraints. They call for institutions with larger resources to invest in scaling up and further developing this lightweight approach to language modeling.

_The article was updated on June 26, 2024 at 9:20 AM to remove an inaccurate power estimate related to running a LLM locally on a RTX 3060 created by the author._
