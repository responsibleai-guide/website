---
layout: base.njk
title: Recommended Open Models
description: Research and recommendations for the best open models to use as alternatives to proprietary AI services.
---

# Recommended Open Models

## Qwen3.6-27B

[Qwen3.6-27B](https://huggingface.co/Qwen/Qwen3.6-27B) is a Causal Language Model with Vision Encoder. It features 27B parameters, and it can run on a domestic GPU (like the Nvidia 5070 ti).

This model can be reliably used for small programming tasks, or data analysis (e.g.: updating configurations on library based projects, or round-trips between data formats). The vision encoder capability means the model is able to read images and produce text outputs from them; for example, you could feed it a flowchart and ask questions about it.

Something to keep in mind: the context window is only 262K tokens; A 2MB file is approximately equivalent to 500,000 tokens, which means the model would not be able to parse it.

Qwen refers to the large language model family built by Alibaba Cloud. In this organization, they continuously release large language models (LLM), large multimodal models (LMM), and other AGI-related projects. 
