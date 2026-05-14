---
layout: default
title: "1.4 AI Adoption Drivers"
parent: "Domain 1 — Essential AI Knowledge (38%)"
nav_order: 4
---

# 1.4 Factors Behind Recent AI Improvements and Adoption

## What the exam tests

Why AI has improved so dramatically in the last decade — the intersection of data, compute, and algorithm advances that made modern deep learning possible.

---

## The three pillars of modern AI progress

```
         Data          ×        Compute        ×      Algorithms
  (labeled datasets,      (GPU parallelism,         (Transformers,
   internet scale,         Tensor Cores,            attention, RLHF,
   synthetic data)         cloud AI infra)            scaling laws)
         │                      │                         │
         └──────────────────────┴─────────────────────────┘
                                │
                     Modern AI capabilities
                 (LLMs, image generation, AlphaFold,
                  autonomous driving, drug discovery)
```

---

## 1. Data — the fuel

### Scale
- Pre-2010: AI research used thousands to millions of labeled examples
- Today: LLMs train on trillions of tokens from internet text; vision models use billions of images
- ImageNet (2009): 1.2M labeled images that enabled the deep learning breakthrough

### Data types
- **Labeled data** (supervised learning): requires human annotation — expensive, but necessary for high-accuracy classifiers
- **Unlabeled/self-supervised data** (language models, BERT, GPT): text is self-supervised — "predict the next word" doesn't need human labels
- **Synthetic data**: AI-generated data for training (simulation, diffusion models generating training images)

### Data velocity
Real-time data from IoT, logs, cameras, and user interactions means continuous model improvement and online learning opportunities.

---

## 2. Compute — the engine

### GPU parallelism
The single biggest enabler. Deep learning is fundamentally matrix multiplication at massive scale — GPUs perform these operations 100–1000× faster than CPUs for typical DL workloads.

### Tensor Core evolution
Each NVIDIA generation roughly doubles effective training throughput:

| Year | GPU | Peak Tensor Core TFLOPS (FP16) |
|---|---|---|
| 2017 | V100 | 125 |
| 2020 | A100 | 312 |
| 2022 | H100 | 989 |
| 2024 | B200 | ~2,250 |

### Scaling laws (Chinchilla / GPT)
Empirically proven: model performance scales predictably with compute and dataset size. This gave researchers a roadmap — "add more compute + more data = better model" — which justified massive GPU investments.

### Cloud AI infrastructure
AWS, Azure, GCP, and Oracle Cloud made GPU clusters accessible without upfront capital investment, lowering the barrier for organizations to experiment with large AI models.

---

## 3. Algorithms — the architecture

### Transformer (2017 — "Attention Is All You Need")
Before Transformers, RNNs processed sequences token-by-token — slow and struggled with long-range dependencies. The Transformer's self-attention mechanism processes all tokens simultaneously and captures dependencies at any distance. This enabled:
- Parallelism during training (more GPU utilization)
- Models to scale to trillions of parameters
- GPT, BERT, LLaMA, Gemini, Claude, and every modern LLM

### Transfer learning and fine-tuning
Pre-train once on massive data → fine-tune cheaply for specific tasks. This made large models economically practical: one training run produces a foundation model that powers hundreds of downstream applications.

### RLHF (Reinforcement Learning from Human Feedback)
Technique for aligning LLMs to human preferences (ChatGPT's breakthrough ingredient). Enables models to follow instructions, be helpful, and avoid harmful outputs.

### PEFT / LoRA (Parameter-Efficient Fine-Tuning)
Fine-tune only a small fraction of parameters (adapters), dramatically reducing fine-tuning compute and storage cost. Made LLM customization accessible to organizations without massive GPU clusters.

---

## 4. Ecosystem maturity

| Factor | Impact |
|---|---|
| **Open source frameworks** (PyTorch, TensorFlow, JAX) | Lowered development barrier; massive community |
| **HuggingFace / model hubs** | Pre-trained models freely available; reduced time-to-value |
| **MLOps tooling** (MLflow, Kubeflow, W&B) | Industrialized model lifecycle management |
| **Cloud APIs** (OpenAI, Anthropic, Google) | AI capabilities without infrastructure expertise |
| **NVIDIA NGC** | Optimized, certified containers and models ready to deploy |

---

## Self-check questions

1. What two resources do scaling laws say determine model quality?
2. What architectural innovation in 2017 enabled the modern LLM era?
3. Why is self-supervised learning important for large language models?
4. What is RLHF and what problem does it solve?
5. Name two factors that made GPU compute more accessible to enterprises.

<details>
<summary>Answers</summary>
1. Compute (FLOPS) and data (tokens/parameters) — Chinchilla scaling laws showed optimal ratios between these two.<br>
2. The Transformer architecture (self-attention mechanism) — processes all tokens in parallel and captures long-range dependencies efficiently.<br>
3. Language modeling (predict next token) is self-supervised — it doesn't require expensive human labeling. The internet provides essentially unlimited unlabeled text, enabling training on trillion-token corpora.<br>
4. Reinforcement Learning from Human Feedback. It aligns a pre-trained LLM's outputs to human preferences by training a reward model on human rankings, then using RL to optimize the LLM against that reward model.<br>
5. Any two of: cloud GPU rentals (AWS/Azure/GCP), NVIDIA NGC containers, decreasing GPU cost per FLOP, pre-trained models via HuggingFace.
</details>
