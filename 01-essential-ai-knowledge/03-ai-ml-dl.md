---
layout: default
title: "1.3 AI vs ML vs DL"
parent: "Domain 1 — Essential AI Knowledge (38%)"
nav_order: 3
---

# 1.3 AI vs Machine Learning vs Deep Learning

## What the exam tests

The nested definitions of AI, ML, and DL — and knowing which technologies (neural networks, transformers, CNNs) fall into which category.

---

## The three nested fields

```
┌─────────────────────────────────────────────────────────┐
│  Artificial Intelligence (AI)                            │
│  Any technique enabling machines to mimic human         │
│  intelligence: reasoning, planning, perception, NLP     │
│                                                         │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Machine Learning (ML)                             │  │
│  │  Systems that learn from data without being        │  │
│  │  explicitly programmed for each task               │  │
│  │                                                    │  │
│  │  ┌──────────────────────────────────────────────┐ │  │
│  │  │  Deep Learning (DL)                           │ │  │
│  │  │  ML using neural networks with many layers   │ │  │
│  │  │  (depth) — learns hierarchical features       │ │  │
│  │  │  directly from raw data                       │ │  │
│  │  └──────────────────────────────────────────────┘ │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

---

## Definitions

### Artificial Intelligence (AI)
The broadest category — any computer system that performs tasks that normally require human intelligence.

- **Examples:** rule-based expert systems, chess engines, speech recognition, image classification, language translation
- **Does NOT require learning** — a hard-coded decision tree qualifies as AI
- **Does NOT require a neural network**

### Machine Learning (ML)
A subset of AI where the system learns from examples (data) rather than following explicit rules programmed by a human.

**Three learning paradigms:**

| Paradigm | How it works | Examples |
|---|---|---|
| **Supervised** | Learns from labeled (input, output) pairs | Image classification, regression, fraud detection |
| **Unsupervised** | Finds structure in unlabeled data | Clustering, anomaly detection, dimensionality reduction |
| **Reinforcement** | Agent learns by reward/penalty from environment | Game playing, robotic control, recommendation tuning |

**Examples of ML algorithms (non-deep):** Decision trees, random forests, gradient boosting (XGBoost), SVMs, k-means, PCA

### Deep Learning (DL)
A subset of ML using **artificial neural networks with many layers** — the "depth" refers to the number of layers between input and output. More layers = the model learns increasingly abstract features.

**Why deep learning dominates modern AI:**
- Learns features automatically from raw data (no manual feature engineering)
- Scales with more data and compute in ways shallow ML cannot
- Enabled by GPUs (GPU parallelism makes training feasible) and large datasets

---

## Key deep learning architectures

| Architecture | Full name | Primary use |
|---|---|---|
| **CNN** | Convolutional Neural Network | Image recognition, video, spatial data |
| **RNN/LSTM** | Recurrent NN / Long Short-Term Memory | Sequences (older approach, largely replaced) |
| **Transformer** | — | Language models, vision, multimodal (current dominant paradigm) |
| **GAN** | Generative Adversarial Network | Image synthesis, data augmentation |
| **Diffusion Model** | — | Image/video generation (Stable Diffusion, DALL-E) |

### The Transformer architecture
The current foundation of virtually all large language models (GPT, LLaMA, Gemini) and many vision models (ViT). Key innovation: the **attention mechanism** — allows every token to attend to every other token in the sequence, capturing long-range dependencies.

**Why Transformers need GPUs:** The attention mechanism is an O(n²) matrix operation over sequence length n. For context windows of 128K tokens, this generates enormous matrix multiplications perfectly suited to GPU Tensor Cores.

---

## Summary table

| | AI | ML | DL |
|---|---|---|---|
| Requires learning from data | No | Yes | Yes |
| Requires neural networks | No | No | Yes |
| Requires GPUs | No | Sometimes | Yes (at scale) |
| Feature engineering | Manual | Manual/Auto | Automatic |
| Example | Expert system | Random forest | GPT-4, ResNet |

---

## Self-check questions

1. Is a rule-based chatbot that uses if/else logic an example of AI, ML, or DL?
2. What makes deep learning "deep"?
3. Which neural network architecture currently dominates large language models?
4. What type of ML does reinforcement learning fall under?
5. Why did deep learning become practical only around 2012?

<details>
<summary>Answers</summary>
1. AI only — it follows explicit rules without learning from data.<br>
2. The number of layers (depth) between input and output. More layers enable learning increasingly abstract representations of data.<br>
3. The Transformer architecture (with self-attention mechanism).<br>
4. It is its own paradigm — separate from supervised and unsupervised learning. The agent learns through trial and error using reward signals from an environment.<br>
5. Two factors converged: (1) GPUs became programmable for general compute (CUDA, 2007), making training feasible; (2) large labeled datasets became available (ImageNet, 2009). AlexNet (2012) demonstrated this combination conclusively.
</details>
