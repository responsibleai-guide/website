---
layout: base.njk
title: Recommended Open Models
description: Research and recommendations for the best open models to use as alternatives to proprietary AI services, plus tooling to run them locally.
---

# Recommended Open Models

You can run an AI model on your own computer, for free, with nothing leaving the machine. No account, no subscription, no company reading your prompts.

We have two practical decisions to take:
1. Which model
2. What to run it with

---

## Start Here

If you have never done this before, this is the whole process:

1. Install [LM Studio][lm-studio]. It is a normal desktop app for Mac, Windows, and Linux.
2. Work out how much memory your computer has (see below).
3. Search in the app for one of the models in our table, download it, and start typing.

That is it. No terminal, no code. If you are not sure where to begin, download **OLMo 3.1 7B** and see how you get on. It runs on 8 GB.

The rest of this page explains why we picked those models, and what to do when you outgrow the simple option.

---

## Will It Run on My Computer?

Memory is what decides. Not processor speed, not disk space.

Where to look for your number:

- **Mac (M1 or newer):** the memory figure on the spec sheet, e.g. "16 GB". The chip shares it with the graphics.
- **Windows or Linux with a dedicated graphics card:** the memory on the card itself, not the computer's main RAM. A machine with 32 GB of RAM and an 8 GB graphics card counts as 8 GB here.
- **No dedicated graphics card:** models still run using main RAM. Expect words to appear at reading speed rather than instantly.

| Memory | What you can run | What it feels like |
| --- | --- | --- |
| 4 GB | Small models (3B) | Quick questions, short text, phones |
| 8 GB | Mid-size models (7-9B) | A capable everyday assistant |
| 16 GB | The same, with longer documents | Comfortable, no compromises |
| 24 GB or more | Large models (27-32B) | Close to a commercial service |

**8 GB is enough to do real work.** That is the important number on this page. A 7B model compressed to Q4 is about 4.5 GB, so it fits on a modest laptop, an older graphics card, or a second-hand machine. You do not need expensive hardware to get out from under a subscription.

Two bits of jargon you cannot avoid:

- **7B, 27B, 32B** count the model's parameters in billions. Bigger means more capable and more memory.
- **Quantization** is compression. Models are published at several sizes, labelled Q3, Q4, Q8 and so on. **Q4 is the standard choice** and roughly halves the memory needed for a small quality cost. LM Studio picks a sensible one for you.

---

## What "Open Model" Means

Most models called open are only **open-weight**. You can download and run them, but the training data and code stay private.

A fully open model releases three things:

- **Weights**, so you can run and fine-tune it yourself.
- **Training data**, so provenance, consent, and copyright can be audited.
- **Training code and documentation**, so it can be reproduced and studied.

Very few capable models release all three. Calling an open-weight release open source is common enough that researchers have a name for it: [open-washing][open-washing].

Openness is a gradient rather than a yes/no. The [Model Openness Framework][model-openness-framework] grades it. The [OSI's Open Source AI Definition][osi-ai-definition] is another reference point, though its handling of training data is still contested.

The same applies to the tooling further down this page. "Open" in a project's name is not a license.

---

## Choosing a Model (August 2026)

Smallest first, because the small ones are good now.

| Memory | Model | Openness | Good for |
| --- | --- | --- | --- |
| 4 GB | [SmolLM3-3B][smollm3] | Fully open | Old laptops, no graphics card, phones |
| 8 GB | [OLMo 3.1 7B][olmo3-collection] | Fully open | **Start here.** Everyday use |
| 8 GB | [Comma v0.1][comma-hf] | Fully open, openly licensed data | Proving a model can be trained ethically |
| 8-16 GB | [Qwen3.5-9B][qwen35-9b] | Open-weight | Coding on modest hardware |
| 24 GB | [OLMo 3.1 32B][olmo3-collection] | Fully open | The most capable fully open option |
| 24 GB | [Qwen3.6-27B][qwen36-27b] | Open-weight | Coding, data tasks, reading images |

The download is a one-off: 2 GB for the smallest, around 4.5 GB for a 7B. After that it runs offline, forever, with no further data cost. On a metered or intermittent connection that matters more than the memory figure.

What sits behind those picks:

- **Fully open:** [OLMo 3 / 3.1][olmo3-blog] (Ai2, Apache 2.0) publishes weights, training data, code, and checkpoints. The strongest genuinely open option today, and the one to reach for first. The 7B version is the sweet spot for most people.
- **Fully open and tiny:** [SmolLM3-3B][smollm3] (Hugging Face, Apache 2.0) publishes its weights, data mixture, and training code. It runs on hardware nothing else here will touch, and handles summarising, drafting, and translation between its six languages.
- **Proof it can be done ethically:** the [Comma models][comma-models] are trained only on openly licensed and public domain text. Capability is dated, but they prove a model with a known, untainted corpus is possible.
- **Open-weight, the pragmatic middle:** Qwen, DeepSeek, and similar releases run locally, so you avoid lock-in and data exposure. Their training data is undisclosed, so the provenance concerns are the same as for proprietary models. [Qwen3.5-9B][qwen35-9b] is the practical choice if you want to write code on an 8 GB machine.
- **Not open, but relevant:** [Tabnine][tabnine] publishes no training data, so its provenance claims cannot be audited. It does flag generated code matching public code on GitHub along with the source license, and offers IP indemnification and air-gapped deployment for enterprise customers.

Do not assume you need the biggest model you can fit. A 7B is quick and perfectly capable of summarising, drafting, translating, and answering questions. Step up only when the work genuinely needs reasoning. Published benchmarks say little about your own tasks, so try two on real work before settling.

<details>
<summary><strong>More detail on Qwen3.6-27B</strong></summary>

From Alibaba Cloud, under Apache 2.0. It reads images as well as text, so you can feed it a flowchart and ask questions about it.

Qwen reports it beating the previous open-weight flagship, Qwen3.5-397B-A17B - roughly fifteen times larger - on every major coding benchmark, including SWE-bench Verified (77.2 vs 76.2). In practice: near-flagship coding capability on a single domestic GPU rather than a datacenter.

Budget 24 GB. At Q4 the weights alone are around 16.5 GB, so a 16 GB card only manages it with heavier compression or by offloading part of the work to the processor.

It holds about 262,000 tokens at once, its "context window". A token is roughly three-quarters of a word, so that is a long book's worth of text, but not unlimited: a 2 MB text file is about double that and will not fit in one pass.

</details>

For evaluating models on your own tasks rather than on published benchmarks, [OpenCompass][opencompass] runs reproducible comparisons. For image generation rather than text, [PD12M][pd12m] is a public domain image dataset, giving the same provenance guarantee the Common Pile gives for text.

---

## What to Run It With

Running the model yourself is what delivers the privacy and independence. No prompts leaving your machine, no per-token billing, no service to be deprecated.

### Most people: LM Studio

**[LM Studio][lm-studio]** is a desktop app with model search, download, and chat in one window. Free for personal and internal business use.

The trade-off, worth knowing before you commit: the app itself is proprietary and cannot be audited or modified, even though its command-line tool and SDKs are MIT. It is a convenience layer over open models, not an open tool. If that is unacceptable for your organization, use llama.cpp's built-in interface instead.

### Developers and servers: llama.cpp

**[llama.cpp][llama-cpp]** (MIT) is the engine nearly everything else is built on, LM Studio included. Give `llama-server` a model file and it serves an OpenAI-compatible API on port 8080 plus a built-in web chat page. No Python environment, no container. Running it directly avoids the overhead a wrapper adds to every request.

### Coding agents

**[OpenCode][opencode]** points a terminal coding agent at any OpenAI-compatible endpoint, including a local `llama-server`. This is what makes a local model useful for real development rather than only for chat.

### Phones

- **[PocketPal AI][pocketpal]** (MIT) is fully offline chat on Android and iOS. No account, and no connection needed once a model is downloaded. Workable on 4 GB of RAM, with SmolLM3-3B a good fit.
- **[MNN][mnn]** (Apache 2.0, Alibaba) is for developers embedding inference in an app. It is substantially faster than llama.cpp on mobile hardware, with the [MNN-LLM paper][mnn-llm-paper] reporting large speedups on Android CPU and GPU. Its official chat apps are a working reference implementation.

### A whole team

**[Open WebUI][open-webui]** is an excellent chat interface. It connects to a llama.cpp backend, serves it over a web page, and installs like an app on mobile. Two caveats: you need a server and a domain, and [since v0.6.6 its license][open-webui-license] adds branding restrictions that make it no longer OSI-approved open source (v0.6.5 and earlier remain BSD-3-Clause). Fine for internal use, but check the terms before redistributing or rebranding.

### A note on Ollama

[Ollama][ollama] is the usual recommendation for an easy start, and we no longer suggest it as a first choice. It has a [history of crediting llama.cpp poorly][ollama-credit] despite being built on it, and now [measurably trails llama.cpp on throughput][ollama-throughput] by anywhere from 10% upwards. LM Studio covers ease of use and llama.cpp is straightforward to run directly, so the wrapper has little left to offer.

---

_A starting point. We will expand this page as we test more of these ourselves._

[lm-studio]: https://lmstudio.ai
[open-washing]: https://doi.org/10.1145/3630106.3659005
[model-openness-framework]: https://arxiv.org/abs/2403.13784
[osi-ai-definition]: https://opensource.org/ai/open-source-ai-definition
[smollm3]: https://huggingface.co/HuggingFaceTB/SmolLM3-3B
[olmo3-collection]: https://huggingface.co/collections/allenai/olmo-3
[comma-hf]: https://huggingface.co/common-pile
[qwen35-9b]: https://huggingface.co/Qwen/Qwen3.5-9B
[qwen36-27b]: https://huggingface.co/Qwen/Qwen3.6-27B
[olmo3-blog]: https://allenai.org/blog/olmo3
[comma-models]: https://github.com/r-three/common-pile
[tabnine]: https://www.tabnine.com/protection/
[opencompass]: https://github.com/open-compass/opencompass
[pd12m]: https://huggingface.co/datasets/Spawning/PD12M
[llama-cpp]: https://github.com/ggml-org/llama.cpp
[opencode]: https://github.com/sst/opencode
[pocketpal]: https://pocketpal.dev/
[mnn]: https://github.com/alibaba/MNN
[mnn-llm-paper]: https://arxiv.org/abs/2506.10443
[open-webui]: https://openwebui.com
[open-webui-license]: https://docs.openwebui.com/license/
[ollama]: https://ollama.com
[ollama-credit]: https://news.ycombinator.com/item?id=47624999
[ollama-throughput]: https://github.com/ollama/ollama/issues/14861
